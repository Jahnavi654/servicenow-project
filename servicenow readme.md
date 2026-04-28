# 🚀 Automated Employee Onboarding & Offboarding System
### ServiceNow Global Certification Capstone Project

---

## 📋 Project Overview

The **Automated Employee Onboarding & Offboarding System** is a ServiceNow-based solution that automates the end-to-end lifecycle of employee onboarding and offboarding processes. The system eliminates manual work by automating approvals, task assignments, notifications, and record management through Flow Designer and Service Catalog.

---

## 🎯 Objectives

- Automate employee onboarding and offboarding workflows using **ServiceNow Flow Designer**
- Create a self-service **Service Catalog** item for HR to submit requests
- Implement **approval workflows** with manager-based approvals
- Auto-generate **department tasks** (IT, Facilities, Security) upon approval
- Send **automated email notifications** to HR and requesters
- Maintain employee lifecycle records in a **custom table**

---

## 🏗️ Project Architecture

```
Service Portal (Frontend)
        ↓
Service Catalog Item — "Onboard New Employee"
        ↓
Flow Designer — "onBoarding Flow"
        ↓
    ┌───────────────────────────────┐
    │  Get Catalog Variables        │
    │  Create Employee Lifecycle    │
    │  Ask for Approval (Manager)   │
    │         ↓                     │
    │   If Approved:                │
    │   → Send Email                │
    │   → Create IT Task            │
    │   → Create Facilities Task    │
    │   → Create Security Task      │
    │   → Wait for All Tasks Done   │
    │   → Update Lifecycle Record   │
    │   → Update RITM               │
    │   → Send Completion Email     │
    │         ↓                     │
    │   If Rejected:                │
    │   → Update Lifecycle Record   │
    │   → Update RITM               │
    │   → Send Rejection Email      │
    └───────────────────────────────┘
```

---

## 🛠️ Technologies Used

| Technology | Purpose |
|---|---|
| ServiceNow PDI (Personal Developer Instance) | Development environment |
| Flow Designer | Workflow automation |
| Service Catalog | User-facing request form |
| Custom Table (u_employee_lifecycle) | Data storage |
| Catalog UI Policies | Dynamic form behavior |
| sc_task | Department task management |
| Email Notifications | Automated alerts |

---

## 📁 Project Structure

```
Automated-Employee-Onboarding-Offboarding/
│
├── README.md
├── screenshots/
│   ├── 01_custom_table/
│   │   ├── table_creation.png
│   │   └── table_fields.png
│   ├── 02_service_catalog/
│   │   ├── catalog_item.png
│   │   └── catalog_variables.png
│   ├── 03_flow_designer/
│   │   ├── flow_overview.png
│   │   ├── trigger_config.png
│   │   ├── get_catalog_variables.png
│   │   ├── create_record.png
│   │   ├── ask_for_approval.png
│   │   ├── if_approved_branch.png
│   │   ├── catalog_tasks.png
│   │   ├── wait_for_condition.png
│   │   ├── update_records.png
│   │   └── if_rejected_branch.png
│   ├── 04_ui_policies/
│   │   ├── ui_policy_onboarding.png
│   │   └── ui_policy_offboarding.png
│   └── 05_testing/
│       ├── service_portal_form.png
│       ├── request_submitted.png
│       ├── approval_screen.png
│       └── request_completed.png
```

---

## ⚙️ Setup & Configuration

### Prerequisites
- ServiceNow Personal Developer Instance (PDI)
- Admin access to the instance

### Step 1: Create Custom Table
1. Navigate to **System Definition → Tables**
2. Create table: `u_employee_lifecycle`
3. Add fields: Requested for, Employee ID, Department, Joining Date, Manager, Request Type, RITM, Status

### Step 2: Create Service Catalog Item
1. Navigate to **Service Catalog → Catalog Definitions → Maintain Items**
2. Create item: `Onboard New Employee`
3. Add variables: requested_for, employee_id, department, joining_date, managers, request_type, assets_needed, special_access, exit_date, assets_to_collect, access_revocations

### Step 3: Configure Catalog UI Policies
1. Open catalog item → **Catalog UI Policies** tab
2. Create policy: Show Joining Date when Request Type = Onboarding
3. Create policy: Show Exit Date when Request Type = Offboarding

### Step 4: Build the Flow in Flow Designer
1. Navigate to **Flow Designer**
2. Create new flow: `onBoarding Flow`
3. Set trigger: Service Catalog
4. Add actions as per the flow architecture above

### Step 5: Activate the Flow
1. Click **Save** → Click **Activate**

---

## 🔄 Flow Details

### Trigger
- **Type:** Service Catalog
- **Item:** Onboard New Employee

### Actions

| Step | Action | Details |
|---|---|---|
| 1 | Get Catalog Variables | Reads form inputs from catalog item |
| 2 | Create Record | Creates record in u_employee_lifecycle |
| 3 | Ask for Approval | Sends approval request to Manager |
| 4 | If Approved | Branches to approval path |
| 5 | Send Email | Notifies manager of approval needed |
| 6 | Create IT Task | "Create accounts & assign hardware" |
| 7 | Create Facilities Task | "Prepare workspace" |
| 8 | Create Security Task | "Issue ID badge & building access" |
| 9 | Wait for Condition | Waits for all sc_tasks = Closed Complete |
| 10 | Update Lifecycle Record | Status = Completed |
| 11 | Update RITM | State = Closed Complete |
| 12 | Send Email | Notifies HR & Requester: Onboarding Complete |
| 13 | If Rejected | Branches to rejection path |
| 14 | Update Lifecycle Record | Status = Rejected |
| 15 | Update RITM | State = Closed Incomplete |
| 16 | Send Email | Notifies requester of rejection |

---

## 📋 Department Tasks Created on Approval

| Task | Assignment Group | Due Date |
|---|---|---|
| Create accounts & assign hardware | IT – Account Provisioning | Joining date - 1 day |
| Prepare workspace | Facilities – Workspace | Joining date - 1 day |
| Issue ID badge & building access | Security – Access | Joining date |

---

## 🧪 Testing

### How to Test the Flow
1. Go to your ServiceNow instance URL + `/sp` (Service Portal)
2. Search for **"Onboard New Employee"**
3. Fill in the form:
   - Request Type: Onboarding
   - Employee ID: EMP001
   - Department: IT
   - Joining Date: (future date)
   - Manager: (select a user)
4. Click **Order Now**
5. Navigate to **My Approvals** and approve the request
6. Verify 3 sc_tasks are created
7. Close all tasks → verify lifecycle record updates to Completed

---

## 📸 Screenshots Guide

> See `screenshots/` folder for all implementation screenshots

| Screenshot | What it Shows |
|---|---|
| table_creation.png | Custom table u_employee_lifecycle |
| catalog_item.png | Service Catalog item configuration |
| flow_overview.png | Complete flow in Flow Designer |
| if_approved_branch.png | Approved path with all actions |
| if_rejected_branch.png | Rejected path with all actions |
| service_portal_form.png | End user form in Service Portal |
| request_completed.png | Final completed request |

---

## 👤 Author

- **Project:** ServiceNow Global Certification Capstone
- **Platform:** Skill Wallet – SmartBridge
- **Module:** ServiceNow Global Certification (BITAM)

---

## 📝 License

This project is developed as part of an academic/certification capstone. All ServiceNow configurations are built on a Personal Developer Instance (PDI).
