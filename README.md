# 🏗️ Hybrid Enterprise Infrastructure & Architecture

**Architect:** John Belita

**Location:** Calgary, AB | **Email:** johnalbertbelita@gmail.com

**Certifications:** CompTIA Security+ (2026)

> 🚨 **NOTE:** This repository documents the architecture, deployment, and security hardening of a hybrid cloud enterprise IT environment.
> ➡️ **To see how I actively manage, support, and troubleshoot this environment using Jira Service Management, view my [Enterprise Incident Management Repository](INSERT_YOUR_OTHER_REPO_LINK_HERE).**

## 🚀 Project Overview

This repository serves as documented proof of my hands-on proficiency in Systems Administration and Infrastructure Engineering. I engineered this comprehensive environment from the ground up to bridge the gap between formal Cybersecurity education and modern enterprise architecture.

### 🌐 Network Architecture & IP Schema

To ensure reliable communication and strict isolation, the environment was built on a dedicated `/16` NAT Network topology.

* **Network Segment:** `10.0.0.0/16` (Subnet Mask: `255.255.0.0`)

* **Default Gateway / Edge:** `10.0.0.1`

* **Primary Domain Controller (YYC-DC-01):** `10.0.0.10` (Static)

* **DHCP Scope (Client Endpoints):** `10.0.0.100` – `10.0.0.200`

* **Internal Active Directory Forest:** `belita.com`

* **Microsoft 365 / Entra Tenant:** `belitalab.onmicrosoft.com`

* **Google Workspace Tenant:** `belita.online`

### 🛠️ Tech Stack & Tools

* **Infrastructure:** Windows Server 2022, Windows 11 Pro, Oracle VirtualBox

* **Identity & Cloud:** Active Directory (AD DS), Microsoft 365, Google Workspace, Microsoft Entra ID

* **Endpoint Provisioning:** Microsoft Intune (MDM), WDS (PXE Boot)

* **Network & Security:** DHCP, DNS, MX/SPF/DMARC, Group Policy (GPO), NTFS/RBAC

* **Automation:** PowerShell (Bulk Provisioning)

* **Data Governance:** SharePoint Online, OneDrive, Intune Settings Catalog

## 🏗️ 1. Infrastructure Implementation & Networking

*Building the foundational server environment and establishing the domain.*

* **VM Configuration:** Provisioned Windows Server 2022 with optimized resource allocation to host core infrastructure roles.

* **Network Setup:** Configured a Static IP (`10.0.0.10`) for reliable DNS resolution and standardized the hostname (`YYC-DC-01`) to align with enterprise location-based naming conventions.

![Static IPv4 Assignment](screenshots/server-static-ip.png)
> **Figure 1.1: Static IPv4 Assignment** - Securing the Domain Controller's network identity with explicit DNS loopback pointers to establish the forest root.

![Hostname Standardization](screenshots/server-rename.png)
> **Figure 1.2: Hostname Standardization** - Renaming the server to YYC-DC-01, establishing a scalable, location-aware naming convention.

* **Domain Controller:** Promoted the server to a DC, establishing the forest root `belita.com`.

![AD DS Promotion](screenshots/server-promotion-ad.png)
> **Figure 1.3: AD DS Promotion** - Successfully promoting the Windows Server to a Domain Controller for the internal belita.com domain.

* **Client Join:** Deployed Windows 11 Pro endpoints, aligned local DNS to the DC (`10.0.0.10`), and successfully authenticated to the local domain.

![Domain Authentication](screenshots/client-domain-join.png)
> **Figure 1.4: Domain Authentication** - Verifying end-to-end connectivity and successful domain authentication from the Windows 11 client endpoint.

## 📂 2. Identity & Access Management (IAM)

*Designing the organizational structure and identity lifecycles.*

* **OU Architecture:** Designed a hierarchical Organizational Unit (OU) structure to logically separate Admins, Users, and Service Accounts.

![Organizational Unit Architecture](screenshots/ad-ou-structure.png)
> **Figure 2.1: Organizational Unit Architecture** - Designing a scalable OU hierarchy separating Service Accounts, Standard Users, and Administrative Tiers for granular policy application.

* **Access Restrictions:** Enforced strict Account Expiry and Logon Hour restrictions to mitigate unauthorized access outside business hours.

![Time-Based Access Control - Server Side](screenshots/ad-logon-hours.png)
![Time-Based Access Control - Client Side](screenshots/client-logon-denied-msg.png)
> **Figure 2.2: Time-Based Access Control** - Enforcing strict logon hour restrictions within ADUC (Top) and validating the policy enforcement at the client endpoint (Bottom) to mitigate after-hours insider threats.

* **PowerShell Automation:** Engineered a PowerShell script utilizing `Import-Csv` and `New-ADUser` to ingest HR spreadsheets and dynamically provision Active Directory accounts en masse.

![PowerShell Bulk Provisioning](screenshots/powershell-ad-automation.png)
> **Figure 2.3: PowerShell Bulk Provisioning** - Executing an automated ingestion script to parse HR CSV data, dynamically generate SamAccountNames, and enforce mandatory initial password resets (Zero Trust).

## 🗄️ 3. File Server, RBAC, and Security Hardening

*Implementing Role-Based Access Control and automating security protocols.*

* **File Share Hierarchy:** Designed a structured file share hierarchy and managed access strictly via Security Groups rather than individual users to ensure scalable management.

![RBAC Security Groups](screenshots/fs-rbac-groups.png)
![NTFS Permissions](screenshots/fs-ntfs-permissions.png)
> **Figure 3.1: RBAC & NTFS Permissions** - Implementing Role-Based Access Control (RBAC) by mapping explicit NTFS folder permissions directly to Active Directory Security Groups.

