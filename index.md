# Benchmarking LLMs on Local Search & Discovery

⚠️ **Work in progress**

This page is a **placeholder summary report** for the upcoming **LLM Geospatial / Local Search Benchmark**.

It currently contains a **draft overview and preliminary results**.  
The full dataset, prompt set, and evaluation pipeline are being prepared and will be released later.

---

# Summary

Local search is one of the most common things people ask AI assistants to do:

- find restaurants
- explore neighborhoods
- check business hours
- book tables

Yet almost no benchmarks evaluate how well LLMs perform on **real-world place discovery tasks**.

Most existing LLM benchmarks focus on math, coding, or reasoning — even though **~46% of Google searches have local intent**.

This project benchmarks how well modern LLMs handle **location-based queries and merchant interaction tasks.**

---

# TL;DR

*(Preliminary results — subject to revision)*

- We evaluated **4 major LLM providers** on **345 prompts across 50+ cities**.
- The benchmark includes **2,415 model runs** across different search configurations.
- The best model scored **~90.7 / 100**, but still recommended incorrect venues roughly **8% of the time**.
- Without search grounding, failure rates increased significantly.
- The hardest problem wasn't *finding places* — it was **following user constraints**.

---

# Benchmark Overview

### Dataset

| Metric | Value |
|------|------|
| Prompts | 345 |
| Cities | 50+ |
| Model runs | 2,415 |
| LLM providers | 4 |

Prompts were designed to simulate realistic queries users ask AI assistants.

Example prompts:

- "Affordable rooftop bars in Gangnam"
- "Is the National Museum of Anthropology in Mexico City open on Monday?"
- "Best way to get from the Colosseum to Trastevere?"

---

# Task Categories

The benchmark evaluates three types of user interactions.

| Category | Description |
|------|------|
| Explore | Discover places matching criteria |
| Obtain | Retrieve factual information about a venue |
| Engage | Interact with a merchant (book, contact, etc.) |

---

# Headline Results (Draft)

| Model / Configuration | Score |
|---|---|
| OpenAI (Search ON) | ~90.7 |
| Gemini Flash (Search ON) | ~86 |
| OpenAI (Search OFF) | ~86 |
| Claude (Search ON) | ~85 |
| Claude (Search OFF) | ~80 |

Scores combine multiple evaluation dimensions including factual accuracy and constraint adherence.

---

# Key Finding 1: Closed or Incorrect Venues Still Appear

Across the benchmark, models occasionally recommended:

- permanently closed businesses
- venues in the wrong location
- nonexistent establishments

Even with search enabled, some errors remained.

---

# Key Finding 2: Search Grounding Helps — But Not Everywhere

Search grounding improved **discovery-style queries**.

However, it sometimes degraded performance in **merchant interaction tasks**, where models switched from providing guidance to summarizing web snippets.

---

# Key Finding 3: Constraint Matching Is the Hardest Problem

Models often retrieved real venues but failed to respect the user’s constraints.

Example prompt:

> "Affordable rooftop bars in Gangnam"

Typical failure modes:

- expensive cocktail bars
- venues without rooftop seating
- places outside the requested neighborhood

---

# Methodology (Brief)

Evaluation pipeline:

1. Generate LLM response
2. Extract venue entities from response
3. Verify entities using external sources
4. Score response across multiple rubric dimensions

Evaluation dimensions include:

| Dimension | Weight |
|---|---|
| Completeness | 20% |
| Factual accuracy | 40% |
| Constraint adherence | 30% |
| Actionability | 10% |

---

# What Is Included In This Repo

This repository currently contains:

- placeholder summary report
- draft benchmark tables
- selected examples
- brief methodology description

---

# Planned Release

The following components are being prepared for open release:

- full prompt dataset
- evaluation pipeline
- benchmarking harness
- extended benchmark results
- additional analysis

---

# Repository Status

This repository currently hosts a **preliminary public summary** of the benchmark.

Additional materials and updates will be added as the project progresses.

---

# Contact

If you are interested in collaboration or early access to the dataset, feel free to reach out.
