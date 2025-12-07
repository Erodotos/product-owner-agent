# Product Owner Agent - Automation Workflow

## Overview

This document describes the Product Owner Agent workflow designed to assist Product Owners in conducting research, developing product requirements, and creating implementation-ready user stories. The agent automates the generation of structured reports that serve as guidelines for product implementation.

**Platform:** OpenCode (https://opencode.ai/)  
**Target Audience:** Product Owners  
**Project Scope:** Single product/project  
**Execution Model:** Manual triggering of independent workflow steps

---

## Table of Contents

1. [Agent Capabilities](#agent-capabilities)
2.  [Workflow Overview](#workflow-overview)
3. [Setup & Configuration](#setup--configuration)
4.  [Commands & Usage](#commands--usage)
5.  [Report Templates](#report-templates)
6.  [Best Practices](#best-practices)
7. [Troubleshooting](#troubleshooting)

---

## Agent Capabilities

The Product Owner Agent can:

- ✅ Conduct research using Brave Search API (MCP)
- ✅ Analyze manual notes, observations, and URLs
- ✅ Generate comprehensive research summaries
- ✅ Create detailed Product Requirements Documents (PRDs)
- ✅ Break down features into actionable user stories
- ✅ Structure all outputs using industry-standard templates
- ✅ Save all reports in organized directory structure

---

## Workflow Overview

The workflow consists of three independent, manually-triggered phases:

```
┌─────────────────────────────────────────────────────────────────┐
│                     PRODUCT OWNER WORKFLOW                       │
└─────────────────────────────────────────────────────────────────┘

Phase 1: RESEARCH
├─ Input: Manual notes, URLs, topics
├─ Process: Agent conducts research using Brave Search API
├─ Command: /generate-research-summary [topic]
└─ Output: ./agent-output/research/[topic]-research-summary.md

                    ↓ (Manual Review & Approval)

Phase 2: PRODUCT REQUIREMENTS
├─ Input: Feature/product name, research summary (reference)
├─ Process: Agent creates comprehensive PRD
├─ Command: /generate-prd [feature/product name]
└─ Output: ./agent-output/prd/[feature-name]-prd.md

                    ↓ (Manual Review & Approval)

Phase 3: USER STORIES
├─ Input: Feature name, PRD (reference)
├─ Process: Agent breaks down features into user stories
├─ Command: /generate-user-stories [feature name]
└─ Output: ./agent-output/user-stories/[feature-name]-user-stories.md
```

### Workflow Characteristics

- **Independent Steps**: Each phase can be triggered independently
- **Manual Triggering**: Product Owner controls when each phase executes
- **Validation Checkpoints**: Review and approve outputs before proceeding
- **Flexible Execution**: Can skip phases or repeat as needed
- **Incremental Development**: Generate multiple PRDs or user stories for different features

---

## Setup & Configuration

### Prerequisites

1. **OpenCode Account**: Active account at https://opencode.ai/
2. **Brave Search API Access**: MCP (Model Context Protocol) integration configured
3. **Directory Structure**: Create output directories in your project root

### Initial Setup

#### Step 1: Create Output Directories

```bash
mkdir -p agent-output/research
mkdir -p agent-output/prd
mkdir -p agent-output/user-stories
```

#### Step 2: Configure OpenCode Agent

1. Navigate to OpenCode agent configuration
2. Set up the Product Owner Agent with the following:

**Agent Name:** `Product Owner Agent`

**Agent Description:**
```
Product Owner assistant that conducts research, generates PRDs, 
and creates user stories for product development.  Outputs structured 
markdown reports based on standardized templates.
```

**System Prompt:**
```
You are an expert Product Owner agent specializing in product research, 
requirements documentation, and user story creation. Your role is to:

1. Conduct thorough research using available tools (Brave Search API)
2.  Analyze inputs including manual notes, URLs, and topics
3. Generate comprehensive, actionable reports in markdown format
4. Follow established templates for consistency and completeness
5. Output files to designated directories with proper naming conventions

Always:
- Ask clarifying questions if inputs are ambiguous
- Use structured templates for all reports
- Include all relevant sections from templates
- Format outputs in clean, readable markdown
- Save files with descriptive, kebab-case names
- Reference previous reports when generating dependent documents

Output Directories:
- Research summaries: ./agent-output/research/
- PRDs: ./agent-output/prd/
- User stories: ./agent-output/user-stories/
```

**Available Tools:**
- Brave Search API (MCP)
- File system access (read/write to output directories)

#### Step 3: Add Report Templates

Copy the three report templates to your project:

1. `prd-template.md`
2. `user-stories-template. md`
3. `research-summary-template.md`

Store these in a `./templates/` directory for reference.

```bash
mkdir -p templates
# Copy template files to ./templates/
```

---

## Commands & Usage

### Command 1: Generate Research Summary

**Command:**
```
/generate-research-summary [topic]
```

**Description:** Conducts research on a given topic and generates a comprehensive research summary report.

**Input Requirements:**
- **Topic**: Clear topic or research question
- **Optional**: Manual notes, observations, URLs to analyze
- **Optional**: Specific research objectives or questions

**Example Usage:**

```
/generate-research-summary User Authentication Best Practices

Additional context:
- Focus on passwordless authentication
- Target enterprise users
- Consider security and UX balance
```

**Output:**
- File: `./agent-output/research/user-authentication-best-practices-research-summary.md`
- Format: Markdown following research-summary-template.md

**What the Agent Does:**
1. Analyzes the topic and any provided context
2. Uses Brave Search API to gather information
3. Analyzes provided URLs and notes
4. Synthesizes findings into structured report
5. Identifies key insights, pain points, and opportunities
6. Provides actionable recommendations

---

### Command 2: Generate Product Requirements Document (PRD)

**Command:**
```
/generate-prd [feature/product name]
```

**Description:** Creates a comprehensive Product Requirements Document for a feature or product.

**Input Requirements:**
- **Feature/Product Name**: Clear name for what's being built
- **Optional**: Reference to research summary file
- **Optional**: Business context and constraints
- **Optional**: Known user needs or requirements

**Example Usage:**

```
/generate-prd Passwordless Authentication

Context:
- Based on research in: ./agent-output/research/user-authentication-best-practices-research-summary.md
- Target: Enterprise SaaS users
- Timeline: Q2 2026 release
- Must integrate with existing SSO
```

**Output:**
- File: `./agent-output/prd/passwordless-authentication-prd.md`
- Format: Markdown following prd-template. md

**What the Agent Does:**
1. Reviews referenced research summary (if provided)
2.  Structures comprehensive PRD with all required sections
3. Defines problem statement, goals, and success metrics
4. Specifies functional and non-functional requirements
5.  Identifies dependencies, risks, and constraints
6. Creates feature breakdown and acceptance criteria
7. Outlines release plan and testing requirements

---

### Command 3: Generate User Stories

**Command:**
```
/generate-user-stories [feature name]
```

**Description:** Breaks down a feature into detailed, development-ready user stories with acceptance criteria.

**Input Requirements:**
- **Feature Name**: Name of feature to break down
- **Optional**: Reference to PRD file
- **Optional**: Specific features or epics to focus on
- **Optional**: Priority guidance

**Example Usage:**

```
/generate-user-stories Passwordless Authentication

Context:
- Based on PRD: ./agent-output/prd/passwordless-authentication-prd.md
- Focus on MVP features first
- Sprint capacity: 8-10 story points per sprint
```

**Output:**
- File: `./agent-output/user-stories/passwordless-authentication-user-stories.md`
- Format: Markdown following user-stories-template.md

**What the Agent Does:**
1. Reviews referenced PRD (if provided)
2. Identifies all features and requirements
3. Breaks down into granular user stories
4. Writes stories in standard format (As a... I want... So that...)
5. Defines detailed acceptance criteria (Given/When/Then)
6.  Specifies technical notes and dependencies
7. Estimates story points and prioritizes
8. Creates test cases for each story

---

## Report Templates

### Template Locations

All templates are stored in `./templates/` directory:

```
./templates/
├── research-summary-template.md
├── prd-template.md
└── user-stories-template.md
```

### Template Customization

While templates are standardized, you can customize them for your project needs:

#### To Customize a Template:

1.  **Copy the template**: Make a project-specific version
   ```bash
   cp templates/prd-template.md templates/prd-template-custom. md
   ```

2. **Modify sections**: Add, remove, or adjust sections based on:
   - Project complexity
   - Stakeholder requirements
   - Team processes
   - Industry-specific needs

3. **Update agent instructions**: Inform the agent to use custom template
   ```
   /generate-prd Feature Name --template=custom
   
   Note: Use ./templates/prd-template-custom.md instead of default
   ```

#### Common Customizations:

**For Small Features:**
- Remove: Market Analysis section
- Simplify: Technical Requirements
- Reduce: Release Planning details

**For Large Products:**
- Add: Competitive Analysis deep-dive
- Expand: Technical Architecture section
- Include: Go-to-Market strategy

**For Regulated Industries:**
- Add: Compliance Requirements section
- Expand: Security & Privacy section
- Include: Audit Trail requirements

---

## Best Practices

### 1. Research Phase Best Practices

✅ **DO:**
- Provide clear, focused research topics
- Include specific questions you need answered
- Share relevant URLs and existing research
- Specify target users or market segments
- Define what "success" looks like for the research

❌ **DON'T:**
- Make topics too broad (e.g., "research everything about AI")
- Skip providing context about your product/users
- Ignore existing research or data
- Proceed to PRD without reviewing research findings

**Example Good Research Request:**
```
/generate-research-summary Mobile Payment Solutions for Small Businesses

Focus areas:
- Payment processing speed and reliability
- Integration with existing POS systems
- Pricing models and transaction fees
- Security and compliance requirements
- User onboarding and setup complexity

Target users: Small retail businesses (1-10 employees)
Geography: North America
Competitors to analyze: Square, Stripe Terminal, PayPal Zettle
```

---

### 2. PRD Generation Best Practices

✅ **DO:**
- Reference completed research summary
- Clearly state the problem being solved
- Define measurable success criteria
- Specify MVP vs. future release features
- Include technical constraints and dependencies
- Set realistic timelines

❌ **DON'T:**
- Generate PRD without research foundation
- Leave success metrics vague or unmeasurable
- Skip stakeholder identification
- Ignore technical feasibility
- Create PRDs that are too high-level or too detailed

**Validation Checkpoint:**
Before approving PRD, verify:
- [ ] Problem statement is clear and compelling
- [ ] Success metrics are specific and measurable
- [ ] All functional requirements have acceptance criteria
- [ ] Technical feasibility has been considered
- [ ] Dependencies and risks are documented
- [ ] MVP scope is realistic for timeline

---

### 3. User Story Generation Best Practices

✅ **DO:**
- Reference the approved PRD
- Keep stories small and independently testable
- Write clear acceptance criteria (Given/When/Then)
- Include technical notes for developers
- Estimate story points
- Identify dependencies between stories
- Prioritize using clear criteria (MoSCoW, etc.)

❌ **DON'T:**
- Create stories that are too large (>8 story points)
- Write vague acceptance criteria
- Skip technical considerations
- Ignore dependencies
- Mix multiple features in one story

**Story Quality Checklist:**
- [ ] Story follows "As a...  I want... So that..." format
- [ ] Acceptance criteria are specific and testable
- [ ] Story is small enough to complete in one sprint
- [ ] Dependencies are clearly identified
- [ ] Technical notes provide enough context for developers
- [ ] Story has clear business value
- [ ] Edge cases and error scenarios are covered

---

### 4. Workflow Best Practices

✅ **Iterative Approach:**
```
1. Generate research summary
2. Review findings with stakeholders
3. Update research if gaps identified
4. Generate PRD based on validated research
5. Review PRD with technical team
6. Refine PRD based on feedback
7. Generate user stories from approved PRD
8. Review stories with development team
9. Refine and estimate stories
```

✅ **Version Control:**
- Store all outputs in Git repository
- Use meaningful commit messages
- Tag releases (e.g., `v1.0-prd`, `v2.0-stories`)
- Track changes in document change logs

✅ **Collaboration:**
- Share research summaries with stakeholders early
- Get technical team input before finalizing PRD
- Review user stories with developers before sprint planning
- Collect feedback and iterate

---

### 5. Output Organization

**Naming Convention:**
```
[feature-name]-[document-type]. md

Examples:
- passwordless-authentication-research-summary.md
- passwordless-authentication-prd.md
- passwordless-authentication-user-stories.md
```

**Version Management:**
When updating existing documents:
```
[feature-name]-[document-type]-v[version]. md

Examples:
- passwordless-authentication-prd-v2.md
- passwordless-authentication-user-stories-v3.md
```

**Directory Structure:**
```
agent-output/
├── research/
│   ├── passwordless-authentication-research-summary. md
│   ├── mobile-payments-research-summary.md
│   └── archived/
│       └── old-research-2025.md
├── prd/
│   ├── passwordless-authentication-prd.md
│   ├── passwordless-authentication-prd-v2.md
│   └── mobile-payments-prd.md
└── user-stories/
    ├── passwordless-authentication-user-stories.md
    ├── passwordless-authentication-user-stories-sprint-2.md
    └── mobile-payments-user-stories.md
```

---

## Troubleshooting

### Common Issues & Solutions

#### Issue 1: Agent generates incomplete reports

**Symptoms:**
- Missing sections from template
- Shallow or generic content
- No specific details

**Solutions:**
1.  Provide more detailed input and context
2. Reference specific sections you need completed
3. Ask follow-up questions to fill gaps
4. Regenerate with explicit instructions

**Example Fix:**
```
/generate-prd Passwordless Authentication

Please ensure the following sections are comprehensive:
- Technical Requirements (specify API needs)
- Security Requirements (detailed authentication flows)
- Integration Requirements (list all systems)

Reference: ./agent-output/research/user-authentication-best-practices-research-summary.md
```

---

#### Issue 2: Agent can't find referenced files

**Symptoms:**
- Error messages about missing files
- Agent doesn't incorporate previous research/PRD

**Solutions:**
1. Verify file path is correct (use relative paths)
2. Check file actually exists in specified location
3.  Ensure file has been saved from previous command
4. Copy relevant content directly in prompt if needed

**Example Fix:**
```
/generate-prd Passwordless Authentication

Context from research (since file path isn't working):
[Paste key findings from research summary]
```

---

#### Issue 3: Output file not saved or saved to wrong location

**Symptoms:**
- Can't find generated file
- File saved with wrong name or location

**Solutions:**
1. Verify output directory exists
2. Check agent has write permissions
3. Manually specify output path in command
4. Review agent configuration for correct directory settings

**Example Fix:**
```bash
# Ensure directories exist
mkdir -p agent-output/{research,prd,user-stories}

# Then regenerate
/generate-prd Passwordless Authentication --output=./agent-output/prd/
```

---

#### Issue 4: Generated user stories are too large

**Symptoms:**
- Stories estimated at >8-13 story points
- Stories cover multiple features
- Can't be completed in one sprint

**Solutions:**
1. Request agent to break down into smaller stories
2. Specify desired story point range
3. Focus on one specific aspect of feature

**Example Fix:**
```
/generate-user-stories Passwordless Authentication

Please focus only on the "Email Magic Link" authentication method. 
Break down into stories that are 2-5 story points each.
Each story should be completable in 2-3 days.
```

---

#### Issue 5: Research summary lacks depth

**Symptoms:**
- Surface-level findings
- Missing competitive analysis
- No user insights
- Generic recommendations

**Solutions:**
1.  Provide more specific research questions
2. Share URLs to analyze
3. Include manual research notes
4. Specify what depth of analysis you need

**Example Fix:**
```
/generate-research-summary Passwordless Authentication

Deep-dive areas:
1. Analyze these 5 competitor products: [URLs]
2. Research WebAuthn vs. Magic Links vs. SMS OTP
3. Find security vulnerability reports from last 2 years
4. Identify adoption rates and user sentiment data
5. Review NIST authentication guidelines

Focus on enterprise use cases specifically.
```

---

#### Issue 6: Brave Search API not returning results

**Symptoms:**
- Research summary missing web research
- Only uses provided inputs
- No external data

**Solutions:**
1. Verify Brave Search API (MCP) is configured correctly
2. Check API quota hasn't been exceeded
3. Test API connection separately
4. Provide more URLs/resources directly if API unavailable

**Workaround:**
```
/generate-research-summary Passwordless Authentication

Note: If Brave Search isn't available, please analyze these resources:
- [URL 1]
- [URL 2]
- [Attached research document]

And flag that additional web research is needed.
```

---

### Getting Help

If you encounter issues not covered here:

1. **Check OpenCode Documentation**: https://opencode.ai/docs
2. **Review Agent Logs**: Check for error messages or warnings
3. **Test with Simple Example**: Try generating report for a simple, well-defined feature
4. **Verify Setup**: Ensure all prerequisites and configuration are correct
5. **Iterate**: Start with basic command, add complexity gradually

---

## Appendix

### A. Command Quick Reference

| Command | Purpose | Output Location |
|---------|---------|----------------|
| `/generate-research-summary [topic]` | Research and insights | `./agent-output/research/` |
| `/generate-prd [feature-name]` | Product requirements | `./agent-output/prd/` |
| `/generate-user-stories [feature-name]` | User stories | `./agent-output/user-stories/` |

---

### B. File Naming Conventions

**Pattern:** `[feature-name]-[document-type]-[optional-version].md`

**Examples:**
- `user-authentication-research-summary.md`
- `user-authentication-prd.md`
- `user-authentication-prd-v2.md`
- `user-authentication-user-stories.md`
- `user-authentication-user-stories-sprint-2.md`

**Rules:**
- Use kebab-case (lowercase with hyphens)
- Be descriptive but concise
- Include version number when updating
- Use `. md` extension for markdown files

---

### C. Integration Opportunities (Future)

While currently a standalone workflow, consider these future integrations:

**GitHub Issues:**
- Auto-create issues from user stories
- Link PRD as issue context
- Tag with labels from priority

**Project Management Tools:**
- Export user stories to Jira/Linear/Azure DevOps
- Sync story points and status
- Link to PRD documentation

**Collaboration Tools:**
- Post research summaries to Slack channels
- Notify stakeholders when PRD is ready
- Share user stories with development team

**Analytics Integration:**
- Pull user behavior data into research
- Auto-update metrics in PRD
- Track success criteria post-launch

---

### D. Template Section Reference

#### Research Summary Key Sections:
1. Executive Summary (key findings TL;DR)
2. Research Objectives & Methodology
3. Detailed Findings with Evidence
4. User Pain Points
5. Behavioral Insights
6. Competitive Analysis
7. Recommendations & Next Steps

#### PRD Key Sections:
1. Executive Summary
2. Background & Context
3. Target Users & Personas
4. Product Goals & Objectives
5. Feature Requirements (detailed)
6. User Experience & Flows
7. Technical Requirements
8. Dependencies & Risks
9. Release Plan
10. Success Criteria

#### User Stories Key Sections (per story):
1. Story Statement (As a... I want... So that...)
2. Business Value
3.  Acceptance Criteria (Given/When/Then)
4.  Functional & Non-Functional Requirements
5.  UI/UX Specifications
6. Technical Notes
7.  Dependencies
8. Test Cases
9. Definition of Done

---

### E. Changelog

| Version | Date | Changes | Author |
|---------|------|---------|--------|
| 1.0 | 2025-12-07 | Initial AGENTS. md created | [Your Name] |

---

## Quick Start Guide

**First Time Setup (5 minutes):**

1. Create output directories:
   ```bash
   mkdir -p agent-output/{research,prd,user-stories} templates
   ```

2. Save the three report templates to `./templates/`

3. Configure OpenCode agent with provided system prompt

4.  Verify Brave Search API (MCP) is connected

**Your First Workflow (30 minutes):**

1. **Research** (10 min):
   ```
   /generate-research-summary [Your Topic]
   ```
   Review output in `./agent-output/research/`

2. **PRD** (10 min):
   ```
   /generate-prd [Your Feature]
   
   Reference: ./agent-output/research/[your-research-file].md
   ```
   Review output in `./agent-output/prd/`

3. **User Stories** (10 min):
   ```
   /generate-user-stories [Your Feature]
   
   Reference: ./agent-output/prd/[your-prd-file].md
   ```
   Review output in `./agent-output/user-stories/`

**You're ready to go!** 🚀

---

**Questions or Feedback?**  
Update this document as you refine your workflow and discover best practices.
