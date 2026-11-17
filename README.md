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

1. **Sketch box** (right panel): title (defaults to "Sketch of Subject Improvements (For
   Illustration)"), address, city, file number. There's no date field: printed
   and exported files are named with the day they're made, and `areas.json`
   records when it was generated.
   Each exported page is headed with the address and city.
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
   to the grid (*Grid snap* in the View menu, 1 by default). Then type the walls into the command line and press Enter,
   or click the corners: the next wall follows the mouse (dashed, with its length), snapping to existing
   corners, and locks only when you click or press Enter. Clicking the start point closes the area. After an area
   closes, the next click on the plan starts a new one.
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

**Adding onto a building (a covered area, an addition).** Set *Next area*
(e.g. *Canopy / covered*), then with *Draw* click the building's corner where
the new part starts: the click snaps onto the corner and the cursor goes to the
command box. Type its walls (`12` ↓ Enter, `40` → Enter, `12` ↑ Enter) and
press **A** to close. The wall along the building is closed for you, along the
building's own walls if the shape starts and ends on it.

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

**Area labels and callouts.** Each area's label is its name (always bold),
its size, its type if unheated or covered, and "Built 1985 | Ceiling 12' / 16'" on
one line. When the label doesn't fit inside the area at that text size (a
small area, or the plan scaled down to fit the page), it's drawn as a
**callout**: a box outside the area with a leader line to a dot in it.
Each callout takes the first direction (its own, then the other compass
points) that keeps it on the page and clear of the plan and other callouts;
on a page export the plan is made as large as it can be with every callout
still on the page. Drag a callout to move it.

**Dimensions.** Decimal places are set next to the format (0 to 0.000).
*Dims side* under *Selected* puts an area's dimensions inside or outside its
walls (for every selected area). With
*Select*, clicking a dimension hides it from the plan; on screen it stays faint
and struck through so it can be clicked back.

**Rotate and copy.** `[` and `]` turn the selection 15° counter-clockwise /
clockwise; with Shift, 5°. The same steps are buttons at the top of *Selected*,
and *Edit ▸ Rotate…* takes any angle. With nothing selected they turn the whole
page, with its underlay and its north arrow. A selected label turns its text;
a whole-page turn keeps labels level. Shift+click and fence-select
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
outside the wall (`DK 9'×10'`, `GR 12'×14'`) and are counted per building in
the calculations footer, the printed table (`2 × 9'×10'`) and `areas.json`
(`dock_doors` and `grade_level_doors`: count and sizes). Overhead doors from
older sketches are grade-level doors.

**Year built and ceiling height.** Select an area (a section) to note its year
built and its ceiling height: one height, or several separated by commas
(`16, 24'6`). They show on the plan under the area's name and size, one line
each ("Built 1985", "Ceiling 16' / 24.5'"); untick *View ▸ details* to hide them. They also
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

