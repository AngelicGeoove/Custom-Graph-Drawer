# Node Graph Viewer

![single-file](https://img.shields.io/badge/single--file-index.html-blue) ![no-build](https://img.shields.io/badge/no%20build-step-green) ![license](https://img.shields.io/badge/license-MIT-lightgrey)

A single-file, zero-dependency web app that draws node-to-node connections from plain text or JSON. It works for any node-based software (Shader Graph, Blender Geometry Nodes, Material Maker, or your own), because nodes, ports, settings and parameters are all defined in the input.

It was built so an AI agent (or a person) can describe a node graph as text and you can see the wiring drawn out, instead of reading long lists of connections.

> Built with assistance from Anthropic's Claude. Author: Angelic Groove.

**Open `index.html` in a browser, or host it with GitHub Pages (see below).** There is no build step.

Try it: paste `examples/minimal.txt` or `examples/shader-graph.txt` into the app and press **Draw**.

## Features

- Paste text or JSON and the graph is drawn with named input and output ports and curved wires
- Automatic left-to-right layout, then drag nodes, pan, and pinch or scroll to zoom
- Tap a node to highlight everything feeding into it and everything it feeds, and open the inspector
- **Node properties** (settings shown under the title) and **port default values** (shown while an input is unconnected)
- **Parameters** (exposed variables): floats, ints, bools, vectors, colors, textures, with ranges. They appear as typed nodes and in a parameter list that shows where each is used
- **Templates**: pick a node library (Unity Shader Graph, Blender Geometry Nodes, Material Maker starter) and declare nodes by type only; ports fill in automatically. Save your own templates from the current graph
- **Reference button**: copies the active template's node list plus a short syntax guide, ready to paste to an agent so it uses exact node and port names
- JSON export of the current graph
- Warnings for unreadable lines, undeclared ports, multiply-connected inputs, unknown node types and unused parameters
- Works on phones (touch pan, pinch zoom, tabbed layout) and respects light and dark mode
- Saves your text and custom templates in the browser's `localStorage`

## Text format

```
# Comments start with # or //

# Parameter:   param id "Label": type = default {setting: value}
param dmg "Damage Amount": float = 0 {min: 0, max: 1}
param tint "Tint Color": color = #e0204a

# Node:        id "Label" {Setting: Value}: In, In=default -> Out, Out
uv "Screen Position" {Mode: Default}: -> Out
mul "Multiply": A, B=0.5 -> Out
frag "Fragment": Base Color, Alpha=1 ->

# Connections: id.Port -> id.Port   (a parameter can be just: id -> node.Port)
uv.Out -> mul.A
dmg -> mul.B
mul.Out -> frag.Base Color
```

Rules:

- `id` has no spaces or dots. Port names may contain spaces.
- Inputs go left of `->`, outputs go right. `Name=value` sets a default for an input.
- A node referenced in a connection but never declared is created automatically.
- With a template selected, a node can be declared by type only: `m1 "Multiply"`, `m1: Multiply`, or with settings `m1 "Multiply" {Mode: X}`.

## JSON format

```json
{
  "parameters": [
    { "id": "dmg", "label": "Damage Amount", "type": "float", "value": 0,
      "props": { "min": 0, "max": 1 } }
  ],
  "nodes": [
    { "id": "mul", "label": "Multiply", "props": { "Mode": "Default" },
      "inputs": ["A", { "name": "B", "value": 0.5 }], "outputs": ["Out"] },
    { "id": "add", "type": "Add" }
  ],
  "edges": [["dmg", "mul.B"], ["mul.Out", "add.A"]]
}
```

- Inputs and outputs are port names, or `{ "name", "value" }` for an input default.
- A node with only `type` (no inputs or outputs) takes its ports from the active template.
- Edges are `["from.Port", "to.Port"]`. A parameter source can be just its id.
- The **JSON** button in the app exports the current graph in this format.

## Templates

A template is a list of node types in the same line format as node declarations, grouped by `##` category lines:

```
## Math
Add: A, B -> Out
Lerp: A, B, T -> Out
## Outputs
Fragment (Unlit): Base Color, Alpha ->
```

Built-in templates live in the `BUILTIN` object in `index.html`. Custom ones are stored in `localStorage`. Built-ins: Unity Shader Graph, Blender Geometry Nodes, Blender Shader Nodes, Material Maker. Material Maker ports are simplified from the official docs (`rodzill4.github.io/material-maker/doc`, `github.com/RodZill4/material-maker` `library/base.json`) — verify against your version. Blender Shader ports follow `docs.blender.org/manual` socket names.

## Deploy with GitHub Pages

1. Push these files to a repository.
2. Go to **Settings → Pages**.
3. Under **Build and deployment**, set the source to **Deploy from a branch**, branch `main`, folder `/ (root)`.
4. After a minute the app is live at `https://<your-username>.github.io/<repo-name>/`.

## Acknowledgements

Created by Angelic Groove with assistance from Anthropic's Claude (AI-assisted design, parsing, layout, and docs). Human-reviewed and tested before release.

## Ideas for later

- Groups / frames and comment boxes
- Typed ports with type-colored wires and type-mismatch warnings
- Export to SVG / PNG
- Import and export templates as files
- Saving node positions
- Verified, complete templates for more software

## License

MIT. See `LICENSE`.
