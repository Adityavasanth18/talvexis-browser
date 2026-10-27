# Talvexis Browser

**AI-powered browser automation for real-world tasks.**

**Project maintainer:** Aditya Devarakodna

Talvexis Browser brings AI-assisted interaction to Google Chrome. It can observe a web page, interpret what's visible, and take actions such as navigating, clicking, typing, scrolling, filling forms, and organizing tabs. It also supports recording a manual workflow and turning it into a reusable routine.

The goal is simple: make complex, multi-step browser tasks easier to automate while keeping the user in control.

## Highlights

- **Visual browser automation** — interact with pages using screenshots and browser controls.
- **Multi-step workflows** — navigate between pages, complete forms, and extract useful information.
- **Record, compile, replay** — record a browser task and convert it into a reusable routine.
- **Tab and session management** — work across multiple tabs and preserve browser sessions.
- **Local web interface** — manage browser access and chat with the agent from a simple UI.
- **Developer-friendly tools** — use the API, WebSocket interfaces, and evaluation tools.

## How It Works

```text
      AI Model
          |
          v
  Talvexis Browser Server
   (FastAPI + WebSocket)
          |
          v
     Chrome Extension
          |
          v
    Browser Interaction
```

The server coordinates AI requests and browser actions. The Chrome extension connects to the server and performs actions in the browser. The web interface lets you configure the AI model, select a browser instance, and run tasks.

**Current browser support:** Google Chrome with the Talvexis Browser extension.

## Getting Started

### 1. Install the dependencies

Clone your Talvexis Browser repository and open its directory, then run:

```bash
# Recommended
uv sync

# Alternative
pip install -e .
```

For development dependencies:

```bash
uv sync --group dev
```

### 2. Start the server

```bash
uv run local-chrome-server serve
```

The local services use these addresses:

- Web interface: `http://localhost:8765`
- WebSocket server: `ws://127.0.0.1:8766`

### 3. Configure the AI model

Open `http://localhost:8765` and complete the initial configuration. Enter your model name, provider base URL, and API key, then save the settings.

Example configuration:

| Setting | Example |
| --- | --- |
| Model | `dashscope/qwen3.5-plus` |
| Alternative model | `dashscope/qwen3.5-flash` |
| API base URL | `https://dashscope.aliyuncs.com/compatible-mode/v1` |
| API key | Your own provider API key |

Model settings are stored locally in `~/.openbrowser/llm_config.json`. You can change them later from the settings control in the web interface.

### 4. Build the Chrome extension

From the project directory:

```bash
cd extension
npm install
npm run build
```

### 5. Load the extension

1. Open `chrome://extensions/` in Google Chrome.
2. Turn on **Developer mode**.
3. Select **Load unpacked**.
4. Choose the project's `extension/dist` directory.
5. Open the extension page and copy the browser UUID it displays.

**Security:** Treat the browser UUID as a private access token. Anyone with access to it may be able to control that browser instance.

### 6. Connect and start using Talvexis Browser

1. Open `http://localhost:8765`.
2. Paste the browser UUID into the **BROWSER UUID** field.
3. Enter a task in the chat interface.
4. Allow pop-ups for sites involved in your workflow if Chrome blocks required new tabs.

Example tasks:

```text
Open a website and summarize its main content.

Search multiple pages and compare the results.

Fill out a form using the details I provide.

Record a repetitive browser workflow so I can run it again.
```

## Reusable Browser Routines

Talvexis Browser can turn a demonstrated workflow into a routine:

1. **Record:** Select **Record → Start recording** and complete the task manually.
2. **Review:** Stop recording and inspect the captured actions and keyframes.
3. **Compile:** Select **Compile Routine** to turn the trace into a structured routine.
4. **Save:** Give the routine a descriptive name.
5. **Replay:** Run the saved routine from the routine launcher or command box.

Replay follows the compiled workflow rather than blindly reproducing every recorded click.

## Development

Useful commands:

```bash
# Start the extension development build
cd extension
npm run dev

# Check TypeScript types
npm run typecheck
```

The repository is organized around these components:

```text
server/       Backend, agent orchestration, API, WebSocket
extension/    Chrome extension and browser action handlers
frontend/     Web interface
skill/        Agent integration skills
eval/         Browser automation evaluation tools
```

To list the available browser evaluation tasks, run:

```bash
python eval/evaluate_browser_agent.py --list
```

To run a specific evaluation, supply a configured model alias and browser UUID:

```bash
python eval/evaluate_browser_agent.py --test techforum --chrome-uuid YOUR_BROWSER_UUID --model-alias default
```

## Project Information

**Talvexis Browser**  
**By Aditya Devarakodna**
