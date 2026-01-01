# oxlint-config-standard

Shareable oxlint configuration based on [neostandard](https://github.com/neostandard/neostandard).

## Overview

This package provides an `.oxlintrc.json` configuration file that approximates the neostandard ESLint configuration as closely as possible for oxlint. Since oxlint focuses on linting (not formatting), this configuration includes only the linting rules from neostandard.

## Installation

```bash
npm install --save-dev oxlint-config-standard oxlint
```

## Usage

Create a `.oxlintrc.json` in your project root:

```json
{
  "extends": ["./node_modules/oxlint-config-standard/.oxlintrc.json"]
}
```

> **Note**: Unlike ESLint, oxlint's `extends` field requires an explicit file path (only `.json` format is supported). You cannot use npm package names like `"oxlint-config-standard"` directly.

## What's Included

This configuration maps the following neostandard rule categories to oxlint:

### Core ESLint Rules
- **Correctness rules**: Rules that catch outright wrong or useless code (e.g., `no-debugger`, `no-const-assign`)
- **Best practices**: Rules for idiomatic patterns (e.g., `eqeqeq`, `curly`, `no-eval`)
- **Modernization**: `no-var` set to "warn" (matches neostandard's modernization config)
- **Suspicious patterns**: Rules for likely wrong code (e.g., `no-extend-native`, `no-extra-bind`)

### Plugin Rules
- **Import rules**: Module import/export rules (e.g., `import/first`, `import/no-duplicates`)
- **Node.js rules**: Node-specific rules (e.g., `node/no-new-require`)
- **Promise rules**: Promise best practices (e.g., `promise/param-names`)
- **React/JSX rules**: React and JSX rules (e.g., `react/jsx-key`, `react/no-children-prop`)

## What's NOT Included

### Stylistic/Formatting Rules
Oxlint focuses on linting, not formatting. The following neostandard rules are **not included** because they're formatting-related:

- All `@stylistic/*` rules (indentation, spacing, quotes, semicolons, etc.)
- Use [oxfmt](https://www.npmjs.com/package/oxfmt), [Prettier](https://prettier.io/), or [Biome](https://biomejs.dev/) for formatting

### Unsupported Rules
Some neostandard rules don't have oxlint equivalents:

- `camelcase` - Not available in oxlint
- `object-shorthand` - Not available in oxlint
- `dot-notation` - Available but turned off in neostandard modernization config
- Various Node.js rules like `n/handle-callback-err`, `n/no-callback-literal`, etc.
- Some advanced import-x rules

## Environment & Globals

The configuration sets:
- **Environment**: ES2022 + Node.js
- **Globals**: `document`, `navigator`, `window` (readonly)

This matches neostandard's default environment configuration.

## Differences from neostandard

1. **No formatting rules**: Use a separate formatter for code style
2. **Fewer plugin rules**: Some neostandard plugin rules aren't available in oxlint yet
3. **React rules included by default**: The JSX/React rules are included (you can disable them if not needed)
4. **No TypeScript-specific config**: Oxlint handles TypeScript automatically

## Rule Count

This configuration enables approximately **130 rules**, compared to neostandard's full ruleset. The focus is on correctness, best practices, and catching common errors.

## Testing

Run oxlint with this configuration:

```bash
npx oxlint .
```

## Performance

Oxlint is significantly faster than ESLint (typically 50-100x faster) because it's written in Rust. This makes it ideal for large codebases and CI/CD pipelines.

## Contributing

If you find rules that should be added or adjusted, please open an issue or PR.

## License

MIT
