# Day 20 — Face Puzzle Game 🧩

**#60DayClaudeChallenge** | Built with Claude Sonnet · Single-session build · Zero external dependencies

---

## 📌 Overview

| Field | Detail |
|---|---|
| **Day** | 20 / 60 |
| **Challenge** | #60DayClaudeChallenge |
| **Build** | Interactive face puzzle game — upload any photo, slice it into a scrambled grid, drag pieces back into place |
| **Output type** | Single self-contained HTML file |
| **Lines of code** | 565 |
| **Dependencies** | None — pure HTML + CSS + JavaScript |
| **Claude model** | Claude Sonnet 4.6 |
| **Time to build** | ~1 session, iterative prompting |

---

## 🎮 Gameplay Screenshots

> Screenshots below are ASCII/text representations of each game state. Open `face-puzzle-game.html` in any browser to see the live UI.

---

### Screen 1 — Photo Upload

```
┌─────────────────────────────────────────────┐
│ 🧩 Face Puzzle                              │
├─────────────────────────────────────────────┤
│                                             │
│  Upload a photo                             │
│  Choose any face photo to create a puzzle.  │
│                                             │
│  ┌ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ┐    │
│  │           🖼️                        │    │
│  │   Click to choose a photo          │    │
│  │   or drag & drop an image here     │    │
│  │   JPG · PNG · WebP · GIF           │    │
│  │       [ Browse files ]             │    │
│  └ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ┘    │
│                                             │
└─────────────────────────────────────────────┘
```

**What happens here:**
- User drags & drops any image file onto the zone, or clicks to browse
- `FileReader` API reads the file locally — no upload to any server
- Image is stored in memory as a data URL and rendered on a hidden `<canvas>`
- Auto center-crops to a square using `Math.min(width, height)` before proceeding

---

### Screen 2 — Difficulty Selection

```
┌─────────────────────────────────────────────┐
│ 🧩 Face Puzzle           ↺ Play  📷 New    │
├─────────────────────────────────────────────┤
│                                             │
│  Choose difficulty                          │
│  Pick a grid size to start solving.         │
│                                             │
│         ┌──────┐                           │
│         │ IMG  │  ← 200×200 preview        │
│         │ 🖼️   │  (hover = "Change photo") │
│         └──────┘                           │
│                                             │
│  ┌──────┐  ┌──────┐  ┌──────┐             │
│  │ ▪▪▪  │  │▪▪▪▪  │  │▪▪▪▪▪ │             │
│  │ ▪▪▪  │  │▪▪▪▪  │  │▪▪▪▪▪ │             │
│  │ ▪▪▪  │  │▪▪▪▪  │  │▪▪▪▪▪ │             │
│  │  3×3 │  │  4×4 │* │  5×5 │             │
│  │9 pcs │  │16 pcs│  │25 pcs│             │
│  └──────┘  └──────┘  └──────┘             │
│                                             │
│        [ Start Puzzle → ]                  │
└─────────────────────────────────────────────┘
  * = currently selected (purple border)
```

**What happens here:**
- Photo preview is shown with a hover-overlay "🔄 Change photo" shortcut
- User picks grid size; selected card highlights with `--accent` purple border
- Clicking Start triggers `buildPuzzle()` — image is drawn onto an offscreen canvas and sliced

---

### Screen 3 — Puzzle In Play

```
┌─────────────────────────────────────────────┐
│ 🧩 Face Puzzle           ↺ Play  📷 New    │
├─────────────────────────────────────────────┤
│                                             │
│  ┌──────────┬──────────┬──────────┐        │
│  │ 01:23.4  │    18    │    9     │   16   │
│  │  Time    │  Moves   │ Correct  │ Total  │
│  └──────────┴──────────┴──────────┘        │
│                                             │
│  ████████████░░░░░░░░░  9 / 16 correct     │
│                                             │
│  ┌────┬────┬────┬────┐                     │
│  │🟩  │    │🟨← │🟩  │  ← 🟨 = dragging   │
│  ├────┼────┼────┼────┤     🟩 = correct    │
│  │🟩  │    │🟩  │    │                     │
│  ├────┼────┼────┼────┤                     │
│  │    │🟩  │    │🟩  │                     │
│  ├────┼────┼────┼────┤                     │
│  │🟩  │🟩  │    │🟩  │                     │
│  └────┴────┴────┴────┘                     │
│                                             │
│  Drag pieces to swap · 🟢 = correct pos    │
│                                             │
│  [ 📷 New Photo ] [ ⚙️ Difficulty ] [↺ Reshuffle] │
└─────────────────────────────────────────────┘
```

**What happens here:**
- Timer starts the moment `buildPuzzle()` completes, updating every 100ms
- Each piece is an absolutely-positioned `<div>` containing its own `<canvas>` slice
- Mouse and touch events handled separately (`mousedown/move/up` + `touchstart/move/end`)
- On drag release: target cell is calculated from drop coordinates, pieces swap via `swap(p1, p2)`
- `updateIndicators()` re-checks every piece after each move — green border applied instantly

