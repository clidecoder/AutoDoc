# AutoDoc 📝

> Intelligent documentation automation for every push

AutoDoc is a Git pre-push hook that automatically maintains your project documentation by analyzing commits and leveraging Claude AI to keep your CLAUDE.md, CHANGELOG.md, version numbers, and release notes perfectly synchronized with your codebase.

## ✨ Features

- **🤖 AI-Powered Updates** - Uses Claude to intelligently update documentation based on commit context
- **📋 Automatic Changelog** - Generates properly formatted changelog entries following [Keep a Changelog](https://keepachangelog.com/) standards
- **🔢 Semantic Versioning** - Automatically bumps version numbers based on conventional commits
- **📜 Release Notes** - Generates user-friendly release notes when pushing tags
- **🎯 Conventional Commits** - Parses and categorizes commits (feat, fix, breaking changes, etc.)
- **🌲 Branch Awareness** - Configurable to run only on main/master or all branches
- **🎨 Formatted Output** - Color-coded terminal output for better visibility

## 🚀 Quick Start

### Prerequisites

- [Claude CLI](https://docs.anthropic.com/en/docs/claude-cli) installed and configured
- [Husky](https://typicode.github.io/husky/) for Git hooks
- Node.js project (or any project with version tracking)

### Installation

1. **Install Husky** (if not already installed):
   ```bash
   npm install --save-dev husky
   npx husky install
   ```

2. **Add AutoDoc**:
   ```bash
   npx husky add .husky/pre-push
   ```

3. **Copy the AutoDoc script** into `.husky/pre-push`

4. **Make it executable**:
   ```bash
   chmod +x .husky/pre-push
   ```

## ⚙️ Configuration

Edit the configuration section at the top of the script:

```bash
# Configuration
MAIN_BRANCH="main"              # or "master"
ENABLE_ON_ALL_BRANCHES=false    # Set to true to run on all branches
```

## 📖 How It Works

When you run `git push`, AutoDoc:

1. **Analyzes Commits** - Examines all commits in the push range
2. **Categorizes Changes** - Groups commits by type (features, fixes, etc.)
3. **Determines Version Bump** - Calculates appropriate semantic version increase
4. **Updates Documentation** - Uses Claude to intelligently update:
   - `CLAUDE.md` - Project-specific documentation
   - `CHANGELOG.md` - Formatted changelog entries
   - Version in `package.json` or `VERSION` file
   - `RELEASE_NOTES.md` - When pushing tags
5. **Commits Changes** - Automatically commits documentation updates
6. **Continues Push** - Proceeds with the original push including the new commit

## 🏗️ Commit Convention

AutoDoc works best with [Conventional Commits](https://www.conventionalcommits.org/):

- `feat:` New features → Minor version bump
- `fix:` Bug fixes → Patch version bump
- `feat!:` or `BREAKING CHANGE:` → Major version bump
- `docs:`, `style:`, `refactor:`, `test:`, `chore:` → Organized in changelog

### Examples:
```bash
git commit -m "feat: add user authentication"
git commit -m "fix: resolve memory leak in data processor"
git commit -m "feat!: change API response format"
```

## 📁 Files Managed

### CLAUDE.md
Your project's main documentation file. AutoDoc updates this with relevant changes, removing outdated information and adding new details.

### CHANGELOG.md
Follows the [Keep a Changelog](https://keepachangelog.com/) format:
- Groups changes by type (Added, Changed, Fixed, etc.)
- Includes version numbers and dates
- Links to version comparisons

### VERSION or package.json
Automatically updates version numbers following semantic versioning.

### RELEASE_NOTES.md
Generated when pushing tags, contains user-friendly summaries of changes.

## 🎯 Best Practices

1. **Use Conventional Commits** - Helps AutoDoc categorize changes accurately
2. **Write Descriptive Commits** - Better commit messages = better documentation
3. **Review Before Pushing** - Check the generated documentation in the commit
4. **Configure for Your Workflow** - Adjust branch settings based on your team's needs

## 🛠️ Customization

### Skip AutoDoc for a Push
```bash
git push --no-verify
```

### Run Only on Certain Files
Add file filtering to the script:
```bash
# Only run if source files changed
if echo "$changes" | grep -q "src/"; then
    # Run AutoDoc
fi
```

### Custom Documentation Format
Modify the Claude prompts in the script to match your documentation style.

## 🤝 Contributing

AutoDoc is designed to be hackable! Feel free to:
- Customize the Claude prompts for your project's needs
- Add support for additional documentation files
- Integrate with your CI/CD pipeline
- Share your improvements with the community

## 📊 Example Output

```
🤖 Analyzing commits for documentation update...
✓ CLAUDE.md updated
✓ CHANGELOG.md updated
✓ Version bumped to 2.1.0
✅ Documentation updated and committed
Ready to push with updated documentation!
```

## 🐛 Troubleshooting

### AutoDoc not running?
- Ensure the script is executable: `chmod +x .husky/pre-push`
- Check if Husky is properly installed: `npx husky install`

### Claude CLI errors?
- Verify Claude CLI is installed: `which claude`
- Check authentication: `claude auth status`

### Documentation not updating?
- AutoDoc only commits if files actually change
- Check the branch configuration matches your workflow
- Review the terminal output for any errors

## 📄 License

This tool is provided as-is for use in your projects. Customize and adapt as needed!

---

Built with ❤️ to keep your documentation as fresh as your code
