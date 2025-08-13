# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.1.1] - 2025-08-13

### Fixed
- Restore changelog and disable CLAUDE.md updates

## [1.1.1] - 2025-08-13

### Fixed
- Improve Claude prompts to output only file content instead of commentary
- Bypass Claude CLI permissions in git hook environment to enable proper file updates

## [1.1.0] - 2025-08-13

### Added
- Complete rebuild of AutoDoc system with timeout handling and optimization
- 60-second timeouts on Claude CLI calls with 2 retry attempts
- Comprehensive error handling and fallback mechanisms
- Timestamped logging with colored output for better visibility
- Biome linter and formatter integration into pre-push workflow

### Changed
- Dramatically reduced prompt sizes (500 char limit) to prevent token overflow
- Optimized commit parsing to limit to 5 recent commits
- Smart file size limits for context (10-20 lines vs full file contents)
- Batch processing optimizations to reduce total execution time

## [1.0.0] - 2025-08-13

### Added
- Initial AutoDoc system with Git pre-push hook integration
- Husky configuration for automated documentation updates
- Conventional commit parsing and categorization
- Automatic semantic versioning based on commit types
- CLAUDE.md and CHANGELOG.md auto-generation
- Release notes generation for tagged releases
