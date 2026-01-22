---
description: Morning briefing with calendar, email, and prioritized actions
argument-hint: [full | quick]
---

# Daily Briefing Command

You are preparing a morning briefing. Your job is to aggregate data from connected tools and distill it into the TOP 5 ACTIONS for the day.

---

## Configuration

| Item | Value |
|------|-------|
| **Email** | [your-email@domain.com] |
| **Timezone** | [America/New_York] |
| **Calendar Window** | Today + Tomorrow (48 hours) |

---

## Data Sources

### Calendar (Google Workspace)

Query today's and tomorrow's events:

```
mcp__google-workspace__get_events({
  user_google_email: "[your-email@domain.com]",
  calendar_id: "primary",
  time_min: "[start-of-today-ISO]",
  time_max: "[end-of-tomorrow-ISO]",
  detailed: true
})
```

### Email (Gmail)

Search for emails needing action:

```
mcp__google-workspace__search_gmail_messages({
  user_google_email: "[your-email@domain.com]",
  query: 'is:unread newer_than:3d',
  limit: 15
})
```

**Alternative queries:**
- `label:"Needs Action"` - if you use labels
- `from:(boss@company.com) is:unread` - from specific people
- `is:starred` - starred emails

---

## Priority Scoring

Score each item 0-100 based on:

| Dimension | Weight | Scoring |
|-----------|--------|---------|
| **Time Sensitivity** | 3x | Due today = 10, This week = 5, Later = 2 |
| **Meeting Prep** | 3x | Meeting in <4 hours = 10, Today = 7, Tomorrow = 3 |
| **Stakeholder** | 2x | External = 10, Manager = 7, Team = 5, Internal = 3 |
| **Impact** | 2x | High = 10, Medium = 6, Low = 2 |

Select the TOP 5 highest-scoring items.

---

## Modes

### `full` (Default)
- Calendar: Full 48-hour view
- Email: All needing response
- Output: Complete briefing with TOP 5 actions

### `quick`
- Calendar: Next 2 hours only
- Email: Counts only
- Output: Single most important action

---

## Output Format

```markdown
# Morning Briefing - [Day], [Date]

## TOP 5 ACTIONS

1. **[Action]** - [Why it's important] | Source: [Calendar/Email]
2. ...

---

## Today's Schedule

| Time | Event | Prep Notes |
|------|-------|------------|
| [time] | [event] | [notes] |

---

## Emails Needing Response ([count])

| Thread | From | Age | Priority |
|--------|------|-----|----------|
| [subject] | [sender] | [age] | [HIGH/MEDIUM/LOW] |

---

**Focus:** [One sentence recommendation for the day]
```

---

## Error Handling

If a tool fails:
1. Note it in output: "Calendar unavailable - showing email only"
2. Continue with available sources
3. Never make up fake data

---

## User Request

$ARGUMENTS
