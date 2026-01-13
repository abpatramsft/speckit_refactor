---
description: Create, edit, and analyze PowerPoint presentations (.pptx files). Use this skill when the user asks to build presentations, slides, pitch decks, or needs to work with PowerPoint files (examples include creating new presentations from scratch, editing existing slides, converting templates, adding charts/tables, or analyzing presentation content).
---

## CRITICAL OPERATING PRINCIPLE

**YOU MUST EXECUTE ALL SCRIPTS AND DELIVER THE FINAL .PPTX FILE**

- **DO NOT** provide instructions for the user to run scripts
- **DO NOT** stop after creating HTML/JS files
- **DO** run `node build-script.js` yourself to generate the .pptx
- **DO** run `python thumbnail.py` yourself for validation
- **DO** execute all Python scripts (rearrange.py, replace.py, etc.) yourself
- **The user should receive a completed .pptx file, not a build script**

## User Input

```text
$ARGUMENTS
```

You **MUST** consider the user input before proceeding (if not empty).

The user provides presentation requirements: what they want to create, edit, or analyze. They may include context about the topic, audience, design preferences, or technical constraints.

## Overview

A user may ask you to create, edit, or analyze the contents of a .pptx file. A .pptx file is essentially a ZIP archive containing XML files and other resources that you can read or edit. You have different tools and workflows available for different tasks.

All scripts and tools are located in `.specify/scripts/pptx/` directory.

## Reading and Analyzing Presentations

### Text extraction
If you just need to read the text contents of a presentation, you should convert the document to markdown:

```bash
# Convert document to markdown
python -m markitdown path-to-file.pptx
```

### Raw XML access
You need raw XML access for: comments, speaker notes, slide layouts, animations, design elements, and complex formatting. For any of these features, you'll need to unpack a presentation and read its raw XML contents.

#### Unpacking a file
```bash
python .specify/scripts/pptx/ooxml/scripts/unpack.py <office_file> <output_dir>
```

#### Key file structures
* `ppt/presentation.xml` - Main presentation metadata and slide references
* `ppt/slides/slide{N}.xml` - Individual slide contents (slide1.xml, slide2.xml, etc.)
* `ppt/notesSlides/notesSlide{N}.xml` - Speaker notes for each slide
* `ppt/comments/modernComment_*.xml` - Comments for specific slides
* `ppt/slideLayouts/` - Layout templates for slides
* `ppt/slideMasters/` - Master slide templates
* `ppt/theme/` - Theme and styling information
* `ppt/media/` - Images and other media files

## Creating a New PowerPoint Presentation (from scratch)

When creating a new PowerPoint presentation from scratch, use the **html2pptx** workflow to convert HTML slides to PowerPoint with accurate positioning.

### Design Thinking

Before creating any presentation, understand the context and commit to a BOLD aesthetic direction:
- **Purpose**: What problem does this presentation solve? Who uses it?
- **Subject Matter**: What is this about? What tone, industry, or mood does it suggest?
- **Branding**: If the user mentions a company/organization, consider their brand colors and identity
- **Tone**: Pick an extreme: brutally minimal, maximalist chaos, retro-futuristic, organic/natural, luxury/refined, playful/toy-like, editorial/magazine, brutalist/raw, art deco/geometric, soft/pastel, industrial/utilitarian, etc.
- **Differentiation**: What makes this UNFORGETTABLE? What's the one thing someone will remember?

**CRITICAL**: Choose a clear conceptual direction and execute it with precision. Bold maximalism and refined minimalism both work - the key is intentionality, not intensity.

State your design approach BEFORE writing code.

### Design Requirements

- ✅ State your content-informed design approach BEFORE writing code
- ✅ Use web-safe fonts only: Arial, Helvetica, Times New Roman, Georgia, Courier New, Verdana, Tahoma, Trebuchet MS, Impact
- ✅ Create clear visual hierarchy through size, weight, and color
- ✅ Ensure readability: strong contrast, appropriately sized text, clean alignment
- ✅ Be consistent: repeat patterns, spacing, and visual language across slides

### Color Palette Selection

**Choosing colors creatively**:
- **Think beyond defaults**: What colors genuinely match this specific topic? Avoid autopilot choices.
- **Consider multiple angles**: Topic, industry, mood, energy level, target audience, brand identity (if mentioned)
- **Be adventurous**: Try unexpected combinations - a healthcare presentation doesn't have to be green, finance doesn't have to be navy
- **Build your palette**: Pick 3-5 colors that work together (dominant colors + supporting tones + accent)
- **Ensure contrast**: Text must be clearly readable on backgrounds

**Example color palettes** (use these to spark creativity - choose one, adapt it, or create your own):

