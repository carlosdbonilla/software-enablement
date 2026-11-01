# 02 — Install Terraform & Set Up AWS

[← Prev](01-what-is-iac.md) · [Index](00-index.md) · [Next: Create Infrastructure →](03-create-infrastructure.md)

📘 Official tutorial: [Install Terraform](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)

---

## 🎯 Goal
Get three things ready: **Terraform**, the **AWS CLI**, and **AWS credentials** Terraform can use.

## 💡 The Idea
Terraform runs on your laptop and calls the AWS API for you. To do that, it needs **permission**, which means AWS credentials.

> **Analogy:** Terraform is a contractor. Installing it hires the contractor. **AWS credentials are the badge** that lets them into the building.

## 🧪 Do It

### Step 1 — Install Terraform

**Mac (Homebrew)**
```bash
brew tap hashicorp/tap
brew install hashicorp/tap/terraform
```

**Windows (Chocolatey)**
```powershell
choco install terraform
```
(Or download the zip from the [install page](https://developer.hashicorp.com/terraform/install) and put `terraform.exe` in a folder on your PATH.)

**Verify** (in a new terminal):
```bash
terraform -version
terraform -help
```

**Optional — tab completion** (Mac uses zsh):
```bash
touch ~/.zshrc
terraform -install-autocomplete
```
Restart your terminal, then type `terraform pl` and press Tab.

### Step 2 — Install the AWS CLI
```bash
brew install awscli      # Mac
aws --version
```
(Windows: download the installer from the AWS CLI page.)

### Step 3 — Create AWS credentials
1. Sign in to the AWS console. **Don't use the root account for daily work.**
2. Go to **IAM → Users → Create user** (e.g., `terraform-learner`).
3. Give it permissions. For learning, `AdministratorAccess` is simplest; in real jobs you'd use least privilege.
4. Open the user → **Security credentials → Create access key** (choose "Command Line Interface").
5. Save the **Access key ID** and **Secret access key**. The secret is shown only once.

### Step 4 — Give the credentials to Terraform

**Option A — environment variables (what the official tutorial uses):**
```bash
export AWS_ACCESS_KEY_ID="AKIA..."
export AWS_SECRET_ACCESS_KEY="your-secret"
aws configure list          # confirm the CLI sees them
```
These last **only for this terminal window**.

**Option B — a saved profile (easier day to day):**
```bash
aws configure
# AWS Access Key ID:     AKIA...
# AWS Secret Access Key: ****
# Default region name:   us-west-2
# Default output format: json

aws sts get-caller-identity   # proves the credentials work
```
Saved in `~/.aws/credentials`. Terraform finds it automatically.

> The AWS provider uses the **same credentials as the AWS CLI**. If `aws sts get-caller-identity` works, Terraform will too.

### Step 5 — Editor
Install the **HashiCorp Terraform** extension in VS Code for colors, autocomplete and formatting.

## 🔑 Key Terms

| Term | Meaning |
|---|---|
| **IAM user** | An identity in your AWS account |
| **Access key ID / secret access key** | The username/password pair programs use to call AWS |
| **AWS CLI** | Command-line tool for AWS |
| **Profile** | A named set of saved credentials |
| **Region** | Where in the world AWS creates resources (tutorials use `us-west-2`, Oregon) |

## ⚠️ Watch Out
- **Never put access keys in `.tf` files or push them to GitHub.** Bots scan GitHub for leaked AWS keys within minutes.
- `export` variables disappear when you close the terminal. If Terraform suddenly says *"No valid credential sources found,"* export them again or use a profile.
- In a company setting you'd usually sign in with **SSO** (`aws sso login`) instead of long-lived keys.

## 🧠 Recall
1. What command checks that Terraform is installed?
2. What two values make up an AWS access key?
3. Which command proves your AWS credentials work?
4. Why shouldn't keys go in `.tf` files?

<details><summary>Answers</summary>

1. `terraform -version`
2. Access key ID and secret access key.
3. `aws sts get-caller-identity` (or `aws configure list` to see what's loaded).
4. `.tf` files go to Git/GitHub, so the keys would leak.
</details>

## 📌 Remember
- Install **Terraform + AWS CLI**
- Credentials via **env vars or `aws configure`**, never in code
- If the AWS CLI works, Terraform works

---

[← Prev](01-what-is-iac.md) · [Index](00-index.md) · [Next: Create Infrastructure →](03-create-infrastructure.md)
