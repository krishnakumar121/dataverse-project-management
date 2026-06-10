# Power Apps Navigation Map

## App Structure

### Site Map Organization

```
Project Management App
├── Projects (Group)
│   ├── Projects (Table View)
│   │   ├── Active Projects (View)
│   │   ├── My Projects (View)
│   │   ├── Overdue Projects (View)
│   │   └── All Projects (View)
│   ├── Tasks (Table View)
│   │   ├── My Tasks (View)
│   │   ├── Task Kanban (View - by Status)
│   │   ├── Overdue Tasks (View)
│   │   └── All Tasks (View)
│   └── Milestones (Table View)
│       ├── Upcoming Milestones (View)
│       ├── Active Milestones (View)
│       └── All Milestones (View)
│
├── Resources & Planning (Group)
│   ├── Team Members (Table View)
│   │   ├── All Team Members (View)
│   │   ├── Available Members (View)
│   │   └── Team Directory (View)
│   └── Resource Allocation (Table View)
│       ├── By Project (View)
│       ├── By Team Member (View)
│       ├── Utilization (View)
│       └── All Allocations (View)
│
└── Risk Management (Group)
    ├── Risk Register (Table View)
    │   ├── High Risk Items (View)
    │   ├── Active Risks (View)
    │   ├── Risk Dashboard (View)
    │   └── All Risks (View)
    └── Project Updates (Table View)
        ├── Recent Updates (View)
        ├── By Project (View)
        └── All Updates (View)
```

## View Specifications

### Project Views

#### Active Projects
- **Type:** List
- **Filter:** Status = 'Active' OR Status = 'New'
- **Columns:** Project Name, Status, Owner, Start Date, End Date, Priority, Health Status
- **Sort:** End Date (ascending)
- **Records:** Paginated (50 per page)

#### My Projects
- **Type:** List
- **Filter:** Owner = Current User
- **Columns:** Project Name, Status, Progress %, Tasks Completed, Next Milestone
- **Sort:** End Date (ascending)
- **Records:** All

#### Overdue Projects
- **Type:** List
- **Filter:** End Date < Today AND Status ≠ 'Completed'
- **Columns:** Project Name, End Date, Days Overdue, Status, Owner
- **Sort:** Days Overdue (descending)
- **Records:** All

#### Project Dashboard (Visual)
- **Type:** Card view
- **Shows:** Status, Progress, Budget, Team Size, Active Tasks, Risks
- **Quick Actions:** Edit, Close, Archive, Add Task

### Task Views

#### My Tasks
- **Type:** List
- **Filter:** Assigned To = Current User AND Status ≠ 'Completed'
- **Columns:** Title, Project, Due Date, Priority, Status, Progress %
- **Sort:** Due Date (ascending)
- **Records:** All

#### Task Kanban Board
- **Type:** Kanban
- **Group By:** Status (New → In Progress → Blocked → Completed)
- **Stack By:** Priority (Critical, High, Medium, Low)
- **Cards Show:** Title, Assigned To, Due Date, Progress
- **Drag & Drop:** Change status by dragging

#### Overdue Tasks
- **Type:** List
- **Filter:** Due Date < Today AND Status ≠ 'Completed'
- **Columns:** Title, Project, Assigned To, Due Date, Days Overdue
- **Sort:** Days Overdue (descending)
- **Alert:** Highlight in red

#### Task by Project
- **Type:** List
- **Group By:** Project
- **Filter:** Status ≠ 'Completed'
- **Columns:** Task, Status, Assigned To, Due Date, Priority
- **Sort:** Project Name, then Due Date

### Resource Views

#### Resource by Project
- **Type:** List
- **Group By:** Project
- **Columns:** Team Member, Role, Hours Allocated, Utilization %, Start Date, End Date
- **Sort:** Project Name
- **Highlighting:** Overutilized = Red, Optimal = Green, Underutilized = Yellow

#### Resource by Team Member
- **Type:** List
- **Group By:** Team Member
- **Columns:** Project, Role, Hours Allocated, Utilization %
- **Sort:** Team Member Name
- **Shows:** Total utilization across all projects

#### Utilization Dashboard
- **Type:** Visual
- **Shows:** 
  - Overutilized members (>100%)
  - Optimal utilization (50-100%)
  - Underutilized members (<50%)
  - Total team capacity
  - Forecast for next 4 weeks

### Risk Views

#### High Risk Items
- **Type:** List
- **Filter:** Risk Score ≥ 6 AND Status ≠ 'Closed'
- **Columns:** Risk Description, Impact, Probability, Risk Score, Owner, Status
- **Sort:** Risk Score (descending)
- **Highlighting:** Critical = Red

