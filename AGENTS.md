# Product Owner Agent - Automation Workflow

## Overview

This document describes the Product Owner Agent workflow designed to assist Product Owners in conducting research, developing product requirements, and creating implementation-ready user stories. The agent automates the generation of structured reports that serve as guidelines for product implementation.

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
/generate-research-summary --topic [topic] --resources-location [resources-directory](optional)
```

**Description:** Conducts research on a given topic and generates a comprehensive research summary report.

**Input Requirements:**
- **Topic**: Clear topic or research question
- **Optional**: Manual notes, observations, URLs to analyze provided under resources-location

**Example Usage:**

```
/generate-research-summary --topic "User Authentication Best Practices" --resources-location "./resources/authentication"
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
/generate-prd --feature [feature/product name] --research [research-summary-file-path](optional)
```

**Description:** Creates a comprehensive Product Requirements Document for a feature or product.

**Input Requirements:**
- **Feature/Product Name**: Clear name for what's being built
- **Optional**: Reference to research summary file

**Example Usage:**

```
/generate-prd --feature "Passwordless Authentication" --research "./agent-output/research/user-authentication-best-practices-research-summary.md"
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
/generate-user-stories --feature [feature name] --prd [prd-file-path](optional)
```

**Description:** Breaks down a feature into detailed, development-ready user stories with acceptance criteria.

**Input Requirements:**
- **Feature Name**: Name of feature to break down
- **Optional**: Reference to PRD file

**Example Usage:**

```
/generate-user-stories --feature "Passwordless Authentication" --prd "./agent-output/prd/passwordless-authentication-prd.md"
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

---

### 3. User Story Generation Best Practices

✅ **DO:**
- Reference the approved PRD
- Keep stories small and independently testable
- Write clear acceptance criteria (Given/When/Then)
- Include technical notes for developers
- Identify dependencies between stories

❌ **DON'T:**
- Create stories that are too large (>8 story points)
- Write vague acceptance criteria
- Skip technical considerations
- Ignore dependencies
- Mix multiple features in one story

**Story Quality Checklist:**
- [ ] Story follows "As a...  I want... So that..." format
- [ ] Acceptance criteria are specific and testable
- [ ] Dependencies are clearly identified
- [ ] Technical notes provide enough context for developers
- [ ] Story has clear business value
- [ ] Edge cases and error scenarios are covered

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

**Directory Structure:**
```
agent-output/
├── research/
│   ├── passwordless-authentication-research-summary. md
│   ├── mobile-payments-research-summary.md
├── prd/
│   ├── passwordless-authentication-prd.md
│   └── mobile-payments-prd.md
└── user-stories/
    ├── passwordless-authentication-user-stories.md
    └── mobile-payments-user-stories.md
```

---

## Appendix

### A. Command Quick Reference

| Command | Purpose | Output Location |
|---------|---------|----------------|
| `/generate-research-summary --topic [topic] --resources-location [resources-directory](optional)` | Research and insights | `./agent-output/research/` |
| `/generate-prd --feature [feature/product name] --research [research-summary-file-path](optional)` | Product requirements | `./agent-output/prd/` |
| `/generate-user-stories --feature "Passwordless Authentication" --prd "./agent-output/prd/passwordless-authentication-prd.md"` | User stories | `./agent-output/user-stories/` |

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
- Use `. md` extension for markdown files

---

### C. Integration Opportunities (Future)

While currently a standalone workflow, consider these future integrations:

**GitHub Issues:**
- Auto-create issues from user stories
- Link PRD as issue context
- Tag with labels from priority

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
| 1.0.1 | 2025-12-08 | Initial AGENTS. md created | Erodotos Demetriou |
