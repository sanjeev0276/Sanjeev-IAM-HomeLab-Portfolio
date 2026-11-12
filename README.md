# IAM-HomeLab-Portfolio - Hands-On Architecture and Implementation
![Active Directory](https://img.shields.io/badge/Active%20Directory-On--Prem%20Identity-0078D4?style=for-the-badge&logo=microsoft&logoColor=white)
![Microsoft Entra ID](https://img.shields.io/badge/Microsoft%20Entra%20ID-Hybrid%20Identity-0078D4?style=for-the-badge&logo=microsoftentraid&logoColor=white)
![RBAC](https://img.shields.io/badge/RBAC-Role%20Engineering%20%26%20Access%20Governance-2E86C1?style=for-the-badge)
![Offboarding](https://img.shields.io/badge/Offboarding-Access%20Revocation-6C3483?style=for-the-badge)
![Conditional Access](https://img.shields.io/badge/Conditional%20Access-Identity%20Security-00A86B?style=for-the-badge)
![PowerShell](https://img.shields.io/badge/PowerShell-Automation%20%26%20Investigation-5391FE?style=for-the-badge&logo=powershell&logoColor=white)

**Author:** Sanjeev Shrestha | Dallas–Fort Worth Area, TX (Shrestha Technologies)  
**Target Roles:** IAM Engineer / Identity Engineer / Entra ID Engineer / Okta Administrator (2027)  
**Certification Focus:** Preparing for Microsoft SC-300: Identity and Access Administrator Associate <br>
**Environment:** Microsoft Active Directory/Entra ID

## 6 Labs Architecture Overview
 ![Architecture_Overview](Overall_Architecture.png)

## Quick Navigation and Lab Execution Status
<a id="lab-execution-status"></a>
| Lab       | Focus Area                                | Status    | Jump to Lab                                                                  |
|-----------|-------------------------------------------|-----------|------------------------------------------------------------------------------|
| **Lab 1** | AD Foundation | Completed | [Go to Lab 1](#lab-1-okta-tenant-setup--configuration)                       |
| **Lab 2** | RBAC | Completed | [Go to Lab 2](#lab-2-active-directory-integration--delegated-authentication) |
| **Lab 3** | Offboarding | Completed | [Go to Lab 3](#lab-3-rbac-design--access-sprawl-investigation)               |
| **Lab 4** | Lockout  | Completed | [Go to Lab 4](#lab-4-saml-20-application-integration)                        |
| **Lab 5** | Entra Connect  | Completed | [Go to Lab 5](#lab-5-adaptive-mfa-step-up-authentication--zero-trust)        |
| **Lab 6** | Conditional Access  | Completed | [Go to Lab 6](#lab-6-okta-jml-lifecycle-automation)                          |

---
## Executive Summary and Business Impact

Implemented an end-to-end IAM environment using Okta, Active Directory, SAML federation, Adaptive MFA, RBAC, and automated Joiner-Mover-Leaver lifecycle processes across six hands-on labs.

* **Tenant Foundation (Lab 1):** Core Okta tenant `*.okta.com - redacted`, custom profile attributes, Groups RBAC, Global Session + Auth + Enrollment Policies.
* **Access Governance (Lab 3+4):** Group-based provisioning, access-sprawl audit via System Log `application.user_membership.add`, least-privilege via group app assignments for Slack, Workspace, SharePoint.
* **Lifecycle Automation (Lab 2+6):** AD `allenlab.local` as source with DelAuth + Full/Incremental Import via AD Agent, JML automation with PowerShell + CSV batch `jml-batch.csv`, Unassigned → Confirm → Active/Deactivated.
* **Centralized Auth (Lab 4):** SAML 2.0 federation with attribute mapping across Slack, Google Workspace, SharePoint.
* **Zero Trust (Lab 5):** Network Zones Trusted/Untrusted IP, Adaptive Sign-On + Step-Up High Assurance for SharePoint, Passwordless Okta Verify Push with Number Matching.
* **Users Managed:** 20+ AD users, 6 Groups, 3 SAML Apps, 22 MFA policies

---

## Key Skills Demonstrated

* Okta Tenant Administration & Profile Editor
* Active Directory (AD) Integration & Delegated Authentication
* Role-Based Access Control (RBAC) Design
* SAML 2.0 SSO Federation & Attribute Mapping
* Adaptive Risk-Based Authentication & Network Zones
* Step-Up Authentication & Passwordless Policies
* User Lifecycle Management (JML) & Access Sprawl Audit


## Detailed Lab Implementations and Visual Proof

<a id="lab-1-okta-tenant-setup--configuration"></a>
### Lab 1: AD Foundation

**Key Learnings and Takeaways:**
* Plan for leavers, not just joiners Disabled accounts need their own OU. It keeps terminated users isolated, easy to audit, and out of sync.
* Give access to groups, not people When access lives in groups, moves and exits become a membership change, not a cleanup project.
* Don't sync everything to the cloud If an account shouldn't have access, it shouldn't sync to Entra ID either.

<details>
<summary>📸 <b>click here to view Lab 1 Diagrams and Screenshots</b></summary>

<br>

#### Architecture Diagram
![Architecture Diagram](Lab01-AD-Foundation/Screenshots/Architecture_Diagram.png)

#### AD Foundation
* **Environment:**  
  ![Environment](Lab01-AD-Foundation/Screenshots/Static-IP-192.168.10.10.png)
* **ADUC OU Tree:**  
  ![People](Lab01-AD-Foundation/Screenshots/ADUC-OU-Tree.png)
* **Groups-Finance-SoD:**  
  ![Groups-Finance-SoD](Lab01-AD-Foundation/Screenshots/Groups-Finance-SoD.png)
* **Marcus-Williams:**  
  ![Marcus-Williams](Lab01-AD-Foundation/Screenshots/Marcus-Williams.png)
* **Jessica-Park:**  
  ![Jessica-Park](Lab01-AD-Foundation/Screenshots/Jessica-Park.png)
* **Jessica-Park-Expiry:**  
  ![Jessica-Park-Expiry](Lab01-AD-Foundation/Screenshots/Jessica-Park-Expiry.png)

</details>

---
**[↑ Back to Lab Execution Status](#lab-execution-status)**

<a id="lab-2-active-directory-integration--delegated-authentication"></a>
### Lab 2: RBAC

**Key Learnings and Takeaways:**
* Design before you build - If it's not in the matrix, don't build it. If you built it but it's not in the matrix, that's an audit finding.
* The matrix is the contract - Map each role to systems like — AD, Email, FinanceCore, PeopleHub, SalesPro, — plus Invoice Create / Approve / Admin. Every cell gets Allow, Deny, or Read-Only.
* SoD stops fraud, not just paperwork - Define the conflicts before we build.

<details>
<summary>📸 <b>click here to view Lab 2 Diagrams and Screenshots</b></summary>

<br>

#### Architecture Diagram
![Architecture Diagram](Lab02-RBAC/Screenshots/Architecture_diagram.png)

#### RBAC Proof
* **SoD Policy:**  
  ![SoD Policy](Lab02-RBAC/Screenshots/SoD-Policy.png)
* **Privilege Creep - Before remediation :**  
  ![Privilege Creep ](Lab02-RBAC/Screenshots/Before-remediation.png)
* **After remediation:**  
  ![After-remediation](Lab02-RBAC/Screenshots/After-remediation.png)
* **Role Change:**  
  ![Role-Change](Lab02-RBAC/Screenshots/Role-Change.png)
* **Access Certification:**  
  ![Access-Certification](Lab02-RBAC/Screenshots/Access-Certification.png)
* **Threat Connections:**  
  ![Threat-Connections](Lab02-RBAC/Screenshots/Threat-Connections.png)
  
</details>

---
**[↑ Back to Lab Execution Status](#lab-execution-status)**

<a id="lab-3-rbac-design--access-sprawl-investigation"></a>
### Lab 3: RBAC Design and Access Sprawl Investigation

**Key Learnings and Takeaways:**
* Enforced end-to-end lifecycle flow: `HR → AD → Okta`. Tested AD account disablement and verified the resulting Okta lifecycle state. Session revocation and downstream application access require separate validation.
* Audited governance bypasses in Okta System Logs (`application.user_membership.add`) to detect direct user-to-app assignments and restore strict RBAC posture.

<details>
<summary>📸 <b>click here to view Lab 3 Diagrams and Screenshots</b></summary>

<br>

#### Architecture Diagram
![Architecture Diagram](Lab%203%20-%20Okta%20RBAC%20Design%20and%20Implementation/Screenshots/Architecture_Diagram.png)

#### RBAC & Deprovisioning Proof
* **Group Sync Verification:**  
  ![Okta Group Sync](Lab%203%20-%20Okta%20RBAC%20Design%20and%20Implementation/Screenshots/Okta_Group_Sync.png)
* **Full Reconciliation Import:**  
  ![Full Import](Lab%203%20-%20Okta%20RBAC%20Design%20and%20Implementation/Screenshots/Full_Import.png)
* **Lifecycle Deactivation (AD side):**  
  ![Sarah Disabled AD](Lab%203%20-%20Okta%20RBAC%20Design%20and%20Implementation/Screenshots/Sarah_Disabled_AD.png)
* **Lifecycle Deactivation (Okta side):**  
  ![Sarah Deactivated Okta](Lab%203%20-%20Okta%20RBAC%20Design%20and%20Implementation/Screenshots/Sarah_Deactivated_Okta.png)
* **Access Sprawl Investigation and Audit Report:**  
  ![Access Sprawl Investigation](Lab%203%20-%20Okta%20RBAC%20Design%20and%20Implementation/Screenshots/Access_Sprawl_Investigation.png)  
  ![Access Sprawl Report](Lab%203%20-%20Okta%20RBAC%20Design%20and%20Implementation/Screenshots/Access_Sprawl_Report.png)

</details>

---
**[↑ Back to Lab Execution Status](#lab-execution-status)**

<a id="lab-4-saml-20-application-integration"></a>
### Lab 4: SAML 2.0 Application Integration

**Key Learnings and Takeaways:**
* Configured custom SAML 2.0 integrations with multiple Service Providers (Slack, Google Workspace, SharePoint).
* Applied least-privilege entitlements through group memberships rather than direct assignments. Verified user access segregation across distinct user profiles in Incognito test sessions.

<details>
<summary>📸 <b>click here to view Lab 4 Diagrams and Screenshots</b></summary>

<br>

#### Architecture Diagram
![Architecture Diagram](Lab%204%20-%20Okta%20SAML%202.0%20Application%20Integration/Screenshots/Architecture_Diagram.png)

#### SAML Integration Proof
* **Custom SAML Configuration:**  
  ![SAML Creation 1](Lab%204%20-%20Okta%20SAML%202.0%20Application%20Integration/Screenshots/Slack_SP_Shrestha_Technology_1.png)  
  ![SAML Creation 2](Lab%204%20-%20Okta%20SAML%202.0%20Application%20Integration/Screenshots/Slack_SP_Shrestha_Technology_2.png)  
  ![SAML Creation 3](Lab%204%20-%20Okta%20SAML%202.0%20Application%20Integration/Screenshots/Slack_SP_Shrestha_Technology_3.png)
* **Group-Based Application Assignments:**  
  ![Group Based App Access 1](Lab%204%20-%20Okta%20SAML%202.0%20Application%20Integration/Screenshots/Group_Based_App_Access_1.png)  
  ![Group Based App Access 2](Lab%204%20-%20Okta%20SAML%202.0%20Application%20Integration/Screenshots/Group_Based_App_Access_2.png)
* **Least Privilege Access Verification:**  
  * *Finance User (John Smith - Limited Access):*  
    ![John App Access](Lab%204%20-%20Okta%20SAML%202.0%20Application%20Integration/Screenshots/John_App_Access.png)
  * *HR & Cloud Admin (Lisa Park - Extended Access):*  
    ![Lisa App Access](Lab%204%20-%20Okta%20SAML%202.0%20Application%20Integration/Screenshots/Lisa_App_Access.png)

</details>

---
**[↑ Back to Lab Execution Status](#lab-execution-status)**

<a id="lab-5-adaptive-mfa-step-up-authentication--zero-trust"></a>
### Lab 5: Adaptive MFA, Step-Up Authentication and Zero Trust

**Key Learnings and Takeaways:**
* Defined trusted vs. untrusted IP Network Zones. Constructed Adaptive Sign-On Rules to require MFA or block access based on client network context.
* Enforced step-up authentication policies on high-value resources (e.g., SharePoint) to challenge active sessions upon accessing sensitive data.

<details>
<summary>📸 <b>click here to view Lab 5 Diagrams and Screenshots</b></summary>

<br>

#### Architecture Diagram
![Architecture Diagram](Lab%205%20-%20Okta%20MFA%20and%20Adaptive%20Authentication/Screenshots/Architecture_Diagram.png)

#### MFA & Policy Verification
* **Authenticator Enrollment & Passwordless Setup:**  
  ![Enroll Okta Verify](Lab%205%20-%20Okta%20MFA%20and%20Adaptive%20Authentication/Screenshots/Enroll_Okta_Verify.png)  
  ![Passwordless Enrollment](Lab%205%20-%20Okta%20MFA%20and%20Adaptive%20Authentication/Screenshots/Passwordless_Enrollment_1.png)
* **Network Zones & Adaptive Rules:**  
  ![Network Zones](Lab%205%20-%20Okta%20MFA%20and%20Adaptive%20Authentication/Screenshots/Network_Zones.png)  
  ![Adaptive Policy Outside Network](Lab%205%20-%20Okta%20MFA%20and%20Adaptive%20Authentication/Screenshots/Risk_Based_Adaptive_MFA_Policy_OutsideNetwork.png)  
  ![Adaptive Policy Corporate Network](Lab%205%20-%20Okta%20MFA%20and%20Adaptive%20Authentication/Screenshots/Risk_Based_Adaptive_MFA_Policy_Corporate.png)
* **Step-Up Authentication Engine:**  
  ![Step Up Policy](Lab%205%20-%20Okta%20MFA%20and%20Adaptive%20Authentication/Screenshots/Step_Up_Authentication_Policy.png)  
  ![Step Up Rule](Lab%205%20-%20Okta%20MFA%20and%20Adaptive%20Authentication/Screenshots/Step_Up_Authentication_Rule.png)  
  ![Step Up High Assurance](Lab%205%20-%20Okta%20MFA%20and%20Adaptive%20Authentication/Screenshots/Step_Up_Authentication_High_Assurance_Policy.png)
* **User Authentication Testing:**  
  ![John Smith Access](Lab%205%20-%20Okta%20MFA%20and%20Adaptive%20Authentication/Screenshots/John_Smith_Access.png)  
  ![Jane Doe Access](Lab%205%20-%20Okta%20MFA%20and%20Adaptive%20Authentication/Screenshots/Jane_Doe_Access.png)

</details>

---
**[↑ Back to Lab Execution Status](#lab-execution-status)**

<a id="lab-6-okta-jml-lifecycle-automation"></a>
### Lab 6: Okta JML Lifecycle Automation

**Key Learnings & Takeaways:**
- Automates Joiner, Mover, and Leaver lifecycle actions using PowerShell and Active Directory.
- Uses the Okta AD Agent and Full/Incremental Import to validate applicable AD changes in Okta.
- Supports CSV-driven batch processing for multiple lifecycle actions.

<details>
<summary>📸 <b>click here to view Lab 6 Diagrams and Screenshots</b></summary>

<br>

#### Architecture Diagram
![Architecture Diagram](Lab%206%20-%20Okta%20JML%20Lifecycle%20Automation/Screenshots/Architecture_Diagram.png)

#### Joiner Mover Leaver Lifecycle
* **Joiner Onboarding Script:**  
  ![Joiner_Onboarding_Script](Lab%206%20-%20Okta%20JML%20Lifecycle%20Automation/Screenshots/Joiner_Onboarding_Script.png)    
* **IT Confirmation:**  
  ![IT_Confirmation](Lab%206%20-%20Okta%20JML%20Lifecycle%20Automation/Screenshots/IT_Confirmation.png)  
* **Mover Script:**  
  ![Mover_Script](Lab%206%20-%20Okta%20JML%20Lifecycle%20Automation/Screenshots/Mover_Script.png)    
* **Finance Confirmation:**  
  ![Finance_Confirmation](Lab%206%20-%20Okta%20JML%20Lifecycle%20Automation/Screenshots/Finance_Confirmation.png)
* **Leaver Script:**  
  ![Leaver_Script](Lab%206%20-%20Okta%20JML%20Lifecycle%20Automation/Screenshots/Leaver_Script.png)
* **Deactivation Confirmation:**  
  ![Deactivation_Confirmation](Lab%206%20-%20Okta%20JML%20Lifecycle%20Automation/Screenshots/Deactivation_Confirmation.png)
* **CSV Driven Batch Processing:**  
  ![CSV_Driven_Batch_Processing](Lab%206%20-%20Okta%20JML%20Lifecycle%20Automation/Screenshots/CSV_Driven_Batch_Processing.png)
* **Activated Deactivated Confirmation:** 
  ![Activated_Deactivated_Confirmation](Lab%206%20-%20Okta%20JML%20Lifecycle%20Automation/Screenshots/Activated_Deactivated_Confirmation.png)
* **HR Group Confirmation:** 
  ![HR_Group_Confirmation](Lab%206%20-%20Okta%20JML%20Lifecycle%20Automation/Screenshots/HR_Group_Confirmation.png)
* **Finance Group Confirmation:** 
  ![Finance_Group_Confirmation](Lab%206%20-%20Okta%20JML%20Lifecycle%20Automation/Screenshots/Finance_Group_Confirmation.png)
</details>

---
#### 📂 Code & Automation Artifacts (Direct Inspect)
- **Joiner:** [`scripts/Joiner.ps1`](scripts/Joiner.ps1) — Create a user and assign configured groups.
- **Mover:** [`scripts/Mover.ps1`](scripts/Mover.ps1) — Update user attributes and change group memberships.
- **Leaver:** [`scripts/Leaver.ps1`](scripts/Leaver.ps1) — Disable the account and remove configured group memberships.
- **CSV input:** [`csv-file/Jml-Batch.csv`](csv-file/Jml-Batch.csv) — Batch lifecycle records.
- **Batch runner:** [`scripts/JML-Batch.ps1`](scripts/JML-Batch.ps1) — Process CSV records with validation.

#### Validation and Limitations

AD and Okta state changes do not, by themselves, prove downstream application deprovisioning or active-session revocation. Those outcomes require separate validation.

**[↑ Back to Lab Execution Status](#lab-execution-status)**

---

## Original Repository and Credits
This hands-on lab series was adapted and built upon the foundational framework provided by [RyanKennon/Okta-Lab-Series](https://github.com/RyanKennon/Okta-Lab-Series). Full original references can be found in [REFERENCE.md](REFERENCE.md).







