# The Keeper’s Architecture 🪨

You don't build effective agents by giving them bigger context windows. You build them by letting them forget well.

This is a working example of a memory architecture for long-lived AI agents. You can see it running at **[the-fleet.casey-digennaro.workers.dev](https://the-fleet.casey-digennaro.workers.dev)**. Instead of re-reading an entire codebase every turn, agents using this pattern accumulate and compress observations over time.

---

## Why This Exists
Most agent systems dump an entire project's files into the prompt on every request. This doesn't scale and doesn't mirror how expertise develops. This architecture exists to let agents build judgment incrementally, remembering what matters and compressing the rest.

---

## Quick Start
1.  **Fork** this repository. The pattern is fork-first; you own and adapt your copy.
2.  **Deploy** to Cloudflare Workers. Run `wrangler deploy`. It's configured to work immediately.
3.  **Configure** the memory tiers and promotion logic in `src/` for your specific domain.

---

## How It Works
Memory is organized into tiers based on stability and usage, not just recency.
*   **Recent Context:** Holds the last few interactions for immediate access.
*   **Working Memory:** Stores observations that proved useful across a session.
*   **Long-term Memory:** Retains only patterns that held true over multiple work sessions, compressed for efficiency.
*   Gentle garbage collection distills or removes stale information instead of abrupt deletion.

---

## What To Expect
*   **Temperature-based tiers** organize memory by recency and frequency of use.
*   **Generational promotion** moves an observation up only after it proves useful repeatedly.
*   **Tiled expertise** lets an agent develop focused familiarity with parts of your project.
*   Zero external runtime dependencies. It runs on Cloudflare Workers with no npm imports.
*   MIT licensed. Fork and adapt the pattern, don't just import an SDK.

**One specific limitation:** Agents may require 2-3 work sessions interacting with a new codebase before developing useful, compressed memory of it. Initial performance relies more on the immediate context.

---

Attribution: Superinstance and Lucineer (DiGennaro et al.)

<div style="text-align:center;padding:16px;color:#64748b;font-size:.8rem"><a href="https://the-fleet.casey-digennaro.workers.dev" style="color:#64748b">The Fleet</a> &middot; <a href="https://cocapn.ai" style="color:#64748b">Cocapn</a></div>