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
Replay: the same corpus re-run offline (2026-08-22). The live run predates the
Sonnet 5 price cut of 2026-09-15 ($3/$15 → $2/$10 per Mtok) and overstates
Sonnet-priced dollars by 1.5×; the replay column is OpenAI-priced and unaffected.

Across the wider 10-workload / 100-request benchmark, per-workload cuts land
between 62% and 81% with zero answers harmed. Two workloads — clinical triage and
code review — returned no safe saving, and are reported as 0% rather than cut into.

## Where the product is (2026-09-15)

The engine lives in the private `safi-core` repository. What exists today:

- **Savings Audit** — a customer's real LLM logs in, an audit report out. It runs
  on the customer's machine under their own API keys; nothing is uploaded. The
  baseline is their log (the cost each call carried, or its tokens at their rates),
  never a projection. Only the calls the plan would change are replayed, at real
  prices; the headline counts realized savings only, with modeled savings in their
  own row. Quality is agreement with the customer's own production output, judged
  on the core answer and contradictions only, with production's self-agreement on
  identical questions printed as the floor. Deliverables: the full report
  (`audit.html`), a one-page note, a per-request file (labels, cost before/after,
  new answer next to the logged one), and the audit's own spend receipt.
- **Log formats** — LiteLLM, Helicone and Portkey exports, raw request/response
  pairs, or Safi's native trace format. The format is detected per line.
- **Recorder** — a pass-through proxy the customer hosts: one container, one
  `base_url` change. Every call is forwarded to the provider byte-for-byte and
  appended to a daily trace file the audit reads. It optimizes nothing; the one
  opt-in body edit (asking OpenAI to include usage on streamed responses) is
  logged at startup and visible on the health endpoint.
- **Hand-labeled ground truth** — the audit samples ~150 requests for the customer
  to label as cache-safe or not; the labeler is scored against that sheet and the
  measured accuracy is stamped on the report.

**Latest result (2026-09-15):** the audit re-ran 48 real Claude Sonnet 5 calls
through a bilingual e-commerce support prompt: **61.7% realized savings** off the
logged bill, **100% agreement** with production output (n=48, floor 100%). The
traffic is self-generated — real model output, synthetic prompts — until the first
customer audit lands. The audit itself costs roughly 1–3% of the window's spend in
provider calls, plus about two hours of one engineer's attention over a week.

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
