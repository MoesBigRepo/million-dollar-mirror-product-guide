> Canonical product guide source. Update both files in this folder whenever the product gains or changes a feature or function.

*Product Architecture Guide*

# Million Dollar Mirror

A local-first behavioral operating system for daily execution.

This system is a single-user personal operations product that converts day-to-day behavior into structured execution pressure. It combines daily planning, task capture, focused time tracking, project prioritization, money logging, lightweight health and knowledge tracking, and a derived interpretation layer that produces scores, alerts, diagnostics, forecasts, and reset prompts.

- Type: Workflow-heavy personal ops system
- Mode: Local-first PWA
- Use: Repeated daily operation
- Pattern: Event-sourced + derived guidance
- Audience: Single self-directed operator
- Surface: Dashboard + execution + analysis

## Scope note

This page explains the currently implemented product surface and clearly labeled inferences drawn from the codebase. It is not a marketing page and it is not a speculative roadmap.

- Implemented in current repo
- Strong inference from structure
- Open clarification or unresolved scope

Core thesis: the product does not stop at capturing tasks. It interprets behavior, identifies the weakest operating area, and turns drift into explicit recovery actions.

## On this page

- [1. Product at a Glance](#glance)
- [2. Core Problem & Solution](#problem)
- [3. User Types & Role Layers](#roles)
- [4. Feature Architecture](#features)
- [5. Functional Logic](#logic)
- [6. Benefits by Layer](#benefits)
- [7. System Flow / Lifecycle](#flow)
- [8. Information Architecture](#ia)
- [9. Key Entities / Objects](#entities)
- [10. Notes, Constraints & Risks](#risks)
- [11. Not a Developer Version](#plain-english)
- [12. Final Summary](#summary)

*Product at a Glance*

## The system in one screenful

The product behaves less like a simple productivity app and more like a compact behavioral control system. It tracks what the user planned, what they actually did, how that activity changes the operating state, and what correction the system believes should happen next.

*Primary users*

### Single self-directed operator

Currently implemented as a one-person system, likely for a founder, builder, or highly structured individual.

- Implemented
- Audience inferred

*Primary problem solved*

### Execution drift hidden across multiple life systems

It surfaces where momentum is failing instead of treating tasks, time, money, health, and discipline as separate silos.

- Weakest category becomes the forcing function

*Main system type*

### Local-first personal operating model

Runs in the browser, stores data locally, and derives guidance from event history without requiring a backend service.

- Local-first

*Core operating model*

### Capture → replay → derive → interpret → recover

User actions are turned into events, then summarized into scores, diagnostics, and reset prompts.

- Event-sourced daily loop

*Complexity level*

### Medium-high

The product combines data capture, workflow execution, behavioral interpretation, and automation-assisted recovery.

- Structural inference

*Primary value created*

### Clear next action under pressure

Instead of only recording work, the system keeps translating behavior into what should happen next.

- Less ambiguity, faster correction

*Core Problem and Solution*

## Why the system exists and how it solves the gap

*Problem*

### Normal productivity tools do not explain drift

- Task lists capture work items but rarely connect them to health, money, attention, and discipline.
- Time tools show duration but not whether the time supported the most fragile part of the system.
- Journals and reviews create reflection, but often too late and without deterministic next steps.
- Financial logging shows outcomes, not whether the user is applying enough revenue pressure before cash lands.

*Structural solution*

### One behavioral loop with a derived guidance layer

- The day begins with an explicit mission, emotional state, and non-negotiable commitments.
- Work, money, health, knowledge, and discipline signals are captured in a shared event stream.
- The system computes category scores and identifies the weakest operating area.
- Interpretation layers then produce alerts, diagnostics, reviews, forecasts, and reset actions that feed the next day.

Why this matters: the product is designed to reduce the distance between behavior and correction. It does not just record activity. It tries to make behavioral failure legible quickly enough that the user can react while the system is still recoverable.

*User Types / Role Layers*

## Who this system is built around

The current implementation is not multi-user. Still, the architecture implies several role layers that matter when explaining the system and planning future evolution.

*Primary role*

### Personal operator

The main user is the person whose day, work, and behavior are being managed. They open the day, execute, log pressure, and review outcomes.

- Needs: clarity, structure, honest feedback, visible priority.
- Trying to do: avoid drift, move meaningful work, protect non-negotiables.
- Most relevant system areas: Dashboard, Daily, Projects, Money.

- Implemented

*Invisible support layer*

### System integrity layer

Not a visible human role, but a real operational layer inside the product. It preserves local state, active timer continuity, event replay, and transaction safety.

- Needs: reliable hydration, rollback on write failures, duplicate protection.
- Trying to do: keep guidance trustworthy and state coherent.
- Most relevant system areas: persistence, derivation, validation rules.

- Implemented

*Future role possibility*

### Coach, reviewer, or accountability partner

The language and outputs suggest a product that could eventually support external review or coaching, but no such role exists in the current implementation.

- Potential need: view weekly drift, diagnose patterns, guide recovery.
- Potential system areas: review engine, diagnostics, forecast, history.
- Status: not implemented, only implied by the behavioral framing.

- Open scope
- Strong inference

*Important constraint*

### No multi-tenant or admin surface today

There are no accounts, orgs, permissions, shared workspaces, or admin views in the present repo. The system should be explained as a single-user product first, not as an unfinished team platform.

- Design implication: current IA can stay personal and day-centric.
- Communication implication: avoid implying role complexity that does not exist.

- Implemented constraint

*Feature Architecture*

## The product organized by functional responsibility

The feature set makes the most sense when grouped by operational role: daily execution, domain-specific engines, interpretation layers, recovery mechanics, and supporting infrastructure.

### Daily execution loop

- Central implemented surface

#### Morning Check-in

Opens the day with energy, anxiety, mission, non-negotiables, and a target count.

- What it does: creates or updates the current daily log and emits a check-in event.
- Why it matters: sets the frame for what “today” means and feeds discipline, mind, and health-related scoring inputs.
- Users affected: primary operator.
- Workflow role: start-of-day anchor.
- Dependencies: hydration must complete first.

#### Task Board

Captures actionable work for the day, with category tagging, optional project linkage, and completion state.

- What it does: creates daily tasks and records completion events.
- Why it matters: converts abstract pressure into executable units and feeds category performance.
- Users affected: primary operator.
- Workflow role: main execution surface.
- Dependencies: UI expects today to be open before manual add.

#### Quick Logs + Focus Timer

Provides low-friction evidence for exercise, substance-free behavior, medical compliance, and focused work time.

- What it does: records binary quick logs, starts and stops a single active timer, and persists focused minutes.
- Why it matters: gives the system proof of activity without large forms.
- Users affected: primary operator.
- Workflow role: mid-day behavioral capture.
- Dependencies: one active timer at a time.

#### Evening Review

Closes the day with notes and a count of completed non-negotiables.

- What it does: updates the daily log, emits a day-review event, and strengthens the review and discipline layers.
- Why it matters: gives the system an explicit closeout instead of leaving the day structurally open.
- Users affected: primary operator.
- Workflow role: end-of-day reconciliation.
- Dependencies: requires an existing daily log.

### Domain engines

- Operational modules

#### Project Engine

Maintains a ranked stack of projects based on urgency and income weight, with active, blocked, and shipped states.

- Key functions: create project, update status, raise urgency, queue today’s move, start a project-linked focus block.
- Benefit: keeps work selection tied to leverage instead of random task accumulation.
- Users affected: primary operator.
- Workflow relevance: supports the projects category and shipping signal.

#### Money Engine

Combines revenue-pressure actions with actual cash logging.

- Key functions: queue revenue tasks, record income, expense, and debt payments.
- Benefit: distinguishes between effort that pushes revenue and realized money that already landed.
- Users affected: primary operator.
- Workflow relevance: supports money scoring, survival pressure, and diagnostics.

#### Book Engine

Tracks active books, page goals, and daily reading progress.

- Key functions: create book, define daily page target, log progress.
- Benefit: keeps knowledge accumulation operational rather than aspirational.
- Users affected: primary operator.
- Workflow relevance: supports the knowledge score.
- Status: implemented, but secondary in current product hierarchy.

#### Health Detail

Captures weight, sleep, medical compliance, and an optional note.

- Key functions: save health log with sanitized numeric inputs.
- Benefit: connects physical stability to execution quality.
- Users affected: primary operator.
- Workflow relevance: supports health scoring and recovery interpretation.
- Status: implemented, but not yet elevated to its own major section.

### Interpretation layer

- Derived guidance

#### Score Snapshot + Priority Focus

Computes seven category scores and a life score, then identifies the weakest operating area.

- What it does: turns summaries into a ranked pressure model.
- Why it matters: creates a single “controlling” category instead of competing priorities.
- Users affected: primary operator.
- Dependencies: event replay and daily summary construction.

#### Alerts + Nudges

Flags collapse risk, inactivity, non-negotiable failure, and priority instability.

- Key functions: create human-readable warning states and short mentor-style nudges.
- Benefit: surfaces trouble as a visible operating condition, not as an abstract score decline.
- Workflow relevance: triage and immediate interpretation.

#### Diagnostics + Recommendations

Explains why the user is stuck and proposes action aligned to the source of drift.

- Key functions: classify issues such as revenue avoidance, project stagnation, or time misallocation.
- Benefit: replaces vague self-judgment with named constraints.
- Dependencies: score history, time breakdown, reviews, project priorities.

#### Forecast + Adaptive Mentor Brief

Projects near-term readiness and produces a condensed pressure summary.

- What it does: estimates collapse risk, projected life score, and at-risk categories.
- Why it matters: gives the user a forward-looking reason to intervene now.
- Workflow relevance: supports triage, not just historical review.

### Recovery and automation layer

- Assisted correction

#### Prepared Resets + Automation Triggers

Arms structured reset suggestions for the areas most likely to continue failing.

- What it does: suggests non-negotiables and a reset mission tied to priority, diagnostics, and forecast risk.
- Why it matters: makes recovery preparation explicit before the next day starts.
- Workflow relevance: bridge between interpretation and next action.
- Important note: this is preparation, not full autopilot.

#### Review Command Loop

Builds today’s action set from the latest priority, diagnostics, automation triggers, and review drift.

- What it does: can apply the top reset and queue missing non-negotiables into today.
- Why it matters: compresses interpretation into direct execution.
- Dependencies: live review state plus available trigger data.

#### XP, Streak, Penalty Debt, Comeback

Tracks reinforcement, missed-day penalties, and accelerated recovery after gaps.

- Key functions: award XP for real operating behavior, penalize ignored focus, activate comeback multiplier after a lapse.
- Benefit: makes continuity and recovery legible, not just outcome quality.
- Workflow relevance: motivation and recovery framing.

#### Priority-aware task seeding

New tasks and resets can be aligned to the currently weakest category instead of being neutral by default.

- What it does: keeps the board biased toward corrective action.
- Why it matters: reduces the chance that the user fills the day with non-relevant work.
- Dependencies: current priority focus and automation logic.

### Supporting infrastructure

- Hidden but UI-relevant

#### Local persistence + hydration gate

The app blocks on hydration before showing the routed interface.

- What it does: restores records and active timer state from Dexie/local storage.
- Why it matters: prevents guidance from rendering on stale or partial state.

#### Event replay + derived state pipeline

Ordered events are replayed into daily summaries, then transformed into scores and higher-order guidance.

- What it does: keeps interpretation deterministic and reconstructable.
- Why it matters: separates raw action capture from meaning.

#### Validation + dedupe rules

The system protects itself from normalized duplicates, invalid money values, invalid health numbers, and duplicate task creation.

- Benefit: reduces accidental state corruption.
- UX implication: some invalid actions are ignored silently, which creates a communication challenge.

#### Transaction rollback safety

When a primary record and its matching event should be written together, the store uses transaction boundaries and rollback behavior.

- Why it matters: keeps the behavioral model and visible records in sync.
- Workflow relevance: trustworthiness of all higher-order outputs.

*Functional Logic / How the System Works*

## The mechanics underneath the interface

The product is easiest to understand when viewed as a five-stage loop: capture, persist, measure, interpret, and recover. The UI is the visible layer of that loop, not the whole system.

### 1. Capture

The user opens the day, adds tasks, logs quick wins, starts timers, updates projects, records money, and adds health or reading signals.

### 2. Persist

Each meaningful action writes a primary record and appends an event so the day can be reconstructed and audited by behavior.

### 3. Measure

Events are replayed into daily summaries: focus minutes, non-negotiable completion, money totals, reading progress, health signals, and more.

### 4. Interpret

Scores, rolling windows, alerts, diagnostics, reviews, forecast, and mentor guidance are derived from the latest state.

### 5. Recover

The system prepares resets, suggests recovery actions, and can seed today’s board from review pressure instead of leaving correction manual.

*Strict dependencies*

### What must happen before something else

- Hydration must complete before the routed interface is shown.
- A timer must start before it can stop and persist a time log.
- Daily manual task creation in the Daily screen expects a current daily log.
- Closing the day requires that the day already exists.

*Out-of-order behavior*

### What can happen independently

- Projects, money entries, books, and health logs can be created without a formal onboarding sequence.
- Some supporting actions can auto-create today if needed through the store layer.
- Review, forecast, and diagnostics are always recomputed from current state after mutations.

*Automated rules*

### What the system handles for the user

- Daily summaries and score derivation.
- Priority focus selection based on weakest category.
- Alert, diagnostic, recommendation, and forecast creation.
- Duplicate-safe task and reset handling.
- Cross-midnight time splitting and comeback-state detection.

*Manual steps*

### What still relies on explicit user intent

- Applying a prepared reset.
- Priming the command loop into today’s tasks.
- Choosing project status changes.
- Logging money, health, and reading progress.
- Closing the day honestly instead of leaving it structurally open.

*Validation and state changes*

### Protective logic that shapes UX

- Duplicate tasks are blocked by normalized title per day.
- Project weights clamp into a supported range.
- Negative money values are rejected.
- Non-finite health inputs are sanitized away.
- Only one active timer can exist at a time.

*Sensitive points*

### Actions that need extra communication clarity

- Task completion appears effectively one-way in the UI.
- Automation is additive and not explicitly undoable.
- Some protected actions fail silently instead of giving user-facing feedback.
- The user may not understand why some flows auto-create “today” while others do not.

*Benefits by Layer*

## What value each layer creates

*User benefits*

### Clearer daily control

- Turns vague pressure into a mission and protected non-negotiables.
- Shows one dominant weakness instead of many competing priorities.
- Connects daily behavior to visible operating consequences.

*Operational benefits*

### Faster recovery from drift

- Diagnostics explain why the system is slipping.
- Prepared resets and command loops shorten the distance between insight and action.
- Project and money activity stay tied to the same operating model.

*System benefits*

### Deterministic interpretation

- Event replay makes scores and guidance reconstructable.
- Validation rules reduce accidental drift in stored state.
- Local-first persistence keeps the app available without a backend dependency.

*Risk-reduction benefits*

### Earlier visibility into failure modes

- Inactivity and weak-category alerts surface issues before the week fully collapses.
- Forecasting turns trend decay into visible near-term risk.
- Review carry-forward reduces the chance that lessons stay trapped in reflection.

Commercial or business value: not explicitly defined in the current source material. If productized, the strongest inferred value is a high-retention personal operations product that differentiates by connecting capture, diagnosis, and recovery in one loop.

- Business value inferred

*System Flow / Lifecycle*

## How the operating cycle moves across a day and beyond

1. **Launch and hydrate**
   The app restores local records and any active timer state before showing the routed product surface.
2. **Open the day**
   The user completes a morning check-in, or the system creates today implicitly when a queued reset or task demands a day container.
3. **Define the operating contract**
   Mission, emotional state, and non-negotiables create the standard the rest of the day will be measured against.
4. **Execute and record proof**
   Tasks, timer blocks, revenue actions, money entries, health logs, and book progress all become evidence of what actually happened.
5. **Derive the current operating state**
   Scores, weak-category pressure, alerts, reviews, diagnostics, and forecast are recomputed from the latest event-backed state.
6. **Close the day honestly**
   The evening review confirms what was actually protected and adds narrative context to the operating record.
7. **Translate interpretation into recovery**
   The system prepares resets, proposes next actions, and offers a command loop to seed tomorrow’s corrective work.
8. **Carry the week and month forward**
   Review snapshots compare periods, flag wins or decline, and influence which category receives the next round of pressure.

*Information Architecture / Major Sections*

## How the product is structurally divided today

The present IA is organized around operating mode, not around pure data types. This makes sense for a personal ops product, but it also concentrates many advanced ideas into the “More” area.

*Current section*

### Dashboard

Compressed readiness view: life score, weakest category, alerts, forecast snapshot, project ranking, and reset prompts.

*Current section*

### Daily

Main execution surface for check-in, quick logs, timer, tasks, reset application, command-loop priming, and day closeout.

*Current section*

### Projects

Project stack organized by urgency and income leverage, with lightweight status updates and action seeding.

*Current section*

### Money

Operational money view focused on revenue pressure, cash movement capture, and money-state visibility.

*Current section*

### More

Analysis hub for forecast, diagnostics, reviews, recommendations, automation, books, and health.

*Structural strength*

### Day-centric navigation

The current structure keeps the operating loop legible: read the state, open the day, move projects and money, then inspect the deeper analysis layer.

*Structural pressure point*

### “More” is carrying too many jobs

It currently behaves as analysis console, recovery console, health screen, and knowledge screen at the same time.

*Likely future split*

### History, settings, and detail views

There is no dedicated history, audit, export, or settings layer yet, but the model already justifies those surfaces.

- Future / not implemented

*Key Entities / Objects*

## The main things inside the system and how they relate

*Core record*

### Daily Log

Represents the day’s mission, emotional state, non-negotiables, target, closeout notes, and completion count.

*Core record*

### Task

Represents executable work for a specific day, optionally linked to a project and marked as non-negotiable.

*Core record*

### Project

Represents a meaningful work stream ranked by urgency and income potential, with active, blocked, and shipped states.

*Core record*

### Time Log / Active Timer

Represents focused time, category allocation, and optional project-linked work sessions.

*Core record*

### Money Log

Represents an income, expense, or debt payment entry used for operational money state.

*Core record*

### Book

Represents an active reading object with page totals, current progress, and a daily pace target.

*Core record*

### Health Log

Represents a daily health detail entry with optional sleep, weight, compliance, and note data.

*System object*

### Event

Represents the behavioral history the product replays to build summaries and guidance.

*Derived layer*

### Guidance objects

Includes scores, alerts, nudges, diagnostics, recommendations, forecast, reviews, automation triggers, and XP state.

*Relationship view*

### How the object model fits together

#### Daily Log

Anchors the day and its review state.

#### Task

Belongs to a daily log and may reference a project.

#### Project

Can be linked from tasks and timers.

#### Time Log

Tracks focused work by category and project.

#### Money Log

Feeds money performance and survival pressure.

#### Book

Feeds the knowledge system.

#### Health Log

Feeds physical stability inputs.

#### Event Stream

Canonical behavioral record used for replay and derivation.

#### Daily summaries

Normalize raw events into measurable daily metrics.

#### Scores + reviews

Translate summaries into readiness and trend memory.

#### Diagnostics + forecast

Explain what is failing and what could fail next.

#### Guidance + resets

Return to the user as actions, warnings, and recovery prompts.

In plain language: the system sees what you did, figures out what it means, and uses that to shape your next day.

*Important Notes / Constraints / Risks*

## Where the product is most sensitive

The biggest risks are not visual. They are structural: guidance overlap, unclear attribution, overloaded analysis surfaces, and silent protective logic that may confuse users if not explained cleanly.

*Hierarchy risk*

### Too many “next action” layers

Priority focus, alerts, nudges, diagnostics, recommendations, prepared resets, automation triggers, and command loops all point toward action. Their relationship must stay explicit.

*Navigation risk*

### The “More” section is overloaded

Forecasting, reviews, diagnostics, automation, books, and health all live in one place, mixing analysis with data-entry surfaces.

*State visibility risk*

### Protective logic is sometimes invisible

Duplicates, invalid values, and blocked actions are often ignored safely rather than explained clearly to the user.

*Workflow risk*

### Today is not created consistently across all surfaces

Some flows require an explicit check-in while others can create today implicitly. That distinction needs careful communication.

*Trust risk*

### The language is strong and behaviorally coercive

Terms like collapse, avoidance, and pressure can feel powerful, but they require transparent framing to remain credible and useful.

*Editability risk*

### No clear undo or full history management yet

Money logs, health entries, books, and many actions do not currently expose a rich correction path in the UI.

*Scope ambiguity*

### Books and health may be core or may be satellites

The system includes them today, but the current hierarchy does not prove whether they belong as equal pillars beside Projects and Money.

- Needs product clarification

*Platform evolution risk*

### Multi-user, sync, and settings are undefined

The codebase is clearly single-user local-first today. Any shift toward coaching, team review, or cloud sync changes the IA significantly.

- Not defined in source

*Mobile readability risk*

### Dense, long-form analytical content can become fatiguing

The current conceptual model contains enough detail that mobile layouts need ruthless hierarchy and compression to stay usable.

*Not a Developer Version*

## The simple version in everyday language

This part is written for someone who does not build software. It keeps the idea simple, concrete, and easy to explain back.

*What it is*

### A tool that helps you run your day

Million Dollar Mirror helps you see what you planned, what you really did, and what you need to fix next.

*What it watches*

### Your real life, not just your to-do list

It looks at work, time, money, health, learning, and discipline so you can see where things are slipping.

*How it works*

### You log your day and the app reads the pattern

You check in, do tasks, track focus time, log money and health, and the app turns that into a simple picture of how your day is going.

*What makes it different*

### It does more than store information

Most apps just save notes and tasks. This one also tries to tell you where you are weak and what needs attention first.

*The 5-step loop*

### How the product works in plain English

#### 1. Start the day

You open the day and decide what matters most.

#### 2. Log what happens

You track your tasks, focused work, money, health, and progress.

#### 3. See the pattern

The app looks at your actions and finds the part of life that needs the most help.

#### 4. Get a warning

If you are slipping, the app points it out instead of letting the problem stay hidden.

#### 5. Fix the next move

The app helps you choose the next action so tomorrow is stronger than today.

### What the main screens mean

- Dashboard: shows how you are doing right now.
- Daily: where you run the day.
- Projects: where you manage your bigger work.
- Money: where you track cash activity.
- More: where deeper review and extra tools live.

### One-sentence takeaway

This is a personal control system that helps you catch slippage early, understand what is going wrong, and take the right next step.

In the simplest terms: it helps you stop lying to yourself about your day.

*Final Summary*

## What this system is, and what the reader should now understand

### System takeaway

Million Dollar Mirror is a day-centered, local-first personal operating product. It helps the user track what happened, understand what it means, and decide what to fix next without having to piece everything together by hand.

What makes it different is the full loop: open the day, record what happened, find the weakest area, and turn that diagnosis into a corrective next move.

### Reader takeaway

- This is not a generic productivity tool; it is a behavioral operating model.
- The product is currently single-user, local-first, and strongly focused on repeated daily use.
- Its core strength is the connection between capture, interpretation, and recovery.
- Its main design sensitivity is keeping all guidance layers legible without overwhelming the user.

If this page has done its job, the reader should now understand what the product is, what each major part does, why those parts exist, and how the full system works as one loop.
