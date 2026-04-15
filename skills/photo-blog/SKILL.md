---
name: photo-blog
description: >-
  Photo blog generator with AI cover image. Analyze photos with Gemini 3 Pro, score
  and select highlights with diversity optimization, generate narrative-driven blog with
  poetic title, scene insights, and tips. Generates diverse AI cover images using a
  89-template style library matched to blog content (mood, theme, photo count).
  Supports 1-9 images, theme/style keywords, and triple output (HTML, rich text, PNG).
argument-hint: <image_directory_or_file>
metadata:
  execution_mode: sandbox
  adk_additional_tools:
    - imagen_generate
---

# Photo Blog Generator (with AI Cover)

Generate a beautiful, narrative-driven photo blog with an AI-generated cover image. Analyzes photos using Gemini 3 Pro for scene understanding, selects highlights with diversity optimization, generates a diverse cover via template-matched Gemini 3.1 Flash Image, and produces a styled blog with title, narrative, insights, and practical tips.

## When to Use

Trigger this skill when the user:
- Asks to create a photo blog, photo story, or image-based article
- Wants a **life summary**, daily recap, travel log, or memory collage from photos
- Says "summarize my recent photos", "make a photo diary", "create a visual story"
- Provides photos and asks for a narrative / writeup / summary / review
- Requests a styled blog post from a collection of images

## After Generation

After delivering the blog, proactively suggest:
1. "Would you like a **comic version** of this?" (invoke life-comic skill)
2. "Want to try a **different theme**?" and list the `suggested_themes` from the output

## Usage

The `main.py` script lives in the same directory as this SKILL.md. Use the directory where this file is located:

```bash
python3 <SKILL_DIR>/main.py <image_dir_or_files> \
    [--max-highlights 9] \
    [--output blog.html] \
    [--date 2026-04-13] \
    [--theme "food journey"] \
    [--style "minimalist"] \
    [--format html|richtext|png|all] \
    [--skip-cover] \
    [--save-analysis analysis.json]
```

### Arguments

| Arg | Description | Default |
|-----|-------------|---------|
| `input` | Image directory or file path | required |
| `--max-highlights` | Number of highlight photos (1-9) | 9 |
| `--output` | Output file path | `blog_output.html` |
| `--date` | Date for footer (auto-detected from EXIF if omitted) | auto |
| `--theme` | Theme keyword to guide generation (e.g., "food", "nightlife") | auto |
| `--style` | Style keyword (alias for --theme) | auto |
| `--format` | Output format: `html` / `richtext` / `png` / `all` | `all` |
| `--skip-cover` | Skip AI cover generation, use original photo as hero | false |
| `--save-analysis` | Save analysis JSON for debugging | none |

### Output Format Selection

By default (`--format all`), all three formats are generated every time:
- **HTML**: self-contained page with embedded images (best for Cursor / Claude Code)
- **Rich Text**: Markdown compatible with Copilot block (`format: "markdown"`) (best for chat agents)
- **PNG**: single composite image (best for sharing / social)

The agent should pick the most appropriate format to display inline based on context. After delivering inline content, always provide download links for both PNG and HTML:
- **PNG image** — shareable long image for mobile
- **HTML page** — 供内测体验

### Image Count Support

Supports **1 to 9** input images. Works with a single photo up to large albums (auto-selects top 9 highlights from any number of inputs).

### Theme / Style Keywords

Pass `--theme` to guide generation toward a specific angle. If the photos don't match the theme (fewer than 2 relevant photos), the skill falls back to auto-detected themes and returns `suggested_themes` with 3 alternatives.

## AI Cover Image

By default, an AI-generated cover image replaces the hero photo at the top of the blog. The cover is:

- **Template-driven**: Matched from a library of 89 analyzed reference styles
- **Content-aware**: Scoring considers photo count, mood, theme, and visual diversity
- **Style-diverse**: Diversity penalty ensures consecutive runs produce different aesthetics (kawaii, grunge, minimalist, magazine, retro, etc.)

Use `--skip-cover` to fall back to the original highlight photo as hero.

## Capabilities

- Gemini 3 Pro multi-modal photo understanding (scene, mood, objects, narrative hooks)
- Multi-dimensional scoring (visual appeal, story value, emotion, uniqueness, technical quality)
- Diversity-optimized highlight selection (mood + location + scene variety)
- EXIF-based date extraction and orientation correction
- Theme-guided or auto-detected narrative generation
- AI cover image with 89-template style library and content-aware matching
- Triple output: HTML, rich text (Markdown), PNG composite
