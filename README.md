# Party Mode Plugin

Multi-agent conversation orchestration for Claude Code - enables dynamic group discussions with 10 AI agents, each with unique personality and expertise.

## Features

- **10 Specialized Agents**: Business analysts, architects, developers, testers, and more
- **Intelligent Agent Selection**: Automatically selects 2-3 relevant agents per topic
- **Character Consistency**: Each agent maintains their unique voice and principles
- **Cross-Agent Dialogue**: Agents can reference and build on each other's responses
- **Knowledge Extension**: Agents like Murat (Test Architect) can load domain-specific knowledge
- **Graceful Exit**: Proper session closure with agent farewells

## Agents

| Icon | Name | Role | Specialty |
|------|------|------|-----------|
| 🧙 | BMad Master | Coordinator | Platform expertise, moderation |
| 📊 | Mary | Business Analyst | Requirements, market research |
| 🏗️ | Winston | Architect | System design, "boring technology" |
| 💻 | Amelia | Developer | TDD, implementation, precision |
| 📋 | John | Product Manager | Strategy, "WHY?", prioritization |
| 🚀 | Barry | Quick Flow Dev | Rapid prototyping, efficiency |
| 🏃 | Bob | Scrum Master | Agile process, story prep |
| 🧪 | Murat | Test Architect | Quality, CI/CD, risk analysis |
| 📚 | Paige | Technical Writer | Documentation, clarity |
| 🎨 | Sally | UX Designer | User experience, empathy |

## Usage

Start a Party Mode session:

```
/bmad-party-mode
```

Or with a topic:

```
/bmad-party-mode How should we design our authentication system?
```

### During the Session

- Ask any question - relevant agents will respond
- Name a specific agent to prioritize their response
- Agents will naturally debate and build on each other

### Ending the Session

Say any of these to exit:
- `*exit`
- `goodbye`
- `end party`
- `quit`

## Architecture

```
bmad-party-mode/
├── .claude-plugin/
│   └── plugin.json          # Plugin manifest
├── commands/
│   └── bmad-party-mode.md   # Main /bmad-party-mode command
├── skills/
│   └── party-mode-orchestration/
│       ├── SKILL.md         # Orchestration rules
│       └── references/
│           ├── agent-selection.md
│           └── conversation-rules.md
├── data/
│   ├── agent-index.json     # Lightweight agent index
│   ├── agents/              # Full agent profiles (loaded on-demand)
│   │   ├── architect.json
│   │   ├── analyst.json
│   │   └── ...
│   └── knowledge/           # Extensible knowledge bases
│       └── tea/
│           ├── index.json
│           └── testing-strategy.md
└── README.md
```

## Extending Knowledge

To add knowledge for an agent:

1. Create a knowledge index in `data/knowledge/{agent-id}/index.json`
2. Add markdown files for each topic
3. Update the agent's profile to reference the knowledge config

Example for adding Playwright knowledge to Murat:

```json
// data/knowledge/tea/index.json
{
  "topics": [
    {
      "id": "playwright",
      "name": "Playwright Testing",
      "keywords": ["playwright", "e2e", "browser"],
      "file": "playwright.md"
    }
  ]
}
```

## Configuration

The plugin uses split architecture for efficiency:

- **agent-index.json**: Lightweight (~1KB) - loaded at session start
- **Agent profiles**: Full data (~500B each) - loaded only for selected agents
- **Knowledge files**: Loaded only when topic matches

## License

MIT
