+++
title = "After the Free Tier: how a hobbyist continues living with LLMs"
date = 2026-02-09
draft = false
description = "After RPM/TPM limits tightened, I was forced to re-evaluate how I use LLMs and redesign a cost-efficient, sustainable setup for personal needs."
[taxonomies]
categories = ["Technology"]
[extra]
cover.image = "images/llm-usage-shift-cover.png"
cover.alt = ""
+++

# From “free riding” to intentional cost optimization

After my previous post about [LLM providers tightening RPM/TPM limits and how that broke my workflow](@/ai-rate-limits-shock.md), I realized something quite clearly:  
the real problem wasn’t that free tiers were getting worse, but that I had grown used to treating LLMs as an *“unlimited”* resource.

When everything was free, I didn’t really need to think about how often I called the API, how many tokens were being burned, or how many times the same context was shoved back into the model during a single workflow. As long as it worked, that was good enough.

But when the free tier stopped being sufficient, my first reaction wasn’t to look for ways to bypass limits or create extra accounts. Instead, I paused and asked a different question:  
**what do I actually need LLMs for, and which parts of my workflow are truly worth paying for?**

From that point on, the way I approached LLMs started to change. I no longer saw them as something that *had* to be free, nor did I assume that paying per token was always the right default. Instead, I began to separate my use cases, measure real costs, and choose the pricing model that fit each type of work.

This post is the story of that transition: from “using it for free as long as possible” to using LLMs with more intention around cost. Not to optimize to an extreme, but to keep my personal automations and coding workflows running smoothly, sustainably, and without turning every LLM interaction into a source of token anxiety.

# Branch 1: Pay-on-demand for small, low-token automations

## Why I chose to pay (but not blindly)

Once the free tier was no longer enough, my instinct wasn’t to dodge limits, but to re-examine what I actually use LLMs for on a daily basis. Looking closely at my personal automations, I realized something fairly simple: most of them don’t need *free* LLMs — they need *reliable* ones.

Automation is different from casual chatting or one-off questions. It runs regularly, on a schedule, and if it gets blocked by quotas or RPM limits, the entire downstream workflow stalls. At that point, paying no longer felt like “losing the advantage of free”, but more like buying a small piece of infrastructure so things could keep running.

More importantly, I didn’t accept paying for everything. I only paid for the parts where LLM involvement actually added value.

## Not every automation needs a “powerful” LLM

I used to have a pretty common assumption: if you’re calling an LLM, you should use the smartest, strongest model available — otherwise it’s not “worth it”.

That mindset quickly turned into waste when applied to automation.

Most of my automations are handled entirely by plain code:
- data filtering,
- content aggregation,
- rule application,
- input normalization.

The LLM only appears at the very end, where language understanding or human-like judgment is needed: summarization, classification, or suggesting actions. For these tasks, I don’t need deep reasoning or high creativity. What I need is a model that’s *good enough*, stable, and predictable in cost.

Right now, I’m using pay-on-demand with Groq, while also experimenting in parallel with Gemini 2.0 Flash because of its attractive pricing. Groq is well known for its extremely high throughput, and that’s genuinely impressive — but for my personal use, throughput isn’t the deciding factor.

My automations aren’t real-time systems, and they don’t serve thousands of requests per minute. They run intermittently throughout the day, each LLM call being a small, low-token task. So instead of optimizing for raw speed, I care more about:
- cost per request/token,
- and whether the model is “smart enough” for simple decisions.

In other words, high throughput is nice to have, not the core reason I choose a provider.

## A concrete example: a mail analysis app

The clearest example of how I use pay-on-demand LLMs is my personal Gmail sync app. Most of the work here doesn’t require an LLM at all:

- syncing emails,
- filtering by sender, label, and time,
- extracting the main content,
- removing signatures, ads, and noise.

All of this is handled by code. The LLM is only called at the final step, where it reads the already-condensed content and answers a few very specific questions: is this email important, does it require a quick response, and if so, what should I do next?

Because the input is tightly controlled, token usage is low and consistent. The LLM isn’t the center of the system — it’s just a thin “decision layer”. This usage pattern is exactly what makes pay-on-demand sensible: I pay only for the value the LLM actually adds, nothing more.

## LLMs as a “smart function”, not the system core

