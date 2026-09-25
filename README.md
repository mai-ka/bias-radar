# Bias Radar

*(working title, also "Fair Source")*

**An open-source tool that scans training corpora for representation bias
*before* model training — proposed for [Prototype Fund](https://www.prototypefund.de/en)
2026 (Software Infrastructure track).**

> **Status: proposal / concept stage.** This repository documents the idea for
> the funding application. Development has not started yet — there is no
> working code here.

## The problem

AI systems are trained mostly on web crawls, Wikipedia, books, and code
repositories. A 2024 UNESCO study found that these sources associate female
names with family and home, male names with career and leadership. A UC
Berkeley Haas study found gender bias in 44% of 133 AI systems studied. The
issue isn't limited to gender — and it doesn't start with the finished model,
it's already in the training data. Anyone training a model today has almost no
accessible tool to check for this beforehand.

## The approach

An open-source tool (a library plus a simple interface) that scans text
corpora or data sources for representation bias — e.g. gender, role
assignment, origin — *before* training. Output: a readable bias report that
shows where a data source is skewed, referencing the documentation
obligations for training-data governance under the EU AI Act.

**Target audience**: small and mid-sized teams that fine-tune models or run
their own language models but have no dedicated fairness research team, plus
NGOs and institutions (e.g. UN Women) that need a low-barrier review tool for
partner projects.

## How this differs from existing tools

IBM's [AI Fairness 360](https://ai-fairness-360.org/) and the
[Data Nutrition Project](https://datanutrition.org/) mainly target finished
models or structured/tabular datasets — not raw text/web corpora *before*
training — and neither connects to EU AI Act obligations.

## Team

- **Maika Paetzold** ([mettage.com](https://mettage.com)) — concept,
  societal framing, communication
- **Fabian Franz** ([github.com]https://github.com/Franzelfx)** — development (ML/AI,
  full-stack, DevOps), already focused on GDPR and EU AI Act compliance

## Proposed milestones (6 months, to be refined with NexPatch)

1. **Months 1–2**: define bias metrics, select test corpora
2. **Months 2–4**: build the analysis engine and the report
3. **Months 4–5**: test phase with real datasets and first external users
4. **Months 5–6**: documentation, public repository, launch
