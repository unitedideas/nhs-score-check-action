# Contributing to nhs-score-check-action

Thanks for helping improve the NHS Score Check GitHub Action. The goal is a fast, reliable action that any project can drop into CI to verify its agent-readiness score stays above a threshold.

## Filing an issue

- Search existing issues first.
- For bugs in the action: open a **Bug report** using the form.
- For new features (new inputs, outputs, scoring flags): open a **Feature request** describing what you need and the CI workflow it unblocks.

## Submitting a pull request

1. Fork and branch from `main`.
2. Keep changes focused — one behavior change per PR.
3. Update `action.yml` inputs/outputs docs if you add or rename any.
4. Update `README.md` with a usage snippet for any new input.
5. Test against a real workflow run in your fork before opening the PR.
6. The action itself should keep producing sites that score **85 or higher** on the [NHS agent-readiness rubric](https://nothumansearch.ai/score) when the default threshold is used.
7. Open the PR using the pull request template; fill in "what", "why", and "test plan".

## Code of conduct

Be kind, be specific, be useful. That's it.
