# Part 5 - Patch Management, Reports, and Audit Documentation

## Overview
In this section, we cover patch management using **Action1**, a cloud-based remote monitoring and management (RMM) tool. We walk through deploying missing updates to endpoints, monitoring update status, and generating reports for audit and documentation purposes.

---

## Environment
| Machine | Role | IP Address |
|---|---|---|
| Windows Server 2022 VM | Domain Controller / Managed Endpoint | 10.0.0.10 |
| Windows 11 VM | Domain-joined Workstation / Managed Endpoint | 10.0.0.x |
| Domain | example.com | — |
| Patch Management Tool | Action1 (cloud-based RMM) | — |

---

## Part 1 — Setting Up Action1

### What is Action1?
Action1 is a cloud-based patch management and remote monitoring tool used by IT teams to manage updates, monitor endpoints, and generate reports across an entire environment — all from a single dashboard.

### Step 1 — Create an Action1 Account

1. Go to **[www.action1.com](https://www.action1.com)** and sign up for a free account
2. Verify your email and log into the **Action1 Dashboard**

### Step 2 — Install the Action1 Agent on Windows Server 2022

1. In the Action1 dashboard, navigate to **Add Endpoint**
2. Download the **Action1 Agent installer**
3. Run the installer on **Windows Server 2022**
4. Once installed, go back to the **Action1 Dashboard**
5. Confirm the endpoint appears as **Connected** in the dashboard

> 💡 **Note:** The Action1 agent can be deployed to multiple endpoints across your environment. In a real enterprise, this would be pushed out to all machines automatically using a deployment tool or Group Policy.

---

## Part 2 — Deploying Missing Updates

### Step 1 — Review Available Actions

1. In the **Action1 Dashboard**, click on **Actions** in the left panel
2. Browse through the available options to get familiar with what Action1 can do:
   - Software deployment
   - Script execution
   - Patch management
   - System configuration

### Step 2 — Deploy Missing Updates

1. Click on **Missing Updates** in the Action1 dashboard
2. Review the list of updates that are missing from your endpoints
3. Click **Select All** to select all missing updates
4. Click **Deploy Updates**
5. Click **Next Step**
6. Click **Next Step** again to confirm the deployment settings
7. Select **Run Now** to immediately begin deploying updates

### Step 3 — Monitor Update Status

After deploying, Action1 provides real-time feedback on the update process:

- **Status** — Shows whether the update is pending, in progress, or completed for each endpoint
- **Details** — Shows exactly what each update is doing, including which patches are being installed

> 💡 **Help Desk Tip:** In a real enterprise environment, patch management is critical for maintaining security. Unpatched systems are one of the most common attack vectors. IT teams typically schedule patch deployments during off-hours (e.g. nights or weekends) to minimize disruption to users.

---

## Part 3 — Reports and Audit Documentation

### Why Documentation Matters
In a professional IT environment, you are often required to document changes, updates, and the state of your environment. This documentation is used for:

- **Audits** — Proving to auditors that systems are up to date and compliant
- **Management reports** — Giving leadership visibility into the health of IT assets
- **Troubleshooting** — Having a record of what changed and when

### Generating an Endpoint Report

An example scenario: *An auditor asks how many assets are in the environment and what their current status is.*

1. In the **Action1 Dashboard**, go to **Managed Endpoints**
2. Review the list of all connected endpoints and their details
3. Use the built-in **report templates** to generate a printout of your machines

Action1 includes a variety of built-in report templates covering:

| Report Type | Use Case |
|---|---|
| Managed Endpoints | Full list of all devices in the environment |
| Missing Updates | List of patches not yet applied per endpoint |
| Installed Software | Inventory of software installed across endpoints |
| Update Deployment History | Record of all patches deployed and their status |
| System Information | Hardware and OS details per machine |

4. Select the relevant template → generate and export the report
5. Save the report as a **PDF or CSV** to share with an auditor or manager

> 💡 **Help Desk Tip:** Getting comfortable with generating reports is a valuable IT skill. Whether it's for a security audit, a compliance check, or a manager asking about asset inventory — knowing how to quickly pull accurate reports from your tools makes you a much more effective IT professional.

---

## Summary

By the end of this section you should have:

- ✅ Created an Action1 account and installed the agent on Windows Server 2022
- ✅ Confirmed the endpoint appears as connected in the Action1 dashboard
- ✅ Reviewed available actions in the Action1 console
- ✅ Identified and deployed all missing updates to endpoints
- ✅ Monitored the status and details of the update deployment in real time
- ✅ Navigated to Managed Endpoints and generated an asset report
- ✅ Explored built-in report templates for audits and documentation

---
