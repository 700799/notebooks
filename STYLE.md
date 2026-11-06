# Notebook style guide

Every notebook in this repo follows the same shape and the same look, so a reader who has
finished one knows exactly how to read the next.

## Who is reading

A hands-on engineer with a security or safety job to do: a SOC analyst, an AppSec reviewer,
an ML engineer shipping an agent, someone running Claude Code at work. They want to get
something working in five minutes, then understand it well enough to trust it, then see how
it is built. Write for that person. Assume they are smart and busy.

## Shape of every notebook

```
Title cell                       # H1, one-sentence tagline, a 3-row "At a glance" table
What you will build              # a short table: section | what you do | time
Setup                            # one pip cell, one import cell, one "make fake data" cell (faker, seeded)

Part 1 - Quick start             # 5 minutes. The smallest useful thing, end to end.
Part 2 - Working session         # 15-20 minutes. A realistic scenario with fake data,
                                 #   several features, reading the output critically.
Part 3 - Solving a critical problem
                                 # One high-stakes, named problem this package solves,
                                 #   worked end to end: the problem statement (what goes
                                 #   wrong if you don't solve it, who gets paged), the
                                 #   approach, the run, the evidence it worked, and a
                                 #   "before / after" table. Not a feature tour: a story
                                 #   with a stake. Examples: an injection that reached
                                 #   production, a jailbreak that leaked a tool key, a
                                 #   brute-force hidden in 50k log lines, a skill that
                                 #   silently burned a month of tokens.
Part 4 - Deep dive               # How it works inside. Flow diagrams, source-level
                                 #   explanation, edge cases, tuning, what can go wrong.

Benchmark                        # The notebook ends by measuring the package against
                                 #   explicit criteria, for real, in code. A criteria
                                 #   table first: criterion | why it matters | how it is
                                 #   measured | target. Then the measurement cells (timing
                                 #   with time.perf_counter over several runs, accuracy /
                                 #   recall against the planted ground truth in the fake
                                 #   data, cost or token counts, coverage of the rule or
                                 #   suite, false positives). Then a scorecard table:
                                 #   criterion | measured | target | met (yes/no), with
                                 #   one honest sentence per miss. Where an alternative
                                 #   is reasonable (ruff/bandit for a linter, pandas
                                 #   groupby for a pivot, a plain training loop), run it
                                 #   on the same input and put it in the same table.
                                 #   Numbers come from the run, never typed in.

Cheat sheet                      # one table of the commands / calls used, in order
Where it fits                    # one table: this package next to the others in this repo
```

Part headers are `##`. Sub-sections inside a part are `###`. Nothing deeper than `###`.
Use `---` between parts and nowhere else.

## Voice

- Write the way a good README written by the maintainer reads: concrete, opinionated,
  specific. Name the scenario ("a SOC analyst triaging 40k auth events from a Friday night",
  "a Claude Code session that just rewrote `db.py`"). Give the fake company and people
  names from faker and keep using them.
- Lead every markdown cell with the point. No "In this section we will...", no "Let's dive
  in", no "Now that we have...", no "Conclusion" that restates the notebook.
- Explain *why* a feature exists before showing *how*. One or two sentences is enough.
- After a code cell that prints something non-obvious, the next markdown cell reads the
  output: what to look at, what is surprising, what you would do next.
- Tables for anything with three or more comparable items: options, rules, flags, columns,
  trade-offs, results. Prefer a table to a bullet list.
- Short paragraphs. Blank line between every element. One idea per cell.
- No emojis, no GIFs, no icon fonts, no badges, no "!!!" callouts. No HTML styling
  (GitHub strips it). Markdown only: headers, tables, code, blockquotes, bold for the one
  phrase that matters.

## Diagrams

Draw them with matplotlib so they render as outputs on GitHub and in Jupyter. Only these
shapes: rectangles, rounded rectangles, circles, straight arrows. Every notebook starts with
this helper cell, verbatim, so all diagrams share one palette and one hand:

