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
# Normal development flow - AutoDoc runs automatically
git add .
git commit -m "feat: add new feature"
git push  # AutoDoc analyzes commits and updates docs

# Skip AutoDoc for emergency pushes
git push --no-verify

# Check current version
grep version package.json  # or cat VERSION

# View generated documentation
cat CLAUDE.md
cat CHANGELOG.md
cat RELEASE_NOTES.md  # Available after tag pushes
```

### Testing and Validation
```bash
# Test the pre-push hook locally
.husky/pre-push

# Validate documentation updates
git status
git diff CLAUDE.md
git diff CHANGELOG.md
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