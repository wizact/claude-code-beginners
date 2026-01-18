# Claude Code Walkthrough for Product Owners

## What is Claude Code?

Claude Code is an AI coding assistant that works directly in your terminal. While traditionally used by engineers, **we're seeing more and more non-developers using Claude Code (and similar tools like Cursor) to automate their daily tasks**. Product owners, designers, analysts, and other roles are using these tools to:

- Automate repetitive data processing tasks
- Generate reports and documentation
- Create prototypes and mockups
- Build simple tools and scripts without engineering help
- Transform and analyze data (CSV, JSON, etc.)

**The barrier to automation is lower than ever** - you don't need to be a developer to benefit from AI-assisted coding.

## General Walkthrough

### Opening Claude Code & UI Overview

When you launch Claude Code, you'll see a clean terminal-based interface with several key elements:

**IDE Integration and File Context**
- Claude Code integrates with your code editor (VS Code, Cursor, etc.)
- When you open a file in your IDE, Claude automatically sees it and can reference it
- This means Claude has context about what you're currently working on without you having to explain

**Model Prompt**
- At the top, you'll see which AI model is powering your session (e.g., "Sonnet 4.5")
- Different models have different capabilities and speeds
- Think of this like choosing between different levels of expertise

**Choosing the Right Model:**
- **Haiku**: Fast and cost-effective for simple, straightforward tasks
  - Quick file edits, simple questions, routine tasks
  - Best when speed matters more than complexity
- **Sonnet**: Balanced model for most day-to-day work (default)
  - Feature development, bug fixes, code reviews
  - Good balance of capability and speed
- **Opus**: Most capable for complex, nuanced tasks
  - Architectural decisions, complex refactoring
  - When quality and thoroughness are critical
  - Use sparingly due to higher cost

**Permissions and User Questions**
- Claude will ask permission before running potentially disruptive commands
- Throughout work, Claude may ask clarifying questions to ensure it understands your requirements
- This collaborative approach prevents mistakes and ensures alignment

---

## Modes

Claude Code operates in different modes depending on the type of work being done:

### Edit Mode
- The default working mode
- Claude reads, writes, and modifies code files
- Shows you exactly what changes are being made
- Like watching someone type in real-time

### Accept Edit Mode
- After Claude proposes changes, you review them
- You can accept, reject, or modify suggested edits
- Ensures you maintain control over your codebase
- Nothing happens without your approval

### Plan Mode
- Used for large or complex tasks
- Claude creates a detailed implementation plan before writing any code
- Shows you the approach, files to be modified, and step-by-step breakdown
- You review and approve the plan before work begins
- Prevents wasted effort on wrong approaches

**When Plan Mode is Used:**
- Multi-file changes
- New features with architectural decisions
- Complex refactoring
- Anything where the approach isn't immediately obvious

---

## Common Commands & Shortcuts

### Shift+? - Cheatsheet
- Opens a quick reference guide
- Shows all available keyboard shortcuts
- Your go-to when you forget a command

### @ - File/Folder Reference
- Use `@filename` to explicitly reference a file
- Example: "Explain how @auth.py works"
- Ensures Claude looks at the exact file you care about
- Can also reference entire folders: `@src/components/`

### / - Command Panel
- Type `/` to see all available commands
- Includes both built-in commands and custom ones your team created
- Like a menu of shortcuts for common tasks

**Common Built-in Commands:**
- `/help` - Get assistance
- `/clear` - Start fresh conversation
- `/context` - See what Claude currently remembers
- `/compact` - Compress conversation history
- `/tasks` - View running background tasks
- `/agents` - Manage AI agents
- `/skills` - View available skills
- `/plugins` - Manage MCP integrations

---

## Concept: Context Window

### What is the Context Window?

Think of Claude's context window as its "working memory":
- It's the amount of information Claude can actively think about at once
- Measured in "tokens" (roughly 1 token ≈ 4 characters)
- Current models have large context windows (hundreds of thousands of tokens)
- But even with large windows, there are limits

**Real-World Analogy:**
- Like your desk space when working
- You can only have so many documents spread out before it gets cluttered
- At some point, you need to file things away and clear space

### Issues with Context Limits

**"Forgot the Middle" Problem:**
- When conversations get very long, Claude might lose track of details from the middle
- Similar to how humans struggle to remember everything in a long meeting
- Earlier and later parts are remembered better than the middle

