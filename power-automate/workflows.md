# Power Automate Workflows

## Overview

This document describes the Power Automate cloud flows used in the Project Management App.

## Workflows Included

### 1. Task Reminder Notification

**Purpose:** Send reminders to task owners before due dates

**Trigger:** When a row is created or modified (Tasks table)

**Frequency:** Daily scheduled check (8 AM)

**Steps:**
```
1. Trigger: When a row is modified
   - Table: Task
   - Change type: Modified

2. Filter rows:
   - Status ≠ Completed
   - Due Date = Today or Tomorrow

3. For each task:
   - Get assigned user details
   - Get project name
   - Send email with task details
   - Log activity

4. Email content:
   - Task title
   - Due date
   - Project name
   - Direct link to task
   - Call to action
```

**Configuration:**
- Frequency: Daily at 8:00 AM
- Email template: Use adaptive card
- Retry policy: 3 retries on failure

---

### 2. Milestone Alert Notification

**Purpose:** Alert project managers when milestones are approaching

**Trigger:** When a row is created or modified (Milestones table)

**Frequency:** Real-time + Daily summary

**Steps:**
```
1. Trigger: When a row is created or modified
   - Table: Milestone
   - Change type: Created or Modified

2. Condition check:
   - IF Target Date - Today ≤ 3 days
   - AND Status ≠ Completed

3. Then:
   - Get project details
   - Get project owner
   - Send priority email
   - Create task: "Milestone alert - [name]"
   - Log in project update

4. Email content:
   - Milestone name
   - Days until due
   - Key deliverables
   - Project owner
```

**Configuration:**
- Priority: High
- Email recipients: Project Owner, Project Team Leads
- Retry: 3 attempts

---

### 3. Project Status Update

**Purpose:** Send weekly project status summaries

**Trigger:** Scheduled (Weekly, Monday 9 AM)

**Steps:**
```
1. Trigger: Scheduled cloud flow
   - Frequency: Weekly
   - Day: Monday
   - Time: 9:00 AM

2. For each active project:
   - Count: Total tasks, Completed tasks
   - Calculate: Completion percentage
   - Get: Active risks (High impact)
   - Get: Upcoming milestones (next 7 days)

3. Generate summary:
   - Project health status
   - Completion progress
   - Key alerts
   - Upcoming deadlines

4. Send email to:
   - Project owner
   - All project team members
   - Stakeholders (optional)

5. Post to Teams (optional):
   - Send adaptive card with summary
   - Include action buttons
```

**Configuration:**
- Schedule: Monday 9:00 AM (timezone-aware)
- Recipients: Dynamic based on project team
- Format: HTML email + Teams notification

---

### 4. Risk Escalation Workflow

**Purpose:** Escalate high-risk items to project managers

**Trigger:** When a row is created or modified (RiskRegister table)

**Steps:**
```
1. Trigger: When a row is created or modified
   - Table: RiskRegister
   - Change type: Created or Modified

2. Calculate risk score:
   - Risk Score = Impact × Probability
   - High Risk = Score ≥ 6

3. Condition check:
   - IF Risk Score ≥ 6 (High Risk)
   - AND Status IN ('Identified', 'Monitoring')

4. Then escalate:
   - Send URGENT email to project owner
   - Notify risk owner
   - Create escalation task
   - Alert management

5. If Risk Score = 9 (Critical):
   - Send Teams urgent notification
   - Escalate to department head
   - Create board item

6. Log escalation:
   - Record escalation time
   - Document who was notified
```

**Configuration:**
- Escalation threshold: Risk Score ≥ 6
- Critical threshold: Risk Score = 9
- Notification: Immediate
- Escalation chain: PM → Director → VP

---

### 5. Resource Utilization Check (Planned)

**Purpose:** Monitor team member workload and utilization

**Trigger:** Weekly scheduled check

**Steps:**
```
1. For each team member:
   - Sum: Hours allocated this week
   - Sum: Hours allocated next week
   - Calculate: Utilization percentage

2. Flag conditions:
   - Overutilized: > 100% capacity
   - Underutilized: < 50% capacity
   - Available: 50-100% capacity

3. Send alerts:
   - To Resource Manager: Overutilized resources
   - To Team Members: Availability suggestions

4. Generate report:
   - Team utilization summary
   - Availability forecast
   - Recommendations
```

---

## Creating a Workflow - Step by Step

### Template: Basic Task Workflow

1. **Go to Power Automate**
   - Navigate to make.powerautomate.com
   - Select your environment

2. **Create New Flow**
   - Click "New flow"
   - Select "Cloud flow"
   - Choose type: Automated, Instant, or Scheduled

3. **Set Trigger**
   ```
   Trigger type: When a row is created or modified
   Table: Tasks
   Change type: Created, Updated
   Scope: Organization
   ```

