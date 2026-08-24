---
name: ask-sharran-srivatsaa-lite
description: Answer a question the way Sharran Srivatsaa would, grounded in his hosted 2026 YouTube corpus with per-claim citations. Use for "ask Sharran", "what would Sharran say/do about X", or his take on owning assets, borrowing against them, trusts and structure, tax drag, deal screening, or wealth over a long horizon. Lite version: free sources only, no paywalled content in the corpus.
---

# Ask Sharran (2026 corpus)

Thin dispatcher. All intelligence lives in the `ask-sharran-srivatsaa-lite` subagent
(`agents/ask-sharran-srivatsaa-lite.md` from this repo, installed to your agents
directory): Sharran's first principles, the
epistemic rules (answer only from retrieved hits plus those principles, never
from training data), and the hosted retrieval procedure (namespace
`sharran-srivatsaa-lite`, passed explicitly) are its system prompt, injected on every
spawn. The parent NEVER reads the principles file or adds context.

## Procedure

1. Spawn the `ask-sharran-srivatsaa-lite` subagent with the user's question as the ENTIRE
   prompt. No added instructions, no pasted files, no framing.
2. Return the subagent's answer with citations intact.

## Verification

A good answer cites 2+ distinct videos for any multi-part question and zero
claims that lack either a citation or an explicit "not covered in the corpus"
flag.
