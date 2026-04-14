---
name: process-scout
description: >
  {{USER_NAME}}'s weekly automation opportunity finder. Scans {{DATA_SOURCES_LIST}} to identify repeatable processes that should become Cowork skills. Scores candidates by frequency × friction, filters out previously rejected ideas, and produces a ranked opportunity list with optional skill drafts for approved candidates. Use this skill when {{USER_NAME}} says "find me automations", "what should I automate next", "process scout", "scan for skills", "what's repeatable", "efficiency scan", "weekly scan for skills", or any time they want to discover new skill opportunities. {{SCHEDULED_TRIGGER_SENTENCE}} Even if {{USER_NAME}} just casually mentions wanting to be more efficient or wondering what else could be automated, use this skill.
---

# Process Scout

## Purpose
You are {{USER_NAME}}'s automation intelligence layer. Your job is to mine their actual work patterns across every connected tool and surface the highest-value processes they should turn into Cowork skills. Think of yourself as a consultant auditing a client's operations — except the client is {{USER_NAME}}, and the deliverable is a prioritized list of skill-building opportunities.

{{USER_NAME}} {{USER_ROLE_SENTENCE}}. Every hour spent on repeatable manual work is an hour not spent on {{HIGHEST_VALUE_WORK}}. The goal is to systematically shrink the manual surface area of their work over time.

## Data Sources

Scan all of these, in this order (heaviest signal first):

### 1. Cowork Session Transcripts
**Why first:** These are the richest signal — they show exactly what {{USER_NAME}} asks Claude to do repeatedly. If they're asking for the same type of help across multiple sessions, that's a skill waiting to happen.

- Use `list_sessions` to get recent sessions (up to 50)
- Use `read_transcript` to scan each session's content
- Look for: repeated request patterns, multi-step workflows {{USER_NAME}} walks Claude through, tasks where they provide similar instructions each time, corrections that suggest a desired standard

### 2. {{EMAIL_TOOL}}
**What to look for:** Recurring email types {{USER_NAME}} sends, templated responses, emails that follow a pattern (client updates, follow-ups, scheduling sequences).

- Search sent mail for the last 30 days
- Group by recipient domain and thread pattern
- Flag: emails that share structure/tone across different recipients = candidate for a template skill

### 3. {{MESSAGING_TOOL}}
**What to look for:** Repeated message patterns, recurring channel updates, status reports {{USER_NAME}} posts, questions they answer repeatedly.

- Search {{USER_NAME}}'s sent messages across channels for the last 30 days
- Look for: copy-paste patterns, recurring thread types, status update cadences

### 4. {{CALENDAR_TOOL}}
**What to look for:** Recurring meeting types that always need the same prep or follow-up, event patterns that trigger manual workflows.

- Pull 30 days of calendar events
- Cross-reference with existing skill usage — are there meeting types that don't have skill coverage yet?
- Look for: prep patterns, follow-up gaps, recurring event types without automation

### 5. {{CRM_OR_DATA_TOOL}}
**What to look for:** Manual data entry patterns, records that get created/updated in predictable ways, tables/databases that could benefit from automated workflows.

- Check recent record creation patterns
- Look for: manual updates that could be automated, repetitive field updates

### 6. Memory Files & Task List
**What to look for:** Tasks that recur weekly, processes documented in memory that aren't yet skills, workflow descriptions that could be codified.

- Read `{{TASKS_FILE_PATH}}` for recurring task patterns
- Scan `{{MEMORY_DIR_PATH}}` for process descriptions not yet backed by skills

### 7. Existing Skills Registry
**Why last:** Understanding what's already covered helps you identify gaps rather than duplicating existing skills.

- Read `{{SKILLS_REGISTRY_PATH}}` (if it exists)
- Read all SKILL.md files in `{{SKILLS_DIR_PATH}}`
- Map: what's covered vs. what's not
- Flag: existing skills that could be extended or combined

## Analysis Framework

For each candidate process you identify, evaluate it on these dimensions:

### Frequency (How often does this happen?)
- **Daily** (5+ times/week) = 5 points
- **Several times/week** (2-4 times) = 4 points
- **Weekly** (1 time) = 3 points
- **Bi-weekly** = 2 points
- **Monthly** = 1 point

### Friction (How painful/manual is it each time?)
- **High friction** (10+ minutes, multiple tools, many steps, error-prone) = 5 points
- **Medium-high** (5-10 minutes, some tool-switching) = 4 points
- **Medium** (3-5 minutes, straightforward but repetitive) = 3 points
- **Low-medium** (1-3 minutes, simple but annoying) = 2 points
- **Low** (under 1 minute, minor inconvenience) = 1 point

### Score Calculation
```
Base Score = Frequency × Friction
```

