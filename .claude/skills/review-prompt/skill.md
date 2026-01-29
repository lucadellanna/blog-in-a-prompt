# Review PROMPT.md

Review the Blog in a Prompt PROMPT.md file for correctness, consistency, and executability.

## Trigger phrases
- "review the prompt"
- "check the prompt"
- "audit PROMPT.md"
- "validate the prompt"

## What this skill does

Performs a comprehensive review of PROMPT.md to ensure it will execute correctly when read by Claude Code.

### 1. Structural consistency
- Verify all section cross-references are correct (e.g., "see Section 7.0" actually points to the right content)
- Check that numbered steps are sequential and none are missing
- Verify all internal links between sections are valid

### 2. Technical accuracy
- Verify CLI commands are correct and use valid flags
- Check that version numbers are consistent (e.g., Next.js version in create-next-app matches package.json pinning)
- Verify file paths mentioned match the required project structure
- Check that code examples are syntactically correct

### 3. Logical flow
- Verify the order of operations makes sense (e.g., can't reference files before they're created)
- Check for circular dependencies or impossible sequences
- Ensure prerequisites are mentioned before they're needed

### 4. Contradiction detection
- Look for conflicting instructions (e.g., "do X" in one place, "don't do X" in another)
- Check that timing instructions are consistent (e.g., when to create skills)
- Verify allowlists/blocklists don't contradict each other

### 5. Completeness
- Verify all mentioned files have creation instructions
- Check that all user questions have handling instructions for responses
- Ensure error scenarios have recovery paths

### 6. Executability
- Identify any ambiguous instructions that could be interpreted multiple ways
- Flag instructions that assume context not provided
- Check for missing details that would block execution

## Output format

Provide findings in these categories:
- **Critical**: Issues that would cause the prompt to fail
- **Important**: Issues that could cause problems or confusion
- **Suggestions**: Improvements that aren't strictly necessary

For each finding, include:
1. The specific location (section number and relevant text)
2. What the issue is
3. Suggested fix

## Example output

```
## Critical Issues

### 1. Version mismatch (Section 2.5 vs 7.3)
- **Location**: Section 2.5 pins `next: "^14.0.0"`, Section 7.3 uses `create-next-app@15`
- **Issue**: These versions conflict - the installed version won't match the pinned version
- **Fix**: Align both to the same major version

## Important Issues

### 1. Missing step reference (Section 8.1)
- **Location**: "Use the repository name gathered in Section 7.0"
- **Issue**: Section 7.0 asks about repo name, but Section 8.1 also has AskUserQuestion for repo name
- **Fix**: Remove duplicate question from Section 8.1

## Suggestions

### 1. Consider adding timeout handling
- **Location**: Section 9.3 deployment validation
- **Issue**: No guidance on what to do if Vercel takes longer than 30 seconds
- **Fix**: Add polling logic or extended timeout instructions
```
