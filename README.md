# PharmaPilot AI

AI-powered copilot for pharmaceutical batch documentation, deviation management, and human error investigation.

**Try it:** open `index.html` in a browser. No build, no dependencies, no backend.

## Why this exists

Anyone who has run an inspection line knows the moment: the batch is running, syringes are flowing through the machine, and then you realize the AQL tab was never scanned. Or the eject counts don't match the trays. Or the final reconciliation is 18 units short and the shift ends in twenty minutes.

None of these are big problems. But each one means stopping, working out what state the system is in, fixing it manually, and writing it up — while the line keeps running.

BatchPilot explores a simple idea: **what if an assistant watched the batch with you and handled the small stuff?** Not replacing the execution system, not making decisions for you — just cutting the time between "something's off" and "it's fixed and documented."

## The process it models

The demo simulates a visual inspection batch of prefilled syringes, start to finish:

1. Batch takeover from filling (24,000 units received)
2. Material verification — Ringcode reel, label reels, format parts
3. Machine setup — recipe and eject mode
4. Automated inspection — good units are labelled; defects are ejected into three streams: physical defects and particle/liquid defects go to manual re-inspection, uncertain units go to 100% manual control (rails 4–6)
5. Muster & AQL sampling, prompted by the system, always taken from the good stream
6.–8. Re-inspection, 100% control, label verification
9. Quantity reconciliation: received = good + ejects + samples + machine breakage
10. Batch record review

A **live reconciliation panel** tracks every category and shows the balance (Δ) at all times — because in the end, the numbers have to close.

## What the assistant can do

Five situations are built in, all taken from real operator life:

- **Missed AQL tab scan.** The units left the good stream but were never booked, so the balance shows Δ −125. The original tab is glued into the batch record — so the assistant issues a replacement tab with a **real, scannable Code 128 barcode** rendered on screen. Scan it, the booking lands, the balance closes.
- **Ejects booked to the wrong category.** 60 physical defects booked as particle — the assistant explains why this is a rebooking rather than a deviation, and corrects it.
- **Forgotten eject mode.** A rework run started in normal mode; the assistant walks through the safe way back and documents it with timestamps.
- **Wrong material at the machine.** A rejected reel scan — the assistant points to the correct lot and notes what happened in one sentence.
- **Final count doesn't balance.** The assistant works through the categories, finds the unbooked machine breakage, and closes the reconciliation. And if the AQL issue is still open, it's smart enough to say "part of this delta is that — let's fix it first."

In every case the assistant **drafts** documentation — the operator confirms it. It reads system state; it never writes around it.

## Design principles

- The execution system stays the single source of truth. The assistant is a layer next to it, not inside it.
- Every action is proposed, never executed silently. The operator confirms.
- Documentation is drafted at the moment the issue happens, not reconstructed at the end of the shift.
- The quantity balance is always visible — reconciliation shouldn't be a surprise at batch end.

## Tech

Single self-contained HTML file. Vanilla JS, no framework, no external libraries. The barcode is a from-scratch Code 128B encoder (~25 lines) rendered as SVG — point a handheld scanner or a phone at the screen and it reads. The quantity model is checked so that every scenario path reconciles to Δ 0.

## Status & disclaimer

This is a stand-alone concept demo with scripted conversations and mock data. It is not connected to, derived from, or affiliated with any real manufacturing system, product, or company. All batch numbers, materials, quantities, and process steps are invented.

Possible next steps: connect the chat to a real LLM, add a rules engine for state detection, and expand the exception library.
