# NuxView

## 📝 The Vision: Bridging CLI and GUI
NuxView was conceived as a high-performance bridge between the raw power of the Linux Command Line Interface (CLI) and the intuitive speed of a Graphical User Interface (GUI). While the terminal is the gold standard for server management, complex directory structures often become "hidden" behind text-based lists. 

**NuxView** demystifies these structures by providing a dynamic, node-based visualization that allows developers to "see" their file systems as living architectural maps.

---

## 🏗️ System Architecture & Stack

The application is built using a modern, decoupled three-tier architecture to ensure maximum performance and maintainability.

### **1. Frontend Layer (React & Vite)**
-   **React 18**: Chosen for its efficient state reconciliation and component-based structure.
-   **React Flow (Svelte Flow core)**: The engine used for the 2D graph view. It handles zooming, panning, and rendering edges.
-   **Zustand (Conceptual)**: State management logic for tracking expanded paths and shared metadata.
-   **Vite**: used as the build tool to ensure sub-second hot-reloading during development and optimized tree-shaking for production.

### **2. Backend Layer (FastAPI & Python 3)**
-   **FastAPI**: A high-concurrency Python framework that provides lightning-fast request handling via Python's `async/await`.
-   **Parallel Scanning**: We implemented a multi-threaded directory walker. Instead of a single-threaded loop, NuxView spawns parallel workers to traverse the file system, allowing it to index thousands of files in a fraction of the time.
-   **RESTful API**: Clean endpoints for node discovery, full system scans, and health monitoring.

### **3. Infrastructure & CLI**
-   **Unified Binary (`nuxview`)**: A specialized bash wrapper that manages the Python virtual environment, uvicorn server, and log rotations. It treats NuxView as a controllable service.

---

## 🧩 Comprehensive Component Breakdown

### **The Visual Tree (Interactive Graph)**
The graph is not just a static image; it's a living representation of state.
-   **Dagre Layout Engine**: Every node placement is calculated using the Dagre algorithm, ensuring a logical Left-to-Right flow.
-   **Folder Nodes**: Custom nodes that display folder names, specific iconography, and expand/collapse triggers.
-   **Spacer Edge Utility**: One of our key innovations. To prevent overlapping in dense directories, we inject "invisible edges" that act as physical spacers in the Dagre coordinate system, forcing a clean vertical gap between sibling labels.

> [!NOTE]
> **Expand All Strategy**: To prevent browser crashes, "Expand All" is capped at 3 levels by default, encouraging users to drill down manually into deeper structures.

![Graph View - Detailed Node Structure](https://raw.githubusercontent.com/NaveenAkalanka/NuxView/main/docs/screenshots/graph_deep_view.png)
*(Placeholder: Graph Visualization Screenshot)*

### **The SidePanel (Hierarchical Explorer)**
A high-density file tree designed for familiar navigation.
-   **Overflow & Nested Scrolling**: We implemented specialized CSS logic to allow infinite horizontal expansion. Deeply nested files will never be "cut off"; instead, the panel becomes horizontally scrollable, preserving the full system path.
-   **Lazy-Loaded Nodes**: To save memory, folder contents are only fetched from the backend when a user clicks the chevron, ensuring the UI remains snappy even on low-spec servers.

### **Unified Two-Way Sync Engine**
This is the "glue" of NuxView. 
-   **Tree-to-Panel**: Clicking a graph node immediately scrolls the SidePanel to that specific item and highlights it with a neon accent.
-   **Panel-to-Tree**: Clicking an explorer item triggers a reverse lookup. The graph automatically expands the entire parent path leading to that node and pans the camera to center the newly discovered folder.

---

## 🛠️ Problem Solving & Bug History

During the development cycle, we encountered and resolved several engineering hurdles:

### **1. The Overlap Headache**
-   **Issue**: Folder labels with long names would overlap vertically in the graph, making the UI unreadable.
-   **Solution**: We bypassed the standard Dagre constraints by injecting "invisible" spacer nodes between real folders. These nodes take up physical space but remain invisible to the user, effectively "pushing" the real folder labels apart.

### **2. Stale PID Conflict**
-   **Issue**: If the server crashed or the terminal was closed forcefully, the `nuxview.pid` file would remain. On the next start, the script would think the app was already running and refuse to launch.
-   **Solution**: We upgraded the CLI to perform a **Living Check**. It now validates the PID *against* the API health endpoint. If the PID exists but the API doesn't respond, the script recognizes it as a "stale" state, cleans up the old file, and starts fresh automatically.

### **3. Missing Log Directories**
-   **Issue**: In fresh installations, `nuxview start` would fail with "File not found" for logs.
-   **Solution**: We moved directory verification from the installer alone to the CLI `ensure_dirs` function. Now, every time `nuxview` is invoked, it checks for `logs/` and `data/` and recreates them if they've been deleted.

---

## 🚀 Deployment & Usage Guide

### **Installation Prerequisites**
Ensure your server has:
-   **Python 3.8+** (with `python3-venv` package).
-   **curl** and **git** (for source fetching).
-   **Access to Port 4897** on your local network/firewall.

### **Step-by-Step Installation**
1.  **Run the Installer**:
    ```bash
    curl -sL https://raw.githubusercontent.com/NaveenAkalanka/NuxView/main/remote_install.sh | bash
    ```
2.  **Access the UI**: Open your browser at `http://your-server-ip:4897`.

### **CLI Commands Reference**
| Command | Usage | Description |
| :--- | :--- | :--- |
| `nuxview start` | `nuxview start --port 5000` | Launches the server (defaults to 4897). |
| `nuxview status` | `nuxview status` | Reports PID, API health, and install paths. |
| `nuxview update` | `nuxview update` | Pulls latest code and rebuilds production assets. |
| `nuxview stop` | `nuxview stop` | Kills the backend process gracefully. |
| `nuxview uninstall`| `nuxview uninstall`| Wipes all app files and user data safely. |

---

## 🔒 Security & Performance

-   **Read-Only Integrity**: NuxView does not include any "Modify" or "Delete" endpoints. Your system is safe from accidental changes.
-   **Low Overhead**: The backend is designed to sleep when not in use, consuming negligible CPU cycles.
-   **LAN Default**: The server binds to `0.0.0.0` to allow you to visualize your server from a tablet, laptop, or phone on the same network.

![Mobile View - Responsive Tab System](https://raw.githubusercontent.com/NaveenAkalanka/NuxView/main/docs/screenshots/mobile_responsive.png)
*(Placeholder: Mobile UI Screenshot)*

---

## 🔗 Repository
Visit the official source for updates, issue tracking, and contributions:  
**[NuxView GitHub Repository](https://github.com/NaveenAkalanka/NuxView)**

---
<p align="center">
  <sub>© 2025 NuxView Project. Licensed under MIT.</sub><br>
  <sub>Designed & Developed by <strong>Naveen Akalanka</strong></sub>
</p>
