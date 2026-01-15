# Hytale Agent Skills

> **AI-powered skills for comprehensive Hytale modding** - Works with Claude AI in [Antigravity](https://antigravity.google) or any AI agent that supports the skills format.

## What is this?

A collection of **18 AI agent skills** that transform Claude (or compatible AI assistants) into expert Hytale modders. Just give this repo to your AI and start asking about modding - no tutorials needed.

**Skills cover:** Content creation, 3D modeling, Java plugins, animations, audio, VFX, UI/HUD, world generation, server hosting, and team collaboration.

## Quick Start

### Just Give the Link to Claude

Share this with your AI assistant:
```
https://github.com/Z3nLotus/hytale-agent-skills
```

Then ask things like:
- *"Create a new Hytale Pack called MyFirstMod"*
- *"Help me make a custom block with two states"*
- *"What are Hytale's art style guidelines?"*
- *"Set up a private server for my friends"*

The AI will clone the repo, read the skills, and guide you through everything.

### Manual Setup (Optional)

If your AI can't clone repos automatically, open the folder in [Antigravity](https://antigravity.google) or point your agent to `.agent/skills/`.

### AI-Assisted 3D Modeling (Optional)

Want Claude to directly control Blockbench? See the `blockbench-mcp` skill for MCP setup.

## Available Skills

| Skill | Description | Status |
|-------|-------------|--------|
| `skill-creator` | Meta skill for creating new skills (Apache 2.0, Anthropic) | ✅ Ready |
| `hytale-pack-creator` | Create Packs (blocks, items, JSON assets) | ✅ Ready |
| `hytale-model-creator` | Blockbench modeling with Hytale art style | ✅ Ready |
| `blockbench-mcp` | AI-assisted modeling via MCP protocol | ✅ Ready |
| `hytale-plugin-dev` | Java 25 plugin development | ✅ Ready |
| `java-25-hytale` | Modern Java features for Hytale | ✅ Ready |
| `gradle-hytale` | Gradle build system for plugins | ✅ Ready |
| `hytale-prefab-builder` | Structure/prefab creation | ✅ Ready |
| `hytale-texture-artist` | Pixel art textures | ✅ Ready |
| `hytale-ecs` | Entity Component System patterns | ✅ Ready |
| `hytale-animation` | Blockbench animation and rigging | ✅ Ready |
| `hytale-npc-ai` | NPC behavior, dialogue, and AI | ✅ Ready |
| `git-workflow` | Team collaboration and version control | ✅ Ready |
| `hytale-world-gen` | World generation and biome creation | ✅ Ready |
| `hytale-server-admin` | Server setup, config, and hosting | ✅ Ready |
| `hytale-audio` | Sound effects, music, and audio | ✅ Ready |
| `hytale-vfx` | Particles, effects, and trails | ✅ Ready |
| `hytale-ui` | Custom HUDs, menus, and interfaces | ✅ Ready |

## Skills Structure

```
.agent/
└── skills/
    ├── skill-creator/           # Meta skill (how to make more skills)
    │   ├── SKILL.md
    │   ├── scripts/
    │   └── references/
    ├── hytale-pack-creator/     # Pack/block/item creation
    │   ├── SKILL.md
    │   ├── assets/templates/    # JSON templates
    │   └── references/
    ├── hytale-model-creator/    # 3D modeling for Hytale
    │   ├── SKILL.md
    │   └── references/
    └── blockbench-mcp/          # AI-Blockbench integration
        └── SKILL.md
```

## Requirements

- **For using skills**: Any AI assistant that can read markdown files, ideally [Antigravity](https://antigravity.google) with Claude
- **For Hytale modding**: [Hytale](https://hytale.com/) game installed
- **For 3D modeling**: [Blockbench](https://www.blockbench.net/) with Hytale plugin
- **For MCP integration**: Node.js 18+, pnpm (see `blockbench-mcp` skill)

## How Skills Work

Skills use a **progressive disclosure** system:

1. **Metadata** (always loaded): Skill name + description (~100 words)
2. **SKILL.md body** (on trigger): Core instructions (<500 lines)
3. **References** (on demand): Detailed docs loaded when needed

This keeps context efficient while providing deep expertise when required.

## Contributing

Want to add a skill? Use the `skill-creator` skill! Ask your AI:

> "I want to create a new skill for [topic]. Help me design it following the skill-creator guidelines."

Then submit a pull request!

## Official Resources

- [Hytale Official Website](https://hytale.com/)
- [Hytale Modding Strategy](https://hytale.com/news/2025/11/hytale-modding-strategy-and-status)
- [Hytale Model Creation Guide](https://hytale.com/news/2025/12/an-introduction-to-making-models-for-hytale)
- [Blockbench](https://www.blockbench.net/)
- [Community Docs](https://hytalemodding.dev/en/docs)

## License

This repository is licensed under the **MIT License** (see [LICENSE](LICENSE)).

### Attribution

The `skill-creator` skill is adapted from [Anthropic's Skills Repository](https://github.com/anthropics/skills) and is licensed under **Apache 2.0**. See `.agent/skills/skill-creator/LICENSE.txt` for full terms.

All other skills in this repository are original works.

### Trademarks

Hytale is a trademark of Hypixel Studios. This project is not affiliated with or endorsed by Hypixel Studios.

---

**Made with ❤️ for the Hytale modding community**
