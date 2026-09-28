# Investigation

Use for a question whose deliverable is an answer, diagnosis, or recommendation. This playbook does not authorize a code change, tracker update, or PR.

1. State the question and what evidence would settle it. Inspect the relevant implementation, runtime evidence, history, or primary documentation. Run read-only checks when they can test a claim.
2. Separate observations from inferences. Test competing explanations for uncertain behavior. If evidence is unavailable, name the missing observation instead of filling it with a plausible story.
3. Answer at the user's requested depth. Cite files, logs, tests, or documentation that support material claims. For a decision, compare the actual alternatives and make a recommendation with its tradeoff.
4. Stop at the answer. If the user then requests a change, route that work to Bug fix, Feature, or Refactoring. Do not treat an investigation as implicit permission to implement its recommendation.

Use a table only when several alternatives or repeated mappings are easier to compare that way.
