# Windows Virtualization Setup & Troubleshooting Guide

## Example PC

- **Model:** Dell OptiPlex 7460 AIO
- **CPU:** Intel Core i7-8700 @ 3.20 GHz
- **CPU cores:** 6
- **Logical processors:** 12
- **Installed RAM:** 8 GB
- **Storage:** SATA SSD
- **Final result:** ✅ Virtualization supported and enabled

---

# 1. Change Boot Order First

Before installing an operating system or booting from installation media, check the **boot order**.

Enter BIOS/UEFI and open:

**General → Boot Sequence**

Make sure the device you want to boot from is placed first, for example:

1. **USB / USB Storage Device** — if installing from a bootable USB
2. **DVD drive** — if installing from a DVD
3. **Internal SSD / Windows Boot Manager** — normal Windows boot

After the operating system is installed, you can put:

**Windows Boot Manager / Internal SSD**

back at the top.

> **Important:** If the PC keeps booting into the existing Windows installation instead of your installer, check the boot order first.

---

# 2. Enable Virtualization in BIOS/UEFI

On this Dell PC, the virtualization settings are under:

**BIOS → Virtualization Support**

The available options are:

### Virtualization
Enables **Intel VT-x**.

This is the main hardware virtualization feature required for virtual machines.

**Recommended:** ✅ Enable

### VT for Direct I/O
Enables **Intel VT-d**.

This provides advanced I/O and device virtualization support.

**Recommended:** ✅ Enable

### Trusted Execution
Intel Trusted Execution Technology (**TXT**).

This is normally not required for VMware, VirtualBox, WSL2, or normal VM use.

**Recommended:** ⬜ Leave disabled unless specifically required

### Recommended BIOS Setup

- ✅ Virtualization — Enabled
- ✅ VT for Direct I/O — Enabled
- ⬜ Trusted Execution — Disabled for normal VM use

Then:

1. Click **Apply**
2. Save changes
3. Exit BIOS
4. Restart the PC

---

# 3. Enable Windows Virtualization Features

Open:

**Control Panel → Programs → Turn Windows features on or off**

On this PC, these features were enabled:

- ✅ **Virtual Machine Platform**
- ✅ **Windows Hypervisor Platform**

### What They Do

| Feature | Purpose |
|---|---|
| Virtual Machine Platform | Provides virtualization components used by WSL2 and other VM-based features |
| Windows Hypervisor Platform | Lets compatible virtualization software use the Windows hypervisor |
| Hyper-V | Microsoft's complete virtualization platform and Hyper-V Manager |

> The full **Hyper-V** feature may not appear on every Windows edition.

After changing Windows Features, click **OK**.

Windows may display:

> **Windows completed the requested changes. Windows needs to reboot your PC.**

Click:

**Restart now**

---

# 4. Confirm Virtualization Is Enabled

After Windows restarts:

1. Press **Ctrl + Shift + Esc**
2. Open **Task Manager**
3. Select **Performance**
4. Select **CPU**
5. Look for:

> **Virtualization: Enabled**

### Possible Results

| Result | Meaning |
|---|---|
| **Virtualization: Enabled** | ✅ Hardware virtualization is working |
| **Virtualization: Disabled** | CPU supports it, but it is disabled in BIOS |
| No virtualization information | Check BIOS settings and CPU specifications |

On this Dell OptiPlex 7460 AIO, Task Manager showed:

> **Virtualization: Enabled**

So the setup was successful.

---

# 5. Important RAM Lesson From This PC

This computer has **8 GB of physical RAM**.

When creating a virtual machine, I first tried assigning:

> **4 GB RAM**

but the virtualization software reported that there was **not enough available memory**.

After reducing the VM memory to:

> **2 GB RAM**

the virtual machine worked.

## Why This Happened

The host Windows system also needs RAM.

With only 8 GB installed:

```text
8 GB total physical RAM
        ↓
Windows + background programs use part of it
        ↓
Only the remaining RAM is available for the VM
```

So assigning 4 GB to the VM can fail when Windows is already using too much memory.

## What Worked

For this PC:

> ✅ **2 GB VM RAM worked**

