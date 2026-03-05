# Contributing to Softaworks Agent Skills

Thank you for your interest in contributing! Here's how to get started.

## Adding a New Skill

1. Create a new directory under the appropriate category
2. Follow the [Agent Skills](https://agentskills.io/) format
3. Include a `SKILL.md` with clear instructions
4. Add relevant scripts and assets

## Development Setup

```bash
# Clone the repository
git clone https://github.com/softaworks/agent-toolkit.git
cd agent-toolkit

# Test a skill locally
npx skills add . --skill <skill-name>
```

## Pull Request Process

1. **Fork** the repository
2. **Create a branch** from `main`: `git checkout -b feat/my-skill`
3. **Follow existing patterns** — look at how current skills are structured
4. **Test your changes** locally with Claude Code or another compatible agent
5. **Submit a PR** with a clear description of what the skill does

## Code Style

- Use clear, descriptive names for skills and scripts
- Include documentation in each skill's `SKILL.md`
- Keep scripts focused and well-commented

## License

By contributing, you agree that your contributions will be licensed under the project's existing license.