After a while, I realized the real issue isn’t cloud LLM vs local LLM — it’s the role LLMs play in the architecture. When you treat the LLM as the center, everything gets pushed into context, tokens explode, and costs spiral out of control. But when you treat the LLM like a smart function — clear input, constrained output — costs become far more predictable.

This approach works extremely well for small, low-token automations. However, its limitations became obvious when I applied the same thinking to coding agents and larger codebases. In those cases, tokens are no longer small, context is no longer compact, and pay-per-token turns into a completely different problem.

# Branch 2: Renting GPUs by the hour to run “local LLMs”

## When pay-per-token starts working against you

Using pay-on-demand LLMs for small automations worked great, but things went out of sync when I applied the same model to coding agents. Here, the LLM doesn’t just read a short input and reply — it has to constantly interact with the code: open files, understand structure, modify one part, then re-read related parts to avoid breaking something else.

The problem is that every iteration consumes new tokens. Not because the questions are getting harder, but because the context has to be resent over and over. During long debugging or refactoring sessions, tokens stop representing new value and start representing the cost of reminding the LLM what it already “knew” in the previous step.

Even with modern context management tools and provider-side caching, token consumption in this use case is still massive.

At that point, it became clear that pay-per-token simply doesn’t fit open-ended, iterative coding work.

### Use case 1: Large or unfamiliar workspaces

The first place this problem became obvious was working with relatively large workspaces. They don’t need to be huge monorepos — just a project with:
- multiple directories,
- a few custom libraries,
- a non-trivial structure.

In these cases, a coding agent needs to read quite a lot just to understand the context.

Every time I asked it to modify some logic, the LLM had to:
- reload relevant files,
- cross-check them against the overall structure,
- ensure changes didn’t break other parts.

If the result wasn’t quite right, the loop repeated. Tokens burned quickly, even though the task itself didn’t feel particularly complex. It was frustrating: I wasn’t asking more, the project simply couldn’t be reduced to short, clean contexts.

That’s when I realized: with large workspaces, token cost doesn’t scale with problem difficulty, but with context fragmentation.

### Use case 2: Learning playgrounds

The second use case burns tokens even more aggressively — and I think many people can relate: using LLMs as a playground to learn a new framework from scratch.

When I don’t understand a framework yet, my early questions are vague, and many of my assumptions are simply wrong. Learning isn’t a clean Q&A process — it’s:
- ask → try → fail,
- ask again → fix → fail again,
- keep asking “why” with layers of overlapping context.

Each loop makes the context longer and messier, filled with temporary ideas, experiments, and misunderstandings. The LLM has to carry all of that history forward, and token usage grows rapidly. Ironically, this is when I need LLMs the most — because I don’t even know what I don’t know yet.

After a few rounds, it became clear: pay-per-token is especially hostile to exploratory learning, where mistakes and repetition are normal.

## Why pay-per-token doesn’t fit these use cases

After these two use cases, I stopped and looked at the problem differently. Previously, my reflex was to ask: is this model smart enough? or is this provider optimizing tokens well? But the more I worked with coding agents and learning playgrounds, the more those questions missed the point.

The issue wasn’t that the LLM was bad, or that my prompts were wrong. The issue was the pricing model itself. Pay-per-token assumes that each new token delivers new value. That assumption holds for short, well-defined interactions — but it collapses in long, iterative, exploratory sessions.

In both cases — large workspaces and learning new frameworks — most tokens aren’t spent solving new problems, but maintaining context: repeating directory structures, code already read, previous assumptions, including wrong ones. Tokens stop being the cost of “thinking” and become the cost of short-term memory.

Once I realized this, it was clear that this wasn’t a model-selection or prompt-optimization problem anymore. I needed a different cost model — one that fits work where iteration count can’t be predicted. A model where I could freely try, fail, fix, and ask again without constantly watching the token counter.

That realization led me to a different approach: paying for *time*, not for repeated units of context. And that’s where renting GPUs by the hour to run a “local LLM” started to make sense — not because it’s absolutely cheaper, but because it fits how I actually work.

## Renting GPUs by the hour: “local LLM”, but not really local

Once I identified pay-per-token as the core mismatch, the next question was straightforward: is there a way to pay for an open-ended work session instead of each context iteration?

The answer I found was hourly GPU rental to run a “local” LLM — even though it’s not local in the traditional sense.

