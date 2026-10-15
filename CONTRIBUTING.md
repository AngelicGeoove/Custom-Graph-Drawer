# Contributing

Thanks for trying Node Graph Viewer! This is a small single-file project by Angelic Groove, built with assistance from Anthropic's Claude.

## Quick start

1. Open `index.html` directly in a browser — no build step.
2. Try `examples/minimal.txt` and `examples/shader-graph.txt`.
3. Edit, press **Draw**, check warnings at the bottom of the input pane.

## What to contribute

- Verified templates for more software (Blender, Unity, Unreal, Material Maker, etc.)
- Bug reports with pasted input text that fails to parse
- Small UX / accessibility fixes (keep it zero-dependency, single-file)

## Ground rules

- Keep `index.html` single-file. No npm, no bundler, no external assets.
- Keep it working from `file://` and GitHub Pages.
- Don't commit personal data from `localStorage` exports.
- Disclose AI assistance in PRs if you used it — this project does.
