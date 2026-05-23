# 16 JavaScript File With Empty Prettier Config Only

Result: does not reproduce.

## Steps

1. Open this folder as a WebStorm project.
2. Open `src/repro.js`.
3. Type `str.if`.
4. Press `Tab`.

## Observed

The generated `if` block is empty. A JavaScript file plus an empty Prettier config is not enough to reproduce the issue without `package.json`.
