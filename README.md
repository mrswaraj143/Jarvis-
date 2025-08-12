# JarvisAI-YouTube (Python Voice Assistant)

A lightweight voice‑controlled personal assistant inspired by Iron Man's Jarvis. It uses speech recognition for voice input, OpenAI for conversational responses, and simple system automation to open websites, tell time, and more.

> Based on the tutorial referenced here: [YouTube walkthrough](https://youtu.be/Z3ZAJoi4x6Q).

---

## Features

- Natural language conversation via OpenAI completions (context‑aware chat)
- Voice commands to open popular sites (YouTube, Wikipedia, Google)
- Utility commands: tell the current time, play a local music file
- Simple app launching (macOS examples included)
- Saves AI outputs for certain prompts to local files

---

## Project Structure

```
JarvisAI-YouTube-main/
  ├─ config.py          # Holds your OpenAI API key
  ├─ main.py            # Core voice assistant app (speech + actions + AI chat)
  └─ openaitest.py      # Minimal OpenAI API test script
```

---

## Tech Stack

- Python 3.8+
- SpeechRecognition (Google Web Speech API backend)
- OpenAI Python SDK
- Standard library: `webbrowser`, `os`, `datetime`

> Note: `numpy` is imported in `main.py` but not used; it can be removed if desired.

---

## Setup

### 1) Clone and create a virtual environment (recommended)

```bash
# Clone
git clone <this-repo-url>
cd JarvisAI-YouTube-main

# Create and activate venv (Windows PowerShell)
python -m venv .venv
. .venv\Scripts\Activate.ps1

# macOS/Linux
# python3 -m venv .venv
# source .venv/bin/activate
```

### 2) Install dependencies

```bash
pip install --upgrade pip
pip install speechrecognition openai numpy

# Microphone support requires PyAudio
# Windows (easiest route):
pip install pipwin
pipwin install pyaudio

# macOS (with Homebrew):
# brew install portaudio
# pip install pyaudio

# Linux (Debian/Ubuntu):
# sudo apt-get install portaudio19-dev python3-pyaudio
# pip install pyaudio
```

### 3) Configure your OpenAI API key

Edit `config.py` and replace the placeholder with your real key:

```python
# config.py
apikey = "sk-...your-key-here..."
```

> Security tip: For production, prefer environment variables over hardcoding keys.

---

## Run

- Start the assistant:

```bash
python main.py
```

- Test OpenAI connectivity only:

```bash
python openaitest.py
```

You should hear a greeting and see “Listening...” in the console. Speak a command into your microphone.

---

## Voice Commands (examples)

- "Open YouTube" / "Open Google" / "Open Wikipedia"
- "Open music" (plays a hardcoded local mp3 path; update the path in `main.py`)
- "What is the time" / "the time"
- "Using artificial intelligence ..." (triggers AI text generation saved to file)
- "Reset chat" (clears conversation history)
- "Jarvis Quit" (exits)

Anything else is treated as a chat prompt and answered via OpenAI.

> Recognition language defaults to Indian English `en-in`. Change to `en-US` (or your locale) in `takeCommand()` if needed.

---

## Platform Notes (TTS and app launching)

The current `say(text)` implementation uses the macOS `say` command:

```python
os.system(f'say "{text}"')
```

- Windows: Replace `say()` with a TTS library like `pyttsx3` or use PowerShell’s SpeechSynthesizer, e.g.
  ```python
  import subprocess
  def say(text):
      script = f"Add-Type -AssemblyName System.speech; ($speak = New-Object System.Speech.Synthesis.SpeechSynthesizer).Speak(\"{text}\");"
      subprocess.call(["powershell", "-Command", script])
  ```
- Linux: Use `espeak`, `spd-say`, or a TTS library (e.g., `pyttsx3`).

Additionally, app‑launch examples like FaceTime and Passky are macOS‑specific. Remove or replace these with apps available on your OS.

---

## Configuration and Paths

- Music file path (`open music`) is hardcoded in `main.py`. Update it for your machine.
- Website list is in `sites` (array of name + URL). Add your own entries.
- Speech language is set to `en-in`. Change the `language` argument of `recognize_google()` as desired.

---

## Troubleshooting

- PyAudio install issues (Windows): use `pipwin install pyaudio`.
- Microphone not detected: ensure the default input device is active and accessible by Python.
- Google recognition errors: intermittent network hiccups are common; try again or add retries.
- No voice output on Windows/Linux: update `say()` per the Platform Notes section.

---

## Important Note on OpenAI Models

This project uses `openai.Completion.create` with the `text-davinci-003` model as in the original tutorial. Newer OpenAI SDKs and models favor the Chat Completions API (e.g., `gpt-3.5-turbo`, `gpt-4o`). If you upgrade, you’ll need to adapt the code accordingly.

---

## Roadmap (Ideas)

- Migrate to Chat Completions API with streaming responses
- Cross‑platform TTS built into the code (no OS shelling)
- Richer intent parsing and more commands (play specific songs, open apps dynamically)
- Better error handling and retry logic for speech and network
- Store API keys in environment variables and load via `os.getenv`

---



