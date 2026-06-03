# Omnivore User Manual

**Omnivore** — 8-bit Software Archaeology Toolbox. A cross-platform retrocomputing reverse engineering environment for working with disk images, executables, and emulation of 8-bit computers (Atari 400/800, Apple II+, and generic 6502 systems).

---

## Installation

See `README.rst` for build/install instructions.

**Launch commands:**
| Mode | Command |
|------|---------|
| GUI application | `python run.py` or `omnivore` |
| CLI disk tool | `python -m atrip` or `atrip` |
| Standalone emulator | `python emu.py` or `omnimu` |

---

## User Interface

### Main Window
- **Menu bar** — dynamically changes based on the active editor/viewer.
- **Toolbar** — context-sensitive buttons for the active tool.
- **Tab bar** — each open document appears as a tab.
- **Tile area** — the central workspace, split into resizable panes (tiles), each holding a viewer.
- **Sidebars** — viewers can be docked to left, right, top, or bottom edges.
- **Status bar** — shows current sector/offset, file info, and status messages.
- **Minibuffer** — command-line input area (GDB-like debugger commands in emulator mode).

### Tile Layout
Tiles are resizable panes that contain viewer controls. You can:
- Drag a viewer tab to split tiles horizontally or vertically
- Dock viewers to sidebars
- Save/restore layouts via the Layout menu
- Close viewers from the right-click context menu

---

## Supported File Formats

### Disk Images
| Format | Description | Read | Write |
|--------|-------------|------|-------|
| XFD | XFormer raw disk dump | Yes | Yes |
| ATR | Nick Kennedy's ATR (16-byte header) | Yes | Yes |
| DCM | Disk Communicator (Bob Puff compression) | Yes | Yes |
| CAS | Atari cassette images | Yes | Yes |
| DSK | Apple II DOS 3.3 raw sector dump | Yes | Yes |

### File Systems
| Filesystem | Platform | Read | Write |
|------------|----------|------|-------|
| DOS 2 (90K) | Atari 8-bit | Yes | Yes |
| DOS 2 (180K) | Atari 8-bit | Yes | Yes |
| DOS 2.5 (130K) | Atari 8-bit | Yes | Yes |
| DOS 3.3 | Apple II | Yes | Yes |
| SpartaDOS | Atari 8-bit | In development | No |
| MyDOS | Atari 8-bit | Partial | No |

### Executables
| Format | Platform | Read | Write |
|--------|----------|------|-------|
| .xex | Atari 8-bit executable | Yes | Yes |
| .car | Atari cartridge image | Yes | No |
| BSAVE | Apple II | Yes | Yes |
| KBoot | Atari xex in boot disk | Yes | Yes |

### Archives (transparent)
Zip, Tar, gzip, bzip2, lzma/xz, Unix compress (.Z), lz4, DCM.

---

## Viewers

### Hex Viewer
The hexadecimal byte editor. Features:
- Inline hex and ASCII editing
- Selection, cut/copy/paste
- Find (Ctrl+F) and find next
- Undo/redo (Ctrl+Z / Shift+Ctrl+Z)
- Byte manipulation: set to 0, $FF, NOP, bitwise ops, rotate, shift, arithmetic
- Comment system — right-click to add/remove comments on bytes
- Label system — name memory locations
- Segment creation from selection

### Bitmap Viewer
Renders bytes as pixel graphics:
- Multiple bitmap encoding formats (select from viewer toolbar)
- Color palette management (ANTIC color registers, NTSC/PAL)
- Configurable rendering width
- Zoom controls

### Pixel Map Viewer
Per-pixel bitmap display with fine-grained control over pixel mapping.

### Character Viewer
ANTIC font-based character display — renders bytes as text characters using Atari ANTIC font encoding.

### Disassembly Viewer
6502/65C02/6809 disassembler powered by libudis:
- Mark bytes as code or data
- Mini-assembler (inline assembly modification)
- Disassembly type marking
- Multiple CPU architecture support

### Map Editor
Tile-based map/level editor with drawing tools:
- Rectangle Select, Eyedropper, Draw, Line, Square, Filled Square
- Rectangular selection and paste
- Configurable tile sizes

### Info Viewers
Sidebar panels showing:
- Segment info (name, origin, length)
- Comments on selected bytes
- Undo history
- Segment list/browser

### Label Viewer
Memory map label viewer — shows named address ranges (e.g., OS labels for Atari).

### Segment Viewer
Lists all segments (boot, VTOC, directory, files) in the current disk image.

### Apple II Viewers
- HGR (Hi-Res Graphics) page 1 and 2 viewer
- Text/Lo-Res page 1 and 2 viewer
- Apple II display modes

### Jumpman Level Editor
Full level editor for the Atari 8-bit game Jumpman:
- See "Jumpman Level Editor" section below.

---

## Emulator

Three emulators are available:

| Emulator | Platform | CPU | Video | Resolution |
|----------|----------|-----|-------|------------|
| Atari 800 | Atari 8-bit family | 6502 (libatari800) | ANTIC/GTIA | 320x192 |
| Generic 6502 | Custom hardware | 6502 (lib6502) | Minimal | Debug only |
| Crabapple | Apple II+ | 6502 (lib6502) | Text/HGR | 560x384 |

