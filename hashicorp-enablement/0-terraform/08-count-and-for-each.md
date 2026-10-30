# 08 — Count, For Each & Other Meta-Arguments

[← Prev](07-variables-outputs-locals.md) · [Index](00-index.md) · [Next: Modules →](09-modules.md)

---

## 🎯 Goal
Create many resources from one block, and control how resources behave.

## 💡 The Idea
**Meta-arguments** are special settings that work on **any** resource, no matter the provider.

| Meta-argument | What it does | Analogy |
|---|---|---|
| `count` | Make **N copies** | "Print 3 copies" |
| `for_each` | Make **one per item** in a list/map | "One name tag per guest" |
| `depends_on` | Force an order | "Pour the foundation before the walls" |
| `lifecycle` | Change create/delete behavior | Safety rules |

---

## `count` — make N copies

From the course:
```hcl
resource "local_file" "example" {
  count    = 3
  filename = "${path.module}/example-${count.index}.txt"
  content  = "kunal is a good instructor!"
}
```
Creates `example-0.txt`, `example-1.txt`, `example-2.txt`.

- `count.index` starts at **0**
- Refer to one: `local_file.example[0]`

**Handy trick — on/off switch:**
```hcl
count = var.create_bucket ? 1 : 0     # 1 = create it, 0 = skip it
```

---

## `for_each` — one per item

```hcl
resource "local_file" "env" {
  for_each = toset(["dev", "staging", "prod"])
  filename = "${path.module}/${each.key}.txt"
  content  = "This is ${each.key}"
}
```
Creates `dev.txt`, `staging.txt`, `prod.txt`.

- `each.key` = the current item
- Refer to one: `local_file.env["dev"]`

From the course project — upload every image in a folder to S3:
```hcl
resource "aws_s3_object" "images" {
  for_each = fileset("nodejs-mysql/public/images", "**")
  bucket   = aws_s3_bucket.app.bucket
  key      = "images/${each.key}"
  source   = "nodejs-mysql/public/images/${each.key}"
}
```

### `count` vs `for_each` — which should I use?

| Use **`count`** when… | Use **`for_each`** when… |
|---|---|
| Things are identical | Things have names |
| You need an on/off switch | You might add/remove items later |

> **Why it matters:** with `count`, deleting the middle item **shifts the numbers**, so Terraform may destroy and recreate everything after it. `for_each` uses names, so only the removed item is affected. **When in doubt, use `for_each`.**

---

## `depends_on` — force an order

Terraform usually figures out the order from references. Use `depends_on` only when there's a hidden connection.

```hcl
resource "aws_instance" "server" {
  # ...
  depends_on = [aws_s3_bucket.app]   # wait for the bucket first
}
```

## `lifecycle` — safety rules

```hcl
lifecycle {
  prevent_destroy       = true    # refuse to delete this (protect databases!)
  create_before_destroy = true    # build the new one before removing the old one
  ignore_changes        = [tags]  # ignore changes made outside Terraform to these
}
```

## 🧪 Try It
1. Use `count = 3` to make 3 files. Then change to `count = 2` and `plan` — which one gets removed?
2. Use `for_each` with `["dev", "staging", "prod"]`. Remove `"staging"` and `plan` — only `staging.txt` goes.

## 🔑 Key Terms

| Term | Meaning |
|---|---|
| `count.index` | Copy number, starting at 0 |
| `each.key` / `each.value` | The current item in `for_each` |
| `toset([...])` | Turns a list into a set (needed for `for_each` on a list) |
| `depends_on` | Explicit ordering |
| `prevent_destroy` | Blocks deletion |

## ⚠️ Watch Out
- `for_each` needs a **set** or **map**, not a plain list → wrap lists in `toset()`.
- You can't use `count` and `for_each` on the same resource.

## 🧠 Recall
1. With `count = 3`, what are the index numbers?
2. Why is `for_each` usually safer than `count`?
3. How do you make a resource optional?
4. Which lifecycle setting protects a database from being deleted?

<details><summary>Answers</summary>

1. 0, 1, 2.
2. Items are tracked by name, so removing one doesn't shift and recreate the others.
3. `count = var.something ? 1 : 0`
4. `prevent_destroy = true`
</details>

## 📌 Remember
- `count` = N copies · `for_each` = one per named item
- Prefer `for_each` for named things
- `prevent_destroy` for anything precious

---

[← Prev](07-variables-outputs-locals.md) · [Index](00-index.md) · [Next: Modules →](09-modules.md)
