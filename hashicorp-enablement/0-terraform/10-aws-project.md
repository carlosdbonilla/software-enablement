# 10 — Final Project: Node.js App on AWS

[← Prev](09-modules.md) · [Index](00-index.md) · [Next: Review Sheet →](11-review-sheet.md)

---

## 🎯 Goal
Use everything you've learned to build a real app environment on AWS.

> 💸 **This costs a little money while it runs.** Finish in one sitting and run `terraform destroy` at the end.

## 💡 What we're building

```
        ┌─────────────── AWS (us-east-1, default VPC) ───────────────┐
        │                                                             │
Users ──┼──▶  EC2 server  ─────── port 3306 ──────▶  RDS MySQL DB     │
:3000   │     (Node.js app)                          (stores data)    │
        │          │                                                  │
        │          └──────▶  S3 bucket (stores images)                │
        └─────────────────────────────────────────────────────────────┘
```

| Piece | AWS service | Job |
|---|---|---|
| Server | **EC2** | Runs the Node.js app |
| Database | **RDS (MySQL)** | Stores app data |
| File storage | **S3** | Stores images |
| Firewalls | **Security groups** | Control who can connect |

### Concepts you'll use
Providers & resources (04) · workflow (05) · state (06) · variables, outputs, locals (07) · `for_each` & `depends_on` (08)

---

## Before you start (one-time AWS setup)

