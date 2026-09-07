# Enterprise Offboarding Automation (ServiceNow Scoped App)

An automated IT & IAM Offboarding governance solution built on ServiceNow. This application orchestrates end-to-end access revocation, CMDB asset reclamation, and team management continuity.

---

## 📌 Problem & Business Impact
Manual offboarding processes expose enterprises to security risks, stranded hardware, and orphaned team approvals:
- **Logical Security Risks:** Delayed account deactivation leaves corporate systems vulnerable.
- **Asset Loss:** Lack of CMDB synchronization leads to unreturned hardware.
- **Orphaned Approvals:** Terminating a team manager without reassigning group ownership breaks internal approval chains.

---

## ⚙️ Architecture & Automation Logic (Flow Designer)

The core workflow triggers dynamically upon creating an offboarding request and executes three distinct streams:

```
[Trigger: Offboarding Request Created]
        │
        ├──► [IAM Deprovisioning & Security Check]
        │           ├── Target is Admin? ──► Skip Lockout (Safety Prevention)
        │           └── Target is Non-Admin ──► Update sys_user (Active: false, Locked out: true)
        │
        ├──► [CMDB Asset Recovery Check]
        │           ├── Asset Count == 0 ──► Automatically Complete Request
        │           └── Asset Count > 0 ──► Generate Sub-Task for Hardware Collection
        │
        └──► [Organizational Continuity Check]
                    └── Manages Groups in sys_user_group? ──► Generate Sub-Task for Manager Reassignment
```
---

## 🛠️ Key Technical Implementations
* **IAM Automated Deprovisioning:** Direct integration with `sys_user` to disable login capabilities (`locked_out=true`, `active=false`) while safeguarding administrator accounts.
* **Dynamic Task Generation:** Automatically spins off contextual fulfillment tasks assigned to proper resolver groups (Hardware Support / Service Desk) linked to the parent record.
* **CMDB Reconciliation:** Queries active Configuration Items / Hardware Assets assigned to the user to maintain single-source-of-truth accuracy.
* **Data Integrity Enforcement:** Implements UI Policies to lock system-calculated metrics (e.g., Asset Count) from manual tampering.

---

## 🚀 Installation & Deployment

1. In your ServiceNow Instance, open **Studio**.
2. Select **Import from Source Control**.
3. Provide the repository URL: https://github.com/FahadFZ1/enterprise-offboarding-automation.git
4. Authenticate using your GitHub Personal Access Token (PAT).
