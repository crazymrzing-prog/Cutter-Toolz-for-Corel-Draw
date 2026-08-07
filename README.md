# Cutter Toolz

I'm not a Code writer much of this was created using AI

Two CorelDRAW VBA macros — **Quik Cutz** and **Snake Cutz** — for turning a single sticker/cut shape into a full sheet layout automatically, plus a workspace file that adds toolbar buttons for both. Ported from the "Quik Cutz" and "Snake Cutz" Inkscape 1.x extensions.

## What's in this repo

| File | Description |
|---|---|
| `Cutter_Toolz.cdws` | CorelDRAW workspace — adds a toolbar with buttons that run both macros |
| `QuikCutz.gms` | Quik Cutz macro project (grid cutting tool) |
| `SnakeCutz.gms` | Snake Cutz macro project (chained circle/ellipse cutting tool) |

---

## Installation

### Option 1: Import the Workspace file (`.cdws`) — easiest

Installs everything, including the toolbar buttons, in one step.

1. Open CorelDRAW.
2. Go to **Tools > Customization** (or **Tools > Options**, depending on version).
3. Click **Workspace** in the left-hand tree (the top-level item).
4. Click **Import/Export...** at the bottom of the dialog.
5. Choose **Import a workspace from file**, browse to `Cutter_Toolz.cdws`, and select it.
6. Follow the wizard, make sure everything (toolbars, commands, macros) is checked, then **Finish**.
7. Restart CorelDRAW when prompted.
8. If the toolbar isn't visible, enable it via **Window > Toolbars > Cutter Toolz**.

### Option 2: Install the `.gms` macro files manually

Copy the `.gms` files into CorelDRAW's macro (GMS) folder. There are two valid locations depending on your setup — use whichever one already has other `.gms` files in it, or create the folder if it doesn't exist yet.

**Option A — Program Files (all users):**
```
C:\Program Files\Corel\CorelDRAW <YourVersion>\Draw\GMS\en\
```

**Option B — AppData Roaming (current user only):**
```
C:\Users\<YourUsername>\AppData\Roaming\Corel\CorelDRAW <YourVersion>\Draw\GMS\en\
```

Steps:
1. Close CorelDRAW if it's open.
2. Copy `QuikCutz.gms` and `SnakeCutz.gms` into one of the folders above (replace `<YourVersion>` and `<YourUsername>` with your actual CorelDRAW version and Windows username).
3. Open CorelDRAW.
4. Go to **Tools > Macros > Manage Macros** (or press **Alt+F11** to open the VBA editor and confirm the projects loaded).
5. The `QuikCutz` and `SnakeCutz` projects should now appear in the macro list.
6. Run a macro via **Tools > Macros > Run Macro**, or assign it to a toolbar button/keyboard shortcut for quicker access.

---

## Plugin Overview

### Quik Cutz — grid layout from a single sticker

Select **one native rectangle** (the cut line) plus, optionally, any artwork you want repeated with it (image, text, group), then run the macro.

- **Fill Area sizing** — you enter a target width/height in mm; the number of columns and rows is worked out automatically from the cut shape's own size (no need to count copies yourself).
- **Build direction** is fixed — the grid always builds right and down from your original selection, which stays put as the top-left cell.
- **Stub / Join Options** — one 5-way choice controls how the cut lines are finished:
  1. **Plain grid** — no stubs, no joins, no border.
  2. **Stub extensions only** — each line overshoots its true end by a set length (default 5 mm), useful for reliable pierce/tie-off points; ends stay unjoined.
  3. **Stubs + join with connectors** — every horizontal divider becomes one continuous path, every vertical divider becomes another, with automatically filleted (rounded) corners at each join.
  4. **Stubs + connectors + join both paths** — everything in option 3, plus the horizontal and vertical paths are joined into a single path with one sharp corner.
  5. **Stubs + border frame** — adds a rectangle traced around the whole grid, in addition to the two cut paths.
- **Keep Original Object(s)** — off (default) removes the original cut rectangle and folds your original artwork into the output; on leaves your original selection completely untouched and duplicates fresh copies into every cell.
- **Demo/test mode** — colors the horizontal path red and vertical path blue so you can sanity-check the layout before cutting.
- **Output** — cut lines are grouped as `QuikCutz` (`QuikCutz_1`, `QuikCutz_2`, ...); duplicated artwork is grouped separately as `Images` (`Image_1`, `Image_2`, ...).

### Snake Cutz — chained circle/ellipse layout

Select **one native circle or ellipse** (the cut line) plus, optionally, artwork to repeat, then run the macro. Rectangles, stars, or converted paths aren't accepted — the shape must be a true circle/ellipse.

- **Fill Area sizing** — same idea as Quik Cutz: give a target width/height in mm and the number of copies per chain, and the number of parallel chains, are calculated automatically.
- **Build direction** — Horizontal (left to right) or Vertical (top to bottom); each circle sits tangent to the next with no gap.
- **Multiple chains** — extra parallel chains are added to fill the perpendicular dimension, spaced by the shape's diameter plus a "Gap between chains" value (mm) that you set.
- **Connect chains** — on (default) joins every chain into one continuous cut path; off leaves each chain as its own separate closed path.
- **Keep Original Object(s)** — same behavior as Quik Cutz: off folds your original selection into the output, on leaves it untouched.
- **Demo/test mode** — draws the forward pass in red, return pass in blue, and connectors in green as separate preview paths.
- **Output** — cut lines are grouped as `SnakeCutz` (`SnakeCutz_1`, `SnakeCutz_2`, ...); duplicated artwork is grouped separately as `Images` (`Image_1`, `Image_2`, ...).

### Cutter_Toolz (workspace)

Not a macro itself — a CorelDRAW workspace file that adds a toolbar with one-click buttons wired to both macros above, so you don't need to open the Macros menu every time.

---

## Notes

- Both macros are ports of existing Inkscape 1.x Python extensions (`quik_cutz_grid.py` and `snake_cutz.py`), rewritten against CorelDRAW's VBA object model.

