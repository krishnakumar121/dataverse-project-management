# Project Management App - Setup Guide

Complete step-by-step guide to set up the Dataverse Project Management App.

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Environment Setup](#environment-setup)
3. [Create Dataverse Tables](#create-dataverse-tables)
4. [Import Sample Data](#import-sample-data)
5. [Create Model-Driven App](#create-model-driven-app)
6. [Configure Views & Forms](#configure-views--forms)
7. [Deploy Power Automate Workflows](#deploy-power-automate-workflows)
8. [Set Up Security](#set-up-security)
9. [Testing](#testing)
10. [Go Live](#go-live)

## Prerequisites

### Required
- ✅ Microsoft 365 subscription
- ✅ Power Apps license (per user or per app)
- ✅ Dataverse environment with sufficient storage
- ✅ Power Automate license (for workflows)
- ✅ Admin access to environment

### Recommended
- Power BI license (for analytics)
- SharePoint Online (for document integration)
- Microsoft Teams integration

## Environment Setup

### Step 1: Access Your Dataverse Environment

1. Navigate to [Power Apps Portal](https://make.powerapps.com)
2. Select your environment from the dropdown (top-right)
3. If you don't have an environment:
   - Click **New environment**
   - Enter name: `Project Management`
   - Select region
   - Click **Create**

### Step 2: Create Dataverse Database

1. In Power Apps, go to **Dataverse** → **Databases**
2. Click **Create database**
3. Enter database details:
   - **Name:** Project Management DB
   - **Currency:** USD
   - **Language:** English
4. Click **Create**
5. Wait for database creation (2-5 minutes)

### Step 3: Verify Environment

- Navigate to **Dataverse** → **Tables**
- You should see standard tables (Account, Contact, etc.)

## Create Dataverse Tables

### Overview of Tables to Create

```
1. Project (Main table)
2. Task
3. TeamMember
4. Milestone
5. Resource
6. RiskRegister
7. ProjectUpdate
```

### Step 1: Create Project Table

1. Go to **Dataverse** → **Tables**
2. Click **New table**
3. Enter details:
   - **Name:** Project
   - **Display name (Plural):** Projects
   - **Primary name column:** Project Name
4. Click **Create**
5. Add columns:

| Column Name | Display Name | Data Type | Required | Notes |
|---|---|---|---|---|
| Description | Description | Multi-line text | No | |
| Start Date | Start Date | Date and Time | Yes | |
| End Date | End Date | Date and Time | Yes | |
| Status | Status | Choice | Yes | New, Active, On Hold, Closed |
| Priority | Priority | Choice | Yes | Low, Medium, High, Critical |
| Budget | Budget | Currency | No | |
| Owner | Owner (Project Manager) | Lookup (User) | Yes | |
| Client/Department | Client/Department | Text | No | |
| Health Status | Health Status | Choice | No | Green, Yellow, Red |

### Step 2: Create Task Table

1. Click **New table**
2. Name: **Task**
3. Add columns:

| Column Name | Display Name | Data Type | Required | Notes |
|---|---|---|---|---|
| Title | Title | Text | Yes | |
| Description | Description | Multi-line text | No | |
| Status | Status | Choice | Yes | New, In Progress, Blocked, Completed |
| Priority | Priority | Choice | Yes | Low, Medium, High, Critical |
| Due Date | Due Date | Date and Time | Yes | |
| Assigned To | Assigned To | Lookup (User) | Yes | |
| Project | Project | Lookup (Project) | Yes | |
| Estimated Hours | Estimated Hours | Decimal | No | |
| Actual Hours | Actual Hours | Decimal | No | |
| Dependencies | Dependencies | Text | No | |

### Step 3: Create TeamMember Table

1. Click **New table**
2. Name: **TeamMember**
3. Add columns:

| Column Name | Display Name | Data Type | Required | Notes |
|---|---|---|---|---|
| Name | Name | Text | Yes | |
| Email | Email | Email | Yes | |
| Role | Role | Choice | Yes | PM, Developer, Analyst, Designer, QA |
| Skills | Skills | Text | No | |
| Availability | Availability | Choice | Yes | Available, Busy, On Leave |
| Department | Department | Text | No | |

### Step 4: Create Milestone Table

1. Click **New table**
2. Name: **Milestone**
3. Add columns:

| Column Name | Display Name | Data Type | Required | Notes |
|---|---|---|---|---|
| Milestone Name | Milestone Name | Text | Yes | |
| Target Date | Target Date | Date and Time | Yes | |
| Status | Status | Choice | Yes | Not Started, In Progress, Completed |
| Description | Description | Multi-line text | No | |
| Project | Project | Lookup (Project) | Yes | |
| Key Deliverables | Key Deliverables | Text | No | |

### Step 5: Create Resource Table

1. Click **New table**
2. Name: **Resource**
3. Add columns:

| Column Name | Display Name | Data Type | Required | Notes |
|---|---|---|---|---|
| Team Member | Team Member | Lookup (TeamMember) | Yes | |
| Project | Project | Lookup (Project) | Yes | |
| Role on Project | Role on Project | Choice | Yes | Lead, Senior, Junior, Support |
| Hours Allocated | Hours Allocated | Decimal | Yes | |
| Start Date | Start Date | Date and Time | Yes | |
| End Date | End Date | Date and Time | No | |
| Utilization | Utilization | Choice | No | Underutilized, Optimal, Overutilized |

### Step 6: Create RiskRegister Table

1. Click **New table**
2. Name: **RiskRegister**
3. Add columns:

| Column Name | Display Name | Data Type | Required | Notes |
|---|---|---|---|---|
| Risk Description | Risk Description | Multi-line text | Yes | |
| Impact | Impact | Choice | Yes | Low, Medium, High, Critical |
| Probability | Probability | Choice | Yes | Low, Medium, High |
| Mitigation Plan | Mitigation Plan | Multi-line text | No | |
| Owner | Owner | Lookup (User) | Yes | |
| Status | Status | Choice | Yes | Identified, Monitoring, Mitigated, Closed |
| Project | Project | Lookup (Project) | Yes | |

### Step 7: Create ProjectUpdate Table

1. Click **New table**
2. Name: **ProjectUpdate**
3. Add columns:

| Column Name | Display Name | Data Type | Required | Notes |
|---|---|---|---|---|
| Title | Title | Text | Yes | |
| Content | Content | Multi-line text | Yes | |
| Posted By | Posted By | Lookup (User) | Yes | |
| Posted Date | Posted Date | Date and Time | Yes | |
| Project | Project | Lookup (Project) | Yes | |
| Update Type | Update Type | Choice | No | Status, Issue, Achievement |

## Import Sample Data

### Option 1: Using CSV Files

1. Navigate to each table
2. Click **Get data** → **Upload data**
3. Select CSV file from `data/` folder
4. Map columns
5. Click **Upload**

### Option 2: Manual Entry

1. For testing, create a few sample records manually
2. Go to each table
3. Click **New**
4. Fill in sample data
5. Click **Save**

## Create Model-Driven App

### Step 1: Create New App

1. Go to **Power Apps** → **Apps**
2. Click **New app** → **Model-driven app**
3. Enter app name: **Project Management App**
4. Click **Create**
5. Power Apps Studio opens

### Step 2: Add Tables to App

1. In App Designer, click **Add tables**
2. Select tables:
   - ✅ Project
   - ✅ Task
   - ✅ TeamMember
   - ✅ Milestone
   - ✅ Resource
   - ✅ RiskRegister
   - ✅ ProjectUpdate
3. Click **Add**

### Step 3: Configure Navigation

1. Go to **Navigation** section
2. Arrange tables logically
3. Click **Save**

### Step 4: Publish App

1. Click **Publish**
2. Wait for publishing to complete
3. Click **Play** to test

## Configure Views & Forms

### Create Custom Views

1. Go to **Tables** → **Project**
2. Click **Views** → **New**
3. Create views:
   - **Active Projects** - Filter: Status = Active
   - **My Projects** - Filter: Owner = Current User
   - **Overdue Projects** - Filter: End Date < Today

### Create Task Kanban View

1. Go to **Tables** → **Task**
2. Click **Views** → **New** → **Kanban**
3. Configure:
   - **Group By:** Status
   - **Stack By:** Priority

## Deploy Power Automate Workflows

### Task Reminder Flow

1. Go to **Power Automate**
2. Click **New flow** → **Cloud flow** → **Automated**
3. Trigger: When a row is created or modified (Task table)
4. Add condition: Due Date within 2 days
5. Action: Send email notification
6. Save and activate

### Milestone Alert Flow

1. Click **New flow** → **Automated**
2. Trigger: When a row is created or modified (Milestone table)
3. Condition: Target Date within 3 days
4. Action: Send email to project owner
5. Save and activate

## Set Up Security

### Create Security Roles

1. Go to **Settings** → **Security Roles**
2. Create roles:
   - **Project Manager:** Create/Read/Write/Delete all
   - **Team Member:** Create/Read/Write Tasks and own assignments
   - **Stakeholder:** Read-only access
   - **Admin:** Full access

### Assign Roles to Users

1. Go to **Settings** → **Users**
2. Select user
3. Click **Edit**
4. Assign appropriate security role
5. Click **Save**

## Testing

### Functional Tests

- ✅ Create new project
- ✅ Add tasks to project
- ✅ Assign team members
- ✅ Create milestones
- ✅ Test views and filters
- ✅ Verify workflows trigger
- ✅ Test mobile experience

### User Acceptance Testing

1. Create test users
2. Assign security roles
3. Have users test key workflows
4. Gather feedback
5. Make adjustments

## Go Live

### Pre-Launch Checklist

- ✅ All tables created and verified
- ✅ Sample data loaded
- ✅ Model app published
- ✅ Power Automate workflows active
- ✅ Security roles configured
- ✅ User training completed
- ✅ Documentation finalized

### Launch Steps

1. Notify users of launch date
2. Ensure training is complete
3. Activate all workflows
4. Monitor for issues
5. Provide user support

---

**For detailed information, see the other documentation files in the `/docs` folder.**