### Emulator Viewers
- **Video** — emulator screen display
- **History** — CPU instruction history (last N instructions)
- **Memory** — memory access visualization

### Emulator Controls
| Key | Action |
|-----|--------|
| F2 | Atari Option key |
| F3 | Atari Select key |
| F4 | Atari Start key |
| F5 | Warmstart (system reset) |
| Shift+F5 | Coldstart |
| F6 | Previous history state |
| F7 | Next history state |
| F8 | Pause/Resume |
| F9 | Step (single instruction) |
| F10 | Step over |
| F11 | Step out |
| Shift+F9 | Break at VBI |
| F12 | Break at frame boundary |
| Ctrl+T | Boot current disk image |

### Debugger
GDB-like command interface in the minibuffer:
| Command | Description |
|---------|-------------|
| `b ADDR [if COND]` | Set breakpoint at address |
| `w COND` | Set watchpoint |
| `c [COUNT]` | Continue execution |
| `s [N]` | Step N instructions |
| `n [N]` | Step over N instructions |
| `f` | Finish (continue to end of function) |
| `u [ADDR]` | Continue until address |
| `d [RANGE]` | Delete breakpoints |
| `info` | Show CPU/ANTIC/POKEY/GTIA registers |

**Breakpoint types:** address, condition, cycle count, frame count, scan line, VBI, DLI.

### Emulator Features
- Rewind / step backward through execution history
- Save/restore emulator state
- Screen capture (single frame and animation)
- CPU state modification
- Memory map labels
- Atari keyboard mapping (full key table)

---

## Jumpman Level Editor

Full-featured editor for Jumpman (Atari 800) levels:
- Browse level list from disk image
- Visual level editor with playfield renderer
- Draw tools: select, girder, double girder, ladder, up rope, down rope, erase, coin, respawn
- Assembly source compilation (MAC/65 compatible via ATasm)
- Level metadata display

---

## Byte Manipulation Operations

Select bytes in the hex viewer, then use:

### Set Operations
| Shortcut | Action |
|----------|--------|
| Ctrl+0 | Set to $00 |
| Ctrl+9 | Set to $FF |
| Ctrl+3 | Set to NOP ($EA) |

### Bitwise Operations
| Shortcut | Action |
|----------|--------|
| Ctrl+1 | Bitwise NOT (invert) |
| Ctrl+7 | AND with value |
| Ctrl+6 | XOR with value |
| Ctrl+\ | OR with value |

### Shift/Rotate
| Shortcut | Action |
|----------|--------|
| Ctrl+< | Rotate left |
| Ctrl+> | Rotate right |

### Arithmetic
| Shortcut | Action |
|----------|--------|
| Ctrl+= | Add value |
| Ctrl+- | Subtract value |
| Shift+Ctrl+- | Subtract from value |
| Ctrl+8 | Multiply by value |
| Ctrl+/ | Divide by value |
| Shift+Ctrl+/ | Divide from value |

---

## Layout Management

- **Save Layout** — save the current tile arrangement and viewer state.
- **Restore Layout** — restore a previously saved layout.
- **Default Layout** — reset to the editor's default layout.
- Layouts are stored as JSON in the application's template directory.

---

## Command-Line Tool (`atrip` / `python -m atrip`)

Command-line disk image inspection and extraction:

```
atrip <command> <file>
```

See `README.atrip.rst` for full documentation.

---

## Filesystem Browser

When a disk image is loaded, the filesystem is shown as a segment tree:
- **Boot segment** — DOS boot sectors
- **VTOC** — Volume Table of Contents (sector allocation map)
- **Directory** — file listing with flags, sizes, and sector pointers
- **Files** — each file is a segment that can be inspected or extracted

Right-click on files to:
- View file info
- Extract file data
- Copy to clipboard
- Add comments

---

## Configuration

- **Preferences** (Ctrl+,) — global application settings (font, colors, behavior).
- **Per-editor settings** — viewer-specific controls (width, zoom, renderer, palette).
- **Layout templates** — stored in `omnivore/templates/` as `.default_layout` JSON files.
- **Label/memory map files** — `.labels` files in `omnivore/templates/` defining OS symbol tables.

---

## Architecture Notes

Omnivore is built on two layers:

- **ATRip** — hierarchical filesystem parsing library for 8-bit disk images (sector-level access, VTOC parsing, directory traversal, file extraction).
- **Sawx** — Simple wxPython application framework providing multi-tab, multi-frame editor with dynamic menus, toolbars, and tile layout management.

Data flows: `File → Container (compression/archives) → Media (disk image) → Filesystem (directory/file parsing) → Segments (byte-level access) → Viewers (display/editing)`.

---

## Tips

- Use **Ctrl+; and Shift+Ctrl+;** to add/remove comments on selected bytes — comments persist across sessions.
- The **Segment List** sidebar shows all data regions; click to navigate.
- Drag viewer tabs to rearrange your workspace.
- In the emulator, F8 pauses and F9 single-steps; use the debugger for advanced breakpoints.
- Layout templates are JSON — you can edit them manually to set up custom viewer arrangements.
- The Jumpman level editor can compile modified levels back into the game disk image.
- File comments and labels are stored inside the project file, not the original disk image.
