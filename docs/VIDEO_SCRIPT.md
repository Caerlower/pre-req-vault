# Part 1 video script (≤ 3 minutes)

Record with this diagram open: `docs/vault-architecture.jpg`  
Speak clearly. YouTube: upload → enable captions (auto is fine) → Unlisted or Public.

---

## 0:00 – 0:20 | Hook + problem

> Hi, I'm walking through my Turbin3 prerequisite Vault program.
>
> The goal is a personal SOL vault on Solana: deposit and withdraw funds safely.
> On withdraw, the vault also registers my GitHub username with Turbin3's
> registration program using a Cross-Program Invocation — a CPI.

---

## 0:20 – 0:50 | Big picture

> Solana programs are stateless. All state lives in accounts that you pass in.
>
> On the left is the user wallet — the signer.
> In the middle is the Vault program — the code I deployed.
> On the right are the accounts that hold state: vault_state and the vault that holds SOL.
>
> Separately, there's the registration program. My vault calls into it on withdraw.

*(Point at each box on the diagram as you name it.)*

---

## 0:50 – 1:30 | Accounts + PDAs

> There are two main PDAs — Program Derived Addresses.
>
> vault_state is derived from seeds `state` plus the user pubkey. It stores two bumps.
> The vault PDA is derived from seeds `vault` plus the vault_state address. That's where SOL sits.
>
> PDAs have no private key. The program can sign for them using seeds, which is how
> we move SOL out of the vault on withdraw and close without a keypair for the vault itself.

---

## 1:30 – 2:20 | Instruction flow

> The lifecycle is four instructions.
>
> One — Initialize: creates vault_state and remembers the bumps.
> Two — Deposit: the user transfers SOL into the vault. The user signs that transfer.
> Three — Withdraw: the vault PDA signs a transfer back to the user for the amount.
>          Then my program does a CPI to registration's initialize, passing my GitHub username.
>          That creates the prereqs account under the registration program.
> Four — Close: empty the vault, close vault_state, return rent to the user.

*(Trace the arrows: init → deposit → withdraw → CPI → close.)*

---

## 2:20 – 2:50 | Why the CPI matters

> The CPI is the verification step. After a successful withdraw, registration records
> my GitHub on-chain. That proves the call went through my deployed vault program,
> not a direct call to registration alone.
>
> One registration per wallet — so withdraw with CPI is the path that matters.

---

## 2:50 – 3:00 | Close

> That's the architecture: user, vault program, PDAs for state and SOL,
> and a CPI from withdraw into registration. Thanks for watching.

---

## Recording tips

- Keep total under 3:00; aim for ~2:30.
- Zoom the diagram so labels are readable on camera.
- If you stumble, pause and restart that section — no need for perfect production.
- After upload: YouTube Studio → Subtitles → Auto-generate (or upload this script).
