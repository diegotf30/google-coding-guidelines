# google-coding-guidelines

A language-agnostic code style skill for Claude and other agents. It condenses the [Google C++ Style Guide](https://google.github.io/styleguide/cppguide.html) into rules that apply to any language: naming, comments, scope, functions, ownership, errors, and formatting.

The skill is one file: [skills/google-coding-guidelines/SKILL.md](skills/google-coding-guidelines/SKILL.md).

## Install

In Claude Code, add this repository as a plugin marketplace, then install the plugin:

```
/plugin marketplace add diegotf30/google-coding-guidelines
/plugin install google-coding-guidelines@google-coding-guidelines
```

To install it by hand, copy `skills/google-coding-guidelines` into `~/.claude/skills/` (all projects) or `.claude/skills/` (one project).

For other agents, paste the body of `SKILL.md` into the system prompt or the project's agent instructions file (`AGENTS.md`, `CLAUDE.md`).

## License

The content is derived from the Google C++ Style Guide and is licensed under [CC BY 3.0](https://creativecommons.org/licenses/by/3.0/), the same license as the source. See [LICENSE](LICENSE).
