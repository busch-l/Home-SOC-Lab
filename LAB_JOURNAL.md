# Home Security Lab Journal - Lucas Busch

This journal documents the setup and configuration of a virtualized cybersecurity lab designed for defensive analysis and offensive security practice.

---

## Phase 1: Initial VM Provisioning and Network Configuration

### **Date: 8/4/2025**

**Objective:** To provision the core virtual machines within Oracle VirtualBox and establish the foundational private network topology.

### **1.1 - Provisioning the Attacker VM: Kali Linux**

**Description:**
The primary offensive machine for this lab is Kali Linux. I downloaded the pre-built VirtualBox image (`.7z` archive) from the official Kali website. This method was chosen for efficiency, as it bypasses the need for a full OS installation and includes VirtualBox Guest Additions for better integration. The VM was added to VirtualBox by registering its existing `.vbox` file.

**Network Configuration:** The VM's primary network adapter (Adapter 1) was configured to connect to the **Internal Network** named `lab-net`. This isolates the machine within our lab environment.

**Configuration Screenshot:**
![Kali Linux Network Settings](kali-linux-in-virtualbox.png)

---

### **1.2 - Provisioning the SIEM: Wazuh**

**Description:**
The core of the lab's defensive monitoring capabilities is the Wazuh Security Information and Event Management (SIEM) platform. I downloaded the official Open Virtual Appliance (`.ova`) file, which contains a fully configured Wazuh server, indexer, and dashboard. The VM was added using VirtualBox's "Import Appliance" feature.

**Network Configuration:** Post-import, the VM's network adapter was switched to the **Internal Network** named `lab-net`. This places the SIEM server on the same private network as the other lab components, allowing it to receive logs from agents.

**Configuration Screenshot:**
![Wazuh Network Settings](Wazuh_in_VirtualBox.png)

---

### **1.3 - Provisioning the Vulnerable Target: Metasploitable2**

**Description:**
To serve as a safe and intentionally vulnerable target for security testing, I provisioned the Metasploitable2 VM. This VM is distributed as a VMware machine, so a new VirtualBox machine was created to use the existing `Metasploitable.vmdk` virtual hard disk.

**Process:**
A new VirtualBox VM was created with a Linux/Ubuntu (64-bit) profile and 1024 MB of RAM. In the hard disk setup wizard, the existing `Metasploitable.vmdk` file was selected as the primary disk.

**Network Configuration:** The new VM's network adapter was then configured to use the **Internal Network** named `lab-net`.

**Configuration Screenshot:**
![Metasploitable2 Network Settings](Metasploitable_in_VirtualBox.png)

---

### **1.4 - Provisioning the Gateway/Firewall: pfSense**

**Description:**
The network gateway, router, and firewall for the entire lab is a pfSense VM. This will handle DHCP services for the internal lab network and manage all traffic flowing in and out. A new VM was created, and the pfSense `.iso` installer will be used for setup in the next phase.

**Network Configuration:**
This VM requires a dual-homed network configuration:
*   **Adapter 1 (WAN):** Configured as **NAT** to receive an internet connection from the host machine.
*   **Adapter 2 (LAN):** Configured as **Internal Network** with the name `lab-net` to connect to the internal lab.

**Configuration Screenshot:**
*(Note: The following image shows the dual-adapter setup for pfSense)*
![pfSense Network Settings](pfsense_in_VirtualBox.png)

---
**End of Phase 1.** The lab environment is now fully provisioned and networked. The next phase will be the sequential power-on and initial configuration of each component.
