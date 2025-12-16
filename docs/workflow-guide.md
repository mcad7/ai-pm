# AI-PM Workflow Guide

Complete guide to the automated Brief → EPIC workflow using Claude Code Action.

## Table of Contents

1. [Overview](#overview)
2. [Getting Started](#getting-started)
3. [Creating a Brief](#creating-a-brief)
4. [Processing a Brief into EPIC](#processing-a-brief-into-epic)
5. [Understanding EPICs](#understanding-epics)
6. [Labels System](#labels-system)
7. [Future: Breaking Down EPICs](#future-breaking-down-epics)
8. [Troubleshooting](#troubleshooting)
9. [FAQ](#faq)

## Overview

### What is this workflow?

The AI-PM workflow automates the conversion of Project Briefs into structured EPICs using Claude AI. This ensures consistent project documentation and faster time-to-execution.

### Workflow Architecture

```
┌─────────────────────┐
│  1. Create Brief    │ ← User fills issue template
│     (Issue)         │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│  2. Add 'process'   │ ← User adds label
│     label           │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│  3. GitHub Actions  │ ← Automatic
│     triggers        │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│  4. Claude reads    │ ← AI processing
│     & analyzes      │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│  5. EPIC created    │ ← New issue created
│     with structure  │   with proper labels
└─────────────────────┘
```

### Key Benefits

- **Consistency:** Every EPIC follows the same structure
- **Speed:** EPIC created in ~2 minutes after adding 'process' label
- **Traceability:** Clear links between Briefs and EPICs
- **Automation:** No manual copying or formatting needed
- **Intelligence:** AI generates missing success criteria when needed

## Getting Started

### Prerequisites

Before using this workflow, ensure:

1. **GitHub App Installed:** Claude app must be installed on the repository
   - Install from: https://github.com/apps/claude

2. **API Key Configured:** Repository must have `ANTHROPIC_API_KEY` secret
   - Admin access required to add secrets
   - Go to: Settings → Secrets and variables → Actions

3. **Labels Created:** Run the setup-labels workflow once
   - Go to: Actions tab → Setup Repository Labels → Run workflow
   - Verifies 16 labels are created

### First-Time Setup

**Step 1: Install Claude GitHub App**
```
1. Visit https://github.com/apps/claude
2. Click "Install"
3. Select your repository (mcad7/ai-pm)
4. Grant required permissions (Contents, Issues, Pull Requests)
```

**Step 2: Add API Key Secret**
```
1. Go to repository Settings
2. Navigate to: Secrets and variables → Actions
3. Click "New repository secret"
4. Name: ANTHROPIC_API_KEY
5. Value: Your Anthropic API key (starts with sk-ant-)
6. Click "Add secret"
```

**Step 3: Create Labels**
```
1. Go to Actions tab
2. Select "Setup Repository Labels" workflow
3. Click "Run workflow"
4. Wait for completion (~30 seconds)
5. Verify in Issues tab → Labels (should see 16 labels)
```

## Creating a Brief

### Step-by-Step Instructions

**1. Navigate to Issues**
- Go to the repository Issues tab
- Click "New issue"

**2. Select Brief Template**
- Click "📋 Project Brief"
- Template form will load

**3. Fill Required Fields**

**Project Name** (required)
```
Short, descriptive name for the project
Example: "User Authentication System"
```

**Project Type** (required)
```
Select from dropdown:
- New Feature (brand new functionality)
- Enhancement (improvement to existing feature)
- Technical Debt (refactoring, code quality)
- Integration (connecting with external systems)
- Bug Fix (fixing existing issues)
- Infrastructure (DevOps, deployment, etc.)
- Other
```

**Project Overview** (required)
```
High-level description including:
- What needs to be built
- Why it's important (business value)
- What problem it solves

Keep under 500 characters for simple projects.

Example:
"Build a two-factor authentication system to improve account
security and meet compliance requirements. Current system only
uses passwords, creating security risks."
```

**Scope & Requirements** (required)
```
List features and functionality as bullet points.
One requirement per line.

Example:
- User can enable 2FA via SMS or authenticator app
- Admin dashboard shows 2FA adoption metrics
- Integration with existing user management system
- Backup codes for account recovery
```

**4. Fill Optional Fields (Recommended)**

**Success Criteria**
```
How will we know the project is complete?
If left empty, Claude will generate criteria based on requirements.

Example:
- Users can successfully enable and use 2FA
- 95% of authentication tests pass
- Security audit completed
- Documentation published
```

**Technical Constraints & Dependencies**
```
Any technical limitations or dependencies

Example:
- Must use existing authentication API
- Requires database schema migration
- Depends on User Service v2.0 release
```

**Priority**
```
Select urgency level:
- Critical - Blocking other work
- High - Important for next release
- Medium - Nice to have soon
- Low - Future consideration
```

**Stakeholders**
```
Use @mentions
Example: @username1 @username2
```

**Technical Context**
```
Current tech stack or architectural notes

Example:
- Backend: Node.js + Express
- Frontend: React
- Database: PostgreSQL
```

**5. Submit Brief**
- Review all fields
- Click "Submit new issue"
- Brief is created with 'brief' label automatically

### Brief Best Practices

**DO:**
- ✅ Use clear, concise language
- ✅ Be specific about requirements
- ✅ Include business value in overview
- ✅ Use bullet points for requirements
- ✅ Mention dependencies early

**DON'T:**
- ❌ Write essays (keep it brief)
- ❌ Mix multiple projects in one brief
- ❌ Assume context (be explicit)
- ❌ Skip required fields
- ❌ Use vague requirements

## Processing a Brief into EPIC

### Triggering the Workflow

**When to Process:**
- After Brief is reviewed and approved
- When ready to start planning work
- Once all clarifications are addressed

**How to Trigger:**

1. **Open the Brief issue**
2. **Add the 'process' label:**
   - Right sidebar → Labels
   - Click gear icon
   - Select "process"
   - Label applied

3. **Workflow automatically starts:**
   - GitHub Actions triggered
   - Monitor in Actions tab
   - Takes ~1-2 minutes

### What Happens During Processing

**Step 1: Workflow Triggers** (immediate)
- GitHub detects 'process' label added
- Verifies issue has 'brief' label
- Starts process-brief workflow

**Step 2: Claude Reads Brief** (~20 seconds)
- Extracts all YAML form fields
- Parses project type and priority
- Analyzes requirements and scope

**Step 3: Claude Creates EPIC** (~60-90 seconds)
- Generates structured EPIC issue
- Maps priority and type to labels
- Generates success criteria if missing
- Creates implementation tracking checklist

**Step 4: Links Created** (~10 seconds)
- Adds comment to Brief with EPIC link
- Links Brief in EPIC header
- Applies all labels to EPIC

**Step 5: Complete**
- Brief updated with success message
- EPIC ready for breakdown
- Notification sent to watchers

### Monitoring Progress

**Via GitHub Actions:**
```
1. Go to Actions tab
2. Find "Process Brief to EPIC" workflow
3. Click on running workflow
4. View real-time logs
5. Check for errors or warnings
```

**Via Issue Comments:**
```
- Brief issue will receive comment when complete
- Comment includes EPIC number and labels
- Click link to view new EPIC
```

## Understanding EPICs

### EPIC Structure

**Title Format:**
```
[EPIC] {Project Name}

Example: [EPIC] User Authentication System
```

**Body Sections:**

**1. Header (Metadata)**
```markdown
> **Source Brief:** #123
> **Type:** New Feature
> **Priority:** High
```

**2. Overview**
- Complete overview from Brief
- Business value and problem statement
- Why this project matters

**3. Business Value**
- Extracted or summarized from overview
- Clear articulation of benefits
- Expected outcomes

**4. Scope & Requirements**
- All requirements from Brief
- Bullet point format
- One requirement per line

**5. Success Criteria**
- Criteria from Brief OR
- AI-generated based on requirements
- Measurable outcomes

**6. Technical Context**
- Dependencies & Constraints
- Tech Stack information
- Architectural notes

**7. Stakeholders**
- @mentioned stakeholders
- Decision makers
- Key contacts

**8. Implementation Tracking**
- User Stories placeholder (checklist)
- Definition of Done (checkboxes)
- Progress tracking

**9. Additional Context**
- Links to designs, research
- Related issues
- Documentation

**10. Footer**
- Link to source Brief
- Auto-generation note

### EPIC Labels

Every EPIC has 3 types of labels:

**1. Hierarchy Label:**
- `epic` (always present)

**2. Priority Label:**
- `critical` (blocking work)
- `high` (important for next release)
- `medium` (nice to have soon)
- `low` (future consideration)

**3. Type Label:**
- `feature` (new functionality)
- `enhancement` (improve existing)
- `technical-debt` (refactoring)
- `integration` (external systems)
- `bug-fix` (fixing issues)
- `infrastructure` (DevOps work)

### Using EPICs

**Review EPIC:**
1. Open EPIC issue
2. Verify all information transferred correctly
3. Check success criteria make sense
4. Confirm labels are appropriate

**Next Steps:**
1. Share EPIC with team
2. Estimate complexity
3. When ready, add 'breakdown' label (future feature)
4. User stories will be auto-generated

## Labels System

### Label Categories

**Hierarchy Labels** (Purple)
```
brief          - E1D5E7 - Project brief submitted
epic           - 8E44AD - Large body of work
user-story     - C39BD3 - User-focused feature
task           - D7BDE2 - Technical implementation
```

**Process Labels** (Green)
```
process        - 0E8A16 - Trigger: Convert brief to epic
breakdown      - (future) - Trigger: Create user stories
```

**Priority Labels** (Red/Orange/Yellow)
```
critical       - B60205 - Blocking other work
high           - D93F0B - Important for next release
medium         - FBCA04 - Nice to have soon
low            - FEF2C0 - Future consideration
```

**Type Labels** (Blue/Purple/Red)
```
feature        - 1D76DB - New feature development
enhancement    - 5DADE2 - Enhancement to existing
technical-debt - 7D3C98 - Technical debt/refactoring
integration    - 16A085 - Integration with externals
bug-fix        - E74C3C - Bug fix or correction
infrastructure - 34495E - Infrastructure or DevOps
```

### Label Mapping Logic

**From Brief to EPIC:**

Priority Mapping:
```
Brief Priority Field              → EPIC Label
"Critical - Blocking other work"  → critical
"High - Important for next release" → high
"Medium - Nice to have soon"      → medium
"Low - Future consideration"      → low
(not selected)                    → medium (default)
```

Type Mapping:
```
Brief Project Type → EPIC Label
"New Feature"      → feature
"Enhancement"      → enhancement
"Technical Debt"   → technical-debt
"Integration"      → integration
"Bug Fix"          → bug-fix
"Infrastructure"   → infrastructure
"Other"            → feature (default)
```

## Future: Breaking Down EPICs

### Phase 2: EPIC → User Stories

**Coming Soon:** Automatic user story generation from EPICs.

**How it will work:**
1. Add 'breakdown' label to EPIC
2. Claude analyzes EPIC requirements
3. Generates 3-8 user stories
4. Format: `[STORY] As a {user}, I want {goal}`
5. Each story inherits priority and type labels
6. EPIC checklist updated with story links

**User Story Format:**
```markdown
[STORY] As a user, I want to enable 2FA so that my account is more secure

## Parent EPIC
#123 - User Authentication System

## Description
{Detailed description}

## Acceptance Criteria
- [ ] Criterion 1
- [ ] Criterion 2

## Technical Notes
{Implementation notes}
```

### Phase 3: User Story → Tasks

**Future Feature:** Break stories into technical tasks.

**Trigger:** Add 'breakdown' label to User Story

**Task Format:**
```markdown
[TASK] Create 2FA enrollment API endpoint

## Parent Story
#456 - As a user, I want to enable 2FA

## Technical Details
{Implementation specifics}

## Estimated Complexity
Medium (4-6 hours)
```

## Troubleshooting

### Common Issues

**Issue: Workflow doesn't trigger**

Symptoms:
- Added 'process' label but nothing happens
- No workflow run in Actions tab

Solutions:
1. Verify issue has 'brief' label (must have both)
2. Check Actions tab for errors
3. Verify workflow file exists: `.github/workflows/process-brief.yml`
4. Check repository permissions

**Issue: ANTHROPIC_API_KEY not found**

Symptoms:
- Workflow fails immediately
- Error: "Secret ANTHROPIC_API_KEY not found"

Solutions:
1. Go to Settings → Secrets and variables → Actions
2. Verify `ANTHROPIC_API_KEY` secret exists
3. If missing, add it (requires admin access)
4. Re-run workflow

**Issue: EPIC created but labels missing**

Symptoms:
- EPIC exists but only has 'epic' label
- Priority or type labels not applied

Solutions:
1. Manually add missing labels
2. Check Brief had priority/type selected
3. Verify labels exist in repository (run setup-labels workflow)
4. Report issue for investigation

**Issue: EPIC structure incorrect**

Symptoms:
- Missing sections in EPIC
- Formatting issues
- Content not transferred

Solutions:
1. Check Brief had all required fields filled
2. Review workflow logs in Actions tab
3. Manually edit EPIC to fix structure
4. Report if consistent issue

**Issue: Claude couldn't read Brief**

Symptoms:
- Comment: "⚠️ Unable to read Brief issue"
- EPIC not created

Solutions:
1. Verify Brief was created using issue template
2. Check Brief is not corrupted
3. Try removing and re-adding 'process' label
4. Create new Brief if issue persists

### Getting Help

**Check Workflow Logs:**
```
1. Go to Actions tab
2. Find failed workflow run
3. Click on run
4. Expand steps to see detailed logs
5. Look for error messages
```

**Report Issues:**
- Open new issue in repository
- Tag with 'bug' label
- Include:
  - Brief issue number
  - EPIC issue number (if created)
  - Workflow run link
  - Description of problem
  - Expected vs actual behavior

## FAQ

**Q: How long does it take to create an EPIC?**
A: Typically 1-2 minutes after adding the 'process' label.

**Q: Can I edit the EPIC after it's created?**
A: Yes! EPICs are regular issues and can be edited anytime.

**Q: What if I made a mistake in the Brief?**
A: Edit the Brief issue, then either manually update the EPIC or close it and create a new one.

**Q: Can I process multiple Briefs at once?**
A: Yes, add 'process' label to each. They'll be processed in parallel.

**Q: What happens to the Brief after EPIC is created?**
A: Brief stays open with a link to the EPIC. You can close it manually if desired.

**Q: Do I need to fill all optional fields?**
A: No, but more information = better EPIC. Claude will handle empty fields gracefully.

**Q: Can I customize the EPIC structure?**
A: Yes, edit `.github/workflows/process-brief.yml` to modify the prompt.

**Q: What if Claude generates wrong success criteria?**
A: Manually edit the EPIC to correct them. Consider filling success criteria in Brief next time.

**Q: Can I re-process a Brief?**
A: Remove and re-add 'process' label, but this will create a duplicate EPIC.

**Q: How do I track progress on EPICs?**
A: Use the checkboxes in "Implementation Tracking" section. Future: project boards.

**Q: When will EPIC → User Stories be available?**
A: Phase 2 feature, timeline TBD. Follow repository updates.

**Q: Can I use this for bugs or tech debt?**
A: Yes! Select appropriate Project Type in Brief.

---

**Last Updated:** 2025-12-16
**Version:** 1.0.0 (Phase 1)
