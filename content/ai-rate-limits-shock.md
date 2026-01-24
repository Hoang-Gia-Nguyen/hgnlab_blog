+++
title = "The Sudden Squeeze on Free AI API Limits"
date = 2025-12-25
description = "Why free and low-tier AI API limits have tightened, how it impacts hobbyists, and what to consider next."
[taxonomies]
categories = ["AI"]
[extra]
cover.image = "images/cover-ai-limits.png"
cover.alt = "Graph showing shrinking free-tier API limits"
+++

Over the last one to two years, most major AI providers have significantly tightened their free or low-tier API limits — slashing requests-per-minute (RPM) and requests-per-day (RPD), or removing free API usage entirely. If you mostly use web UIs, you may barely notice. But if you automate workflows or build small apps on free tiers, the change feels… savage.

**What changed and why it’s shocking**
- For web users: the interactive experience remains okay — typical session limits are generous enough for daily personal tasks.
- For API users: automation breaks. Background jobs, CLI tools, small bots, and personal integrations start hitting stricter RPM/RPD or get blocked behind paid plans.
- For hobbyists: even modest monthly fees are still a decision, especially when the app earns nothing. There’s also key-management risk under pay-as-you-go.

**Examples from big providers (last 1–2 years)**
As of 2025-12-25, here are provider notes with citations (limits vary by account, tier, and model):
- OpenAI:
  - Current: org/project-level limits measured by RPM/RPD/TPM; values vary by model and usage tier. See Rate Limits and Usage Tiers: https://platform.openai.com/docs/guides/rate-limits (includes examples of headers like x-ratelimit-limit-requests and x-ratelimit-limit-tokens; values are illustrative).
  - Change trend: phasing out free API credits; stricter per-org policies; pay-as-you-go with monthly usage caps and automatic tier graduation.
- Google (Gemini):
  - Current: tiered limits (Free, Tier 1–3) measured by RPM/TPM/RPD; view active limits in AI Studio. Docs: https://ai.google.dev/gemini-api/docs/rate-limits (last updated 2025-12-23), models: https://ai.google.dev/models/gemini, AI Studio limits: https://aistudio.google.com/usage?timeRange=last-28-days&tab=rate-limit.
  - Batch API: enqueued tokens caps per model and tier documented in the rate limits page.
- Anthropic (Claude):
  - Current: limits vary by plan and account; measured in requests/tokens/concurrency; view in Console. Docs home: https://platform.claude.com/docs (rate limit page moved/restructured), status: https://status.anthropic.com/.
- OpenRouter:
  - Current: free models are limited to 50 requests/day; if you purchase ≥10 credits, the free model limit rises to 1000 requests/day. FAQ: https://openrouter.ai/docs/faq#how-are-rate-limits-calculated. Limits reference: https://openrouter.ai/docs/api-reference/limits.
- Others (e.g., Cohere, Mistral-hosted APIs):
  - Current: quotas depend on model and plan; consult official docs and dashboards. Cohere docs: https://docs.cohere.com/; Mistral docs: https://docs.mistral.ai/.

**Why providers reduced free/API limits**
- Avoid fraud and abuse: some actors create hundreds or thousands of free accounts and run commercial applications on free keys.
- Compute pressure: bigger models and more users increase inference costs and capacity constraints.
- Post-trial monetization: the trial era is largely over; AI is mainstream and relied upon — time to charge sustainably.

**Impact difference: web vs API**
- Web UI: human-paced usage, anti-abuse controls, and session-based rate limits make it easy to keep experience acceptable.
- API: machine-paced calls spike concurrency; stricter RPM/RPD protects infrastructure and costs, but breaks small automations.

**Pricing isn’t deadly — but matters for hobbyists**
- Pay-as-you-go is reasonable for many, but hobby projects often have unpredictable bursts.
- Risk: exposing API keys in personal tools or small apps under usage-based billing.
- Mitigations: server-side proxies, scoped keys, daily caps, usage alerts, and test with smaller models.

**What I’ll do next (as a hobbyist)**
- Survey more generous providers and community-friendly tiers.
- Consider paying a small monthly amount where it makes sense and set hard budget caps.
- Add circuit breakers to automation (retry/backoff, queueing, nightly schedules within limits).
- Prefer local or hybrid setups where possible (small local models for routine tasks, cloud only when needed).

**Call for sources**
Authoritative links to current limits (checked 2025-12-25):
- OpenAI — Rate limits & usage tiers: https://platform.openai.com/docs/guides/rate-limits
- Google Gemini — Rate limits: https://ai.google.dev/gemini-api/docs/rate-limits (last updated 2025-12-23); Models: https://ai.google.dev/models/gemini; AI Studio limits: https://aistudio.google.com/usage?timeRange=last-28-days&tab=rate-limit
- Anthropic Claude — Docs home: https://platform.claude.com/docs; Status: https://status.anthropic.com/
- OpenRouter — Free model rate limits: https://openrouter.ai/docs/faq#how-are-rate-limits-calculated; Limits reference: https://openrouter.ai/docs/api-reference/limits
- Mistral — Docs: https://docs.mistral.ai/
- Cohere — Docs: https://docs.cohere.com/

If you automate with free AI APIs, you’ve likely felt this squeeze. I’m still deciding whether to migrate to more generous providers or accept a small monthly cost for my hobby. Either way, as soon as i find something, I’ll share notes and setups that help keep our tiny projects alive.