#### Active Risks
- **Type:** List
- **Filter:** Status IN ('Identified', 'Monitoring', 'Mitigating')
- **Columns:** Description, Project, Impact, Probability, Score, Owner, Mitigation Plan
- **Sort:** Risk Score (descending)

#### Risk Dashboard
- **Type:** Visual
- **Shows:**
  - Total risks by project
  - Risk distribution (High/Medium/Low)
  - Status breakdown
  - Owner assignments
  - Trend analysis

### Milestone Views

#### Upcoming Milestones
- **Type:** Timeline
- **Filter:** Target Date >= Today AND Status ≠ 'Completed'
- **Columns:** Name, Target Date, Days Until Due, Status, Project, Owner
- **Sort:** Target Date (ascending)
- **Highlighting:** Due within 3 days = Yellow

#### Milestone Progress
- **Type:** List with progress indicator
- **Filter:** Status = 'In Progress'
- **Columns:** Name, Project, Target Date, Status, Completion %, Key Deliverables
- **Sort:** Target Date (ascending)

## Form Specifications

### Project Main Form

**Tabs:**
1. **Overview**
   - Project Name (Text, Required)
   - Description (Multi-line Text)
   - Status (Choice Dropdown, Required)
   - Priority (Choice Dropdown, Required)
   - Owner (Lookup to User, Required)
   - Client/Department (Text)

2. **Timeline**
   - Start Date (Date and Time, Required)
   - End Date (Date and Time, Required)
   - Duration (Calculated field)
   - Days Remaining (Calculated field)

3. **Budget**
   - Budget (Currency)
   - Spent to Date (Calculated)
   - Remaining Budget (Calculated)
   - Budget Status (Green/Yellow/Red)

4. **Health & Status**
   - Health Status (Choice)
   - Progress % (Integer)
   - Last Update Date (Calculated)
   - Status Notes (Multi-line Text)

5. **Related Data** (Sub-grids)
   - Tasks (Quick view)
   - Milestones (Quick view)
   - Team Members (Quick view)
   - Risks (Quick view)

### Task Quick Form

**Tabs:**
1. **Details**
   - Title (Text, Required)
   - Description (Multi-line Text)
   - Project (Lookup, Required)
   - Status (Choice, Required)
   - Priority (Choice, Required)

2. **Assignment**
   - Assigned To (Lookup, Required)
   - Due Date (Date and Time, Required)
   - Estimated Hours (Decimal)
   - Actual Hours (Decimal)

3. **Related Items**
   - Dependencies (Multi-select Choice)
   - Blocked By (Lookup)
   - Related Milestone (Lookup)

### Resource Allocation Form

**Tabs:**
1. **Allocation Details**
   - Team Member (Lookup, Required)
   - Project (Lookup, Required)
   - Role on Project (Choice, Required)
   - Start Date (Date and Time, Required)
   - End Date (Date and Time)

2. **Hours & Utilization**
   - Hours Allocated (Decimal, Required)
   - Allocation % (Calculated)
   - Total Project Hours (Calculated)
   - Utilization Level (Calculated)
   - Status (Red/Yellow/Green)

3. **Notes**
   - Comments (Multi-line Text)
   - Special Skills (Text)
   - Constraints (Multi-line Text)

## Quick Actions

### Global Quick Actions
- **New Project** - Create new project
- **New Task** - Create new task
- **New Risk** - Create new risk
- **My Dashboard** - Go to personal dashboard

### Project Quick Actions
- **Add Task** - Add task to project
- **Add Team Member** - Add team member
- **Add Milestone** - Add milestone
- **View Tasks** - See all tasks
- **Close Project** - Change status to closed
- **Archive** - Archive project

### Task Quick Actions
- **Mark Complete** - Change status to completed
- **Mark Blocked** - Flag as blocked
- **Assign To** - Reassign task
- **View Project** - Go to parent project

## Search Configuration

### Searchable Fields
- Project Name
- Task Title
- Team Member Name
- Risk Description
- Milestone Name

### Search Scopes
- Global Search (all entities)
- Project-specific (all project data)
- Task Search
- Team Search
- Risk Search

## Mobile Navigation

### Mobile Menu Structure
```
Home
├── My Dashboard
├── My Tasks (Primary)
├── My Projects
├── Team
└── More
    ├── Milestones
    ├── Resources
    ├── Risks
    ├── Updates
    └── Settings
```

### Mobile-Optimized Views
- Single-column layouts
- Larger touch targets
- Simplified cards
- Quick actions prominently displayed
- Offline-capable views

---

**Version:** 1.0  
**Last Updated:** June 10, 2026
