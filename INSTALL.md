# Install — Token Saver

## 1. Install RTK

> ⚠️ **Name collision warning:** Another project named "rtk" (Rust Type Kit) exists on crates.io.
> If `rtk gain` fails after install, you have the wrong package.
> Use `cargo install --git https://github.com/rtk-ai/rtk` (the `--git` flag) instead of `cargo install rtk`.

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
rtk --version   # should show a version (any v0.x)
rtk gain        # should show stats — if error, you have the wrong rtk (see warning above)
```
