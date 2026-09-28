# Refactoring

Use when structure should change while behavior stays the same. A requested behavior change belongs to Feature; a defect correction belongs to Bug fix.

1. Identify the behavior contract and all affected callers. Before editing, choose a useful pin: an existing test, a characterization test, a fixture replay, or an observable baseline. Typecheck and lint alone do not establish behavioral equivalence.
2. Describe the target structure and why it will be easier to maintain. Keep the boundary of the refactor explicit. If evidence shows the work also needs a behavior change, separate or re-scope that change rather than hiding it here.
3. Move in coherent steps, checking the pinned behavior after each meaningful step. Remove replaced code and migrate callers in scope. Preserve unrelated user changes. No routine reset, broad cleanup, or compatibility layer that the actual callers do not need.
4. Verify the resulting behavior on the relevant surface when possible. Check tests, typecheck, lint, and any affected build. Inspect the diff for silent changes to error handling, ordering, data shape, or public contracts.
5. Report the old and new structure, the equivalence evidence, any unverified path, and why the change is worth its review cost. Use [Opening a PR](opening-a-pr.md) only when the project workflow or user request calls for one.

For large, multi-session refactors, keep a decision trail with `show-me-your-work` if installed.
