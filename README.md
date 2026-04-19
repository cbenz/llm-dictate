# tiny-dictate

Minimal desktop voice dictation tool: no daemon, no GUI, just keyboard shortcuts and notifications.

Opinionated workflow: press a keyboard shortcut to start recording, press it again to stop and transcribe (or cancel with another keyboard shortcut).

Based on the following tools:

- audio recording with `arecord`
- transcription with `llm groq-whisper`
- result pasted into active window via clipboard + `ydotool Shift+Insert`
- status notifications with `dunstify`

## Installation

### Dependencies

- `arecord` (alsa-utils)
- [dunst](https://dunst-project.org/)
- [llm](https://github.com/simonw/llm)
- [llm-groq-whisper](https://github.com/simonw/llm-groq-whisper) llm plugin
- [wl-clipboard](https://github.com/bugaevc/wl-clipboard)
- [ydotool](https://github.com/ReimuNotMoe/ydotool)

### Configure `llm` for Groq Whisper

```bash
llm install llm-groq-whisper
llm keys set groq
# Paste your Groq API key
```

### Install script

Install the script in your PATH, for example `~/.local/bin`:

```bash
install -m 755 tiny-dictate ~/.local/bin/tiny-dictate
```

### Configure keyboard shortcuts

Example with i3/sway:

```text
bindsym $mod+backslash exec ~/.local/bin/tiny-dictate toggle
bindsym $mod+Shift+backslash exec ~/.local/bin/tiny-dictate cancel
```

## Usage

```text
Usage: tiny-dictate <command>

Commands:
  start   Start recording
  stop    Stop recording and transcribe
  cancel  Cancel recording
  toggle  Start if idle, stop if recording
  status  Show status (idle or working)
```
