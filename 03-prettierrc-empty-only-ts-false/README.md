# 03 Empty Prettier Config Only

Result: does not reproduce.

## Steps

1. Open this folder as a WebStorm project.
2. Open `src/repro.ts`.
3. Type `str.if`.
4. Press `Tab`.

## Observed

The generated `if` block is empty. An empty Prettier config without `package.json` is not enough to reproduce the issue.
