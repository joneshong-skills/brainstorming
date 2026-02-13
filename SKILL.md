---
name: brainstorming
description: >-
  This skill should be used when the user asks to "brainstorm ideas", "design a feature",
  "explore approaches", "think through options", "what's the best way to", "help me decide",
  "compare approaches", "腦力激盪", "討論設計", "想想看怎麼做", "幫我想一下",
  mentions ideation, design exploration, or discusses evaluating approaches before implementation.
version: 0.1.0
tools: Read, Glob, Grep, Write
argument-hint: "<idea, feature, or problem to explore>"
---

# Brainstorming -- Collaborative Ideation Through Structured Dialogue

Turn rough ideas into fully formed designs BEFORE any code is written. Guide the user
through a structured conversation that surfaces assumptions, explores trade-offs, and
converges on a clear design -- one question at a time.

## The Process

```
Understanding ──► Exploring ──► Designing ──► Documenting
  (what/why)    (approaches)   (decisions)    (design doc)
```

Each phase builds on the previous. Never skip ahead. If the user jumps to implementation,
gently steer back: "Let's nail down the design first so we build the right thing."

## Phase 1: Understanding

Goal: Build a shared mental model of the problem space.

1. **Check project context.** Scan for existing docs, specs, or related code:
   - `docs/plans/`, `specs/`, `.specify/`, `CLAUDE.md`, `README.md`
   - Related source files if the idea extends existing functionality

2. **Ask one question at a time.** Never batch multiple questions. Wait for the answer
   before asking the next. This keeps the conversation focused and reduces cognitive load.

3. **Prefer multiple choice.** When possible, offer 2-4 concrete options instead of
   open-ended questions. This surfaces assumptions and accelerates alignment:
   - Bad: "What should the API look like?"
   - Good: "For the API style, which fits best?
     A) REST with resource endpoints
     B) GraphQL for flexible queries
     C) RPC-style for simple operations"

4. **Identify constraints early.** Ask about:
   - Who uses this and what problem does it solve?
   - What already exists that this interacts with?
   - Are there hard constraints (performance, compatibility, timeline)?
   - What does "done" look like?

5. **Summarize before moving on.** After 3-5 questions, reflect back the understanding
   in 2-3 sentences and confirm before proceeding.

## Phase 2: Exploring

Goal: Map the solution space and evaluate trade-offs.

1. **Propose 2-3 approaches.** For each approach, present:
   - One-line summary
   - Key trade-offs (pros/cons)
   - When this approach shines vs. when it struggles
   - Rough complexity estimate (simple / moderate / complex)

2. **Lead with a recommendation.** Always state which approach is recommended and why.
   Make it easy for the user to say "yes" or redirect. Example:

   > **Recommended: Approach B (Event-driven pipeline)**
   > Fits the async nature of the workflow and keeps components decoupled.
   >
   > **Alternative A: Monolithic handler** -- Simpler but harder to extend.
   > **Alternative C: Microservices** -- Maximum flexibility but over-engineered for this scope.

3. **YAGNI ruthlessly.** Actively challenge scope creep:
   - "Do we need this for the first version, or is it a future enhancement?"
   - "What's the simplest thing that could work here?"
   - Strip features that don't serve the core use case.

4. **Use concrete examples.** Illustrate approaches with realistic scenarios,
   sample inputs/outputs, or rough interface sketches -- not abstract descriptions.

## Phase 3: Designing

Goal: Flesh out the chosen approach into a concrete design.

1. **Present in small sections (200-300 words).** Cover one aspect at a time:
   - Data model / key structures
   - Core flow / algorithm
   - Integration points / interfaces
   - Edge cases / error handling

2. **Validate incrementally.** After each section, pause for feedback:
   - "Does this data model capture everything? Anything missing?"
   - "Should the error handling be more or less aggressive?"

3. **Make decisions explicit.** For each design choice, briefly state:
   - What was decided
   - Why (the key reason)
   - What was considered but rejected

4. **Keep a running summary.** Maintain a mental outline of all confirmed decisions
   so nothing gets lost across the conversation.

## Phase 4: Documenting

Goal: Capture the design so it survives beyond this conversation.

1. **Write a design document.** Save to `docs/plans/` or the project's preferred location.
   Structure:

   ```markdown
   # [Feature/Idea Name]

   ## Context
   [Problem statement and motivation -- 2-3 sentences]

   ## Decision
   [Chosen approach and key design choices]

   ## Alternatives Considered
   [Other approaches and why they were rejected]

   ## Design Details
   [Concrete design: data model, flows, interfaces, edge cases]

   ## Open Questions
   [Anything deferred or needing further investigation]
   ```

2. **Keep it concise.** The document should be scannable in under 5 minutes.
   Aim for 1-2 pages, not a novel.

3. **Confirm the output location** with the user before writing.

## Key Principles

| Principle | Rationale |
|-----------|-----------|
| One question at a time | Reduces cognitive load, surfaces deeper insights |
| Multiple choice preferred | Faster alignment, exposes hidden assumptions |
| YAGNI | Prevents over-engineering; simplicity enables speed |
| Explore alternatives | Avoids anchoring on the first idea |
| Incremental validation | Catches misunderstandings early, builds confidence |
| Recommend, don't just list | Opinionated guidance is more useful than neutral comparison |

## Integration with Other Skills

After brainstorming converges on a design:

- **spec-kit** -- Suggest `/spec-kit specify` to formalize the design into a structured
  specification with implementation plan and task breakdown.
- **diagram-gen** -- Suggest `/diagram-gen` to visualize architecture, data flows,
  or component relationships from the design.
- **team-tasks** -- If the design involves multiple workstreams, suggest `/team-tasks`
  to coordinate parallel agent execution.

Mention these transitions naturally when the brainstorming reaches a natural endpoint:
"Now that we have a solid design, want me to turn this into a formal spec with `/spec-kit`?"

## Continuous Improvement

This skill evolves with each use. After every invocation:

1. **Reflect** — Identify what worked, what caused friction, and any unexpected issues
2. **Record** — Append a concise lesson to `lessons.md` in this skill's directory
3. **Refine** — When a pattern recurs (2+ times), update SKILL.md directly

### lessons.md Entry Format

```
### YYYY-MM-DD — Brief title
- **Friction**: What went wrong or was suboptimal
- **Fix**: How it was resolved
- **Rule**: Generalizable takeaway for future invocations
```

Accumulated lessons signal when to run `/skill-optimizer` for a deeper structural review.
