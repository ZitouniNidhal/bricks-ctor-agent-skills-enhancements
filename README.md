# BRICKS CTOR Agent Skills

[![Validate & Test](https://github.com/mybigday/bricks-ctor-agent-skills/actions/workflows/validate.yml/badge.svg)](https://github.com/mybigday/bricks-ctor-agent-skills/actions)
[![MIT License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Built-in to CTOR v2.25+](https://img.shields.io/badge/CTOR-v2.25%2B-brightgreen)](https://ctor.bricks.tools)

Official plugins for the [CTOR](https://ctor.bricks.tools) desktop agent, maintained by the BRICKS team. A comprehensive collection of skills for document parsing, AI integration, 3D modeling, templating, and device telemetry.

## 📋 Table of Contents

- [Quick Start](#-quick-start)
- [Installation](#-installation)
- [Skills Overview](#-skills-overview)
- [Contributing](#-contributing)
- [Development](#-development)
- [Documentation](#-documentation)
- [License](#license)

## 🚀 Quick Start

### For CTOR v2.25+

The marketplace is **built-in** — no installation needed! Simply:

1. Open CTOR → **Plugins** panel (bottom)
2. Find "BRICKS CTOR Agent Skills" with the "Built-in" badge
3. Open the plugin and toggle skills you want
4. Updates appear automatically on version bumps

### For Earlier CTOR Versions

1. Go to **Plugins → Add marketplace**
2. Enter: `mybigday/bricks-ctor-agent-skills`
3. Open and select skills

### For Claude Code

```
/plugin marketplace add mybigday/bricks-ctor-agent-skills
```

## 📦 Installation

### Prerequisites

- **CTOR v2.25+** (built-in) or **CTOR v2.0-2.24** (manual add)
- **Claude Code** (optional, for cross-tool usage)
- Skill-specific requirements listed below

### Local Testing

Test skills locally before publishing:

```bash
# Clone repository
git clone https://github.com/mybigday/bricks-ctor-agent-skills.git
cd bricks-ctor-agent-skills

# Install dependencies
bun install

# Validate (must pass before any changes)
bun run validate

# In CTOR: Plugins → Add marketplace → Local folder → [path to repo]
```

## 🎯 Skills Overview

### 📄 Document Essentials

Parse business files on your machine and import content as app data, media, or searchable knowledge.

| Skill | Purpose | File Types |
|-------|---------|-----------|
| **pdf** | Extract text from PDFs; wire runtime ingestion | `.pdf` |
| **docx** | Extract headings, lists, tables, images | `.docx` |
| **xlsx** | Parse spreadsheets into structured rows | `.xlsx`, `.csv`, `.tsv` |
| **pptx** | Extract slides, notes, images; rebuild as media | `.pptx` |

**Note**: All parsing runs locally. Word/sheet/deck parsers are self-contained; PDF uses cached `officeparser` CLI.

### 🤖 AI Features Toolkit

Choose the right AI generator, size models for hardware, and implement end-to-end patterns.

| Skill | Purpose |
|-------|---------|
| **ai-generators** | Decision guide for generator + platform matching |
| **ai-model-selection** | Hardware-aware GGUF sizing and quantization |
| **ai-recipes** | Voice kiosk, RAG, captions, generative media patterns |

**Generators Supported**: llama.cpp, MLX, NeuroPilot, Qualcomm QNN, ONNX, Apple Intelligence, Anthropic, OpenAI-compatible APIs

### 🎨 Scene3D Studio

Full 3D content pipeline from Blender to deployed apps.

| Skill | Purpose | Requirements |
|-------|---------|--------------|
| **blender-pipeline** | Drive Blender headlessly; create & optimize models | Blender 3.6+ |
| **scene3d-authoring** | Compose objects, lights, camera, environment | - |
| **scene3d-interactions** | Per-frame scripts, raycast picking, interactions | - |

**Optional**: Blender MCP server for live connection (requires `uv` + BlenderMCP addon)

### 📋 App Templates

Distill existing apps into reusable prompt templates.

| Skill | Purpose |
|-------|---------|
| **template-creator** | Convert project to 5-10 plain-language requirements |

**Note**: Formerly built-in; legacy CTOR builds skip manual install

### 📊 Device Telemetry

Analyze real device behavior via metrics, events, and screenshots.

| Skill | Purpose |
|-------|---------|
| **bricks-telemetry** | Device faults, usage analysis, screen reconstruction from metrics + Activity Log |

**Requirements**:
- `bricks` CLI (logged into workspace)
- OAuth credentials for Activity Log (`bricks auth login`)

---

## 🤝 Contributing

We welcome contributions! Whether adding new skills, fixing bugs, improving docs, or suggesting features.

### Before You Start

1. Read [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines
2. Review [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md)
3. Check existing [issues](https://github.com/mybigday/bricks-ctor-agent-skills/issues)

### Contribution Types

- 🐛 **Bug fixes**: Report in Issues with reproduction steps
- ✨ **New skills**: Follow [DEVELOPMENT.md](DEVELOPMENT.md) structure
- 📚 **Documentation**: Improve guides, examples, or SKILL.md files
- 🔒 **Security**: Follow [SECURITY.md](SECURITY.md) for vulnerability reporting
- 🎯 **Improvements**: Refactoring, performance, developer experience

### Quick Contribution Flow

```bash
# 1. Fork and clone
git clone https://github.com/YOUR_USERNAME/bricks-ctor-agent-skills.git

# 2. Create feature branch
git checkout -b feat/your-feature-name
# or: git checkout -b fix/your-fix-name

# 3. Make changes and test
bun run validate    # Must pass
bun test           # All tests pass

# 4. Commit with conventional format
git commit -m "feat(plugin-name): description"

# 5. Push and create PR
git push origin feat/your-feature-name
# Then open PR on GitHub with [template](.github/pull_request_template.md)
```

---

## 💻 Development

### Setup

```bash
# Clone and install
git clone https://github.com/mybigday/bricks-ctor-agent-skills.git
cd bricks-ctor-agent-skills
bun install
```

### Commands

```bash
# Validate manifests (required before any PR)
bun run validate

# Run tests
bun test

# View test coverage
bun test --coverage

# Add new dependency
bun add package-name

# Update dependencies
bun update
```

### Project Structure

```
bricks-ctor-agent-skills/
├── .bricks-plugin/       # CTOR plugin manifest
├── .claude-plugin/       # Claude Code manifest (sync with .bricks-plugin)
├── plugins/              # Skill implementations
│   ├── document-essentials/
│   ├── ai-features-toolkit/
│   ├── scene3d-studio/
│   ├── app-templates/
│   └── device-telemetry/
├── scripts/
│   └── validate.mjs      # Validation rules
├── .github/
│   ├── ISSUE_TEMPLATE/   # Issue templates
│   ├── pull_request_template.md
│   └── workflows/        # CI/CD
├── CONTRIBUTING.md       # Contribution guide
├── DEVELOPMENT.md        # Developer guide
├── SECURITY.md          # Security policy
└── CODE_OF_CONDUCT.md   # Community standards
```

### Creating a New Skill

See [DEVELOPMENT.md](DEVELOPMENT.md) for complete guide. Quick overview:

```typescript
// plugins/plugin-name/skills/skill-name/index.ts
export async function skillName(input: SkillInput): Promise<SkillOutput> {
  // Implementation
}
```

```markdown
<!-- plugins/plugin-name/skills/skill-name/SKILL.md -->
# Skill Name

Description, parameters, examples, error handling...
```

Update both `.bricks-plugin/manifest.json` and `.claude-plugin/manifest.json` with skill definition and version bump.

### Validation Rules

Enforced by `bun run validate`:

- ✅ JSON syntax in manifests
- ✅ Skill names: kebab-case, 3-50 characters
- ✅ Descriptions: 1-160 characters
- ✅ Version sync between `.bricks-plugin` and `.claude-plugin`
- ✅ No reserved skill names
- ✅ Required metadata fields present

---

## 📚 Documentation

### For Users

- **[Installation & Quick Start](#-quick-start)** - Get up and running
- **[Skills Overview](#-skills-overview)** - What each skill does
- **[CTOR Documentation](https://ctor.bricks.tools)** - CTOR user guide

### For Contributors

- **[CONTRIBUTING.md](CONTRIBUTING.md)** - How to contribute
- **[DEVELOPMENT.md](DEVELOPMENT.md)** - Complete dev guide
- **[CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md)** - Community standards
- **[SECURITY.md](SECURITY.md)** - Security practices & reporting

### Resources

- [CTOR Official Site](https://ctor.bricks.tools)
- [BRICKS Documentation](https://bricks.tools)
- [Conventional Commits](https://www.conventionalcommits.org/)
- [Semantic Versioning](https://semver.org/)

---

## 🔒 Security

For security vulnerability reporting, see [SECURITY.md](SECURITY.md). **Do not** create public GitHub issues for security concerns.

Key practices:
- ✅ All parsing runs locally (no server calls)
- ✅ No secrets or API keys in repository
- ✅ Input validation on all user inputs
- ✅ Dependency vulnerability monitoring
- ✅ MIT licensed — full source transparency

---

## 📝 Changelog

See [CHANGELOG.md](CHANGELOG.md) for version history and updates.

---

## 👥 Support & Community

- **Questions?** Open an [issue](https://github.com/mybigday/bricks-ctor-agent-skills/issues) with `question` label
- **Found a bug?** Use [bug report template](.github/ISSUE_TEMPLATE/bug_report.md)
- **Have a feature idea?** Use [feature request template](.github/ISSUE_TEMPLATE/feature_request.md)
- **Want to contribute?** Start with [CONTRIBUTING.md](CONTRIBUTING.md)

---

## 📄 License

MIT License — See [LICENSE](LICENSE) for details.

All skills ship **readable in this repository** — nothing is downloaded at install time beyond plugin metadata. Full transparency and control.

---

## 🙏 Acknowledgments

Built and maintained by the BRICKS team with ❤️ and contributions from our community.

**Happy building!** 🚀

---

<div align="center">

Made with ❤️ by [BRICKS](https://bricks.tools) | [Report an Issue](https://github.com/mybigday/bricks-ctor-agent-skills/issues) | [Contribute](CONTRIBUTING.md)

</div>
