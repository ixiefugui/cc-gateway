# Repository Guidelines

## Project Structure & Module Organization

CC Gateway is a Node.js reverse proxy written in TypeScript. `src/index.ts` loads configuration, initializes OAuth, and starts the proxy. Request forwarding lives in `src/proxy.ts`; identity and telemetry transformations live in `src/rewriter.ts`. Authentication, OAuth, logging, configuration, and outbound proxy support have separate modules in `src/`.

`src/scripts/` contains token and identity generators; `scripts/` contains shell setup and client-launcher tools. Tests live in `tests/`, branding assets in `.github/`, and compiled output in `dist/`. Use `config.example.yaml` as the configuration reference. Run commands from this directory, alongside `package.json`.

## Build, Test, and Development Commands

Use Node.js 22 or newer.

- `npm ci`: install dependencies from the lockfile.
- `npm run dev`: start the gateway with automatic reload through `tsx watch`.
- `npm run build`: compile strict TypeScript into `dist/`.
- `npm start`: run the compiled gateway; build first.
- `npm test`: execute the rewrite regression suite.
- `npm run generate-identity` and `npm run generate-token`: generate local configuration values.
- `docker compose up --build -d`: build and start the container after configuring `config.yaml`.

## Coding Style & Naming Conventions

Match existing TypeScript: two-space indentation, single quotes, and no semicolons. Use ES module imports with `.js` extensions for local modules and `import type` for types. Use camelCase for functions and variables, PascalCase for types, and lowercase hyphenated filenames where needed. Preserve snake_case configuration keys. No formatter or lint script is currently configured; use the compiler and surrounding code as guides.

## Testing Guidelines

`tests/rewriter.test.ts` uses Node's strict assertions and a custom synchronous test runner, executed by `tsx`. Add descriptive `test('rewrites …', ...)` cases covering changed transformations, preserved fields, and malformed input. Keep fixtures synthetic and tests independent of credentials or network access. New test files require updating the test command to execute them. Run `npm test` and `npm run build` before submitting code. No coverage threshold is configured.

## Commit & Pull Request Guidelines

History uses prefixes such as `feat:`, `fix:`, `docs:`, and `chore:`. Keep commits focused and subjects descriptive. Pull requests should explain the problem, resulting behavior, configuration changes, and validation performed. Link related issues when applicable; include request/response examples for rewriting changes. Update the README and example configuration when behavior or settings change.

## Security & Configuration

Keep OAuth credentials, client tokens, `config.yaml`, generated `clients/`, and TLS keys local; these paths are ignored. Use placeholders in examples and redact sensitive data from logs and pull requests.
