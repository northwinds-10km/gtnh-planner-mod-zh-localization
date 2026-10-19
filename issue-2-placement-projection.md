# Placement keys: multi-place shows only one ghost projection

## Bug description
When placing **multiple identical machines** into the world with the placement keys (from the board), only a single ghost projection is rendered — for the first (or one) machine only. The remaining machines have no visual guidance at all, so the player cannot see where the rest will land and has to place them from memory, which easily causes misalignment. This makes multi-machine placement impractical for line planning.

## Environment
- GTNH Planner: 0.2.0-beta.2
- GTNH modpack: 2.9.0-RC-2
- Java 25

## Steps to reproduce
1. On a plan board, select several machines of the same type (e.g. a row of identical machines).
2. Use the placement keys to place them into the world in one go.
3. Observe: only one ghost projection appears in the world.

## Expected behavior
Each pending machine gets its own ghost projection (with its position offset and facing), so the player can verify the whole layout before/while placing. Ideally placements are confirmed one at a time: after each confirmation the projection advances to the next machine (carrying offset/rotation), until all are placed; placement can be cancelled at any time with already-placed machines kept.

## Actual behavior
One projection only. The other machines are placed "blind".

## Suggested behavior (open to discussion)
1. Confirm/rotate/cancel key assignment for the sequential flow.
2. Whether an early cancel keeps the unplaced machines in a resumable "pending placement" state.
3. The order in which projections advance (board card order vs. world position order).
