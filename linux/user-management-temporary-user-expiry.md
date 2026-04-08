# User Management: Temporary User with Expiry

## 📌 Overview

In enterprise environments, temporary access is often granted to developers, contractors, or external collaborators. To maintain security and proper access control, such accounts must have a predefined expiration date.

In this scenario, a temporary user account needs to be created for a developer working on the Nautilus project, with access limited to a specific timeframe.



## 🎯 Objective

Create a user named `rose` with an **account expiry date** set to `2027-03-28` on the target server.



## 🧠 Scenario

A developer requires temporary access to the system for a short-term assignment. To enforce security policies, the account must:

* exist on the system
* automatically expire after a defined date
* prevent long-term or forgotten access



## 🛠️ Implementation

### 1. Connect to Target Server

```bash
ssh <user>@app-server-1
```



### 2. Create the User with Expiry Date

```bash
sudo useradd -e 2027-03-28 rose
```



### 3. Verify User Creation

```bash
id rose
```



### 4. Verify Account Expiry Date

```bash
sudo chage -l rose
```

Expected output:

```text
Account expires : Mar 28, 2027
```



## 🔐 Security Considerations

* Limits access duration automatically
* Prevents accumulation of unused accounts
* Reduces risk of unauthorized access over time
* Supports compliance with access control policies



## 💡 Best Practices

* Always define an expiry date for temporary users

* Use ISO date format:

  ```bash
  YYYY-MM-DD
  ```

* Regularly audit expired accounts:

  ```bash
  chage -l <username>
  ```

* Combine with:

  * least privilege access
  * role-based permissions
  * periodic access review



## ❗ Troubleshooting

### Issue: Expiry date not applied

**Cause:** Incorrect date format

**Fix:**

```bash
useradd -e YYYY-MM-DD username
```



### Issue: User still active after expiry

**Cause:** System time mismatch

**Fix:**

```bash
date
timedatectl
```

Ensure server time is correct.



### Issue: Need to update expiry date

**Fix:**

```bash
sudo chage -E 2027-03-28 rose
```



## 🚀 Real-World Application

This approach is widely used for:

* contract developers
* third-party vendors
* internship or training access
* temporary DevOps or audit access

It ensures that access is **automatically revoked without manual intervention**.



## 📚 Reference

Hands-on scenario based on DevOps lab practice (KodeKloud).