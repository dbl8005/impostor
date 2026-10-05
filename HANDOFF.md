# Handoff: Impostor

You're starting a new open-source project from scratch. This file is the full context — the previous session's research and decisions are summarized here.

## One-liner

**Impostor is "Among Us" for code review: an AI agent secretly plants a bug in a real diff, and you have to catch it before you merge.**

## Why this project

- Goal: a fun, viral, simple open-source project for people who build with AI agents.
- Real pain underneath the fun: "comprehension debt" — people ship AI-written code they never actually read. Impostor makes reading AI diffs a game and trains the skill.
- Checked in Oct 2026: agent pets, "Wrapped" recaps, achievements/XP, TTS narrators, Claude-vs-Codex races, prompt golf, and Twitch Plays Claude all exist already. No "planted bug in your own repo" game was found. The closest is research (`7vik/AmongUs`: LLMs playing social deception), which isn't about code.

## Core loop (one round)

1. `impostor deal`: pick a real recent change or a function from the user's repo and copy it into a **throwaway git worktree**. Never touch the user's branch.
2. **Saboteur agent** plants exactly ONE subtle bug (off-by-one, flipped condition, swallowed error, removed auth check, timezone slip, wrong default). It wraps the bug in a believable refactor so the diff isn't a lonely one-liner.
3. **Fairness rule (important):** the saboteur must also write a hidden test that fails *because of* the bug, which proves the bug is real. The existing test suite should still pass, so the bug isn't trivially caught.
4. Player reviews the diff (terminal TUI first, browser later), flags a line + gives a short guess, with a timer running.
5. **Reveal:** bug location, why it matters, the proof test, the score.

## Modes (build in this order)

1. **Solo, local:** you vs. the saboteur on your own repo. ← MVP
2. **Spectator:** saboteur agent vs. reviewer agent; the user watches and optionally bets.
3. **Daily (the viral part):** one pre-generated puzzle per day on a well-known OSS repo, played as a static website. **No API key, no install.** Ends with a Wordle-style share card: `Impostor #42 🟥🟩🟩 caught in 1:32`.

## Tech decisions (proposed; confirm with the user if you deviate)

- TypeScript, Node CLI, published to npm (`npx impostor`).
- Saboteur via Claude Agent SDK (default model: latest Claude). Codex CLI as an optional engine later.
- Bug-category catalog with difficulty levels (easy / sneaky / evil).
- Store the round manifest (bug location + explanation) hashed or encrypted until reveal, so the player can't peek.
- License: MIT. The user prefers free/open-source tooling only, so no paid dependencies.

## Hard problems to think about early

- Bugs must be subtle but **fair**. The proof test is the guardrail. Reject rounds where the proof test doesn't fail or the existing suite breaks.
- Picking good target code: functions with logic, not boilerplate.
- Language support: start with JS/TS repos only.
- Cost per round: keep the saboteur prompt tight and show the token cost per round.

## First tasks for this session

1. Use the brainstorming / spec skill to confirm scope with the user (MVP = solo local mode).
2. Write `docs/SPEC.md`: CLI commands, round lifecycle, scoring, file layout.
3. Scaffold the TS CLI (`impostor deal`, `impostor guess`, `impostor reveal`).
4. Build a hand-written fake round first (no AI), to get the review UX right before wiring in the saboteur.

## Sibling project

`~/dev/repos-and-dungeons`: a roguelike visualizer of agent sessions. It could later become Impostor's spectator view. Keep the two projects separate for now.

## User notes

- The user has ADHD. Lead with the next action, use numbered steps, give concrete time estimates, and keep it short.
- GitHub: `dbl8005/impostor` (public).
