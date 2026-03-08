# Thoughts

#### @pinky line 1 stores brain repo URL
<!-- rating: 800 | created: 2026-03-09 | last_used: 2026-03-09 | concepts: identity, brain-repo, pinky -->
<!-- sources: @pinky, SKILL.md -->
@pinky line 1 is the brain repo URL ({SLUG}.patb), NOT the source repo URL. Lines 2+ are linked brain repo URLs. If missing, create with derived brain URL on line 1. This changed from earlier versions where line 1 was the source URL.

#### SETUP.md is the single-source installer
<!-- rating: 750 | created: 2026-03-08 | last_used: 2026-03-09 | concepts: install, setup -->
<!-- sources: SETUP.md -->
SETUP.md handles both INSTALL and UPDATE modes; RESSURECT=TRUE forces UPDATE path re-installing from scratch while preserving user content (thoughts.md, changes.md, tree.md, sync.md, @pinky linked repos).

#### Selection pass — relevance formula
<!-- rating: 700 | created: 2026-03-09 | last_used: 2026-03-09 | concepts: context-loading, selection, relevance -->
<!-- sources: SKILL.md -->
Relevance formula for context selection: `rating + recency_bonus(+20 if last_used ≤ 3 days, +10 if ≤ 7 days) + repo_match_bonus(+15 if note sources exist in current workspace)`. Top MAX_CONTEXT_NOTES (default 8) loaded into session. Full pool used for explicit queries.

#### SKILL.md is fetched remotely at session start
<!-- rating: 700 | created: 2026-03-08 | last_used: 2026-03-09 | concepts: skill, version-check -->
<!-- sources: SKILL.md -->
SKILL.md local copy is just a bootstrap pointer with a FETCH and EXECUTE directive. Version check compares install.version and skill.version line 2 (timestamp) against remote to trigger auto-update.

#### Prune pass at startup
<!-- rating: 650 | created: 2026-03-09 | last_used: 2026-03-09 | concepts: prune, memory-lifecycle -->
<!-- sources: SKILL.md -->
At startup, notes with `rating < PRUNE_THRESHOLD` are deleted from thoughts.md and logged in changes.md. PRUNE_THRESHOLD defaults to MIN_RATING. After prune, only top MAX_CONTEXT_NOTES notes (by relevance) are loaded into the prompt.

#### Concept tagging and concepts.md
<!-- rating: 650 | created: 2026-03-09 | last_used: 2026-03-09 | concepts: concept-graph, tagging, bfs -->
<!-- sources: SKILL.md -->
Each note can carry 1-3 concept tags in the `concepts` field of its metadata comment. concepts.md is auto-generated (never edit manually) — aggregates all tags with co-occurrence `related` links. Queries do BFS expansion up to CONTEXT_DEPTH hops on the concept graph before text scoring.

#### @brain YAML config keys (full set)
<!-- rating: 600 | created: 2026-03-08 | last_used: 2026-03-09 | concepts: config, brain-repo -->
<!-- sources: SKILL.md, @brain -->
@brain stores user-editable YAML: SKILL_URL, PATB_URL (optional override), FOLLOW, AVOID, MAX_NOTES, MIN_RATING, PRUNE_THRESHOLD, MAX_CONTEXT_NOTES (default 8), MAX_CONTEXT_FILES (default 5), MAX_LINKED_REPOS (default 3), CONTEXT_DEPTH (default 2). Must be preserved across updates.

#### Rating scale is 0–1000
<!-- rating: 600 | created: 2026-03-09 | last_used: 2026-03-09 | concepts: rating, memory-lifecycle -->
<!-- sources: SKILL.md -->
The rating scale is 0–1000 (bumped from 0–100 in Phase 4). Old notes with rating ≤ 100 created before the upgrade are multiplied by ×10 on first load. Score events: used_in_reasoning +300, confirmed_by_code +500, unused_recall -100, contradicted -800.

#### @commit creates per-scope commits
<!-- rating: 600 | created: 2026-03-09 | last_used: 2026-03-09 | concepts: commit, workflow -->
<!-- sources: SKILL.md -->
The `@commit` command groups changed files by scope (feature, fix, refactor, docs, etc.) and creates one commit per scope, then pushes. After push, the post-push hook runs: fetch both repos, compare source HEAD vs sync.md, index new commits into thoughts.md/tree.md/changes.md.

#### @plan / @process workflow
<!-- rating: 600 | created: 2026-03-09 | last_used: 2026-03-09 | concepts: plan, workflow, todos -->
<!-- sources: @plan, SKILL.md -->
@plan is a todo file at source root. Content above the thick separator (█████) is freeform ideas/text. Content below is AI-generated todos. `@process` triggers AI to pick the most impactful next todo, gather context, solve it, delete the todo, then commit. Renamed from PLAN.md → @brain (source) → @plan across phases.

#### SLUG derivation
<!-- rating: 550 | created: 2026-03-08 | last_used: 2026-03-09 | concepts: identity, slug -->
<!-- sources: SKILL.md -->
SLUG derivation: last URL path segment → strip .git → lowercase → replace non-alphanum (except - _) with -. BRAIN_ROOT = ~/.patb/{SLUG}.patb/. BRAIN_REPO_URL priority: PATB_URL from @brain → @pinky line 1 → derived {SOURCE_REPO_URL}.patb.

#### PATB_URL overrides brain repo URL
<!-- rating: 550 | created: 2026-03-09 | last_used: 2026-03-09 | concepts: config, brain-repo, identity -->
<!-- sources: SKILL.md, @brain -->
PATB_URL in @brain YAML overrides the derived brain repo URL (useful when brain repo name differs from {source}.patb). Only include PATB_URL in @brain when it differs from the default derivation.

#### Related-notes field for direct note links
<!-- rating: 450 | created: 2026-03-09 | last_used: 2026-03-09 | concepts: related-notes, concept-graph -->
<!-- sources: SKILL.md -->
Notes can include `related-notes: title1, title2` in their metadata comment for direct note-to-note links within the pool. Optional — omit when not applicable. Works alongside concept tags but allows explicit named links.
