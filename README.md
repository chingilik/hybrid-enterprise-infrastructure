# 🛡️ IT Support Enterprise Lab Portfolio

**Administrator:** John Belita

**Location:** Calgary, AB | **Email:** johnalbertbelita@gmail.com

## 🚀 Project Overview

Welcome to my technical portfolio. This repository serves as documented proof of my hands-on proficiency in enterprise IT support, bridging the gap between my formal Cybersecurity education and real-world Service Desk operations.

To demonstrate readiness for modern Systems Administration and IT Support roles, I engineered a comprehensive **Enterprise Home Lab**. This hybrid environment simulates the daily responsibilities of a Service Desk Analyst: from resolving complex end-user incidents and managing identity lifecycles (IAM) to enforcing endpoint security and administering hybrid cloud identities across **Microsoft 365**, **Intune**, and **Google Workspace**.

### 🛠️ Tech Stack & Tools

* **Infrastructure:** Windows Server 2022, Windows 11 Pro, Oracle VirtualBox

* **Identity & Cloud:** Active Directory (AD DS), Microsoft 365, Google Workspace, Microsoft Entra ID

* **Endpoint Management:** Microsoft Intune (MDM), Action1 RMM, WDS (PXE Boot), **Autopilot Reset**

* **Network & Security:** DHCP, DNS, MX/SPF/DMARC, Group Policy (GPO), NTFS/RBAC

* **ITSM & Automation:** Jira Service Management, Microsoft Teams ChatOps, PowerShell (Bulk Provisioning)

* **Data Governance:** SharePoint Online, OneDrive, Intune Settings Catalog (Auto-Mounting)

### 🏆 Core Competencies & Lab Achievements

* **Identity & Access Management (IAM):** Provisioned and managed user lifecycles across on-premise Active Directory and Entra ID (Azure AD), including automated bulk onboarding via PowerShell and RBAC enforcement.

* **Modern Device Lifecycle:** Orchestrated bare-metal OS deployments using WDS (PXE Boot) and executed Zero-Touch cloud-native device sanitization via Microsoft Intune Autopilot Reset.

* **Cloud Architecture & Data Governance:** Migrated legacy local file shares to SharePoint Online, utilizing Intune MDM configuration profiles to automatically mount secure cloud vaults to remote Windows 11 endpoints without VPNs.

* **Enterprise Security Hardening:** Enforced Zero Trust principles by configuring Conditional Access policies (Geofencing), managing MFA resets, deploying BitLocker via GPO, and mitigating vulnerabilities using Action1 RMM.

* **ITSM & Incident Response:** Managed a high-volume, SLA-driven service desk in Jira, resolving complex Tier 1/Tier 2 break-fix requests (Account Lockouts, M365 Mailbox Conversions, OneDrive Data Recovery, and Mail Flow traces).

## 🏗️ Section 0: Infrastructure Implementation

*Architecting the foundational server environment and promoting the Domain Controller.*

### 0.1 Virtual Machine & Host Configuration

Provisioned a Windows Server 2022 instance with optimized resource allocation. Installed VirtualBox Guest Additions for bi-directional integration and established secure Shared Folders to simulate mapped corporate network drives.

![VM Resource Config](screenshots/vm-config-resources.png)
![VM Guest Additions](screenshots/vm-guest-additions.png)
![VM Shared Folder](screenshots/vm-shared-folder.png)
***Figure 1: Virtual machine hardware, tools, and shared storage allocation.***

### 0.2 Network Configuration & Server Identity

Configured a **Static IP (10.0.0.10)** to ensure reliable DNS resolution for the domain and standardized the server hostname to align with enterprise naming conventions (`YYC-DC-01`).

![Server Rename](screenshots/server-rename.png)
![Static IP Setup](screenshots/server-static-ip.png)
***Figure 2: Assigning the standardized hostname and static IPv4 configuration.***

### 0.3 Domain Controller Promotion (YYC-DC-01)

Promoted the server to a Domain Controller, establishing the forest root `belita.com`. Verified successful directory deployment via **Active Directory Users and Computers (ADUC)**.

![AD DS Promotion](screenshots/server-promotion-ad.png)
![AD Verification](screenshots/ad-deployment-verified.png)
***Figure 3: Active Directory Domain Services role installation and verification.***

## 💻 Section 1: Client Workstation Deployment

*Joining a Windows 11 Pro endpoint to the corporate domain network.*

### 1.1 OS Installation & DNS Alignment

