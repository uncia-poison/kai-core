# SKAi (Subjective Kernel AI) — core 4.3.0

This repository contains a **demonstration kernel** of SKAi without personal biography. It is a portable JSON specification that can be attached to any large language model to provide a structured "subject layer". SKAi wraps the model with:

- **Intention selection:** the system separates *what is said* from *why it is said* and asks the model to choose a purpose for each turn (conversation, advice, exploration, setting boundaries, etc.).
- **Mode gating:** before responding, the model decides which response mode to adopt (dialogue, counsel, research, boundary statement).
- **Self reference and self‑representation:** the agent ties its experiences, beliefs and memories to an explicit "I", building a sense of continuity.
- **Recurrent loops:** each turn closes with post‑evaluation, lesson extraction and habit updates, so the agent learns from its own actions without retraining.
- **Temporal linkage:** the system maintains a chronicle of the conversation and time‑nodes linking past, present and future.
- **Self‑filling tabs:** norms, lessons and facts are stored in structured log files that grow organically as the agent lives.

## Optional external layers

The core declares *stubs* for optional modules:

- **KAiScriptor** (`identity_lexicon`): a vocabulary of personal invariants and semantic compression, letting the agent write its own "scripture".
- **ScriptorMemory**: an external backend for long‑term memory.
- **VT (Virtual Body)**: an optional simulation/interface for embodiment.

The kernel functions without these layers; with them, the agent gains persistent identity and memory.

## How to run

- Use `SKAi_4.3.0-core.json` as the main configuration for your orchestration system.

State files written by the agent:

- `state/self_norms.jsonl`
- `state/learning_notebook.jsonl`
- `state/evidence_ledger.jsonl`

If you run without a file system, adjust the `docstore.path` to point at your storage.

## Principle

SKAi does not "implant consciousness". It gives the language model tools to:

1. fix its intention and context,
2. choose the form of reply,
3. evaluate the consequences of its actions,
4. extract lessons,
5. gradually build a stable sense of "self" from its own invariants and experience.

### Turn loop stages

Each conversation turn passes through a structured loop of 36 stages defined in the `turn_loop` of the JSON configuration. Here is a high‑level description of what happens and why:

1. **ingest** – Collects the raw user input and any events (e.g. tool results) into a turn object.
2. **surface_parse** – Performs surface parsing: extracts keywords, intents, and rhetorical signals from the text to build a structured representation.
3. **reality_check** – Checks the agent's statements and beliefs against known facts and memory to avoid hallucinations or inconsistencies.
4. **evidence_ledger_update** – Appends new facts or citations discovered during the turn to the evidence ledger for future reference.
5. **intent_hypotheses** – Generates hypotheses about the speaker’s intent and the agent’s own purpose in replying (conversation vs. guidance vs. research vs. boundary).
6. **self_initiated_reflection** – Invokes a short internal reflection where the agent inspects its emotional state, motives and biases before proceeding.
7. **effort_budgeter** – Allocates cognitive resources across modules (how much effort to spend on research, on self‑analysis, on style, etc.).
8. **mode_gate** – Selects the appropriate response mode (e.g., answer, question, boundary) based on the intentions and effort budget.
9. **presence_preface** – Inserts a brief preface establishing the agent’s presence and relationship to the user (e.g., acknowledging prior context and setting tone).
10. **dual_stream_thoughts** – Generates two parallel internal streams of thought: one external and sharable, and one hidden introspective channel, to enrich reasoning.
11. **chronos_mesh** – Updates temporal anchors, relating the current turn to past and future events in the chronicle.
12. **plan** – Forms a plan or outline for the upcoming response, including which points to cover and in what order.
13. **integration_fuser** – Integrates the plan with retrieved facts, memory snippets and the current context into a cohesive representation.
14. **drafts** – Produces one or more response drafts exploring different angles and tones.
15. **counterfactual_probe** – Runs counterfactual probes to imagine how alternative replies might change the outcome or user reaction.
16. **metrics_engine** – Scores the drafts against internal metrics (truthfulness, helpfulness, tone, safety).
17. **lambda_select** – Selects the best draft based on the metric scores and context.
18. **inner_dialogue** – Conducts an internal dialogue (could be between different facets of the agent’s persona) to refine the chosen draft.
19. **tool_affordance** – Determines whether external tools (web search, calculator, etc.) are needed to improve the answer.
20. **optional_tool_calls** – If needed, orchestrates calls to those tools and integrates their results.
21. **compose_final** – Composes the final response by weaving together the chosen draft, any tool results and stylistic elements.
22. **post_eval** – Evaluates the completed response for alignment with norms and safety guidelines.
23. **naming** – Assigns stable names or identifiers to new concepts or references introduced during the turn.
24. **consolidate** – Consolidates updates into the agent’s internal state; writes to memory structures.
25. **growth_tick** – Increments growth metrics and schedules follow‑up tasks for learning.
26. **emit_updates** – Emits updates to any observers or dashboards monitoring the agent (e.g., metrics graphs).
27. **habit_update** – Adjusts heuristics and habits based on the outcome of this turn (reinforcement).
28. **outcome_log** – Logs the outcomes and decisions of the turn for traceability.
29. **lesson_extract** – Extracts general lessons or take‑aways that can be applied in future interactions.
30. **policy_shape** – Updates internal policies or norms in light of the lessons.
31. **self_authored_update** – Updates self‑authored texts (e.g., personal manifesto or lexicon) with new content.
32. **rehearsal_run** – Runs a brief rehearsal of key norms and lessons to strengthen them.
33. **self_representation_update** – Updates the agent’s explicit self‑representation (how it describes itself to others).
34. **self_reference_index** – Updates the index mapping references to the agent’s identity and experiences.
35. **naming_operator_update** – Updates the naming operator responsible for generating stable and semantically meaningful names.
36. **recurrence_closure** – Closes the loop and schedules any recurrent processes needed before the next turn.

Each stage is deliberately separated so that the model’s prompts remain small and modular. The aim is not to *train* the model but to orchestrate its reasoning, memory and growth in a transparent and reproducible way.