* **Access Validation:** Confirmed "Access Denied" triggers for unauthorized users to verify NTFS permission inheritance and least-privilege access.

![Least Privilege Validation](screenshots/fs-access-denied.png)
> **Figure 3.2: Least Privilege Validation** - Confirming successful inheritance blocking with a hard 'Access Denied' trigger for an unauthorized user account.

* **GPO Hardening:** Deployed domain-wide Group Policy Objects (GPOs) to enforce a strict 12-character minimum password policy, 90-day rotations, and a 3-attempt Account Lockout threshold.

![Security Policy Hardening - Password](screenshots/gpo-password-policy.png)
![Security Policy Hardening - Lockout](screenshots/gpo-lockout-policy.png)
> **Figure 3.3: Security Policy Hardening (GPO)** - Deploying domain-wide Group Policy Objects to enforce enterprise-grade password complexity and strict brute-force lockout thresholds.

## 💿 4. Automated OS Deployment (WDS)

*Architecting a PXE Boot environment for bare-metal OS provisioning.*

* **Image Management:** Extracted and published `boot.wim` and `install.wim` images to the Windows Deployment Services (WDS) Server.

![WDS Image Management](screenshots/wds-console-images.png)
> **Figure 4.1: WDS Image Management** - Extracting and mounting Windows PE boot environments and OS payloads to the Windows Deployment Services console.

* **Advanced Troubleshooting:** Resolved UDP Port 67 conflicts with the co-hosted DHCP server and optimized the TFTP Maximum Block Size to fix packet fragmentation (Error 1460).

![Network Protocol Optimization - DHCP](screenshots/wds-dhcp-conflict-fix.png)
![Network Protocol Optimization - TFTP](screenshots/wds-tftp-block-size.png)
> **Figure 4.2: Network Protocol Optimization** - Resolving UDP Port 67 listening conflicts between DHCP/WDS (Top) and adjusting TFTP Maximum Block Sizes to eliminate virtual packet fragmentation (Bottom).

* **PXE Execution:** Successfully deployed a Windows 10 image to a bare-metal client via PXE network boot.

![Zero-Touch PXE Boot Execution 1](screenshots/wds-pxe-boot-screen.png)
![Zero-Touch PXE Boot Execution 2](screenshots/wds-windows-setup.png)
> **Figure 4.3: Zero-Touch PXE Boot Execution** - A bare-metal client successfully requesting a DHCP IP, fetching the WDS boot image over the network, and initiating the automated Windows setup environment.

## ☁️ 5. Hybrid Cloud Identity (Entra Connect)

*Bridging on-premise Active Directory with the Microsoft 365 Cloud.*

* **Entra Connect Sync:** Configured alternative UPN suffixes (`belitalab.onmicrosoft.com`) and deployed Microsoft Entra Connect Sync, establishing a secure bridge between local AD and Entra ID.

![UPN Suffix Routing](screenshots/entra-ad-upn-config.png)
> **Figure 5.1: UPN Suffix Routing** - Appending the external cloud domain as a valid Alternate UPN Suffix within local Active Directory Domains and Trusts to prepare for identity syncing.

* **Verification:** Verified the successful replication of local user identities and password hashes into the Microsoft 365 Admin Center.

![Entra Connect Synchronization](screenshots/entra-m365-active-users.png)
> **Figure 5.2: Entra Connect Synchronization** - Verifying the successful automated replication of local AD identities into the Microsoft 365 Admin Center for seamless Single Sign-On (SSO).

## ☁️ 6. Cloud SaaS Administration (Google Workspace)

*Demonstrating platform-agnostic administration and strict email security.*

* **Tenant & Security:** Provisioned a Google Workspace tenant and managed organizational email routing by configuring secondary Email Aliases.

![GWS Tenant & Email Aliases](screenshots/gws-admin-console.png)
> **Figure 6.1: GWS Tenant & Email Aliases** - Managing organizational identities and configuring secondary email aliases to route department-level communications without supplemental licensing.

* **Access Control:** Engineered Google Groups to act as centralized security boundaries for RBAC and enforced restricted sharing permissions.

![Collaborative Access Control](screenshots/gws-admin-console-groups.png)
> **Figure 6.2: Collaborative Access Control** - Engineering Google Groups to act as centralized security boundaries for RBAC across enterprise Shared Drives.

## 📱 7. Modern Endpoint Management (Intune)

*Implementing Zero-Touch deployment and MDM policy enforcement.*

* **MDM Enrollment:** Orchestrated the enrollment of Windows 11 endpoints into Microsoft Intune (Entra ID Joined) for over-the-air (OTA) management.

![MDM Device Enrollment](screenshots/intune-enrolled-devices.png)
> **Figure 7.1: MDM Device Enrollment** - Orchestrating the transition from legacy AD joining to Microsoft Entra ID Join, bringing endpoints under comprehensive cloud management.

* **Configuration Profiles:** Engineered and deployed profiles to enforce enterprise security baselines.

![Configuration Profile Enforcement](screenshots/intune-policy-applied.png)
> **Figure 7.2: Configuration Profile Enforcement** - Validating the successful deployment of Intune security baselines natively on the client Operating System.

* **SharePoint Auto-Mount:** Engineered an Intune Settings Catalog profile leveraging the OneDrive administrative template to silently auto-mount secure SharePoint document libraries directly to remote users' File Explorers.

![Zero-Touch Data Governance](screenshots/sharepoint-intune-automount.png)
> **Figure 7.3: Zero-Touch Data Governance** - Leveraging an Intune payload to silently auto-mount secure SharePoint cloud vaults natively within the remote user's File Explorer, eliminating the need for traditional VPNs.
