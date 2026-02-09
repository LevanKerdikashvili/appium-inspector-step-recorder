# Appium Inspector — Step Recorder Plugin

A custom [Appium Inspector](https://github.com/appium/appium-inspector) plugin with an integrated **Step Recorder** tab. Record, edit, organize, and replay mobile test steps directly from the inspector UI.

> **⚠️ Disclaimer:** This plugin was developed with the assistance of AI. Bugs and unexpected behavior are possible. This project is intended for **personal use only** and is **not intended for commercial purposes**.

> **Based on:** [appium-inspector](https://github.com/appium/appium-inspector) (Apache-2.0)

---

## Features

- **Record** — Auto-capture find, click, sendKeys, clear, tap, swipe actions
- **Edit** — Modify action type, element reference, locator, text, coordinates
- **Reorder** — Drag & drop steps to rearrange
- **Replay** — Run single step or all steps with visual progress
- **Skip** — Toggle steps to exclude from execution
- **Duplicate** — Clone any step
- **Wait** — Pause execution for N seconds
- **Call Folder** — Execute a saved folder's steps as a single step
- **Folders** — Organize, rename, move/copy step sets between folders
- **Export/Import** — JSON export/import + HTML report generation

---

## Installation

```bash
# Clone this repository
git clone https://github.com/LevanKerdikashvili/appium-inspector-step-recorder.git

# If you already have the official inspector plugin, uninstall it first
appium plugin uninstall inspector

# Install from the local clone (use absolute path)
appium plugin install --source=local ~/appium-inspector-step-recorder

# Start Appium with the plugin enabled
appium --use-plugins=inspector
```

Then open your browser at: **http://localhost:4723/inspector**

---

## How to Use

### Getting Started

1. Start an Appium session
2. Find the **"Step Recorder"** tab in the Session Inspector
3. The tab has two sub-tabs: **Steps** and **Folders**

### Recording

- Click **⏺ Record** → interact with your app → actions are captured automatically
- Click **⏹ Stop** to finish recording
- Supported: Find, Click, SendKeys, Clear, Tap, Swipe

### Editing & Managing Steps

- **Edit**: Click ✏️ on any step to modify its action, element, text, or coordinates
- **Reorder**: Drag steps by the ⠿ handle
- **Skip**: Click 👁 to toggle skip
- **Duplicate**: Click 📋 on a step
- **Add manually**: Click + to insert a new step (Find, Click, SendKeys, Tap, Swipe, Wait, Call Folder)

### Running Steps

- **Single step**: Click ▶ on any step
- **All steps**: Click ▶ Run All — steps show blue (running), green (passed), red (failed)
- **Stop**: Click ⏹ to abort

### Wait & Call Folder

- **Wait**: Pauses execution for a configurable number of seconds
- **Call Folder**: Executes all steps from a saved folder as one step — useful for reusable flows (e.g., Login)

### Folders

- Create folders, save step sets, load/rename/move/copy/run them
- **Run**: Click ▶ on a saved step set to execute it directly
- **Rename**: Click ✏️ on folder or step set name

### Export & Import

- **JSON**: Export/import step collections as `.json` files
- **HTML Report**: Generate a styled HTML report of current steps

---

## License

[Apache-2.0](https://github.com/appium/appium-inspector/blob/main/LICENSE)

Based on [Appium Inspector](https://github.com/appium/appium-inspector) by the Appium team.
