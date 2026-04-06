# User Management: Non-Interactive Shell

## 📌 Overview

In enterprise environments, certain system users are created solely for running background services or automation tools. These users should not be allowed to log in interactively.

In this case, a system administrator is required to create a user for a backup agent in a company environment. The user must exist on the system but must not have access to an interactive shell.



## 🎯 Objective

Create a user named `rose` with a **non-interactive shell** and **disabled login access** on the target server.



## 🧠 Scenario

A backup agent tool requires a dedicated system user to run its processes. For security reasons, this user must:

* exist on the system
* not allow interactive login
* reduce the risk of unauthorized access



## 🛠️ Implementation

### 1. Identify the Correct `nologin` Path

```bash
which nologin
```

Example output:

```text
/usr/sbin/nologin
```



### 2. Create the User with Non-Interactive Shell

```bash
useradd -s $(which nologin) rose
```



### 3. Lock the User Password (IMPORTANT)

```bash
passwd -l rose
```

This ensures the system does not attempt password authentication.



### 4. Verify User Configuration

```bash
grep rose /etc/passwd
```

Expected output (example):

```text
rose:x:1001:1001::/home/rose:/usr/sbin/nologin
```



### 5. Validate Login Restriction

```bash
su - rose
```

Expected result:

```text
This account is currently not available.
```



## 🔐 Security Considerations

* Prevents direct shell access for service accounts
* Reduces attack surface in case of credential exposure
* Enforces the principle of least privilege



## 💡 Best Practices

* Always detect shell path dynamically:

  ```bash
  $(which nologin)
  ```

* Use non-interactive shells for:

  * service accounts
  * automation users
  * monitoring or backup agents

* Avoid assigning `/bin/bash` unless interactive access is required

* Combine with:

  * locked passwords (`passwd -l`)
  * minimal permissions
  * proper file ownership



## ❗ Troubleshooting

### Issue: `su: Authentication failure`

**Cause:** Password authentication is still enabled

**Fix:**

```bash
passwd -l rose
```



### Issue: User can still access shell

**Cause:** Incorrect shell assigned

**Fix:**

```bash
usermod -s $(which nologin) rose
```



### Issue: `nologin` not found

**Fix:**

```bash
which nologin
```

Use the returned path instead of hardcoding `/sbin/nologin`.



## 🚀 Real-World Application

This approach is commonly used in:

* backup agents
* CI/CD runners (e.g., Jenkins, GitLab Runner)
* monitoring tools
* system services

By restricting login capability, organizations ensure that service accounts are only used for their intended purpose.



## 📚 Reference

Hands-on scenario based on DevOps lab practice (KodeKloud).
