---
name: marp-slide
description: Create professional Marp Markdown presentation slides with 7 built-in themes (default, minimal, colorful, dark, gradient, tech, business). Use when users request Marp slides, Markdown-based presentations, or Marp documents. Outputs .md files with embedded CSS themes, image layouts, and automatic quality improvements. Prefer over PowerPoint/Google Slides when user wants Markdown-native, version-controllable presentations.
---

# Marp Slide Creator

Create professional, visually appealing Marp presentation slides with 7 pre-designed themes and built-in best practices.

## Available Themes

| Theme | Template | Best For |
|-------|----------|----------|
| Default | `template-basic.md` | General seminars, lectures |
| Minimal | `template-minimal.md` | Academic, content-focused |
| Colorful | `template-colorful.md` | Creative events, youth |
| Dark | `template-dark.md` | Tech talks, evening events |
| Gradient | `template-gradient.md` | Visual-focused, creative |
| Tech | `template-tech.md` | Dev tutorials, tech meetups |
| Business | `template-business.md` | Corporate, proposals |

For detailed theme colors, styles, and selection guidance, read `references/theme-selection.md`.

## Workflow

1. **Understand requirements** - Identify content, target audience, and formality level
2. **Select theme** - Match content type to theme (see quick selection below); if uncertain, consult `references/theme-selection.md`; default to Default theme
3. **Read references** - Always start with `references/marp-syntax.md`; add `references/image-patterns.md` for images, `references/advanced-features.md` for math/emoji, `references/theme-css-guide.md` for custom themes
4. **Apply template** - Copy from appropriate `assets/template-*.md` file (CSS is already embedded)
5. **Structure content** - Title slide with `<!-- _class: lead -->` + h1; content slides with h2 + 3-5 bullet points; keep titles concise (5-7 chars in Japanese); keep text to 15-25 chars per line
6. **Add images** - Use Marp syntax: `![bg right:40%](image.png)` for side images, `![bg](image.png)` for full background
7. **Validate** - Run through the Quality Checklist below before delivering
8. **Output** - Save to `/mnt/user-data/outputs/` with descriptive `.md` filename

**Quick theme selection:**
- **Technical/Developer** → tech | **Business/Corporate** → business | **Creative/Event** → colorful or gradient
- **Academic/Simple** → minimal | **General/Unsure** → default | **Dark background** → dark or tech

## Handling "Make It Look Good" Requests

When users give vague instructions like "良い感じにして", "かっこよく", or "make it cool":

1. **Infer theme from content**:
   - Business content → business theme
   - Technical content → tech or dark theme
   - Creative content → gradient or colorful theme
   - General → default theme

2. **Apply best practices automatically**:
   - Shorten titles to 5-7 characters
   - Limit bullet points to 3-5 items
   - Add adequate whitespace
   - Use consistent structure

3. **Enhance visual hierarchy**:
   - Use h3 for sub-sections when appropriate
   - Break up dense text into multiple slides
   - Ensure logical flow (intro → body → conclusion)

4. **Maintain professional tone**:
   - Match formality to content
   - Use parallel structure in lists
   - Keep technical terms consistent

## Image Integration

For slides with images, consult `references/image-patterns.md` for detailed syntax.

Common patterns:
- **Side image**: `![bg right:40%](image.png)` - Image on right, text on left
- **Centered**: `![w:600px](image.png)` - Centered with specific width
- **Full background**: `![bg](image.png)` - Full-screen background
- **Multiple images**: Multiple `![bg]` declarations

Example lecture pattern:
```markdown
## Slide Title

![bg right:40%](diagram.png)

- Explanation point 1
- Explanation point 2
- Explanation point 3
```

## Quality Checklist

Before delivering slides, verify:
- [ ] Theme selected appropriately for content
- [ ] CSS theme is embedded in the file
- [ ] Title slide uses `<!-- _class: lead -->`
- [ ] All h2 titles are concise (5-7 chars)
- [ ] Bullet points are 3-5 items per slide
- [ ] Images use proper Marp syntax
- [ ] File saved to outputs directory
- [ ] Content follows best practices

## References

### Core Documentation
- **Marp syntax**: `references/marp-syntax.md` - Basic Marp/Marpit syntax (directives, frontmatter, pagination, etc.)
- **Image patterns**: `references/image-patterns.md` - Official image syntax (bg, filters, split backgrounds)
- **Theme CSS guide**: `references/theme-css-guide.md` - How to create custom themes based on Marpit specification
- **Advanced features**: `references/advanced-features.md` - Math, emoji, fragmented lists, Marp CLI, VS Code
- **Official themes**: `references/official-themes.md` - default, gaia, uncover themes documentation

### Quality & Selection Guides
- **Theme selection**: `references/theme-selection.md` - How to choose the right theme for content
- **Best practices**: `references/best-practices.md` - Quality guidelines for "cool" slides

### Templates & Assets
- **Templates**: `assets/template-*.md` - Starting points with embedded CSS for each theme (7 themes)
- **Standalone CSS**: `assets/theme-*.css` - CSS files for reference (already embedded in templates)

### Official External Links
- **Marp Official Site**: https://marp.app/
- **Marpit Directives**: https://marpit.marp.app/directives
- **Marpit Image Syntax**: https://marpit.marp.app/image-syntax
- **Marpit Theme CSS**: https://marpit.marp.app/theme-css
- **Marp Core GitHub**: https://github.com/marp-team/marp-core
- **Marp CLI GitHub**: https://github.com/marp-team/marp-cli
