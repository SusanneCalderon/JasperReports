# Team Shared Memory

This directory contains shared knowledge and context across team members working on the JasperReports repository.

## Purpose

Since Claude Code conversation history is local to each machine, this directory serves as **version-controlled shared memory** that all team members can access and update.

## Files

- **recent-work.md** - Latest changes, ongoing work, and current context
- **known-issues.md** - Bugs, gotchas, workarounds, and debugging tips
- **learned-patterns.md** - Best practices, tips for JasperReports/Jaspersoft Studio

## When to Update

Update these files after:
- Solving a non-trivial problem
- Discovering database quirks or JSONB patterns
- Making significant changes to report structure
- Finding workarounds for Jaspersoft Studio issues
- Learning something that would help the next person

## How to Update

After completing work, ask Claude:
```
Please update .claude-memory/[relevant-file].md with what we learned/changed today
```

Then commit and push:
```bash
git add .claude-memory/
git commit -m "Update team memory: [brief description]"
git push
```

## Before Starting Work

Always pull latest and check these files for recent team context:
```bash
git pull
cat .claude-memory/recent-work.md
```
