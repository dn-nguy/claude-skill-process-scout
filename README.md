# Process Scout — Skill Template

A starter skill you can customize to become your own weekly automation opportunity finder. It scans the tools you actually use, finds repeatable manual work, scores it, and helps you turn the highest-value processes into Cowork skills.

## What's in this folder
- **SKILL.md** — the templated skill file with `{{PLACEHOLDERS}}` for your name, tools, and file paths
- **README.md** — this file, with the starter prompt you'll drop into Cowork

## How to use it

1. **Attach `SKILL.md`** to a new Cowork conversation (drag it in, or upload it as a file)
2. **Paste the starter prompt below** into that same conversation
3. **Answer the interview questions** Cowork asks you
4. **Review the final SKILL.md** Cowork drafts for you
5. **Save it** to your own skills directory (Cowork will tell you where)

That's it. You'll end up with a personalized `process-scout` skill that knows your tools, your workflows, and your trigger phrases.

---

## Starter prompt (copy-paste this into Cowork)

```
I've attached a templated SKILL.md for a skill called "process-scout." I want to customize it for my own setup so I can use it as an automation opportunity finder for my own work.

Please do the following:

1. Read the attached SKILL.md carefully. Note every {{PLACEHOLDER}} in the file — these are the fields I need to fill in.

2. Interview me using the AskUserQuestion tool (one question at a time, multiple choice where possible, "Other" for free text). Cover:
   - My name and how I should be referred to in the skill
   - My role / business / what I do (one sentence)
   - What I consider my highest-value work (the thing I'd rather be doing instead of manual tasks)
   - Which email tool I use (Gmail, Outlook, Superhuman, etc.)
   - Which messaging tool I use (Slack, Teams, Discord, none, etc.)
   - Which calendar tool I use (Google Calendar, Outlook, Cal.com, etc.)
   - Which CRM or structured data tool I use (Airtable, Notion, HubSpot, Salesforce, none, etc.)
   - Where my tasks live (file path, e.g. TASKS.md, or a tool name)
   - Where my memory / notes directory lives (file path or tool)
   - Where my existing skills live (directory path, typically .claude/skills/)
   - Whether I have a skills registry JSON file (and if so, the path)
   - Where scout decision memory should be stored (file path)
   - Where scheduled-run outputs should go (directory path)
   - Where my skill usage log lives (file path, or "don't track usage")
   - Whether I want this to run on a schedule (weekly? monthly? manual only?)
   - Any additional trigger phrases I want beyond the defaults

3. Show me a summary of my answers before writing anything. Let me correct anything.

4. Once I confirm, invoke the skill-creator skill (/skill-creator) and use my answers to generate a final, personalized SKILL.md. Replace every {{PLACEHOLDER}} in the template with my values. Remove any data source sections for tools I said I don't use.

5. Show me the final SKILL.md for review. Tell me exactly where to save it on my machine (based on my existing skills directory path).

6. After I approve, also tell me:
   - How to test it (a sample trigger phrase to try)
   - How to set up the scheduled task if I asked for one
   - Any manual file/directory creation I need to do (e.g. creating the decisions.json file)

Rules:
- Do not write any files until I've confirmed the interview answers AND reviewed the final SKILL.md
- Keep the interview tight — don't ask follow-up questions unless an answer is genuinely ambiguous
- If I say "I don't use X" for a data source, drop that section entirely from the final skill rather than leaving a placeholder
- Preserve the scoring framework, decision memory structure, and workflow phases exactly as written in the template — only customize the placeholders
```

---

## What to do after the skill is created

- Try triggering it with a phrase like "what should I automate next" to test
- If you asked for it to run on a schedule, ask Cowork to set up the weekly scheduled task (e.g. "schedule process-scout to run every Monday at 9am") — it will use the schedule skill to wire it up
- Let it run for a week or two before expecting great results — it needs scan history to learn your patterns
- When it surfaces a candidate you accept, it'll draft the new skill for you automatically

## Questions or feedback

Built by Dawn Nguyen / Kiln Studio as a community template. Customize freely.
