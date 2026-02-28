# Auto-Snap Vertices

A FreeCAD macro that automatically snaps nearby unrestrained vertices in a sketch by adding coincident constraints. Useful for cleaning up imported geometry, DXF/SVG traces, or any sketch where endpoints are close but not properly joined.

## Quick Start

1. Open a sketch in FreeCAD
2. Select the sketch object in the model tree
3. Run `auto_snap_vertices.FCMacro` (Macro > Macros... > select > Execute)
4. Configure options in the dialog and click **Apply**

## Installation

Copy `auto_snap_vertices.FCMacro` into your FreeCAD Macro directory:

- **macOS:** `~/Library/Application Support/FreeCAD/Macro/`
- **Linux:** `~/.local/share/FreeCAD/Macro/`
- **Windows:** `%APPDATA%\FreeCAD\Macro\`

## Configuration Options

When you run the macro, a dialog appears with the following settings:

### Snap Tolerance

**Maximum snap distance** (default: 0.1 mm)

The maximum distance (in mm) between two vertices for them to be considered snap candidates. Only vertex pairs closer than this value will be joined. Adjust based on your geometry:

- **Tight tolerance (0.01 - 0.05 mm):** For precision work where only nearly-overlapping points should snap. Good when you have intentionally close but separate vertices.
- **Default (0.1 mm):** A reasonable starting point for most imported geometry.
- **Loose tolerance (0.5 - 5.0 mm):** For rough sketches or poorly imported geometry where gaps may be larger. Use with preview mode first to verify results.

### Options

#### Preview Mode

When enabled, the macro shows a detailed report of every snap it *would* make, without actually applying any changes. Each candidate displays:

- The vertex pair (geometry index and point type)
- The distance between them
- The XY coordinates of both points
- Whether the constraint is valid or potentially redundant

Use this to audit what the macro will do before committing, especially with loose tolerances.

#### Skip Construction Geometry

When enabled, vertices belonging to construction lines/circles are excluded from snapping. Useful when your sketch has construction geometry used as reference that shouldn't be joined to real geometry.

#### Skip External Geometry

When enabled, vertices from external references (edges linked from other sketches or features) are excluded from snapping.

#### Validate Constraints (recommended)

Enabled by default. Before adding each coincident constraint, the macro checks whether an identical or equivalent constraint already exists. This prevents:

- Duplicate coincident constraints on the same vertex pair
- Constraint conflicts that could over-constrain the sketch
- Redundant constraints where two points are already indirectly related through a shared third point

Disabling this may be slightly faster on very large sketches but risks adding redundant constraints.

## How It Works

The macro operates in several phases:

### 1. Constraint Map

Before analyzing vertices, the macro scans all existing constraints in the sketch and builds an internal map of relationships. It tracks:

- **Coincident constraints** — direct point-to-point connections
- **PointOnObject, Symmetric, and Fixed constraints** — other relationships that indicate a vertex is already positioned

This map is used throughout the process to avoid creating redundant constraints.

### 2. Vertex Collection

All vertices in the sketch are collected, including:

- **Start points** of lines, arcs, etc.
- **End points** of lines, arcs, etc.
- **Center points** of circles and arcs

Vertices are filtered based on your construction/external geometry settings.

### 3. Candidate Matching

For each unconstrained vertex, the macro finds the nearest unconstrained neighbor within the snap tolerance. It skips pairs that:

- Are the same vertex
- Are already constrained (directly or indirectly)
- Have already been matched as part of another pair

Each pair is processed only once (A-to-B, never also B-to-A).

### 4. Constraint Application

For each candidate pair, the macro:

1. Validates that the constraint won't duplicate an existing one (if validation is enabled)
2. Creates a `Coincident` constraint joining the two vertices
3. Updates its internal constraint map so subsequent pairs account for the new relationship
4. Logs the result to the FreeCAD console (Report View)

After all constraints are applied, the sketch is recomputed.

## Console Output

The macro logs progress and results to the FreeCAD Report View (`View > Panels > Report View`):

- Number of vertices found and analyzed
- Number of existing constrained points
- Each successful snap with vertex IDs and distance
- Warnings for skipped redundant constraints
- Errors if any constraint fails to apply

## Tips

- **Always save your file before running.** While the macro is careful about constraint validation, snapping many vertices at once on complex sketches can occasionally produce unexpected results.
- **Start with preview mode** on a new sketch to understand what the macro will do before applying changes.
- **Use tight tolerances first**, then widen if needed. It's easier to run the macro again with a looser tolerance than to undo unwanted snaps.
- **Check the Report View** after running to see exactly what was snapped and whether any constraints were skipped.

## License

MIT
