# Development Guide

Complete guide for setting up your development environment and understanding the project structure.

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Project Structure](#project-structure)
3. [Setup](#setup)
4. [Creating Skills](#creating-skills)
5. [Testing](#testing)
6. [Validation](#validation)
7. [Publishing](#publishing)
8. [Troubleshooting](#troubleshooting)

## Prerequisites

### Required

- **Node.js** 18+ or **Bun** (recommended for this project)
- **Git** for version control
- **Text editor** (VS Code recommended)

### Optional (plugin-specific)

- **Python** 3.8+ (for Python-based skills)
- **Blender** 3.6+ (for Scene3D Studio features)
- **BRICKS CLI** (for device telemetry features)
- **uv** (for BlenderMCP addon)

## Project Structure

```
bricks-ctor-agent-skills/
├── .bricks-plugin/           # BRICKS CTOR plugin manifest
│   ├── manifest.json
│   └── skills/
├── .claude-plugin/           # Claude Code plugin manifest
│   ├── manifest.json
│   └── skills/
├── plugins/                  # Plugin implementations
│   ├── document-essentials/
│   ├── ai-features-toolkit/
│   ├── scene3d-studio/
│   ├── app-templates/
│   └── device-telemetry/
├── scripts/
│   ├── validate.mjs          # Manifest & skill validation
│   └── ...
├── CHANGELOG.md
├── CONTRIBUTING.md
├── DEVELOPMENT.md            # This file
└── README.md
```

### Manifest Architecture

Skills are declared in **two manifest locations** for compatibility:

- `.bricks-plugin/manifest.json` - CTOR standalone
- `.claude-plugin/manifest.json` - Claude Code integration

Both must be kept in sync (version numbers, skill definitions).

### Skill Structure

Each skill lives in a subdirectory:

```
plugins/{plugin-name}/skills/{skill-name}/
├── SKILL.md                  # User-facing documentation
├── index.ts                  # TypeScript implementation
├── index.test.ts             # Tests
├── manifest.json (optional)  # Skill-level config
└── README.md (optional)      # Additional context
```

## Setup

### 1. Clone the Repository

```bash
git clone https://github.com/mybigday/bricks-ctor-agent-skills.git
cd bricks-ctor-agent-skills
```

### 2. Install Dependencies

```bash
# Using Bun (preferred)
bun install

# Or using npm/yarn
npm install
```

### 3. Verify Setup

```bash
bun run validate
```

## Creating Skills

### Step 1: Choose Your Plugin Category

- **document-essentials**: File parsing (PDF, DOCX, XLSX, PPTX)
- **ai-features-toolkit**: AI/ML generators and models
- **scene3d-studio**: 3D content and Blender integration
- **app-templates**: Project templating utilities
- **device-telemetry**: Device metrics and diagnostics

### Step 2: Create Skill Directory

```bash
# Example: Adding an image parser to document-essentials
mkdir -p plugins/document-essentials/skills/image-parser
cd plugins/document-essentials/skills/image-parser
```

### Step 3: Implement SKILL.md

Document your skill comprehensively:

```markdown
# Image Parser Skill

Extract metadata and content from images using AI.

## Parameters

- `imagePath` (string): Path to image file
- `extractText` (boolean): Extract text via OCR
- `detectObjects` (boolean): Detect objects in image

## Returns

```json
{
  "metadata": { "width": 1920, "height": 1080 },
  "text": "extracted text",
  "objects": [{ "label": "person", "confidence": 0.95 }]
}
```

## Example

```typescript
const result = await imageParser({
  imagePath: './photo.jpg',
  extractText: true
});
```
```

### Step 4: Implement the Skill

```typescript
// index.ts
import { readFile } from 'fs/promises';

export interface ImageParserInput {
  imagePath: string;
  extractText?: boolean;
  detectObjects?: boolean;
}

export interface ImageParserResult {
  metadata: Record<string, unknown>;
  text?: string;
  objects?: Array<{ label: string; confidence: number }>;
}

export async function imageParser(
  input: ImageParserInput
): Promise<ImageParserResult> {
  try {
    const imageBuffer = await readFile(input.imagePath);
    
    // Your implementation here
    const result: ImageParserResult = {
      metadata: {
        size: imageBuffer.length,
        // ... more metadata
      }
    };

    if (input.extractText) {
      // Implement OCR
      result.text = '';
    }

    if (input.detectObjects) {
      // Implement object detection
      result.objects = [];
    }

    return result;
  } catch (error) {
    throw new Error(`Failed to parse image: ${error}`);
  }
}
```

### Step 5: Add Tests

```typescript
// index.test.ts
import { describe, it, expect } from 'bun:test';
import { imageParser } from './index';

describe('imageParser', () => {
  it('should extract image metadata', async () => {
    const result = await imageParser({
      imagePath: './test-image.jpg'
    });
    
    expect(result.metadata).toBeDefined();
    expect(result.metadata.size).toBeGreaterThan(0);
  });

  it('should extract text when enabled', async () => {
    const result = await imageParser({
      imagePath: './test-image.jpg',
      extractText: true
    });
    
    expect(result.text).toBeDefined();
  });

  it('should handle missing files gracefully', async () => {
    expect(
      imageParser({ imagePath: './nonexistent.jpg' })
    ).rejects.toThrow();
  });
});
```

### Step 6: Update Manifests

Add to `.bricks-plugin/manifest.json`:

```json
{
  "skills": {
    "image-parser": {
      "name": "Image Parser",
      "description": "Extract metadata and content from images",
      "instructions": "Use this to analyze image files.",
      "version": "1.0.0"
    }
  }
}
```

Update `.claude-plugin/manifest.json` identically.

## Testing

### Run All Tests

```bash
bun test
```

### Run Specific Test

```bash
bun test plugins/document-essentials/skills/image-parser/index.test.ts
```

### Test Coverage

```bash
bun test --coverage
```

## Validation

### Check Everything

```bash
bun run validate
```

This checks:

- ✅ JSON syntax in manifests
- ✅ Skill naming conventions (kebab-case)
- ✅ Description length (1-160 chars)
- ✅ Required fields present
- ✅ Version consistency between manifests
- ✅ No reserved skill names used

### Manual Validation Steps

```bash
# Validate manifests
node -e "JSON.parse(require('fs').readFileSync('.bricks-plugin/manifest.json'))" && echo "✓ Valid JSON"

# Check naming conventions
ls plugins/*/skills/ | grep -E '^[a-z]+(-[a-z]+)*$' || echo "✗ Non-conforming names found"

# Verify version sync
diff <(jq '.version' .bricks-plugin/manifest.json) <(jq '.version' .claude-plugin/manifest.json) || echo "✗ Version mismatch"
```

## Publishing

### Pre-Publication Checklist

- [ ] All tests pass: `bun test`
- [ ] Validation passes: `bun run validate`
- [ ] SKILL.md is complete
- [ ] Version bumped in both manifests
- [ ] CHANGELOG.md updated
- [ ] PR reviewed and approved

### Version Bumping

Use semantic versioning:

```bash
# In .bricks-plugin/manifest.json and .claude-plugin/manifest.json

{
  "version": "1.0.0",  // MAJOR.MINOR.PATCH
  "skills": {
    "your-skill": {
      "version": "1.0.0"  // Align with parent version
    }
  }
}
```

### Update Changelog

```markdown
## [1.0.0] - 2024-09-15

### Added
- New image parser skill for extracting metadata and content from images
- OCR support for text extraction
- Object detection capability

### Fixed
- Memory leak in image processing pipeline

### Changed
- Improved error handling for corrupted image files
```

## Troubleshooting

### Common Issues

#### Validation Fails

```bash
# Error: skill name not kebab-case
# Fix: Rename directory to match kebab-case pattern
mv plugins/my_skill plugins/my-skill

# Error: version mismatch between manifests
# Fix: Ensure both manifests have identical version numbers
```

#### Tests Fail with Import Errors

```bash
# Ensure correct paths
import { imageParser } from './index';  // ✓ correct
import imageParser from './index';      // ✗ wrong (no default export)

# Clear cache
bun run validate --force
rm -rf node_modules/.bun
bun install
```

#### TypeScript Errors

```bash
# Update types
bun add -D @types/node

# Regenerate tsconfig if needed
bun run configure:ts
```

#### Plugin Not Appearing in CTOR

- Verify version was bumped
- Check manifest JSON syntax
- Validate skill names (no spaces, kebab-case)
- Ensure descriptions < 160 chars
- Restart CTOR application

## Performance Tips

- **Local Parsing**: Keep all file parsing local (no server calls)
- **Streaming**: Stream large files instead of loading entirely
- **Caching**: Cache parsed results with appropriate TTL
- **Progress**: Show progress for long operations
- **Timeouts**: Set reasonable timeouts for CLI operations (bricks commands)

## Getting Help

- Review existing skills in `plugins/*/skills/` for patterns
- Check `CONTRIBUTING.md` for community guidelines
- Open an issue with `question` label
- Review git history: `git log --oneline plugins/`

---

Happy coding! 🎉
