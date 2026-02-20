# pythonic-deslop

A [Claude Code skill](https://docs.anthropic.com/en/docs/claude-code/skills) that prevents LLMs from writing sloppy Python. Targets the recognizable "AI house style" — over-classing, defensive try/except everywhere, annotation noise, premature abstraction — and steers toward clean, idiomatic code instead.

## Install

Clone into your Claude Code skills directory:

```sh
git clone https://github.com/jbohnslav/pythonic-deslop ~/.claude/skills/pythonic-deslop
```

Or if you'd rather keep the repo elsewhere and symlink:

```sh
git clone https://github.com/jbohnslav/pythonic-deslop ~/code/pythonic-deslop
ln -s ~/code/pythonic-deslop ~/.claude/skills/pythonic-deslop
```

Claude will automatically load it when writing Python, or you can invoke it manually with `/pythonic-deslop`.

## License

Apache 2.0