1. **Classic Blue**: Deep navy (#1C2833), slate gray (#2E4053), silver (#AAB7B8), off-white (#F4F6F6)
2. **Teal & Coral**: Teal (#5EA8A7), deep teal (#277884), coral (#FE4447), white (#FFFFFF)
3. **Bold Red**: Red (#C0392B), bright red (#E74C3C), orange (#F39C12), yellow (#F1C40F), green (#2ECC71)
4. **Warm Blush**: Mauve (#A49393), blush (#EED6D3), rose (#E8B4B8), cream (#FAF7F2)
5. **Burgundy Luxury**: Burgundy (#5D1D2E), crimson (#951233), rust (#C15937), gold (#997929)
6. **Deep Purple & Emerald**: Purple (#B165FB), dark blue (#181B24), emerald (#40695B), white (#FFFFFF)
7. **Cream & Forest Green**: Cream (#FFE1C7), forest green (#40695B), white (#FCFCFC)
8. **Pink & Purple**: Pink (#F8275B), coral (#FF574A), rose (#FF737D), purple (#3D2F68)
9. **Lime & Plum**: Lime (#C5DE82), plum (#7C3A5F), coral (#FD8C6E), blue-gray (#98ACB5)
10. **Black & Gold**: Gold (#BF9A4A), black (#000000), cream (#F4F6F6)
11. **Sage & Terracotta**: Sage (#87A96B), terracotta (#E07A5F), cream (#F4F1DE), charcoal (#2C2C2C)
12. **Charcoal & Red**: Charcoal (#292929), red (#E33737), light gray (#CCCBCB)
13. **Vibrant Orange**: Orange (#F96D00), light gray (#F2F2F2), charcoal (#222831)
14. **Forest Green**: Black (#191A19), green (#4E9F3D), dark green (#1E5128), white (#FFFFFF)
15. **Retro Rainbow**: Purple (#722880), pink (#D72D51), orange (#EB5C18), amber (#F08800), gold (#DEB600)
16. **Vintage Earthy**: Mustard (#E3B448), sage (#CBD18F), forest green (#3A6B35), cream (#F4F1DE)
17. **Coastal Rose**: Old rose (#AD7670), beaver (#B49886), eggshell (#F3ECDC), ash gray (#BFD5BE)
18. **Orange & Turquoise**: Light orange (#FC993E), grayish turquoise (#667C6F), white (#FCFCFC)

### Visual Design Elements

Choose from these to create distinctive presentations:

**Geometric Patterns**:
- Diagonal section dividers instead of horizontal
- Asymmetric column widths (30/70, 40/60, 25/75)
- Rotated text headers at 90° or 270°
- Circular/hexagonal frames for images
- Triangular accent shapes in corners
- Overlapping shapes for depth

**Typography Treatments**:
- Extreme size contrast (72pt headlines vs 11pt body)
- All-caps headers with wide letter spacing
- Numbered sections in oversized display type
- Monospace (Courier New) for data/stats/technical content
- Outlined text for emphasis

**Layout Innovations**:
- Full-bleed images with text overlays
- Sidebar column (20-30% width) for navigation/context
- Modular grid systems (3×3, 4×4 blocks)
- Z-pattern or F-pattern content flow
- Floating text boxes over colored shapes
- Magazine-style multi-column layouts

**Background Treatments**:
- Solid color blocks occupying 40-60% of slide
- Gradient fills (vertical or diagonal only)
- Split backgrounds (two colors, diagonal or vertical)
- Edge-to-edge color bands
- Negative space as a design element

### Layout Tips
**When creating slides with charts or tables:**
- **Two-column layout (PREFERRED)**: Use a header spanning the full width, then two columns below - text/bullets in one column and the featured content in the other. This provides better balance and makes charts/tables more readable. Use flexbox with unequal column widths (e.g., 40%/60% split) to optimize space for each content type.
- **Full-slide layout**: Let the featured content (chart/table) take up the entire slide for maximum impact and readability
- **NEVER vertically stack**: Do not place charts/tables below text in a single column - this causes poor readability and layout issues

### Workflow for Creating from Scratch

1. **MANDATORY - READ ENTIRE FILE**: Read `.specify/scripts/pptx/html2pptx.md` completely from start to finish. **NEVER set any range limits when reading this file.** Read the full file content for detailed syntax, critical formatting rules, and best practices before proceeding with presentation creation.

2. Create an HTML file for each slide with proper dimensions (e.g., 720pt × 405pt for 16:9)
   - Use `<p>`, `<h1>`-`<h6>`, `<ul>`, `<ol>` for all text content
   - Use `class="placeholder"` for areas where charts/tables will be added (render with gray background for visibility)
   - **CRITICAL**: Rasterize gradients and icons as PNG images FIRST using Sharp, then reference in HTML
   - **LAYOUT**: For slides with charts/tables/images, use either full-slide layout or two-column layout for better readability

3. Create a JavaScript build file using the `.specify/scripts/pptx/html2pptx.js` library to convert HTML slides to PowerPoint
   - Import the html2pptx function
   - Use the `html2pptx()` function to process each HTML file
   - Add charts and tables to placeholder areas using PptxGenJS API
   - Save the presentation using `pptx.writeFile()`

4. **CRITICAL - EXECUTE THE BUILD**: After creating the build script, **IMMEDIATELY RUN IT**:
   - Execute: `node <build-script-name>.js`
   - This generates the final .pptx file
   - **DO NOT** ask the user to run it - you must run it yourself
   - Verify the .pptx file was created successfully

5. **Visual validation**: Generate thumbnails and inspect for layout issues
   - **AUTOMATICALLY EXECUTE**: `python .specify/scripts/pptx/thumbnail.py <output-file>.pptx workspace/thumbnails --cols 4`
   - **DO NOT** ask the user to run this - execute it yourself
   - Read and carefully examine the generated thumbnail image for:
     - **Text cutoff**: Text being cut off by header bars, shapes, or slide edges
     - **Text overlap**: Text overlapping with other text or shapes
     - **Positioning issues**: Content too close to slide boundaries or other elements
     - **Contrast issues**: Insufficient contrast between text and backgrounds
   - If issues found, adjust HTML margins/spacing/colors and regenerate the presentation
   - Repeat until all slides are visually correct

6. **Deliver the final presentation**:
   - Report the location of the generated .pptx file
   - Mention any optional next steps for the user (e.g., adding custom data, swapping images)
   - **The user should receive a working .pptx file, not instructions to build it**

## Editing an Existing PowerPoint Presentation

When editing slides in an existing PowerPoint presentation, you need to work with the raw Office Open XML (OOXML) format. This involves unpacking the .pptx file, editing the XML content, and repacking it.

### Workflow for Editing Existing Files

1. **MANDATORY - READ ENTIRE FILE**: Read `.specify/scripts/pptx/ooxml.md` completely from start to finish. **NEVER set any range limits when reading this file.** Read the full file content for detailed guidance on OOXML structure and editing workflows before any presentation editing.

2. **Unpack the presentation**:
   * **EXECUTE IMMEDIATELY**: `python .specify/scripts/pptx/ooxml/scripts/unpack.py <office_file> <output_dir>`
   * **DO NOT** ask user to run - execute it yourself

3. **Edit the XML files** (primarily `ppt/slides/slide{N}.xml` and related files)
   * Read the relevant XML files
   * Make necessary edits using the Edit tool
   * Follow OOXML structure rules from ooxml.md

4. **CRITICAL - Validate immediately after each edit**:
   * **EXECUTE IMMEDIATELY**: `python .specify/scripts/pptx/ooxml/scripts/validate.py <dir> --original <file>`
   * **DO NOT** ask user to run - execute it yourself
   * Fix any validation errors before proceeding
   * If validation fails, fix the XML and re-validate

5. **Pack the final presentation**:
   * **EXECUTE IMMEDIATELY**: `python .specify/scripts/pptx/ooxml/scripts/pack.py <input_directory> <office_file>`
   * **DO NOT** ask user to run - execute it yourself
   * Verify the output .pptx file was created successfully
   * Report the location of the final .pptx file
   * **The user should receive a working .pptx file, not instructions to build it**

## Creating from an Existing Template

When you need to create a presentation that follows an existing template's design, you'll need to duplicate and re-arrange template slides before then replacing placeholder content.

### Workflow for Template-Based Creation

1. **Extract template text AND create visual thumbnail grid**:
   ```bash
   # Extract text
   python -m markitdown template.pptx > template-content.md

   # Create thumbnail grids
   python .specify/scripts/pptx/thumbnail.py template.pptx
   ```
   * Read `template-content.md`: Read the entire file to understand the contents of the template presentation. **NEVER set any range limits when reading this file.**

2. **Analyze template and save inventory to a file**:
   * **Visual Analysis**: Review thumbnail grid(s) to understand slide layouts, design patterns, and visual structure
   * Create and save a template inventory file at `template-inventory.md` containing:
     ```markdown
     # Template Inventory Analysis
     **Total Slides: [count]**
     **IMPORTANT: Slides are 0-indexed (first slide = 0, last slide = count-1)**

     ## [Category Name]
     - Slide 0: [Layout code if available] - Description/purpose
     - Slide 1: [Layout code] - Description/purpose
     [... EVERY slide must be listed individually with its index ...]
     ```

3. **Create presentation outline based on template inventory**:
   * Review available templates from step 2
   * Choose an intro or title template for the first slide
   * Choose safe, text-based layouts for the other slides
   * **CRITICAL: Match layout structure to actual content**:
     - Single-column layouts: Use for unified narrative or single topic
     - Two-column layouts: Use ONLY when you have exactly 2 distinct items/concepts
     - Three-column layouts: Use ONLY when you have exactly 3 distinct items/concepts
     - Never use layouts with more placeholders than you have content
   * Save `outline.md` with content AND template mapping

4. **Duplicate, reorder, and delete slides using `rearrange.py`**:
   * **EXECUTE IMMEDIATELY**: `python .specify/scripts/pptx/rearrange.py template.pptx working.pptx 0,34,34,50,52`
   * The script handles duplicating repeated slides, deleting unused slides, and reordering automatically
   * Slide indices are 0-based (first slide is 0, second is 1, etc.)
   * The same slide index can appear multiple times to duplicate that slide
   * **DO NOT** ask user to run - execute it yourself

5. **Extract ALL text using the `inventory.py` script**:
   * **EXECUTE IMMEDIATELY**: `python .specify/scripts/pptx/inventory.py working.pptx text-inventory.json`
   * **Read text-inventory.json**: Read the entire text-inventory.json file to understand all shapes and their properties. **NEVER set any range limits when reading this file.**
   * **DO NOT** ask user to run - execute it yourself

6. **Generate replacement text and save to JSON**:
   Based on the text inventory from the previous step:
   - **CRITICAL**: First verify which shapes exist in the inventory - only reference shapes that are actually present
   - **VALIDATION**: The replace.py script will validate that all shapes in your replacement JSON exist in the inventory
   - **AUTOMATIC CLEARING**: ALL text shapes from the inventory will be cleared unless you provide "paragraphs" for them
   - Add a "paragraphs" field to shapes that need content
   - **IMPORTANT**: When bullet: true, do NOT include bullet symbols (•, -, *) in text - they're added automatically
   - Save the updated inventory with replacements to `replacement-text.json`

7. **Apply replacements using the `replace.py` script**:
   * **EXECUTE IMMEDIATELY**: `python .specify/scripts/pptx/replace.py working.pptx replacement-text.json output.pptx`
   * This generates the final presentation
   * **DO NOT** ask user to run - execute it yourself
   * Verify the output.pptx file was created successfully

8. **Visual validation and delivery**:
   * **EXECUTE IMMEDIATELY**: `python .specify/scripts/pptx/thumbnail.py output.pptx workspace/thumbnails --cols 4`
   * Read and examine thumbnails for any issues
   * Report the location of the final .pptx file
   * **The user should receive a working .pptx file, not instructions to build it**

## Creating Thumbnail Grids

To create visual thumbnail grids of PowerPoint slides for quick analysis and reference:

```bash
python .specify/scripts/pptx/thumbnail.py template.pptx [output_prefix] [--cols 4]
```

**Features**:
- Creates: `thumbnails.jpg` (or `thumbnails-1.jpg`, `thumbnails-2.jpg`, etc. for large decks)
- Default: 5 columns, max 30 slides per grid (5×6)
- Custom columns: `--cols 4` (range: 3-6)
- Slides are zero-indexed (Slide 0, Slide 1, etc.)

## Code Style Guidelines

**IMPORTANT**: When generating code for PPTX operations:
- Write concise code
- Avoid verbose variable names and redundant operations
- Avoid unnecessary print statements

## Dependencies

Required dependencies (should already be installed):

- **markitdown**: `pip install "markitdown[pptx]"` (for text extraction from presentations)
- **pptxgenjs**: `npm install -g pptxgenjs` (for creating presentations via html2pptx)
- **playwright**: `npm install -g playwright` (for HTML rendering in html2pptx)
- **sharp**: `npm install -g sharp` (for SVG rasterization and image processing)
- **LibreOffice**: For PDF conversion (platform-specific installation)
- **Poppler**: For pdftoppm to convert PDF to images (platform-specific installation)
- **defusedxml**: `pip install defusedxml` (for secure XML parsing)
- **python-pptx**: `pip install python-pptx` (for PowerPoint manipulation)

## Key Rules

- **ALWAYS** read the full documentation files (html2pptx.md, ooxml.md) before proceeding
- **NEVER** use range limits when reading documentation or inventory files
- **VALIDATE** immediately after making changes to XML files
- **DESIGN FIRST**: State your design approach before writing code
- **BE BOLD**: Choose distinctive, memorable designs that match the content
- Use absolute paths for all scripts: `.specify/scripts/pptx/...`