## Recommended VM RAM With 8 GB Host RAM

| VM Type | Suggested RAM |
|---|---:|
| Lightweight Linux | 2 GB |
| Basic Linux desktop | 2–3 GB |
| Windows VM | 2–4 GB, depending on available host memory |
| Multiple VMs | Not recommended with only 8 GB total RAM |

> Do not automatically assign half of the computer's total RAM to a VM. Check how much memory Windows is already using first.

---

# 6. Check Available Memory Before Starting a VM

Open:

**Task Manager → Performance → Memory**

Look at:

- **In use**
- **Available**
- Total installed memory

Example:

```text
Installed RAM: 8 GB
Windows currently using: ~3 GB
Available: ~5 GB
```

You should leave enough RAM for Windows itself.

A safer approach on an 8 GB machine is usually:

> **Start the VM with 2 GB RAM**

Then increase it later only if necessary.

---

# 7. Recommended Upgrade

For basic virtualization:

> **8 GB RAM = usable, but limited**

For a much better experience:

> **16 GB RAM = recommended**

With 16 GB RAM, you can more comfortably run:

- VMware Workstation
- VirtualBox
- WSL2
- Windows VMs
- Linux VMs
- Cybersecurity labs
- More than one light VM

---

# 8. Correct Setup Order

For future use, follow this order:

```text
1. Enter BIOS/UEFI
        ↓
2. Set the correct boot order
        ↓
3. Enable Intel Virtualization / VT-x
        ↓
4. Enable VT for Direct I/O / VT-d
        ↓
5. Save and restart
        ↓
6. Enable Windows virtualization features if needed
        ↓
7. Restart Windows
        ↓
8. Task Manager → CPU
        ↓
9. Confirm "Virtualization: Enabled"
        ↓
10. Create the virtual machine
        ↓
11. Start with about 2 GB RAM on an 8 GB PC
        ↓
12. Boot from the ISO/USB installer
```

---

# 9. Troubleshooting

## VM says "Not Enough Memory"

Try:

1. Shut down the VM
2. Close unnecessary applications
3. Open **Task Manager → Performance → Memory**
4. Check available RAM
5. Reduce VM RAM

On this PC:

```text
4 GB VM RAM → ❌ Not enough memory
2 GB VM RAM → ✅ Worked
```

---

## VM Does Not Boot From Installer

Check:

1. ISO/USB installation media is connected
2. Boot order is correct
3. USB/DVD/virtual optical drive is before the internal disk
4. Restart the VM or PC

---

## Task Manager Says "Virtualization: Disabled"

Go back to BIOS:

**Virtualization Support → Virtualization → Enable**

Save and restart.

---

# 10. Final Checklist

- [x] Correct boot order configured
- [x] CPU supports virtualization
- [x] BIOS Virtualization / Intel VT-x enabled
- [x] VT for Direct I/O enabled
- [x] Windows Virtual Machine Platform enabled
- [x] Windows Hypervisor Platform enabled
- [x] PC restarted
- [x] Task Manager shows **Virtualization: Enabled**
- [x] 4 GB VM RAM was too high in this situation
- [x] Reduced VM RAM to **2 GB**
- [x] VM successfully worked with 2 GB RAM

---

# Quick Memory Note

```text
BOOT ORDER
    ↓
BIOS VT-x / VT-d
    ↓
WINDOWS VIRTUALIZATION FEATURES
    ↓
RESTART
    ↓
TASK MANAGER: VIRTUALIZATION ENABLED
    ↓
CREATE VM
    ↓
8 GB HOST RAM?
Start around 2 GB VM RAM
    ↓
BOOT INSTALLER
```

## Main Lessons

> **Lesson 1:** Check the boot order before trying to boot an installer.

> **Lesson 2:** BIOS virtualization must be enabled before using virtual machines.

> **Lesson 3:** "8 GB installed RAM" does not mean all 8 GB is available to the VM.

> **Lesson 4:** If 4 GB gives a "not enough memory" error, reduce the VM memory. On this PC, **2 GB worked successfully**.

> **Lesson 5:** For frequent VM use, upgrading the PC to **16 GB RAM** would make virtualization much easier.
