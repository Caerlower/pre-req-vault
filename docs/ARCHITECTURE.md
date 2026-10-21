# Pre-Req Vault — Architecture

Diagram image: [`vault-architecture.jpg`](./vault-architecture.jpg)

## Overview

Personal SOL vault built with Anchor. Programs are code; accounts hold state.

| Piece | Role |
|-------|------|
| **User wallet** | Signer; owns the vault |
| **Vault program** | `initialize`, `deposit`, `withdraw`, `close` |
| **`vault_state` PDA** | Seeds `["state", user]` — stores vault + state bumps |
| **`vault` PDA** | Seeds `["vault", vault_state]` — holds SOL |
| **Registration program** | External; `initialize(github)` records GitHub username |
| **`application_account` PDA** | Seeds `["prereqs", user]` under registration program |

## Flow

1. **Initialize** — create `vault_state`, save bumps  
2. **Deposit** — user → vault (user signs)  
3. **Withdraw** — vault → user (vault PDA signs) **+ CPI** → registration `initialize` with GitHub  
4. **Close** — drain vault, close `vault_state`, rent back to user  

## Why CPI on withdraw

Turbin3 verifies registration happened through *your* deployed vault's withdraw path, not a standalone client call to the registration program.
