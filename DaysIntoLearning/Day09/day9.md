# Day 9: Implementing a Differentially Private CPU Tracker 🚀🖥️

Welcome to Day 9 of #30DaysOfFlCode! Today, we implemented a **[Differentially Private CPU Tracker](https://syftbox-documentation.openmined.org/cpu-tracker-1)** and tackled some interesting challenges along the way. Let's dive into the details! 🛠️

---

## 📄 Overview

This script collects **CPU usage samples**, calculates the mean CPU usage, and adds **differential privacy** to the results. It stores:
- The private mean in a **secure private folder**.
- The differentially private mean in a **publicly accessible folder** with restricted permissions.
- It also implements a **cooldown mechanism** to ensure the script doesn't run too frequently. ⏱️

---

## 🔑 Features
- **Differential Privacy**: Protects sensitive CPU usage data with noise using the `diffprivlib` library.
- **Permission Management**: Handles public and private folder creation with custom permissions.
- **Execution Interval Check**: Prevents frequent execution using a simple timestamp-based cooldown.

---

## 🚩 Challenges Faced

While working on this project, I encountered two significant issues:

### 1️⃣ **Scoping Issue**
The `client` variable was initially scoped inside the `main()` function, but our helper functions, `create_restricted_public_folder` and `create_private_folder`, needed access to it. This led to a `NameError`.

#### Original Code (Problematic)
```python
def create_restricted_public_folder(cpu_tracker_path: Path) -> None:
    os.makedirs(cpu_tracker_path, exist_ok=True)
    permissions = SyftPermission.datasite_default(email=client.email)  # ❌ `client` not defined
    permissions.read.append("aggregator@openmined.org")
    permissions.save(cpu_tracker_path)
```

#### Fix
I passed the `client` variable as a parameter to the helper functions to resolve the scoping issue.

```python
def create_restricted_public_folder(cpu_tracker_path: Path, client: Client) -> None:
    os.makedirs(cpu_tracker_path, exist_ok=True)
    permissions = SyftPermission.datasite_default(email=client.email)  # ✅ Access through parameter
    permissions.read.append("aggregator@openmined.org")
    permissions.save(cpu_tracker_path)
```

### 2️⃣ **Local Variable Shadowing**
Script named a local variable `mean` inside the `main()` function, which conflicted with the `mean` function imported from the `statistics` module. This caused an `UnboundLocalError`.

#### Original Code (Problematic)
```python
mean = mean(cpu_usage_samples)  # ❌ `mean` shadows the imported function
```

#### Fix
We renamed the local variable to `mean_value` to avoid shadowing.

```python
mean_value = mean(cpu_usage_samples)  # ✅ No conflict with the imported function
```

---


## 🧩 What's Next?

I will take learnings from this tutorial and few more resources and try to build our proposed "RING APP" with project partners.

---

### 💬 Share Your Thoughts
If you found this interesting or have any questions, feel free to reach out! Let's keep learning and building! 🚀

#30DaysOfFlCode #DifferentialPrivacy #FederatedLearning