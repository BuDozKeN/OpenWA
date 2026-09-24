# OpenWA (BuDozKeN fork) — agent guide

Fork of rmyndharis/OpenWA. `main` mirrors upstream; our work lives on `dasupport/main` (the Da_Support gateway,
images built on `dasupport-*` tags). Branch units from `dasupport/main`; upstream contributions branch from `main`
and must not carry this file or `.agents/`. Repo profile: `.agents/profile.yaml`.

<!-- BEGIN:dev-framework (generated from ~/.claude/policy — run sync-framework.py; never edit here) -->
<!-- policy-stamp version=0.20.1 sha256=b6384120d5dc427d774e45a1214ae19ead23fb123b0c227d7c808a9410e5691a -->
## Multi-agent development framework — core rules (generated; do not edit here)

Canonical: `~/.claude/policy/policy.yaml` + `dev-framework.md` v0.20.1. **Load the full framework
(the `dev-framework` skill) before:** coordinating an Orca Run, starting any T2/T3 change, choosing a model,
or changing these rules.

1. **Autonomous preparation, human release.** Stop for the owner only at: G1 Batched executive summary; owner approves merge; G2 Summary + parity check + rollback path; owner approves deploy;
   G3 Owner explicitly accepts a T2/T3 merge without a required veto, or overrides a veto; G4 Destructive or outward-facing: prod data, real-person messages, deletions, spend, credentials. Everything else is decided, recorded, and shown in the next G1 summary.
2. **One change unit = one worktree = one branch = one PR = one writer.** Others read and review, never edit.
   Branch `<type>/<issue#>-<slug>`, set at creation, never renamed. Max 3 open units per
   Run, 5 overall.
3. **Two tab levels.** `LEAD · <unit> · <model>` — one per unit, the owner's tab, kept until the owner closes it.
   `↳ <Role> · <model> · <unit> → reports to LEAD` — helpers, released after an accepted `worker_done`.
   Register every dispatch with `python ~/.claude/policy/register_role.py` (validates against live Orca; never edit the
   registry by hand). Titles are for humans; route by Dispatch ID.
4. **Tier floor is computed** (`tier_floor.py`) and may be raised, never lowered. T1: builder + scripted gates.
   T2: + cross-provider review. T3: design veto first, then review + security.
5. **Models by rung, never by habit.** mechanical claude-haiku-4-5-20251001 / codex -p mech;
   bounded claude-sonnet-5 / codex -p build; open-ended claude-opus-5-5 /
   codex -p deep; hardest claude-fable-5-1 / codex -p max (reserve,
   never fanned out). Default: Codex implements, Claude reviews; vetoes are never lowered.
6. **Degraded mode:** if a provider is out, independence may degrade, gates never; T2/T3 without a
   cross-provider veto queue or go to G3.
7. **Evidence, not claims.** Gates run in an ephemeral no-credential container; only the broker posts
   `gates/policy`. Done = every acceptance item backed by evidence bound to the head SHA, else "unverified".
8. **An unresolved veto queues the unit — it never ships.** Absence (timeout, silence, unverifiable) authorises
   nothing.
9. **Subagents never touch git state** (no stash/reset/checkout/restore/clean/add/commit/switch) and never
   write helper scripts into the repo. No deletions: write to fresh paths.
10. **Every unit has a brief** (outside git): what & why, success, how, who's in charge, now/next, needs you?
11. **A repo profile may add, tailor or mark n/a with a reason — never remove a safeguard.** Rule changes are
    sized by `change_class.py` (§31): class 2 needs Claude and Codex to agree (uncapped); class 1 gets max 2 rounds,
    then the owner decides; both then go to G1.
12. **Prompts:** one message, one finish line; no "think carefully" lines; ask for evidence, not reasoning.
    Reviewers: "List only problems you'd block the merge for. For each one, give the file and line."
    Summaries open with "Waiting on you". Veto workers never auto-switch models on a flag. Unattended
    workers get `generated/worker-template.md`; sessions with the owner present do NOT.
13. **Orca is the substrate.** Run `orca skills get orca-cli` (+ `orchestration` when coordinating) before the first
    orca command of a session. Place units only with `unit_launch.py` (R1 → create with --setup skip, no agent → launcher-run setup + proof
    → host_preflight.py → worker-start → launch_check.py). Status lives in the worktree comment; G1/G2 summaries stay
    local/private, never public Orca artifacts. Every unit closes with a retro whose findings are each routed to a
    guard, rule, skill, profile or memory — unrouted findings block G1.
<!-- END:dev-framework -->
