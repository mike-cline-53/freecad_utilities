# FreeCAD Utilities

A collection of FreeCAD macros and utility scripts.

## Macros

### [auto_snap_vertices](auto_snap_vertices/)

Automatically snaps nearby unrestrained vertices in a FreeCAD sketch by adding coincident constraints. Features include configurable snap tolerance, preview mode, construction/external geometry filtering, and constraint validation to prevent redundant constraints.

**Usage:**
1. Open a sketch in FreeCAD
2. Select the sketch object
3. Run `auto_snap_vertices.FCMacro`
4. Configure options in the dialog and apply

### [smart_select](smart_select/)

Select similar faces and edges in a FreeCAD document based on configurable criteria such as area, length, and position. Originally by [WayofWood](https://github.com/WayofWood).

**Usage:**
1. Select a face or edge in FreeCAD
2. Run `smart_select.FCMacro`
3. Choose your matching criteria (same object, same area, same X/Y/Z, precision)
4. Matching geometry will be added to the selection

## Installation

Copy the contents of any macro folder into your FreeCAD Macro directory:

- **macOS:** `~/Library/Application Support/FreeCAD/Macro/`
- **Linux:** `~/.local/share/FreeCAD/Macro/`
- **Windows:** `%APPDATA%\FreeCAD\Macro\`

## License

Individual macros may have their own licenses. See each macro file for details.
