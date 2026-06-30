# Install — Token Saver

## 1. Install RTK

### macOS (Homebrew)
```bash
brew install rtk
```

### Linux/macOS (script)
```bash
curl -fsSL https://raw.githubusercontent.com/rtk-ai/rtk/refs/heads/master/install.sh | sh
```

### Windows
1. Download `rtk-x86_64-pc-windows-msvc.zip` from [releases](https://github.com/rtk-ai/rtk/releases)
2. Extract `rtk.exe` to a folder in your PATH (e.g. `C:\Users\<you>\.local\bin`)
3. Verify: `rtk --version`

### Cargo
```bash
cargo install --git https://github.com/rtk-ai/rtk
```

## 2. Install this Skill

### OpenCode
Place `SKILL.md` where your AI tool can find it, or load via:

```bash
skill("rtk")
```

Or install the OpenCode plugin (auto-rewrite):
```bash
rtk init -g --opencode
```

### Claude Code
```bash
rtk init -g
```

### Codex (OpenAI)
```bash
rtk init -g --codex
```

### Cursor
```bash
rtk init -g --agent cursor
```

### Gemini CLI
```bash
rtk init -g --gemini
```

### Windsurf
```bash
rtk init -g --agent windsurf
```

## 3. Verify

```bash
rtk --version   # should show v0.34.x+
rtk gain        # should show stats
```
