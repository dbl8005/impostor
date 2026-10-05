# Impostor

"Among Us" for code review: an AI agent secretly plants a bug in a real diff, and the player has to catch it before merging.

Full context, decisions, and first tasks: @HANDOFF.md

## Rules
- Status: design phase. No code yet. Start by confirming MVP scope (solo local mode) with the user, then write `docs/SPEC.md`.
- Stack: TypeScript Node CLI (`npx impostor`), Claude Agent SDK for the saboteur. MIT license. Free/open-source dependencies only.
- Never modify the user's working branch. Every round runs in a throwaway git worktree.
- Every planted bug needs a proof test that fails because of the bug. Reject rounds without one.
- The user has ADHD. Lead with the next action, use numbered steps, give concrete time estimates, and keep it short.