**Hallucination Risk:**
- When context is overloaded, Claude might "fill in gaps" with incorrect information
- Like when someone recalls a meeting detail incorrectly because they were juggling too much

### Managing Context

**`/context` Command:**
- Shows you what Claude currently has in its working memory
- See files, conversations, and information Claude is tracking
- Helps you understand what Claude "knows" right now

**`/compact` or `#` for Compression:**
- Summarizes long conversations to free up space
- Keeps important details, removes redundant information
- Like taking meeting notes instead of keeping a full transcript
- `#` references allow you to save and retrieve these summaries later

**`/clear` Command:**
- Starts a completely fresh conversation
- Wipes Claude's memory of the current session
- Use when switching to a completely different task
- Like closing all your browser tabs and starting clean

**Best Practices:**
- Use `/compact` periodically on long sessions
- Use `/clear` when changing topics completely
- Reference specific files with `@` instead of pasting large code blocks
- Break very large tasks into smaller, focused sessions

---

## Memory: Constitutional Instructions

Claude Code has two types of memory that guide its behavior:

### CLAUDE.md File - Long-term Memory

**What is it?**
- A configuration file that lives in your codebase (`.claude/CLAUDE.md`)
- Contains instructions that Claude follows for your project
- Like a "team playbook" that Claude reads every time

**What goes in CLAUDE.md?**
- Team coding conventions and standards
- Preferred tools and libraries
- Git workflow preferences
- Testing requirements
- Project-specific context

**Example:**
```markdown
# Our Project Guidelines

## Git Workflow
- Always create feature branches
- Sign all commits with `-S`
- Add "AI-Assisted" label to PRs

## Testing
- Write tests before implementing features
- Run tests locally before committing

## Code Style
- Use TypeScript strict mode
- Prefer async/await over promises
```

**Benefits:**
- Claude automatically follows your team's rules
- No need to repeat instructions in every session
- Ensures consistency across all AI-assisted work
- New team members (and AI) follow same standards

**Global vs. Project CLAUDE.md:**
- Global (`~/.claude/CLAUDE.md`): Applies to all projects
- Project (`.claude/CLAUDE.md`): Specific to one repository
- Project-specific settings override global ones

#### Why Product Owners Should Care About CLAUDE.md

As a product owner, you can use CLAUDE.md to encode product knowledge directly into the development workflow:

**Product Terminology:**
- Define domain-specific terms and concepts
- Ensure Claude uses correct business vocabulary
- Example: "A 'listing' refers to an item for sale, not a search result"

**User Personas:**
- Document key user types and their needs
- Help Claude understand context when implementing features
- Example: "Power sellers need bulk upload tools; casual sellers prefer simple forms"

**Critical Verification Areas:**
- Specify which parts of the system require extra testing
- Define quality gates for sensitive features
- Example: "Payment processing must include integration tests and manual QA sign-off"

**Business Rules:**
- Document rules that must be enforced in code
- Prevent technical implementations that violate business logic
- Example: "Users under 18 cannot create listings in certain categories"

**Example Product Owner Section in CLAUDE.md:**
```markdown
## Product Context

### User Types
- **Buyers**: Browse listings, make purchases, leave reviews
- **Sellers**: Create listings, manage inventory, respond to questions
- **Admins**: Moderate content, handle disputes, manage platform

### Critical Business Rules
- All financial transactions must be logged for audit
- Sellers must verify identity before listing high-value items
- Refunds require approval workflow (no automatic refunds)

### Testing Requirements
- Payment features: Integration tests + manual QA
- Search functionality: Performance tests (< 500ms response)
- User data changes: Must include audit trail
```

This ensures engineers (and Claude) understand the "why" behind features, not just the "what."

### # - Session-based Memory

**What is it?**
- A way to save and reference conversation summaries
- Creates "bookmarks" in your work that you can return to
- Like taking notes during a long project

**How it works:**
- Long conversations get automatically summarized
- You can reference these summaries with `#` symbol
- Helps maintain context across multiple work sessions

**Use Cases:**
- Continue work from where you left off yesterday
- Share context between team members
- Reference decisions made in previous sessions

**Example:**
```
"Continue the authentication feature we discussed #yesterday"
```

**Difference from CLAUDE.md:**
- `#` is temporary (session-based)
- CLAUDE.md is permanent (version controlled)
- `#` is for workflow continuity
- CLAUDE.md is for team standards