With services like RunPod, I can rent GPUs with around 20–24GB of VRAM by the hour at a very reasonable price. What matters here isn’t benchmarks or throughput, but the completely different working mindset: during that session, I can ask, tweak, experiment, and redo things freely without thinking about how many tokens each iteration costs.

Paying by the hour creates a fixed cost frame for each session. I know that for those 3–4 hours, whether I refactor ten times, read dozens of files, or get stuck on a tiny detail, the cost stays within a clear boundary. This is perfect for unpredictable coding sessions — sometimes things flow, sometimes you get stuck.

At that point, the LLM stops being an API you have to ration, and becomes a shared workspace. I can let coding agents read entire workspaces, keep context longer, and accept repetition as a natural part of thinking. Instead of squeezing everything into short prompts to save tokens, I can focus on solving the problem.

Hourly GPU rental doesn’t magically make the LLM smarter, but it completely changes the usage psychology. Asking another question no longer feels like “buying more cost”, but like making better use of a session I’ve already paid for. For me, this is where it clearly outperforms pay-per-token in long, iterative, exploratory use cases.

## Tips for optimizing LLM sessions when renting GPUs

After renting GPUs by the hour for a while, I realized that costs don’t just come from working time, but also from the initial minutes spent setting up the environment. And since services like RunPod start billing the moment you click “rent GPU”, every minute of waiting is real money.

The most important tip for me is: if the service allows it, **build your own custom template (custom image)**.

In simple terms, this is an image that already contains everything you need:
- libraries,
- tools,
- runtimes,
- and the models you use regularly.

Each time you rent a GPU, instead of starting from a blank environment and reinstalling everything, you can launch directly from this image and start working almost immediately.

Some services offer network drives to persist personal setup between sessions. This is convenient, but it comes at a clear cost: you pay for storage size × storage time, even when you’re not using a GPU. For people who work daily, that might be fine — but for me, who only rents GPUs when needed, paying for an idle disk doesn’t make sense.

By contrast, building a personal image works the other way around. Even if the image is large, server-side download speeds are extremely high. My current image is around 20GB, and it takes only about 3–4 minutes to pull and start. Compared to reinstalling environments and downloading models step by step, the difference is huge.

The key takeaway is this: **setup time is hidden cost**. When you pay by the hour, every minute of waiting reduces the effectiveness of your session. A custom image turns each GPU rental into a real “work shift”, instead of spending half the time preparing.

For me, this has been one of the simplest optimizations with the biggest impact after switching to hourly GPU rentals.

# Closing: paying the right way for the right rhythm of work

Looking back at last month, the amount I actually spent on LLMs is… almost funny.

- Groq API (pay-on-demand, low-token): ~0.8 USD  
- RunPod (hourly GPU rental): ~4 USD  

Ironically, the most expensive part wasn’t the “real work”, but the early phase of tinkering:
- building custom images to initialize pods,
- trial-and-error,
- repeatedly testing automation tools with large, expensive models.

Once things stabilized:
- I had a personal image with libraries, models, and familiar configs, taking only ~3 minutes to spin up RunPod; each 4-hour session now costs about ~1 USD,
- my automations settled on models that are good enough for the job and friendly to a personal budget.

What’s interesting is that this was also the month I built the **most useful personal tools** since I started vibing with LLM-assisted coding.

Not because I used better models, but because I stopped feeling the pressure of “burning tokens” while learning or being wrong.

In the end, a clear pattern emerged:

Small automations, clear tasks, few calls → pay-on-demand per token is more than enough.

When dealing with:
- large workspaces,
- reading other people’s code,
- learning new frameworks from scratch,
- frequent trial and error  
→ hourly GPU rental is cheaper, more comfortable, and psychologically much healthier.

I don’t think there’s a single “optimal” model.
There’s only the model that fits your working rhythm at a given moment.

For me right now:
- LLMs aren’t something I keep “on” all day,
- they’re a workbench I can ***rent when needed, and return when I’m done***.

Looking back, this isn’t a story about finding a cheaper way to use LLMs, but about learning:
- what role LLMs play in each use case,
- how to use them with clearer intent,
- and how to pay with clearer intent.

The real question isn’t free vs paid, or cheap vs expensive — it’s whether you actually control how you’re using the tool.
