# notebooks

Hands-on notebooks for six PyPI packages: [lintex](https://pypi.org/project/lintex/), [supersafe](https://pypi.org/project/supersafe/), [baby-llm](https://pypi.org/project/baby-llm/), [skill-scoreboard](https://pypi.org/project/skill-scoreboard/), [tunepac](https://pypi.org/project/tunepac/) and [bts-pivot](https://pypi.org/project/bts-pivot/). The focus is cyber and AI safety: catching what coding agents get wrong, red-teaming agents, tuning models to refuse, and triaging logs.

Every notebook is executed end to end, so the outputs you see on GitHub are real. Data is fake (seeded `faker`), nothing needs an API key, and anything that does is guarded and says so.

## How each notebook is laid out

| Section | Time | What you get |
|---|---|---|
| Part 1: Quick start | 5 min | The smallest useful thing, end to end |
| Part 2: Working session | 15 to 20 min | A realistic scenario, several features, reading the output critically |
| Part 3: Solving a critical problem | 10 min | One high-stakes failure worked from problem to evidence, with a before/after table |
| Part 4: Deep dive | 20+ min | How it works inside, with flow diagrams and the source |
| Benchmark | 5 min | Explicit criteria, real measurements, a scorecard with honest misses |

## Single-package notebooks

| # | Notebook | Package | The problem it solves |
|---|---|---|---|
| 01 | [lintex](01-lintex.ipynb) | `lintex` | An agent's 2 a.m. edit with SQL injection and silently elided code never reaches production |
| 02 | [supersafe](02-supersafe.ipynb) | `supersafe` | A model swap leaks customer records and a token through a poisoned ticket |
| 03 | [baby-llm](03-baby-llm.ipynb) | `baby-llm` | A model that refuses unsafe requests but also refuses benign security questions |
| 04 | [skill-scoreboard](04-skill-scoreboard.ipynb) | `skill-scoreboard` | A review skill that is bypassed 70% of the time and burns a month of token budget |
| 05 | [tunepac](05-tunepac.ipynb) | `tunepac` | A nine-hour fine-tune ruined by a typo'd key, a wrong chat template and eval leakage |
| 06 | [bts-pivot](06-bts-pivot.ipynb) | `bts-pivot` | A low-and-slow credential-stuffing run hidden inside a dashboard's totals |

## Combination notebooks

| # | Notebook | Packages | The question it answers |
|---|---|---|---|
| 07 | [secure-coding-skills](07-secure-coding-skills.ipynb) | lintex + skill-scoreboard | Does your secure-coding skill actually make the agent write safer code? |
| 08 | [red-team-analytics](08-red-team-analytics.ipynb) | supersafe + bts-pivot | How do you turn a red-team run into findings you can act on? |
| 09 | [safety-tuning-loop](09-safety-tuning-loop.ipynb) | baby-llm + tunepac + supersafe | How do you train a model to refuse, then prove it? |

## Running them

```bash
pip install jupyter faker matplotlib pandas
pip install lintex supersafe skill-scoreboard bts-pivot tunepac baby-llm   # whichever you need
jupyter lab
```

Notebooks 03, 05 and 09 train tiny models on CPU and pull in `torch`; 03 takes about three minutes. To re-execute one from scratch:

```bash
jupyter nbconvert --to notebook --execute --inplace 01-lintex.ipynb
```

The writing and diagram conventions are in [STYLE.md](STYLE.md).
