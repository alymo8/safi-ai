# Safi — صافي

**The AI cost layer.** Marketing site for Safi, a service that cuts a company's
LLM bill without changing the models, the prompts, or the answers.

**→ Live site: https://alymo8.github.io/safi-ai/**

---

## What Safi does

Most AI traffic is routine, but all of it gets billed at the price of the
strongest model in the stack. Safi profiles a workload, routes and reshapes each
request to the cheapest path that still answers it correctly — and refuses to cut
where cutting isn't safe. Customers change one line of config; models, prompts and
provider contracts stay exactly as they are.

Pricing is a share of proven savings. A month with no savings gets no invoice.

## The benchmark behind the numbers

Identical traffic run three ways — untouched, through a blunt one-size-fits-all
optimiser, and through Safi:

| Path             | Live run | Replay | Broken answers |
| ---------------- | -------: | -----: | -------------: |
| Do nothing       | baseline | baseline |            0 |
| Blunt optimiser  |    −26%  |  −52%  |         **10** |
| Safi             |    −44%  |  −79%  |          **0** |

Live run: 40 requests × 3 paths on Claude Sonnet 5 / Haiku 4.5 (2026-07-31).
Replay: the same corpus re-run offline (2026-08-22).

Across the wider 10-workload / 100-request benchmark, per-workload cuts land
between 62% and 81% with zero answers harmed. Two workloads — clinical triage and
code review — returned no safe saving, and are reported as 0% rather than cut into.

## The Arabic edge

Everything above is language-agnostic. Arabic is where Safi goes further, because
Arabic is priced worse to begin with: the same word can cost 3.59 tokens on one
path and 1.51 on another — a 2.4× gap for an identical sentence and an identical
answer. Safi knows where Gulf, Egyptian, Levantine and Modern Standard Arabic land
cheapest.

## This repository

A single static page, no build step, no dependencies.

| Path          | What it is                                                     |
| ------------- | -------------------------------------------------------------- |
| `index.html`  | The entire site — inline CSS, one inline script for the form.   |
| `favicon.svg` | Site icon.                                                      |
| `.nojekyll`   | Tells GitHub Pages to serve the files as-is.                    |

Fonts load from Google Fonts; the audit form posts to Formspree. Everything else
is self-contained.

**Run it locally:**

```bash
python -m http.server 8000
# then open http://localhost:8000
```

Opening `index.html` directly in a browser works too.

**Deploying:** GitHub Pages serves `main` at the repository root. Pushing to `main`
publishes; there is no build to run.

---

© 2026 Safi. Built in MENA.
