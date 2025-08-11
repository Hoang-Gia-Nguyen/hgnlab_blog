+++
title = "How I’m Doing TDD with ‘Vibe Coding’ (Codex + Gemini + GitHub Actions)"
date = 2025-08-11
draft = false
description = "How I’m Doing TDD with ‘Vibe Coding’ (Codex + Gemini + GitHub Actions)"
[taxonomies]
categories = ["AI", "Technology"]
tags = ["multi-agent", "agentic-ai"]
[extra]
cover.image = "images/tdd-vibe-coding-cover.png"
cover.alt = "A cool vibe coding banner"
+++

I’ve been experimenting with a multi-agent (free way, not something like CrewAI), _vibe-coding_ workflow that still stays faithful to **test-driven development (TDD)**. The cast:

- **OpenAI Codex** as the _developer_ (I got 3 months free via a Shopee promo).
- **GPT chat** as the _PM/spec writer_ to refine requirements.
- **Gemini (via Github Action Workflow)** as the _code reviewer_, triggered from **GitHub Actions**.
- **GitHub Actions** runs the full test suite on every push/PR.

The goal is simple: write good tests first, then let AI iterate until **the tests pass**—no hand-holding.

---

# Why TDD for vibe coding?

Traditional “ask AI, click run, manually test, repeat” burns time because _you_ sit there mediating. With TDD:

- Tests become the contract.  
- AI is forced to implement until green.  
- CI enforces quality on every change, not just the first landing.

---

# My step-by-step flow

1) **Co-define requirements with GPT chat**  
   I start a chat and ask GPT to help **clarify scope, edge cases, and acceptance criteria**. We translate that into plain-English scenarios that can map to tests later.

2) **Ask Codex to write the tests first**  
   I prompt Codex with “Write the test plan and the failing tests for these requirements. The tests must run locally and in GitHub Actions.”  
   > This is the highest-effort human step: I push Codex to produce _effective_, minimal, and portable tests (no external services, hermetic fixtures, fast runtime).

3) **Wire up CI early**  
   I add a GitHub Actions workflow that:
   - installs deps,
   - runs the tests,
   - and blocks merges on failure.

4) **Ask Codex to implement the feature**  
   I remind Codex explicitly:  
   - **It must not modify tests.**  
   - It must implement until **all tests pass locally**.  
   AI has to run the tests and iterate untill all test cases return green results.

5) **Open a PR from Codex’s web interface**  
   Once green locally, I trigger a PR with one click.

6) **Automatic review with Gemini on PR**  
   When the PR opens, **GitHub Actions** runs a small **Gemini CLI script** to review the diff (naming, complexity, missed cases, security gotchas). While Gemini reviews, I also do a quick manual test.

7) **If something’s off, loop back**  
   I paste the review notes (mine + Gemini’s) back to Codex and say “fix without touching tests.” Repeat until green.

8) **Merge it**  
   With the tests in the workflow, **every future commit** is automatically validated by **the whole suite**, not just the new cases.

---

# Workspace that I apply this method
[Expense Tracker App](https://github.com/Hoang-Gia-Nguyen/expense-tracker-worker)
This is a serverless application that records my personal spending. It serves a small static front end and exposes a JSON API backed by a Cloudflare D1 database. Hope that you can take a look and grab something.

---

# Guardrails I rely on

- **Tests first, then implementation.** It keeps the conversation objective.
- **Fast and deterministic tests.** Sub-second where possible; hermetic fixtures.
- **No network in unit tests.** Use fakes/mocks; integration tests are separate.
- **CI as the source of truth.** Local is nice; CI is the gate.
- **AI cannot edit tests.** If a test is wrong, _I_ edit it after discussion, not the model.

---

# Benefits I’m seeing

- **Less babysitting:** AI iterates until the tests pass; I’m not glued to the screen.  
- **Better regression safety:** The whole suite runs on every commit/PR.  
- **Multi-agent clarity:** GPT writes the spec, Codex implements, Gemini reviews—clean separation of duties.  
- **Speed with confidence:** I move faster without sacrificing correctness.

---

# Useful links

- **OpenAI Codex** — [Codex](https://openai.com/codex/)
- **Gemini CLI in Github Action** — [google-github-actions/run-gemini-cli](https://github.com/google-github-actions/run-gemini-cli)  
