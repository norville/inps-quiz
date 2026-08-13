# Quiz 'Concorso INPS 499 — Assistenti informatici'

**→ [norville.github.io/inps-quiz](https://norville.github.io/inps-quiz/)**

A practice app for the written exam of the Italian INPS public competition for 499
Assistenti informatici. One HTML page, no dependencies, no installation and no server:
the questions live in a JSON file next to the page, and everything runs in the browser.

The app's interface and the question bank are in Italian.

## Using it

Just open <https://norville.github.io/inps-quiz/>. Nothing to install, no account, no
sign-up.

Your results are kept in your own browser and are never sent anywhere. That also means
they don't follow you to another browser or device, and clearing the site's data erases
them — see [Saving your results](#saving-your-results) for how to keep a copy.

## Running it locally

From the project folder:

```bash
python3 -m http.server
```

then open <http://localhost:8000/>.

The page also works if you just double-click it, but then the browser won't let it read
the question bank sitting next to it, and it will offer you a file picker instead.

## Modes

**Per argomento** (by topic) — every question from one of the manual's 13 parts, in
random order and untimed. This is the study mode: at the end, alongside your mistakes,
it shows each question's historical success rate, sorted from the one you get wrong
most often to the one you know best, so you can see what to go back to.

**Simulazione esame** (exam simulation) — 60 questions drawn from all parts, with at
least one question per part, and a 120-minute countdown that hands the test in by
itself when it runs out. It is scored out of 30 — half a point per correct answer — with
the 21/30 pass mark highlighted.

**Ripasso degli errori** (review mistakes) — replays only the questions you just got
wrong.

## How it works

**The options are always shuffled.** In the question bank, 54% of the correct answers
are the letter B (A 14%, C 27%, D 5%): keeping the original order would train you to
remember the position instead of the content, inflating scores for nothing.

**History accumulates.** Every test you hand in goes into an archive with its date,
score, time and per-question outcome. From that the page derives the per-question
statistics and your average across exam simulations. Archives can be exported and
imported again, and merging is idempotent: importing the same file twice creates no
duplicates.

**The data stays yours.** There is no server and no outbound connection: results live
in your browser and, if you want, in a file on your own disk.

### Saving your results

Results live in your browser by default, and that is the only copy. To keep them beyond
it, use the buttons under the archive: «Esporta archivio» downloads a dated JSON file,
and «Importa archivio» loads one back — on any browser and any machine. Importing merges
rather than replaces, and importing the same file twice adds nothing, so re-importing is
always safe.

If you want this to be automatic, export into a folder your Dropbox, Drive or Syncthing
client already syncs: because merging is idempotent, the same archive can be imported on
every device you study from.

## The question bank (for developers)

The questions live in `inps-quiz-questions.json`, kept separate from the page so they can
be fixed or extended without touching the code. They are edited in the repository and
published with the site — the app offers no way to change them from the browser. The
structure is:

```json
{
  "parti": [
    { "id": "Parte I", "titolo": "Informatica di base e gestione di sistemi di backup e recovery" }
  ],
  "domande": [
    {
      "parte": "Parte I",
      "sezione": "Capitolo 1 — Dai segnali elettrici agli algoritmi",
      "n": 1,
      "testo": "Un byte è composto da:",
      "options": [
        { "l": "A", "t": "2 bit" },
        { "l": "B", "t": "4 bit" },
        { "l": "C", "t": "8 bit" },
        { "l": "D", "t": "16 bit" }
      ],
      "risposta": "C"
    }
  ]
}
```

Every question needs `parte`, `sezione`, `n`, `testo`, between 2 and 6 `options`, and
`risposta` holding the correct letter. The `parti` list only supplies titles and
ordering: parts that aren't listed are appended at the end. The file is validated on
load and, if something doesn't add up, the page lists the problems instead of starting
with broken data.

The `id` field is optional but useful: it's the key that keeps the historical
statistics attached to a question. Without it the key is derived from `parte`,
`sezione` and `n`, so renumbering or moving a question makes it lose its history.

The included bank holds 602 questions across 13 parts.

## Appearance

Light, dark, or automatic following your system settings, via the button in the top
right corner. The choice is remembered.

## Deployment

The site is served by GitHub Pages from `main`, at
<https://norville.github.io/inps-quiz/>. There is no build step: pushing to `main`
publishes.

## Project files

| File | Contents |
| --- | --- |
| `index.html` | the whole application: markup, styles and logic |
| `inps-quiz-questions.json` | the question bank |

---
