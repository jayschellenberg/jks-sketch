# JKS Sketch

Re-enters a hand-drawn field sketch as a clean, dimensioned plan with computed
areas. One self-contained file, `sketch.html`: open it in Edge or Chrome
(double-click). No install, no server, no network. Nothing leaves the machine.

**Outside the spine (ruled 2026-10-24, `docs/RULINGS.md`).** No script reads its output
and it changes nothing in the platform. Its files are filed into a job by hand,
like a photo. Wiring `areas.json` into the manifest or the workbook is held until
after the first delivery.

## Workflow

1. **Sketch box** (right panel): title (defaults to Schedule B's "Layout of
   Subject Improvements"), property, file number, preparer, date.
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
5. **Fix**: with *Select*, click an area. Edit its name, category, the
   "counts toward building area" box, or its **vectors** (the walls as text). Change
   a number and press *Apply vectors*. Click a dimension to hide it (shared
   walls); click it again to bring it back.
6. **Output**: *Save* (editable), *SVG*/*PNG*/*Print* (the plan), *areas.json* (the
   numbers). In Edge/Chrome a save dialog opens, so pick the job's `dropzone/`
   folder directly.

## Command language

Up is north on the page. Several commands can go on one line: `R40 D28 L40 C`.

| Type | Means |
|---|---|
| `R40` `L12.5` `U28` `D6` | wall right / left / up / down |
| `R40'6` `R40'6"` `R40-6` | 40 ft 6 in (converted when drawing in metres) |
| `R40'6-1/2"` `R40.54` | fractional inches, or decimal feet. Everything is computed in decimal feet; the plan shows `40.5'` or `40'6"` (toolbar) |
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
| `C` | close back to the start |
| `CW` | close along existing walls: follow the walls already drawn back to the start |
| `E` | end as an open line (fence, lot line) |
| Enter on empty line | close |
| Backspace on empty line | remove the last wall |
| Esc | cancel the area being drawn / clear selection |
| Ctrl+Z / Ctrl+Y | undo / redo |
| `25.5` then an arrow | number-pad drawing: the wall is drawn that way at once |
| Arrows (empty line) | nudge the selection one grid step; Shift ×10 |
| Ctrl+A / Ctrl+C / Ctrl+V | select all shown / copy / paste |
| Shift+click, drag on empty space | add to the selection / fence-select (Select tool) |
| Wheel / right-drag | zoom / pan |

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

**Area library.** *Library…* edits the categories: code (GLA1, GAR…), label,
counts toward building area, default factor, fill. Changes belong to this
sketch and are saved with it; *Save as my default* makes new sketches start
with the same list. *Label* in the toolbar chooses what each area's label
shows: code, name, size. *Interior / fence line* is for lines that are drawn
but never measured; an open line (`E`) becomes one unless it's a lot line.

**Dimensions.** Decimal places are set next to the format (0 to 0.000). With
*Select*, clicking a dimension hides it from the plan; on screen it stays faint
and struck through so it can be clicked back.

**Rotate and copy.** *Rotate…* turns the selection, or the whole sketch (and its
underlay) when nothing is selected, by any angle. Shift+click and fence-select
pick several items; drag, arrows, Delete, Ctrl+C and Rotate act on them all.

**Doors and windows.** Choose a kind (door, double door, window, overhead
door, plain opening) and width under *Doors, windows, symbols*, then with the
*Door/Window* tool click a wall: the opening is centred where you click,
snapped to the grid. Select the area to edit each opening: kind, wall (numbered
as in the vectors), centre distance from the wall's first corner, width, swing
*out* instead of into the room, and ⇄ to hinge at the other jamb. Openings
belong to their area: they move, rotate, mirror and copy with it, and never
change an area. Curved walls take no openings. `areas.json` lists each area's
openings (kind, wall, width).

**Symbols.** Click a symbol in the palette (stairs, toilet, sink, tub, shower,
range, refrigerator, washer/dryer, water heater, furnace, electrical panel,
fireplace, column, car, dock leveller, ramp, tree, arrow), then click the plan
to place it; keep clicking for more. Symbols are drawn at real size, so they
scale with the plan. Select one to rotate it (⟲ ⟳ 90°, or any angle) or resize
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

**Print scale.** Leave the scale box blank and the plan fits the page (the title
block says "Not to scale"). Enter a scale (`1" = 10'`, or `1:100` in metres) and
SVG and Print come out at that real size; *Paper* and orientation set the print
page, and the status bar warns when the plan won't fit.

## Limits, on purpose

- Curves are circular arcs only.
- Areas are independent shapes: `CW` copies the walls it traces, it doesn't
  link them.

## Future features

- **DISTO laser input**: a Leica DISTO over Web Bluetooth in Edge/Chrome filling
  the wall length as you measure.
