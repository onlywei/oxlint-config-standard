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

Some neostandard rules don't have oxlint equivalents (~21 linting rules total):

**Core ESLint Rules (13 rules):**
- `camelcase` - Naming convention enforcement
- `object-shorthand` - ES6 object literal shorthand
- `dot-notation` - Available but turned OFF in neostandard modernization config
- `no-dupe-args` - Duplicate function parameters (covered by parser)
- `no-octal` - Octal literals
- `no-octal-escape` - Octal escape sequences
- `no-undef-init` - Initializing to undefined
- `no-unmodified-loop-condition` - Loop conditions that never change
- `no-unreachable-loop` - Unreachable loop detection
- `no-use-before-define` - Variable hoisting issues
- `one-var` - Variable declaration style
- `prefer-const` - Const over let preference
- `prefer-regex-literals` - Regex literal notation

**Node.js Plugin Rules (5 rules):**
- `n/handle-callback-err` - Callback error handling
- `n/no-callback-literal` - Callback literal checks
- `n/no-deprecated-api` - Deprecated Node.js APIs (set to 'warn' in modernization)
- `n/no-path-concat` - Path concatenation with __dirname/__filename
- `n/process-exit-as-throw` - Treat process.exit() as throw

**React Plugin Rules (3 rules):**
- `react/jsx-uses-react` - Not needed in React 17+ with new JSX transform
- `react/jsx-uses-vars` - Variable usage tracking in JSX
- `react/no-deprecated` - Deprecated React APIs

**Import Plugin Rules:**
- `import-x/export` - Export validation
- Some other advanced import-x rules

**Note:** The above count excludes 60+ `@stylistic/*` formatting rules which are intentionally not supported.

## Environment & Globals

The configuration sets:
- **Environment**: ES2022 + Node.js
- **Globals**: `document`, `navigator`, `window` (readonly)

This matches neostandard's default environment configuration.

**Note**: Like neostandard, this configuration includes **both** Node.js and browser globals in a single config. This means:
- Browser globals (`document`, `navigator`, `window`) are available even in Node.js code
- Node.js rules are active even in browser code

This is the same approach used by neostandard (which includes a `// TODO: Should only be active for server side scripts` comment). While not ideal for strictly typed environments, it works for most isomorphic JavaScript projects.

Future versions may include separate configs for Node.js-only, browser-only, and isomorphic SSR code.

## Differences from neostandard

1. **No formatting rules**: Use a separate formatter for code style
2. **Fewer plugin rules**: Some neostandard plugin rules aren't available in oxlint yet
3. **React rules included by default**: The JSX/React rules are included (you can disable them if not needed)
4. **No TypeScript-specific config**: Oxlint handles TypeScript automatically

## Rule Count

This configuration enables **132 rules** total.

**Neostandard's linting rules (excluding formatting):**
- 110 base rules (from `base.js`)
- 21 React rules (from `jsx.js`)
- **Total: 131 linting rules**

**Our coverage:**
- We implement **110 of neostandard's 131 linting rules** (84% coverage)
- We're missing **21 unsupported rules** (listed above)
- We have **132 total rules** because setting `"categories": { "correctness": "error" }` enables 22 additional correctness rules from oxlint that aren't explicitly in neostandard's config

**Result:** This config is actually **stricter** than neostandard in some areas (more correctness rules), while missing some rules due to oxlint limitations.

**Excluded from count:** Neostandard also has ~74 formatting rules (`@stylistic/*`) which are intentionally not included since oxlint focuses on linting, not formatting.

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
