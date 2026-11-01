# 08 — Destroy Infrastructure

[← Prev](07-modules-and-vpc.md) · [Index](00-index.md) · [Next: HCP Terraform →](09-hcp-terraform.md)

📘 Official tutorial: [Destroy infrastructure](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/aws-destroy)

---

## 🎯 Goal
Remove infrastructure two ways: **one resource at a time**, or **everything at once**.

## 💡 The Idea
Destroying is a normal part of the lifecycle: retiring an old environment, cleaning up tests, and **not paying for what you don't use**.

> **Analogy:** Terraform is a contractor with a full blueprint. To remove one room, **erase it from the blueprint**. To clear the lot, say **"demolish everything you built."**

| Goal | How |
|---|---|
| Remove **one** resource | Delete (or comment out) it in the code → `terraform apply` |
| Remove **everything** | `terraform destroy` |

## 🧪 Do It

### Part 1 — Remove a single resource

**1. Comment out the instance in `main.tf`:**
```hcl
# resource "aws_instance" "app_server" {
#   ami           = data.aws_ami.ubuntu.id
#   instance_type = var.instance_type
#
#   vpc_security_group_ids = [module.vpc.default_security_group_id]
#   subnet_id              = module.vpc.private_subnets[0]
#
#   tags = {
#     Name = var.instance_name
#   }
# }
```

**2. Comment out the output in `outputs.tf`.** It points to the instance, so leaving it would break the configuration:
```hcl
# output "instance_hostname" {
#   description = "Private DNS name of the EC2 instance."
#   value       = aws_instance.app_server.private_dns
# }
```

**3. Apply:**
```bash
terraform apply
```
```
Plan: 0 to add, 0 to change, 1 to destroy.
```
Type `yes`. The instance is terminated; the VPC stays.

> 💡 This is the declarative model in action: **"not in the code" = "shouldn't exist."**

### Part 2 — Destroy everything
```bash
terraform destroy
```
The plan lists every managed resource with a `-`:
```
Plan: 0 to add, 0 to change, 15 to destroy.

Do you really want to destroy all resources?
  Terraform will destroy all your managed infrastructure, as shown above.
  There is no undo. Only 'yes' will be accepted to confirm.
```
Type `yes`. Terraform deletes in **reverse dependency order** (subnets and route tables before the VPC):
```
Destroy complete! Resources: 15 destroyed.
```

**Check:** the AWS console (us-west-2) shows no `example-vpc` and no running instance.

## Destroy options

| Command | Use |
|---|---|
| `terraform destroy` | Remove everything in this project |
| `terraform plan -destroy` | **Preview** a destroy without doing it |
| `terraform destroy -target=aws_instance.app_server` | Destroy one resource. **Emergencies only**; prefer editing the code |
| `terraform state rm ADDRESS` | Make Terraform **forget** a resource without deleting it in AWS |

## 🔑 Key Terms

| Term | Meaning |
|---|---|
| **Destroy** | Delete all infrastructure the project manages |
| **Reverse dependency order** | Dependents are deleted before the things they depend on |
| **`-target`** | Limit an operation to one resource (exceptional use) |
| **`state rm`** | Stop managing a resource without deleting it |

## ⚠️ Watch Out
- **There is no undo.** Read the destroy plan.
- `destroy` only removes what **this project's state** knows about. Anything you created by hand in the console stays (and keeps billing).
- Protect critical resources in real projects:
  ```hcl
  lifecycle {
    prevent_destroy = true
  }
  ```
- **Next lesson rebuilds everything** with `terraform apply`, so uncomment the instance and output when the tutorial tells you to.

## 🧠 Recall
1. Two ways to remove infrastructure?
2. Why did you also comment out the output?
3. In what order does Terraform destroy things?
4. How do you preview a destroy?
5. What's the difference between `destroy -target` and `state rm`?

<details><summary>Answers</summary>

1. Remove it from the code and `apply` (one resource), or `terraform destroy` (everything).
2. It referenced the deleted instance, which would make the configuration invalid.
3. Reverse dependency order: dependents first (subnets before the VPC).
4. `terraform plan -destroy`
5. `destroy -target` deletes the real resource; `state rm` only makes Terraform forget it.
</details>

## 📌 Remember
- **Not in code = shouldn't exist** → `apply` removes it
- `terraform destroy` = everything, **no undo**
- **Always destroy after practice** 💸

---

[← Prev](07-modules-and-vpc.md) · [Index](00-index.md) · [Next: HCP Terraform →](09-hcp-terraform.md)