**Labels (free text).** The *Label* tool places text anywhere: type it in
the box that appears beside the *Label* button (it offers common room names
and the ones you've used) and each click places it, or leave that empty and type when asked.
Double-click a label to change its text; under *Selected* set its size, bold
and angle (or turn it with `[` `]`).

**North.** The page's north arrow (top right, on screen and on every page)
points where *View ▸ North points* says, in degrees counter-clockwise from up,
and turns with the drawing when you rotate the whole page. A *North arrow*
symbol placed on a page replaces it there and turns like any symbol.

**Moving doors, windows and symbols.** With *Select*, drag a door or window:
it slides along its wall, or onto another wall, centre snapped to the grid.
Drag a symbol anywhere. The *Door/Window* and *Symbol* tools do the same when
you click on one that's already placed (clicking elsewhere places a new one).

## Files it writes

| File | What it is |
|---|---|
| `Sketch.sketch.json` (`Sketch-<file no.>…` with a file number, e.g. `Sketch-26109175-20261027.pdf`) | the editable drawing (with underlay). Reopen with *Open* |
| `Sketch-20261029.svg` / `.png` / `.pdf` (Print → Save as PDF), dated the day it's made | the plan: title block, north arrow, dimensions, labels, optional area table |
| `Sketch areas.json` | every closed area: name, building, level, category, counts-toward-building-area, area in **both** sq ft and m², perimeter; per-building totals by level and category; site area |

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

**New job.** *File ▸ New* starts clean: an empty sketch and every setting
back to its starting value (Next area: Building 1 / Main / Main floor /
heated; doors and windows; view and units; underlay; export: Letter,
portrait, fit, 12 pt, 300 dpi, Totals). Kept: your saved area-library
default, auto-fit, and anything copied, so it can be pasted into the new job.

**Typing.** After clicking the start corner, just type: the click never takes
the cursor out of the command box, and with Draw on, typing anywhere goes to
it. A length waiting for its arrow shows on the plan beside the corner
(`50  → ← ↑ ↓`).

**Recover.** Besides the current sketch, the browser keeps the last dozen
sketches you worked on. *File ▸ Recover…* lists them (file, site, areas, last change)
and reopens one. It's a safety net for a sketch replaced before it was saved;
*Save* is the record.

**Pages.** *Page* in the toolbar picks the page; its *⋯* menu adds a page (site
plan, each building, each floor), renames one, or deletes one (Undo brings it
back). Each page is its
own drawing; totals, the area calculations and `areas.json` cover every page,
and on a page's table the totals say "(all pages)". Each sheet is headed with
the title and page name, the address and city, and "Page 2 of 3". Copy on
one page and paste on another to start from the same outline, in place.

**Heated, unheated and covered.** Set *Type* (under *Next area* before
drawing, or under *Selected*) to *Heated* (the default), *Unheated* or
*Covered*. Unheated and covered sections are hatched on the plan (single hatch
unheated, cross-hatch covered), always labelled with their type, and totalled
apart from the main building area, which then reads "building area (heated)".
`areas.json` gives each area's `type` (`heated`, `unheated`, `covered`) and per
building `unheated_area`, `covered_area` and
`total_including_unheated_and_covered`; `building_area` is the heated GBA.
Older sketches with semi-cold or cold storage open with those sections as
unheated.

**Pages out: fit, text size, PDF, JPG.** Every export is a real page: the
chosen *paper* and orientation less a 0.4" margin, or *Report figure*
(6.5 × 9", no margin) to drop into a letter-size report at full size.

- *Scale* blank: the plan, title block and table are fitted to the page in
  both directions, so a sheet is always one page. A scale (`1" = 10'`, or
  `1:100` in metres) draws at that size and warns when it won't fit.
- Nothing on a finished page is smaller than **12 pt**: *Smallest text* starts
  at 12 and can only be raised; dimensions, labels, door tags, the table and
  the title all scale up from it. On screen the sketch text uses the same
  proportions (nothing under 12 px).
- Pages default to **portrait**. On a narrow page the title shrinks to fit
  beside the file / page lines (or those go under it), and the area table
  sizes its columns to the page: if they still don't fit, the Built / ceiling
  / type details go on a line under each area, then a Building / level
  column that's the same on every row moves into the Area heading.
  A dimension too long for its wall at that size is shortened (a curve drops
  its rise) or left off, and the status bar says how many. Letters inside a
  symbol (REF, W, D) that would print smaller are left off.
- *PNG* and *JPG* are the page at the chosen *dpi* (300 by default). Placed at
  their true page size, their text is at least the minimum.
- *Under the plan* (Export menu) chooses what's printed below the drawing.
  **Totals** (the default) is a short strip of the core figures, wrapping only
  when needed: GBA (heated when unheated or covered space is split out), the
  unheated and covered areas, combined totals that differ from GBA, the level shown, site area,
  site improvements, footprint and site coverage, then the dock and grade
  doors by size (e.g. `Doors: 1 DK 9'×10', 1 GR 12'×14'`). The per-area
  details are already on the plan. **Full area table** prints the table
  instead; **Nothing** leaves the space to the plan. The page notes run
  together as one paragraph.
- *Print* (and Save as PDF) makes one sheet per page. SVG, PNG and JPG are of
  the page being shown.
- *Export ▸ Copy page to clipboard* (Ctrl+Shift+C) copies the page as a
  picture to paste into Word, Outlook or an email. Word shrinks a pasted
  picture to fit; use *Report figure* paper so its text stays full size.

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
