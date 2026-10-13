IMAGE QUALITY EVALUATION — OFFLINE RANKING AND PAIRWISE COMPARISON

Open index.html in a desktop browser. Keep the images folder beside it.
No server, installation, account, internet connection, or native JPEG XL support
is required. If supplied, Image Quality Evaluation standalone.html contains all image assets
and can be moved on its own, but takes substantially more memory to open.

Rank the three samples from best (left) to worst (right) by dragging a sample's
header. The arrow buttons also work with a mouse, touch, or keyboard. On narrow
screens, the ranking runs from top to bottom. Select ties if you cannot see a
difference. Submit and continue explicitly submits a judgment; merely opening
a trial or moving a sample never submits the initial order.

PAIRWISE MODE (OPTIONAL)

Settings > Use pairwise comparisons enables two-sample evaluation. Select
Left better, Indistinguishable, or Right better, then Submit and continue.
A preference is required; merely opening a pair never submits a judgment.
The two samples keep fixed left/right positions, including on narrow screens.
All three encoder pairs are included for every source and distance: 279 trials
in total, or 93 per distance. The existing image files are reused.

Each mode keeps its own queue, anonymous assignments, current comparison,
drafts, and submitted judgments. Switching modes resumes that mode's progress.
Participant label, distance filter, zoom, and display settings are shared.
Three-way rankings are never converted into pairwise judgments. Existing
sessions and older backups resume in three-way mode with their progress intact.

Pairwise scheduling uses nine randomly ordered blocks, three per distance.
Each block visits all 31 sources once, selecting a different encoder pair for
each source on each visit to that distance. Within a block, source order is
randomized with a spacing constraint: at least eight other comparisons separate
repeated source images, both overall and when filtered to a single distance.
Left/right assignments are balanced within every encoder pair and distance,
to within one presentation, and are saved for the entire session.

Confidence and observations are hidden by default. Enable them in Settings
(the gear in the top-right corner). This preference is saved with the session;
hiding the fields preserves any existing entries.

All displayed viewports share zoom and pan. Drag inside an image to pan when zoomed.
Inspect opens a larger viewer; 1/2/3 (1/2 in pairwise mode) switch samples, and
Escape closes the viewer. Original references are disabled by default. Enable
them inside Settings (the gear in the top-right corner); the Source tab is then
available in Inspect, and R selects the reference. At 100%, one image pixel
occupies one CSS pixel (physical display pixels depend on display scaling).
Fit resizes the displayed samples to the same scale. Zooming above 100% uses
nearest-neighbor display, so interpolation does not hide fine artifacts.

Three-way mode has 93 trials: 31 sources at distances 1, 5, and 10, with three saved
effort-7 encoder configurations per trial: GJXL, GJXL adaptive EPF, and libjxl.
Each comparison uses the SAME source and distance; it is not a matched-bitrate
study. Trial order and label-to-encoder assignments are randomized for every
new session and saved. Assignments are balanced over the six permutations
within each distance, to within one presentation. Labels A/B/C have no stable
encoder meaning across trials. Identities, sizes, and metric scores are hidden
in the study interface. This is participant blinding, not encryption: someone
inspecting source files, developer tools, or exported CSV can find identities.

The builder verifies every source and JXL against today's encoding manifests.
Display images are full-resolution PNGs decoded with djxl v0.12.0, explicit
16-bit output, and sRGB (RGB_D65_SRG_Rel_SRG). No images are resized or encoded
as JPEG. Original PNGs are copied unchanged for references. The browser and
display still determine final color rendering. display-manifest.json retains
decoder provenance and hashes. LICENSE.txt covers the source photos (CC0).

SAVING AND EXPORT

Every drag, tie, preference, note, and submitted judgment is saved in localStorage when the
browser permits it. Reopen the SAME path in the SAME browser/profile to resume.
Moving the file, changing browser, private browsing, or clearing browser data
can make that storage unavailable. An on-page notice reports saving errors.
Settings > Download backup exports both modes in a portable JSON copy; Restore
backup imports both modes after validating the dataset and every trial. A backup is the reliable way
to move progress between machines, file paths, or browsers. Images are never
put into localStorage. Avoid editing one session in several tabs at once.

Export CSV includes SUBMITTED judgments from the SELECTED MODE only, across
all distances. Filenames and the comparison_mode column identify the method.
Exports reveal encoder identities; avoid reading them until the study is done.

Three-way exports retain three rows per trial. Rank 1 means best; ties use
competition ranks: 1,1,3 or 1,2,2 or 1,1,1. Pairwise exports use one row per pair,
recording left/right encoders, preference (left/right/tie), and the winner
(blank for ties). They include the pair ID and its original source-trial ID.
Both formats contain source/distance, original encoded bytes and SHA-256,
display PNG SHA-256, notes, confidence, reference use, timestamps, view settings,
and active viewing time (excludes background-tab time).
Text cells that could be interpreted as spreadsheet formulas are escaped.
Draft edits to a submitted judgment are saved separately and only replace its
exported result when Update and continue is pressed.

Settings > New session asks for confirmation before replacing current browser
progress in BOTH modes. The selected mode stays active. Export a JSON backup
first if you want to retain the previous session.

REBUILD FROM SAVED ENCODING RESULTS

From the gjxl repository:
  python3 tools/blind_quality/build.py --output "/path/to/Quality Evaluation"
Add --single-file to also create the self-contained embedded HTML.
The only preparation dependency is the existing djxl CLI; Python uses its
standard library. The app itself uses only HTML, CSS, and vanilla JavaScript.
Rebuilding does not run an encoder, benchmark, or metric calculation.

CHECKS

Run the scheduling, legacy-migration, backup-validation, and CSV checks with:
  node tools/blind_quality/verify_pairwise.cjs "/path/to/Quality Evaluation/index.html"
The checks create artificial judgments in memory and do not modify app sessions.
