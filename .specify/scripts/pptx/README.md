# PowerPoint Generation Scripts for SpecKit

This directory contains scripts and tools for creating, editing, and analyzing PowerPoint presentations (.pptx files) within the SpecKit framework.

## Overview

The PPTX agent (`speckit.pptx`) provides comprehensive PowerPoint capabilities:
- **Create presentations from scratch** using HTML-to-PowerPoint conversion
- **Edit existing presentations** by manipulating Office Open XML (OOXML) structure
- **Use templates** by duplicating and customizing existing slide decks
- **Analyze presentations** by extracting text and generating thumbnail grids

## Directory Structure

```
.specify/scripts/pptx/
├── README.md                    # This file
├── SKILL.md                     # Complete reference documentation
├── html2pptx.md                 # HTML to PowerPoint conversion guide
├── ooxml.md                     # Office Open XML technical reference
├── html2pptx.js                 # HTML to PowerPoint converter
├── inventory.py                 # Extract text inventory from presentations
├── rearrange.py                 # Duplicate, reorder, and delete slides
├── replace.py                   # Apply text replacements to slides
├── thumbnail.py                 # Generate thumbnail grids
└── ooxml/
    └── scripts/
        ├── pack.py              # Pack OOXML directory to .pptx
        ├── unpack.py            # Unpack .pptx to OOXML directory
        └── validate.py          # Validate OOXML structure
```

## Quick Start

### Create a New Presentation from Scratch

1. Read the documentation:
   ```bash
   cat .specify/scripts/pptx/html2pptx.md
   ```

2. Create HTML files for each slide (720pt × 405pt for 16:9)

3. Convert HTML to PowerPoint using html2pptx.js

4. Generate thumbnails for validation:
   ```bash
   python .specify/scripts/pptx/thumbnail.py output.pptx
   ```

### Use an Existing Template

1. Extract template inventory:
   ```bash
   python -m markitdown template.pptx > template-content.md
   python .specify/scripts/pptx/thumbnail.py template.pptx
   ```

2. Rearrange slides:
   ```bash
   python .specify/scripts/pptx/rearrange.py template.pptx working.pptx 0,34,34,50,52
   ```

3. Extract text inventory:
   ```bash
   python .specify/scripts/pptx/inventory.py working.pptx text-inventory.json
   ```

4. Apply replacements:
   ```bash
   python .specify/scripts/pptx/replace.py working.pptx replacement-text.json output.pptx
   ```

### Edit an Existing Presentation

1. Read the documentation:
   ```bash
   cat .specify/scripts/pptx/ooxml.md
   ```

2. Unpack the presentation:
   ```bash
   python .specify/scripts/pptx/ooxml/scripts/unpack.py input.pptx unpacked/
   ```

3. Edit XML files in the unpacked directory

4. Validate changes:
   ```bash
   python .specify/scripts/pptx/ooxml/scripts/validate.py unpacked/ --original input.pptx
   ```

5. Pack the presentation:
   ```bash
   python .specify/scripts/pptx/ooxml/scripts/pack.py unpacked/ output.pptx
   ```

## Usage with SpecKit Agent

To use the PPTX agent in SpecKit:

```bash
# In your SpecKit environment
@speckit.pptx Create a presentation about AI trends with 5 slides
```

The agent will:
1. Analyze the topic and choose an appropriate design
2. Create HTML slides with proper formatting
3. Convert to PowerPoint using the scripts
4. Generate thumbnails for validation
5. Deliver the final .pptx file

## Key Features

### Design-First Approach
The agent analyzes content and chooses appropriate design elements before creating presentations, ensuring context-appropriate aesthetics.

### Three Creation Methods
1. **HTML2PPTX**: Convert HTML to PowerPoint with pixel-perfect positioning
2. **Template-Based**: Leverage existing templates by duplicating and customizing
3. **OOXML Editing**: Direct XML manipulation for complex modifications

### Validation & Quality Control
- Automatic thumbnail generation for visual inspection
- XML validation to prevent corrupted files
- Text overflow detection
- Layout issue identification

## Dependencies

Required packages:
- `markitdown` - Text extraction from presentations
- `pptxgenjs` - PowerPoint generation from HTML
- `playwright` - HTML rendering
- `sharp` - Image processing
- `python-pptx` - PowerPoint manipulation
- `defusedxml` - Secure XML parsing
- LibreOffice - PDF conversion
- Poppler - PDF to image conversion

## Documentation

For complete details, see:
- [SKILL.md](./SKILL.md) - Complete reference
- [html2pptx.md](./html2pptx.md) - HTML conversion guide
- [ooxml.md](./ooxml.md) - XML structure reference

## Source

These scripts are adapted from [Anthropic's Claude Skills](https://github.com/anthropics/skills/tree/main/skills/pptx) to work within the SpecKit framework.
