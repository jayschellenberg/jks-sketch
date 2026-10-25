# JKS Sketch

Re-enters a hand-drawn field sketch as a clean, dimensioned plan with computed
areas. One self-contained file, `sketch.html`: open it in Edge or Chrome
(double-click). No install, no server, no network. Nothing leaves the machine.

**Outside the spine (ruled 2026-10-24).** No script reads its output
and it changes nothing in the platform. Its files are filed into a job by hand,
like a photo. Wiring `areas.json` into the manifest or the workbook is held until
after the first delivery.

## Toolbar

One row on a laptop screen. Always visible: *File* menu (New, Open, Recover)
and *Save*; the tools (Draw, Select, Label, Door/Window, Symbol, Measure,
Photo); Undo, Redo, the *Edit* menu (select all, copy, paste, delete, rotate),
*Fit* and *auto*; the page and level being shown; *View* (units, dimension
format and decimals, what area labels show, the area library); *Underlay*
(load, calibrate, move, opacity, rotation, remove); *Export* (SVG, PNG, JPG,
areas.json, and page setup: paper, orientation, scale, smallest text, image
dpi, area table); *Print*; Help.

**Version.** The version (now 1.0, 2026-10-25) shows next to the name, in
Help, at the foot of every exported page, in saved sketches (`saved_with`)
and in `areas.json` (`generator`), so a shared copy can be told apart. It's
`APP_VERSION` near the top of the script; bump it with each release.

## Workflow

