# Changelog

All notable changes to this project will be documented in this file.

## [1.99.10] - 2026-06-02

### Added
- Dependency checker script (`scripts/check_deps`) with 30 checks.
- `MANUAL.md` — comprehensive user manual covering all features, file formats,
  viewers, emulator controls, keyboard shortcuts, and layout management.
- Missing default layout templates for `Unknown media` and `No Filesystem`
  document types (`omnivore/templates/`).
- GL library pre-load (`ctypes.CDLL`) for arm64 wxPython in `emu.py` (matching
  existing fix in `run.py`).
- Early wx import check in `emu.py` with helpful error message when virtual
  environment is not activated.

### Fixed
- **Build system:** `setup.py` — replaced hardcoded numpy include path with
  dynamic `np.get_include()`.
- **Numpy 2.x overflow errors:** `atrip/filesystems/atari_dos2.py` — cast all
  `media[...]` subscript results to `int()` to prevent numpy scalar overflow in
  `get_file()`. Added `dtype=` parameter to `np.arange()` calls.
- **Numpy 2.x overflow errors:** `atrip/media_type.py` — cast sector/size to
  `int()` in `get_index_of_sector()`; added `dtype=` on `np.arange()`.
- **Numpy 2.x overflow errors:** `atrip/filesystems/atari_cas.py` — added
  `dtype=` on `np.arange()`.
- **Numpy 2.x overflow errors:** `atrip/machines/atari8bit/jumpman/parser.py`
  — mask level data bytes with `& 0xff` before `np.asarray(..., dtype=np.uint8)`
  to prevent negative-integer overflow; cast `hx`/`hy` to `int()` in
  `is_bad_harvest_position()` to suppress numpy underflow warnings.
- **Python 3 / wxPython 4 compatibility:** `sawx/ui/prefs_dialog.py`,
  `sawx/ui/popuputil.py`, `sawx/ui/tilemanager.py` — replaced `/` with `//` for
  integer division in wx drawing calls.
- **Python 3 SyntaxWarnings:** `sawx/utils/textutil.py`,
  `sawx/ui/exception_handler.py` — raw strings for regex and removed stray `\`
  in format strings.
- **NoneType crashes:** `sawx/frame.py` — guarded `set_title()` against
  `active_editor is None`.
- **NoneType crashes:** `sawx/application.py` — guarded `on_idle_after()`
  against `active_editor is None`.
- **NoneType crashes:** `omnivore/editors/byte_editor.py` — guarded
  `linked_base`, `segment_uuid`, `idle_when_active` against
  `focused_viewer is None`.
- **Editor layout restore crash:** `omnivore/editors/tile_manager_base_editor.py`
  — fixed `TypeError: string indices must be integers` in `restore_layout` when
  viewer metadata is a dict (not a list).
- **Gtk widget sizing assertions:** `sawx/ui/tilemanager.py` — clamped child
  panel sizes to `>= 1` and constrained header/minibuffer/footer sizes to
  available space to prevent `gtk_widget_set_size_request: height >= -1`.
- **LZW decompression:** `atrip/compressors/unix_compress.py` — handle string
  input in `bytearray()` for Python 3 compatibility.
- **README.rst** — updated with Ubuntu 24.04 build instructions, webkit2gtk
  version notes, arm64 wxPython notes, dependency checker usage, and editable
  install step.

### Changed
- Version bumped from 1.99.9 to 1.99.10.
