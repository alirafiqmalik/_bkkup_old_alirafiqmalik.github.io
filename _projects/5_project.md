---
layout: page
title: Web-Browsing Task Agent with Memory
description: A tiny AI agent that browses the web, remembers what it learns, and self-checks answers with a lightweight formal model.
img: assets/img/12.jpg
importance: 1
category: fun
related_publications: true
---

A lightweight, resume-ready **AI agent** that takes a natural-language goal, **browses** for answers, **remembers** prior results, supports **follow-ups**, and **checks** its final output against a small formal model.

---

## Why this project

- **Interview-ready:** Demonstrates an agent loop (observe → plan → act → reflect/memory).
- **Practical:** LLM + search/scrape + persistence (SQLite/JSON).
- **Trust layer:** A tiny **checker** catches common failure modes (e.g., hallucinated citations, illegal phase jumps).

---

## Features

- Natural-language goals (e.g., “Find the latest average gas price in Newark, NJ”).
- Browser/search tool via `requests + BeautifulSoup` or a search API.
- **Memory** of prior queries and results for follow-ups.
- Answer synthesis with **citations** (titles + URLs).
- **Formal-checker gate** before returning answers.
- Lightweight stack; finishable in **2–3 hours**.

<div class="row">
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/1.jpg" title="Agent loop" class="img-fluid rounded z-depth-1" %}
  </div>
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/3.jpg" title="Memory (SQLite)" class="img-fluid rounded z-depth-1" %}
  </div>
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/5.jpg" title="Web results & citations" class="img-fluid rounded z-depth-1" %}
  </div>
</div>

<div class="caption">
  Left: the observe → plan → act → reflect loop. Middle: a simple SQLite store for session memory. Right: parsed web results with citations.
</div>

You can also mix regular text between image rows, even citations {% cite einstein1950meaning %}.  
Below is the **architecture sketch** and a **checker stub** you can adapt.

```text
┌─────────┐     ┌─────────┐     ┌─────────┐     ┌──────────────┐
│ Observe │ ─→  │  Plan   │ ─→  │  Act    │ ─→  │ Reflect/Save │
└────┬────┘     └────┬────┘     └────┬────┘     └──────┬───────┘
     │               │               │                 │
     │               │               │                 │
     │          LLM (optional)  Search/Scrape    Memory (SQLite/JSON)
     │                                              ▲
     └─────────────────────────────── Answer ←──────┘
```

```python
def check_finish(answer, observations, transitions):
    assert observations, "Finish without observations"
    cited = extract_urls(answer)
    observed = {o.url for o in observations}
    assert cited.issubset(observed), "Cited URL not observed"
    assert is_valid_transition_sequence(transitions), "Illegal phase jump"
```

<div class="row">
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/5.jpg" title="CLI run: query" class="img-fluid rounded z-depth-1" %}
  </div>
</div>

<div class="caption">
  A simple CLI plans, fetches 3–5 sources, extracts numbers/dates, and synthesizes a short answer with 2+ citations.
</div>

### Getting started

```bash
git clone https://github.com/you/web-browsing-agent-memory
cd web-browsing-agent-memory
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
```

```yaml
# config.yaml
search:
  provider: serper # or serpapi, scrape
  api_key: ${SEARCH_API_KEY}
llm:
  provider: openai # optional
  model: gpt-4o-mini
  api_key: ${OPENAI_API_KEY}
memory:
  backend: sqlite
  path: data/agent.sqlite
```

<div class="row justify-content-sm-center">
  <div class="col-sm-8 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/6.jpg" title="2/3: Architecture diagram" class="img-fluid rounded z-depth-1" %}
  </div>
  <div class="col-sm-4 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/11.jpg" title="1/3: Tests passing" class="img-fluid rounded z-depth-1" %}
  </div>
</div>

<div class="caption">
  The 2/3 panel highlights the overall architecture; the 1/3 panel shows minimal tests keeping the checker honest.
</div>

The code is simple: wrap screenshots with `<div class="col-sm">` inside a `<div class="row">` (see the <a href="https://getbootstrap.com/docs/4.4/layout/grid/">Bootstrap Grid</a>).  
Make images responsive with `img-fluid`; add `rounded z-depth-1` for polish.

{% raw %}

```html
<div class="row justify-content-sm-center">
  <div class="col-sm-8 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/6.jpg" title="2/3: Architecture diagram" class="img-fluid rounded z-depth-1" %}
  </div>
  <div class="col-sm-4 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/11.jpg" title="1/3: Tests passing" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
```

{% endraw %}
