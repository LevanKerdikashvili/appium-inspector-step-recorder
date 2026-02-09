# Appium Inspector — Step Recorder Plugin

A custom [Appium Inspector](https://github.com/appium/appium-inspector) plugin with an integrated **Step Recorder** tab. Record, edit, organize, and replay mobile test steps directly from the inspector UI.

> **Based on:** [appium-inspector](https://github.com/appium/appium-inspector) (Apache-2.0)

---

## Table of Contents

- [Features](#features)
- [Installation](#installation)
  - [Option 1: Install from GitHub (Appium Plugin)](#option-1-install-from-github-appium-plugin)
  - [Option 2: Replace existing plugin manually](#option-2-replace-existing-plugin-manually)
- [How to Use](#how-to-use)
  - [Getting Started](#getting-started)
  - [Recording Steps](#recording-steps)
  - [Step Table](#step-table)
  - [Action Types](#action-types)
  - [Editing Steps](#editing-steps)
  - [Running Steps](#running-steps)
  - [Drag & Drop Reorder](#drag--drop-reorder)
  - [Skip / Hide Steps](#skip--hide-steps)
  - [Duplicate Steps](#duplicate-steps)
  - [Add Steps Manually](#add-steps-manually)
  - [Wait Action](#wait-action)
  - [Call Folder Action](#call-folder-action)
  - [Folder Management](#folder-management)
  - [Rename Folders and Saved Steps](#rename-folders-and-saved-steps)
  - [Run Folder Steps](#run-folder-steps)
  - [Move / Copy Between Folders](#move--copy-between-folders)
  - [Export & Import (JSON)](#export--import-json)
  - [Export HTML Report](#export-html-report)
- [Keyboard Shortcuts](#keyboard-shortcuts)
- [Troubleshooting](#troubleshooting)
- [License](#license)

---

## Features

- **Record** — Automatically capture element find, click, sendKeys, clear, tap, and swipe actions
- **Edit** — Modify any step's action type, element reference, locator, text, or coordinates
- **Reorder** — Drag & drop steps to rearrange execution order
- **Replay** — Re-run a single step or all steps sequentially with visual progress
- **Skip/Hide** — Toggle individual steps to exclude them from execution
- **Duplicate** — Clone any step instantly
- **Wait** — Insert pause steps with configurable duration (seconds)
- **Call Folder** — Reference and execute an entire folder's steps as a single step
- **Folders** — Organize steps into named folders, rename folders and saved step sets
- **Run Folder** — Execute all steps in a saved step set directly from the Folders tab
- **Move/Copy** — Move or copy saved step sets between folders
- **Export/Import** — Save and load step collections as JSON files
- **HTML Report** — Generate a styled HTML report of all current steps

---

## Installation

### Option 1: Install from GitHub (Appium Plugin)

```bash
# Install the plugin from this repository
appium plugin install LevanKerdikashvili/appium-inspector-step-recorder --source=github

# Start Appium with the plugin enabled
appium --use-plugins=inspector
```

Then open your browser and navigate to:

```
http://localhost:4723/inspector
```

### Option 2: Replace existing plugin manually

If you already have `appium-inspector-plugin` installed:

1. Find the installed plugin location:
   ```bash
   appium plugin list --installed
   ```

2. Navigate to the plugin directory (typically `~/.appium/node_modules/appium-inspector-plugin/`)

3. Replace the `dist-browser/` folder with the one from this repository:
   ```bash
   # Clone this repo
   git clone https://github.com/LevanKerdikashvili/appium-inspector-step-recorder.git

   # Replace the dist-browser folder
   cp -r appium-inspector-step-recorder/dist-browser/ ~/.appium/node_modules/appium-inspector-plugin/dist-browser/
   ```

4. Restart Appium:
   ```bash
   appium --use-plugins=inspector
   ```

### Option 3: Run from source

```bash
# Clone the original appium-inspector with step recorder changes
git clone https://github.com/LevanKerdikashvili/appium-inspector-step-recorder.git
cd appium-inspector-step-recorder

# Install dependencies (if building from the full source repo)
npm install

# For plugin mode — the built plugin is already in dist-browser/
# Just point your Appium plugin path here
```

---

## How to Use

### Getting Started

1. Start an Appium session as usual
2. In the Session Inspector, find the **"Step Recorder"** tab (alongside Source, Commands, Recorder, etc.)
3. The tab has two sub-tabs: **Steps** and **Folders**

### Recording Steps

1. Click the **red Record button** (⏺) in the action bar
2. Interact with your app — click elements, type text, perform gestures
3. Each action is automatically captured as a step in the table
4. Click the **Stop button** (⏹) to stop recording

**Automatically recorded actions:**
| App Interaction | Recorded Step |
|---|---|
| Find element | `Find` — stores strategy + selector |
| Click element | `Click` — references the found element |
| Type into element | `SendKeys` — references element + text |
| Clear element | `Clear` — references the found element |
| Tap on screen | `Tap` — stores (x, y) coordinates |
| Swipe on screen | `Swipe` — stores start → end coordinates |

### Step Table

Each step row shows:
| Column | Description |
|---|---|
| **⠿** | Drag handle for reordering |
| **#** | Step number |
| **Action** | Color-coded action tag (Find, Click, SendKeys, etc.) |
| **Details** | Element locator, text, coordinates, or folder reference |
| **Actions** | Skip, Run, Edit, Duplicate, Add, Delete buttons |

### Action Types

| Action | Tag Color | Description |
|---|---|---|
| **Find** | Blue | Find element by strategy/selector |
| **Click** | Green | Click on a found element |
| **SendKeys** | Purple | Type text into a found element |
| **Clear** | Orange | Clear a found element's text |
| **Tap** | Cyan | Tap at screen coordinates (x, y) |
| **Swipe** | Magenta | Swipe from (x1, y1) to (x2, y2) |
| **Wait** | Gold | Pause execution for N seconds |
| **Call Folder** | Volcano | Execute all steps from a saved folder |

### Editing Steps

1. Click the **Edit button** (✏️) on any step
2. The row becomes editable:
   - **Action type** dropdown to change the action
   - **Element reference** selector (for Click/SendKeys/Clear) — links to a Find step
   - **Text input** for SendKeys text, locator strings, coordinates
   - **Folder selector** for Call Folder action
3. Click **✓** to save or **✕** to cancel

**Coordinate format:**
- Tap: `(x, y)` — e.g., `(350, 500)`
- Swipe: `(x1, y1) → (x2, y2)` — e.g., `(200, 800) → (200, 200)`

### Running Steps

**Single step:**
- Click the **▶ Run** button on any step to execute it immediately

**All steps:**
- Click the **▶ Run All** button in the action bar
- Steps execute sequentially with visual indicators:
  - 🔵 Blue pulse = currently running
  - 🟢 Green = completed successfully
  - 🔴 Red = failed
- Click **⏹ Stop** to abort execution

**How element actions work on re-run:**
- The Step Recorder first **re-finds the element** using the stored strategy/selector
- Then performs the action (click, sendKeys, clear) on the freshly found element
- This ensures actions work even if element IDs have changed

### Drag & Drop Reorder

- Grab any step by its **drag handle** (⠿) on the left
- Drag it to a new position in the step list
- Steps renumber automatically

### Skip / Hide Steps

- Click the **👁 Eye** button to toggle a step's skip state
- Skipped steps appear faded and are excluded from "Run All" execution
- Click again to re-enable

### Duplicate Steps

- Click the **📋 Copy** button to duplicate a step
- The duplicate is inserted immediately after the original

### Add Steps Manually

1. Click the **+ Add** button on any step row (inserts after that step) or use the action bar's add button
2. Select an **Action Type** from the dropdown
3. Fill in the required fields:
   - **Find Element**: enter `[strategy] selector` (e.g., `[id] com.example:id/button`)
   - **Click/Clear**: select an element reference from a Find step
   - **SendKeys**: select element reference + enter text
   - **Tap**: enter coordinates as `(x, y)`
   - **Swipe**: enter as `(x1, y1) → (x2, y2)`
   - **Wait**: enter number of seconds
   - **Call Folder**: select a folder from the dropdown
4. Click **Add** to insert the step

### Wait Action

- **Purpose:** Pause execution between steps (similar to `Thread.sleep()`)
- **Value:** Number of seconds (supports decimals, e.g., `0.5`, `2`, `5`)
- **Display:** Gold "Wait" tag with ⏰ icon, shows duration like `2s`
- **Use case:** Wait for animations, loading screens, or dynamic content

### Call Folder Action

- **Purpose:** Execute all steps from a saved folder as a single step
- **Setup:**
  1. Save some steps to a folder first (see Folder Management)
  2. Add a new step → select "Call Folder" action type
  3. Select the target folder from the dropdown
- **Execution:** When this step runs, it executes every step in every saved step set within that folder, sequentially
- **Display:** Volcano-colored "Call Folder" tag with 📁 icon, shows folder name
- **Use case:** Reuse common flows like "Login", "Navigate to Profile", etc.

### Folder Management

**Create a folder:**
1. Go to the **Folders** tab
2. Click **"New Folder"**
3. Enter a name (e.g., "Login", "Checkout", "Profile")

**Save current steps to a folder:**
1. Click the **💾 Save to Folder** button in the action bar
2. Select a target folder
3. Enter a name for this step collection (e.g., "Login with valid credentials")
4. Click **Save**

**Load steps from a folder:**
- Click the **⬆ Upload** button on any saved step set to load it into the step table

### Rename Folders and Saved Steps

**Rename a folder:**
1. Click the **✏️ Pencil** button next to the folder name
2. Edit the name in the inline input field
3. Press **Enter** or click **✓** to save, or **✕** to cancel

**Rename a saved step set:**
1. Click the **✏️ Pencil** button next to the step set name
2. Edit the name inline
3. Press **Enter** or click **✓** to save

### Run Folder Steps

- Each saved step set in a folder has a **▶ Run** button
- Click it to execute all steps in that set sequentially
- A loading spinner shows while steps are running
- Results are reported via success/error messages

### Move / Copy Between Folders

- **Copy:** Click the **📋 Copy** button on a saved step set → select target folder
- **Move:** Click the **↔ Transfer** button → select target folder
- These buttons are disabled if only one folder exists

### Export & Import (JSON)

**Export:**
1. Click the **⬇ Download** button in the action bar
2. A `.json` file is downloaded containing all current steps

**Import:**
1. Click the **📁 Import** button in the action bar
2. Select a `.json` file previously exported
3. Steps are loaded into the step table

**JSON format:**
```json
[
  {
    "action": "findAndAssign",
    "params": ["id", "com.example:id/username", "el1", false]
  },
  {
    "action": "click",
    "params": ["el1", null]
  },
  {
    "action": "wait",
    "params": [2]
  }
]
```

### Export HTML Report

- Click the **📋 Report** button in the action bar
- A styled HTML file is downloaded with a table view of all steps
- Includes step numbers, actions, and details

---

## Keyboard Shortcuts

| Key | Context | Action |
|---|---|---|
| **Enter** | Edit mode | Save current edit |
| **Enter** | Rename input | Save rename |

---

## Troubleshooting

**Steps don't execute on re-run:**
- Make sure the app is in the same state as when the steps were recorded
- Element locators may have changed — edit the Find step's selector

**Swipe/Tap gives W3C error:**
- This is fixed in the current version — coordinates are now properly formatted with W3C pointer action types

**Folder data is lost:**
- Folders are stored in `localStorage` — clearing browser data will remove them
- Use Export (JSON) to back up important step collections

**Plugin not loading:**
- Verify Appium is started with `--use-plugins=inspector`
- Check the Appium server logs for plugin loading errors
- Ensure the `dist-browser/` folder is present in the plugin directory

---

## License

[Apache-2.0](https://github.com/appium/appium-inspector/blob/main/LICENSE)

Based on [Appium Inspector](https://github.com/appium/appium-inspector) by the Appium team.