1. Create an AWS account and an **IAM user** with an access key (don't use the root account).
2. Install the **AWS CLI** and save your keys:
   ```bash
   aws configure --profile vscode
   ```
3. In the AWS console, create an **EC2 key pair** (for SSH) and note its name.
4. Find your **default VPC ID** (VPC console → "Your VPCs").
5. Get the app code:
   ```bash
   mkdir aws-project && cd aws-project
   git clone https://github.com/verma-kunal/nodejs-mysql.git
   ```

## Project files

```
aws-project/
├── nodejs-mysql/      ← the app (cloned above)
├── providers.tf
├── variables.tf
├── terraform.tfvars
├── s3.tf
├── rds.tf
├── ec2.tf
└── outputs.tf
```

---

## `providers.tf`
```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region  = "us-east-1"
  profile = "vscode"      # the profile you created with aws configure
}
```

## `variables.tf`
```hcl
variable "vpc_id" {
  type = string
}

variable "key_name" {
  type = string
}

variable "db_password" {
  type      = string
  sensitive = true
}
```

## `terraform.tfvars`
```hcl
vpc_id   = "vpc-xxxxxxxx"     # your default VPC ID
key_name = "my-keypair"       # your EC2 key pair name
```
Set the password as an environment variable so it never lands in a file:
```bash
export TF_VAR_db_password="ChooseAStrongPassword123"
```

## `s3.tf` — bucket + upload images
```hcl
resource "aws_s3_bucket" "app" {
  bucket = "nodejs-app-files-yourname-123"   # must be unique across ALL of AWS

  tags = {
    Name = "NodeJs Static Files"
  }
}

resource "aws_s3_object" "images" {
  for_each = fileset("nodejs-mysql/public/images", "**")   # one per file
  bucket   = aws_s3_bucket.app.bucket
  key      = "images/${each.key}"
  source   = "nodejs-mysql/public/images/${each.key}"
}
```

## `rds.tf` — database + its firewall
```hcl
resource "aws_security_group" "rds_sg" {
  name   = "allow_mysql"
  vpc_id = var.vpc_id

  ingress {
    from_port       = 3306
    to_port         = 3306
    protocol        = "tcp"
    security_groups = [aws_security_group.ec2_sg.id]   # only the app server can connect
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

resource "aws_db_instance" "db" {
  identifier             = "nodejs-rds"
  engine                 = "mysql"
  engine_version         = "8.0"
  instance_class         = "db.t3.micro"
  allocated_storage      = 10
  db_name                = "tf_demo"
  username               = "admin"
  password               = var.db_password
  vpc_security_group_ids = [aws_security_group.rds_sg.id]
  skip_final_snapshot    = true    # lets destroy work without a backup (practice only)
}

locals {
  # endpoint looks like "host:3306" → keep just the host
  db_host = element(split(":", aws_db_instance.db.endpoint), 0)
}
```

## `ec2.tf` — server + its firewall
```hcl
resource "aws_security_group" "ec2_sg" {
  name   = "allow_ssh_http"
  vpc_id = var.vpc_id

  ingress {
    description = "SSH"
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    description = "Node.js app"
    from_port   = 3000
    to_port     = 3000
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

resource "aws_instance" "server" {
  ami                         = "ami-0e86e20dae9224db8"   # Ubuntu, us-east-1 only
  instance_type               = "t2.micro"
  key_name                    = var.key_name
  vpc_security_group_ids      = [aws_security_group.ec2_sg.id]
  associate_public_ip_address = true

  # Startup script: runs once when the server first boots
  user_data = <<-EOF
    #!/bin/bash
    git clone https://github.com/verma-kunal/nodejs-mysql.git /home/ubuntu/app
    cd /home/ubuntu/app
    apt update -y && apt install -y nodejs npm
    echo "DB_HOST=${local.db_host}"                   >  .env
    echo "DB_USER=${aws_db_instance.db.username}"     >> .env
    echo "DB_PASS=${var.db_password}"                 >> .env
    echo "DB_NAME=${aws_db_instance.db.db_name}"      >> .env
    echo "TABLE_NAME=users"                           >> .env
    echo "PORT=3000"                                  >> .env
    npm install
    npm start
  EOF

  depends_on = [aws_s3_bucket.app]

  tags = {
    Name = "nodejs-server"
  }
}
```

> 💡 **Notice:** the server's startup script uses the database's details. That reference makes Terraform **build the database first, automatically.**

## `outputs.tf`
```hcl
output "server_ip" {
  value = aws_instance.server.public_ip
}

output "db_endpoint" {
  value = aws_db_instance.db.endpoint
}

output "bucket_name" {
  value = aws_s3_bucket.app.bucket
}
```

---

## 🧪 Run it

```bash
terraform init
terraform fmt
terraform validate
terraform plan          # read it! count the "+" lines
terraform apply         # type yes — the database takes ~5-10 minutes
```

**Check it worked**
```bash
terraform output
# open in your browser:  http://<server_ip>:3000
```
Also look in the AWS console: EC2 instance, RDS database, S3 bucket with an `images/` folder.

**Clean up 💸**
```bash
terraform destroy
```

---

## ⚠️ Watch Out

| Problem | Fix |
|---|---|
| `BucketAlreadyExists` | S3 names are global — pick a more unique name |
| `InvalidAMIID` | That AMI ID only works in `us-east-1` — match your region |
| `No valid credential sources` | Re-run `aws configure --profile vscode` |
| App doesn't load right away | Startup script takes a few minutes after the server starts |
| Zero images uploaded | The `nodejs-mysql` folder must be inside your project folder |

## 🧠 Recall
1. Which 4 AWS services does this project use, and what does each do?
2. Why is the database created before the server, with no `depends_on` for it?
3. How does the database's firewall allow only the app server in?
4. Why is the password set with `TF_VAR_db_password` instead of in a file?
5. What's the last command you must run?

<details><summary>Answers</summary>

1. EC2 (runs app), RDS (database), S3 (images), security groups (firewalls).
2. The server's `user_data` references the database's values, so Terraform builds the database first.
3. Its ingress rule allows port 3306 only from the EC2 security group.
4. So the secret never gets saved in a file that might go to GitHub.
5. `terraform destroy`
</details>

## 📌 Remember
- Real projects = **providers + resources + variables + outputs** working together
- References create the build order for you
- **Always destroy when done**

---

[← Prev](09-modules.md) · [Index](00-index.md) · [Next: Review Sheet →](11-review-sheet.md)
