# ScrollSync Documentation

## 1. Project Information
**Project Name:** ScrollSync  
**Version:** 1.0.0  
**Type:** Browser Extension (Manifest V3)  
**Core Purpose:** Automates web page scrolling and interaction using user-defined speeds, intervals, and external gamepad controllers. Designed for a "chill" or hands-free browsing experience.

---

## 2. Aims & Objectives
- **Automated Navigation:** Provide hands-free scrolling for long-form articles, social feeds, and image galleries.
- **Accessibility:** Enable users to control their browser using gamepads (Xbox, PlayStation, etc.) instead of a traditional mouse/keyboard.
- **Customization:** Offer fine-grained control over scroll speed, direction, and interaction intervals.
- **Smart Interaction:** Implement "Smart Clicking" logic that identifies the best targets (like gallery "next" buttons) automatically.

---

## 3. System Architecture
The extension follows the standard **Chrome Extension Manifest V3** architecture, integrated with a modern **React + Vite** build pipeline.

### High-Level Architecture
- **UI Layer (Popup):** Built with React 18 and TailwindCSS. The popup (`index.html`) is served via the `action` property in manifest.json. Features 4 navigation tabs (Vert, Horz, Auto, Pad). Uses `@phosphor-icons/react` for iconography.
- **Control Layer (Background Service Worker):** Acts as the central state manager using `chrome.storage.local`. Handles context menus ("Enable/Disable ScrollSync", "Show/Hide Floating Controls"), badge updates, and coordinates state across tabs. Per-tab state is stored with keys like `tabState_{tabId}`.
- **Execution Layer (Content Script):** Injected into every page (`<all_urls>`). Performs DOM manipulations, gamepad polling via `Navigator.getGamepads()`, floating icon management, and the overlay panel (iframe of popup).
- **Guide Page:** A dedicated `guide.html` page accessible from the popup UI (Info button) and context menu.

---

## 4. Directory Structure
```text
ScrollSync/
├── dist/                # Production build output
├── src/
│   ├── background/      # Service Worker (State & Context Menus)
│   ├── content/         # Content Script (Scroll, Gamepad, Floating UI)
│   ├── App.jsx          # Main React UI Component
│   ├── main.jsx         # React Entry Point
│   └── index.css        # Tailwind & Global Styles
├── guide.html           # Help/Usage guide page
├── icon.png             # Extension icon (also icon.svg)
├── manifest.json       # Extension configuration
├── tailwind.config.js   # Style configuration
├── vite.config.mjs      # Build & Extension bundling config
└── package.json        # Dependencies & scripts
```

---

## 5. System Components & Functions

### A. Static Scrolling (Vertical/Horizontal)
- **Function:** Automatically scrolls the page at a constant pixel-per-second rate (0-500 px/s).
- **Tabs:** "Vert" tab for vertical scrolling, "Horz" tab for horizontal scrolling.
- **Logic:** Uses `requestAnimationFrame` for smooth sub-pixel scrolling.
- **Optimization:** Dynamically identifies the most "scrollable" element on the page (handling `overflow: auto` containers) or falls back to `window`.
- **Exclusive Mode:** Only one mode (Vert/Horz/Nav/Pad) can be active at a time.

### B. Smart Pulse (Auto-Click)
- **Function:** Simulates clicks at defined intervals (1s to 60s).
- **Intelligence:** Analyzes the viewport for large images (`img`, `canvas`, `[role="img"]`) to determine the best click position.
- **Modes:** Left, Center, or Right screen positioning.
- **Visual Feedback:** Shows a cyan dot that scales up and fades out at click location.

