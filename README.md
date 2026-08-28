# How to Check Whether a Windows PC Supports Virtualization

## PC Used in This Example

- **Model:** Dell OptiPlex 7460 AIO
- **CPU:** Intel Core i7-8700 @ 3.20 GHz
- **CPU cores:** 6
- **Logical processors:** 12
- **Memory:** 8 GB
- **Storage:** SATA SSD
- **Result:** **Virtualization is supported and enabled**

---

## 1. Check Virtualization Support in BIOS/UEFI

Restart the PC and enter the BIOS/UEFI setup.

On this Dell PC, the virtualization settings were located under:

**BIOS → Virtualization Support**

The available options were:

- **Virtualization**  
  Enables Intel VT-x. This is the main hardware virtualization feature required for virtual machines.

- **VT for Direct I/O**  
  Enables Intel VT-d. This provides advanced I/O/device virtualization support.

- **Trusted Execution**  
  Intel Trusted Execution Technology (TXT). This is normally **not required** for standard virtual machines.

### Recommended Settings

- ✅ **Virtualization:** Enabled
- ✅ **VT for Direct I/O:** Enabled
- ⬜ **Trusted Execution:** Can remain disabled for normal VM use

After changing the settings:

1. Click **Apply**
2. Exit the BIOS
3. Restart the PC

---

## 2. Check Windows Virtualization Features

Open:

**Control Panel → Programs → Turn Windows features on or off**

On this PC, the following Windows virtualization components were enabled:

- ✅ **Virtual Machine Platform**
- ✅ **Windows Hypervisor Platform**

### What They Mean

| Feature | Purpose |
|---|---|
| Virtual Machine Platform | Provides virtualization components used by features such as WSL2 |
| Windows Hypervisor Platform | Lets compatible virtualization software use the Windows hypervisor |
| Hyper-V | Microsoft's full virtual machine platform and Hyper-V Manager |

> Note: The full **Hyper-V** entry was not visible on this PC. Availability of full Hyper-V depends on the installed Windows edition and configuration.

After enabling Windows virtualization features, Windows may ask you to restart.

Click:

**Restart now**

---

## 3. Confirm Virtualization in Task Manager

After Windows restarts:

1. Press **Ctrl + Shift + Esc**
2. Open **Task Manager**
3. Select **Performance**
4. Select **CPU**
5. Look near the bottom-right for:

**Virtualization: Enabled**

### Possible Results

| Result | Meaning |
|---|---|
| **Virtualization: Enabled** | Hardware virtualization is supported and enabled |
| **Virtualization: Disabled** | CPU supports virtualization, but it is disabled in BIOS/UEFI |
| Virtualization information is missing | Check BIOS settings and CPU specifications |

On this Dell OptiPlex 7460 AIO, Task Manager showed:

> **Virtualization: Enabled**

This confirmed that hardware virtualization was working correctly.

---

## 4. Final Result

This PC is ready to use virtualization software such as:

- VMware Workstation
- Oracle VirtualBox
- WSL2
- Other virtualization software compatible with Windows

The CPU supports hardware virtualization and Windows is detecting it correctly.

---

## 5. RAM Recommendation for Virtual Machines

This PC currently has **8 GB RAM**.

Suggested VM memory allocations:

- Lightweight Linux VM: **2–4 GB**
- Windows VM: **4 GB minimum**
- Avoid running several VMs at the same time with only 8 GB RAM

For a better virtualization or cybersecurity lab experience:

> **Upgrade to 16 GB RAM or more if possible.**

---

## Quick Memory Note

```text
BIOS Virtualization / Intel VT-x
        ↓
Hardware virtualization enabled
        ↓
Windows virtualization components
        ↓
Task Manager → CPU
        ↓
Virtualization: Enabled
        ↓
PC is ready for virtual machines
```

## Short Checklist

- [x] CPU supports virtualization
- [x] BIOS Virtualization enabled
- [x] VT for Direct I/O enabled
- [x] Windows Virtual Machine Platform enabled
- [x] Windows Hypervisor Platform enabled
- [x] PC restarted
- [x] Task Manager shows **Virtualization: Enabled**
