## HealthTaki

<!--
# HealthTaki

Escrowed payments for medical personnel, on Stellar.

A patient locks funds for a named provider at the time of booking. The provider claims them once the service is delivered. If the provider never claims, the patient takes the money back after an agreed deadline. No company account sits in the middle, and no one at HealthTaki can move a single unit of either party's money.

## why escrow

Plain wallet-to-wallet payment is fine when both sides already trust each other. Medical care is often the first meeting between two strangers, and money and service do not change hands at the same moment.

Pay-up-front asks the patient to trust a provider they have not met. Pay-after asks the provider to deliver on the hope of getting paid. Both are common, and both put the risk entirely on one side.

Escrow moves the risk into a contract. The patient's funds leave their wallet at booking, so the provider knows the money is real. The funds do not reach the provider until the service is done, so the patient keeps recourse. A time-based refund window means neither party can strand the other indefinitely.

## what is not in the middle

There is no admin key on the escrow contract. There is no arbiter, no multisig held by us, and no pause function. The contract moves funds in exactly two directions: forward to the provider when the provider authorizes it, or back to the patient once the refund window opens.

This is a deliberate constraint rather than a stage of development. It also means we cannot help you recover a payment sent to the wrong address, and we cannot reverse a release. Read the contract before you trust it with money.

## the repositories

| Repository | Language | What it owns |
|---|---|---|
| `healthTaki-contract` | Rust, Soroban | Custody. Create, release, refund, and the events an indexer reads. |
| `healthtaki-backend` | Rust, Axum, Postgres | Provider identity and profiles. Never touches funds. |
| `healthtaki-frontend` | Next.js, TypeScript | Wallet connection, balances, payment history, and all signing. |

The three are independent programs. Nothing imports anything, and the only value that crosses every boundary is a Stellar public key.

Authority flows one way. The frontend authorizes, the contract enforces, the backend observes. The backend can mark an invoice paid because the chain says so. It can never cause a payment to move.

## identity without passwords

A provider's identity is their Stellar wallet. The backend issues a one-time nonce, Freighter signs it under SEP-53, and the server verifies that signature against the claimed public key before issuing a session token.

There is no password database and no credential stuffing surface. The same key a provider already uses to receive money is what proves who they are.

Provider verification stays off-chain, in the backend, because a profile is mutable and a contract is not. The escrow contract itself will send funds to any address it is given. It has no opinion about who is a clinician.

## current state

Honest version: the contract is written and tested, the backend auth flow works, the frontend reads balances and incoming payments, and the three are not yet wired to each other.

What exists today is a provider dashboard that can receive classic Stellar payments, an identity service with no caller, and an escrow contract with no UI. There is no patient-facing surface at all, which is the largest single gap, since the patient is the party who funds and refunds every payment.

Per-repo READMEs list their own next steps. The order we are working in is auth client, then invoicing, then the payment indexer, then the patient surface and escrow UI.

## a note on the memo field

Each escrow payment carries a free-form memo intended to hold an invoice or appointment reference. That memo is written to a public ledger and it is permanent.

Keep it opaque. An identifier that means nothing without the backend database is the only safe thing to put there. Anything descriptive becomes a permanent public record connecting two wallets to a medical service.

## contributing

Issues are labelled by repository, by difficulty, and by what they block. Anything marked `needs-design` is an open question and should be discussed in the thread before code is written.

Contract changes need a test for every new error branch, without exception. Frontend work that does not require Stellar knowledge is labelled as such, so React contributors can find it.

## networks

Testnet only. Do not point this at mainnet yet.
**Here are some ideas to get you started:**

🙋‍♀️ A short introduction - what is your organization all about?
🌈 Contribution guidelines - how can the community get involved?
👩‍💻 Useful resources - where can the community find your docs? Is there anything else the community should know?
🍿 Fun facts - what does your team eat for breakfast?
🧙 Remember, you can do mighty things with the power of [Markdown](https://docs.github.com/github/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)
-->
