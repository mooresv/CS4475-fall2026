# Installing Ubuntu 24.04 LTS in a VM — Student Instructions

These instructions cover both Windows laptops and MacBooks. Follow the section for your machine. **Read the "Which VMM should I use?" section first** — the right choice depends on whether your Mac has Apple Silicon (M1/M2/M3/M4) or an Intel chip.

## Minimum VM specs
- 4 GB RAM allocated to the VM (6–8 GB recommended if your laptop can spare it)
- 4 vCPUs recommended (2 minimum)
- 40 GB virtual disk (25 GB is the bare OS minimum — leave headroom for coursework)

---

## Which VMM should I use?

| Platform | Recommended VMM | Why |
|---|---|---|
| Windows laptop | **VMware Workstation Pro** (free for personal/educational use) or **VirtualBox** (free, open source) | Workstation Pro is now free for students; VirtualBox is a lighter free fallback if Workstation misbehaves on your hardware |
| Intel MacBook | **VMware Fusion Pro** (free) or **VirtualBox** | Same reasoning as Windows |
| Apple Silicon MacBook (M1/M2/M3/M4) | **UTM** (free) or **VMware Fusion Pro** (free, ARM-capable) or **Parallels Desktop** (paid, best performance) | You need an **ARM64** build of Ubuntu, not the standard x86_64 ISO — see note below |

> **Note on Broadcom/VMware licensing:** VMware Workstation Pro and Fusion Pro are free for personal, educational, and commercial use as of the current Broadcom licensing terms — no license key needed, but a free Broadcom account is required to download. If your institution has a site license or a different sanctioned tool, defer to that instead.

---

## Step 1 — Download Ubuntu 24.04 LTS

Go to **ubuntu.com/download/desktop** and download the correct image:
- **Intel/AMD (Windows PCs and Intel Macs):** the standard `ubuntu-24.04.x-desktop-amd64.iso` (use the latest point release, e.g. 24.04.3)
- **Apple Silicon Macs:** the `arm64` server or desktop image (Apple Silicon VMs generally use the ARM64 build — check current UTM/Fusion documentation for the recommended image)

Verify the SHA256 checksum against the value posted on the download page before installing.

---

## Step 2 — Windows: enable virtualization and install a VMM

1. **Enable hardware virtualization in BIOS/UEFI** (skip if already on):
   - Restart, enter BIOS/UEFI setup (usually Del, F2, or F10 at boot)
   - Enable **Intel VT-x** or **AMD-V**, and enable **VT-d/IOMMU** if present
   - Save and reboot
2. **Install VMware Workstation Pro:**
   - Create a free Broadcom account, go to My Downloads → Free Software Downloads, search "Workstation," download the Windows installer
   - Run the installer, accept defaults; when prompted for a license, choose the personal-use / no-key option
   - *Alternative:* install **VirtualBox** from virtualbox.org if Workstation Pro won't install or run correctly on your hardware
3. If you're on Windows with **Hyper-V** enabled (common on Windows Pro/Education), disable it first — Hyper-V and VMware/VirtualBox conflict. Turn it off via "Turn Windows features on or off" → uncheck Hyper-V → reboot.

---

## Step 3 — Intel Mac: install VMware Fusion Pro

1. Create a free Broadcom account, download **VMware Fusion Pro** for macOS
2. Open the DMG, drag Fusion to Applications, launch it
3. Choose the free/personal-use license option when prompted (no key required)

---

## Step 4 — Apple Silicon Mac: install UTM (or Fusion Pro)

1. Download **UTM** from mac.getutm.app (free, or via the Mac App Store for a small fee that supports development)
2. Open the DMG, drag UTM to Applications, launch it
3. *Alternative:* VMware Fusion Pro also supports Apple Silicon now — same install process as Step 3, just make sure you download the ARM64 Ubuntu image, not x86_64

---

## Step 5 — Create the VM

Steps are similar across VMware Workstation/Fusion, VirtualBox, and UTM:

1. Click **New/Create a New Virtual Machine**
2. Point it at the Ubuntu ISO you downloaded
3. Set:
   - Guest OS: Linux → Ubuntu 64-bit (or ARM64 if applicable)
   - RAM: 4–8 GB
   - CPUs: 2–4
   - Disk: 40 GB, dynamically allocated
4. **UTM-specific:** choose "Virtualize" (not "Emulate") for Apple Silicon running ARM64 Ubuntu — emulation works but is much slower
5. Finish the wizard and power on the VM

---

## Step 6 — Install Ubuntu inside the VM

1. The VM boots to the Ubuntu installer (GRUB menu → "Try or Install Ubuntu")
2. Choose your language and keyboard layout
3. Select **Normal installation**; you can leave "Download updates" and third-party software checked
4. Choose **Erase disk and install Ubuntu** — this only affects the *virtual* disk, not your real Mac/Windows drive
5. Set timezone, create a username/password
6. Let it install and reboot when prompted; remove/eject the ISO if the VMM doesn't do this automatically so it doesn't boot the installer again
7. Once booted to the desktop, install the VMM's guest tools/additions for better performance and shared clipboard:
   - VMware: VM menu → Install VMware Tools
   - VirtualBox: Devices menu → Insert Guest Additions CD
   - UTM: install `spice-vdagent` via `sudo apt install spice-vdagent` if using SPICE display

---

## Troubleshooting quick list
- **"VT-x is disabled" error:** go back to BIOS and enable virtualization (Step 2.1)
- **Black screen / won't boot on Windows:** disable Hyper-V, Device Guard, and Core Isolation/Memory Integrity in Windows Security
- **Apple Silicon: installer won't boot:** confirm you downloaded the ARM64 image, not x86_64 — x86_64 Ubuntu will not run on Apple Silicon even in a VM
- **Slow performance:** confirm the VM is set to "Virtualize" mode (not emulated), and that you allocated at least 4 GB RAM / 2 vCPUs
- **Which point release?** Always grab the latest 24.04.x point release (currently 24.04.3) rather than the original 24.04 image — it includes two years of accumulated bug and security fixes and the same installer flow
