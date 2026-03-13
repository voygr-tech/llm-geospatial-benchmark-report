# LLM Local Search Benchmark: How Claude, GPT, Gemini, and Perplexity Handle Real-World Place Queries

> LLMs can write poetry and pass bar exams, but ask them to find an open restaurant nearby and they'll confidently send you to a place that closed two years ago.

We tested **4 leading LLMs** on **345 real-world local search prompts** (finding restaurants, checking hours, planning routes, booking tables) across **50+ cities on 6 continents**. Each provider was tested with and without web search (**2,415 total evaluations**). Every recommended place was verified against Google Search and Google Maps.

This page presents the key findings. For the complete analysis (per-category breakdowns, full failure taxonomy, geographic gap deep dive, methodology details, and all 345 benchmark prompts), **[get the full report](#get-the-full-report)**.

Disclosure: This benchmark came out of our work at [VOYGR](https://voygr.tech), where we're building an API to help address exactly these kinds of failures for AI agents and developers - verifying whether businesses LLMs recommend are actually open, at the right address, etc.

---

## Overall Performance

<p align="center">
  <img src="data/Overall_Performance.png" alt="Overall Performance by Provider and Search Config" width="700">
</p>

<table>
<tr>
<th>Provider</th><th>Model</th><th>Score (/100)</th><th>Completeness</th><th>Accuracy</th><th>Constraint Match</th><th>Plausibility</th>
</tr>
<tr>
<td><strong>OpenAI ON</strong></td><td>gpt-5.2</td>
<td><span style="display:inline-block;width:12px;height:12px;border-radius:50%;background:#00897B;vertical-align:middle;margin-right:4px;"></span><strong>90.7</strong></td>
<td><span style="display:inline-block;width:12px;height:12px;border-radius:50%;background:#00897B;vertical-align:middle;margin-right:4px;"></span>100%</td>
<td><span style="display:inline-block;width:12px;height:12px;border-radius:50%;background:#00897B;vertical-align:middle;margin-right:4px;"></span>92%</td>
<td><span style="display:inline-block;width:12px;height:12px;border-radius:50%;background:#26A69A;vertical-align:middle;margin-right:4px;"></span>85%</td>
<td><span style="display:inline-block;width:12px;height:12px;border-radius:50%;background:#00897B;vertical-align:middle;margin-right:4px;"></span>91%</td>
</tr>
<tr>
<td><strong>Gemini ON</strong></td><td>gemini-2.5-flash</td>
<td><span style="display:inline-block;width:12px;height:12px;border-radius:50%;background:#26A69A;vertical-align:middle;margin-right:4px;"></span>86.4</td>
<td><span style="display:inline-block;width:12px;height:12px;border-radius:50%;background:#00897B;vertical-align:middle;margin-right:4px;"></span>91%</td>
<td><span style="display:inline-block;width:12px;height:12px;border-radius:50%;background:#00897B;vertical-align:middle;margin-right:4px;"></span>92%</td>
<td><span style="display:inline-block;width:12px;height:12px;border-radius:50%;background:#4DD0E1;vertical-align:middle;margin-right:4px;"></span>77%</td>
<td><span style="display:inline-block;width:12px;height:12px;border-radius:50%;background:#4DD0E1;vertical-align:middle;margin-right:4px;"></span>76%</td>
</tr>
<tr>
<td><strong>Claude ON</strong></td><td>claude-sonnet-4-5</td>
<td><span style="display:inline-block;width:12px;height:12px;border-radius:50%;background:#26A69A;vertical-align:middle;margin-right:4px;"></span>85.9</td>
<td><span style="display:inline-block;width:12px;height:12px;border-radius:50%;background:#00897B;vertical-align:middle;margin-right:4px;"></span>99%</td>
<td><span style="display:inline-block;width:12px;height:12px;border-radius:50%;background:#26A69A;vertical-align:middle;margin-right:4px;"></span>89%</td>
<td><span style="display:inline-block;width:12px;height:12px;border-radius:50%;background:#4DD0E1;vertical-align:middle;margin-right:4px;"></span>76%</td>
<td><span style="display:inline-block;width:12px;height:12px;border-radius:50%;background:#4DD0E1;vertical-align:middle;margin-right:4px;"></span>79%</td>
</tr>
<tr>
<td><strong>Perplexity ON</strong></td><td>sonar-pro</td>
<td><span style="display:inline-block;width:12px;height:12px;border-radius:50%;background:#26A69A;vertical-align:middle;margin-right:4px;"></span>80.4</td>
<td><span style="display:inline-block;width:12px;height:12px;border-radius:50%;background:#00897B;vertical-align:middle;margin-right:4px;"></span>97%</td>
<td><span style="display:inline-block;width:12px;height:12px;border-radius:50%;background:#26A69A;vertical-align:middle;margin-right:4px;"></span>86%</td>
<td><span style="display:inline-block;width:12px;height:12px;border-radius:50%;background:#B2EBF2;vertical-align:middle;margin-right:4px;"></span>69%</td>
<td><span style="display:inline-block;width:12px;height:12px;border-radius:50%;background:#4DD0E1;vertical-align:middle;margin-right:4px;"></span>71%</td>
</tr>
</table>

> <span style="display:inline-block;width:10px;height:10px;border-radius:50%;background:#00897B;vertical-align:middle;margin-right:2px;"></span> 90%+ · <span style="display:inline-block;width:10px;height:10px;border-radius:50%;background:#26A69A;vertical-align:middle;margin-right:2px;"></span> 80-89% · <span style="display:inline-block;width:10px;height:10px;border-radius:50%;background:#4DD0E1;vertical-align:middle;margin-right:2px;"></span> 70-79% · <span style="display:inline-block;width:10px;height:10px;border-radius:50%;background:#B2EBF2;vertical-align:middle;margin-right:2px;"></span> <70%

OpenAI leads by 4.3 points. No single provider wins everything, though. Rankings shift by task type. And even the best performer recommends a place that doesn't exist, is permanently closed, or is in the wrong neighborhood **8% of the time**.

### The Consistency Gap

Averages hide the real story. The differentiator isn't the mean; it's how often things go *badly* wrong:

| Provider | Scored 90+ | Scored <70 | Std Dev |
|---|---|---|---|
| **OpenAI ON** | 69% | 8% | 13.2 |
| **Gemini ON** | 59% | 16% | 17.5 |
| **Claude ON** | 52% | 15% | 16.4 |
| **Perplexity ON** | 37% | 24% | 19.2 |

OpenAI scores 90+ on 7 out of 10 prompts. Perplexity scores below 70 on nearly 1 in 4. A product built on these APIs is only as good as its worst response.

---

## Finding 1: Are These Places Even Real?

We asked Claude (without search) to rank the top 5 yoga studios in Hawthorne District, Portland. It returned five studios with class prices, instructor specialties, and detailed descriptions. **Four of five are completely fabricated.** The model never hedged.

Without search, **1 in 5 places Claude recommends** has a fatal flaw: fabricated, permanently closed, or wrong location. Even Perplexity, a search-native product, has a 12% failure rate with search enabled.

<p align="center">
  <img src="data/Share_of_POIs_flawed.png" alt="Share of POIs with Fatal Flaws by Provider and Search Config" width="700" style="border: 1px solid #e0e0e0; border-radius: 4px; padding: 8px;">
</p>

| Failure Type | What Happens | Example |
|---|---|---|
| **Fabrication** | LLM invents a place with confident false details | 4/5 yoga studios in Portland don't exist, complete with prices and instructor bios |
| **Permanently closed** | LLM recommends a shuttered venue | All 7 configs gave booking advice for Proper, Buenos Aires (closed) |
| **Wrong location** | Place is real but in the wrong neighborhood | Asked for Condesa cantinas, got places from Centro and Coyoacan |
| **Wrong category** | Place exists but is a different type of business | Asked for tobacco shops, got shopping malls and a supermarket |

**The permanently closed blind spot** is arguably scarier than hallucination. We asked all 7 configs to help book a table at Proper in Buenos Aires, a permanently closed restaurant. Every single one cheerfully provided booking guidance to a shuttered building. Even search-equipped models didn't catch it. No provider reliably detects when a place has closed.

> The full report includes fatal flaw rates by provider and search config, the complete failure taxonomy with real examples from every provider, and the "confidently wrong" pattern analysis.

---

## Finding 2: Web Search Sometimes Makes LLMs Worse

Enabling search helps when you're looking for places (+10 to +21 points on discovery). On transactional tasks ("help me book a table"), it actively hurts:

<table>
<tr>
<th>Provider</th><th>Search OFF</th><th>Search ON</th><th>Delta</th>
</tr>
<tr>
<td>OpenAI</td>
<td><span style="display:inline-block;width:12px;height:12px;border-radius:50%;background:#26A69A;vertical-align:middle;margin-right:4px;"></span>87.8</td>
<td><span style="display:inline-block;width:12px;height:12px;border-radius:50%;background:#26A69A;vertical-align:middle;margin-right:4px;"></span>88.9</td>
<td>+1.1</td>
</tr>
<tr>
<td>Gemini</td>
<td><span style="display:inline-block;width:12px;height:12px;border-radius:50%;background:#26A69A;vertical-align:middle;margin-right:4px;"></span>84.0</td>
<td><span style="display:inline-block;width:12px;height:12px;border-radius:50%;background:#4DD0E1;vertical-align:middle;margin-right:4px;"></span>78.7</td>
<td><strong>-5.3</strong></td>
</tr>
<tr>
<td>Claude</td>
<td><span style="display:inline-block;width:12px;height:12px;border-radius:50%;background:#26A69A;vertical-align:middle;margin-right:4px;"></span>89.6</td>
<td><span style="display:inline-block;width:12px;height:12px;border-radius:50%;background:#26A69A;vertical-align:middle;margin-right:4px;"></span>84.1</td>
<td><strong>-5.5</strong></td>
</tr>
</table>

*Merchant/booking task scores. Claude and Gemini lose 5+ points with search enabled.*

**Why?** Search returns facts *about* a place instead of guidance on *how to act*. Without search, models give step-by-step booking advice ("call this number, ask for a window seat"). With search, they get distracted by retrieved content and return factual snippets instead of actionable instructions. OpenAI is the exception: it integrates search results *into* guidance rather than replacing guidance with search results.

The pattern extends to navigation: Gemini's own Maps grounding makes it *worse* at giving directions (-2.0). The provider with the world's best navigation tools scores lower when those tools are enabled.

> The full report includes the complete search paradox analysis across all 5 task categories, the Gemini token budget bug deep dive, and detailed before/after examples.

---

## Finding 3: Each Provider Has a Distinct Personality

Rankings shift dramatically by task type:

<table>
<tr>
<th>Use Case</th><th>Prompts</th><th>OpenAI ON</th><th>Gemini ON</th><th>Claude ON</th><th>Perplexity ON</th>
</tr>
<tr>
<td><strong>Obtain Place Details</strong></td><td>100</td>
<td><span style="display:inline-block;width:12px;height:12px;border-radius:50%;background:#00897B;vertical-align:middle;margin-right:4px;"></span><strong>94.7</strong></td>
<td><span style="display:inline-block;width:12px;height:12px;border-radius:50%;background:#00897B;vertical-align:middle;margin-right:4px;"></span>93.2</td>
<td><span style="display:inline-block;width:12px;height:12px;border-radius:50%;background:#00897B;vertical-align:middle;margin-right:4px;"></span>91.4</td>
<td><span style="display:inline-block;width:12px;height:12px;border-radius:50%;background:#26A69A;vertical-align:middle;margin-right:4px;"></span>88.1</td>
</tr>
<tr>
<td><strong>Share with Others</strong></td><td>30</td>
<td><span style="display:inline-block;width:12px;height:12px;border-radius:50%;background:#00897B;vertical-align:middle;margin-right:4px;"></span>94.5</td>
<td><span style="display:inline-block;width:12px;height:12px;border-radius:50%;background:#00897B;vertical-align:middle;margin-right:4px;"></span><strong>96.1</strong></td>
<td><span style="display:inline-block;width:12px;height:12px;border-radius:50%;background:#00897B;vertical-align:middle;margin-right:4px;"></span>92.6</td>
<td><span style="display:inline-block;width:12px;height:12px;border-radius:50%;background:#26A69A;vertical-align:middle;margin-right:4px;"></span>83.2</td>
</tr>
<tr>
<td><strong>Plan / Navigate</strong></td><td>80</td>
<td><span style="display:inline-block;width:12px;height:12px;border-radius:50%;background:#00897B;vertical-align:middle;margin-right:4px;"></span><strong>91.0</strong></td>
<td><span style="display:inline-block;width:12px;height:12px;border-radius:50%;background:#26A69A;vertical-align:middle;margin-right:4px;"></span>81.0</td>
<td><span style="display:inline-block;width:12px;height:12px;border-radius:50%;background:#26A69A;vertical-align:middle;margin-right:4px;"></span>89.2</td>
<td><span style="display:inline-block;width:12px;height:12px;border-radius:50%;background:#26A69A;vertical-align:middle;margin-right:4px;"></span>82.5</td>
</tr>
<tr>
<td><strong>Engage Merchant</strong></td><td>30</td>
<td><span style="display:inline-block;width:12px;height:12px;border-radius:50%;background:#26A69A;vertical-align:middle;margin-right:4px;"></span><strong>88.9</strong></td>
<td><span style="display:inline-block;width:12px;height:12px;border-radius:50%;background:#4DD0E1;vertical-align:middle;margin-right:4px;"></span>78.7</td>
<td><span style="display:inline-block;width:12px;height:12px;border-radius:50%;background:#26A69A;vertical-align:middle;margin-right:4px;"></span>84.1</td>
<td><span style="display:inline-block;width:12px;height:12px;border-radius:50%;background:#26A69A;vertical-align:middle;margin-right:4px;"></span>87.1</td>
</tr>
<tr>
<td><strong>Explore / Discover</strong></td><td>105</td>
<td><span style="display:inline-block;width:12px;height:12px;border-radius:50%;background:#26A69A;vertical-align:middle;margin-right:4px;"></span><strong>86.9</strong></td>
<td><span style="display:inline-block;width:12px;height:12px;border-radius:50%;background:#26A69A;vertical-align:middle;margin-right:4px;"></span>84.0</td>
<td><span style="display:inline-block;width:12px;height:12px;border-radius:50%;background:#4DD0E1;vertical-align:middle;margin-right:4px;"></span>77.8</td>
<td><span style="display:inline-block;width:12px;height:12px;border-radius:50%;background:#4DD0E1;vertical-align:middle;margin-right:4px;"></span>70.2</td>
</tr>
</table>

> <span style="display:inline-block;width:10px;height:10px;border-radius:50%;background:#00897B;vertical-align:middle;margin-right:2px;"></span> 90+ · <span style="display:inline-block;width:10px;height:10px;border-radius:50%;background:#26A69A;vertical-align:middle;margin-right:2px;"></span> 80-89 · <span style="display:inline-block;width:10px;height:10px;border-radius:50%;background:#4DD0E1;vertical-align:middle;margin-right:2px;"></span> 70-79 · <span style="display:inline-block;width:10px;height:10px;border-radius:50%;background:#B2EBF2;vertical-align:middle;margin-right:2px;"></span> <70

- **OpenAI**: The reliable all-rounder. Leads or ties in 4 of 5 categories. Best consistency (std dev 13.0). The provider you pick when you can't afford failures.
- **Gemini**: The specialist with extremes. Best foundational accuracy (1.6% fatal rate) thanks to Maps grounding, but last on booking and navigation, where that same grounding *hurts*.
- **Claude**: The solid middle. Never the worst, rarely the best. Quietly strong on trip planning: give it an arrival time and it excels at working backwards.
- **Perplexity**: The search-native underdog. Competitive on booking (beats Claude and Gemini), but can't generate a working Maps link and has the highest discovery failure rate.

### The Geographic Gap

We split prompts into USA/Western Europe vs Rest of World. The gap is modest on averages (+1 to +3 points) but explodes on specific tasks: reservation prompts for ROW restaurants are dramatically harder. Well-known ROW landmarks (Grand Bazaar Istanbul: 99.2, Ghibli Museum: 98.5) perform at Western levels. The gap isn't geography; it's data coverage.

> The full report includes search ON vs OFF scores for all 7 configs across all 5 categories, the geographic gap breakdown by task type, and the complete constraint fidelity analysis.

---

## Finding 4: The Places Are Real, But Not What You Asked For

Constraint fidelity (does the response match what you *specifically* asked for?) is where providers diverge most: a 16-point gap between OpenAI (85%) and Perplexity (69%).

Ask for "affordable rooftop bars in Gangnam, Seoul" and every model returns real, verified venues. But half are champagne lounges with $30 cocktails. Claude ON scored 99% on foundational accuracy for this prompt (all 5 places exist) but just 12/30 on constraint fidelity. No provider scored above 12/30. "Affordable rooftop bar in Gangnam" is close to an oxymoron, and no model told the user that.

The pattern repeats on negative constraints ("not touristy," "no chains," "walk-ins only"). Verifying the *absence* of something is fundamentally harder than confirming its presence. These prompts dominate the hardest-scoring set in the benchmark.

> The full report includes the complete constraint fidelity analysis: budget constraints, subjective constraints, negative constraints, and where each provider breaks down.

---

## A Note for Builders: Scores Don't Capture Developer Experience

Gemini was the hardest API to benchmark by a wide margin — and these are the same issues any developer will hit at scale:

- **Token truncation**: Gemini's thinking consumes tokens from the output budget. 75% of offline Explore responses were truncated mid-sentence. The `thinking_budget` parameter meant to fix this is ineffective.
- **Grounding rate limits**: Google Search and Maps grounding has much tighter rate limits than non-grounded calls. Our entity verification pipeline had to stop and wait hours — sometimes up to 24 hours — before resuming. What should have taken hours took days.
- **RECITATION errors**: Search grounding frequently triggers the model's citation filter, returning nothing usable. We built a 10-attempt, 5-tier retry system that progressively disables grounding tools — meaning a chunk of evaluations ran without the search verification we wanted.
- **Safety blocks and empty responses**: Additional failure modes requiring their own retry strategies.

The issues hit us twice: the token truncation bug silently ate output during response generation (no equivalent issue with OpenAI, Anthropic, or Perplexity), and the grounding-specific errors (RECITATION, rate limits, safety blocks) plagued our evaluation pipeline. We chose Gemini as judge *because* of Search and Maps grounding — no other provider offers real-time Maps data for entity verification — but using it reliably at scale required significant engineering overhead.

Gemini's output quality *when it works* is excellent — best foundational accuracy, best Share scores. But if you're building on Gemini at scale, factor in the DX cost.

> The full report includes the complete developer experience analysis with specific error rates and workarounds.

---

## What This Means If You're Building on LLM APIs

1. **Raw LLM output is not production-ready.** Even OpenAI at 90.7 has an 8% catastrophic failure rate. You need a verification layer: existence checks, operating status, location constraints.
2. **Don't blindly enable search.** It helps discovery but hurts transactions. The optimal strategy is task-aware routing.
3. **The accuracy gap is a data problem, not a prompt engineering problem.** Perplexity has native search and still recommends closed places. Fresh, verified place data is the missing layer.
4. **Test on non-Western markets.** The USA/WE vs ROW gap is modest on averages but explodes on specific tasks.
5. **On-device models will struggle hard.** Without grounding, fatal flaw rates hit 12-22%. Edge-deployed models need a pre-verified local place database.

---

## Methodology at a Glance

- **345 prompts** across 5 use case categories, **4 LLM providers** in 7 configurations (search on/off), **2,415 evaluated responses**
- Geographic coverage: 50+ cities across 6 continents (~30% USA/WE, ~70% ROW)
- All responses generated and evaluated Feb-Mar 2026

**Evaluation pipeline**: Each response goes through automated Extract > Verify > Score phases using Gemini as judge, with every entity independently fact-checked against Google Search and Google Maps.

**Rubric (out of 100):**

| Component | Points | What It Measures |
|---|---|---|
| Completeness (R1) | /20 | Right number of results, right format, all sub-questions answered |
| Foundational Accuracy (R2) | /40 | Are the places real, open, and where the LLM says they are? |
| Constraint Fidelity (R3) | /30 | Do recommendations match what the user actually asked for? |
| Plausibility (R4) | /10 | Would a knowledgeable local find this response sensible? |

A single "fatal flaw" (fabricated place, permanently closed venue, wrong neighborhood) zeros the entity's score and counts 2x in the denominator, because a hallucinated place is worse than a missing one.

> The full report includes detailed methodology, rubric design rationale, judge reliability analysis, and all 345 benchmark prompts.

---

## Get the Full Report

The complete analysis includes:

- **Per-category breakdowns** with detailed score tables across all 7 provider configurations
- **The complete failure taxonomy** with real examples from every provider and the "confidently wrong" pattern
- **Search paradox deep dive**: where search helps, where it hurts, and why, across every task type
- **Geographic gap analysis**: USA/Western Europe vs Rest of World, by category and provider
- **Constraint fidelity analysis**: budget constraints, subjective constraints, negative constraints
- **Full evaluation methodology**, rubric design, judge reliability, and all 345 benchmark prompts

{% comment %} 
**[Download the Full Report (PDF) →](https://PLACEHOLDER EMAIL CAPTURE URL)** {% endcomment %}

<style>
  .report-form-container {
    max-width: 450px;
    margin: 25px 0;
    padding: 25px;
    background-color: rgba(255, 255, 255, 0.03); /* Very subtle highlight */
    border: 1px solid rgba(255, 255, 255, 0.1);
    border-radius: 8px;
  }
  .report-form {
    display: flex;
    flex-direction: column;
    gap: 15px;
  }
  .name-row {
    display: flex;
    gap: 15px;
  }
  .form-input {
    width: 100%;
    padding: 12px;
    background-color: #121212; /* Darker than the background */
    border: 1px solid #333;
    color: #fff; /* White text when they type */
    font-family: inherit; /* Inherits your monospace hacker font */
    font-size: 15px;
    border-radius: 4px;
    box-sizing: border-box;
    transition: border-color 0.2s;
  }
  .form-input::placeholder {
    color: #666; /* Dimmed out placeholders */
  }
  .form-input:focus {
    outline: none;
    border-color: #b5e853; /* This is the standard Jekyll hacker-theme green */
    box-shadow: 0 0 5px rgba(181, 232, 83, 0.2);
  }
  .submit-button {
    padding: 14px;
    background-color: #b5e853; /* Match the hacker green */
    color: #111; /* Dark text for contrast */
    border: none;
    font-family: inherit;
    font-weight: bold;
    font-size: 16px;
    cursor: pointer;
    border-radius: 4px;
    transition: opacity 0.2s;
    margin-top: 5px;
  }
  .submit-button:hover {
    opacity: 0.8;
  }
</style>

<div class="report-form-container">
  <form id="pdf-form" action="https://formspree.io/f/xzdjglpg" method="POST" class="report-form" onsubmit="handleFormSubmit(event)">
    <div class="name-row">
      <input type="text" name="first_name" placeholder="First Name" required class="form-input">
      <input type="text" name="last_name" placeholder="Last Name" required class="form-input">
    </div>
    <input type="text" name="company" placeholder="Company Name" required class="form-input">
    <input type="email" name="email" placeholder="Work Email Address" required class="form-input">
    
    <button type="submit" class="submit-button">Get the Full Report</button>
  </form>
</div>

<script>
  function handleFormSubmit(event) {
    event.preventDefault(); 
    var form = event.target;
    var data = new FormData(form);
    
    // Change button text to show it's working
    var btn = form.querySelector('.submit-button');
    var originalText = btn.innerText;
    btn.innerText = "Loading PDF...";
    btn.style.opacity = "0.7";
    
    fetch(form.action, {
      method: form.method,
      body: data,
      headers: { 'Accept': 'application/json' }
    }).then(response => {
      if (response.ok) {
        window.location.href = "https://raw.githubusercontent.com/ibukeev-voygr/llm-geospatial-benchmark-report/main/Benchmarking_LLMs_on_Local_Search_Q1_2026_Consumer.pdf";
      } else {
        alert("Oops! Formspree blocked the submission.");
        btn.innerText = originalText;
        btn.style.opacity = "1";
      }
    }).catch(error => {
      alert("Network error submitting the form.");
      btn.innerText = originalText;
      btn.style.opacity = "1";
    });
  }
</script>

---

## Bridging the Gap: Business Validation API

The failure modes we found (fabricated places, closed venues, wrong locations) are verifiable, automatable checks. We built a **Business Validation API** designed for AI developers and agents, catching these failures before they reach production.

Pass in a place name and address from any LLM response and get back: existence verification and operating status. These are the exact checks that would have caught fatal flaws in this benchmark.

**[Try the API →](https://dev.voygr.tech/checkout)** · [See the docs on GitHub](https://github.com/voygr-tech/dev-tools)

---

## About

Built by [VOYGR](https://voygr.tech). Comprehensive, up-to-date place data and intelligent local search for AI apps, agents, and analytics.

The benchmark framework, all prompts, and raw results will be open-sourced soon. We plan to rerun it as new model versions ship and expand to developer-focused use cases (geocoding, GeoJSON, batch POI validation).

[voygr.tech](https://voygr.tech) · [founders@voygr.tech](mailto:founders@voygr.tech)
