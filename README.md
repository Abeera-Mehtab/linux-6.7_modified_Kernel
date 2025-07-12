# 🐧 Modified Linux Kernel 6.7 - Priority-Based CFS Scheduler

This repository contains a **custom-modified version of the Linux 6.7 kernel** that alters the default process scheduling behavior of the Completely Fair Scheduler (CFS). The goal is to **prioritize high-privilege and critical tasks**—such as those executed with `sudo`—over standard user processes, enabling more efficient and responsive system behavior in high-demand or security-sensitive environments.

---

## 📌 What is the Completely Fair Scheduler (CFS)?

The CFS is the default scheduler used in the Linux kernel. It operates on the principle of distributing CPU time fairly across all processes based on a metric called **virtual runtime**. Each task receives CPU access proportionally, ensuring equal treatment across users and applications.

While this is ideal for general-purpose computing, it may not suit **performance-critical environments** where certain processes (e.g., administrative, security, or system-level) should take precedence over others.

---

## 🚀 What We Modified

This kernel changes the core logic of the scheduling mechanism by:

- Introducing a **priority-aware enhancement** to the standard fair scheduling logic.
- Implementing **priority tables** and tracking privilege levels of tasks.
- Dynamically adjusting the scheduling weight and CPU access of tasks based on their privilege level.

The result is a scheduling model where **root or sudo-level processes are granted CPU access more responsively**, reducing latency and improving overall system efficiency in managed or multi-user setups.

---

## 🛠️ How to Compile and Install This Kernel

The steps below will guide you through downloading, configuring, compiling, and installing this modified Linux 6.7 kernel.

### Step 1: Check Your Current Kernel Version

```
uname -a
```

---

### Step 2: Install Dependencies

```
sudo apt update
sudo apt install build-essential git fakeroot ncurses-dev xz-utils libssl-dev bc flex libelf-dev bison dwarves
```

---

### Step 3: Download Linux 6.7 Source Code

```
wget https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.7.tar.xz
tar -xf linux-6.7.tar.xz
cd linux-6.7
```

---

### Step 4: Prepare the Kernel Config

```
cp -v /boot/config-$(uname -r) .config
make localmodconfig
scripts/config --disable SYSTEM_TRUSTED_KEYS
scripts/config --disable SYSTEM_REVOCATION_KEYS
scripts/config --set-str CONFIG_SYSTEM_TRUSTED_KEYS ""
```

---

### Step 5: Apply the Modified Scheduler Code

Navigate to the `sched/` directory inside the Linux source:

```
cd path/to/linux-6.7/kernel/sched
```

Then replace the contents of that folder with the files from this repository (not the folder itself, just the files):

```
cp /path/to/cloned/linux-6.7_modified_Kernel/* .
```

> ⚠️ Ensure you only copy the **files**, not the entire folder, to correctly overwrite the original scheduler code.

---

### Step 6: Compile the Kernel

To reduce system resource usage, it’s recommended to switch to a terminal-only interface (e.g., Ctrl+Alt+F3) before compiling.

```
fakeroot make -j3
```

For multi-core systems, you may use `-j$(nproc)` to compile faster.

---

### Step 7: Install the Kernel

```
sudo make modules_install
sudo make install
```

This will copy the new kernel image, System.map, and configuration files to `/boot` and update GRUB.

---

### Step 8: Reboot into the New Kernel

Reboot your system and select the new kernel version from the GRUB boot menu.

---

### Step 9: Verify Kernel Installation

After rebooting, confirm you're running the new kernel:

```
uname -a
```

You should see the version string reflect your newly built kernel (e.g., `6.7.0-custom`).


---

## 🧠 Why This Kernel Modification Matters

- Prioritizes **critical and administrative processes** automatically.
- Enhances **responsiveness for root-level or system services**.
- Suitable for **cybersecurity labs**, **research environments**, or **high-performance setups**.
- A learning-oriented project to explore **kernel development and scheduling internals**.

---

## ⚠️ Disclaimer

This kernel is for **educational and research use only**. Install it only in a virtual machine or test environment. Not recommended for production systems.

---

## 📬 Contact

Maintainer: **Abeera Mehtab**  
GitHub: [@Abeera-Mehtab](https://github.com/Abeera-Mehtab)

---
