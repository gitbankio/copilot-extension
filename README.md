# gitbank-copilot-extension

GitHub Copilot Extension for Gitbank. Type `@gitbankbot` directly in GitHub Copilot to manage your Web3 vault on Base L2.

Works in github.com/copilot, VS Code Copilot Chat, and GitHub Mobile. No config files. No API keys. Zero setup.

## Install

Go to the [Gitbank Copilot Extension](https://github.com/apps/gitbankbot) on GitHub Marketplace and click Install.

Then open GitHub Copilot and type:

```
@gitbankbot balance
@gitbankbot deposit 100 USDC
@gitbankbot swap 10 USDC to WETH
```

## Commands

### Read (instant, no confirmation needed)

| Command | Description |
|---|---|
| `@gitbankbot balance` | Current WETH and USDC balances in your vault |
| `@gitbankbot history` | Last 5 on-chain transactions with Basescan links |

### Write (queues a confirm code)

| Command | Description |
|---|---|
| `@gitbankbot deposit 100 USDC` | Lock USDC or WETH into your vault |
| `@gitbankbot withdraw 50 USDC to 0x1234...` | Withdraw to any wallet address |
| `@gitbankbot swap 10 USDC to WETH` | Uniswap v3 swap inside the vault, relayer pays gas |
| `@gitbankbot send 20 USDC to @alice` | Send to another GitHub user's vault |
| `@gitbankbot launch token DevFund symbol DEV` | Launch a token on Base mainnet |

Write commands return a confirm code. Open [github.com/gitbankio/playground/discussions/4#new_comment_form](https://github.com/gitbankio/playground/discussions/4#new_comment_form) and post `@gitbankbot confirm <code>` to authorize. Your GitHub account (YubiKey, 2FA, passkey) is the only auth that matters.

## How it works

The extension agent lives at `POST /api/copilot`. GitHub sends the user's token in the `X-GitHub-Token` header. The agent:

1. Calls `GET https://api.github.com/user` with the token to verify identity
2. Parses the command using Claude Haiku
3. Read commands query the vault on Base Mainnet and stream the result
4. Write commands create a pending record in the DB and return a confirm code

The confirm step uses GitHub as the authorization layer. Only a webhook event signed by GitHub's servers from the authenticated user can execute the transaction.

## Copilot Extension agent protocol

The agent receives:

```json
POST /api/copilot
X-GitHub-Token: <user-access-token>
Content-Type: application/json

{
  "messages": [
    { "role": "user", "content": "@gitbankbot deposit 100 USDC" }
  ]
}
```

And streams back OpenAI-compatible SSE:

```
data: {"id":"cplt-...","object":"chat.completion.chunk","choices":[{"delta":{"content":"Deposit queued: 100 USDC"},"index":0}]}
data: [DONE]
```

## Source

The full implementation is part of the Gitbank server monorepo at [gitbankio/server](https://github.com/gitbankio/server).

The agent route is at `api-server/src/routes/copilot.ts`.

## Vault

Gitbank vaults are soul-bound smart contracts on Base Mainnet, anchored to GitHub user IDs. One vault per GitHub account. Deploy yours at [gitbank.io](https://gitbank.io).

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

Apache-2.0. See [LICENSE](LICENSE).
