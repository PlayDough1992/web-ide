# PD-WebDev: In-Browser Web IDE

PD-WebDev is a modern, single-file, in-browser web development IDE featuring a Monaco Editor core, tabbed editing, and a session-based virtual file system. It is designed for rapid prototyping, teaching, and web development directly in your browser—no installation required.

## Features

### 🗂️ File Explorer
- **Create and delete files/folders** via a modern sidebar.
- **In-memory file system**: All files and folders exist in your browser session (no server required).
- **Download** any file or folder (folders as .zip archives).
- **Move** files and folders using a two-pane modal for intuitive organization.

### 📝 Monaco Editor
- **Tabbed editing**: Open multiple files at once, switch between them easily.
- **Syntax highlighting** for HTML, CSS, JS, TypeScript, Python, Markdown, and more.
- **Auto-detects language** based on file extension.
- **Modern UI**: Clean, dark theme.

### ▶️ Run Web Files
- **Run HTML files** in a built-in webview tab (iframe) with a single click.
- **Inlines CSS and JS**: When you run an HTML file, any linked CSS/JS files from your project are automatically inlined for accurate preview.

### 📦 Download
- **Download individual files** as the extension they are in PD-WebDev.
- **Download folders** as .zip archives, preserving the folder structure.
- **Project root** is zipped as a single top-level folder for easy extraction.

### 🖼️ Modern UI/UX
- **Modern design**: Subtle blurs, rounded corners, and vibrant accent colors.
- **Responsive layout**: Works on desktops and large tablets.
- **Accessible modals** for file/folder creation and moving.

## Getting Started
1. Open [PD-WebDev](https://playdough1992.github.io/web-ide/) in your browser.
2. Use the sidebar to create files/folders, edit code, and organize your project.
3. Click the green ▶️ button to run your HTML file in a new tab within the IDE.
4. Download files or folders as needed.

## Dependencies
- [Monaco Editor](https://microsoft.github.io/monaco-editor/) (CDN) for the editor.
- [JSZip](https://stuk.github.io/jszip/) (CDN, loaded on demand for zipping)

## Limitations
- All files/folders are stored in memory and will be lost when you close or refresh the browser tab, UNLESS YOU DOWNLOAD THEM.
- No server-side features; everything runs client-side in your browser.

## License
MIT License. Free for personal and educational use.

---

Enjoy fast, distraction-free web development—right in your browser!
