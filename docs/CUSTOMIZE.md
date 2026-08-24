# Making it yours

🇮🇹 [Questa guida in italiano](PERSONALIZZA.md)

Everything is in `index.html`. There are only two places you ever edit:

1. the **`:root` block** at the top of the `<style>` — colours, fonts, geometry;
2. the **HTML body** — the words on the sheet.

No build step. Save the file, reload the browser, print.

> **Working with an AI agent instead?** Jump to [§8](#8-handing-it-to-an-ai-agent)
> for ready-to-paste prompts. Point the agent at [`../AGENTS.md`](../AGENTS.md)
> first — it carries the constraints that keep the sheet printable.

---

## 1. Make it yours in ten minutes

The sheet ships with the author's own life areas. Replace them and it is your
planner. Four edits, in order of how much they matter.

### The four routes — ⚠️ each name appears twice

This is the one edit that goes wrong. Every route name is written in **two**
places and they must agree:

**A — the card**, in section 02:

```html
<div class="route-head">
  <span class="route-name display">Musica</span>          <!-- ← your area -->
  <span class="route-tag mono">Hobby</span>               <!-- ← a short gloss -->
</div>
```

**B — the log row**, in section 03, same order, rows 01 to 04:

```html
<div class="col-label">
  <span class="num mono">01</span>
  <span class="text">Musica</span>                        <!-- ← must match A -->
</div>
```

The shipped four are `Musica` (Hobby), `Progetti` (Extra, più di un hobby),
`Studio` (Università), `Proattività` (Sport, cultura, lettura). Pick four areas
that are genuinely distinct for you — four flavours of "work" will make the log
useless, because you will not know which column to mark.

**Rows 05 and 06 are not routes.** `Famiglia ✓` is the protected row and
`Not-to-do ✓` is the limit row. They have no card in section 02, they carry no
target, and they keep their `log-row-protected` / `log-row-limit` classes. You
can rename `Famiglia` to whatever your non-negotiable is, but keep it a
constraint — do not give it a target.

### The baseline boxes

Section 01, four `.tile` blocks. Change the descriptions to match how you
actually think:

```html
<div class="tile tile-protected">
  <div class="tile-kicker mono">non negoziabile</div>
  <div class="tile-title display">Protetto</div>
  <div class="tile-note">Famiglia &amp; impegni fissi</div>   <!-- ← yours -->
  <div class="tile-fill"></div>
</div>
```

Keep the first box green (`tile-protected`) and the third red (`tile-limit`).
Those two poles are what make the rest of the sheet realistic.

### The legend

Section 03, `.legend`. Six symbols and one blank box:

```html
<span class="legend-item"><span class="glyph">⚡</span>fatto con slancio</span>
```

Change the emoji, change the labels. **Six items is what fits** — a seventh
squeezes the trailing dotted line to nothing. Keep the final `legend-custom`
block: the empty box is where you invent a symbol by hand, on paper.

Resist turning these into a scoring scale. `🧱 blocked` and `✕ skipped` are
different *facts*, and that difference is the whole point of the log.

### The header

```html
<h1 class="display">La Settimana Possibile</h1>
<p class="masthead-sub">Si pianifica prima, si fa il consuntivo dopo. …</p>
```

The `/ 52` next to the week field assumes you number weeks across the year.
Change or delete it as you like.

---

## 2. Recolour it

Edit the `:root` block. The eleven palette tokens reach every rule in the file —
you never need to hunt for hex values further down.

```css
:root {
  --paper:      #EEEAE0;   /* sheet background */
  --paper-tile: #F5F2EA;   /* neutral boxes */
  --dot:        #DCD6C6;   /* dot grid */
  --ink:        #26344A;   /* text, borders, main rules */
  --ink-soft:   #C9C2B2;   /* secondary rules, inner separators */
  --accent:     #B5533C;   /* numbering, kickers, highlights */

  --protected:      #2F4A3C;   /* green: non-negotiable */
  --protected-bg:   #DCE8DE;
  --protected-soft: #B7CCBB;

  --limit:      #9C4A4A;       /* red: what you avoid */
  --limit-bg:   #F3E4E0;
  --limit-soft: #DFC4BD;
}
```

**Keep the semantics.** `--protected` has to read as *safe / non-negotiable* and
`--limit` as *stop / avoid*. Swap them for two arbitrary colours and nothing
errors, but the sheet stops meaning anything at a glance.

Two drop-in alternatives:

<details>
<summary><strong>Prussian</strong> — cooler, higher contrast</summary>

```css
--paper: #EDEAE3;  --paper-tile: #F6F4EE;  --dot: #D6D2C7;
--ink: #1F3A5F;    --ink-soft: #C2BFB4;    --accent: #C1663F;
--protected: #35594A;  --protected-bg: #D9E6DD;  --protected-soft: #B0C8B8;
--limit: #8F4545;      --limit-bg: #F2E1DD;      --limit-soft: #DCC0BA;
```
</details>

<details>
<summary><strong>Graphite</strong> — near-monochrome, cheapest to print</summary>

```css
--paper: #F2F1EE;  --paper-tile: #FAF9F7;  --dot: #DAD8D3;
--ink: #2B2B2B;    --ink-soft: #C6C4BF;    --accent: #6E6A63;
--protected: #3F4A3F;  --protected-bg: #E3E7E1;  --protected-soft: #C3CBC2;
--limit: #5A4444;      --limit-bg: #ECE4E2;      --limit-soft: #D2C3C0;
```
</details>

**Printing on an inkjet?** Already handled — by default the sheet prints on a
white background regardless of what you set `--paper` / `--paper-tile` to. The
tokens above only govern the on-screen preview (and a printout where you opted
back into the tint, see below); an `@media print` rule forces the neutral
backgrounds to white specifically at print time, since the tint covers 100 % of
an A3 sheet and would saturate a full page every week. The green (`--protected`)
and red (`--limit`) rows print as designed — they are the ~10 % of the sheet
worth the ink.

Want the tinted background on paper too? Add `class="print-tinted"` to `<body>`
before printing.

---

## 3. Change the fonts

Two coordinated edits: the `<link>` in `<head>` downloads the files, the `:root`
tokens declare the use. Change one without the other and you silently fall back
to system fonts.

```css
--font-display: 'Space Grotesk', system-ui, sans-serif;   /* headings */
--font-body:    Inter, system-ui, sans-serif;             /* running text */
--font-mono:    'IBM Plex Mono', ui-monospace, monospace; /* labels, numbers */
```

`--font-mono` does the heavy lifting: every uppercase micro-label on the sheet
uses it. Pick a mono with a real lowercase and comfortable letter-spacing — the
labels are set at 6 pt and a cramped face turns them to mud on paper.

**Want it to work offline?** Delete the `<link>`, download the three `.woff2`
files, and embed them as base64 `@font-face` rules. The file grows from ~30 KB
to ~350 KB and becomes genuinely self-contained.

---

## 4. Turn off the crop marks

The four corner marks are for trimming the sheet to true size. To hide them:

```html
<body class="no-fiducials">
```

Same for the `· strappa qui ·` tear line — delete the `.tearline` block if you
are not tearing anything.

---

## 5. Translate it

The sheet is Italian. If you translate, **translate all of it** — a half-Italian
sheet reads like a bug. The strings, in document order: the tear line, the
masthead kicker / title / subtitle, `Nome` and `Settimana`, the four section
labels `01`–`04`, the baseline tiles, the route fields (`Traguardo della
settimana`, `Quando`, `Rituale d'inizio, 60 sec`), the log heading and hint, the
day abbreviations `Lun…Dom` and `Tot`, the legend, the two review titles, and
the score row (`Punteggio settimana`, `cerchia un numero`, `umore della
settimana`, `una cosa che cambio`).

Also update `<html lang="it">` and the `<title>`.

Watch the widths: the mono labels are set in a fixed 34 mm column and in
fixed-width fields. German and Finnish will overrun where Italian fits — check
with [§7](#7-check-your-work) rather than assuming.

---

## 6. Change the paper size

⚠️ **Two places, and they cannot be linked.** The `@page` rule cannot read CSS
custom properties, so both must be edited together:

```css
:root { --sheet-w: 420mm; --sheet-h: 297mm; }
@page { size: 420mm 297mm; margin: 0; }
```

**This alone does not give you an A4 version.** The layout is hand-tuned in
absolute units: 6 pt labels stay 6 pt on a sheet 30 % smaller, fixed `mm`
heights stay put, and the content overflows. A genuine A4 variant means
re-tuning the type scale and every fixed height — a real piece of work, not a
one-line change.

If you just want it on A4 today, leave it at A3 and choose **"fit to page"** in
the print dialog. Everything shrinks by ~29 %, the 6 pt labels land near 4.2 pt,
and it is legible but tight. Print one and decide with it in your hand.

---

## 7. Check your work

The sheet is specified in millimetres, so "looks fine" is not a check. Open
`index.html` in Chrome, open DevTools, and run the verification snippet from
[`../AGENTS.md` §5](../AGENTS.md#5-verification--mandatory).

The two failures that do not show on screen:

- **content overflow** — text longer than the original pushes past the sheet and
  gets silently clipped by `overflow: hidden`;
- **a second blank page** at print time, from sub-millimetre rounding.

So always finish with `Ctrl+P` and confirm the dialog says **1 page**.

---

## 8. Handing it to an AI agent

Start the agent on [`../AGENTS.md`](../AGENTS.md) — it carries the hard
constraints (single file, no JS, no build, exactly 420 × 297 mm) and the
verification procedure. Without it, agents reliably try to "modernise" this into
a React app with a build pipeline.

**Retheme:**

```
Read AGENTS.md first. In index.html, recolour the sheet to <describe the mood>
by editing only the :root palette tokens — do not hard-code hex values in rules.
Keep --protected reading as "safe/non-negotiable" and --limit as "stop/avoid".
Then run the AGENTS.md §5 verification and report the numbers.
```

**Re-theme the content:**

```
Read AGENTS.md first. In index.html, replace the four routes with:
  01 <name> (<short gloss>)
  02 <name> (<short gloss>)
  03 <name> (<short gloss>)
  04 <name> (<short gloss>)
Each name appears TWICE — the card in section 02 and the matching log row in
section 03. Update both and keep the order aligned. Do not touch rows 05
(protected) or 06 (limit). Then run the AGENTS.md §5 verification.
```

**Translate:**

```
Read AGENTS.md first. Translate every user-visible string in index.html to
<language>, including the tear line, day abbreviations and legend — leave
nothing in Italian. Update <html lang> and <title>. Then run the AGENTS.md §5
verification and confirm no label overflows its column.
```

Whatever the task, require the agent to **report the §5 numbers back**. An agent
that says "done, looks good" has not measured anything, and this is a document
where a 0.5 mm error costs you a second sheet of A3.