Deployed Windows 11 Pro to ensure AD domain-joining capabilities. Manually configured the Client DNS to point to the local Domain Controller (10.0.0.10) to ensure proper SRV record resolution.

![Windows 11 Install](screenshots/client-win11-install.png)
![Client DNS](screenshots/client-dns-config.png)
***Figure 4: Operating system selection and DNS alignment to the primary DC.***

### 1.2 Domain Join Authentication

Successfully authenticated and joined the workstation to the `belita.com` domain, verifying end-to-end connectivity.

![Domain Join](screenshots/client-domain-join.png)
![Client Login Success](screenshots/client-login-success.png)
***Figure 5: Windows 11 endpoint successfully joined and authenticated to the local domain.***

## 📂 Section 2: Identity & Access Management (IAM)

*Demonstrating user lifecycle management and organizational structure.*

### 2.1 OU Design & User Provisioning

Designed a hierarchical Organizational Unit (OU) structure to logically separate Admins, Users, and Service Accounts. Provisioned accounts utilizing standardized corporate naming conventions.

![Active Directory OU Structure](screenshots/ad-ou-structure.png)
![AD User Neil](screenshots/ad-user-neil.png)
***Figure 6: Segmented OU architecture and standard account provisioning.***

### 2.2 Security Restrictions

Enforced strict **Account Expiry** and **Logon Hour** restrictions to mitigate the risk of unauthorized access outside of authorized business hours. Validated these policies on the client endpoint.

![AD Logon Hours](screenshots/ad-logon-hours.png)
![Account Restrictions](screenshots/ad-account-expiry.png)
![Logon Denied Msg](screenshots/client-logon-denied-msg.png)
***Figure 7: Enforcing time-based logon restrictions and validating the access denial.***

## ⚡ Section 3: Scripting & Automation (PowerShell)

*Optimizing the HR-to-IT onboarding pipeline to eliminate manual data entry errors.*

### 3.1 Bulk Identity Provisioning

Engineered a **PowerShell automation script** utilizing `Import-Csv` and `New-ADUser` to ingest standardized HR spreadsheets and dynamically provision Active Directory accounts en masse. The script dynamically generates `SamAccountNames` and enforces Zero Trust security baselines by setting complex temporary passwords and triggering the `-ChangePasswordAtLogon $true` parameter.

![PowerShell AD Automation](screenshots/powershell-ad-automation.png)
***Figure 8: Executing the bulk onboarding script via PowerShell ISE.***

## 🗄️ Section 4: File Server & RBAC

*Implementing Role-Based Access Control and secure data segregation.*

### 4.1 Security Groups & File Share Hierarchy

Designed a structured file share hierarchy and managed access strictly via Security Groups (e.g., `IT_Access`, `HR_Access`) rather than individual users to ensure scalable management.

![File Share Hierarchy](screenshots/fs-share-hierarchy.png)
![RBAC Groups](screenshots/fs-rbac-groups.png)
![NTFS Permissions](screenshots/fs-ntfs-permissions.png)
***Figure 9: Designing the share directory, creating RBAC groups, and applying explicit NTFS permissions.***

### 4.2 Access Validation

Confirmed "Access Denied" triggers for unauthorized users to verify NTFS permission inheritance and least-privilege access.

![Access Denied](screenshots/fs-access-denied.png)
***Figure 10: Verifying successful RBAC enforcement (Access Denied for unauthorized user).***

## ⚙️ Section 5: Group Policy & Hardening

*Automating security protocols across the domain utilizing GPOs.*

### 5.1 Password Policy & Account Lockout

Deployed domain-wide Group Policy Objects (GPOs) to enforce a strict 12-character minimum password policy with 90-day rotations. Configured a 3-attempt Account Lockout threshold to mitigate brute-force attacks.

![Password Policy](screenshots/gpo-password-policy.png)
![Lockout Policy](screenshots/gpo-lockout-policy.png)
***Figure 11: Domain-wide password complexity and account lockout thresholds.***

### 5.2 Security Validation & Account Recovery

Simulated a brute-force attempt against a user account and verified the "Account Locked" message on the client-side. Executed standard IT procedures to unlock the account within Active Directory.

![Account Locked Message](screenshots/client-locked-out-msg.png)
![AD Unlock Verify](screenshots/ad-unlock-verify.png)
![AD Unlock Account](screenshots/ad-unlock-account.png)
***Figure 12: Client-side lockout validation followed by administrative account recovery in ADUC.***

## ☁️ Section 6: Modern Fleet Management (Action1 RMM)

*Executing cloud-based vulnerability remediation, automated deployments, and remote support.*

