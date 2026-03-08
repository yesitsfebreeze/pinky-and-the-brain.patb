# Changes
<!-- cross-project-relevant changes are logged here -->

#### 2026-03-09 — Phase 4: @commit, @plan, rating scale 0-1000
@commit creates per-scope commits; @plan replaces PLAN.md as source-root todo file with @process command; rating scale bumped from 0-100 to 0-1000 (old notes ×10 on load).

#### 2026-03-09 — Phase 3: concept graph BFS query expansion
Added concept tagging (1-3 tags per note) and concepts.md auto-generation; "what do you know about X" now does BFS on concept co-occurrence graph up to CONTEXT_DEPTH hops; related-notes field for direct note links.

#### 2026-03-09 — Phase 2: context assembly, ranked loading
Added prune pass + selection pass at startup: relevance = rating + recency_bonus + repo_match_bonus; top MAX_CONTEXT_NOTES loaded; explicit queries search full pool; cross-project repos ranked by recency + context overlap.

#### 2026-03-08 — New config keys: PRUNE_THRESHOLD, MAX_CONTEXT_NOTES, MAX_CONTEXT_FILES, MAX_LINKED_REPOS, CONTEXT_DEPTH, PATB_URL
Updated @brain YAML schema to include all new Phase 2-4 config keys with documented defaults. PATB_URL allows overriding brain repo URL when it differs from {source}.patb.

#### 2026-03-08 — @pinky semantics change: line 1 is brain repo URL
@pinky line 1 now stores the brain repo URL ({SLUG}.patb), not the source repo URL. BRAIN_REPO_URL resolution priority: PATB_URL from @brain → @pinky line 1 → derived {SOURCE}.patb.

#### 2026-03-08 — Robust SETUP.md with RESSURECT mode
Hardened SETUP.md for resilient re-install flow. RESSURECT=TRUE forces full re-install from latest main while preserving user content.
