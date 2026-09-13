# Cybersecurity Lab Environment Setup

**WEEK-1 PHASE-2 | Additional Task Virtual Machines: Windows 10 & Android 9**

**Oracle VM VirtualBox — Isolated Multi-VM Cybersecurity Lab**

---

## 📌 Project Extension

This document extends the Phase-1 Week-1 cybersecurity laboratory by adding **Windows 10** and **Android 9** virtual machines to the existing Oracle VM VirtualBox **NAT Network**. The additional machines are configured with static IPv4 addresses and tested for communication with the Kali Linux VM and the Internet.

The original laboratory uses the private NAT Network **"NatNetwork"** with network address `10.0.0.0/24`, Kali Linux at `10.0.0.2/24`, gateway `10.0.0.1`, and DNS `8.8.8.8`.

---

## 🎯 Objectives

- Add Windows 10 and Android 9 virtual machines to the existing Oracle VM VirtualBox lab.
- Configure static IPv4 addressing for the additional virtual machines.
- Configure Windows 10 with IPv4 address `10.0.0.10/24`.
- Configure Android 9 with IPv4 address `10.0.0.9/24`.
- Verify Windows 10 ↔ Kali Linux connectivity.
- Verify Android 9 ↔ Kali Linux connectivity.
- Verify Internet connectivity from the additional virtual machines.
- Troubleshoot Windows Firewall ICMP blocking when required.
- Document the completed multi-machine cybersecurity lab.

---

## 🛡️ Lab Network Topology

| Virtual Machine | IPv4 Address | Subnet | Purpose |
|---|---|---|---|
| Kali Linux | 10.0.0.2 | 255.255.255.0 (/24) | Cybersecurity testing |
| Android 9 | 10.0.0.9 | 255.255.255.0 (/24) | Mobile target / testing VM |
| Windows 10 | 10.0.0.10 | 255.255.255.0 (/24) | Windows target / testing VM |
| Gateway | 10.0.0.1 | 255.255.255.0 (/24) | NAT Network gateway |

---

## ⚙️ Existing Oracle VM VirtualBox Network

Use the existing NAT Network created during Phase-1 Week-1 and add the Phase-2 Windows 10 and Android configuration:

- **Network Name:** NatNetwork
- **IPv4 Network:** 10.0.0.0/24
- **Gateway:** 10.0.0.1
- **Kali Linux:** 10.0.0.2/24
- **Android 9:** 10.0.0.9/24
- **Windows 10:** 10.0.0.10/24
- **DNS:** 8.8.8.8

---

## 🪜 Step-by-Step Procedure — Phase-2 Additional Tasks of Week-1

### Step 1. Configure Windows 10 VM Network

Open the Windows 10 virtual machine in Oracle VM VirtualBox and make sure its network adapter is attached to the existing NAT Network, "NatNetwork." Then configure the Windows 10 guest with the static IPv4 address `10.0.0.10`.

- IPv4 Address: `10.0.0.10`
- Subnet Mask: `255.255.255.0`
- Default Gateway: `10.0.0.1`
- DNS Server: `8.8.8.8`

<img width="1562" height="757" alt="01-windows10-network-config" src="https://github.com/user-attachments/assets/2b45629d-61ff-4ba4-89ff-fee6ebb98dde" />

### Step 2. Test Windows 10 → Kali Linux

From Windows 10, open Command Prompt and ping the Kali Linux IPv4 address `10.0.0.2`.

```
ping 10.0.0.2
```

Successful replies confirm that Windows 10 can communicate with the Kali Linux VM over the isolated NAT Network.

<img width="1465" height="751" alt="02-windows10-ping-kali" src="https://github.com/user-attachments/assets/cc413eef-aa8b-498d-b2b9-cbc143f013a3" />

The supplied Phase-2 task document records that Windows 10 (`10.0.0.10`) successfully pinged Kali Linux (`10.0.0.2`).

### Step 3. Test Windows 10 Internet Connectivity

```
ping 8.8.8.8
```

A successful response confirms Internet reachability through the VirtualBox NAT Network.

<img width="1346" height="746" alt="03-windows10-ping-internet" src="https://github.com/user-attachments/assets/4962a5a0-2df7-4b8d-a38b-da065fe27570" />

### Step 4. Test Kali Linux → Windows 10

From Kali Linux, test communication with the Windows 10 VM:

```
ping 10.0.0.10
```

If the ping fails while Windows 10 is running and correctly addressed, Windows Firewall may be blocking ICMP Echo Request traffic.

<img width="728" height="657" alt="04-kali-ping-windows10-fail" src="https://github.com/user-attachments/assets/5e03f61b-fd61-4fc9-ad99-fe3f01f844fe" />

### Step 5. Allow ICMPv4 on Windows 10 Firewall

The supplied task document provides the following Windows command to allow inbound ICMPv4 Echo Request:

```
netsh advfirewall firewall add rule name="Allow ICMPv4 Echo Request" protocol=icmpv4:8,any dir=in action=allow
```

