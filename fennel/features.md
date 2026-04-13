The following is a concise user manual and features list for the **Bit Project (Fennel Edition)** — a lightweight, extensible AI sandbox built with Fennel and Lua.

---

## 📘 **Bit Project User Manual**

### **Overview**

Bit is an interactive AI assistant that can execute code in a secure sandbox, clone Git repositories, and run simulations. It uses the DeepSeek API (or a mock mode) and is written in Fennel, a Lisp dialect that compiles to Lua.

### **Installation**

1. **Install Lua 5.3+** (if not already present).
2. **Install Fennel**:
   ```bash
   # Via LuaRocks
   luarocks install fennel
   ```
3. **Install required Lua modules**:
   ```bash
   luarocks install luasocket
   luarocks install luafilesystem
   luarocks install dkjson
   ```
4. **Clone or download the Bit project files** to a directory of your choice.

### **Getting Started**

1. **Set your DeepSeek API key** (optional, mock mode works without it):
   ```bash
   export DEEPSEEK_API_KEY="your-key-here"
   ```
2. **Run the application**:
   ```bash
   fennel main.fnl
   ```
3. You will see a prompt: `>`

### **Commands**

| Command | Description |
|:---|:---|
| `exit` | Quit the application. |
| `clean` | Delete the workspace directory (`./workspace`) and all cloned repositories. |

Anything else you type is sent to the AI assistant. The assistant can respond with plain text or invoke built-in tools.

### **Built-in Tools**

The AI can automatically use the following tools when appropriate:

| Tool | Description | Example Usage (by AI) |
|:---|:---|:---|
| **`execute_code`** | Run Lua code in a secure, sandboxed environment with a 30‑second timeout. Output is captured and returned. | *"Run this Lua code: `print(2+2)`"* |
| **`clone_repo`** | Clone a Git repository into the workspace directory (`./workspace`). | *"Clone https://github.com/example/repo.git"* |
| **`run_simulation`** | Execute a cellular automaton (Game of Life) or an agent‑based simulation. | *"Run a Game of Life simulation for 50 steps."* |

**Note:** Only Lua is supported in the sandbox in this version.

### **Configuration**

| Option | Environment Variable | Default |
|:---|:---|:---|
| DeepSeek API key | `DEEPSEEK_API_KEY` | (empty → mock mode) |
| Workspace directory | (hardcoded) | `./workspace` |
| Tool output truncation limit | (hardcoded) | 8000 characters |

### **Plugin System (Advanced)**

You can extend Bit with custom tools by placing Fennel (`.fnl`) files in the `plugins/` directory. Each plugin file should return a table of functions. These functions can then be invoked as tools (integration requires minor code modification; the plugin loader is present but not yet wired into the tool dispatch).

### **Limitations**

- **Sandbox language**: Only Lua is supported out‑of‑the‑box.
- **Network calls**: The sandbox blocks all network and file system access except for a few whitelisted functions (`print`, `math`, `string`, etc.).
- **Git**: Requires `git` to be installed and available in the system PATH.
- **Async**: Tool execution is currently synchronous; long‑running operations will block the REPL.

### **Troubleshooting**

| Symptom | Likely Cause | Solution |
|:---|:---|:---|
| `module 'socket.http' not found` | LuaSocket not installed. | Run `luarocks install luasocket`. |
| `module 'dkjson' not found` | dkjson not installed. | Run `luarocks install dkjson`. |
| `module 'lfs' not found` | LuaFileSystem not installed. | Run `luarocks install luafilesystem` (optional, only needed for plugins). |
| "Execution timed out" | Lua code took longer than 30 seconds. | Simplify code or increase timeout in `sandbox.fnl`. |
| "Clone failed" | Git not installed or network issue. | Install Git and check URL. |

---

## ✨ **Features List**

### **Core Capabilities**

- ✅ Interactive REPL with conversation history
- ✅ DeepSeek API integration (chat + tool calling)
- ✅ Mock mode when API key is missing (clearly labeled)
- ✅ Secure Lua sandbox with:
  - `_ENV`‑based isolation
  - Whitelisted safe functions (`math`, `string`, `table`, etc.)
  - Output capture
  - 30‑second timeout
- ✅ Git repository cloning
- ✅ Cellular automaton (Game of Life) simulation
- ✅ Agent‑based simulation (bouncing particles)
- ✅ Cross‑platform workspace cleanup command
- ✅ Graceful error handling (timeouts, compilation errors, unsupported languages)

### **Architecture & Extensibility**

- ✅ Written in **Fennel** – a Lisp that compiles to Lua
- ✅ Modular design (separate modules for API, sandbox, git, simulation)
- ✅ Plugin loader skeleton (ready for custom tool extensions)
- ✅ Async utilities (channels, coroutines) – prepared for future non‑blocking operations
- ✅ Minimal dependencies (LuaSocket, dkjson, LuaFileSystem)

### **Developer Experience**

- ✅ Under 500 lines of concise, idiomatic Fennel code
- ✅ Easy to modify and extend
- ✅ Compatible with Lua 5.3+

---

The **Bit Project** is a compact yet powerful AI sandbox that demonstrates the elegance of Fennel and the robustness of Lua. It is ready for daily use and serves as an excellent foundation for further experimentation.
