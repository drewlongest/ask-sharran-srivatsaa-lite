# ask-sharran-srivatsaa-lite

A Claude Code agent and skill that answers a question the way [Sharran Srivatsaa](https://www.youtube.com/@sharran) would, grounded exclusively in his hosted 2026 knowledge base (YouTube videos and Shorts, his podcast, his Substack newsletter, and X posts) with per-claim citations to the exact source, never from the model's training data.

Lite means paywalled sources are excluded from the corpus. For Sharran, that exclusion is currently empty: nothing in his source library is flagged paywalled.

## Corpus

The `sharran-srivatsaa-lite` namespace covers five source groups, measured from the 2026-08-24 send ledger (4,184 records total): his long-form YouTube videos and Shorts (2,192 records), his podcast archive (1,573 records across 321 episode syntheses plus their burst passages and Q&A pairs), his Substack newsletter (274 records), 20 monthly archives of his X posts (116 records), and 29 records routed in from multi-speaker episodes he appeared on. No courses, no books. A question that depends on those is out of corpus, and the agent says so plainly instead of guessing.

Measured 2026-08-24 (Pinecone `describe_index_stats` on index `expert-kb`): the `sharran-srivatsaa-lite` namespace holds 4,218 vectors: the 4,184 current corpus records plus 34 legacy vectors left over from the earlier partial send, scheduled for removal.

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

- The corpus is bounded to four public source types: YouTube videos and Shorts, his podcast, his Substack newsletter, and X post archives (plus segments routed in from multi-speaker episodes he appeared on). A question outside those is out of corpus.
- Absence of evidence means the corpus is silent on the topic, never that Sharran disagrees. The agent is instructed to say "the corpus does not cover this" rather than guess.
- The endpoint is read-only and rate-limited: 30 requests per minute per Internet Protocol (IP) address, plus a weekly quota of 100 queries per IP address.

## Status

On 2026-08-22, a direct query against the search endpoint returned HTTP 200 with 3 hits from namespace `sharran-srivatsaa-lite`, confirming the namespace is live and served by the Worker. On 2026-08-24 the full corpus re-upsert landed: 4,184 records sent in delta mode (send ledger, 2026-08-24), replacing the earlier 158-vector partial send. The per-source record counts are in the Corpus section above.

This is an unofficial fan/study project; answers are an analyst's channeling of Sharran Srivatsaa's published positions, not Sharran himself.