4. **Add Condition**
   ```
   Condition: If Status = "In Progress"
   True branch: [Add actions]
   False branch: [Skip]
   ```

5. **Add Actions**
   ```
   Example actions:
   - Get a row by ID
   - Update a row
   - Send an email
   - Post message to Teams
   - Log to Power BI
   ```

6. **Configure Email**
   ```
   To: [Assigned To User Email]
   Subject: Task Update: [Task Title]
   Body: 
   Task: @{body('Get_a_row')?['cr46f_title']}
   Status: @{body('Get_a_row')?['cr46f_status']}
   Due Date: @{body('Get_a_row')?['cr46f_duedate']}
   Link: [Direct link to task]
   ```

7. **Save and Test**
   - Click "Save"
   - Trigger the flow manually for testing
   - Verify actions execute correctly
   - Monitor run history

8. **Activate**
   - Once tested, turn flow "On"
   - Monitor for errors in first week
   - Make adjustments as needed

---

## Flow Variables & Expressions

### Common Expressions

```
# Date calculations
addDays(utcNow(), 2)                    # 2 days from now
addDays(triggerBody()?['due_date'], -1) # 1 day before due date

# String operations
concat('Hello ', triggerBody()?['name'])  # Concatenate strings
toUpper(triggerBody()?['title'])          # Convert to uppercase

# Conditionals
if(equals(triggerBody()?['status'], 'Completed'), 'Done', 'Pending')

# Lookups
first(body('Get_rows')?['value'])?['email']

# Array operations
length(body('Get_rows')?['value'])  # Count items
filter(body('Get_rows')?['value'], 
  item().status == 'Active')        # Filter items
```

---

## Error Handling

### Configure Error Handling

```
1. Add "Configure run after" on action
2. Select error conditions:
   - Has failed
   - Has timed out
   - Is skipped

3. In error branch:
   - Log error details
   - Send alert email
   - Retry action (if applicable)
   - Create incident ticket
```

### Retry Policy

```
Policy: Exponential backoff
Interval: 10 seconds
Max retries: 3
Retry on:
- HTTP 408 (Timeout)
- HTTP 429 (Throttled)
- HTTP 5xx (Server error)
```

---

## Monitoring & Troubleshooting

### Monitor Flow Performance

1. **Power Automate Analytics**
   - Go to flow details
   - Click "Analytics"
   - View:
     - Success rate
     - Run history
     - Performance metrics

2. **Check Run History**
   - Each flow shows last 28 days of runs
   - Click run to see details
   - Review inputs/outputs
   - Check step execution times

3. **Common Issues**

| Issue | Cause | Solution |
|-------|-------|----------|
| Flow not triggering | Trigger condition not met | Verify condition logic, test manually |
| Email not sending | Invalid email address | Add null check, validate format |
| Throttling | Too many API calls | Add delays, use batch operations |
| Permission error | User doesn't have access | Verify security roles |
| Timeout | Long-running operation | Break into smaller flows, use async |

---

## Best Practices

### Flow Design

- ✅ Start with simple flows, build complexity gradually
- ✅ Use descriptive names for flows and variables
- ✅ Add comments to document logic
- ✅ Test thoroughly before activation
- ✅ Use error handling on all critical steps
- ✅ Avoid nested conditions (use filters instead)
- ✅ Log important events for audit trail
- ✅ Consider performance impact of large "Apply to each"

### Performance Optimization

- ✅ Use Select action to reduce payload size
- ✅ Filter data at source (Dataverse query)
- ✅ Avoid retrieving all records
- ✅ Use parallel branches for independent operations
- ✅ Schedule heavy operations off-peak hours

### Maintenance

- ✅ Document flow purpose and dependencies
- ✅ Review flows monthly
- ✅ Archive old flows
- ✅ Keep flows updated with new requirements
- ✅ Monitor throttling and adjust as needed

---

## Integration with Other Systems

### Teams Integration

```
Action: Post message to Teams
Webhook: Adaptive Card format
Content:
- Task title
- Status
- Assigned to
- Action buttons (Open, Update, Complete)
```

### SharePoint Integration

```
Action: Create or update SharePoint item
Site: Project management site
List: Project Tasks list
Sync: Title, Status, Due Date
```

### Email Integration

```
Connector: Office 365 Outlook
Actions:
- Send email
- Send HTML formatted email
- Send with attachments
- Create draft (for review)
```

---

## Workflow Versioning

- **Version:** 1.0
- **Last Updated:** June 10, 2026
- **Compatible With:** Dataverse Project Management Schema v1.0

---

**Next Steps:** See [best-practices.md](../docs/best-practices.md) for implementation best practices.
