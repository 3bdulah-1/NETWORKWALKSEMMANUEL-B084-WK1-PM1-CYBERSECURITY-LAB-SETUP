<div align="center">

# Cybersecurity Lab Environment Setup

**Building an isolated virtual lab for penetration testing and ethical hacking practice**
</div>

<p align="center">
  <img src="https://img.shields.io/badge/Skill-Cybersecurity-404040?style=flat-square&labelColor=C00000" />
  <img src="https://img.shields.io/badge/Ver-Virtualbox%20v7.2-0070C0?style=flat-square&labelColor=000000" />
  <img src="https://img.shields.io/badge/Kali%20Linux-v2026.2-E87500?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white" />
  <img src="https://img.shields.io/badge/Skill-Linux-404040?style=flat-square&labelColor=C00000" />
  <img src="https://img.shields.io/badge/Network-10.0.0.0%2F24-238F89?style=flat-square&labelColor=000000" />
  <img src="https://img.shields.io/badge/Penetration%20Testing-C00000?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white" />
  <img src="https://img.shields.io/badge/Skill-Virtualization-404040?style=flat-square&labelColor=C00000" />
  <img src="https://img.shields.io/badge/GitHub-404040?style=flat-square&labelColor=0070C0&logo=github&logoColor=white" />
  <img src="https://img.shields.io/badge/Kali%20Linux-404040?style=flat-square&labelColor=C00000&logo=kalilinux&logoColor=white" />
  <img src="https://img.shields.io/badge/NetworkWalks-404040?style=flat-square&labelColor=C00000" />
  <img src="https://img.shields.io/badge/Ethical%20Hacking-E87500?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white" />

</p>

# Cybersecurity Lab Environment Setup

A virtual cybersecurity lab built with VirtualBox and Kali Linux on a private NAT Network.

## Overview

This project sets up an isolated lab on my own PC where I can practice security tools safely. Kali Linux is the attacking machine, and it sits on a private network (`10.0.0.0/24`) where target machines can be added later.

## Task Requirements

| # | Requirement | Result |
|---|---|---|
| 1 | VirtualBox as the base (latest recommended version) | VirtualBox 7.2.22 installed |
| 2 | Kali Linux as the attacking machine | Kali Linux 2026.2 VM imported |
| 3 | Network in subnet `10.0.0.0/24` | Lab network is `10.0.0.0/24` |
| 4 | Use a NAT Network with `10.0.0.0/24` | NAT Network `NatNetwork` created with prefix `10.0.0.0/24` |
| 5 | Clipboard and drag and drop enabled | Shared Clipboard and Drag'n'Drop set to Bidirectional |
| 6 | Shared folder with the host's Downloads folder | Host Downloads folder shared with the VM |
| 7 | Kali IP address `10.0.0.2/24` | Static IP `10.0.0.2/24` configured |
| 8 | Kali has full Internet access | Verified with ping and DNS lookup |

## Lab Architecture

<img width="1346" height="616" alt="image" src="https://github.com/user-attachments/assets/e6fcd338-1514-4ee8-95be-1cfd6e540553" />

## Lab Configuration

| Component | Configuration |
|---|---|
| Host OS | windows 11 |
| Host RAM | 16 |
| Processor | AMD |
| Hypervisor | VirtualBox 7.2.22 |
| Attacker OS | Kali Linux 2026.2 |
| VM name | `kali-linux-2026.2-virtualbox-amd64` |
| Kali RAM | 2 |
| Network | NAT Network `NatNetwork`, `10.0.0.0/24`, DHCP enabled |
| Kali IP address | `10.0.0.2/24` (static) |
| Gateway | `10.0.0.1` |
| DNS | `8.8.8.8` |


## Setup Procedure

### Step 1. Install VirtualBox

I installed the latest recommended VirtualBox from the official site. I also installed 7-Zip to extract the Kali VM archive.

<img width="1919" height="1078" alt="Screenshot 2026-10-27 231509" src="https://github.com/user-attachments/assets/ea8e0416-8225-4f82-8b96-69ad9ec35a7d" />

### Step 2. Create the NAT Network

In VirtualBox: **Tools → Network Manager → NAT Networks → Create**.

| Setting | Value |
|---|---|
| Name | `NatNetwork` |
| IPv4 Prefix | `10.0.0.0/24` |
| DHCP | Enabled |
| IPv6 | Disabled |

I used a NAT Network (not plain NAT) because VMs on the same NAT Network can talk to each other and still reach the Internet.

<img width="1919" height="1079" alt="Screenshot 2026-10-27 232009" src="https://github.com/user-attachments/assets/c82340f4-d59f-47b9-92be-ba12ac702d87" />

### Step 3. Import Kali Linux

I downloaded the pre-built VirtualBox image from the official Kali website, extracted it, and added the `.vbox` file with **Machine → Add**. Then I attached the network adapter to the lab network:

**Settings → Network → Adapter 1 → Attached to: NAT Network → Name: `NatNetwork`**