---

## Commands: `/`

Commands are quick shortcuts for controlling Claude Code itself. They're different from skills and agents.

### What Are Commands?

**Characteristics:**
- Start with `/` prefix
- Control Claude Code's behavior
- Usually immediate, simple actions
- Can be built-in or custom

**Difference from Skills:**
- **Commands**: Control the tool itself (like app settings)
- **Skills**: Perform work on your code (like features)

**Difference from Agents:**
- **Commands**: Instant, simple actions
- **Agents**: Complex, multi-step autonomous work

### Example Custom Command: `/wizact-dev-essentials:commit-message`

**Purpose:** Generates well-formatted commit messages following team standards

**What it does:**
1. Analyzes staged changes
2. Understands what changed and why
3. Generates commit message in team's format
4. Follows conventional commits standard

**Usage:**
```
User: "/wizact-dev-essentials:commit-message"

Command output:
feat(auth): add JWT token refresh mechanism

- Implemented token rotation for enhanced security
- Added refresh token endpoint
- Updated authentication middleware
- Includes unit tests for refresh flow

Breaking change: Old tokens expire after 15 minutes
```

**Why create custom commands?**
- Encode team-specific workflows
- Ensure consistency (commit message format, PR templates, etc.)
- Save time on repetitive tasks
- Share best practices across team

### Common Built-in Commands Review

- `/help` - Get assistance with Claude Code
- `/clear` - Start fresh conversation
- `/context` - See current context window contents
- `/compact` - Compress conversation history
- `/rewind` - Undo recent changes and go back to a previous state
- `/resume` - Continue from a previous session or agent task
- `/tasks` - View background tasks
- `/agents` - List and manage agents
- `/skills` - Browse available skills
- `/plugins` - Manage MCP servers

**Time Travel Commands:**
- `/rewind`: Useful when Claude made changes you want to undo - like "undo" but for entire conversations
- `/resume`: Pick up where you left off from a previous session or continue a paused agent's work

---

## Agents: `/agents`

Agents are specialized AI workers that handle specific tasks autonomously. Think of them as team members with specific expertise.

### Built-in Agents

#### General Agent
**Purpose:** Handles complex, multi-step tasks autonomously

**When used:**
- Research questions requiring multiple searches
- Tasks spanning multiple files
- Any work that requires exploration then action

**Demo Example: "Vibecode something"**
```
User: "Create a simple landing page for our product with a hero section,
features grid, and contact form. Make it responsive."

Claude launches General Agent:
1. Agent explores existing styles/components
2. Creates HTML structure
3. Adds CSS styling
4. Implements responsive breakpoints
5. Adds basic form validation
6. Returns completed work
```

**Key Feature:** Works independently and reports back when done

#### Explore Agent (Read-Only)
**Purpose:** Deep architectural understanding without making changes

**When used:**
- "How does authentication work in our app?"
- "Where is payment processing handled?"
- "What's the data flow for user registration?"

**Characteristics:**
- Read-only access (cannot modify files)
- Thorough codebase exploration
- Answers architectural questions
- Maps relationships between components

**Demo Example: "Explore business logic"**
```
User: "How does our order processing system work?
Walk me through the flow from cart to fulfillment."

Explore Agent:
1. Finds order-related files
2. Traces code flow through services
3. Identifies database interactions
4. Maps integration points
5. Explains the complete workflow
```

