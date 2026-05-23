# WebStorm Postfix `.if` + Prettier Reproduction Matrix

## Summary

In WebStorm, JavaScript/TypeScript postfix completion using `.if` may generate an unexpected `a` inside the generated `if` block when a project contains both a `package.json` file and a valid Prettier configuration.

YouTrack issue: [WEB-78115](https://youtrack.jetbrains.com/issue/WEB-78115/JS-TS-postfix-.if-completion-inserts-unexpected-a-inside-generated-if-block-when-Prettier-config-is-present)

This repository contains isolated test projects that document the observed boundary conditions.

## Expected Behavior

Typing:

```ts
str.if
```

and pressing `Tab` should generate an empty `if` block with the caret inside:

```ts
if (str) {
}
```

## Actual Behavior

In affected project configurations, WebStorm generates an unexpected `a` inside the block:

```ts
if (str) {
  a
}
```

or, depending on formatting settings:

```ts
if (str) {
  a;
}
```

## Minimal Reproduction

1. Open a project folder in WebStorm.
2. Ensure the project root contains a `package.json`.
3. Ensure the project root contains a valid Prettier configuration file, even an empty `.prettierrc.json` containing only `{}`.
4. Open `src/repro.ts`.
5. Type `str.if`.
6. Press `Tab`.

## Reproduction Matrix

Each numbered folder is an isolated test project.

The folder name ends with:

- `true`: the issue reproduces
- `false`: the issue does not reproduce

| Folder | Purpose | Result |
| --- | --- | --- |
| `01-empty-ts-false` | Empty TypeScript project without package or Prettier config | Does not reproduce |
| `02-package-only-ts-false` | `package.json` only | Does not reproduce |
| `03-prettierrc-empty-only-ts-false` | Empty Prettier config only | Does not reproduce |
| `04-package-prettierrc-empty-ts-true` | `package.json` plus empty `.prettierrc.json` | Reproduces |
| `05-package-prettierrc-rules-ts-true` | `package.json` plus common Prettier rules | Reproduces |
| `06-package-prettierrc-empty-js-true` | JavaScript file with package plus empty Prettier config | Reproduces |
| `07-package-prettierrc-empty-tsx-true` | TSX file with package plus empty Prettier config | Reproduces |
| `08-package-prettierrc-empty-node-modules-ts-true` | Same as the minimal repro with a `node_modules` folder present | Reproduces |
| `09-package-node-modules-no-prettierrc-ts-false` | Package plus `node_modules`, but no Prettier config | Does not reproduce |
| `10-package-json-prettier-object-ts-true` | Prettier config declared inline in `package.json` as an object | Reproduces |
| `11-package-json-prettier-string-path-ts-true` | Prettier config declared in `package.json` as a local path | Reproduces |
| `12-package-json-prettier-empty-object-ts-true` | Empty Prettier config declared inline in `package.json` | Reproduces |
| `13-package-prettierrc-invalid-ts-false` | Invalid Prettier config file | Does not reproduce |
| `14-package-prettierrc-yaml-ts-true` | YAML Prettier config file | Reproduces |
| `15-package-prettierrc-js-ts-true` | JavaScript Prettier config file | Reproduces |
| `16-prettierrc-empty-only-js-false` | JavaScript file plus empty Prettier config, but no `package.json` | Does not reproduce |
| `17-global-prettier-version-matrix-true` | Same minimal repro tested against multiple global Prettier versions | Reproduces |

## Prettier Version Matrix

The latest Prettier version was checked against the npm registry and with `npm view prettier dist-tags.latest version`.

At the time of testing, the latest Prettier version was `3.8.3`, and the last Prettier 2.x version was `2.8.8`.

The version-specific tests are recorded in `17-global-prettier-version-matrix-true`. WebStorm was restarted between global Prettier package changes so the language service would reload the selected package version.

## Test Environment

All tests in this repository were performed with:

- WebStorm: 2026.1.2
- Build: WS-261.24374.125
- Prettier initially reported by WebStorm language services: 3.0.3
- Prettier package source observed in WebStorm: global npm package
- macOS: 15.4.1
- macOS build: 24E263

Results may differ on other IDE or OS versions.
