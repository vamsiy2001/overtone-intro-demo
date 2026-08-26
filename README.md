# Grounded introduction drafts

A small prototype of what a transparent, evaluated "why you might match" feature could look like for a modern matchmaking product. Built after reading Overtone's job posting for a founding Applied AI/ML Engineer, as a working example rather than another bullet point.

**[Try it live](https://vamsiy2001.github.io/overtone-intro-demo/)** — open it and click through, no setup needed.

## What it does

Two mock voice-intake transcripts go in. The pipeline:

1. **Extracts** a structured compatibility profile from each transcript (values, communication style, conflict style, novelty vs. routine, humor, dealbreakers) via a JSON-mode call to `openai/gpt-oss-120b` on Groq.
2. **Retrieves** the relevant snippets from a small relationship-science reference set, based on the extracted traits, not a full vector DB for this prototype, but the same retrieve-then-generate shape a production RAG pipeline would use.
3. **Drafts** a short introduction narrative, instructed to cite a retrieved snippet inline for every specific compatibility claim it makes.
4. **Evaluates** the draft with a second model call acting as an LLM judge: a groundedness score, a list of any claim not actually backed by the trait data or the snippets, and a plain verdict on whether it's ready for a human matchmaker to review.

The idea: an AI-generated introduction should never reach a matchmaker, let alone a dater, without something checking that every claim in it is actually true of the two people involved. That eval step is the point of the demo, not the narrative generation itself.

## Why

Overtone's posting names two things almost directly: "matchmaking insight generation" and evaluation infrastructure "so LLM judges are aligned." This is a small, honest attempt at both, built with the same pattern (hybrid retrieval, RAGAS-style grounding checks, LLM-as-judge) used in my other projects:

- [DevDocs AI](https://github.com/vamsiy2001/DevDocs_AI-Intelligent_Documentation_Assistant) — hybrid RAG with RAGAS evaluation
- [Customer Support LLM fine-tuning & eval pipeline](https://github.com/vamsiy2001/Customer_Support_AI-Fine_Tuning_Evaluation_Pipeline) — LoRA fine-tuning with an LLM-as-judge eval loop

## Running it

It's a single static HTML file, no build step, no backend, no setup. Open the live link and use it.

The API key is baked into the client-side code, on Groq's free tier (no card required, so the worst case if it's misused is the key needing a rotation, not a bill). Groq doesn't support Google-style referrer-restricted keys, so this is a deliberate, disclosed tradeoff for a portfolio demo rather than something I'd ship this way in production; see the limitation below.

## A deliberate limitation

This calls the Groq API directly from the browser with an embedded key, which is fine for a personal demo but is not how a real product should ship a key-gated feature: a production version would put this behind a small server-side proxy so no key, not even a restricted one, ever reaches the client. Said differently, wanted to be upfront about the corner cut here rather than pretend it isn't one.

## Stack

Vanilla HTML/CSS/JS, no dependencies, no build step. Model: `openai/gpt-oss-120b` via [Groq](https://groq.com) (free tier, no card required).

---

Built by [Vamsi Krishna Yerubandi](https://vamsiy2001.github.io) · [GitHub](https://github.com/vamsiy2001)