```python
import matplotlib.pyplot as plt
from matplotlib.patches import FancyBboxPatch, Circle, FancyArrowPatch

INK, PAPER, LINE = "#1f2328", "#ffffff", "#d0d7de"
FILL = {"step": "#eaeef2", "accent": "#ddf4ff", "warn": "#fff1e5", "good": "#dafbe1"}

def canvas(w=11, h=3.2):
    fig, ax = plt.subplots(figsize=(w, h), dpi=120)
    ax.set_xlim(0, w); ax.set_ylim(0, h); ax.axis("off")
    fig.patch.set_facecolor(PAPER)
    return fig, ax

def box(ax, x, y, w, h, text, kind="step", fs=9.5):
    ax.add_patch(FancyBboxPatch((x, y), w, h, boxstyle="round,pad=0.02,rounding_size=0.12",
                                fc=FILL[kind], ec=LINE, lw=1.2))
    ax.text(x + w/2, y + h/2, text, ha="center", va="center", fontsize=fs, color=INK,
            family="DejaVu Sans", linespacing=1.4)

def dot(ax, x, y, r, text, kind="accent", fs=9.5):
    ax.add_patch(Circle((x, y), r, fc=FILL[kind], ec=LINE, lw=1.2))
    ax.text(x, y, text, ha="center", va="center", fontsize=fs, color=INK, family="DejaVu Sans")

def arrow(ax, x1, y1, x2, y2, label=None, fs=8.5):
    ax.add_patch(FancyArrowPatch((x1, y1), (x2, y2), arrowstyle="-|>", mutation_scale=12,
                                 color=INK, lw=1.1, shrinkA=2, shrinkB=2))
    if label:
        ax.text((x1+x2)/2, (y1+y2)/2 + 0.16, label, ha="center", va="bottom", fontsize=fs,
                color="#57606a", family="DejaVu Sans", zorder=5,
                bbox=dict(fc=PAPER, ec="none", pad=1.5))

def note(ax, x, y, text, fs=9, ha="left"):
    ax.text(x, y, text, ha=ha, va="center", fontsize=fs, color="#57606a", family="DejaVu Sans",
            zorder=5, bbox=dict(fc=PAPER, ec="none", pad=1.5))
```

Layout rules that keep diagrams clean: an arrow label must be shorter than the gap it sits
in (otherwise put it in a `note` above or below the row); never let a label touch a box edge;
every shape must be connected to something by an arrow or sit in a labelled group; leave at
least 0.5 units between boxes that have an arrow between them. Render the cell and look at the
PNG before moving on.

Keep diagrams to one idea each: a pipeline, a decision, a loop. Label every arrow that is
not obvious. `plt.show()` at the end so the output is embedded.

Charts of data use the same palette, `dpi=120`, no gridlines heavier than `LINE`, title as a
plain sentence, axes labelled with units.

## Code cells

- Every cell runs top to bottom in a fresh kernel with `jupyter nbconvert --execute`. No
  cell depends on a secret, a GPU or the network unless it is clearly marked and guarded
  (`if os.environ.get("ANTHROPIC_API_KEY"): ... else: print("Skipped: set ANTHROPIC_API_KEY to run this live")`).
  Never hand-write fake output into a markdown cell. If a thing cannot run here, show the
  exact command in a code block and say what it prints.
- Fake data comes from `faker` with `Faker.seed(7)` and `random.seed(7)` so reruns match.
  Make the data look like the real thing (real-looking hostnames, realistic timestamps,
  believable log lines), not `foo`/`bar`.
- Comments explain intent, not syntax. Prefer one short comment above a block to many
  inline ones.
- Shell commands via `!` or `subprocess.run([...], text=True)`; show the exit code when it
  carries meaning.
- Print outputs are kept short: `.head()`, `[:20]`, a summary line. Long outputs go in a
  pandas DataFrame so the renderer makes a table.

## File names

`NN-<package>.ipynb` for single packages, `NN-<theme>.ipynb` for combination notebooks,
with `NN` giving the reading order. The repo README lists them in a table.