**Why read-only?**
- Prevents accidental changes during research
- Faster (doesn't need to consider modifications)
- Safer for exploratory questions

#### Plan Agent
**Purpose:** Breaks large work into smaller, iterative phases

**When used:**
- Major feature development
- Complex refactoring
- Multi-phase projects
- Anything where approach needs approval first

**How it works:**
1. Analyzes requirements
2. Explores existing codebase
3. Creates detailed implementation plan
4. Presents plan for your approval
5. Waits for go-ahead before proceeding

**Example Plan Output:**
```
# Implementation Plan: Add User Dashboard

## Phase 1: Backend API
- Create dashboard data endpoint
- Add authentication middleware
- Write unit tests

## Phase 2: Frontend Component
- Build dashboard React component
- Integrate with API
- Add loading states

## Phase 3: Integration
- Connect to navigation
- Add route configuration
- End-to-end testing

## Questions:
- Should we cache dashboard data?
- Real-time updates or refresh-based?
```

### Custom Agents

Your team can create specialized agents for recurring workflows.

#### Benefits

**Context Window Management:**
- Agents work in isolated context
- Don't pollute your main conversation
- Can run in background while you continue other work

**Specialized Tasks:**
- Built for specific, repeated workflows
- Encode team knowledge and best practices
- Ensure consistency in how tasks are done

**Parallel Execution:**
- Multiple agents can work simultaneously
- Speed up complex workflows
- Like having multiple team members working at once

#### Example: SpecBuilder Agent

**Purpose:** Creates complete feature specifications from requirements

**What it does:**
1. Takes GitHub issue or user description
2. Generates `requirements.md` (user stories, acceptance criteria)
3. Creates `design.md` (technical architecture)
4. Produces `tasks.md` (breakdown of work items)

**Why this is valuable:**
- Ensures requirements are thorough before development
- Creates documentation automatically
- Links technical tasks back to requirements
- Standardizes how features are specified

**Usage:**
```
User: "Create spec for issue #234"

SpecBuilder Agent:
- Reads GitHub issue
- Analyzes similar features in codebase
- Generates complete specification
- Cross-references requirements with tasks
- Returns structured documentation
```

---

## Skills: `/skills`

Skills are pre-built, specialized workflows for specific tasks. Think of them as expert tools in your toolbox.

### Why Skills Matter

**Benefits:**
- Faster than explaining the same task repeatedly
- Encode best practices for common operations
- Provide consistent results
- Can be shared across teams

**Difference from Agents:**
- Skills are focused, single-purpose tools
- Agents are autonomous workers for complex tasks
- Skills execute immediately
- Agents may explore and plan first

### Example Skill: Frontend Development

**Purpose:** Rapidly scaffold and build modern web interfaces with best practices

**What makes it special:**
- Generates complete component structures
- Follows modern frontend patterns (React, Vue, etc.)
- Includes responsive design and accessibility
- Applies consistent styling and state management

**Demo Example:**
```
User: "Create a product card component with image, title, price,
and add-to-cart button. Make it responsive and accessible."

Frontend skill executes:
- Creates component file with proper structure
- Adds responsive CSS/styling
- Implements accessibility features (ARIA labels, keyboard navigation)
- Includes state management for cart interaction
- Adds proper image optimization
- Follows team's design system

Result:
ProductCard.jsx created with:
- Semantic HTML structure
- Mobile-first responsive design
- Screen reader support
- Keyboard accessible interactions
- Optimized image loading
- Add to cart functionality with state management
```

**Why this is powerful:**
- Saves hours of boilerplate setup
- Ensures accessibility from the start
- Follows best practices automatically
- Maintains consistency across components
- Reduces time from design to implementation

### Other Common Skills

- **Code review**: Automated code quality checks
- **Test generation**: Create test cases from code
- **Documentation**: Generate docs from code comments
- **Dependency analysis**: Find what uses what

---

## MCP (Model Context Protocol)

### What is MCP?

Think of MCP as a way to connect Claude Code to external services and tools - like APIs that give Claude new abilities.

**Simple Analogy:**
- Claude Code is like your phone
- MCP servers are like apps you install
- Each "app" gives Claude access to different tools and data

### Finding MCP Servers

**`/plugins` Command:**
- Lists installed MCP integrations
- Shows available plugins to add
- Manages connections and permissions

**MCP Registry: https://mcpservers.org/**
- Public directory of available integrations
- Community-built and official servers
- Installation instructions for each

### Common MCP Integrations

#### Figma
- Read design files and specs
- Extract colors, spacing, component structure
- Generate code from designs
- Keep implementation in sync with design system

**Use case:**
```
"Look at the Figma file for the dashboard and implement
the component structure in React"
```

#### Jira
- Read issues and project data
- Create/update tickets
- Link code changes to Jira issues
- Track work status

**Use case:**
```
"Create implementation tasks in Jira for the user
authentication epic"
```

#### Snowflake
- Query data warehouse
- Analyze data patterns
- Generate reports
- Validate data transformations

**Use case:**
```
"Query our Snowflake warehouse to find the most
common error codes in the last 30 days"
```

### Demo: Create Trade Me Prototype

**Scenario:** Build a prototype of Trade Me (online marketplace)

**Using Chrome DevTools MCP:**
1. Navigate to trademe.co.nz
2. Take screenshots of key pages
3. Analyze layout and components
4. Extract color scheme and typography
5. Identify interaction patterns

**Implementation:**
```
User: "Create a prototype of Trade Me website.
Focus on the listing page and search functionality."

Claude (using Chrome DevTools MCP):
1. Opens browser and navigates to Trade Me
2. Takes screenshots of listing page
3. Analyzes HTML structure and styles
4. Identifies key components (search bar, filters, listing cards)
5. Extracts color palette and fonts

Claude (creating prototype):
6. Generates HTML structure matching layout
7. Creates CSS with extracted styles
8. Implements responsive grid for listings
9. Adds search bar and filter UI
10. Creates sample listing cards with similar design

Result: Functional prototype matching Trade Me's look and feel
```

**Why MCP is Powerful:**
- Access real-world data and designs
- Integrate with tools your team already uses
- Extend Claude beyond just code editing
- Create workflows spanning multiple services

**Building Custom MCP Servers:**
To build AI-powered customer features (like conversational search for Trade Me), you need to expose your platform's functionality through custom MCP servers. For example, a "Trade Me Search MCP" would allow AI agents to search listings, filter results, and retrieve product details - enabling natural language interactions with your marketplace.

---

## Putting It All Together

### Typical Workflow Example

```
1. User: "We need to add a payment processing feature"

2. Claude enters Plan Mode
   - Explores existing payment-related code
   - Identifies integration points
   - Creates implementation plan
   - Asks clarifying questions

3. User approves plan

4. Claude launches agents in parallel:
   - General agent: Implements backend API
   - Another agent: Creates frontend components
   - Test agent: Writes test coverage

5. During work, Claude uses:
   - Skills (ripgrep) to find similar code patterns
   - MCP (Jira) to create tracking tasks
   - @ references to show specific files
   - CLAUDE.md to follow team standards

6. User reviews changes in Accept Edit Mode

7. Claude runs custom command:
   - /wizact-dev-essentials:commit-message
   - Creates well-formatted commit

8. Work is committed and PR is created
```

### Best Practices for Product Owners

**Start Conversations Well:**
- Reference specific files with `@`
- Link to designs (Figma URLs if MCP connected)
- Include acceptance criteria
- Mention related issues/tickets

**Manage Context:**
- Use `/compact` on long sessions
- Use `/clear` when switching topics
- Save important decisions in CLAUDE.md
- Use `#` to reference previous work

**Leverage Specialization:**
- Use Explore agent for "how does X work?" questions
- Use Plan agent for large features
- Use custom agents for repeated workflows
- Use skills for common search/analysis tasks

**Review Effectively:**
- Understand plan mode outputs (ask questions!)
- Review changed files in Accept Edit Mode
- Verify tests are passing
- Check that standards (CLAUDE.md) were followed

**Build Team Knowledge:**
- Document common workflows as custom skills
- Update CLAUDE.md as standards evolve
- Share useful MCP integrations
- Create custom agents for repeated tasks

---

## Glossary

**Agent:** Autonomous AI worker handling complex, multi-step tasks

**Command:** Quick shortcut to control Claude Code (starts with `/`)

**Context Window:** Claude's working memory - how much information it can actively process

**CLAUDE.md:** Configuration file with team standards and instructions

**MCP (Model Context Protocol):** Integration system connecting Claude to external tools

**Plan Mode:** Mode where Claude creates detailed plans before implementing

**Skill:** Pre-built workflow for specific, focused tasks

**Subagent:** Specialized type of agent (explore, plan, general, etc.)

**Token:** Unit of measurement for context (roughly 4 characters)

---

## Questions & Exploration

As you explore Claude Code, consider:

1. What repetitive tasks could become custom skills?
2. What team standards should go in CLAUDE.md?
3. Which MCP integrations would benefit your workflow?
4. What custom agents would help your team?
5. How can you structure work to leverage Plan Mode effectively?

**Remember:** Claude Code is a tool to augment your team, not replace human judgment. Use it to speed up implementation, but maintain oversight and decision-making authority.

---

## Homework

To learn more and practice with Claude Code, explore this interactive guide:

**[Claude Code for Product Managers](https://ccforpms.com/)**

This resource provides hands-on exercises and real-world scenarios tailored for product owners and non-technical users.
