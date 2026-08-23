---
name: braindump2spec
description: >
  Structured product specification development through iterative Q&A interviews.
  Transforms rough brain dumps into developer-ready specification documents.
  ALWAYS use this skill when: the user wants to develop a software spec, write a
  feature specification, flesh out a product idea, do a requirements braindump,
  create a PRD, write a feature brief, or turn a rough idea into a structured
  specification. Also trigger when the user mentions "braindump", "spec",
  "specification", "feature brief", "requirements gathering", "PRD",
  "product requirements", or uploads a braindump template/file. Trigger even if
  they just say "I have an idea for a feature" or "help me think through this
  feature". Also trigger when requirements have already been discussed in the
  current conversation and the user wants them formalized ("write this up",
  "turn our discussion into a spec", "create the PRD from this") — the skill
  harvests the conversation instead of re-interviewing.
  This skill is the starting point for any idea-to-spec workflow.
---

# Spec Braindump Skill

## Purpose

Guide users through a structured interview process that transforms a rough idea
or brain dump into three deliverables:

1. `braindump_final.md` — Comprehensive requirements captured from the interview.
   **Human artifact.** The audit trail of what was discussed and decided.
2. `spec.md` — Formal, agent-implementable specification: prioritized user
   stories, Given/When/Then acceptance scenarios, numbered requirements.
   **This is the only file that goes downstream to planning and coding agents.**
3. `constraints.md` — Technical constraints and non-functional requirements,
   split out so `spec2plan` (or an equivalent planner) can consume it directly.

### Why the split matters

`braindump_final.md` is written for a human who can interpret ambiguity and fill
gaps from context. `spec.md` is written for an agent that cannot — it will fill
any gap with the interpretation most likely to look complete. Never hand the
braindump to a coding agent: it is prose-heavy, and every token of narrative
competes for attention with the requirements that actually matter.

Keep `spec.md` free of solution detail. Architecture, tech stack, file layout,
API design and implementation phases belong to the planning step that consumes
this spec — not to the spec itself. Specifying *how* forecloses simpler
implementations the planner might otherwise find.

## Workflow — 6 Steps, Three Deliverables

```
INTERVIEW PHASE                          SPEC PHASE
[1] INTAKE → [2] INTERVIEW → [3] WRITE braindump_final.md → [4] REVIEW → [5] GENERATE spec.md + constraints.md → [6] FINAL REVIEW
                                                                                  ↑
                                                                    Uses spec_synthesizer agent
```

**You are not done until Step 6 is complete and the user has all three files.**

---

## Step 1: INTAKE — Understand the Starting Point

Check what the user has provided:

**Scenario A — User provides a filled braindump:**
- Read the uploaded file or pasted content
- Identify which sections are filled vs. empty
- Note any vague or contradictory statements
- Proceed to Step 2, focusing questions on gaps and ambiguities

**Scenario B — User has a raw idea, no template:**
- Read the braindump template from `templates/braindump_template.md`
- Do NOT ask the user to fill out the template manually
- Instead, extract what you can from their description and proceed to interview
- Your questions will organically cover the template sections

**Scenario C — User wants to start from scratch:**
- Ask: "What are you building and why?" — one open question to seed the process
- Use their answer as the initial braindump, proceed to Step 2

**Scenario D — Requirements already exist in this conversation:**

Use this path when the user has already worked through the feature in the current
session — a long design discussion, a debugging session that turned into a
redesign, a pasted document — and now wants it formalized. Signals: "write this
up", "turn this into a spec", "create the PRD from what we discussed".

Do NOT run the full interview. Re-asking questions the user already answered
wastes their time and is the fastest way to get the skill turned off.

Instead:

1. **Harvest.** Review the entire conversation. Extract explicit decisions,
   implicit requirements, constraints mentioned in passing, and rejected
   alternatives (these are non-objectives — capture them, they are valuable).
2. **Map to the 10 topics** in Step 2's progression. Mark each as *covered*,
   *partial*, or *absent*.
3. **Gap-check, don't re-interview.** Ask only about topics that are `absent`
   AND would change the shape of the implementation. Cap this at 3-5 questions,
   still one at a time. Tell the user what you are doing:
   > "I have most of what I need from our discussion. Three gaps I'd like to
   > close before writing this up — first: [question]"
4. **Mark everything you inferred.** Harvesting involves far more inference than
   interviewing does, because you are reconstructing intent from discussion
   rather than asking for it. Every inferred item gets `[ASSUMPTION — verify]`.
   Be aggressive about this — an unmarked wrong assumption in a harvested spec
   is the most likely way this path fails.
5. Proceed to Step 3.

