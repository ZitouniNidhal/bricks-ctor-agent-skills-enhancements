# Contributing to BRICKS CTOR Agent Skills

Thank you for your interest in contributing to the BRICKS CTOR Agent Skills marketplace! This document provides guidelines and instructions for contributing.

## Code of Conduct

This project adheres to a [Code of Conduct](CODE_OF_CONDUCT.md). By participating, you are expected to uphold this code. Please report unacceptable behavior to the project maintainers.

## Getting Started

1. **Fork the repository** on GitHub
2. **Clone your fork** locally:
   ```bash
   git clone https://github.com/YOUR_USERNAME/bricks-ctor-agent-skills.git
   cd bricks-ctor-agent-skills
   ```
3. **Create a feature branch**:
   ```bash
   git checkout -b feature/your-feature-name
   # or for fixes:
   git checkout -b fix/your-fix-name
   ```
4. **Install dependencies**:
   ```bash
   bun install
   ```

## Development Workflow

### Creating a New Skill

1. Create a new directory under `plugins/` with your plugin name:
   ```
   plugins/your-plugin-name/
   ├── skills/
   │   └── your-skill-name/
   │       ├── SKILL.md
   │       ├── index.ts (or .py/.js)
   │       └── test.ts
   ├── manifest.json
   └── README.md
   ```

2. Follow the existing skill structure in the marketplace
3. Ensure skill metadata is complete:
   - Name (kebab-case, 3-50 characters)
   - Description (1-160 characters)
   - Instructions in SKILL.md
   - Version bumping in both manifests

### Validation

All contributions must pass validation:

```bash
bun run validate
```

This checks:
- Manifest syntax and completeness
- Skill naming conventions
- Description length limits
- Dual-manifest synchronization (`.bricks-plugin/` ↔ `.claude-plugin/`)
- Version consistency

### Code Style

- **TypeScript**: Follow existing patterns in the codebase
  - Use strict mode
  - Add proper type annotations
  - Include JSDoc comments for public functions

- **Python**: Follow PEP 8 conventions
  - Use type hints where applicable
  - Keep functions focused and well-documented

- **Naming Conventions**:
  - Directories and files: kebab-case
  - TypeScript classes: PascalCase
  - Constants: UPPER_SNAKE_CASE
  - Private methods: _leadingUnderscore

### Documentation

Every skill must include:

1. **SKILL.md**: Comprehensive skill documentation
   - Purpose and use cases
   - Parameters and return values
   - Example usage
   - Error handling
   - Dependencies (if any)

2. **In-code comments**: Explain complex logic, edge cases, and assumptions

3. **README updates**: Update the main README if adding new skill categories

### Testing

- Write tests for new skills (`.test.ts` or `_test.py`)
- Ensure tests cover:
  - Happy path scenarios
  - Edge cases
  - Error conditions
- Run tests before submitting PR:
  ```bash
  bun test
  ```

## Commit Guidelines

Follow [Conventional Commits](https://www.conventionalcommits.org/):

```
type(scope): subject

body

footer
```

**Types**:
- `feat`: New feature or skill
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Code style changes (no logic change)
- `refactor`: Code refactoring
- `perf`: Performance improvements
- `test`: Adding or updating tests
- `chore`: Maintenance, dependency updates

**Examples**:
```
feat(document-parser): add support for markdown files

- Implement markdown parsing with frontmatter support
- Add extraction of headers, lists, and code blocks
- Includes comprehensive test coverage

Closes #42
```

```
fix(ai-generators): correct device memory calculation

- Fix off-by-one error in RAM allocation
- Add unit tests for edge cases
```

## Pull Request Process

1. **Before submitting**:
   ```bash
   bun run validate    # All checks must pass
   bun test           # All tests must pass
   git pull origin main # Update with latest changes
   ```

2. **Create a descriptive PR**:
   - Use the [PR template](.github/pull_request_template.md)
   - Link related issues with `Closes #123`
   - Include before/after context if applicable
   - Add screenshots for UI changes (if any)

3. **PR Title Format**:
   ```
   type(scope): brief description
   ```

4. **Code Review**:
   - Respond to feedback promptly
   - Push follow-up commits to the same branch
   - Keep PR focused on a single feature/fix
   - Request re-review after making requested changes

5. **Merge**:
   - Squash commits for a clean history (maintainers will do this)
   - Ensure all CI checks pass
   - Get approval from at least one maintainer

## Versioning

- Bump version in both `.bricks-plugin/manifest.json` and `.claude-plugin/manifest.json`
- Follow semantic versioning: `MAJOR.MINOR.PATCH`
- Update `CHANGELOG.md` with user-friendly descriptions

## Reporting Issues

Use the [issue templates](.github/ISSUE_TEMPLATE/) for:

- **Bug reports**: Include reproduction steps, expected vs actual behavior
- **Feature requests**: Describe the use case and expected behavior
- **Questions**: Use Discussions (if enabled) or open an issue with `question` label

## Performance Considerations

- **Script Execution**: All parsers run locally
  - Keep startup time under 2 seconds for interactive features
  - Cache results when applicable
  - Stream large datasets to avoid memory spikes

- **Model Sizing**:
  - Document hardware requirements clearly
  - Test on target devices when possible
  - Provide quantization recommendations

## Security

- Do not commit secrets, API keys, or credentials
- Use environment variables for configuration
- Review [SECURITY.md](SECURITY.md) for security practices

## Questions or Need Help?

- Open a discussion in the repository
- Check existing issues and PRs
- Review the [DEVELOPMENT.md](DEVELOPMENT.md) guide

---

Thank you for contributing to making BRICKS CTOR even better! 🚀