1. **Sketch box** (right panel): title (defaults to Schedule B's "Layout of
   Subject Improvements"), site name, address, file number, preparer, date.
   Each exported page is headed with the site name, or the address when there
   is no site name.
   *Notes* are printed at the foot of every page, one line each. By default:
   "Note: Placement of improvements on site is not to scale." and "For
   illustration only." Edit them per sketch, or clear the box to leave them off.
2. **Optional underlay.** *Image…* loads a photo or scan of the field sketch.
   *Calibrate*: click both ends of a wall whose length you know, type that length,
   and the image is scaled to match. *Move* drags it into place; the ° box rotates it.
   The underlay is never exported.
3. **Next area**: choose building, level and category *before* drawing. The
   category sets the fill colour, the code, whether the area counts toward
   building area and its default factor. You can change all of it afterwards.
4. **Draw**: click the starting corner. It snaps to an existing corner first, then
   to the grid. Then type the walls into the command line and press Enter.
   For an addition, start on the building, type only the new walls and finish
   with `CW` (below).
5. **Fix**: with *Select*, click an area. Its corners show handles: drag one
   to move it (Shift keeps it square), click one and press Delete to remove it,
   double-click a wall to add a corner. Double-click a dimension and type the
   new length: the wall stretches, and every corner beyond its far end moves
   with it, so square buildings stay square. Or edit the **vectors** (the walls
   as text) and press *Apply vectors*. Click a dimension once to hide it
   (shared walls); click it again to bring it back. Name, category, "counts
   toward building area" and the rest are under *Selected*.
6. **Output**: *Save* (editable), *SVG*/*PNG*/*JPG*/*Print* (the plan), *areas.json* (the
   numbers). In Edge/Chrome a save dialog opens, so pick the job's `dropzone/`
   folder directly.

## Command language

Up is north on the page. Several commands can go on one line: `R40 D28 L40 C`.

| Type | Means |
|---|---|
| `R40` `L12.5` `U28` `D6` | wall right / left / up / down |
| `R40'6` `R40'6"` `R40-6` | 40 ft 6 in (converted when drawing in metres) |
| `R40'6-1/2"` `R40.54` | fractional inches, or decimal feet. Everything is computed in decimal feet; the plan shows `40.5'` or `40'6"` (*View* menu) |
| `R10U5` | one diagonal wall, 10 across and 5 up (bays, clipped corners) |
| `R40~5` `R40~-5` | curved wall: corner to corner 40, middle of the curve 5 out of the room (`~-5` bows in). On an open line + bows left |
| `R40~r25` `C~5` | curve by radius instead of rise; the closing wall can curve too. Half-circle bay: `R12~6` |
| `R20~a31.4` `R20~d90` | curve by arc length (measured along the curve) or arc angle. `R12~d180` is a half circle |
| `17>45` `17<30` `17>` | turn: 17 long, turning right (`>`) or left (`<`) 45° off the line of the previous wall; no angle = 90° |
| `20@135` | length 20 at 135° (0° = right, counter-clockwise) |
| `N45-30E120.5` | survey bearing N 45°30′ E, 120.5 long. Use it for lot lines |
| `mR10` | move the start point 10 right without drawing (an addition that starts partway along a wall) |
| `U=` `L=` | run up (left…) until level with the start, so the close comes out square (one dimension missing from the field sketch) |
| `U*` `R*` | run up (right…) until meeting an existing wall, or one of this area's own |
| `C`, or **A** on an empty line | close back to the start. **A** (Apex's auto-close) closes along the building's walls (`CW`) instead when a straight close would cut a diagonal across a building the shape started and ended on |
| `CW` | close along existing walls: follow the walls already drawn back to the start |
| `E` | end as an open line (fence, lot line) |
| Backspace on empty line | remove the last wall |
| Esc | cancel the area being drawn / clear selection |
| Ctrl+Z / Ctrl+Y | undo / redo |
| `100` → Enter | number-pad drawing: click the start, type `100`, press → and the wall appears with its `100'` label (another arrow turns it); Enter confirms it. Then the next wall. Enter on an empty line does nothing, so a double Enter can't close the shape |
| Arrows (empty line) | nudge the selection one grid step; Shift ×10 |
| Ctrl+A / Ctrl+C / Ctrl+V | select all shown / copy / paste |
| Shift+click, drag on empty space | add to the selection / fence-select (Select tool) |
| Wheel / right-drag | zoom / pan |
| *auto* next to *Fit* (on by default) | re-fit the view after each wall confirmed with Enter, so the whole drawing stays in view; untick to stay zoomed in |

**Closure check.** If every typed wall is square and the closing wall comes out
diagonal, the status bar says so. A dimension was mistyped, or misread on the
field sketch. Typing all four walls of a rectangle closes it exactly.

**Attached areas (`CW`).** Start the addition on the building (click a corner,
or `mR10` along a wall), type its own walls until you are back on the
building, then `CW`. It follows the walls already drawn back to the start, even
round corners, along curved walls and across two buildings that touch. Where
there is a choice, it takes the route that doesn't enclose the building you're
attaching to. The traced walls are copied into the new area, so editing the
building later doesn't move the addition; their dimensions are hidden because
the building already shows them. While you type a plain `C`, the status bar
points out when `CW` would close along existing walls instead of a diagonal.

**Levels.** *Show* picks one level: the others are hidden, new areas go on that
level, and SVG, PNG and Print export just that level (the title and file name
say which, and the table gives that level's area and the building's total). To
start an upper floor from the main floor: select the main floor, Ctrl+C, show
the upper level, Ctrl+V. The copy lands in place on that level.

**Area library.** *View ▸ Area library…* edits the categories: code (GLA1, GAR…), label,
counts toward building area, default factor, fill. Changes belong to this
sketch and are saved with it; *Save as my default* makes new sketches start
with the same list. *View ▸ Area labels show* chooses what each area's label
shows: code, name, size. *Interior / fence line* is for lines that are drawn
but never measured; an open line (`E`) becomes one unless it's a lot line.

**Dimensions.** Decimal places are set next to the format (0 to 0.000).
*Dims side* under *Selected* puts an area's dimensions inside or outside its
walls (for every selected area). With
*Select*, clicking a dimension hides it from the plan; on screen it stays faint
and struck through so it can be clicked back.

**Rotate and copy.** *Edit ▸ Rotate…* turns the selection, or the whole sketch (and its
underlay) when nothing is selected, by any angle. Shift+click and fence-select
pick several items; drag, arrows, Delete, Ctrl+C and Rotate act on them all.

**Doors and windows.** Choose a kind (door, double door, window, truck dock
door, grade-level overhead door, plain opening), width and optional height
under *Doors, windows, symbols*, then with the *Door/Window* tool click a wall:
the opening is centred where you click, snapped to the grid. Select the area to
edit each opening: kind, wall (numbered as in the vectors), centre distance
from the wall's first corner, width, height, swing *out* instead of into the
room, and ⇄ to hinge at the other jamb. Openings belong to their area: they
move, rotate, mirror and copy with it, and never change an area. Curved walls
take no openings.

**Dock and grade doors.** A truck dock door (default 9'×10') draws with dock
bumpers; a grade-level overhead door defaults to 12'×14'. Both carry a size tag
outside the wall (`DH 9'×10'`, `GL 12'×14'`) and are counted per building in
the calculations footer, the printed table (`2 × 9'×10'`) and `areas.json`
(`dock_doors` and `grade_level_doors`: count and sizes). Overhead doors from
older sketches are grade-level doors.

**Year built and ceiling height.** Select an area (a section) to note its year
built and its ceiling height: one height, or several separated by commas
(`16, 24'6`). Tick *View ▸ details* to show them on the plan. They also
show in the area calculations, in a *Built / ceiling* column of the printed
table (only when some section has them), and in `areas.json` (`year_built`,
`ceiling_heights`). Ceiling heights convert with the drawing's units.

**Perimeters.** The *Areas* summary has a *Perim* column: each area's own
perimeter, and on each "Combine as" total and each building level the
*outside* perimeter, where a wall shared by two of its areas doesn't count.
`areas.json` gives `combined[].outer_perimeter` and per building
`outer_perimeter_by_level`.

**Symbols.** The *Doors, windows, symbols* section of the panel starts folded;
the Door/Window and Symbol tools open it. Click a symbol in the palette (stairs, toilet, sink, tub, shower,
range, refrigerator, washer/dryer, water heater, furnace, electrical panel,
fireplace, column, car, dock leveller, ramp, tree, arrow, north arrow), then click the plan
to place it; keep clicking for more. Symbols are drawn at real size, so they
scale with the plan, except the *North arrow*, which stays a fixed size and,
placed on a page (turned to true north if the plan isn't), replaces that
page's corner north arrow. Select one to rotate it (⟲ ⟳ 90°, or any angle) or resize
it. They copy, rotate with the sketch and sit on the level being shown, like
labels.

**Room labels.** Type a room name in *Label text* (it offers common ones and
the ones you've used), then each click of the Label tool places it.

## Files it writes

| File | What it is |
|---|---|
| `<file no.> sketch.sketch.json` | the editable drawing (with underlay). Reopen with *Open* |
| `<file no.> sketch.svg` / `.png` | the plan: title block, north arrow, dimensions, labels, optional area table |
| `<file no.> sketch areas.json` | every closed area: name, building, level, category, counts-toward-building-area, area in **both** sq ft and m², perimeter; per-building totals by level and category; site area |

**Separate or combined.** Each area reports its own size. Give adjoining areas
the same *Combine as* name (e.g. `GLA`) and the plan shows one combined total,
drawn as one shape: walls between the parts (same building and level) are
light and dashed, with no dimensions. The table and `areas.json` (`combined`)
still carry each part as well. Leave it blank to keep an area separate.

**Area calculations** (right panel, after Apex's Calcs panel). A tree of
building → category → area. Each area opens to base area, factor, net area and
perimeter. *Expand all* opens everything; *Collapse all* keeps each area's net
figure and folds the detail. Underneath: defined areas, building area per
building, combined totals, site area.

**Factor and negative.** New areas take their category's default factor from
the library. Select an area to set its own *Factor* (net = base × factor,
e.g. 0.5 for a basement counted at half) or tick *Negative* to deduct it (a
stairwell or atrium drawn inside a floor). Every total, the plan label and the
table use the net. A factored area's table row shows the working, e.g.
`Basement (1,120 sf × 0.5)`. Defaults are 1.00 and positive, so older sketches
come out unchanged.

`areas.json` has `"format": "jks-sketch-areas", "version": 1`. Each area carries
`base_area`, `factor`, `negative` and `area`, which is the net. Building area is
the sum of the areas ticked "counts toward building area". Defaults: main/upper
floor, office, retail, warehouse, residential unit and common area are ticked.
Basement, garage, deck, mezzanine, canopy and other are not. Site areas never
count. The appraiser decides, per area.

The browser also autosaves the last sketch locally. That is a convenience, not a
record: *Save* is the record.

**Site improvements and coverage.** Paving, gravel and landscaping go in the
*Asphalt paving*, *Concrete paving*, *Gravel* and *Landscaped* categories
(added to older sketches' libraries automatically). They're totalled on their
own and never count as building area. *Site coverage* = building footprint ÷
site area, shown with the totals, in the printed table and in `areas.json`
(`building_footprint`, `site_coverage_pct`, `site_improvements`). Each area's
*Footprint* box says whether the ground it covers counts; it follows the
category (on for main floors, garages, warehouses, offices…; off for upper
floors, basements, mezzanines, decks, canopies) and can be changed per area.

**Measure.** The *Measure* tool draws a free dimension line between two
clicked points (snapping to corners; Shift keeps it square) for setbacks,
spacing between buildings or apron depth. Label it (e.g. *Setback*) under
*Selected*; *Flip text* puts the text on the other side. Listed in
`areas.json` as `measurements`.

**Photo markers.** The *Photo* tool: click where a photo was taken and drag
the way the camera faced. Markers number themselves 1, 2, 3… (change a number
under *Selected*), print at the minimum text size, and are listed with their
compass facing in `areas.json` as `photos`.

**Recover.** Besides the current sketch, the browser keeps the last dozen
sketches you worked on. *File ▸ Recover…* lists them (file, site, areas, last change)
and reopens one. It's a safety net for a sketch replaced before it was saved;
*Save* is the record.

**Pages.** *Page* in the toolbar picks the page; its *⋯* menu adds a page (site
plan, each building, each floor), renames one, or deletes one (Undo brings it
back). Each page is its
own drawing; totals, the area calculations and `areas.json` cover every page,
and on a page's table the totals say "(all pages)". Each sheet is headed with
the title and page name, the site name or address, and "Page 2 of 3". Copy on
one page and paste on another to start from the same outline, in place.

**Cold and semi-cold storage.** Set *Storage* (under *Next area* before
drawing, or under *Selected*) to *Semi-cold storage* or *Cold storage*. Those
sections are hatched on the plan (single hatch semi-cold, cross-hatch cold),
always labelled with their class, and totalled apart from the main building
area, which then reads "building area (heated)". `areas.json` gives each area's
`storage` (`heated`, `semi_cold`, `cold`) and per building
`semi_cold_storage_area`, `cold_storage_area` and `total_including_storage`;
`building_area` is the heated GBA.

**Pages out: fit, text size, PDF, JPG.** Every export is a real page: the
chosen *paper* and orientation less a 0.4" margin, or *Report figure*
(6.5 × 9", no margin) to drop into a letter-size report at full size.

- *Scale* blank: the plan, title block and table are fitted to the page in
  both directions, so a sheet is always one page. A scale (`1" = 10'`, or
  `1:100` in metres) draws at that size and warns when it won't fit.
- *min … pt* (10 by default) is the smallest text on the finished page;
  dimensions, labels, door tags, the table and the title all scale up from it.
  A dimension too long for its wall at that size is shortened (a curve drops
  its rise) or left off, and the status bar says how many. Letters inside a
  symbol (REF, W, D) that would print smaller are left off.
- *PNG* and *JPG* are the page at the chosen *dpi* (300 by default). Placed at
  their true page size, their text is at least the minimum.
- *Print* (and Save as PDF) makes one sheet per page. SVG, PNG and JPG are of
  the page being shown.

## Limits, on purpose

- Curves are circular arcs only.
- Areas are independent shapes: `CW` copies the walls it traces, it doesn't
  link them, and stretching or moving one area's corner doesn't move an
  attached area's.
- Site coverage adds footprints; two footprint areas drawn on top of each
  other on the same ground count twice.

## Future features

- **DISTO laser input**: a Leica DISTO over Web Bluetooth in Edge/Chrome filling
  the wall length as you measure.