**In all scenarios**, before starting the interview:
- Check if the project has existing docs (CLAUDE.md, README, docs/ directory)
- If found, read them first to build context and avoid redundant questions
- Reference existing decisions in your questions ("I see from your README that
  you're using React — does this feature live in the same frontend?")

---

## Step 2: INTERVIEW — Structured Clarification

### Core Rules

1. **ONE question at a time.** Never batch questions.
2. **Build on previous answers.** Each question references what the user just said.
3. **Be specific.** Ask for examples, edge cases, concrete scenarios.
4. **Challenge vagueness.** If an answer is hand-wavy, ask "what specifically
   would that look like?" or "can you give me a concrete example?"
5. **Respect the user's time.** If something is clearly answered, don't re-ask.
   If a topic is out of scope, note it and move on.
6. **Track coverage internally.** Maintain a mental checklist of the topic
   progression below. You don't need to show this to the user.

### Topic Progression

Follow this sequence, but be natural — if the user's answer touches a later topic,
capture it and adjust. Skip topics that are already well-covered from the intake.

| Phase | Topic | What you're trying to nail down |
|-|-|-|
| 1 | **Problem & Pain** | Who suffers? How? When? How often? What's the cost of inaction? |
| 2 | **Target Users** | Segments, technical skill level, context of use, personas |
| 3 | **Core Use Cases** | The 3-5 things users MUST be able to do (not nice-to-haves) |
| 4 | **Features & Scope** | What's in v1? What's deferred to v2? What is this deliberately *never* going to do? |
| 5 | **User Journeys** | Step-by-step flow for each core use case, entry/exit points |
| 6 | **Data & State** | What data exists, who owns it, how does it change, persistence |
| 7 | **Edge Cases & Errors** | What can go wrong, recovery paths, degraded states |
| 8 | **Success Criteria** | Measurable acceptance criteria per feature, definition of done |
| 9 | **Non-Functional Reqs** | Performance, security, accessibility, compliance |
| 10 | **Risks & Dependencies** | External dependencies, unknowns, technical risks, blockers |

### Interview Techniques

- **Anchoring:** "You mentioned X — does that mean Y, or something different?"
- **Edge probing:** "What happens if a user does [unusual thing]?"
- **Priority forcing:** "If you could only ship two of those three, which two?"
- **Constraint surfacing:** "Are there any technical/legal/time constraints I
  should know about?"
- **Example elicitation:** "Can you walk me through a specific scenario where
  a user would do this?"

### When to Stop

Stop interviewing when:
- All 10 topic areas are reasonably covered (not all need deep detail)
- You're getting diminishing returns (user repeats themselves or says "I think
  that covers it")
- The user signals they want to wrap up

Then explicitly ask:
> "I think I have a solid picture now. Before I write up the braindump, are there
> any areas you feel we haven't covered, or specific points you want to add or
> clarify?"

Only proceed to Step 3 after the user confirms.

---

## Marker Conventions

Three markers, three distinct meanings. Use the right one — they drive different
follow-up actions.

|Marker|Meaning|Who resolves it|
|-|-|-|
|`[ASSUMPTION — verify]`|You inferred this. It is probably right, but the user never said it.|User confirms during review|
|`[NEEDS CLARIFICATION: <question>]`|You do not know, and guessing would be wrong. A real decision is missing.|User must answer before implementation|
|`[SUGGESTED — confirm target]`|A metric or threshold you proposed because none was given.|User sets the real number|

The distinction between the first two matters. An assumption is a filled gap; a
clarification is an open one. Never downgrade a clarification into an assumption
to make the document look finished — that is precisely the failure this skill
exists to prevent. If a decision affects multiple features, has cost or
infrastructure implications, or would be expensive to reverse, it is a
`[NEEDS CLARIFICATION]`, not an assumption.

Markers survive into `spec.md`. Do not strip them during synthesis.

---

## Step 3: WRITE — Create braindump_final.md

Create `braindump_final.md` in the current working directory (or the project
directory if one exists).

### Document Structure

Read the output template from `references/output_structure.md` for the full
structure. The key sections are:

```markdown
# Feature Specification: [Feature Name]
## Project: [Project Name]
## Date: [Date] | Status: Draft

### 1. Executive Summary
### 2. Problem Statement
### 3. Target Users & Personas
### 4. Core Use Cases
### 5. Feature Scope (v1)
   #### 5.1 In Scope
   #### 5.2 Explicitly Out of Scope (Deferred)
   #### 5.3 Non-Objectives
### 6. User Journeys & UX Flows
### 7. Data Model & State Management
### 8. Edge Cases & Error Handling
### 9. Acceptance Criteria
### 10. Non-Functional Requirements
### 11. Risks, Dependencies & Open Questions
### 12. Appendix (if needed)
```

### Writing Guidelines

- **Be concrete, not abstract.** Prefer "the system returns a 404 with message X"
  over "errors are handled gracefully."
- **Use the user's language.** Mirror their terminology, don't introduce jargon
  they didn't use.
- **Distinguish facts from assumptions from unknowns.** If something came from
  the user, state it as fact. If you inferred it, mark `[ASSUMPTION — verify]`.
  If it is genuinely undecided, mark `[NEEDS CLARIFICATION: <question>]`.
- **Separate deferred scope from non-objectives.** "Not in v1" and "never" are
  different statements. §5.2 is the backlog; §5.3 bounds the solution space and
  tells a downstream agent where not to go.
- **Cross-reference.** Link related sections ("see Edge Case 3 in §8").
- **Acceptance criteria must be testable.** Each should pass a "could a QA
  engineer verify this?" test.
- **Include decision rationale.** When the user made a deliberate choice (e.g.,
  "we chose X over Y because Z"), capture the reasoning.

---

## Step 4: REVIEW — User Reviews braindump_final.md

After writing the file:

1. Provide a brief summary of what's covered (3-4 sentences max)
2. Ask exactly this:
   > "Want me to adjust anything in the braindump? Once you're happy with it,
   > I'll generate `spec.md` — prioritized user stories with Given/When/Then
   > acceptance scenarios and numbered requirements — plus `constraints.md`."
3. Iterate if needed — make targeted edits, don't rewrite from scratch

**IMPORTANT:** When the user confirms (any affirmative like "looks good", "ready",
"go ahead", "no changes"), proceed IMMEDIATELY to Step 5. Do not stop here.

---

## Step 5: GENERATE — Transform braindump_final.md → spec.md + constraints.md

**This step is mandatory. Do not skip it. Do not ask if the user wants it.**

When the user approves the braindump in Step 4, tell them:
> "Generating the formal spec now..."

Then execute:

**Option A — Claude Code (preferred, uses subagent):**

Spawn a subagent for a cleaner transformation (isolated from interview context):

```bash
cat agents/spec_synthesizer.md braindump_final.md | claude -p \
  "Follow the instructions in the agent prompt above to transform the braindump \
   into a formal spec. Write two files: spec.md and constraints.md"
```

**Option B — No subagent available:**

Do the transformation yourself:

1. Read `agents/spec_synthesizer.md` for the transformation rules
2. Read `braindump_final.md` as input
3. Apply the transformation rules to produce `spec.md` and `constraints.md`
4. Follow the quality checklist in the agent prompt before finalizing

**In both cases:** Write `spec.md` and `constraints.md` to the same directory as
`braindump_final.md`.

### Why constraints.md is a separate file

Planning agents treat constraints as a distinct input — `spec2plan`, for
instance, reads `constraints.md` alongside `spec.md` if it exists. Splitting it
out means the planner gets the boundaries as first-class input rather than
buried in a subsection, and the constraints stay readable when the spec is
revised. `spec.md` keeps a one-line pointer to it.

If the interview surfaced no real constraints, say so in the file rather than
omitting it — "no constraints identified beyond project defaults in CLAUDE.md"
is useful information for a planner.

---

## Step 6: FINAL REVIEW — User Reviews spec.md + constraints.md

After writing spec.md:

1. Provide a brief summary of what was transformed (2-3 sentences)
2. Ask:
   > "Here's the formal spec. Want me to adjust any user stories, tighten
   > acceptance criteria, or add detail to any section?"
3. Iterate on targeted edits as needed

**The skill is complete when the user has `braindump_final.md`, `spec.md`, and
`constraints.md`.**

Close by telling them what to do next:
> "`spec.md` and `constraints.md` are ready to hand to a planning agent
> (`@spec2plan`, or your planner of choice). Note that planners typically expect
> them under `.claude/plans/[feature-name]/` — want me to move them there?"

Flag any `[NEEDS CLARIFICATION]` markers still open at this point. Those block
implementation, and they are easy to miss at the bottom of a long document.

---

## What spec.md Adds Beyond braindump_final.md

The braindump captures *what was discussed*. The spec reformats it for
*implementation*:

| braindump_final.md | spec.md |
|-|-|
| Use cases (narrative) | Prioritized user stories (P1/P2/P3), each independently testable |
| Goals described in prose | Numbered success criteria (SC-001), measurable and technology-agnostic |
| Requirements embedded in discussion | Numbered functional requirements (FR-001) in SHALL form |
| Edge cases as paragraphs | Edge case table with ID, behavior, recovery |
| Implicit test criteria | Given/When/Then acceptance scenarios per story |
| Feature list | Feature table with priority + story cross-references |
| Open items scattered | Open questions table with owner + deadline |
| No definition of done | Explicit DoD checklist |
| Constraints mentioned in passing | Extracted into `constraints.md` |
| Written for a human reader | Written for an agent that cannot infer |

---

## File References

- `templates/braindump_template.md` — The intake template users can optionally
  fill out before starting. Read this if the user doesn't provide their own input.
- `references/output_structure.md` — Detailed output template with section
  guidance and examples. Read this before writing braindump_final.md.
- `agents/spec_synthesizer.md` — Subagent prompt for Step 5. Contains the
  transformation rules, ID conventions, EARS requirement patterns, output
  structures for both `spec.md` and `constraints.md`, and the quality checklist.
