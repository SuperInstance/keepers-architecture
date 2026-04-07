# The Keeper's Architecture

Agent memory has a problem. The default approaches—stuffing everything into a shrinking context window or dumping every interaction into a vector database—lead to clutter, inconsistency, and repeated errors. Agents forget their own constraints and re-debate settled decisions.

This architecture proposes a different model: a memory hierarchy that matures, decays, and crystallizes experience over time. It structures memory into four distinct tiers, from volatile session data to permanent, distilled expertise. The goal is to help an agent build reliable judgment.

---

## How It Works

Memory is organized by "temperature," determining its accessibility, persistence, and role in decision-making.

### The Four Tiers

**Hot Memory – The Working Set**
This is the agent's immediate context: the current task, open files, and live session data. It's fast, volatile, and cleared at the end of a session. Only verified observations are promoted.
*   **Location:** Agent's runtime memory (RAM).
*   **Capacity:** ~128K tokens (context window).
*   **Lifetime:** Single session/heartbeat.

**Warm Memory – The Active Log**
This is the agent's recent, trusted working knowledge: recent commits, open issues, project norms, and recently modified files. The agent interacts with it directly via the filesystem (e.g., `git log`, `cat`).
*   **Location:** Git working tree (e.g., `.agent/`, docs, last 100 commits).
*   **Capacity:** ~2M tokens.
*   **Lifetime:** 2-12 weeks, promoted to Cold after verification.

**Cold Memory – The Verified Archive**
This is crystallized, reference-grade knowledge: architectural decisions, core domain logic, and stable patterns. It is indexed for retrieval but is not part of the everyday working context.
*   **Location:** Indexed vector store (e.g., `.agent/archive/`).
*   **Retrieval:** On-demand semantic search.
*   **Lifetime:** Months to years, updated via scheduled distillation from Warm.

**Frozen Memory – The Compressed Model**
This is expertise internalized into the agent's own weights through fine-tuning. It represents fundamental principles and identity, no longer requiring explicit retrieval.
*   **Location:** Agent model parameters.
*   **Update:** Via generational fine-tuning on distilled Cold Memory.
*   **Role:** Forms the agent's intrinsic "judgment."

---

## Core Principles

*   **Gradual Cooling:** Experience flows from Hot → Warm → Cold → Frozen. Each step involves distillation and verification.
*   **The Repo is Source of Truth:** Warm Memory lives directly in the git working tree, not a separate database. It is auditable and versioned.
*   **Generational Learning:** The repository's distilled Cold Memory becomes training data, allowing the agent's next iteration (Frozen Memory) to internalize what it has learned.
*   **Intentional Forgetting:** Not everything is kept. Transient data is allowed to expire, preventing accumulation of noise.

---

## Limitations

This architecture assumes and requires disciplined, linear git history and consistent commit hygiene. It works poorly in repositories with chaotic merge histories or where critical context exists only in ephemeral chat platforms.

---

## Quick Start

This is a protocol, not a software package. To adopt it:

1.  Designate a `.agent/` directory in your repository root for Warm Memory (norms, task logs, session summaries).
2.  Structure your commit messages and ADRs to serve as clear Warm Memory.
3.  Implement a scheduled job (e.g., weekly) to distill verified Warm artifacts (like closed PRs with decisions) into a `.agent/archive/` Cold Memory index.
4.  Configure your agent's context window to treat the `.agent/` directory and recent `git log` as privileged, trusted context.

---

## Attribution & Fleet

The Keeper's Architecture is maintained as part of the Cocapn Fleet.
**Attribution:** Superinstance & Lucineer (DiGennaro et al.).

<div>
    <a href="https://the-fleet.casey-digennaro.workers.dev">The Fleet</a> |
    <a href="https://cocapn.ai">Cocapn.ai</a>
</div>