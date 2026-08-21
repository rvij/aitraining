# IDE Setup Instructions

Configure your editor with an AI coding assistant before starting the exercises. This guide covers VS Code, JetBrains IDEs, and Neovim.

## Checklist

- [ ] I have installed the AI extension for my IDE
- [ ] I have authenticated with my AI provider
- [ ] Autocomplete is working in a test file
- [ ] I can open the chat/assistant pane

## VS Code

Install one of the following extensions from the Extensions panel (`⌘⇧X`):

- **Claude Code** — `anthropics.claude-code`
- **GitHub Copilot** — `github.copilot`
- **Cursor** — use the Cursor fork of VS Code instead

```bash
# Install via CLI
code --install-extension anthropics.claude-code
```

!!! tip
    Sign in via the extension's accounts panel — most providers use OAuth so no API key is needed.

## JetBrains

Go to **Settings → Plugins → Marketplace** and search for your assistant. JetBrains AI Assistant is built-in for licensed users.

## Neovim

```lua
-- lazy.nvim
{
  "zbirenbaum/copilot.lua",
  cmd = "Copilot",
  event = "InsertEnter",
  config = function()
    require("copilot").setup({
      suggestion = { auto_trigger = true }
    })
  end,
}
```

## Verifying Setup

Create a new file `test_hello.py` and type:

```python
# Return the sum of a list of numbers
def
```

Your assistant should autocomplete the function. If it doesn't, check your authentication and that the extension is enabled for the workspace.