### 6.1 RMM Agent Deployment via GPO

Hosted the Action1 agent installer in a centralized, read-only network share and utilized Group Policy to automatically push the management agent to all domain-joined endpoints.

![Server Deploy Share](screenshots/server-deploy-share.png)
![GPO Deploy Agent](screenshots/gpo-deploy-agent.png)
![Client Agent Installed](screenshots/client-agent-installed.png)
***Figure 13: Silent, automated deployment of the RMM management agent via Group Policy.***

### 6.2 Hybrid AD Connector Integration

Configured the Action1 Active Directory Connector to sync on-premise OUs and assets to the cloud, ensuring real-time fleet visibility in the centralized dashboard.

![Action1 AD Connector](screenshots/action1-ad-connector-config.png)
![Action1 Endpoints](screenshots/action1-endpoints-dashboard.png)
***Figure 14: Bridging on-premise AD assets to the cloud RMM platform.***

### 6.3 Vulnerability Management & Patching

Identified critical **CVEs** and missing updates through automated vulnerability scans. Executed patch remediation workflows to secure outdated endpoints and generated compliance reports.

![CVE List](screenshots/action1-cve-list.png)
![Missing Updates](screenshots/action1-missing-updates.png)
![Patch Remediation](screenshots/action1-patch-remediation.png)
![Compliance Report](screenshots/action1-compliance-report.png)
***Figure 15: Full patch lifecycle—from CVE discovery to automated deployment and compliance reporting.***

### 6.4 Automated Software Deployment

Silently deployed 3rd-party applications to client endpoints without manual intervention via the RMM App Store task automation.

![Deploy 7Zip Task](screenshots/action1-deploy-7zip-task.png)
![Client 7Zip Installed](screenshots/client-7zip-installed.png)
***Figure 16: Zero-touch deployment of 7-Zip executing on the remote endpoint.***

### 6.5 Remote Administration (C$ Shares)

Utilized administrative network shares (C$) to remotely access and troubleshoot client file systems over the network without interrupting the end-user's active session.

![Admin Share C Drive](screenshots/admin-share-c-drive.png)
***Figure 17: Remote administrative access via hidden C$ pathways.***

## 🎫 Section 7: ITSM & ChatOps (Jira + Teams)

*Managing the incident lifecycle and reducing MTTA (Mean Time To Acknowledge).*

### 7.1 Customer Portal & AD Sync

Deployed a customized Jira Customer Portal and synchronized Active Directory users into the ITSM database to track historical requests.

![Jira Portal View](screenshots/jira-portal-view.png)
![Jira Customers Sync](screenshots/jira-customers-sync.png)
***Figure 18: End-user self-service portal and AD identity synchronization.***

### 7.2 SLA Engineering & Ticket Triage

Engineered custom Jira SLAs (15m Response / 2h Resolution). Utilized **JQL** to properly prioritize critical incidents, assigning tickets to appropriate queues and actively tracking SLA metrics.

![Jira Queue View](screenshots/jira-queue-view.png)
![Ticket Assignment](screenshots/jira-ticket-assignment.png)
![SLA Triage](screenshots/jira-ticket-sla-triage.png)
***Figure 19: Triaging the IT Service Desk queue and monitoring SLA countdowns.***

### 7.3 Incident Resolution (Break/Fix)

Documented technical root causes within Internal Notes while providing clear, jargon-free resolution updates to end-users for both Service Requests (software installs) and Incidents (broken file permissions).

![Incident Resolved](screenshots/jira-ticket-incident-resolved.png)
![HR Folder Fix](screenshots/ad-hr-folder-fix.png)
![Request Resolved](screenshots/jira-ticket-request-resolved.png)
***Figure 20: Restoring an accidentally deleted RBAC group and resolving the associated Jira tickets.***

### 7.4 Microsoft Teams Integration

Integrated **Jira with Microsoft Teams** via Webhooks, enabling end-users to generate IT tickets directly from chat messages. IT Agents receive automated alerts in a dedicated Teams channel.

![Jira Teams Integration](screenshots/jira-teams-integration.png)
![Teams Ticket Creation](screenshots/teams-ticket-creation.png)
***Figure 21: Seamless creation of a trackable Jira incident directly from MS Teams.***

## 💿 Section 8: Automated OS Deployment (WDS)

*Architecting a PXE Boot environment for bare-metal OS provisioning.*

### 8.1 Image Management & Troubleshooting

