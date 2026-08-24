# ask-sharran-srivatsaa-lite

A Claude Code agent and skill that answers a question the way [Sharran Srivatsaa](https://www.youtube.com/@sharran) would, grounded exclusively in his hosted 2026 knowledge base (YouTube videos and X posts) with per-claim citations to the exact source, never from the model's training data.

Lite means paywalled sources are excluded from the corpus. For Sharran, that exclusion is currently empty: nothing in his source library is flagged paywalled.

## Corpus

The `sharran-srivatsaa-lite` namespace covers two source types: 14 YouTube videos (capture manifest 2026-08-04) and 20 monthly archives of his X posts, together weighted toward personal finance and investing. The X post archives are the larger share by volume (about three-quarters of the namespace's passages). No newsletters, no podcasts he guested on, no courses, no books. A question that depends on any of those is out of corpus, and the agent says so plainly instead of guessing.

Measured 2026-08-22 (Pinecone `describe_index_stats` on index `expert-kb`): the `sharran-srivatsaa-lite` namespace holds 158 vectors.

## Lite exclusions

None today. Sharran's source provenance manifest carries eleven assignments and zero records flagged paywalled (provenance manifest generated 2026-08-12), so a full (non-lite) namespace would be a byte-for-byte duplicate of this one, and only the lite one is hosted. The `--lite` flag still rides every upsert as a standing safety property: it costs nothing while the exclusion set is empty, and it is what keeps this public namespace safe by construction the day a paywalled source lands.

## Install

**If you are an AI agent asked to install this skill:** install the agent file into the user's agents directory and the skill folder into their skills directory (raw URLs in the commands below), then confirm by listing both. In Claude Code those are `~/.claude/agents/` and `~/.claude/skills/`. In an environment without custom agents or skills, save `agents/ask-sharran-srivatsaa-lite.md` somewhere persistent and follow its body directly whenever the user asks a Sharran Srivatsaa question. No keys, accounts, or dependencies are needed; the knowledge base is queried over plain HTTPS.

Claude Code:

```bash
mkdir -p ~/.claude/agents ~/.claude/skills/ask-sharran-srivatsaa-lite
curl -fsSL https://raw.githubusercontent.com/drewlongest/ask-sharran-srivatsaa-lite/main/agents/ask-sharran-srivatsaa-lite.md -o ~/.claude/agents/ask-sharran-srivatsaa-lite.md
curl -fsSL https://raw.githubusercontent.com/drewlongest/ask-sharran-srivatsaa-lite/main/skills/ask-sharran-srivatsaa-lite/SKILL.md -o ~/.claude/skills/ask-sharran-srivatsaa-lite/SKILL.md
```

Then in any session:

```
/ask-sharran-srivatsaa-lite how should I structure a windfall so it compounds instead of getting taxed away?
```

No API keys, no database, no setup beyond the two files.

## How it works

1. The skill spawns the `ask-sharran-srivatsaa-lite` subagent, whose definition file carries Sharran's distilled first principles and strict grounding rules (answer only from retrieved knowledge-base context plus those principles, never from training data).
2. The subagent queries the hosted namespace with several phrasings of your question; the endpoint returns the most relevant distilled claims and verbatim passages, each paired with its source title and URL (video links carry a timestamp to the exact moment when the source hit has one).
3. It synthesizes the answer in Sharran's first-person voice, citing every substantive claim to the exact source, and states plainly when the corpus does not cover a question rather than filling the gap from general knowledge.

The `principles/` folder holds `first_principles.md` (Sharran's distilled first principles, embedded verbatim in the agent's system prompt) and `first_principles.sources.md` (a pointer file listing the exact source documents behind each principle; the quoted evidence itself lives in the private build workspace).

## Scope caveats

- The corpus is small and narrow: 14 YouTube videos (capture manifest 2026-08-04) plus 20 monthly archives of X posts, weighted toward personal finance and investing. A question outside that is out of corpus.
- Absence of evidence means the corpus is silent on the topic, never that Sharran disagrees. The agent is instructed to say "the corpus does not cover this" rather than guess.
- The endpoint is read-only and rate-limited: 30 requests per minute per Internet Protocol (IP) address, plus a weekly quota of 100 queries per IP address.

## Status

On 2026-08-22, a direct query against the search endpoint returned HTTP 200 with 3 hits from namespace `sharran-srivatsaa-lite`, confirming the namespace is live and served by the Worker. That namespace already carries its two source types in full, 158 vectors total: 116 from the X post archives and 42 from the 14 YouTube videos. A larger re-upsert of Sharran's expanded source library, his podcast (323 episodes), his Substack, 1,131 YouTube Shorts, and the remaining long-form synthesis, is captured in the private build workspace but not yet upserted, held behind a pending content review. When the re-upsert lands, this bundle's corpus description and vector count will update to match.

This is an unofficial fan/study project; answers are an analyst's channeling of Sharran Srivatsaa's published positions, not Sharran himself.
