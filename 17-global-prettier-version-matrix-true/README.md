# 17 Global Prettier Version Matrix

Result: reproduces across all tested Prettier versions.

## Purpose

This test keeps the same minimal reproduction shape and changes only the global Prettier package version selected by WebStorm.

The minimal reproduction shape is:

- `package.json`
- `.prettierrc.json`
- `src/repro.ts`

## Steps

1. Install a specific global Prettier version.
2. Restart WebStorm so the language service reloads the selected Prettier package.
3. Open this folder as a WebStorm project.
4. Open `src/repro.ts`.
5. Type `str.if`.
6. Press `Tab`.

## Observed Results

| Global Prettier version | How it was selected | WebStorm status bar | Result |
| --- | --- | --- | --- |
| `3.8.3` | `npm install -g prettier@latest` | `Prettier 3.8.3` | Reproduces |
| `3.0.0` | `npm install -g prettier@3.0.0` | `Prettier 3.0.0` | Reproduces |
| `2.8.8` | `npm install -g prettier@2.8.8` | `Prettier 2.8.8` | Reproduces |

## Notes

- At the time of testing, npm reported `3.8.3` as the latest Prettier version.
- At the time of testing, `2.8.8` was the last Prettier 2.x version.
- WebStorm did not always switch the displayed Prettier version immediately after changing the global package. Restarting WebStorm made the status bar reflect the newly installed version.
- The issue is therefore not isolated to a single Prettier major version in this environment.

## Observed

The generated `if` block contains an unexpected `a`.
