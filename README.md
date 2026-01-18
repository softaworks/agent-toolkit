# Agent Skills

A curated collection of skills for AI coding agents. Skills are packaged instructions and scripts that extend agent capabilities across development, documentation, planning, and professional workflows.

Skills follow the [Agent Skills](https://agentskills.io/) format.

---

## 🧭 Quick Navigation

**[📚 Available Skills](#-available-skills)** • **[🚀 Installation](#-installation)** • **[📖 Skill Structure](#-skill-structure)** • **[🤝 Contributing](#-contributing)** • **[📄 License](#-license)** • **[🔗 Links](#-links)**

---

## 📚 Available Skills

### AI Tools & Integrations

#### [codex](skills/codex/README.md)
Use Codex CLI for advanced code analysis, refactoring, and automated editing with GPT-5.2 support.

**[→ Read more](skills/codex/README.md)**

#### [gemini](skills/gemini/README.md)
Leverage Gemini CLI for large-scale code review and analysis with 200k+ context windows.

**[→ Read more](skills/gemini/README.md)**

#### [perplexity](skills/perplexity/README.md)
Search the web and ask questions using Perplexity AI for up-to-date information and research.

**[→ Read more](skills/perplexity/README.md)**

---

### Documentation & Diagrams

#### [backend-to-frontend-handoff-docs](skills/backend-to-frontend-handoff-docs/README.md)
Create comprehensive API handoff documentation for frontend developers after backend implementation.

**[→ Read more](skills/backend-to-frontend-handoff-docs/README.md)**

#### [frontend-to-backend-requirements](skills/frontend-to-backend-requirements/README.md)
Document frontend data needs and API requirements for backend developers without dictating implementation.

**[→ Read more](skills/frontend-to-backend-requirements/README.md)**

#### [c4-architecture](skills/c4-architecture/README.md)
Generate C4 architecture diagrams (Context, Container, Component, Deployment) using Mermaid syntax.

**[→ Read more](skills/c4-architecture/README.md)**

#### [draw-io](skills/draw-io/README.md)
Create, edit, and convert draw.io diagrams with AWS icons, proper layouts, and PNG exports.

**[→ Read more](skills/draw-io/README.md)**

#### [excalidraw](skills/excalidraw/README.md)
Work with Excalidraw diagrams efficiently by delegating to specialized sub-agents to avoid token bloat.

**[→ Read more](skills/excalidraw/README.md)**

#### [marp-slide](skills/marp-slide/README.md)
Create professional presentation slides using Marp with 7 beautiful themes and automatic quality enhancement.

**[→ Read more](skills/marp-slide/README.md)**

#### [mermaid-diagrams](skills/mermaid-diagrams/README.md)
Create software diagrams using Mermaid: flowcharts, sequence diagrams, class diagrams, ERDs, and more.

**[→ Read more](skills/mermaid-diagrams/README.md)**

---

### Development Tools

#### [command-creator](skills/command-creator/README.md)
Create optimized Claude Code slash commands with proper structure, validation, and best practices.

**[→ Read more](skills/command-creator/README.md)**

#### [session-handoff](skills/session-handoff/README.md)
Create comprehensive handoff documents for seamless AI agent session transfers and continuity.

**[→ Read more](skills/session-handoff/README.md)**

#### [reducing-entropy](skills/reducing-entropy/README.md)
Minimize total codebase size through aggressive deletion and simplification. Manual activation only.

**[→ Read more](skills/reducing-entropy/README.md)**

---

### Design & Frontend

#### [design-system-starter](skills/design-system-starter/README.md)
Bootstrap complete design systems with design tokens, component architecture, and accessibility guidelines.

**[→ Read more](skills/design-system-starter/README.md)**

#### [openapi-to-typescript](skills/openapi-to-typescript/README.md)
Convert OpenAPI 3.0 specifications to TypeScript interfaces and runtime type guards.

**[→ Read more](skills/openapi-to-typescript/README.md)**

---

### Utilities

#### [domain-name-brainstormer](skills/domain-name-brainstormer/README.md)
Generate creative domain name ideas and check availability across multiple TLDs (.com, .io, .dev, .ai).

**[→ Read more](skills/domain-name-brainstormer/README.md)**

#### [meme-factory](skills/meme-factory/README.md)
Generate memes using memegen.link API with 100+ popular templates for social media and presentations.

**[→ Read more](skills/meme-factory/README.md)**

#### [web-to-markdown](skills/web-to-markdown/README.md)
Convert webpages to clean Markdown using Puppeteer and Readability for JavaScript-rendered content.

**[→ Read more](skills/web-to-markdown/README.md)**

---

### Planning & Strategy

#### [spec-forge](skills/spec-forge/README.md)
Create detailed, sectionized implementation plans through research, stakeholder interviews, and multi-LLM review.

**[→ Read more](skills/spec-forge/README.md)**

#### [game-changing-features](skills/game-changing-features/README.md)
Find 10x product opportunities and high-leverage improvements through strategic thinking frameworks.

**[→ Read more](skills/game-changing-features/README.md)**

#### [requirements-clarity](skills/requirements-clarity/README.md)
Clarify ambiguous requirements through systematic dialogue before implementation begins.

**[→ Read more](skills/requirements-clarity/README.md)**

---

### Professional Skills

#### [professional-communication](skills/professional-communication/README.md)
Master technical communication for emails, team messaging, meetings, and stakeholder updates.

**[→ Read more](skills/professional-communication/README.md)**

---

### Testing & Quality

#### [qa-test-planner](skills/qa-test-planner/README.md)
Generate comprehensive test plans, manual test cases, regression suites, and bug reports for QA engineers.

**[→ Read more](skills/qa-test-planner/README.md)**

---

### Git Workflow

#### [commit-work](skills/commit-work/README.md)
Create high-quality git commits with smart splitting, Conventional Commits format, and clear messages.

**[→ Read more](skills/commit-work/README.md)**

---

## 🚀 Installation

### Recommended: Universal Installation (Works with all AI agents)

```bash
npx add-skill leonardocouy/agent-skills
```

This method works with multiple AI coding agents:
- ✅ **Claude Code** - Full plugin support
- ✅ **Codex** - Compatible with skill format
- ✅ **Cursor** - Works with agent skills
- ✅ **Other Agent-based tools** - Universal compatibility

### Alternative Methods

**For Claude Code (Plugin)**
```bash
/plugin install agent-skills@leonardocouy
```

**For Claude Code (Manual)**
```bash
cp -r skills/<skill-name> ~/.claude/skills/
```

**For claude.ai**

Add skills to project knowledge or paste SKILL.md contents into the conversation.

---

## 📖 Skill Structure

Each skill contains:
- `SKILL.md` - Detailed instructions for the agent (with YAML frontmatter)
- `README.md` - User-friendly documentation with examples
- `scripts/` - Helper scripts for automation (optional)
- `references/` - Supporting documentation (optional)

---

## 🤝 Contributing

Contributions are welcome! When adding new skills:

1. Follow the [Agent Skills](https://agentskills.io/) format
2. Include both `SKILL.md` (for agents) and `README.md` (for users)
3. Add YAML frontmatter to `SKILL.md` with `name:` and `description:` fields
4. Update this README.md with a link to your skill

---

## 📄 License

MIT

---

## 🔗 Links

- [Agent Skills Format](https://agentskills.io/)
- [Claude Code Documentation](https://docs.anthropic.com/claude/docs)
- [GitHub Repository](https://github.com/leonardocouy/agent-skills)