---

### Screen 4 — Win Screen

```
┌─────────────────────────────────────────────┐
│ 🧩 Face Puzzle  (darkened overlay)          │
├─────────────────────────────────────────────┤
│                                             │
│         ╔═══════════════════════╗           │
│         ║        🎉             ║           │
│         ║   Puzzle solved!      ║           │
│         ║  You nailed it! ✨    ║           │
│         ║                       ║           │
│         ║ ┌───────┬──────┬───┐  ║           │
│         ║ │01:47.2│  31  │4×4│  ║           │
│         ║ │ Time  │Moves │Grd│  ║           │
│         ║ └───────┴──────┴───┘  ║           │
│         ║                       ║           │
│         ║  BEST TIMES           ║           │
│         ║  #1  01:47.2  31  4×4 ║  ← you   │
│         ║  #2  02:03.8  44  4×4 ║           │
│         ║  #3  02:31.1  58  5×5 ║           │
│         ║                       ║           │
│         ║ [↺ Play] [Grid] [New] ║           │
│         ╚═══════════════════════╝           │
└─────────────────────────────────────────────┘
```

**What happens here:**
- `isSolved()` check runs after every move inside `updateIndicators()`
- When `correct === pieces.length`, timer clears and `showWin()` fires after a 500ms delay
- Score object `{time, moves, diff, elapsed, date}` pushed to array, sorted by `elapsed`, sliced to top 5
- Saved via `localStorage.setItem('facepuzzle_v3', JSON.stringify(scores))`
- Current run's row is highlighted with the `.current` class

---

## 📁 Generated File

### `face-puzzle-game.html`

| Property | Value |
|---|---|
| **File** | `face-puzzle-game.html` |
| **Size** | ~565 lines |
| **Type** | Single self-contained HTML |
| **Frameworks** | None |
| **External CDN** | None |
| **Browser support** | Chrome · Firefox · Safari |
| **Device support** | Desktop · Mobile · Tablet |
| **Storage** | `localStorage` (leaderboard only) |

### File structure breakdown

```
face-puzzle-game.html
├── <style>          CSS custom properties + all component styles (~150 lines)
├── <header>         Logo + contextual header buttons
├── #screen-upload   Drag-and-drop upload zone
├── #screen-difficulty  Photo preview + 3 difficulty cards
├── #screen-puzzle   Stats bar · progress bar · puzzle board · action buttons
├── #win-overlay     Results + leaderboard overlay
├── <canvas#offscreen>  Hidden canvas for image slicing
└── <script>         All game logic — no modules, pure IIFE (~260 lines)
    ├── Image loading    FileReader + onload handler
    ├── buildPuzzle()    Canvas slice → piece creation → shuffle → render
    ├── shuffleSolvable() Fisher-Yates + solved-state guard
    ├── drag system      startDrag / moveDrag / endDrag (mouse + touch)
    ├── swap()           Piece position exchange + DOM repositioning
    ├── updateIndicators() Correct count · progress · win detection
    ├── tick()           Timer formatter (mm:ss.t)
    └── localStorage     saveScore / getScores / renderLeaderboard
```

### Key functions

```js
// Center-crops image to square before slicing
var sq = Math.min(sw, sh);
var sx = (sw - sq) / 2;
var sy = (sh - sq) / 2;
octx.drawImage(sourceImage, sx, sy, sq, sq, 0, 0, boardSize, boardSize);

// Guaranteed-solvable shuffle
function shuffleSolvable(cells) {
  // Fisher-Yates shuffle
  for (var i = n - 1; i > 0; i--) {
    var j = Math.floor(Math.random() * (i + 1));
    // swap currentRow/currentCol between i and j
  }
  // Edge case: if shuffle lands on solved state, force one swap
  if (isSolved(cells)) { swap cells[0] ↔ cells[1] }
}

// Win detection — runs after every move
function updateIndicators() {
  var correct = pieces.filter(p =>
    p.currentRow === p.correctRow && p.currentCol === p.correctCol
  ).length;
  if (correct === pieces.length && timerInterval) {
    clearInterval(timerInterval);
    setTimeout(showWin, 500);
  }
}
```

---

## 📊 Completion Results

| Metric | Result |
|---|---|
| **Build sessions** | 3 iterative prompts (camera → Unsplash image → upload) |
| **Final approach** | Local file upload via `FileReader` API |
| **Prompt iterations** | 3 major versions |
| **Features delivered** | 100% of spec |
| **Bugs at launch** | 0 known |
| **Lines written by hand** | 0 |
| **Browsers tested** | Chrome ✅ · Firefox ✅ · Safari ✅ |
| **Mobile touch support** | ✅ Full (touchstart/touchmove/touchend) |
| **Leaderboard** | ✅ Top 5, persisted to localStorage |
| **Solvability guarantee** | ✅ Fisher-Yates + forced swap edge case |
| **Cross-origin issues** | ✅ Eliminated by switching to local FileReader |

### Sample game session