### C. Gamepad Sync
- **Function:** Maps gamepad sticks and buttons to browser actions.
- **Input:** Uses the `Navigator.getGamepads()` API with polling in the main animation loop.
- **Features:**
    - Dual-stick support (selectable Left/Right stick via buttons).
    - Sensitivity adjustment (0-50 range, default 15).
    - V-Scroll / H-Scroll toggle buttons.
    - Smart-Click on Button 0 (A/X) or Trigger (Button 7).
    - "Flick" clicking: Threshold-based (>0.9) stick movement triggers click.
    - Live connection status indicator ("Pad Linked" / "Awaiting Pad").

### D. Floating Controls
- **Function:** A draggable on-page icon providing quick status and access.
- **Shape:** 48px hexagonal/cyber shape (clip-path polygon) with cyan border and glow.
- **Position:** Bottom-right by default, fully draggable.
- **Menu:** Right-click opens radial menu with Pause, Enable, and Guide buttons.
- **Status Line:** Colored bar at bottom (Cyan = active, Red = paused, Gray = disabled).
- **Toggle:** Can be enabled/disabled via popup stats bar ("FLOAT_ON"/"FLOAT_OFF").

### E. Overlay Panel
- **Function:** Slides in the full popup UI as an iframe overlay on the page.
- **Trigger:** Click on the floating icon.
- **Position:** Fixed to top-right of viewport.

### F. Keyboard Shortcuts & Toast Notifications
- **Shortcuts:** `Space` (Pause/Play), `W`/`S` (Adjust speed), `A`/`D` (Navigate Back/Forward).
- **Toast:** Cyber-styled status messages appear at top-center with ">>" prefix (e.g., ">> System Linked", ">> Speed: X px/s"). Dark background with cyan border, clip-path polygon shape.

---

## 6. Technical Stack
- **Framework:** React 18
- **Styling:** TailwindCSS (Custom **"Cyber/Sci-Fi"** theme with scanline effects)
- **Typography:** Orbitron (headers) + Tomorrow (body) from Google Fonts
- **Icons:** @phosphor-icons/react
- **Build Tool:** Vite 5 + @crxjs/vite-plugin
- **Runtime APIs:** Chrome Extension APIs (Storage, Tabs, Scripting, ContextMenus, Action, Runtime, Windows) + Gamepad API

---

## 7. UI Design System (Cyber Theme)
- **Dimensions:** 300px x 500px popup
- **Color Palette:**
  - Background: `#050a0e` (Deep dark blue/black)
  - Primary Accent: `#00f0ff` (Electric cyan)
  - Secondary Accent: `#fcee0a` (Cyber yellow - top bar)
  - Alert/Error: `#ff003c` (Cyber red)
  - Text: White/light gray with monospace styling
- **Visual Effects:** Scanline overlay, clip-path polygons for buttons, glowing shadows

---

## 8. Controls & Shortcuts
| Action | Keyboard | Gamepad |
| :--- | :--- | :--- |
| **Pause/Resume** | `Space` | — |
| **Increase Speed/Interval** | `W` | — |
| **Decrease Speed/Interval** | `S` | — |
| **Navigate Back** | `A` | Left Stick West (Flick >0.9) |
| **Navigate Forward**| `D` | Left Stick East (Flick >0.9) |
| **Smart Click** | — | Button 0 (A/X) or Trigger (Button 7) |

---

## 9. State Management
- **Per-Tab State:** State is stored in `chrome.storage.local` with keys like `tabState_{tabId}`.
- **State Reset:** When a tab reloads (`loading` status), the extension automatically pauses (`isPaused = true`) to prevent unexpected scrolling on new content.
- **IPC Flow:**
  1. Popup sends `updateState` to Background
  2. Background updates storage and sends `refreshUI` to all listeners
  3. Background also forwards the update to the Content Script via `chrome.tabs.sendMessage`
  4. Content Script applies the new settings to the main loop

---

## 10. Deployment & Build
To build the extension for production:
```bash
npm run build
```
The resulting `dist/` folder can be loaded into Chrome via **chrome://extensions > Developer Mode > Load Unpacked**.

For development with HMR:
```bash
npm run dev
```
