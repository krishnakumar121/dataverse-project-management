# Form Specifications

## Project Main Form

### Layout

```
┌─────────────────────────────────────────┐
│ PROJECT MAIN FORM                       │
├─────────────────────────────────────────┤
│ [Overview] [Timeline] [Budget] [Health] │
├─────────────────────────────────────────┤
│                                         │
│ TAB 1: OVERVIEW                         │
│                                         │
│ ┌─────────────────────────────────────┐ │
│ │ Project Name:     [________________] │ │
│ │ Description:      [                ] │ │
│ │                   [                ] │ │
│ │                   [________________] │ │
│ │                                     │ │
│ │ Status:           [Dropdown ▼]      │ │
│ │ Priority:         [Dropdown ▼]      │ │
│ │ Owner:            [Lookup ▼]        │ │
│ │ Client/Dept:      [________________] │ │
│ │                                     │ │
│ │ [Save] [Discard] [Archive]          │ │
│ └─────────────────────────────────────┘ │
│                                         │
│ RELATED DATA (Sub-grids)                │
│ ┌─────────────────┬─────────────────┐   │
│ │ Tasks (5)       │ Risks (2)       │   │
│ ├─────────────────┼─────────────────┤   │
│ │ Title   [...]   │ Risk [...] [5]  │   │
│ │ Title   [...]   │ Risk [...] [6]  │   │
│ │ [+New]          │ [+New]          │   │
│ └─────────────────┴─────────────────┘   │
│                                         │
└─────────────────────────────────────────┘
```

### Tab 1: Overview

| Field | Type | Required | Validation | Notes |
|-------|------|----------|------------|---------|
| Project Name | Text (200) | Yes | Unique, no special chars | Primary field |
| Description | Multi-line (2000) | No | None | Use for context |
| Status | Choice | Yes | New, Active, On Hold, Closed, Cancelled | Drives workflows |
| Priority | Choice | Yes | Low, Medium, High, Critical | Ranking |
| Owner | Lookup (User) | Yes | Active users only | Project manager |
| Client/Department | Text (200) | No | None | Organization info |

### Tab 2: Timeline

| Field | Type | Display | Notes |
|-------|------|---------|-------|
| Start Date | Date | Read-only | Set at creation |
| End Date | Date | Read-only | Set at creation |
| Duration (Days) | Calculated | Read-only | End Date - Start Date |
| Days Remaining | Calculated | Read-only | End Date - Today |
| Status Color | Calculated | Read-only | Green/Yellow/Red |

### Tab 3: Budget

| Field | Type | Display | Notes |
|-------|------|---------|-------|
| Total Budget | Currency | Editable | Project total |
| Budget Spent | Calculated | Read-only | Sum from expenses |
| Budget Remaining | Calculated | Read-only | Total - Spent |
| Budget % Used | Calculated | Visual bar | 0-100% |
| Budget Status | Calculated | Red/Yellow/Green | >80% = red |

### Tab 4: Health & Status

| Field | Type | Display | Notes |
|-------|------|---------|-------|
| Health Status | Choice | Editable | Green, Yellow, Red |
| Completion % | Integer | Editable | 0-100 |
| Progress Notes | Multi-line | Editable | Update notes |
| Last Updated | Date | Calculated | Auto-timestamp |
| Upcoming Risks | Link | Read-only | Count of high risks |
| Overdue Tasks | Link | Read-only | Count of overdue |

### Tab 5: Related Data (Sub-grids)

**Tasks Sub-grid:**
- Columns: Title, Status, Assigned To, Due Date, Priority
- Quick Create: Enabled
- Sort: Due Date ascending
- Records: Top 10

**Milestones Sub-grid:**
- Columns: Name, Target Date, Status, % Complete
- Quick Create: Enabled
- Sort: Target Date ascending
- Records: Top 5

**Team Members Sub-grid:**
- Columns: Name, Role, Hours Allocated, Utilization %
- Quick Create: Enabled
- Sort: Name
- Records: Top 10

**Risks Sub-grid:**
- Columns: Description, Impact, Probability, Score, Status
- Quick Create: Enabled
- Sort: Score descending
- Records: Top 5

---

## Task Form

### Quick Form (for inline editing)

```
┌────────────────────────────────┐
│ TASK QUICK FORM                │
├────────────────────────────────┤
│ Title:      [__________________] │
│ Status:     [Dropdown ▼]        │
│ Assigned:   [Lookup ▼]          │
│ Due Date:   [Date Picker]       │
│ Priority:   [Dropdown ▼]        │
│                                │
│ [Save] [Discard]               │
└────────────────────────────────┘
```

### Main Form

**Tabs:**