<img width="1919" height="1077" alt="image" src="https://github.com/user-attachments/assets/1470a171-efb9-466f-a710-2bc1eb3b79f4" />

### Step 4. Enable Clipboard and Drag and Drop

**Settings → General → Advanced**

- Shared Clipboard: **Bidirectional**
- Drag'n'Drop: **Bidirectional**

<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/848f5fe6-e64a-4fb4-ae24-86fb724daa67" />

### Step 5. Share the Downloads Folder

**Settings → Shared Folders → Add**

- Folder Path: the host's **uni** folder
- Folder Name: uni
- Auto-mount: enabled
- Make Permanent: enabled

Inside Kali, the shared folder is available at /media/sf_uin/
<img width="1914" height="1036" alt="image" src="https://github.com/user-attachments/assets/d488d5e7-8551-458f-993d-231d1a995619" />

### Step 6. Set the Static IP

First I checked the real connection name:

```bash
nmcli connection show
```

Then I set the static configuration:

```bash
sudo nmcli connection modify "Wired connection 1" \
  ipv4.method manual \
  ipv4.addresses 10.0.0.2/24 \
  ipv4.gateway 10.0.0.1 \
  ipv4.dns 8.8.8.8 \
  ipv4.dad-timeout 0

sudo nmcli connection down "Wired connection 1" && sudo nmcli connection up "Wired connection 1"
```

<img width="998" height="381" alt="image" src="https://github.com/user-attachments/assets/34a736c4-a876-46b2-8ac1-5c5f50f42a92" />


### Step 7. Take a Clean Snapshot

After everything worked, I took a snapshot named `Clean Kali - Network Setup` so I can return to this working state at any time.

<img width="1919" height="326" alt="Screenshot 2026-10-28 001216" src="https://github.com/user-attachments/assets/1471b837-5ac9-4b2d-801b-b6ee6699b271" />

## Verification

| Test | Command | Result |
|---|---|---|
| IP address | `ip a` | `eth0` has `10.0.0.2/24` (no `dynamic` flag, so it is static) |
| Gateway | `ping -c 3 10.0.0.1` | 3 of 3 replies, 0% loss |
| Internet | `ping -c 3 8.8.8.8` | 3 of 3 replies, 0% loss |
| DNS | `nslookup networkwalks.com` | Resolved to `192.232.216.135` |
| Nmap | `nmap --version` | Nmap 7.99 |
| Shared folder | /media/sf_uin/ | Host files are visible inside Kali |
| Snapshot | Restore snapshot, then `ip a` | Baseline restored |


## Problems and Solutions

### Problem 1. VirtualBox could not find the virtual disk

```text
Could not open the medium '...\kali-linux-2026.2-virtualbox-amd64.vdi'.
VD: error VERR_FILE_NOT_FOUND opening image file ...
```

**Cause:** VirtualBox saves the file path of the `.vdi` disk. The VM folder was no longer at that path.

**Solution:** Make sure the `.vdi` file is in the VM folder, then re-add the VM (see Problem 2).

### Problem 2. "Same UUID as an existing virtual machine"

```text
Trying to open a VM config '...kali-linux-2026.2-virtualbox-amd64.vbox'
which has the same UUID as an existing virtual machine.
```

**Cause:** The old broken entry was still registered in VirtualBox.

**Solution:**

1. Right-click the old VM → **Remove → Remove Only** (not "Delete all files").
2. Check that the `.vbox` and `.vdi` files are in the VM folder.
3. **Machine → Add** and select the `.vbox` file again.

### Problem 3. Kali was on a DHCP address instead of the static IP

After the VM was restored, `ip a` showed `10.0.0.3/24` marked `dynamic`.

**Solution:** I re-applied the static configuration from Step 6. I also added `ipv4.dad-timeout 0`, which removes the duplicate-address-check delay. After that, `ip a` showed `10.0.0.2/24` and the ping and DNS tests passed.

Connection names can differ between systems, so always run `nmcli connection show` first.

## What I Learned

- A NAT Network lets VMs share a private subnet and still reach the Internet. Plain NAT does not.
- How to set and check a static IPv4 address, gateway, and DNS in Kali with `nmcli`.
- A snapshot gives a safe restore point before experiments.
- VirtualBox tracks VMs by UUID and disks by file path, so moving files can break a working VM.

## Security and Ethical Use

⚠️ This lab is for education only. Use it only on systems you own or have explicit permission to test.

## Tools and Resources

- 7-Zip: https://7-zip.org/download.html
- VirtualBox: https://virtualbox.org/wiki/Downloads
- Kali Linux: https://kali.org/get-kali

## Author

**Abdullah E Elashkar**, Cybersecurity Intern at Networkwalks IT Solutions & Training

GitHub: https://github.com/3bdulah-1
LinkedIn: https://www.linkedin.com/in/abdullah-elashkar-b140b2405/

**Program:** Cybersecurity at Networkwalks | **Week:** 01 | **Project:** Cybersecurity & Pentesting Lab Setup
