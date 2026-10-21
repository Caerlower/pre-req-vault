# Pre-Req Vault — Architecture

## System

```mermaid
flowchart LR
  User([User wallet]) --> VaultProg[Vault program]

  VaultProg --> VS[(vault_state PDA)]
  VaultProg --> V[(vault PDA)]

  VaultProg -.->|CPI on withdraw| Reg[Registration program]
  Reg --> App[(prereqs PDA)]
```

## Accounts

| Account | Seeds | Holds |
|---------|--------|--------|
| `vault_state` | `["state", user]` | bumps |
| `vault` | `["vault", vault_state]` | SOL |
| `application_account` | `["prereqs", user]` under registration | GitHub username |

## Instruction flow

```mermaid
sequenceDiagram
  actor User
  participant Vault as Vault program
  participant VS as vault_state
  participant V as vault
  participant Reg as Registration program

  User->>Vault: initialize
  Vault->>VS: create + store bumps

  User->>Vault: deposit(amount)
  Vault->>V: SOL in (user signs)

  User->>Vault: withdraw(amount)
  Vault->>V: SOL out (vault PDA signs)
  Vault->>Reg: CPI initialize(github)
  Reg-->>Reg: write prereqs PDA

  User->>Vault: close
  Vault->>V: drain remaining SOL
  Vault->>VS: close account
```

## Notes

- Programs hold no state; accounts do.
- Vault PDA has no private key — withdraw/close use program signer seeds.
- Registration CPI runs only after a successful withdraw transfer.
