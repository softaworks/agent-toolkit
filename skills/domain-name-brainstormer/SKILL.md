---
name: domain-name-brainstormer
description: Generates creative domain name suggestions and checks availability across TLDs (.com, .io, .dev, .ai, .app, .co). Use when the user asks to brainstorm domain names, find a website name, search for available domains, register a domain, pick a name for a new project, startup, product, or personal brand.
---

# Domain Name Brainstormer

Generate creative, brandable domain name ideas and check availability across multiple TLDs for projects, startups, products, and personal brands.

## Workflow

1. **Gather context**: Ask the user about the project purpose, target audience, preferred keywords, and any TLD preferences
2. **Generate candidates**: Produce 10-15 domain name ideas using these strategies:
   - Combine relevant keywords (e.g., "code" + "clip" = codeclip)
   - Use portmanteaus, abbreviations, or invented words
   - Keep names short (<15 chars), pronounceable, no hyphens
   - Consider industry conventions (e.g., .dev/.io for developer tools, .ai for ML products)
3. **Check availability**: For each candidate, check availability across requested TLDs using `whois` or DNS lookup:
   ```bash
   # Quick availability check via whois
   whois example.com | grep -i "no match\|not found\|available"
   # Alternative: DNS-based check
   dig +short example.com
   ```
4. **Rank and present**: Present results grouped by availability, with a top pick and runner-up including reasoning
5. **Iterate**: If top choices are taken, generate variations (prefixes, suffixes, alternate TLDs) and re-check

## Output Format

Present results using this structure:

```
## Available Domains
1. snippetbox.com - Directly describes the product, easy to remember
2. snippet.dev - .dev extension signals developer tool, short

## Taken / Premium
- codeshare.com (taken)
- snippets.com (premium)

## Top Pick: snippet.dev
- Short, memorable, audience-appropriate TLD

## Next Steps
- Register your preferred choice
- Want me to check more variations or alternative keywords?
```

## Examples

**Basic request**: "Suggest domain names for a project management tool for remote teams"
- Generate: teamflow.com, remotedesk.io, taskpulse.dev, asyncwork.com, etc.

**With constraints**: "I need a short .ai domain for my writing assistant"
- Focus on: writly.ai, prosecraft.ai, draftai.ai, etc.

**Keyword-based**: "Use 'pixel' or 'studio' for my design agency"
- Generate: pixelstudio.com, studiopixel.design, pixelcraft.io, etc.

**Personal brand**: "Domain for my name John Smith, software engineer"
- Generate: johnsmith.dev, jsmith.io, smithcode.com, etc.

## Validation Checklist

Before presenting final suggestions, verify each name is:
- [ ] Under 15 characters
- [ ] Easy to spell and pronounce
- [ ] Free of trademark conflicts (search USPTO if uncertain)
- [ ] Available on the checked TLD
- [ ] Not easily confused with established brands

