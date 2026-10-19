# Non-recipe generator card sends `power.euPerTick: undefined`; live service rejects the edit

## Bug description
When a generator (power source) is added to a synced plan board via the picker's **"use non-recipe"** option, the client emits an edit op whose `power.euPerTick` field is `undefined`. The live service validates the field as a number and rejects the entire edit with `bad_op`, so the card is never created and the board shows the toast *"An edit could not be saved"*.

## Environment
- GTNH Planner: 0.2.0-beta.2
- GTNH modpack: 2.9.0-RC-2
- Java 25
- Signed in to a cloud account; board is synced

## Steps to reproduce
1. Open a plan board that is synced to the account.
2. Open the power picker and select any generator (e.g. *Large Naquadah Reactor*).
3. Choose **"use non-recipe"** instead of the regular *Add to the plan*.
4. Observe: the toast "An edit could not be saved" appears and no card appears on the board.

## Expected behavior
A non-recipe generator card is created. Since it has no recipe to derive power from, the client should send a sensible default (`0`, or the dataset's nominal value), or omit the field so the server can apply a default.

## Actual behavior
The emitted op contains `power.euPerTick: undefined` and the server rejects it:

```
[21:28:37] [Client thread/WARN] [gtnhplanner/]: The live service refused an edit to
110e22c9-b9e8-41da-bd44-aaecf5db61b0:8f741bdd-3491-429b-bb6b-828759c22ba2
(bad_op: recipes gtnhplanner:b7b6e5b9-09b2-4a36-827b-f5ab73c4129b:
power.euPerTick: Invalid input: expected number, received undefined)
```

## Analysis
The regular *Add to the plan* flow populates `power.euPerTick` from the power dataset, but the non-recipe path builds the card without initializing the power object, so the field is serialized as `undefined`.

## Workaround
Add generators through the regular **Add to the plan** flow in the power picker; that path works correctly.

## Suggested fix
Initialize `power.euPerTick` (e.g. to `0`) when constructing a non-recipe generator card, or skip the field when it has no value instead of sending `undefined`.
