# 🏗️ Hybrid Enterprise Infrastructure & Architecture

**Architect:** John Belita  
**Location:** Calgary, AB | **Email:** johnalbertbelita@gmail.com  
**Certifications:** CompTIA Security+ (2026)

> 🚨 **NOTE:** This repository documents the architecture, deployment, and security hardening of a hybrid cloud enterprise IT environment. 
> ➡️ **To see how I actively manage, support, and troubleshoot this environment using Jira Service Management, view my [Enterprise Incident Management Repository](INSERT_YOUR_OTHER_REPO_LINK_HERE).**

---

## 🚀 Project Overview
This repository serves as documented proof of my hands-on proficiency in Systems Administration and Infrastructure Engineering. I engineered this comprehensive environment from the ground up to bridge the gap between formal Cybersecurity education and modern enterprise architecture.

### 🛠️ Tech Stack & Tools
* **Infrastructure:** Windows Server 2022, Windows 11 Pro, Oracle VirtualBox
* **Identity & Cloud:** Active Directory (AD DS), Microsoft 365, Google Workspace, Microsoft Entra ID
* **Endpoint Provisioning:** Microsoft Intune (MDM), WDS (PXE Boot)
* **Network & Security:** DHCP, DNS, MX/SPF/DMARC, Group Policy (GPO), NTFS/RBAC
* **Automation:** PowerShell (Bulk Provisioning)
* **Data Governance:** SharePoint Online, OneDrive, Intune Settings Catalog

---

## 🏗️ 1. Infrastructure Implementation & Networking
*Building the foundational server environment and establishing the domain.*

* **VM Configuration:** Provisioned Windows Server 2022 with optimized resource allocation (`vm-config-resources.png`).
* **Network Setup:** Configured a Static IP (10.0.0.10) for reliable DNS resolution and standardized the hostname (`YYC-DC-01`).
![Static IP](screenshots/server-static-ip.png)
![Hostname Configuration](screenshots/server-rename.png)

* **Domain Controller:** Promoted the server to a DC, establishing the forest root `belita.com`. 
![Domain Controller Promotion](screenshots/server-promotion-ad.png)

* **Client Join:** Deployed Windows 11 Pro endpoints, aligned DNS, and successfully authenticated to the local domain.
![Windows 11 Domain Join](screenshots/client-domain-join.png)

---

## 📂 2. Identity & Access Management (IAM)
*Designing the organizational structure and identity lifecycles.*

* **OU Architecture:** Designed a hierarchical Organizational Unit (OU) structure to logically separate Admins, Users, and Service Accounts.
![OU Architecture](screenshots/ad-ou-structure.png)

* **Access Restrictions:** Enforced strict Account Expiry and Logon Hour restrictions to mitigate unauthorized access outside business hours.
![Logon Hours](screenshots/ad-logon-hours.png)
![Logon Denied Validation](screenshots/client-logon-denied-msg.png)

* **PowerShell Automation:** Engineered a PowerShell script utilizing `Import-Csv` and `New-ADUser` to ingest HR spreadsheets and dynamically provision Active Directory accounts en masse.
![PowerShell Bulk Provisioning Execution](screenshots/powershell-ad-automation.png)

---

## 🗄️ 3. File Server, RBAC, and Security Hardening
*Implementing Role-Based Access Control and automating security protocols.*

* **File Share Hierarchy:** Designed a structured file share hierarchy and managed access strictly via Security Groups rather than individual users to ensure scalable management.
![RBAC Security Groups](screenshots/fs-rbac-groups.png)
![NTFS Permissions](screenshots/fs-ntfs-permissions.png)

* **Access Validation:** Confirmed "Access Denied" triggers for unauthorized users to verify NTFS permission inheritance and least-privilege access.
![Access Denied Validation](screenshots/fs-access-denied.png)

* **GPO Hardening:** Deployed domain-wide Group Policy Objects (GPOs) to enforce a strict 12-character minimum password policy, 90-day rotations, and a 3-attempt Account Lockout threshold.
![GPO Password Policy](screenshots/gpo-password-policy.png)
![GPO Lockout Policy](screenshots/gpo-lockout-policy.png)

---

## 💿 4. Automated OS Deployment (WDS)
*Architecting a PXE Boot environment for bare-metal OS provisioning.*

* **Image Management:** Extracted and published `boot.wim` and `install.wim` images to the Windows Deployment Services (WDS) Server. 
![WDS Console Configuration](screenshots/wds-console-images.png)

* **Advanced Troubleshooting:** Resolved UDP Port 67 conflicts with the co-hosted DHCP server and optimized the TFTP Maximum Block Size to fix packet fragmentation (Error 1460).
![DHCP Port Conflict Fix](screenshots/wds-dhcp-conflict-fix.png)
![TFTP Block Size Optimization](screenshots/wds-tftp-block-size.png)

* **PXE Execution:** Successfully deployed a Windows 10 image to a bare-metal client via PXE network boot.
![PXE Boot Client Execution](screenshots/wds-pxe-boot-screen.png)
![Windows Setup via Network](screenshots/wds-windows-setup.png)

---

## ☁️ 5. Hybrid Cloud Identity (Entra Connect)
*Bridging on-premise Active Directory with the Microsoft 365 Cloud.*

* **Entra Connect Sync:** Configured alternative UPN suffixes and deployed Microsoft Entra Connect Sync, establishing a secure bridge between local AD and Entra ID.
![Alternative UPN Configuration](screenshots/entra-ad-upn-config.png)

* **Verification:** Verified the successful replication of local user identities and password hashes into the Microsoft 365 Admin Center.
![Synced Active Users](screenshots/entra-m365-active-users.png)

---

## ☁️ 6. Cloud SaaS Administration (Google Workspace)
*Demonstrating platform-agnostic administration and strict email security.*

* **Tenant & Security:** Provisioned a Google Workspace tenant and managed organizational email routing by configuring secondary Email Aliases.
![GWS Admin Console & Aliases](screenshots/gws-admin-console.png)

* **Access Control:** Engineered Google Groups to act as centralized security boundaries for RBAC and enforced restricted sharing permissions.
![GWS Shared Drives and Groups](screenshots/gws-admin-console-groups.png)

---

## 📱 7. Modern Endpoint Management (Intune)
*Implementing Zero-Touch deployment and MDM policy enforcement.*

* **MDM Enrollment:** Orchestrated the enrollment of Windows 11 endpoints into Microsoft Intune (Entra ID Joined) for over-the-air (OTA) management.
![Intune Enrolled Devices Dashboard](screenshots/intune-enrolled-devices.png)

* **Configuration Profiles:** Engineered and deployed profiles to enforce enterprise security baselines.
![Intune Policy Applied Client-Side](screenshots/intune-policy-applied.png)

* **SharePoint Auto-Mount:** Engineered an Intune Settings Catalog profile leveraging the OneDrive administrative template to silently auto-mount secure SharePoint document libraries directly to remote users' File Explorers.
![SharePoint Auto-Mount in File Explorer](screenshots/sharepoint-intune-automount.png)