After applying the rule, repeat the Kali-to-Windows ping test. The supplied task document reports successful ping from Kali `10.0.0.2` to Windows 10 `10.0.0.10` and successful Internet testing to `8.8.8.8`.

<img width="1782" height="672" alt="05-kali-ping-windows10-success" src="https://github.com/user-attachments/assets/6aabf673-af59-4b10-91a0-cb8b4566e139" />

### Step 6. Configure Android 9 VM Network

Open the Android 9 virtual machine in Oracle VM VirtualBox and attach its network adapter to the same NatNetwork. Configure Android 9 with the static IPv4 address `10.0.0.9`.

- IPv4 Address: `10.0.0.9`
- Prefix / Subnet: `/24`
- Gateway: `10.0.0.1`
- DNS: `8.8.8.8`

<img width="777" height="832" alt="06-android9-network-config" src="https://github.com/user-attachments/assets/4913cc5d-f094-49c7-93e8-abba4c5333f0" />

### Step 7. Test Kali Linux → Android 9

From Kali Linux, ping the Android 9 IPv4 address:

```
ping 10.0.0.9
```

The supplied Phase-2 task document records successful communication from Kali Linux `10.0.0.2` to Android 9 `10.0.0.9` and successful Internet testing to `8.8.8.8`.

<img width="1807" height="692" alt="07-kali-ping-android9" src="https://github.com/user-attachments/assets/790453db-d7a4-4fb9-9502-3b407bdecf33" />

---

## 🧪 Connectivity Verification Matrix

| Source | Destination | Test | Expected Result |
|---|---|---|---|
| Windows 10 (10.0.0.10) | Kali (10.0.0.2) | `ping 10.0.0.2` | Successful replies |
| Windows 10 (10.0.0.10) | Internet | `ping 8.8.8.8` | Successful replies |
| Kali (10.0.0.2) | Windows 10 (10.0.0.10) | `ping 10.0.0.10` | Successful replies |
| Kali (10.0.0.2) | Android 9 (10.0.0.9) | `ping 10.0.0.9` | Successful replies |
| Kali (10.0.0.2) | Internet | `ping 8.8.8.8` | Successful replies |
| Android 9 (10.0.0.9) | Internet | `ping 8.8.8.8` | Successful replies |

---

## 🐞 Problems Encountered & Solutions

### Problem 1. Windows 10 Does Not Respond to Ping

If Windows 10 can ping Kali but Kali cannot ping Windows 10, the Windows Firewall may be blocking ICMP.

**Solution:**

```
netsh advfirewall firewall add rule name="Allow ICMPv4 Echo Request" protocol=icmpv4:8,any dir=in action=allow
```

The supplied task specifically identifies the Windows 10 Firewall as a possible cause of failed Kali-to-Windows ping and provides this rule as the solution.

### Problem 2. Incorrect Network Attachment

If VMs cannot communicate, verify that Kali Linux, Windows 10, and Android 9 are all connected to the same VirtualBox NAT Network: **NatNetwork**.

### Problem 3. Incorrect Static IPv4 Configuration

Verify that each VM uses a unique address within `10.0.0.0/24` and that the gateway is `10.0.0.1`. Do not assign the same IP address to two virtual machines.

---

## 💡 What I Learned

- How to extend an isolated VirtualBox cybersecurity lab by adding multiple target virtual machines.
- How static IPv4 addressing allows each lab VM to be uniquely identified.
- How a shared NAT Network enables VM-to-VM communication while providing Internet connectivity.
- How Windows Firewall can affect ICMP-based connectivity testing.
- How to troubleshoot connectivity systematically by testing VM-to-VM and VM-to-Internet communication.
- How to document technical verification using screenshots and command-line test results.

---

## 🔐 Security & Ethical Use

This laboratory is intended strictly for educational and authorized cybersecurity testing. Only test systems owned by the author or systems for which explicit written permission has been granted. Do not use the lab or its tools to scan, exploit, or attack unauthorized systems or networks.

The original Phase-1 document establishes the lab as an isolated environment for authorized cybersecurity learning and testing.

---

## 🔗 Tools & Resources

- **Windows 10:** https://www.microsoft.com/en-us/software-download/windows10
- **android-x86_64-9.0-r2.iso**
  - **Link-1 Android OS:** https://www.android-x86.org/download
  - **Link-2 Android Source Page Release:** https://sourceforge.net/projects/android-x86/files/Release%209.0/

---
## 👤 Author

**Muhammad Abrar**  
Cybersecurity Professional B083B

**LinkedIn:** https://www.linkedin.com/in/muhammadabrar3/

## 📌 Project Information

- **Program:** Cybersecurity at Networkwalks
- **Project:** Cybersecurity & Pentesting Lab Setup — Phase-2, Week-1
- **Additional Task:** Windows 10 and Android 9 Virtual Machines
- **Platform:** Oracle VM VirtualBox
- **Week:** Week-1
- **Phase:** Phase-2
- **Mentor:** waqaskarimccie
- **Academy:** https://networkwalks.com/
