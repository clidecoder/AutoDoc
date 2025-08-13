# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Common Development Commands

### Installation and Setup
```bash
# Install Husky for Git hooks
npm install --save-dev husky
npx husky install

# Create pre-push hook
npx husky add .husky/pre-push

# Copy AutoDoc script into .husky/pre-push and make executable
chmod +x .husky/pre-push

# Verify Claude CLI is available
which claude
claude auth status
```

### Git Workflow with AutoDoc
```bash
# ALWAYS use conventional commits for proper AutoDoc functionality
git add .
git commit -m "feat: add new feature"      # Minor version bump
git commit -m "fix: resolve bug"           # Patch version bump  
git commit -m "feat!: breaking change"     # Major version bump
git commit -m "docs: update README"        # Documentation update
git commit -m "chore: update dependencies" # Maintenance task

# Push triggers AutoDoc analysis and documentation updates
git push  # AutoDoc analyzes commits and updates docs

# Skip AutoDoc for emergency pushes only
git push --no-verify

# Check current version
grep version package.json  # or cat VERSION

# View generated documentation
cat CLAUDE.md
cat CHANGELOG.md
cat README.md     # Now auto-generated/updated
cat RELEASE_NOTES.md  # Available after tag pushes
```

### Conventional Commits (Required)
AutoDoc requires conventional commits for proper functionality. Always follow this format:

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

**Commit Types and AutoDoc Behavior**:
- `feat:` → Minor version bump (1.2.0 → 1.3.0), adds to "Added" in changelog
- `fix:` → Patch version bump (1.2.0 → 1.2.1), adds to "Fixed" in changelog  
- `feat!:` or `BREAKING CHANGE:` → Major version bump (1.2.0 → 2.0.0)
- `docs:` → Documentation updates, no version bump
- `style:` → Code formatting, no version bump
- `refactor:` → Code restructuring, no version bump
- `perf:` → Performance improvements, categorized appropriately
- `test:` → Adding tests, no version bump
- `chore:` → Maintenance tasks, no version bump

**Examples**:
```bash
git commit -m "feat: add README auto-generation to pre-push hook"
git commit -m "fix: resolve shell compatibility in AutoDoc script"
git commit -m "feat!: change API structure for documentation updates"
git commit -m "docs: update installation instructions"
git commit -m "chore: update Husky to latest version"
```

### Testing and Validation
```bash
# Test the pre-push hook locally
.husky/pre-push

# Validate documentation updates
git status
git diff CLAUDE.md
git diff CHANGELOG.md
git diff README.md
```

## High-Level Architecture

### AutoDoc System Components

**Pre-Push Hook (`Pre-Push-Hook.sh`)**:
- Husky-based Git hook that runs before `git push`
- Configurable to run on main branch only or all branches
- Parses conventional commits and categorizes changes
- Integrates with Claude AI for intelligent documentation updates

**Documentation Files Managed**:
- `CLAUDE.md` - Project-specific development guidance (this file)
- `README.md` - Auto-generated/updated project documentation with installation, usage, and features
- `CHANGELOG.md` - Formatted changelog following Keep a Changelog standards
- `VERSION` or `package.json` - Semantic version tracking
- `RELEASE_NOTES.md` - Generated for tag pushes

**Conventional Commit Integration**:
- `feat:` → Minor version bump, added to "Features" section
- `fix:` → Patch version bump, added to "Fixes" section  
- `feat!:` or `BREAKING CHANGE:` → Major version bump
- `docs:`, `style:`, `refactor:`, `perf:`, `test:`, `chore:` → Categorized accordingly

### Key Architecture Patterns

**Commit Analysis Flow**:
1. Hook extracts commit range from Git push context
2. Commits parsed and categorized by conventional commit type
3. Claude AI analyzes changes and determines semantic version bump
4. Documentation files updated based on categorized changes
5. Changes automatically committed with descriptive message

**Version Management**:
- Supports both `package.json` version field and standalone `VERSION` file
- Semantic versioning rules applied based on commit types
- Version bumps calculated automatically from commit analysis

**Claude AI Integration**:
- Uses Claude CLI for natural language processing of commits
- Generates contextually appropriate documentation updates
- Maintains consistency with existing documentation style
- Creates user-friendly release notes for tagged releases

**Configuration Options**:
- `MAIN_BRANCH`: Set target branch (default: "main")
- `ENABLE_ON_ALL_BRANCHES`: Control whether hook runs on all branches
- Branch-specific behavior for documentation updates

### File Processing Logic

**Change Detection**:
- Uses `git diff --name-status` to identify modified files
- Only commits documentation if actual changes detected
- Preserves existing content while adding relevant updates

**Commit Categorization**:
- Regex-based parsing of conventional commit prefixes
- Breaking change detection via `!:` suffix or `BREAKING CHANGE:` text
- Fallback category for non-conventional commits

**Output Management**:
- Color-coded terminal output for better visibility
- Temporary files used to ensure atomic updates
- Cleanup of temporary files on successful completion