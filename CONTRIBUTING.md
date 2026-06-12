# Contributing to gitbankio/copilot-extension

Thanks for your interest in contributing to the Gitbank GitHub Copilot Extension.

## What is this repo

This is the GitHub Copilot Extension for Gitbank. It is an Express.js agent endpoint that lets users type `@gitbankbot` in GitHub Copilot to interact with their Web3 vault on Base L2.

The full implementation lives in the Gitbank server monorepo. This repo documents the agent protocol and serves as the public face of the extension.

## Good contributions

- Improve the command examples or user-facing text in README
- Document new commands or confirm flow changes
- Report bugs via GitHub Issues (include the Copilot surface you used: github.com/copilot, VS Code, or GitHub Mobile)
- Add integration guides for other Copilot surfaces

## How to report a bug

1. Open an issue with the command you typed and the response you got
2. Include which Copilot surface you were using
3. Mention whether it was a read command (balance, history) or a write command (deposit, swap, etc.)

## Rules

- No em dash in any file
- No Replit or internal tooling mentions
- README changes must reflect the actual behavior of the live endpoint

## License

By contributing, you agree that your contributions will be licensed under the Apache-2.0 License.
