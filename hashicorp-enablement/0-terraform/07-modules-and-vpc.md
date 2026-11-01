# 07 — Modules & the VPC

[← Prev](06-variables-and-outputs.md) · [Index](00-index.md) · [Next: Destroy Infrastructure →](08-destroy-infrastructure.md)

📘 Official tutorial: [Manage infrastructure → modules](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/aws-manage)

---

## 🎯 Goal
Use a **module** to build a full AWS network (VPC) in a few lines, then put your server inside it.

## 💡 The Idea
A **module** is a reusable package of Terraform code.

> **Analogy:** A module is a **recipe**. Someone already figured out how to cook it. You fill in the ingredients (inputs) and get the dish (outputs).

| Recipe | Module |
|---|---|
| Ingredients | Input variables |
| Cooking steps | Resources inside |
| The dish | Outputs |

Building a production VPC by hand takes **dozens of resources**: subnets, route tables, internet gateway, associations. The community module **`terraform-aws-modules/vpc/aws`** does it all from about 10 lines.

## Quick networking refresher

| Term | Meaning |
|---|---|
| **VPC** | Your private, isolated network in AWS |
| **CIDR** `10.0.0.0/16` | The VPC's address range (65,536 addresses) |
| **Subnet** `10.0.1.0/24` | A smaller slice (256 addresses) in one Availability Zone |
| **Public subnet** | Has a route to the internet (via an Internet Gateway) |
| **Private subnet** | No direct internet access, safer for servers and databases |
| **Availability Zone (AZ)** | A separate data center in the region (`us-west-2a`, `b`, `c`) |
| **Security group** | Firewall attached to a server |

## 🧪 Do It

### Step 1 — Add the VPC module to `main.tf`
```hcl
module "vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  version = "5.19.0"

  name = "example-vpc"
  cidr = "10.0.0.0/16"

  azs             = ["us-west-2a", "us-west-2b", "us-west-2c"]
  private_subnets = ["10.0.1.0/24", "10.0.2.0/24"]
  public_subnets  = ["10.0.101.0/24"]

  enable_dns_hostnames = true
}
```

| Argument | Meaning |
|---|---|
| `source` | Where the module comes from (Terraform Registry) |
| `version` | **Always pin** module versions |
| `name`, `cidr` | VPC name and address range |
| `azs` | Which Availability Zones to use |
| `private_subnets` / `public_subnets` | Subnet ranges to create |
| `enable_dns_hostnames` | Gives instances DNS names (needed for `private_dns`) |

### Step 2 — Put the server in the new VPC
```hcl
resource "aws_instance" "app_server" {
  ami           = data.aws_ami.ubuntu.id
  instance_type = var.instance_type

  vpc_security_group_ids = [module.vpc.default_security_group_id]
  subnet_id              = module.vpc.private_subnets[0]

  tags = {
    Name = var.instance_name
  }
}
```
| New line | Meaning |
|---|---|
| `module.vpc.default_security_group_id` | Read an **output** of the module: the VPC's default firewall |
| `module.vpc.private_subnets[0]` | The **first** private subnet (lists start at `[0]`) |

**Module reference pattern:** `module.MODULE_NAME.OUTPUT_NAME`

### Step 3 — Install the module
```bash
terraform init
```
Run `init` **every time you add a module or provider**.

### Step 4 — Apply
```bash
terraform apply
```
The plan shows many `+` resources (the VPC, subnets, route tables…) and **`-/+` for the instance**. AWS **can't move a server to a different VPC**, so Terraform replaces it. Type `yes`.

Terraform orders everything automatically: **network first, then the server**, because the server references the module's outputs.

### Step 5 — See what was built
```bash
terraform state list
```
```
data.aws_ami.ubuntu
aws_instance.app_server
module.vpc.aws_default_network_acl.this[0]
module.vpc.aws_default_route_table.default[0]
module.vpc.aws_default_security_group.this[0]
module.vpc.aws_internet_gateway.this[0]
module.vpc.aws_route.public_internet_gateway[0]
module.vpc.aws_route_table.private[0]
module.vpc.aws_route_table.public[0]
module.vpc.aws_route_table_association.private[0]
...
module.vpc.aws_subnet.private[0]
module.vpc.aws_subnet.public[0]
module.vpc.aws_vpc.this[0]
```
**Everything inside the module is prefixed with `module.vpc.`** That's about a dozen resources from 10 lines of code.

### Your final project
```
learn-terraform-get-started-aws/
├── terraform.tf      ← settings
├── main.tf           ← provider, AMI lookup, VPC module, EC2 instance
├── variables.tf      ← instance_name, instance_type
└── outputs.tf        ← instance_hostname
```

---

## Types of modules

| Type | What it is |
|---|---|
| **Root module** | The folder where you run `terraform` (your project) |
| **Child module** | A module called by another module |
| **Registry module** | Published on the [Terraform Registry](https://registry.terraform.io/browse/modules), like the VPC module |
| **Local module** | Your own module in a subfolder: `source = "./modules/web"` |

### Why use modules
- **Reuse:** write once, use for dev, test and prod
- **Consistency:** every team builds the network the same, approved way
- **Simplicity:** dozens of resources behind a few inputs

**How to use any Registry module:** open its page → read **Inputs** (what you can set) and **Outputs** (what you can reference) → pin the `version`.

## 🔑 Key Terms

| Term | Meaning |
|---|---|
| **Module** | Reusable package of Terraform code |
| `source` / `version` | Where it comes from / pinned version |
| `module.NAME.OUTPUT` | Read a module's output |
| **Root module** | Your working folder |
| **VPC / subnet / AZ** | Network / slice of it / data center |

## ⚠️ Watch Out
- **Forgot `terraform init`?** You'll get *"Module not installed."*
- **The instance is replaced** in this step: new ID, and anything stored on it is gone.
- **The server is in a private subnet** with no public IP, so you can't SSH to it from your laptop. That's fine for learning; you'd add a bastion, SSM or a public subnet for access.
- You can only read what a module **outputs**. Its internals are private.

## 🧠 Recall
1. What is a module, using the recipe analogy?
2. How do you read the module's private subnet IDs?
3. What command must you run after adding a module?
4. Why was the EC2 instance replaced?
5. What prefix do module resources have in state?

<details><summary>Answers</summary>

1. A reusable recipe: inputs are ingredients, resources are the steps, outputs are the dish.
2. `module.vpc.private_subnets` (first one: `module.vpc.private_subnets[0]`).
3. `terraform init`
4. AWS can't move an existing instance to a different VPC, so it must be recreated.
5. `module.vpc.`
</details>

## 📌 Remember
- Module = **recipe** · `module.NAME.OUTPUT`
- **Pin versions**, run **`init`** after adding
- 10 lines → a full VPC; Terraform orders **network before server**

---

[← Prev](06-variables-and-outputs.md) · [Index](00-index.md) · [Next: Destroy Infrastructure →](08-destroy-infrastructure.md)