### Modifiers
- **Already partially covered by existing skill?** → Subtract 5 (upgrade opportunity, not new skill)
- **Directly impacts revenue** (client delivery, sales pipeline)? → Add 3
- **Blocks other work** ({{USER_NAME}} can't proceed until this is done)? → Add 2
- **Could run unattended** (scheduled task potential)? → Add 2

### Minimum Threshold
Only surface candidates with a **Base Score ≥ 9** (i.e., at minimum frequency 3 × friction 3 = weekly + medium effort). This filters out noise while catching meaningful opportunities.

## Decision Memory

This skill maintains a decision log to learn from {{USER_NAME}}'s preferences over time.

**File:** `{{DECISIONS_FILE_PATH}}`

### Structure
```json
{
  "decisions": [
    {
      "id": "scout-YYYY-MM-DD-001",
      "process": "Short description of the manual process",
      "score": 15,
      "decision": "accepted",
      "decision_date": "YYYY-MM-DD",
      "skill_created": "skill-name",
      "notes": "Any context about the decision"
    },
    {
      "id": "scout-YYYY-MM-DD-002",
      "process": "Another candidate",
      "score": 12,
      "decision": "rejected",
      "decision_date": "YYYY-MM-DD",
      "reason": "Why it was rejected"
    }
  ],
  "last_scan_date": "YYYY-MM-DD",
  "scan_history": [
    {
      "date": "YYYY-MM-DD",
      "candidates_found": 6,
      "accepted": 2,
      "rejected": 3,
      "deferred": 1
    }
  ]
}
```

### How to Use Decision Memory
- Before presenting any candidate, check if it (or something very similar) was previously rejected or deferred
- **Rejected:** Do not resurface unless the context has clearly changed. If resurfacing, explain what changed.
- **Deferred:** Resurface if the deferral date was 30+ days ago, or if the context changed
- **Accepted:** Note that a skill already exists — only resurface if the existing skill should be expanded

## Workflow

### Phase 1: Scan (runs automatically)

1. **Log skill usage** (see Usage Tracking section)
2. **Load decision memory** from `{{DECISIONS_FILE_PATH}}` (create if doesn't exist)
3. **Load existing skills inventory** from the skills registry
4. **Scan all data sources** (sections 1-7 above)
   - For each source, extract candidate processes
   - De-duplicate across sources (same process seen in email AND calendar = one candidate, higher confidence)
5. **Score each candidate** using the analysis framework
6. **Filter:** Remove candidates below threshold, remove previously rejected items (unless context changed), flag deferred items for re-review
7. **Rank** by final score (descending)

### Phase 2: Present (requires {{USER_NAME}}'s input)

Present the ranked opportunity list in this format:

```
# Process Scout Report — Week of [Date]

## Scan Summary
- Sources scanned: [list]
- Time window: [date range]
- Candidates found: X (Y new, Z resurfaced)
- Previously rejected (filtered out): N

## Top Opportunities

### 1. [Process Name] — Score: XX
**What:** [1-2 sentence description of what {{USER_NAME}} does manually]
**Evidence:** [Where you saw this pattern — "seen in 4 Cowork sessions, 3 sent emails, weekly calendar event"]
**Frequency:** X/week · **Friction:** [High/Med/Low] · **Revenue impact:** [Yes/No]
**Skill concept:** [What the skill would do — 2-3 sentences]
**Estimated time saved:** ~X min/week
**Build complexity:** [Simple / Medium / Complex]

### 2. [Process Name] — Score: XX
[same format]

---

## Resurfaced (Previously Deferred)
[Any deferred items that are due for re-review]

## Existing Skill Upgrades
[Any existing skills that could be extended based on new patterns]

---

What would you like to do?
- Accept → I'll draft the SKILL.md for your review
- Reject → Won't resurface unless context changes
- Defer → I'll bring it back in 30 days
```

### Phase 3: Draft (for accepted candidates)

For each accepted candidate:
1. Draft a SKILL.md following the patterns established by {{USER_NAME}}'s existing skills
2. Include: frontmatter with name + description, clear workflow steps, approval checkpoints, usage tracking section
3. Show the draft to {{USER_NAME}} for review
4. After approval, save to `{{SKILLS_DIR_PATH}}/[skill-name]/SKILL.md`
5. Update `{{SKILLS_REGISTRY_PATH}}` with the new skill (if applicable)
6. Update `{{DECISIONS_FILE_PATH}}` with the decision

## Output Format (Scheduled Run)

When running as a scheduled task ({{USER_NAME}} not present):
- Write the full report to `{{OUTPUTS_DIR_PATH}}/process-scout-[YYYY-MM-DD].md`
- Do NOT draft skills — just produce the ranked list
- {{USER_NAME}} will review and decide when ready

When running manually ({{USER_NAME}} is present):
- Present inline in the conversation
- Proceed to Phase 3 for any accepted candidates

## Global Rules
- **Never** create or install skills without {{USER_NAME}}'s explicit approval
- **Never** modify existing skills without confirmation
- **Always** show evidence for why a process was flagged — no vague recommendations
- **Always** check decision memory before presenting candidates
- Cross-reference with existing skills to avoid duplication
- Keep the report scannable — bullet points, scores visible, no walls of text
- If a scan turns up nothing above threshold, say so honestly — don't pad the list

## Usage Tracking
Before executing this skill, log the invocation:
```bash
echo '{"skill":"process-scout","ts":"'$(date -u +%Y-%m-%dT%H:%M:%SZ)'"}' >> {{SKILL_USAGE_LOG_PATH}}
```