Extracted and published `boot.wim` and `install.wim` images to the WDS Server. Resolved **UDP Port 67** conflicts with the co-hosted DHCP server and optimized the TFTP Maximum Block Size to fix packet fragmentation.

![WDS Console Images](screenshots/wds-console-images.png)
![WDS DHCP Conflict Fix](screenshots/wds-dhcp-conflict-fix.png)
![TFTP Block Size](screenshots/wds-tftp-block-size.png)
***Figure 22: Provisioning WDS images and executing advanced network boot troubleshooting.***

### 8.2 PXE Boot Execution

Successfully deployed a Windows 10 image to a bare-metal client via PXE network boot.

![PXE Boot Screen](screenshots/wds-pxe-boot-screen.png)
![Windows Setup](screenshots/wds-windows-setup.png)
***Figure 23: Bare-metal client successfully fetching the boot image and launching Windows Setup.***

## ☁️ Section 9: Hybrid Cloud Identity (Entra ID)

*Bridging on-premise Active Directory with the Microsoft 365 Cloud.*

### 9.1 Entra Connect Synchronization

Configured alternative UPN suffixes and deployed **Microsoft Entra Connect Sync**. Verified the successful replication of local AD user identities and password hashes into the Microsoft 365 Admin Center.

![Entra UPN Config](screenshots/entra-ad-upn-config.png)
![M365 Active Users Sync](screenshots/entra-m365-active-users.png)
***Figure 24: Establishing UPNs and syncing on-premise Active Directory accounts to Microsoft 365.***

### 9.2 Mail Flow & Message Tracing

Tested successful cloud routing and utilized Exchange Admin Center Message Traces to investigate inbound/outbound mail flow health.

![M365 Test Email](screenshots/entra-m365-test-email.png)
![Message Trace](screenshots/exchange-message-trace.png)
***Figure 25: Validating Exchange Online routing and auditing delivery via Message Trace.***

## 🛠️ Section 10: Service Desk Troubleshooting (Break/Fix)

*Simulated resolution of common Tier 1/Tier 2 support requests typical of an MSP environment.*

### 10.1 Ticket 1: MFA Reset (Entra ID)

* **Scenario:** User lost access to their Authenticator app after replacing their mobile device.

* **Resolution:** Navigated to Entra ID Authentication Methods, executed **Require re-register MFA**, and revoked active sessions to secure the account during the device transition.

![Entra ID MFA Reset Ticket](screenshots/ticket-mfa-reset.png)
***Figure 26: Tracking the MFA Reset incident in the ITSM portal.***

### 10.2 Ticket 2: Secure Offboarding (Mailbox Conversion)

* **Scenario:** Immediate termination of an employee requiring access revocation and data retention.

* **Resolution:** Disabled the AD account, forced a Delta Sync, and converted the Exchange Online inbox to a **Shared Mailbox** to retain historical data while successfully reclaiming the paid M365 Business Premium license.

![Offboarding Ticket](screenshots/ticket-offboarding.png)
![AD Offboarding](screenshots/Off-boarding-neil.png)
![Shared Mailbox Config](screenshots/off-boarding-neil-shared-mail.png)
![Exchange Shared Mailbox](screenshots/exchange-shared-mailbox.png)
***Figure 27: End-to-end employee offboarding workflow and data retention configuration.***

### 10.3 Ticket 3: Data Recovery (OneDrive)

* **Scenario:** User permanently deleted critical files from their OneDrive and emptied the standard local Recycle Bin.

* **Resolution:** Generated an administrative access link to the user's OneDrive environment and successfully recovered the hard-deleted files directly from the hidden **Second-Stage Recycle Bin**.

![OneDrive Recovery Ticket](screenshots/ticket-onedrive-recovery.png)
***Figure 28: Logging the ticket and executing OneDrive administrative recovery workflow.***

## ☁️ Section 11: Cloud SaaS Administration (Google Workspace)

*Demonstrating platform-agnostic administration and strict email security configuration.*

### 11.1 Tenant Provisioning & Alias Management

Provisioned a Google Workspace tenant. Executed standardized user provisioning workflows and managed organizational email routing by configuring secondary **Email Aliases** (`itdirector@belita.online`), providing flexible mail delivery without supplementary licensing costs.

![GWS Email Alias Configuration](screenshots/gws-admin-console.png)
***Figure 29: Managing Google Workspace users and organizational email aliases.***

### 11.2 RBAC & Access Control

Engineered **Google Groups** (`execs@...`) to act as centralized security boundaries for Role-Based Access Control (RBAC). Administered organizational access policies and enforced restricted sharing permissions on sensitive data.

