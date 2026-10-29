# Balance management — clickable prototype

Clickable HTML prototype of the **Balance management** section of Merchant's Office (Flexify), built from the Figma page "dealing (in progress)":
https://www.figma.com/design/86FZlAuyjByKedeQXnYZyO/Merchant-s-Office?node-id=3512-56594

This is a prototype for the team and stakeholders, not production code.

## How to open

Open `index.html` in a browser. No build, no server, no dependencies (DM Sans loads from Google Fonts; without network a system font is used). State lives in memory: a page reload resets everything.

## What's inside

- One self-contained HTML file, vanilla JS.
- Colors, radii and font come from Figma variables and are declared as CSS variables in `:root`.
- Mock data is the `D` / `S` objects at the top of the script: balances (crypto USDT/USDC, Settlements by project, Payouts EUR/USD/GBP), requests `TR-2026-0xx`, saved wallets, notifications.
- Request statuses are a simple state machine: Submitted → In progress → (Needs reply / Awaiting your decision) → Done / Sent / Credited, or Rejected / Cancelled.
- Available = balance − reserved. The amount is reserved on submit and released on reject/cancel.

## Demo panel

The **Demo** button (bottom right) opens a panel that plays the finance team's side:

- pick an active request, then: Take in work, Ask a question, New terms, Wallet check (start / RED), Execute (or Credit for Add funds);
- Reject: with a public reason, AML (general text + Contact support), auto-reject by deadline;
- events: incoming Add funds, FA1 bank transfer (Soon);
- states: next submit fails (T2a/W2a), grid failed to load (G3), page loading, balances outdated, zero balance, no saved wallets (MW5), no notifications (NT2).

## Frames covered

Transfer T0–T4, FT1–FT2, FP1 · Withdraw W0–W5, W1e (crypto only) · Add funds B1, B1a, FA1 · Details D1–D3, C1–C2, S1–S3, RJ1–RJ3, MS1 · Actions R1, N1, N1a, C1a · Export E0–E2 · Filter F1–F3 · Grid states G1–G3 · My wallets MW1–MW5, AD1–AD2 · Home NT1, NT1a, NT2 · zero / loading states.

## Changes in this version

- "Transfer between balances" is now "Transfer".
- Withdraw only from a crypto wallet (USDT · TRON, USDC · Ethereum), no conversion. Fiat withdraw (FW1, FW1a, FW2, D4) removed; TR-2026-015 is now a USDT withdrawal.
- Account menu shows only allowed actions: Settlements — Transfer, Account details, Remove from Home; Payouts — Add funds (Soon), Transfer, Account details, Remove from Home; crypto — Add funds, Transfer, Withdraw, Account details, Remove from Home. Zero available balance hides Transfer and Withdraw.
- "Needs your action" is a one-line bar with request chips (ID, action, time left); a chip opens the request details, "Show in table" filters the grid. Same bar on Home with "Open all".
- Loading is a full-screen blurred overlay with a spinner (as in Pay by link).
- Purpose and supporting documents are asked ONLY in Withdraw from a Settlements account (FW1–FW2): purpose chips + upload (PDF, JPG or PNG, up to 10 MB, up to 5 files); Continue needs at least one document. Transfer and crypto Withdraw ask for neither.
- Withdraw is in the Settlements card menu (in the design, not in the first release).
- "On hold / Nothing on hold" on cards is plain text without a pill; "Nothing on hold" is muted.
- "Needs your action" uses the design-system Warning alert style; chips use variant A · Outline (chosen). The Demo panel also switches outline variants B–D and filled variants E–H — same as the Figma frames "VA · Needs your action — chip variants" and "… — filled chip variants".

## Not from the mockups (invented or simplified)

- Texts not found in Figma: "Balances outdated" banner; "Not enough available funds" banner for crypto sources; "Not an Ethereum address…"; reason why a wallet can't be deleted; public reject reasons other than "Required documents were not provided"; finance question for transfers; "No decision on the new terms by …"; notification titles "Transfer submitted / done", "Request cancelled"; toasts after accept terms / cancel / documents; "This wallet is already saved as …"; export file name for a period.
- Mock data conflicts: holds are computed from requests, so USDT and USD Settlements show "On hold" (main frame shows "Nothing on hold", AD1 shows 5 000.00 on hold). Kraken USDC's full address was changed because in Figma it equals the cabinet's own USDC deposit address. TRON addresses of 33 or 34 characters are accepted (the B1 address has 33).
- Loading overlay text is "Loading… / Please don’t close this window" (the mockup reuses "Processing refund…" from Pay by link).
- At 1440 px the third chip in "Needs your action" scrolls inside the bar; at 1920 px all three fit.
- Simplified: QR is decorative; Export and Deal / Payment confirmation show a toast without a real file; Amount sort ignores currency; Home widgets are simplified; sidebar is 320 px instead of 352 px.

## Open questions from Notes for dev that shaped the prototype

1. Settlements is never a destination.
2. Payouts → crypto wallet is allowed: it is the only way to withdraw fiat (Payouts → crypto wallet → Withdraw). Swap between a crypto wallet and Payouts is active.
3. Transfers with conversion are executed by finance (via Demo).
4. A wallet that didn't pass AML stays in the list, marked and disabled in Withdraw.
5. Purpose is required and preselected for all operations.
6. Deadlines: as in the mockups for seeded requests, 72 h for new ones.
7. The merchant can only reply to a finance question, not write first.
8. Cancel is possible only before the rate is fixed (crypto withdraw, transfer without conversion).
9. "Not available yet" release dates are shown only for EUR Settlements (no data for USD/GBP).
