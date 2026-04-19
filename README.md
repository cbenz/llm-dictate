# llm-dictate

[Demo video](https://www.youtube.com/watch?v=bk6RkUvzxaw)

Minimal desktop toolkit with two scripts:

- `llm-dictate`: voice dictation (start/stop/cancel + transcription)
- `llm-text-transform`: transform selected text with an LLM prompt menu

No daemon, no server.

## Included scripts

### 1) `llm-dictate`

Opinionated speech-to-text (STT) workflow:

- audio recording with `arecord` (16 kHz mono WAV)
- transcription with `llm groq-whisper`
- result pasted into active window via clipboard + `ydotool Shift+Insert`
- status notifications with `dunstify`

Typical flow:

- run `llm-dictate toggle` to start recording
- run `llm-dictate toggle` again to stop and transcribe
- optional `llm-dictate cancel` to abort recording

### 2) `llm-text-transform`

Text transformation from primary selection:

- reads text from the PRIMARY clipboard
- lists prompt files from `~/.agents/prompts/desktop` via `fzf-menu`
- sends selected prompt + text to `llm`
- writes result to CLIPBOARD

Useful for translating, rewriting, summarizing, fixing grammar, etc.

## Shared bash module

`src/llm-desktop-shared.sh` provides shared functions used by both scripts:

- dependency checks
- mako notification replacement/closing
- spinner animation
- clipboard abstraction (`wl-copy` and `wl-paste`)
- integrated `fzf_menu` helper (xterm + fzf)
- short error formatting helpers

Keep this file next to the executable scripts.

## Installation

### Dependencies

Common:

- `llm`
- `dunstify` (from dunst)
- `wl-copy` and `wl-paste` (from wl-clipboard)

For dictation (`llm-dictate`):

- `arecord` (alsa-utils)
- `ydotool`
- `llm-groq-whisper` plugin

For text transform (`llm-text-transform`):

- `xterm`
- `fzf`

### Configure `llm` for Groq Whisper

```bash
llm install llm-groq-whisper
llm keys set groq
# Paste your Groq API key
```

### Install scripts (important with shared file)

Install all three files from `src/` together in the same directory, for example `~/.local/bin`:

```bash
install -m 755 src/llm-dictate ~/.local/bin/llm-dictate
install -m 755 src/llm-text-transform ~/.local/bin/llm-text-transform
install -m 644 src/llm-desktop-shared.sh ~/.local/bin/llm-desktop-shared.sh
```

Why: both executables rely on `llm-desktop-shared.sh` from their own directory.

### Optional: keyboard shortcuts

Example with i3:

```text
bindsym $mod+backslash exec --no-startup-id ~/.local/bin/llm-dictate toggle
bindsym $mod+Shift+backslash exec --no-startup-id ~/.local/bin/llm-dictate cancel
```

You can also bind `~/.local/bin/llm-text-transform` to a shortcut.

## Usage

`llm-dictate`:

```text
Usage: llm-dictate <command>

Commands:
  start   Start recording
  stop    Stop recording and transcribe
  cancel  Cancel recording
  toggle  Start if idle, stop if recording
  status  Show status (idle or working)
```

`llm-text-transform`:

- select text in an app (PRIMARY selection)
- run `llm-text-transform`
- choose a prompt in the menu
- paste the result from CLIPBOARD where needed
