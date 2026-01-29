# Review README.md

Review the Blog in a Prompt README.md for factual accuracy and clarity for non-technical users.

## Trigger phrases
- "review the readme"
- "check the readme"
- "audit README.md"
- "is the readme clear"

## What this skill does

Performs a comprehensive review of README.md from two perspectives: technical accuracy and non-technical user clarity.

### Part 1: Factual accuracy

#### 1.1 URL verification
- Check that all URLs are valid and point to the correct resources
- Verify GitHub raw URLs match the actual repo name
- Check that external links (nodejs.org, github.com, etc.) are correct

#### 1.2 Command accuracy
- Verify all shell commands are syntactically correct
- Check that Mac commands work in Terminal/zsh
- Check that Windows commands work in PowerShell
- Verify installation commands use current/correct package names

#### 1.3 Consistency with PROMPT.md
- Verify claims about what the blog includes/excludes match PROMPT.md
- Check that the publishing workflow description matches the actual flow
- Ensure any technical details align with the implementation

#### 1.4 Version/tool accuracy
- Verify referenced tools exist and work as described
- Check that installation methods are current (e.g., Claude Code install command)
- Flag any outdated information

### Part 2: Non-technical user clarity

#### 2.1 Jargon detection
- Flag technical terms that aren't explained (git, repo, CLI, terminal, etc.)
- Identify acronyms without definitions
- Check that explanations use simple analogies where helpful

#### 2.2 Assumption audit
- Identify places that assume prior knowledge
- Flag steps that might be confusing to someone who's never used Terminal/PowerShell
- Check for missing context that a beginner would need

#### 2.3 Instruction clarity
- Verify each step is actionable and specific
- Check that users know exactly what to type/click
- Ensure success criteria are clear (how do they know it worked?)

#### 2.4 Error handling guidance
- Check if common failure scenarios are addressed
- Verify troubleshooting advice is accessible to non-technical users
- Ensure error recovery doesn't require technical knowledge

#### 2.5 Visual/formatting clarity
- Check that code blocks are clearly marked
- Verify the distinction between "type this" vs "you'll see this"
- Ensure important warnings stand out

### Part 3: User journey analysis

#### 3.1 Complete path verification
- Walk through the entire setup as a new user would
- Identify any gaps or missing steps
- Check that transitions between sections are smooth

#### 3.2 Cognitive load assessment
- Flag sections that might overwhelm a beginner
- Identify opportunities to break complex steps into smaller ones
- Check that the most important information comes first

## Output format

Provide findings in these categories:

### Factual Issues
Issues where the README states something incorrect or outdated.

### Clarity Issues
Places where a non-technical user might get confused or stuck.

### Suggestions
Improvements that would enhance the user experience.

For each finding, include:
1. The specific location (line number or section)
2. The current text (if relevant)
3. What the issue is
4. Suggested improvement

## Example output

```
## Factual Issues

### 1. Outdated installation command
- **Location**: Line 51, Windows Claude Code installation
- **Current**: `irm https://claude.ai/install.ps1 | iex`
- **Issue**: This URL may have changed; verify it's current
- **Suggested**: Confirm URL at claude.ai/download or update to current method

## Clarity Issues

### 1. "Terminal" not explained for beginners
- **Location**: Line 35, "Open Terminal"
- **Current**: "Open Terminal (Applications folder > Utilities folder > Terminal)"
- **Issue**: Users may not know what Terminal is or why they need it
- **Suggested**: Add brief explanation: "Terminal is an app that lets you type commands to your computer. Think of it as a way to talk directly to your Mac."

### 2. Unclear success indicator
- **Location**: Line 46, after `brew install node`
- **Issue**: User doesn't know what success looks like
- **Suggested**: Add: "When it's done, you'll see your prompt again (the line where you type). You can verify it worked by typing `node --version` - you should see a number like v20.x.x"

## Suggestions

### 1. Add estimated time for each step
- **Location**: Throughout "How to Create Your Blog" section
- **Suggestion**: Help users plan by adding "(~2 minutes)" after each major step
```