1. **Details**
   - Title (Text, Required)
   - Description (Multi-line)
   - Status (Choice, Required)
   - Priority (Choice, Required)
   - Project (Lookup, Required)

2. **Assignment**
   - Assigned To (Lookup, Required)
   - Due Date (Date, Required)
   - Estimated Hours (Decimal)
   - Actual Hours (Decimal)

3. **Progress**
   - % Complete (Integer, 0-100)
   - Completion Date (Date)
   - Notes (Multi-line)

4. **Related**
   - Parent Task (Lookup)
   - Child Tasks (Sub-grid)
   - Milestone (Lookup)

---

## Resource Allocation Form

### Layout

```
┌─────────────────────────────────────────┐
│ RESOURCE ALLOCATION FORM                │
├─────────────────────────────────────────┤
│ [Details] [Schedule] [Utilization]      │
├─────────────────────────────────────────┤
│                                         │
│ TAB 1: DETAILS                          │
│                                         │
│ Team Member:  [Lookup ▼]                │
│ Project:      [Lookup ▼]                │
│ Role:         [Dropdown ▼]              │
│ Hours:        [Decimal ____]            │
│ % Allocated:  [Calculated Read-only]    │
│                                         │
│ TAB 2: SCHEDULE                         │
│                                         │
│ Start Date:   [Date Picker]             │
│ End Date:     [Date Picker]             │
│ Duration:     [Calculated]              │
│                                         │
│ TAB 3: UTILIZATION                      │
│                                         │
│ Current %:    [Visual: ████░░░░░ 60%]   │
│ Status:       [Green ✓]                 │
│ Total Hours:  [Calculated]              │
│                                         │
│ [Save] [Discard]                        │
│                                         │
└─────────────────────────────────────────┘
```

---

## Risk Register Form

### Main Form

**Tabs:**

1. **Identification**
   - Description (Multi-line, Required)
   - Category (Choice)
   - Probability (Choice, Required)
   - Impact (Choice, Required)

2. **Assessment**
   - Risk Score (Calculated)
   - Severity (Color-coded)
   - Owner (Lookup, Required)
   - Project (Lookup, Required)

3. **Mitigation**
   - Mitigation Plan (Multi-line, Required)
   - Owner (Lookup)
   - Target Closure Date (Date)
   - Contingency Plan (Multi-line)

4. **Status**
   - Status (Choice)
   - Identified Date (Date, Auto)
   - Last Review (Date)
   - Review Notes (Multi-line)

---

## Milestone Form

### Layout

```
┌─────────────────────────────────┐
│ MILESTONE FORM                  │
├─────────────────────────────────┤
│ Name:         [_______________] │
│ Project:      [Lookup ▼]        │
│ Target Date:  [Date Picker]     │
│ Status:       [Dropdown ▼]      │
│ Description:  [               ] │
│               [_______________] │
│                                 │
│ Deliverables: [               ] │
│               [_______________] │
│                                 │
│ Owner:        [Lookup ▼]        │
│ Completion %: [Integer: ___]    │
│                                 │
│ [Save] [Discard]                │
│                                 │
└─────────────────────────────────┘
```

---

## Form Customization Guidelines

### Best Practices

1. **Organize logically** - Group related fields in tabs
2. **Minimize required fields** - Only mark truly required fields
3. **Use appropriate controls** - Dropdown for choices, Lookup for relationships
4. **Provide defaults** - Auto-populate where possible
5. **Add help text** - Tooltips for complex fields
6. **Validate data** - Client-side + server-side validation
7. **Highlight errors** - Red borders, error messages
8. **Mobile-friendly** - Test on actual devices

### Field Validation

```
Project Name:
- Required
- Max 200 characters
- Pattern: Allow alphanumeric, dash, space
- Unique across tenant

Start Date / End Date:
- Required
- Valid date format
- End Date >= Start Date
- Server-side validation

Budget:
- Optional
- Currency format
- >= 0
- Max 999,999,999.99

Email (in Team Member):
- Format: Valid email
- Required if not disabled
- Check against AD
```

### Business Rules

```
Rule 1: Auto-set Status to 'Active'
- When: Project created
- IF Status is empty
- THEN Set Status = 'New'

Rule 2: Lock fields when closed
- When: Project status = 'Closed'
- THEN Lock all editable fields
- Except: Status, Notes

Rule 3: Validate dates
- When: End Date changed
- IF End Date < Start Date
- THEN Show error and prevent save

Rule 4: Calculate utilization
- When: Hours Allocated saved
- Calculate: % = (Hours Allocated / Total Hours) * 100
- Set Utilization Level based on %
```

---

**Version:** 1.0  
**Last Updated:** June 10, 2026