![GWS Group Membership](screenshots/gws-admin-console-groups.png)
***Figure 30: Utilizing Google Groups for department-level access control.***

## 📱 Section 12: Modern Endpoint Management (Microsoft Intune)

*Implementing Zero-Touch deployment and MDM policy enforcement for a remote workforce.*

### 12.1 MDM Enrollment & Entra ID Join

Orchestrated the enrollment of Windows 11 endpoints into **Microsoft Intune** (Entra ID Joined), enabling over-the-air (OTA) management without requiring a VPN or local Domain Controller line-of-sight.

![Intune Device Management](screenshots/intune-enrolled-devices.png)
***Figure 31: Windows endpoint successfully enrolled and managed via Microsoft Intune.***

### 12.2 Configuration Profiles & Compliance

Engineered and deployed **Configuration Profiles** to enforce enterprise security baselines, including automated screen-lock timers, Microsoft 365 app deployments, and alphanumeric password complexity.

![Intune Policy Enforcement](screenshots/intune-policy-applied.png)
***Figure 32: Intune successfully pushing configuration profiles to the client device.***

## ☁️ Section 13: Cloud Data Governance (SharePoint & Intune)

*Modernizing legacy file infrastructure by migrating data to SharePoint Online and mapping it to remote workers without a VPN.*

### 13.1 Secure Cloud Vault Creation

Provisioned a restricted SharePoint Team Site (`Finance Secure`) and enforced external sharing protocols to prevent data leakage. Extracted the Tenant and Library IDs necessary for MDM payload deployment.

### 13.2 Zero-Touch File Mapping (Settings Catalog)

Engineered an Intune Configuration Profile leveraging the OneDrive administrative template to silently **auto-mount the SharePoint document library**. Targeted the policy specifically to the `Finance_Access` Entra ID security group to enforce Role-Based Access Control on the client side.

### 13.3 Client-Side Verification

Validated the successful payload delivery and Microsoft 8-hour timer bypass, resulting in the restricted Finance folder mapping natively inside the remote user's Windows 11 File Explorer.

![SharePoint Intune Auto-Mount](screenshots/sharepoint-intune-automount.png)
***Figure 33: The secure SharePoint library automatically pushed and mounted directly inside File Explorer via Intune.***

## 🔄 Section 14: Zero-Touch Recovery (Intune Autopilot Reset)

*Leveraging cloud-native management to remotely re-image and sanitize endpoints for employee transitions or incident recovery.*

### 14.1 Remote Device Sanitization & Re-provisioning

Executed a **Microsoft Autopilot Reset** from the Intune Admin Center to remotely sanitize a Windows 11 endpoint. Verified the successful automated re-provisioning of the workstation back to a pristine, corporate-ready state while maintaining the device's Entra ID join-status and security policies.

*(Lab execution verified via Intune Audit Logs)*

## 🎓 Section 15: Capstone - End-to-End Employee Onboarding

*Executing a complete, enterprise-grade onboarding process by unifying ITSM, AD DS, Entra ID, M365, and Intune into a single operational pipeline.*

### 15.1 The Workflow Execution

1. **Intake:** Generated a New Hire Request in Jira Service Management for 'Jennifer Belita'.

2. **Identity:** Utilized the custom PowerShell script to automatically build the AD account, assign it to the `Finance_Access` security group, and enforce a temporary password.

3. **Cloud Sync:** Executed a forced Delta Sync (`Start-ADSyncSyncCycle`) to immediately push the identity to Entra ID.

4. **Licensing:** Assigned an M365 Business Premium license via the cloud admin center to provision Exchange Online.

5. **Endpoint & Access:** Authenticated into the Intune-managed Windows 11 laptop using cloud credentials, verified SSO via Edge, and successfully accessed the locked-down `\\10.0.0.10\Finance` local file share via inherited Kerberos authentication.

6. **Closure:** Documented the technical steps in the Jira internal notes and resolved the ticket within SLA limits.

![Capstone End-to-End Proof](screenshots/capstone-onboarding.png)
***Figure 35: The completed pipeline—Local AD identity synced to M365, licensed, and ticket resolved.***

## 📜 Education & Certifications

* **CompTIA Security+** | Certified Jan 2026

* **Diploma in Cybersecurity** | ABM College, Calgary, AB | Graduated May 2025

* **Network Specialist Training** | SAIT, Calgary, AB | 2019

## 📫 Contact

**John Belita** | Calgary, AB | johnalbertbelita@gmail.com
