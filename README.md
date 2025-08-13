# AutoDoc 🤖

> Automatically update and synchronize documentation when using git

A git hook that automatically updates your README and CLAUDE.md files whenever you commit changes, keeping your documentation always in sync with your codebase.

## Features

- 🔄 Automatic README.md version updates
- 📝 AI-powered changelog generation using Claude
- 🎯 Smart commit message analysis
- ⚡ Zero configuration required
- 🔒 Works seamlessly with Claude CLI permissions

## Installation

Clone and run the install script:

```bash
git clone https://github.com/yourusername/AutoDoc.git
cd AutoDoc
./install.sh
```

The installer will:
1. Set up a global git template directory
2. Configure the post-commit hook
3. Make it available for all your git repositories

## Usage

Simply commit as normal - AutoDoc works automatically:

```bash
git add .
git commit -m "feat: add new API endpoint"
# AutoDoc updates README.md automatically
```

### Bypass AutoDoc

To skip documentation updates for a specific commit:

```bash
git commit -m "wip: temporary changes" --no-verify
```

## How It Works

1. **Post-commit Hook**: Triggers after every commit
2. **Claude Integration**: Uses Claude AI to understand changes
3. **Smart Updates**: Updates only version and changelog sections
4. **Automatic Staging**: Adds the updated README.md to git

## Version History

### v1.1.1
- fix: restore changelog and disable CLAUDE.md updates

### v1.1.0
- feat: completely rebuild AutoDoc with timeout handling and optimization
- fix: handle Claude CLI permission restrictions gracefully
- perf: add 30-second timeout for all operations
- perf: optimize file reading and writing
- refactor: cleaner error handling and output

### v1.0.1
- fix: improve Claude prompts to output only file content
- perf: reduce token usage for efficient updates
- docs: clarify documentation update behavior

### v1.0.0
- Initial release with automatic README updates
- Claude AI integration for intelligent documentation
- Global git hook installation

## Troubleshooting

### Hook Not Running
```bash
# Check if hook is installed
ls -la .git/hooks/post-commit

# Reinstall if needed
cd /path/to/AutoDoc
./install.sh
```

### Permission Issues
The hook handles Claude CLI permissions automatically. If you see permission notices, they're informational only.

### Debugging
Enable debug output:
```bash
DEBUG=1 git commit -m "test commit"
```

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

MIT License - see LICENSE file for details