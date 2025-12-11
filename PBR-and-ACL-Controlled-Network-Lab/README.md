# PBR-and-ACL-Controlled-Network-Lab

## 🌐 Overview

This repository contains the configuration files and documentation for a complex network topology designed to demonstrate **High Availability (HA) Failover** using Policy-Based Routing (PBR) and multiple layers of **Access Control List (ACL)** security.

The lab simulates a small enterprise environment with a primary/secondary internet gateway, internal security zones, and controlled remote access.

## ✨ Key Features & Technologies Demonstrated

Based on the provided diagram and tasks, this project showcases expertise in:

1.  **High Availability (HA) & Redundancy:**
    * Automatic link failover logic: Traffic is routed via the **Primary** link (R1) and automatically switches to the **Secondary** link (R2) upon failure.
    * Automatic link restoration: Traffic reverts to the Primary link once it is restored ("*the primary link will resume as the active connection*"). This is typically achieved using **Route-Maps (PBR)** and IP SLA/Tracking.

2.  **Access Control Lists (ACLs) & Security:**
    * **Internet Restriction:** Preventing all internal users (PC1, PC2, PC3) from accessing the internet, except where specifically permitted.
    * **Inter-VLAN Communication Control:** Blocking communication between specific internal devices (e.g., PC3 should not be able to ping PC1 and PC2).
    * **Service-Specific Blocking:** Denying PC2 access to Router R2 (ping/Telnet/SSH will be blocked).

3.  **Secure Remote Management (SSH):**
    * **VTY Line Hardening:** Configuring `line vty 0 2` for secure management.
    * **User Segmentation:** Allowing only users from the **R3 LAN network (172.16.1.0/24)** to log in via SSH, blocking all external SSH attempts.
    * **Security Configuration:** Setting up `ip domain name`, `crypto key generate rsa`, and SSH version 2.

4.  **Network Services:**
    * Configuring **VLANs** and IP Addressing.
    * Implementing a **DHCP Server** on Router R3 (`R3 ssh+dhcp-server`) to dynamically assign IP addresses to PCs.

## 💻 Topology Diagram

The network topology consists of four main routers (R1, R2, R3, R4), an internal switch, and three virtual client PCs (PC1, PC2, PC3).

*(In a final GitHub README, you would typically insert the image of your network diagram here.)*

## 🛠️ Configuration Details & Tasks Completed

The following tasks were successfully implemented and verified:

| Task Category | Description (Based on Configuration List) | Implementation Note |
| :--- | :--- | :--- |
| **Basic Setup** | VLAN, IP Addressing, DHCP Server | Standard configuration on R3. |
| **Routing** | Static/Default Routing, Route-Map (PBR) | Implemented static/default routes and `route-map` for failover logic. |
| **Security** | SSH, Telnet | SSH v2 configured with restricted VTY access based on source IP. |
| **ACL-1** | PC1 not allowed internet access | Implemented Extended ACL to block traffic originating from PC1. |
| **ACL-2** | PC2 on access and ping block for R2 | Implemented ACL to deny PC2's access and ping attempts to R2. |
| **ACL-3** | PC3 cannot ping PC1 & PC2 | Implemented ACL to block ICMP traffic from PC3 to PC1/PC2. |
| **ACL-4** | SSH use for LAN, other not allow | Implemented a standard or extended ACL on the VTY lines of R3. |
| **PBR/HA** | Route-map for primary/secondary link failover. | Configured on the core router (likely R3) to enforce policy routing. |

## 🚀 How to Run the Lab

This lab was constructed using **GNS3** or **Cisco Packet Tracer**.

1.  **Download:** Download the project file (`.gns3project` or similar) from the link below.
2.  **Load:** Open the file in your preferred network emulator.
3.  **Start:** Start all devices and verify the initial connectivity (all PCs should receive DHCP addresses).
4.  **Verification:** Test the failover mechanism by shutting down the interface connecting R3 to R1 and observing the traffic path change.

### Direct Download

For convenience, you can download the entire project file here:

[**Download PBR-and-ACL-Controlled-Network-Lab.zip**](https://github.com/shuvacst/shuvacst.projects/raw/refs/heads/main/PBR-and-ACL-Controlled-Network-Lab/PBR-and-ACL-Controlled-Network-Lab.zip)

---

## 🔑 Configuration Snippets (Example: Router R3)

*(In a real README, you would provide key configuration snippets here, especially the `route-map` and VTY access list.)*

```cisco
! Example Route-map for PBR on R3
route-map FAILOVER_PBR permit 10
 match ip address PRIMARY_SOURCE_ACL
 set ip next-hop 10.1.1.2 ! R1's IP
!
route-map FAILOVER_PBR permit 20
 match ip address SECONDARY_SOURCE_ACL
 set ip next-hop 192.168.1.2 ! R2's IP
! (etc.)