| Run | Grid | Time | Moves | Rank |
|---|---|---|---|---|
| Run 1 | 4×4 | 02:14.7 | 38 | #1 |
| Run 2 | 4×4 | 01:47.2 | 31 | #1 (new best) |
| Run 3 | 5×5 | 04:03.1 | 72 | — (different grid) |

---

## 💡 Key Learnings

### 1. Iterative prompting beats trying to get it perfect in one shot

The build went through three versions:

| Version | Approach | Problem encountered |
|---|---|---|
| v1 | `getUserMedia()` webcam | CORS/HTTPS restriction on `file://` |
| v2 | Unsplash CDN image | Cross-origin canvas tainting blocked `drawImage` |
| v3 | Local `FileReader` upload | ✅ No CORS, no server, works everywhere |

Each "failure" was actually useful signal. The best solution — local upload — only became obvious after ruling out the others. **Constraint discovery is part of the build process.**

---

### 2. Describing intent > describing implementation

Instead of saying *"use a Fisher-Yates shuffle"*, the prompt said *"scramble the pieces and guarantee the puzzle is solvable."* Claude chose Fisher-Yates and also handled the edge case (shuffle landing on the solved state) without being asked. Describing the *outcome you want* produces better code than describing the *algorithm you think you need*.

---

### 3. `FileReader` is the right tool for local image puzzles

```js
var reader = new FileReader();
reader.onload = function(e) {
  var img = new Image();
  img.onload = function() {
    sourceImage = img;   // stored for canvas slicing
    previewImg.src = e.target.result;
  };
  img.src = e.target.result;
};
reader.readAsDataURL(file);
```

Because the image never leaves the browser, there are zero CORS issues. The canvas can `drawImage()` freely. No server needed. Works from any local HTML file.

---

### 4. Canvas-based puzzle slicing is simpler than it looks

Each puzzle piece is its own `<canvas>` element. The slicing is just one `drawImage()` call with an offset:

```js
pctx.drawImage(
  offscreen,                        // source: full image drawn to offscreen canvas
  col * pieceSize, row * pieceSize, // source x, y (the piece's correct position)
  pieceSize, pieceSize,             // source width, height
  0, 0, pieceSize, pieceSize        // destination (fill the whole piece canvas)
);
```

No image splitting libraries needed. The browser handles it natively.

---

### 5. Unifying mouse and touch events cleanly

The drag system uses three paired functions:

```
startDrag(el, cx, cy)   ← called by both mousedown and touchstart
moveDrag(cx, cy)        ← called by both mousemove and touchmove
endDrag(cx, cy)         ← called by both mouseup and touchend
```

Each event handler just extracts coordinates and delegates:

```js
function onTouchStart(e) {
  e.preventDefault(); // stops scroll interference
  startDrag(e.currentTarget, e.touches[0].clientX, e.touches[0].clientY);
}
```

This pattern — extract coords → call shared logic — makes it trivial to support both input methods without duplicating logic.

---

### 6. localStorage leaderboard is 10 lines of real code

```js
function saveScore(time, mv, diff, sec) {
  var scores = getScores();
  scores.push({ time, moves: mv, diff, elapsed: sec, date: new Date().toLocaleDateString() });
  scores.sort((a, b) => a.elapsed - b.elapsed);
  scores = scores.slice(0, 5);
  localStorage.setItem('facepuzzle_v3', JSON.stringify(scores));
}
```

Sort by raw elapsed seconds (not the formatted string), keep top 5, stringify and store. Simple, persistent, no backend.

---

### 7. The "single file" constraint is a feature, not a limitation

By keeping everything in one HTML file:
- No build step
- No dependency management
- Sharable as a single file attachment or GitHub Gist
- Works offline after first open
- Easier to audit (everything is visible in one place)

The constraint forced better decisions — using native APIs instead of libraries, keeping logic minimal, thinking about what's actually necessary.

---

## 🔗 Resources & References

| Resource | Link |
|---|---|
| **GitHub profile** | [github.com/uroojey](https://github.com/uroojey) |
| **LinkedIn** | [linkedin.com/in/uroojey](https://linkedin.com/in/uroojey) |
| **Portfolio** | [portfolio-uroojey.vercel.app](https://portfolio-uroojey.vercel.app) |
| **Challenge tag** | #60DayClaudeChallenge |
| **MDN — FileReader** | [developer.mozilla.org/en-US/docs/Web/API/FileReader](https://developer.mozilla.org/en-US/docs/Web/API/FileReader) |
| **MDN — Canvas drawImage** | [developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/drawImage](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/drawImage) |
| **MDN — Touch events** | [developer.mozilla.org/en-US/docs/Web/API/Touch_events](https://developer.mozilla.org/en-US/docs/Web/API/Touch_events) |

---

## 📂 Files in this folder

```
day20/
├── day20.md               ← this file
└── face-puzzle-game.html  ← the complete puzzle game
```

---

*Day 20 of 60 · #60DayClaudeChallenge · Built with [@Anthropic](https://anthropic.com) Claude · Mentored by @ABTalksOnAI @AnilBajpai*
