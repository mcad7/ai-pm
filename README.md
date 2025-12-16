# AI-PM: AI-Powered Project Management

Automated project management workflow using Claude AI to convert Project Briefs into structured EPICs with full GitHub integration.

## Overview

AI-PM streamlines the project planning process by automatically converting stakeholder briefs into well-structured EPICs, complete with proper labeling, success criteria, and implementation tracking.

### Key Features

- **Automated Brief → EPIC Conversion:** AI analyzes project briefs and creates structured EPIC issues
- **Intelligent Label Management:** Automatic priority and type label assignment
- **Consistent Structure:** Every EPIC follows the same comprehensive format
- **Traceability:** Clear links between Briefs and EPICs
- **GitHub Native:** Built entirely on GitHub Issues and Actions

### Workflow

```
Brief Issue → Add 'process' label → Claude AI processes → EPIC created
```

**Time to EPIC:** ~2 minutes after adding label

## Quick Start

### 1. Setup (First Time Only)

**Install Claude GitHub App:**
```
https://github.com/apps/claude
```

**Add API Key Secret:**
1. Go to Settings → Secrets and variables → Actions
2. Create new secret: `ANTHROPIC_API_KEY`
3. Add your Anthropic API key

**Create Labels:**
1. Go to Actions tab
2. Run "Setup Repository Labels" workflow
3. Verify 16 labels created

### 2. Create a Brief

1. Go to **Issues** → **New issue**
2. Select **📋 Project Brief** template
3. Fill required fields:
   - Project Name
   - Project Type
   - Overview (with business value)
   - Scope & Requirements
4. Submit issue

### 3. Process Brief into EPIC

1. Open the Brief issue
2. Add label: **`process`**
3. Wait ~2 minutes
4. EPIC automatically created with:
   - Structured format
   - Proper labels (epic + priority + type)
   - Link to source Brief
   - Implementation tracking checklist

### 4. Use the EPIC

1. Review EPIC structure
2. Share with team
3. Track progress using checkboxes
4. (Future) Add 'breakdown' label to generate user stories

## Documentation

**Full Guide:** [docs/workflow-guide.md](docs/workflow-guide.md)

Contents:
- Getting Started
- Creating Briefs (best practices)
- Processing Workflow
- EPIC Structure Explained
- Labels System
- Troubleshooting
- FAQ

## Label System

### Hierarchy Labels
- `brief` - Project brief submitted by stakeholder
- `epic` - Large body of work broken into user stories
- `user-story` - User-focused feature (future)
- `task` - Technical implementation work (future)

### Priority Labels
- `critical` - Blocking other work
- `high` - Important for next release
- `medium` - Nice to have soon
- `low` - Future consideration

### Type Labels
- `feature` - New feature development
- `enhancement` - Enhancement to existing feature
- `technical-debt` - Technical debt or refactoring
- `integration` - Integration with external systems
- `bug-fix` - Bug fix or correction
- `infrastructure` - Infrastructure or DevOps work

## EPIC Structure

Every EPIC includes:

- **Metadata:** Source Brief link, Type, Priority
- **Overview:** Business value and problem statement
- **Scope & Requirements:** All requirements from Brief
- **Success Criteria:** Measurable outcomes
- **Technical Context:** Dependencies, constraints, tech stack
- **Implementation Tracking:** Checkboxes for progress
- **Stakeholders:** Key decision makers

## Future Roadmap

### Phase 2: EPIC → User Stories
- Add 'breakdown' label to EPIC
- Claude generates 3-8 user stories
- Format: `[STORY] As a {user}, I want {goal}`
- Auto-link to parent EPIC

### Phase 3: User Story → Tasks
- Technical task breakdown
- Estimation and complexity
- Implementation specifics

### Phase 4: Project Board Integration
- Automated board management
- Progress tracking visualization
- Status columns and automation

## Architecture

### Workflows

**`.github/workflows/setup-labels.yml`**
- One-time label setup
- Creates/updates 16 labels
- Manual trigger only

**`.github/workflows/process-brief.yml`**
- Main automation workflow
- Triggered when 'process' label added to Brief
- Uses Claude Code Action
- Creates EPIC with proper structure and labels

### Issue Templates

**`.github/ISSUE_TEMPLATE/brief.yml`**
- Structured form for project briefs
- 4 required fields + 6 optional
- Auto-labeled with 'brief'

## Requirements

- GitHub repository with Issues enabled
- Claude GitHub App installed
- Anthropic API key (in repository secrets)
- Repository permissions:
  - Contents: Write
  - Issues: Write
  - Pull Requests: Write

## Technology Stack

- **GitHub Actions:** Workflow automation
- **Claude Code Action:** AI processing
- **Claude Sonnet 4.5:** Language model
- **GitHub Issues:** Data storage
- **YAML:** Configuration and templates

## Examples

### Example Brief
```
Project Name: User Authentication System
Type: New Feature
Priority: High

Overview:
Build a two-factor authentication system to improve account
security and meet compliance requirements.

Scope:
- SMS and authenticator app support
- Admin dashboard for metrics
- Backup codes for recovery
```

### Generated EPIC
```markdown
# Epic: User Authentication System

> Source Brief: #123
> Type: New Feature
> Priority: High

[Full structured EPIC with all sections...]
```

## Troubleshooting

**Workflow not triggering?**
- Verify issue has both 'brief' and 'process' labels
- Check Actions tab for errors
- Verify ANTHROPIC_API_KEY secret exists

**EPIC missing labels?**
- Manually add missing labels
- Verify labels exist (run setup-labels workflow)
- Check Brief had priority/type selected

**More help:** See [Troubleshooting Guide](docs/workflow-guide.md#troubleshooting)

## Contributing

This is an internal project management tool. For issues or suggestions:

1. Create an issue describing the problem
2. Tag with 'bug' or 'enhancement'
3. Include workflow run links if applicable

## License

Internal use only.

---

**Version:** 1.0.0 (Phase 1)
**Last Updated:** 2025-12-16
**Powered by:** [Claude Code Action](https://github.com/anthropics/claude-code-action)
