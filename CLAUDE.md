# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
# Install dependencies
npm install

# Lint (add eslint when a src/ directory exists)
npx eslint . --ext .js,.ts,.jsx,.tsx --fix

# Format
npx prettier --write .

# Type-check (when TypeScript is added)
npx tsc --noEmit

# Run tests
npm test

# Run a single test file
npx jest path/to/file.test.ts
```

## Linter Checks

After every code change, run:
```bash
npx eslint . --ext .js,.ts --fix && npx prettier --write .
```

Fix all lint errors before considering a task complete. Do not suppress rules with `// eslint-disable` without a specific documented reason.

## Project Structure

This is currently a Node.js workspace used for generating document artifacts (HTML, DOCX). The main artifact lives in `claude-code-tutorial/index.html` — a self-contained single-page tutorial rendered in the Claude Desktop preview panel.

- `claude-code-tutorial/index.html` — standalone HTML artifact, no build step required
- `package.json` — Node.js dependencies (currently `docx` for DOCX generation)

## Artifact Conventions

- HTML artifacts must be fully self-contained (no external CDN dependencies, all CSS/JS inline)
- The preview panel renders `claude-code-tutorial/index.html` directly — no server needed
- DOCX files are generated via one-off Node.js scripts using the `docx` npm package, then the script is deleted
