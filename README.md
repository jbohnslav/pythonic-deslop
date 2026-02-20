# pythonic-deslop

An [Agent Skill](https://agentskills.io) that prevents LLMs from writing sloppy Python. Targets the recognizable "AI house style" — over-classing, defensive try/except everywhere, annotation noise, premature abstraction — and steers toward clean, idiomatic code instead.

Works with any agent that supports the [open skill format](https://agentskills.io/specification): Claude Code, Cursor, Gemini CLI, Amp, Goose, Roo Code, and others.

## Install

Clone into your agent's skills directory. For example, with Claude Code:

```sh
git clone https://github.com/jbohnslav/pythonic-deslop ~/.claude/skills/pythonic-deslop
```

Or symlink from wherever you keep it:

```sh
git clone https://github.com/jbohnslav/pythonic-deslop ~/code/pythonic-deslop
ln -s ~/code/pythonic-deslop ~/.claude/skills/pythonic-deslop
```

For other agents, place or link the repo in the appropriate skills directory — see your agent's docs for the path.

## License

Apache 2.0
