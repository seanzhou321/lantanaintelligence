# TDD, BDD, and the Stratified Behavioral Refinement Framework in the Era of AI Coding

**Author:** Yixiong Zhou
**Date:** 2026

---

## Abstract

This paper introduces the **Stratified Behavioral Refinement (SBR)** framework — a new software development lifecycle framework that integrates specification, architecture governance, and testing into a coherent, directed chain designed specifically for autonomous AI code generation. At the centre of SBR is the **trust boundary** — the provability boundary of the test suite, defining the region of system behavior within which autonomous AI generation is trustable. Within this boundary, every behavioral commitment has been formally specified as a failing test before AI generates any implementation; any deviation from the specification is immediately detectable. Outside it, AI generation is unconstrained and unverifiable.

SBR constructs the trust boundary in layers. A three-level SBR test pyramid of unit, component, and contract tests establishes the boundary at every level of implementation, drawing on Gurevich's Abstract State Machine theory as its formal grounding. A BDD Gherkin specification layer, derived from EARS requirements, serves as the ground model and provides the grounding proof that the bounded system realizes the specification in integrated execution. Together they form a verification chain in which no layer of AI-generated code exists without a pre-existing, human-specified failing test at that layer.

The paper traces the intellectual lineage of the framework from Beck's TDD through Cohn's test pyramid, XP's acceptance tests, and North's BDD, and shows how SBR synthesises these practices specifically for the AI coding context. It addresses the generate-then-verify anti-pattern, introduces the two-path model separating test derivation from implementation generation, and applies the trust boundary concept to both greenfield development and bug fixing. The SBR framework directly resolves eight of the sixteen unresolved problems identified in the companion papers, and partially addresses seven more.

---

## Preface

The first two papers in this series established the methodology and structural context for AI-assisted software development. *Waterfall and Agile in the Era of AI Coding* showed that specification quality is the new velocity constraint — AI multiplies speed when the specification is precise, and multiplies chaos when it is not. *Architecture Governance in the Era of AI Coding* showed that AI generation makes structural decisions less visible, not less important, and that explicit, ongoing governance is the only safeguard against structural entropy accumulating at AI speed.

Both papers identified a gap that neither methodology choice nor architecture governance fully closes: the fidelity gap between what was specified and what was implemented. A team can have a precise specification, a sound architecture, and a disciplined governance process, and still end up with AI-generated code that looks correct, passes a cursory review, and fails to realize the actual intent at the level of individual methods, service interactions, or complete user journeys.

This paper closes that gap through the concept of the **trust boundary** — the provability boundary of the test suite, and the governing idea of the SBR framework. The paper builds toward this concept in stages. Parts 1 and 2 establish the true purpose of TDD and BDD, and show why the AI coding era creates both new opportunity and new risk in specification-driven development. Part 3 places SBR in its historical and methodological context, tracing the lineage from XP through BDD and identifying what each prior practice contributed and where each fell short for autonomous AI generation. Part 4 introduces the trust boundary formally and shows how the SBR test pyramid constructs it layer by layer. Part 5 shows how the BDD grounding proof extends verification beyond the trust boundary to the fully assembled system. Part 6 applies the complete framework to a feature development cycle.

The trust boundary is the central contribution of this paper. It also changes how a team reads failure. When AI generation produces a failing test, the failure is not merely a quality gate to clear — it is diagnostic information about the boundary itself. It points either to an implementation that has not satisfied the specification, or to a specification that was not correctly drawn in the first place. These are different problems with different remedies. A team that understands its trust boundary can tell them apart. A team that does not will fix code when it should fix the specification, and fix the specification when it should fix the code.

SBR does not replace TDD or BDD. It gives both practices a precise structural role within a framework whose central purpose is to establish and maintain the trust boundary — the region within which autonomous AI generation is safe, verifiable, and accountable, and within which failure is always a signal worth reading.

This paper is one of three companion papers addressing AI-assisted software development as a layered discipline. *Waterfall and Agile in the Era of AI Coding* addresses methodology — how development process and sprint structure must adapt when AI accelerates implementation. *Architecture Governance in the Era of AI Coding* addresses structure — how architectural decisions are recorded, enforced, and kept coherent at AI generation speed. *TDD, BDD, and the Stratified Behavioral Refinement Framework* addresses verification — how behavior is specified and how the trust boundary that makes AI generation trustable is built and maintained. The unifying principle across all three is that specification precedes generation, architecture constrains structure, and tests define the boundary of trust. This paper is written primarily for software engineers and technical leads who will implement the SBR framework in practice.

---

## Part 1 — The True Intention of TDD and BDD

### Test-Driven Development: A Design Discipline, Not a Testing Strategy

TDD is often understood primarily as a testing strategy, but its deeper purpose is design — it uses tests as the forcing mechanism [1].

The Red-Green-Refactor cycle is the most familiar expression of this discipline:

1. **Red** — Write a failing test that describes exactly one behavior you intend to implement. The test must fail for the right reason — because the behavior does not yet exist, not because of a configuration error or a poorly written test.
2. **Green** — Write the minimum code necessary to make the test pass. Resist the urge to generalize or optimize prematurely.
3. **Refactor** — Improve the structure of both the code and the test without changing behavior. The passing test suite is your safety net.

The discipline is in the first step. Forcing yourself to write the test first requires you to decide what a function is called, what it accepts, what it returns, and how it fails — before you write a single line of business logic. This is the caller's perspective, and it consistently produces cleaner, more usable interfaces than writing implementation first.

The original insight, articulated by Kent Beck, is that TDD's value is not the tests themselves — it is the *thinking* the tests force you to do before you write anything that runs [1]. That thinking requires a cognitive inversion that most practitioners find unnatural.

Rather than asking
   "what do I want this code to do,"
TDD asks
   "how will I know when it does not do it?"

The difference is not semantic. The first question produces an implementation. The second produces a falsifiable specification. In human-paced development, a skilled engineer asking the first question can still produce trustworthy software — tacit judgment fills the gaps between the intent and the test. In the AI coding era, that tacit judgment is absent. The generation agent has no intent to draw on — only the specification it is given. With AI generation, this inversion is not optional — it is the prerequisite.

A codebase built with TDD discipline has high coverage as a side effect, not as a goal. The goal is a design that emerged from continuous small clarifications of intent.

### Behavior-Driven Development: An Executable Specification Language

BDD, pioneered by Dan North, extends TDD's discipline to the boundary between the problem domain and the solution domain [2]. Its core insight is that requirements, tests, and documentation should all be the same artifact — readable by engineers, product owners, and non-technical stakeholders alike.

The canonical BDD format, Gherkin, expresses behavior in Given/When/Then:

```gherkin
Feature: User authentication

  Scenario: Successful login with valid credentials
    Given a registered user with email "user@example.com"
    When the user submits the login form with correct credentials
    Then the user should be redirected to their dashboard
    And a valid session token should be issued

  Scenario: Failed login with incorrect password
    Given a registered user with email "user@example.com"
    When the user submits the login form with an incorrect password
    Then the user should see an authentication error message
    And no session token should be issued
```

BDD's true purpose is to eliminate ambiguity at the requirements boundary. A scenario written in Given/When/Then cannot be vague without becoming obviously incoherent. The discipline of writing it forces agreement on the nouns, verbs, and outcomes of a feature before any code exists.

BDD scenarios are not documentation that ages. They are living specifications that either pass or fail against the running system. A passing BDD suite is the evidence that the system behaves as specified.

### The Unifying Principle

TDD and BDD share one underlying principle: specification precedes implementation. A specification written before implementation cannot be contaminated by the implementation's assumptions — it represents intent, not behavior that already exists. The gap between the two is where bugs and misdirected implementations live.

In the AI coding era, this principle becomes the primary defense against generate-then-verify drift — the dangerous pattern where AI generates the implementation and tests are written afterward to confirm what the code already does. Generate-then-verify produces tests that validate the implementation's assumptions rather than the specification's intent [17]. SBR is the structural answer to that problem.

---

## Part 2 — The AI Acceleration Opportunity

### What AI Does Well in the Specification-Verification Cycle

AI code generation tools are genuinely powerful accelerators — but only when inserted at the right points in the cycle. Their strengths map almost perfectly onto the mechanical, high-friction steps that historically caused developers to skip or defer TDD and BDD work:

The overhead of writing tests first has historically ranged from 15% to 50% of initial implementation cost — a well-documented barrier to consistent TDD adoption [13][14]. But this framing misses an important nuance: human engineers writing code without pre-written tests are not merely being lazy. They are reviewing and interpreting the specification during implementation, and post-hoc tests capture the design decisions made in that process [14]. AI coding removes this implicit review entirely. The AI has no implementation experience to draw on, no specification intuition to apply, and no design judgment to exercise during generation. Without pre-written tests, the AI has no ground truth at all.

AI can generate test file structures, draft Gherkin scenarios from a feature description, produce step definition stubs, and suggest edge cases that human engineers might miss — capabilities that remove the primary mechanical friction that held TDD and BDD adoption back [15][16]. This is the tailwind that the AI era provides: **the mechanical barrier to specification-first discipline has collapsed.**

### The Generate-Then-Verify Anti-Pattern

One of the more consequential failure modes in AI-assisted development is not bad code — it is a false sense of quality. When a developer asks AI to implement a feature and then asks AI to write tests for that implementation, the result is a test suite that achieves high coverage while testing the wrong thing. The tests confirm the implementation's assumptions rather than verifying the specification's intent — a pattern that produces what practitioners call the false confidence problem: everything is green, but the bugs are still there, now protected by tests [17].

Generate-then-verify is insidious because it looks like good engineering. The coverage numbers are high. The tests pass. The code review shows a responsible-looking suite. What it does not show is that the tests were designed to pass the code that exists rather than to verify the behavior that was intended. It produces implementation first and then writes a specification that matches — the inverse of what a specification is for.

### Why SBR Is the Structural Answer

The Stratified Behavioral Refinement framework prevents generate-then-verify drift by constraining AI code generation inside a trust boundary. The trust boundary is fully defined by the specification and design decisions that precede generation: the failing tests at every pyramid level, the BDD scenarios, and the implementation skeleton. Everything inside the boundary is safe for autonomous generation. Everything outside it requires human judgment.

The specification at each level is the failing test — the Red in the Red-Green cycle. The failing test is the input to AI generation; AI generates the implementation that turns it Green. The test, drafted against the human-approved specification and reviewed before generation begins, is the guarantee that the specification preceded the implementation. AI plays the role of the implementation engine; the human owns the specification that defines what Green means.

This is not an additional process burden on top of development. It is development, restructured so that the specification work that was always required happens in the right order and in an executable form.

This practice has deep theoretical roots. Dr. Yuri Gurevich of Microsoft Research developed Abstract State Machine (ASM) theory to provide the mathematical foundations of software specification — proving that any algorithm can be fully and precisely described as a sequence of state transitions from a ground model down through successive refinements to executable implementation [4][5]. The SBR framework is the practical expression of that theoretical chain: each level of the test pyramid refines the level above it, and the BDD specification layer is the ground model made executable.

The cost structure of this workflow reflects a genuine shift in where human effort concentrates. Since AI drafts test structures, Gherkin scenarios, and step definition stubs under human supervision, the human effort of test authorship moves from production to review. The human's role is to judge whether an AI-drafted test is a genuine boundary marker — a faster and more tractable task than authoring tests from scratch, but one that demands the same specification clarity as the original TDD discipline.

---

## Part 3 — Where SBR Sits in the SDLC

### TDD, XP's Acceptance Tests, BDD, and the AI Era

Beck's TDD was always an implementation-level practice. Its discipline of specifying expected behavior before writing the implementation operates at the level of method contracts, class design, and internal structure. Beck never claimed TDD replaced the need for requirements or higher-level design decisions [1][21]. The emergent design philosophy, as he has since clarified, applies to how internal structure develops incrementally through the refactoring cycle — not to the absence of specification upstream.

XP had its own specification layer, separate from unit tests: user stories written collaboratively with an on-site customer, and acceptance tests that the customer and testers wrote together to verify that the system delivered what was promised [22]. This was a deliberate replacement of heavyweight requirements documents with something lighter, more conversational, and more responsive to change [22]. The customer was present to answer questions, clarify stories, and validate behavior continuously. Specification in XP was not absent — it was embedded in conversation and in the ongoing relationship between the team and accessible customers.

Dan North observed a gap that persisted even within this model. Unit tests, operating at the implementation level, were written in developer language and structured around technical concerns. Business stakeholders could not read them, validate them, or use them to verify that the system was delivering the right behavior [2]. North's insight, which led to BDD and Gherkin, was that the specification layer and the test layer needed to converge into a single artifact — one readable by both business and technical participants, and executable against the running system [2][23]. BDD was not a replacement for TDD or for XP's acceptance tests. It was a more precise, more structured form of the same specification intent, expressed in a language that closed the communication gap between requirements and implementation [23].

In the AI coding era, the conversational specification model that XP relied on faces a structural problem that has nothing to do with XP's values. AI can participate in the specification conversation — drafting user stories, surfacing ambiguities, summarizing discussion, and proposing scenario structures. What AI cannot do is bear responsibility for the outcome. A specification decision made through conversation must pass through human judgment and receive explicit approval before it becomes an executable requirement, a design commitment, or a test that constrains generation [14]. Allowing AI to carry conversational output directly into specification without that approval gate is the same category of risk as generate-then-verify: the system moves fast, looks coherent, and encodes assumptions that no human consciously endorsed.

SBR's response is to bring specification upstream and make it explicit before generation begins — not by returning to heavyweight waterfall documents, but by requiring that BDD Gherkin scenarios be written and committed before architecture begins, and that the full test pyramid be specified and confirmed red before AI generates any implementation. This is the lineage completed: from XP's acceptance tests, through North's Gherkin specification layer, to a workflow in which that specification layer anchors everything that follows.

### SBR as a New SDLC Framework

SBR is a framework that spans the entire software development lifecycle, from requirements through deployment. Its function is to assign each prior practice a precise role in a directed chain, specify the handoff between them, and insert AI generation at the point where it can contribute autonomously without introducing drift.

SBR integrates five practices: EARS requirements, spec-driven methodology, architecture governance, TDD, and BDD. Each addressed a real problem in its time and each remains valid in its own domain. What was missing was a framework that connected them into a single coherent chain, one where each layer fed the next with precision, and where AI coding was woven into the chain at the right level rather than grafted onto it as an afterthought.

SBR provides that connection. The companion papers are constituent layers within SBR, each governing a different level of the same chain:

*Waterfall and Agile in the Era of AI Coding* governs the upstream layer — how EARS requirements are produced, how BDD Gherkin scenarios are derived from them, and why upfront behavioral specification is the quality constraint that determines everything downstream.

*Architecture Governance in the Era of AI Coding* governs the structural layer — how architectural decisions are recorded as ADRs, how the implementation skeleton embodies those decisions in executable form, and how fitness functions verify structural integrity alongside behavioral correctness.

This paper governs the verification and generation layer — how the SBR test pyramid specifies behavior at every level of implementation, how AI generation is constrained by pre-existing failing tests, and how the BDD Gherkin suite provides the grounding proof that closes the chain.

The governing invariant runs across all three layers: **specification and design precede generation at every level.** No architectural decision is left implicit. No implementation begins without a failing test. No AI generation proceeds without a bounded, human-specified problem space.

### The Twelve-Step Sequence

The twelve steps below make this chain concrete. They are not a waterfall phase list — they are the ordered handoffs within a single feature cycle, applicable equally to new features and to extensions on existing code.

```
Specification phase (iterative — phases overlap and feed each other):
  1. BDD Gherkin scenarios written       ← from EARS feature specification
  2. Architecture design completed       ← system structure, components, boundaries
  3. Implementation skeleton created     ← packages, interfaces, API contracts, method stubs

  [steps 4–7 proceed in parallel once skeleton exists]
  4. TDD unit tests written              ← expected behavior of each method (L1)
  5. Component tests designed            ← expected behavior of each microservice (L2)
  6. Contract tests written              ← service boundary interface expectations (L3)
  7. Step definitions wired              ← Gherkin scenarios connected to test engine

  8. Full trust boundary confirmed red   ← all tests red before generation begins

Generation and verification phase:
  9. AI generates implementations        ← within trust boundary (L1, L2, L3)
 10. Passing pyramid tests confirmed     ← all three levels green

Specification verification phase (BDD layer):
 11. Full BDD Gherkin suite run          ← grounding proof against assembled system
 12. Smoke tests run on deployment       ← curated BDD subset confirms correct deployment
```

---

## Part 4 — TDD as the Trust Boundary for Autonomous AI Coding

### The Trust Boundary

A test is not merely a check. It is a formal behavioral statement — a human-specified, human-approved declaration of what the system shall do in a defined situation. When an AI-generated implementation passes that test, its correctness in that situation is not assumed or hoped for — it is derived from the specification. The test is the axiom; the passing implementation is the theorem.

The **trust boundary** is the provability boundary of the test suite. It is the region of the system's behavior within which correctness has been established as a candidate theorem — behavior that satisfies all test axioms and cannot be falsified by the current test suite. Within this region, autonomous AI generation is trustable — any deviation from the specified behavior produces an immediately visible failing test.

Outside the trust boundary, the system may behave — but its behavior is unproven. Two distinct conditions produce unproven behavior, and they require different responses:

**Undetected incorrectness** — a behavior that should have been specified was not. A test that should exist is missing or insufficiently precise. An incorrect implementation exists in this gap, but the system has no means to detect it. This is the classical bug: wrong behavior that a more complete specification would have caught and prevented.

**Undefined behavior** — a behavior that was never specified at all. No test is missing because none was ever intended. The system's behavior here is simply unspecified — the formal system has nothing to say about it. Whether the behavior is desirable or not is a question the test suite cannot answer, because no answer was ever written down.

**Misspecified behavior** — a behavior that was specified, but specified incorrectly. A test exists but does not correctly express the intended behavior — it passes or fails for the wrong reason. The boundary marker is present but placed in the wrong location. This is not a missing specification; it is a flawed one.

All three conditions produce the same symptom in production: unexpected behavior. The trust boundary changes differently in each case — a fact that directly shapes how bug fixes are applied.

A bug fix for undetected incorrectness adds or amends tests to cover the gap — new or corrected statements are added to the formal system and the boundary becomes more complete. A bug fix for undefined behavior introduces new tests for previously unspecified territory — new statements are added to the formal system and the boundary is extended. A bug fix for misspecified behavior rewrites an incorrectly drawn test — the boundary is altered to reflect the correct specification.

| Specification condition | Behavior condition | Trust boundary effect |
|---|---|---|
| Incomplete specification | Undetected incorrectness | Boundary becomes more complete |
| Undefined specification | Undefined behavior | Boundary is extended |
| Misspecified specification | Misspecified behavior | Boundary is altered |

### The SBR Test Pyramid and the Stratified Trust Boundary

Beck's TDD and Cohn's test pyramid were introduced independently. TDD (*Test-Driven Development: By Example*, 2002) was a design discipline operating at the unit level — methods, classes, and small modules. The test pyramid (*Succeeding with Agile*, 2009) was an economic and portfolio argument: unit tests are fast and cheap, UI-level tests are brittle and slow, and a healthy test suite should be weighted accordingly [8]. Cohn was describing how to *distribute* tests across levels, not how to *drive design* at each level. The two ideas were complementary but distinct. The industry largely treated them that way — TDD for implementation discipline, the pyramid for portfolio strategy.

SBR brings them together, but the result is conceptually more than either. The SBR test pyramid is more than a test portfolio in Cohn's sense. Fundamentally, it is a stratified trust boundary: a multi-level Abstract State Machine in which each level is independently specifiable, and the tests at that level define the trust boundary for that level of abstraction. SBR extends the test-first discipline to every level of the refinement chain — a direction originated from ATDD [24][25] practices.

Software systems are designed at multiple levels of abstraction simultaneously — methods, service assemblies, and service boundaries are each independent design decisions, each capable of being wrong independently of the others. A trust boundary drawn only at one level leaves all other levels unguarded. In the AI coding context this is not a theoretical concern: AI-generated code can be wrong at any level, and each level's trust boundary must be established before AI generates anything at that level. SBR formalizes this as a refinement chain grounded in Gurevich's ASM theory [4][5]: the BDD specification layer is the ground model; each level of the test pyramid is a formal refinement step that draws a distinct layer of the trust boundary. The full treatment is in Appendix D.

The SBR test pyramid composes these three levels into a coherent structure, all code-level, all driven by the test-first discipline, all run continuously during implementation [8]:

```
         /\
        /L3\         ← Integration / Contract tests
       /----\
      /  L2  \       ← Component tests
     /--------\
    /    L1    \     ← Unit tests
   /------------\
```

| Level | Test Type | Trust Boundary Drawn | Primary Tools |
|---|---|---|---|
| L1 | Unit tests | Method contracts — what each method accepts, returns, and how it fails | JUnit, pytest, Go test, RSpec, etc. |
| L2 | Component tests | Service assembly — how a microservice's internal logic composes and responds | Standard test framework (BDD thinking informs design, not tooling) |
| L3 | Integration / Contract tests | Service boundary contracts — what each service promises to its consumers | Pact, Spring Cloud Contract |

Each level is necessary because the boundary it draws cannot be drawn at any other level. A method that honors its contract (L1 proven) does not guarantee correct service assembly (L2 unproven). Correct service assembly does not guarantee correct interface contracts (L3 unproven). The levels are not redundant — they are independent layers of the trust boundary, each covering territory the others cannot reach.

Before a feature begins, no behavioral statements exist for it — the formal system is silent about its behavior. As specifications are manifested at each pyramid level as tests, new falsifiable statements are added. When all levels pass and the BDD suite confirms grounding, the full set of statements for the feature has been established and verified.

### A Note on Component Tests

Component tests occupy a position in the test pyramid that is frequently unnamed or inconsistently named, which is why they are often confused with either unit tests or integration tests. In a microservice application, a component test spans the full internal stack of a single service, from the API controller through the service layer to the repository layer, with all external dependencies mocked or stubbed. The database driver is mocked. Downstream service clients are stubbed. But the routing, the controller logic, the service logic, the domain model, and the repository interface are all real, exercised code.

These are also called subcutaneous tests (running just below the API surface), service tests (in the microservices community), or slice tests (in the Spring Boot ecosystem) [9]. The name matters less than the scope: one service, all internal layers, all external I/O mocked.

Component tests are written in the team's standard test framework, not in Gherkin. But BDD's Given/When/Then thinking is a valuable design discipline at this level — it forces the developer to define the expected behavior before writing the test code. The mindset is BDD; the tooling is the same unit test framework used at L1. *(The formal basis for Given/When/Then as a specification structure is covered in Appendix D, §D.3.)*

### AI Generation Within the Trust Boundary

With the trust boundary established at all three levels, AI generation operates within a precisely bounded problem space. The boundary is not a description of what AI should do — it defines the extent to which humans can trust what AI has done. Any implementation that deviates from the specified behavior at any level produces a failing test immediately.

At **L1**, the trust boundary covers method contracts. The input to AI is a failing unit test with a precise method signature and assertion. AI generates the method body. It has no latitude to make design decisions — only implementation decisions within the contract the test specifies. If the generated implementation is wrong, the test fails.

At **L2**, the trust boundary covers service assembly. The input to AI is a failing component test that specifies the service's expected behavior at its API boundary, with mocks defining what external dependencies shall receive and return. AI generates the service wiring — the controller, service class, and repository calls. If the assembly is wrong, the component test fails.

At **L3**, the trust boundary covers service interface contracts. The input to AI is a consumer-driven contract that defines what each service expects from its providers [10][11]. AI generates the adapter code and interface implementations. If a contract is violated, the contract test fails.

Once all three levels are green, the trust boundary covers the full implementation. The BDD specification layer then runs — not as a further layer of the trust boundary, but as the grounding proof that the bounded system realizes the specification it was built from. This is covered in Part 5.

The result is a system where every line of AI-generated code was produced within a human-specified, human-reviewed trust boundary. The generate-then-verify anti-pattern has no natural entry point in this structure: there is no point at which AI generates code without a pre-existing failing test defining the boundary it must satisfy.

### Hypothesis Testing as the Discipline That Keeps the Boundary Honest

The trust boundary is only as strong as the tests that draw it. A test that passes for the wrong reasons — one that does not genuinely detect the absence of the behavior it claims to specify — is a false boundary marker. It gives the appearance of a proven region while leaving the implementation unverified.

This is where TDD's hypothesis testing discipline becomes essential. Every test is a hypothesis: *the system shall behave this way.* For the hypothesis to be genuine, it must be falsifiable — the test must be capable of failing when the behavior is absent. The test must be red before the implementation exists. This is TDD's equivalent of Karl Popper's falsification requirement: a hypothesis that cannot be proven false is not a scientific hypothesis [12]. A test that cannot fail is not a boundary marker — it is an illusion of one.

When a test fails after AI generation, there are two hypotheses to investigate:

**Hypothesis 1 — The implementation is wrong.** The AI-generated code does not satisfy the specified behavior. The boundary is real; the implementation has not reached it. Fix the implementation.

**Hypothesis 2 — The test specification is wrong.** The test does not correctly express the intended behavior. The boundary marker is misplaced. The right response is not to fix the code but to revisit the specification and redraw the boundary correctly.

This distinction reveals something structurally important about the SBR workflow. Tests and implementations are derived from the same design, but they must travel separate paths and be produced by separate agents:

**Path 1 — Design to tests:** behavioral specifications derived from the design, written as failing tests, encoding *what* the system shall do at each level. Human-approved before generation begins.

**Path 2 — Design to implementation:** design intent communicated to the generation agent through annotations on the skeleton — method-level comments, class-level documentation, package or service README files, references to ADRs. These encode *how* the system shall be structured. The generation agent reads both the failing tests and these annotations, producing implementations that satisfy the tests within the design constraints.

Keeping these paths separate is not optional. An agent that writes tests and then generates implementation against them has contaminated both paths with the same assumptions — and the falsifiability property of the tests is lost. The tests must be written without knowledge of the implementation; the implementation agent must receive design context through a channel that is independent of the tests.

*The practical guidance for constructing design annotations at each level of the skeleton is covered in the workflow section.*

A scientist who dismisses every failed experiment as a design flaw, without considering the theory, will make slow progress. A developer who treats every failing test purely as a code bug, without considering whether the boundary was correctly drawn, risks building a trust boundary that looks complete but is not.

### Boundary Completeness: The Requirements Traceability Matrix

The hypothesis testing discipline addresses test quality — whether each test is a genuine, falsifiable boundary marker. Quality per test is necessary but not sufficient for trust boundary integrity. A test suite composed entirely of strong, falsifiable tests can still leave significant requirements uncovered. The second property the trust boundary requires is completeness: every specified requirement in current scope maps to at least one BDD scenario, and every BDD scenario maps to pyramid tests at the levels its architecture requires.

**Requirement coverage** is the correct measure of boundary completeness: every EARS requirement currently in scope maps to a BDD scenario, and every BDD scenario maps to pyramid tests at the levels the design requires. The **Requirements Traceability Matrix (RTM)** is the instrument for tracking this. In the SBR context it serves two related purposes: it establishes the traceability chain from each EARS requirement through its BDD scenarios to its pyramid tests at every level, and it makes coverage gaps visible as explicit empty cells rather than inferences from test failure. Traceability answers whether every artifact can be anchored to a requirement; coverage answers whether every requirement has been verified. The same table provides both readings.

The popular code coverage measures something different: which lines were executed during the test run. It says nothing about whether those lines correspond to behaviors the requirements specify. A codebase can achieve 90% code coverage with no test that verifies a single specified feature correctly — because tests authored against an existing implementation will exercise that implementation's actual execution paths regardless of whether those paths correspond to anything a user was promised. High code coverage produced through generate-then-verify is exactly this situation: the metric looks healthy; the trust boundary is uncertain.

In the SBR context, the RTM maps each in-scope requirement down through the specification pipeline:

| EARS Req ID | Requirement summary | BDD Scenario(s) | L3 Contract Test(s) | L2 Component Test(s) | L1 Unit Test(s) | Boundary status |
|---|---|---|---|---|---|---|
| REQ-001 | When valid credentials submitted, system shall issue session token | Scenario: Successful login | `AuthService.login(email, password)` → `{token, userId}` | `AuthService`: valid credentials accepted | `validateCredentials()` returns token | Complete — all red ✓ |
| REQ-002 | When invalid password submitted, system shall return AUTH_FAILED | Scenario: Failed login | `AuthService.login(email, password)` → `{error: AUTH_FAILED}` | Missing | `validateCredentials()` returns AUTH_FAILED | Incomplete — L2 gap ✗ |

REQ-002 has a BDD scenario, an L3 contract test, and an L1 unit test. Code coverage on the relevant methods may appear adequate. The L2 component test is missing — the service-level trust boundary has not been drawn for this requirement. That gap is invisible to code coverage and visible in the RTM. Both requirements carry an L3 contract because the login feature crosses a frontend-to-backend service boundary, and the RPC interface contract must be specified regardless of whether the internal service assembly is fully covered.

A requirement with no BDD scenario is a requirement entirely outside the trust boundary. A requirement with a BDD scenario but no pyramid tests has a system-level boundary marker without a verification chain beneath it. Both are gaps; they require different remedies. The RTM makes both visible before AI generation begins.

The RTM is not a heavyweight documentation artifact. In practice it is a lightweight table maintained alongside the `.feature` files in version control, updated as requirements and tests evolve. Its purpose is a single structural claim: every populated cell at every required level is evidence that the corresponding behavioral commitment has been formally specified; every empty cell is an explicit, visible gap.

---

### Summary: Trust Boundary Integrity

Part 4 has addressed two distinct properties that together determine whether a trust boundary can be relied upon.

The first is **quality** — whether each individual test is an effective boundary marker. A test that cannot fail when the behavior is absent is a false boundary marker. The hypothesis testing discipline governs this property: every test is a falsifiable hypothesis, confirmed red before generation begins. The two-path model — tests derived from design independently of implementation — preserves the independence that makes falsifiability possible. When a test fails after AI generation, it signals either that the implementation has not satisfied the specification, or that the specification was incorrectly drawn. Distinguishing the two requires judgment — from a human reviewer or a reliable AI agent acting under human oversight.

The second is **completeness** — whether the test suite covers all in-scope requirements at every required pyramid level. A set of high-quality tests can still leave requirements entirely unverified. The RTM governs this property: it maps each EARS requirement to its BDD scenarios and pyramid tests, making gaps visible as explicit empty cells rather than silent absences. Tracking test failure rates by level and by sprint complements the RTM — a team whose L2 failures persist after L1 failures have resolved has a component specification gap, not a unit implementation problem.

Neither property implies the other, and both are required for a trust boundary that can be relied upon.

---

## Part 5 — BDD as Grounding Proof

### The Grounding Problem

The SBR test pyramid ensures that each level of implementation is correct in isolation — methods honor their contracts, services are correctly assembled, and boundaries honor their interface agreements. But a pyramid of green tests does not guarantee the most important property of a working system: that the complete, assembled system, running in a production-like environment, actually realizes the feature specification that was written in human-readable language at the specification phase.

The gap between "all pyramid tests pass" and "the system works for users" is where many software projects fail. Components that are individually correct can fail to compose correctly. Services that honor their contracts in isolation can produce wrong behavior when assembled with real data and real timing. The specification that seemed unambiguous in Gherkin can turn out to have been interpreted differently by the engineer who wrote the step definitions than by the product owner who wrote the scenario.

A concrete example illustrates how this gap arises. Consider two services, each correctly specified and verified in isolation: Service A is specified to produce a timestamp in UTC format; Service B is specified to accept and process a timestamp. Service A's component tests confirm it produces a correctly formatted UTC timestamp. Service B's component tests, using a stubbed Service A, confirm it processes the timestamp correctly. The contract test confirms that Service A's timestamp format matches what Service B expects. All pyramid levels are green.

At runtime, however, Service B is deployed in a timezone-aware environment that silently interprets the UTC timestamp as local time. Every individual component specification was correct. Every pyramid test passed. Yet, the composition of two correct components produces wrong behavior under real conditions — and no test at L1, L2, or L3 could have detected it, because each operates in controlled isolation.

This is the failure mode the BDD grounding proof is designed to catch. A Gherkin scenario exercising the full user journey through both services in a staging environment surfaces the failure immediately — not because it is more detailed than the component tests, but because it runs the assembled system against real infrastructure and real conditions rather than each component in isolation.

### What Grounding Means

A system is grounded when its behavior, verified against real infrastructure in the actual execution environment, realizes the specification. Until that verification runs, the system remains an unverified assertion. A system whose BDD Gherkin suite passes is a grounded system: its behavior has been demonstrated to correctly realize the specified intent in execution. Every failing scenario is a falsified grounding claim — a specific, executable statement about what the system promised to do and did not do. Unlike a failing unit test, which points to a specific method, a failing Gherkin scenario points to a gap in the specification at the system level — either a behavior that was incorrectly specified, or one that was never specified for the assembled system as a whole.

### The Full BDD Suite vs. Smoke Tests

The BDD specification layer produces two distinct artifacts. The **full Gherkin suite** covers every EARS requirement as one or more scenarios; it runs against a fully assembled system in a staging or production-like environment and serves as the authoritative verification that the specification is realized. Because it is comprehensive, it is suited to the end of a sprint cycle and to significant staging deployments rather than to every commit.

**Smoke tests** are a curated subset of scenarios, typically tagged `@smoke`, that cover the most critical user journeys and answer a narrower question: is the system alive and correctly wired after this deployment? They run on every deployment and are fast by design. The two artifacts serve distinct purposes: smoke tests provide a deployment health signal; the full suite provides specification coverage.

### The EARS to Execution Pipeline

The complete specification pipeline in SBR runs from natural language requirements through executable verification. EARS — the Easy Approach to Requirements Syntax — is the structured natural language notation that anchors the pipeline at the requirements level [3][3a]:

```
EARS requirement
    → Gherkin BDD scenario (BDD specification layer)
        → Architecture design and skeleton
            → Unit test (L1)
                → Component test (L2)
                    → Interface / Contract tests (L3)
                        → Step definition and test engine
                            → AI-generated implementation
                                → Passing pyramid tests
                                    → Passing BDD suite (grounding proof)
```

Each step makes the specification more concrete without losing the intent of the step above it. The EARS requirement captures stakeholder intent in structured natural language. The Gherkin scenario translates that intent into an executable behavioral claim. The architecture design and skeleton embody the structural decisions that bound the implementation space. The unit, component, and contract tests make the claimed behavior verifiable at every level of the implementation. The step definitions and test engine then wire the Gherkin scenarios to the assembled system, making the grounding proof runnable. AI-generated implementation fills the skeleton against the pre-existing, failing tests — and the pipeline closes when both the pyramid and the BDD suite pass.

When both the SBR test pyramid and the BDD specification layer pass, the pipeline has successfully carried the stakeholder's intent from natural language all the way to verified execution. That is grounding.

### When the BDD Suite Fails After the Pyramid Is Green

A BDD suite failure after all pyramid levels pass is one of the most diagnostically valuable events in the SBR workflow. It means:

- Individual methods satisfy their specifications within isolation (L1 green)
- Service assemblies satisfy their specifications within isolation (L2 green)
- Interface contracts satisfy their specifications within isolation (L3 green)
- But the assembled system, running under real conditions, reveals a specification gap that isolated testing could not expose

This pattern points to a gap in the specification itself — an assumption that was implicit in the Gherkin scenario, a precondition that was missing from the Given clause, a timing or ordering dependency between services that none of the pyramid levels could see. The BDD failure is not a code bug — it is specification feedback. The right response is to trace the failure back through the pipeline to find where the intent was lost.

---

## Part 6 — The Complete AI-Accelerated SBR Workflow

### Overview

The SBR workflow is not a replacement for the Agile sprint or the Waterfall specification phase — it is the quality layer that operates within whatever cadence the team uses. The workflow below describes a single feature cycle, which applies equally to new feature development and feature extensions on existing code. The boundary between the two is intentionally blurred: extending an existing feature is simply new feature development within the constraints of an existing codebase, and SBR treats both the same way.

The workflow has three phases. The **specification phase** comprises three streams: BDD scenario writing, architecture design and skeleton building, and SBR test pyramid test writing. These streams are iterative and overlapping. Architecture design may uncover missing BDD scenarios; therefore loop back into BDD scenario writing. SBR test pyramid test writing may reveal both. Until the trust boundary is complete and confirmed red, the streams continue to feed each other. At that point the **generation and verification phase** begins: AI generates implementations within the trust boundary the SBR test pyramid established. The **specification verification phase** then establishes that the system is grounded to its specification.

The workflow in this section progressively builds the trust boundary from the specification layer down through the SBR test pyramid, handing AI a precisely bounded problem space in which to operate. The developer thinks at the level of specification and design; AI works within the trust boundary that those decisions establish. The boundary is the interface between the two roles: everything outside it requires human judgment; everything inside it is safe for autonomous generation.

A practical starting point for SBR is to design and specify what the team currently understands well enough to implement. The trust boundary is then built for that scope — no more and no less. As development progresses through Agile sprint cycles, customer demos surface new understanding and requirements mature. Each iteration extends the design scope, and the trust boundary grows with it. This is the natural rhythm of Agile development applied at the specification level: the boundary is not built all at once, but incrementally, in step with the team's evolving knowledge. What makes this process more tractable than it may appear is AI assistance. Once design decisions are recorded and BDD scenarios are drafted, AI generates test structures and step definition stubs quickly under human review, and implementations against those tests require little effort beyond supervision. The team's attention can therefore remain where it is most needed: understanding requirements clearly enough to design against them.

For feature extensions on existing code, the existing test suite already defines a set of established behavioral statements. New specifications add new statements to that set; AI implements within the extended boundary; and the full suite, old and new tests together, confirms that the extension realizes the new specification without altering the truth value of any existing statement.

The invariant throughout: **specification and design precede generation at every level.** No AI generation begins at any level until the tests that express the specification and design for that level exist and are confirmed red.

Bug fixes follow the same trust boundary logic; see Appendix C.

---

### Phase 1 — BDD Specification (BDD Layer)

**Step 1.1 — Write the feature brief.**
Before any tool is opened, write a plain-language description of the feature: what user need it addresses, what the observable outcome is when it works, and what the key failure modes are. Two paragraphs is sufficient. This is the input to BDD scenario generation.

**Step 1.2 — Generate and review BDD scenarios.**
Using the feature brief as input, prompt an AI tool to draft a set of Gherkin scenarios covering the happy path, failure paths, and edge cases. Review every scenario critically:
- Does each scenario test observable behavior, not implementation detail?
- Does each "Given" represent a realistic and complete precondition?
- Does each "Then" assert a user-observable outcome?
- Are there missing scenarios the AI did not generate?

Revise, add, and delete until the scenario set accurately captures the feature's behavioral contract. The human owns this artifact — AI drafted it; the human finalized it.

**Step 1.3 — Commit the scenarios as a working draft.**
Commit the `.feature` files to version control as a working draft. These scenarios will evolve as architecture design and test writing reveal gaps — committing early makes that evolution visible and traceable.


---

### Phase 2 — Architecture Design

**Step 2.1 — Design the system structure.**
With the BDD scenarios providing a behavioral contract, design the architectural components the feature requires: microservice involvement, new APIs, data flows between services, and repository boundaries. Record architectural decisions as ADRs. This is the architecture governance layer from the companion document.

**Step 2.2 — Create the implementation skeleton.**
Translate the architecture design into code structure: packages, interfaces, class signatures, method stubs, API route declarations, repository interface definitions. The skeleton compiles but contains no business logic — every method returns a zero value, a placeholder error, or a not-implemented exception.

The skeleton is the contract between architecture and implementation. It embodies every structural decision in executable form. SBR testing now has concrete targets to specify behavior against.

**Step 2.3 — Write design annotations for the generation agent.**
For each element of the skeleton, write annotations that communicate design intent to the AI generation agent. These are the inputs to the generation phase that tests alone cannot provide: the architectural rationale, interaction patterns, responsibility boundaries, and constraints that shaped the design decisions.

Tests encode *what* the system shall do. Design annotations encode *how* the system shall be structured. They are separate artifacts that must travel separate paths to separate agents — one agent derives tests from the design, another generates implementation from the tests and the annotations. An agent that does both contaminates the falsifiability of the tests.

Current approaches include method-level docstrings describing algorithm intent and caller context; class-level documentation of single responsibility and design pattern; package or service README files covering the component's role, dependencies, and governing ADR; and direct references to architecture diagrams as authoritative constraints.

*The specific forms of design annotation may evolve as AI coding tools mature. The separation principle should stay: tests must be authored from the design independently of the implementation. The code generation agent then reads both the design annotations and the tests — but must never modify the tests it implements against without human consent.*

---

### Phase 3 — SBR Test Pyramid Specification (L1–L3) and BDD Wiring

Once the skeleton exists, all specification streams become independent and can proceed in parallel: component tests, unit tests, contract tests, and BDD step definition wiring share no ordering dependency. Different engineers or AI-assisted sessions can tackle them simultaneously. The skeleton is the common substrate that makes parallel specification possible.

**Step 3.1 — Write component tests.**
For each microservice involved in the feature, write component tests that specify the service's expected behavior at its API boundary. Mocks define what external dependencies are expected to receive and return. Structure the test scenario using BDD thinking (Given this API request, When the service processes it, Then this response should be returned), but implement it in the team's standard test framework, not a Gherkin runner.

A component test that passes before any implementation is present is most likely not correctly written — it is not detecting the behavior it claims to verify.

**Step 3.2 — Write TDD unit tests.**
For each method in the skeleton, write a failing unit test that specifies the method's expected behavior. Apply the hypothesis testing discipline: the test is the hypothesis, the red result confirms the hypothesis is falsifiable, and the green result after AI generation confirms the implementation is consistent with it.

Each unit test should specify exactly one behavior. If a test can fail for two different reasons, it needs to be split. The test's precision is the constraint that guides AI generation — imprecise tests produce imprecise implementations.

**Step 3.3 — Write contract tests.**
For each service boundary, write consumer-driven contract tests that specify what each service expects from its providers [10][11]. These are L3 tests — they verify the interface contracts that allow services to be developed and deployed independently.

**Step 3.4 — Wire BDD step definitions.**
With the skeleton in place, wire the step definitions with empty stubs that target the skeleton's concrete method signatures and API contracts. The skeleton gives the step definitions real targets to bind to rather than stubs in a vacuum.

**Step 3.5 — Confirm the full trust boundary is red.**
Run the complete test suite: L1 unit tests, L2 component tests, L3 contract tests, and the BDD Gherkin suite. Confirm every test fails for the right reason: because the behavior does not yet exist, not because of a configuration error or a poorly written test. A test that passes before implementation is present is not detecting the behavior it claims to specify.

Before running the test suite, verify that the RTM is complete for the current scope: every in-scope EARS requirement maps to at least one BDD scenario, and every BDD scenario maps to pyramid tests at the levels the design requires. A red test suite with RTM gaps is a trust boundary with holes. Generation may proceed for covered requirements, but uncovered ones must be explicitly deferred to a later cycle or explicitly accepted as out of scope for this one.

This is the compliance and cultural timestamp of the SBR workflow. The full trust boundary — spanning the specification, architecture, and test streams — is now established and confirmed red. Every behavioral commitment has been formally specified before a single line of implementation exists. AI generation is authorized to begin.

---

### Phase 4 — AI Generation

With the full specification and design in place — BDD Gherkin scenarios committed, component tests written, unit tests written, contract tests written, and the implementation skeleton established — the problem space is precisely bounded. This is the moment to hand the work to AI.

AI coding agents are most effective when given the complete picture: the skeleton, the full test suite, the design context, and the freedom to determine their own plan of execution. Constraining AI to one method or one level at a time prevents it from applying its planning capability across the full problem space. A capable AI agent will read the tests, understand the design intent, identify dependencies between components, and choose its own execution order — much as a diligent and competent contracted engineer would.

The specification-first prompt pattern provides the framing. The following is an illustrative example; the specific wording will evolve as AI coding tools mature, but the structural principle should be preserved: full specification and design context must precede the generation request.

*"Here is the implementation skeleton, the full test suite, and the relevant design context for this feature. The tests define the behavioral and design contract at every level. Generate implementations that make the tests pass. Do not modify the tests. Raise any ambiguity, impasse, or best practice suggestion before proceeding — do not work around them silently."*

The last instruction is the most important. An AI that silently works around an ambiguity or makes an undocumented design decision is producing invisible drift. An AI that surfaces the ambiguity for human resolution is behaving as a trustworthy collaborator. The human's role in this phase is not to supervise every step — it is to be available when AI needs judgment that only the human can provide.

When AI suggests changing a test to fit its implementation, treat this as a diagnostic signal worth investigating. The AI may have identified a genuine specification gap, an incomplete design, or an architectural constraint that was not visible in the test alone. These are valuable signals — not defects to be dismissed, and not proposals to be accepted blindly. Understand the cause before deciding.

---

### Phase 5 — Specification Verification (BDD Layer)

**Step 5.1 — Run the full BDD Gherkin suite.**
With all pyramid levels green, run the full BDD Gherkin suite against the fully assembled system in a staging or production-like environment. This is the grounding proof. A passing suite confirms that the specification is realizable — the system, as assembled, does what the users were promised. A failing suite reveals integration failures that the pyramid levels could not detect.

**Step 5.2 — Treat BDD failures as specification signals.**
When BDD scenarios fail after all pyramid tests pass, the failure is almost never a simple bug in one method. It is a specification gap: an unwarranted implicit assumption in the scenario, a missing precondition, or a service ordering dependency that no pyramid level could detect. Investigate by tracing the failure back through the pipeline to find where the intent was lost.

**Step 5.3 — Run smoke tests on deployment.**
When deploying to any environment, run the curated smoke test subset of the BDD suite. This confirms the system is alive and correctly configured without the overhead of the full suite. A smoke test failure signals a deployment problem; it does not imply a specification failure.

**Step 5.4 — Refactor under green.**
With both the pyramid and the BDD suite passing, ask the AI to suggest structural improvements. Accept suggestions only when the full suite remains green after each change. The passing SBR verification chain, pyramid and BDD layer together, is the safety net that makes refactoring safe and AI-assisted refactoring fast.

---

### The Specification-First Prompt Pattern

The framing of every AI generation prompt determines whether you are practicing SBR or generating code with extra steps. The structural rule is simple: **every prompt must reference a pre-existing, confirmed-red test.**

The examples below are illustrative — they show what should be included and what should be excluded in a specification-first prompt. The optimal phrasing will evolve as AI coding tools mature; the structural principle will not.

**What a specification-first prompt must include:**
- The failing test or tests — the behavioral and design contract AI is generating against
- The implementation skeleton or relevant code context — so AI understands the design decisions already made
- An explicit instruction not to modify the tests — reinforcing that the tests are the specification, not a suggestion
- An instruction to surface ambiguity or design conflicts rather than working around them silently

**What a specification-first prompt must not include:**
- A request to write tests alongside the implementation — this is generate-then-verify
- A vague feature description without a test — this gives AI no ground truth
- Permission to modify existing tests to accommodate the implementation

The following contrasting examples illustrate the difference. In the correct example, the test is the input that constrains the generation. In the incorrect example, AI is asked to define both the specification and the implementation simultaneously:

**Correct — specification-first:**
```
I am implementing the validatePasswordReset() method. Here is my failing unit test:

[paste test code]

The test asserts that validatePasswordReset() with an expired token:
- Returns a failure result with error code TOKEN_EXPIRED
- Does not update the user's password
- Does not invalidate the token (expired tokens are self-invalidating)

Implement validatePasswordReset() to make this test pass. 
Do not modify the test. Do not add behavior not specified in the test.
Raise any ambiguity or design conflict before proceeding.
```

**Incorrect — generate-then-verify:**
```
Implement a password reset validation function and write tests for it.
```

The second prompt will produce code and tests that agree with each other. They may have nothing to do with what was actually required. The first prompt produces implementation constrained by a human-specified behavioral and design claim — the only kind of constraint that makes AI generation trustworthy.

---

## Summary: The SBR Framework at a Glance

**The SBR Test Pyramid — implementation verification:**

| Level | What It Tests | Who Specifies | Who Generates | Correctness Claim |
|---|---|---|---|---|
| L3 — Contract | Service boundary behavior | Human (consumer-driven) | AI (adapter code) | Interfaces honor their contracts |
| L2 — Component | Microservice internal logic | Human (BDD thinking, standard framework) | AI (service wiring) | Service assembly is correct |
| L1 — Unit | Method behavioral contract | Human (TDD tests) | AI (method implementation) | Method honors its promise to callers |

**The BDD Specification Layer — specification verification:**

| Artifact | What It Tests | Who Specifies | When It Runs | Grounding Claim |
|---|---|---|---|---|
| Full BDD Gherkin Suite | Complete user journeys | Human (from EARS requirements) | End of sprint / before release | System realizes the specification |
| Smoke Tests | Critical deployment paths | Human (curated BDD subset) | Every deployment | System is alive and correctly configured |

**The Requirements Traceability Matrix — boundary completeness:**

| Instrument | What It Verifies | When It Is Checked |
|---|---|---|
| RTM | Every in-scope requirement maps to BDD scenarios and pyramid tests at the required levels | Before generation begins (Step 3.5) |

**The trust boundary:** The trust boundary is the central concept of SBR. It represents the requirements and design decisions of the system in executable, verifiable form. The SBR practice — through the test pyramid, the BDD grounding proof, and the RTM — ensures the boundary is both effective and complete. A well-maintained trust boundary is the foundation that makes AI coding automation trustable.

---

## Appendix A — NFR Testing: What SBR Can and Cannot Cover

Non-functional requirements (NFRs) are the system qualities that describe how a system performs rather than what it does. Examples include performance, security, scalability, accessibility, maintainability, and compliance. SBR addresses NFRs unevenly, and understanding that boundary is important for building a complete quality strategy.

### NFRs Directly Testable Within SBR

**Performance and response time.** BDD scenarios can include timing assertions — "When a search query is submitted, the system shall return results within 200ms" maps directly to a Gherkin scenario in the BDD specification layer with a timing check. TDD unit benchmarks can verify performance at the method level. These are behavioral NFRs that describe how the system behaves under normal conditions, and they fall well within SBR's scope.

**Security behaviors.** Authentication failure modes, authorization boundary enforcement, input validation rules, and error response handling are all expressible in Given/When/Then at the SBR test pyramid and BDD specification layer levels. "When an unauthenticated request is received, the system shall return 401 with no data in the response body" is an L2 component test. "When a user attempts to access another user's data, the system shall return 403" is a BDD Gherkin scenario. Security behaviors are among the most important NFRs to specify in SBR precisely because AI generation can produce plausible but insecure defaults if security requirements are not expressed as explicit, failing tests.

**Error handling and resilience.** Circuit breaker behavior, retry logic, graceful degradation under dependency failure, and timeout handling are all specifiable as TDD unit tests (L1) or component tests (L2). These NFRs are particularly well-suited to SBR because they are event-driven — they describe the system's response to specific undesirable conditions, which is exactly the EARS unwanted behavior pattern.

### NFRs Requiring Specialized Tooling Alongside SBR

**Scalability and load.** BDD provides the acceptance criteria — "the system shall support 1,000 concurrent users with response times below 500ms" — but the load generation requires specialized tools (k6, Locust, JMeter, Gatling). SBR defines the pass/fail criterion; the load testing framework provides the execution environment. Both are necessary.

**Accessibility.** BDD scenarios can express accessibility requirements — "When a screen reader user navigates to the login form, all input fields shall have accessible labels" — but automated accessibility verification tools (axe, Lighthouse, Pa11y) are needed to check the rendered DOM against WCAG standards. SBR and accessibility tooling are complementary.

### NFRs Outside SBR's Scope

**Maintainability and code quality.** These are structural NFRs, not behavioral ones. Static analysis tools, coupling metrics, cyclomatic complexity thresholds, and test coverage requirements are enforced by architecture fitness functions, as described in the Architecture Governance companion document. SBR does not cover these — fitness functions do.

**Usability.** Usability requires human judgment about the quality of user experience. It cannot be automated at a meaningful level of fidelity. User research, usability testing, and A/B testing are the appropriate verification mechanisms.

**Legal and regulatory compliance.** Compliance is verified through audit, documentation review, and process assessment — not automated testing. SBR test records may form part of a compliance evidence package, but they cannot substitute for the audit process itself.

### The Complete NFR Coverage Strategy

A complete NFR coverage strategy requires all four mechanisms working together:

- **SBR** covers behavioral NFRs across the SBR test pyramid and BDD specification layer — performance, security, error handling, resilience
- **Specialized tooling** (load testing, accessibility scanners) covers performance-at-scale and accessibility
- **Architecture fitness functions** cover structural NFRs — maintainability, coupling, coverage thresholds
- **Audit and process assessment** covers compliance

---

## Appendix B — How the SBR Framework Addresses the Unresolved Problems of Agile/Waterfall and Architecture Governance

*This appendix presents the assessed resolution of all sixteen unresolved problems identified in the two companion documents, organized by problem nature.*

### Group 1 — Specification Fidelity Problems
*SBR directly solves all problems in this group. Specification fidelity is the framework's core purpose.*

**W3 — Agile's JBGE and code-as-documentation practices conflict with AI's need for precise behavioral specification.**
*Directly solved.* BDD scenarios resolve the tension by making behavioral specification and executable documentation the same artifact — produced before code exists.

**W4 — Specification quality has no verification mechanism.**
*Directly solved.* A specification that cannot be expressed as a failing test is not complete. The executable specification is the verification mechanism. The Requirements Traceability Matrix (RTM), introduced in Part 4, operationalizes this claim: it provides an explicit traceability map from each in-scope EARS requirement to its BDD scenarios and pyramid tests at every required level. RTM completeness is the structural evidence that the trust boundary covers every requirement currently in scope.

**W5 — Formal acceptance criteria are not operationalized.**
*Directly solved.* The EARS → Gherkin pipeline operationalizes acceptance criteria as runnable BDD Gherkin scenarios.

**W8 — The wrong thing can be built faster than ever.**
*Directly solved.* BDD Gherkin scenarios surface the wrong-thing signal before delivery. A failing BDD suite stops wrong-thing accumulation.

**G6 — Fitness functions cover structure, not behavior.**
*Directly solved.* SBR covers the behavioral dimension that fitness functions cannot. Together they provide complete correctness coverage.

**G7 — No downstream verification when generated code diverges from specification.**
*Directly solved.* The SBR verification chain — pyramid and BDD layer — is the downstream verification mechanism. Divergence is discovered at the test boundary, not at delivery.

### Group 2 — Process Cadence Problems
*SBR provides automated gates that scale with AI velocity. Human process redesign remains an open question.*

**W7 — Technical review cadence is unspecified.**
*Directly solved.* The SBR test pyramid running on every commit is the automated review mechanism. Human review shifts to specification and test quality.

**W2 — Sprint mechanics become incoherent under AI velocity.**
*Partially solved.* Specification completeness and test coverage replace story points as sprint metrics. The methodology question of sprint sizing remains open.

**W6 — Technical debt accumulates faster than sprints can absorb it.**
*Partially solved.* L2 component tests and fitness functions make debt visible earlier. Sprint prioritization of debt repayment remains a methodology question.

**G3 — Structural entropy accumulates invisibly at AI speed.**
*Partially solved.* L2 tests catch behavioral entropy. Designing component tests with BDD thinking forces architectural choices to be explicit before generation begins.

**G5 — ADR generation cannot keep pace with AI decision-making.**
*Indirectly addressed.* Test failures at L2/L3 are signals that implicit architectural decisions were made. Which passing tests imply undocumented decisions requiring ADRs remains a human judgment call — an open research question for the field.

### Group 3 — Human Uncertainty Problems
*SBR reduces exposure but cannot eliminate the underlying uncertainty.*

**W1 — Requirements uncertainty is not resolved by AI.**
*Partially solved.* BDD Gherkin scenarios surface the specification-to-reality mismatch earlier and cheaper. Requirements uncertainty is reduced; it is not eliminated. The iterative Agile feedback loop remains necessary.

**G4 — Inconsistent pattern application across AI-generated code.**
*Partially solved.* L2 tests catch behavioral inconsistency. Stylistic inconsistency between two correct implementations is not detectable by tests — it requires governance and context management.

**G8 — AI generation review checkpoint lacks a structured workflow.**
*Partially solved.* The test-first discipline provides the structured workflow. Recording the review as a controlled artifact for regulated change control requires additional governance procedure.

### Group 4 — Regulated Domain Problems
*SBR provides behavioral evidence. Full compliance requires the governance and methodology layers alongside it.*

**G1 — Traceability gaps in AI-generated code.**
*Directly solved (behavioral dimension).* The SBR verification chain provides complete behavioral traceability. Structural traceability requires LDRA/Parasoft tooling as described in the Architecture Governance companion document.

**G2 — Safety cases must account for AI generation methods.**
*Partially solved.* SBR test records form a significant part of the evidence base. Documentation of the AI generation process itself requires a separate governed record.

### Summary

The resolutions marked as partially solved or indirectly addressed represent the current frontier of SBR's application. The analyses presented here are preliminary — they identify the direction in which the framework is useful and the boundaries where it reaches its limits, but they do not constitute validated practice. Further research, empirical case studies, and tooling development are needed to mature these resolutions into established practice. A subsequent paper will address these open questions directly.

| Group | Problem | Resolution |
|---|---|---|
| **Specification Fidelity** | W3, W4, W5, W8, G6, G7 | ✅ Directly solved (6 of 6) |
| **Process Cadence** | W7 | ✅ Directly solved |
| | W2, W6, G3 | ✅ Partially solved |
| | G5 | ⚠️ Indirectly addressed |
| **Human Uncertainty** | W1, G4, G8 | ✅ Partially solved |
| **Regulated Domains** | G1 | ✅ Directly solved (behavioral dimension) |
| | G2 | ✅ Partially solved |

The SBR framework is, at its core, a **specification fidelity mechanism**. It is most powerful when the problem is the gap between what was specified and what was implemented — and it directly solves every problem in that category. It is useful but incomplete when the problem is process cadence, human judgment, or regulatory compliance. Understanding this boundary is as important as understanding the framework itself.

---

## Appendix C — Bug Fixes in the SBR Framework

### What a Bug Means in SBR Terms

In the SBR framework, a bug — whether discovered in testing or after delivery — is evidence of a gap in the trust boundary. The system exhibited behavior that was not correctly specified — either because the specification was incomplete (the behavior was never defined), inconsistent (the behavior was defined contradictorily), or because the implementation did not satisfy a specification that existed. In all three cases, the trust boundary failed to catch the problem: a test that should have caught the incorrect behavior either did not exist or was not precise enough to detect it.

This framing has an important consequence: before any code is changed, the nature of the gap must be understood. Fixing the code without understanding where the trust boundary failed produces a local correction that may not address the underlying cause — and risks introducing new failures in adjacent behavior. The SBR approach to bug fixing is therefore root cause first, tests second, implementation third — in that order, without exception.

### Step 1 — Root Cause Analysis

The first step is to ask AI to analyze the failing behavior and identify the root cause. This is diagnostic work, not implementation work. The prompt should be framed accordingly:

*"Here is the observed bug: [description of the incorrect behavior]. Here is the relevant code: [paste]. Here is the existing test suite: [paste]. Analyze the root cause without modifying any code. Map the root cause to one of three categories: missing requirements, architecture design gap, or implementation design gap. Explain your reasoning."*

The three categories determine where the fix must be applied and at which level of the SBR test pyramid:

**Missing requirements** — the behavior was never specified in the BDD scenarios or EARS requirements. The system was never told what to do in this situation. The gap is at the specification layer — the trust boundary was never drawn here at all.

**Architecture design gap** — the behavior was intended but the component structure, service boundary, or interface contract does not support it correctly. The gap is at L2 or L3 — the trust boundary was drawn at the wrong level of the architecture.

**Implementation design gap** — the behavior was specified and the architecture supports it, but the method implementation is incorrect. The gap is at L1 — the trust boundary was drawn correctly but the implementation crossed it without detection because the test was insufficiently precise.

### Step 2 — Write Failing Tests

With the root cause mapped to a category, write the tests that specify the correct behavior at the appropriate level. The tests must fail against the current codebase — this confirms that they are genuinely verifying the missing behavior, not passing accidentally.

**Do not touch the implementation before the tests are written and confirmed red.** A developer who fixes the code first and writes tests afterward is repeating generate-then-verify in a maintenance context. The tests must precede the fix, for precisely the same reason they must precede new feature implementation.

The level at which tests are written depends on the root cause category:

- **Missing requirements** — write a new BDD Gherkin scenario at the specification layer, then propagate down: write component tests (L2) and unit tests (L1) as needed to cover the specific behavior gap at each level
- **Architecture design gap** — write failing component tests (L2) or contract tests (L3) that expose the incorrect service behavior
- **Implementation design gap** — write a failing unit test (L1) that precisely specifies the correct method behavior

### Step 3 — Implement the Fix

With failing tests in place, provide AI with the complete context: the existing codebase, the existing passing test suite, the new failing tests, and the root cause analysis.

*"Here is the root cause analysis: [analysis]. Here are the new failing tests: [paste]. The existing test suite must continue to pass. Implement the minimum fix to make the new tests pass without modifying any existing tests. Raise any ambiguity or design conflict before proceeding — do not work around them silently."*

The constraint that existing tests must continue to pass is the trust boundary enforcing regression safety. A fix that breaks existing tests has introduced a regression — the trust boundary has been violated. AI should flag this rather than silently modify existing tests to accommodate the fix.

### Step 4 — Verify and Investigate Further

When the new tests pass and the existing suite remains green, the fix is structurally sound. The trust boundary has been extended to cover the previously missing behavior. Deploy the fix and verify it in the live system — confirm that the reported bug no longer manifests in the deployed environment and that no regressions are visible to users. This concludes the bug fix.

But the SBR discipline requires one additional step: **review the root cause for hidden problems**. A bug is rarely isolated. If a missing requirement allowed one incorrect behavior, it may have allowed others in adjacent scenarios. If an architecture design gap produced one incorrect service interaction, adjacent interactions may be similarly affected.

The developer should ask:

- Given the root cause just identified, what other behaviors in the system might be similarly affected?
- Are there adjacent BDD scenarios or component tests that should be strengthened to cover the now-visible class of gap?
- Does this root cause suggest a systematic weakness — a category of requirement that was undertested from the start?

This review may produce additional failing tests, which drive additional fixes through the same cycle. The trust boundary expands incrementally with each iteration, becoming more complete with each bug fixed.

### When New Tests Pass But the Bug Persists

If the new tests pass but the reported bug remains visible in the running system, the root cause analysis was incorrect. The tests verify behavior adjacent to — but not identical to — the actual failure point. The fix addressed the wrong gap.

This is valuable information. The developer now knows what the bug is *not* caused by. Restart the analysis with this knowledge explicitly included:

*"The previous root cause analysis identified [root cause]. Tests were written and pass for that behavior. The bug persists. Given that [previous root cause] has been ruled out, analyze the remaining possibilities."*

Each iteration of analysis → test → implement → verify narrows the search space. The cycle is the scientific method applied to debugging: each experiment either confirms or falsifies a hypothesis about the root cause. The failing tests produced in each iteration are not wasted — they permanently strengthen the trust boundary, even when they did not locate the bug.

### The Bug Fix Cycle at a Glance

```
1. AI analyzes root cause          ← diagnostic only, no code changes
2. Root cause mapped to category   ← missing requirements / architecture gap / implementation gap
3. Failing tests written           ← at the level corresponding to the root cause
4. Tests confirmed red             ← verifies tests are genuine, not accidentally passing
5. AI implements the fix           ← within trust boundary, existing tests must stay green
6. All tests green                 ← bug is fixed and regression safety confirmed
7. Verify fix in the live system   ← confirm the bug is resolved in the deployed environment
8. Review for hidden problems      ← extend the trust boundary where adjacent gaps exist
```

**If tests pass but bug persists:** root cause was incorrect → restart Step 1 with accumulated knowledge.

**The invariant:** Tests specify the correct behavior before any code is modified. The trust boundary is never repaired by changing code without first changing the tests.

---

## Appendix D — Formal Grounding of the Trust Boundary

This appendix provides a semi-formal treatment of the theoretical foundations underlying the trust boundary concept introduced in Part 4. It is intended for readers who want to understand the mathematical basis of the claims made in the body of the paper, or who wish to explore the formal literature more deeply. The body of the paper can be read and applied without this material.

---

### D.1 — The Trust Boundary as a Provability Boundary

The trust boundary concept has a precise structural analogue in formal logic.

A **formal axiomatic system** consists of a set of axioms — statements accepted as true without proof — and a set of inference rules that allow new statements (theorems) to be derived from the axioms. A statement is *provable* within the system if it can be derived from the axioms using the inference rules. A statement that cannot be derived is outside the provability boundary of the system — it is neither confirmed nor denied by the system; it is simply not addressed.

In the SBR trust boundary:

- **Tests are axioms.** Each test is a formal behavioral statement: given this precondition, when this action occurs, then this outcome shall hold. The statement is written and approved by a human before any implementation exists.
- **Tests are also theorems relative to the design.** Tests are derived from design decisions upstream — they are theorems of the design system and axioms of the implementation system. A test that does not correctly reflect the design is a flawed axiom, corrupting the formal system it anchors. This is why human review of tests before generation is not overhead — it is the step that establishes the integrity of the axiom set.
- **A passing implementation demonstrates a candidate theorem.** An implementation that passes all tests exhibits behaviors consistent with all axioms — the test suite cannot falsify them. But multiple different implementations may satisfy the same set of tests. The tests do not uniquely determine the implementation; they bound the space of acceptable implementations. A passing implementation is therefore a candidate theorem: consistent with the formal system, not falsified by it, but not necessarily the unique consequence of the axioms. The candidate becomes more constrained — closer to uniquely determined — as more tests are added.
- **The trust boundary is the provability boundary.** The region of behavior covered by passing tests is the region within which correctness has been established as a candidate theorem. Behavior outside this region is neither proven correct nor proven incorrect — it is outside the system entirely.

This structure has a well-known consequence in formal logic. Gödel's first incompleteness theorem (1931) establishes that any consistent formal system sufficiently expressive to encode arithmetic contains true statements that cannot be proven within the system [D1]. The core insight relevant here is that **provability and truth are not the same thing** — a passing test suite does not establish that the system is correct everywhere; it establishes that the system is correct within the region the tests cover. The analogy to Gödel is limited to this structural point. Gödel's incompleteness is intrinsic and ineliminable — it arises from self-referential statements that any sufficiently expressive system necessarily contains. A well designed software test suite shall not contain such pathological statements: behavioral specifications address external system behavior and are not self-referential. The trust boundary's incompleteness is therefore extrinsic and eliminable — it reflects unwritten tests, not unwritable ones. Adding a new test genuinely closes a gap without generating new structural blind spots elsewhere.

The two conditions that produce behavior outside the trust boundary map directly onto this structure:

- **Undetected incorrectness** corresponds to a missing axiom: a statement that should have been in the formal system but was not included. The implementation is incorrect in this region, but the system has no means to derive that incorrectness from its existing axioms.
- **Undefined behavior** corresponds to a statement that is genuinely outside the intended scope of the formal system. No axiom is missing — the system was never designed to address this case. The behavior exists but the formal system is silent about it.

---

### D.2 — Gurevich's Abstract State Machine Theory and the Refinement Chain

The SBR pyramid is not an arbitrary layer structure. Each level corresponds to a level of abstraction at which system behavior can be formally specified, and the levels form a refinement chain formalized in Gurevich's Abstract State Machine (ASM) theory. An Abstract State Machine is a formal computational model that describes the behavior of a system as a sequence of state transitions governed by explicit rules [D2][D3].

ASM theory makes four formal claims that directly support SBR's structure:

- **Ground model theorem** — every algorithm can be fully and precisely described as an ASM at any level of abstraction. BDD is the ground model: the most abstract, complete specification of system behavior before any structural decisions exist.
- **Refinement** — any two abstraction levels can be formally related by a refinement proof: the lower level faithfully implements the higher without losing behavioral information. Each level of the test pyramid is a formal refinement step.
- **Level independence** — each abstraction level is independently specifiable and independently complete. Pyramid levels are formally distinct: a method-level specification is not an approximation of a component-level one, nor vice versa.
- **Completeness** — every sequential algorithm can be captured by an ASM (the sequential ASM theorem [D2]). The SBR refinement chain is therefore not an approximation — it is formally complete at every level, provided each refinement step is correctly specified.

The SBR pyramid maps onto the ASM refinement chain as follows:

```
BDD Gherkin Suite       ← Ground model: the most abstract specification,
                           expressed in executable natural language.
                           Corresponds to the system-level ASM.

        ↓ refinement

L3 Contract tests       ← Service boundary ASM: state transitions at
                           the interface level. Specifies what each
                           service promises to its consumers.

        ↓ refinement

L2 Component tests      ← Service assembly ASM: state transitions
                           within a single microservice. Specifies how
                           internal components compose.

        ↓ refinement

L1 Unit tests           ← Method-level ASM: state transitions of
                           individual functions. Specifies the most
                           concrete behavioral contracts.

        ↓ refinement

AI-generated            ← Executable implementation: the concrete
implementation             realization of the refinement chain.
```

Each refinement step is a formal claim: the lower level faithfully implements the higher level. A gap in the trust boundary at any level is a missing refinement step — a claim about the system's behavior that has not been formally established.

When all levels pass — L1, L2, L3, and the BDD Gherkin suite — the full refinement chain is complete. The system's behavior has been formally established from the ground model down to executable implementation. This is what the paper calls grounding: not a metaphor, but a formal property of the refinement chain.

---

### D.3 — Temporal Logic and the Given/When/Then Structure

ASM theory is built on a variation of **temporal logic** — the formal language for reasoning about how system state changes over time [D4][D5].

A temporal logic formula expresses properties of state sequences. The most relevant operators for understanding tests as formal specifications are:

- **Precondition (Given):** a predicate that must hold before an action occurs.
- **Action (When):** a transition that changes the system state.
- **Postcondition (Then):** a predicate that must hold after the transition.

This is precisely the structure of a Given/When/Then scenario in Gherkin, and of a unit test's Arrange/Act/Assert pattern. The structural parallel is not accidental. Both BDD's Gherkin notation and the xUnit test framework's assertion model are informal encodings of the same temporal logic statement: *if the system is in state S and action A occurs, then the system transitions to state S'.*

Formally, a test at any level of the SBR pyramid is a **safety property** in temporal logic terms — a statement of the form "bad state S' is never reached from state S via action A." A passing test suite is therefore a set of verified safety properties over the system's state space. The trust boundary is the region of the state space covered by these verified safety properties.

**Liveness properties** — statements of the form "good state S' is eventually reached" — are harder to encode in standard test frameworks and are generally outside the trust boundary established by the pyramid. They require specification techniques beyond unit and component tests, such as model checking or property-based testing. This is a known limitation of the TDD approach that the SBR framework does not resolve; it is a direction for future work.

---

### D.4 — The Incompleteness of the Trust Boundary and Its Implications

The trust boundary is always incomplete. This is not a failure of the SBR framework — it is a fundamental property of any formal specification system applied to a real-world software system.

Three sources of incompleteness are worth distinguishing:

**Intentional incompleteness** — behaviors that were deliberately left unspecified because they are out of scope, not yet prioritized, or handled by a separate component. The boundary does not extend here, and that is by design.

**Accidental incompleteness** — behaviors that should have been specified but were missed. These are the classical bugs-in-waiting: correct behavior was never declared, so incorrect behavior cannot be detected. The remedy is to extend the trust boundary by writing the missing tests.

**Structural incompleteness** — behaviors that arise from the composition of correctly specified components in ways that no individual test covers. This is the gap that the BDD grounding proof addresses: components that are individually correct at L1, L2, and L3 may compose incorrectly at the system level. The BDD suite is the formal instrument that detects structural incompleteness. The timezone example in Part 5 illustrates this precisely: each component's specification was complete and correct at every pyramid level, yet the assembled system failed because the composition of two correct components produced wrong behavior under real deployment conditions — a gap that only the BDD grounding proof could detect.

The distinction between accidental and structural incompleteness explains why a pyramid of green tests is necessary but not sufficient for a working system. Green pyramid tests establish that each refinement step in the chain is locally correct. The BDD grounding proof establishes that the full chain — from ground model to execution — is globally correct. Both are required. *(See Appendix D, §D.4.)*

---

## References

[1] Beck, Kent. *Test-Driven Development: By Example*. Addison-Wesley, 2002.

[2] North, Dan. "Introducing BDD." *Better Software Magazine*, March 2006. https://dannorth.net/introducing-bdd/

[3] Mavin, Alistair, Philip Wilkinson, Adrian Harwood, and Mark Novak. "Easy Approach to Requirements Syntax (EARS)." *Proceedings of the 17th IEEE International Requirements Engineering Conference (RE 2009)*, Atlanta, GA, August 31–September 4, 2009, pp. 317–322. https://doi.org/10.1109/RE.2009.9

[3a] Mavin, Alistair, Philip Wilkinson, Sarah Gregory, and Eero Uusitalo. "Ten Years of EARS." *IEEE Software* 36:5 (September 2019), 10–14. https://doi.org/10.1109/MS.2019.2921164

[4] Gurevich, Yuri. "Sequential Abstract State Machines Capture Sequential Algorithms." *ACM Transactions on Computational Logic* 1:1 (July 2000), 77–111. https://dl.acm.org/doi/10.1145/343369.343384

[5] Börger, Egon and Robert Stärk. *Abstract State Machines: A Method for High-Level System Design and Analysis*. Springer-Verlag, 2003.

[6] Zhou, Yixiong. *Waterfall and Agile in the Era of AI Coding*. Companion document, 2026.

[7] Zhou, Yixiong. *Architecture Governance in the Era of AI Coding*. Companion document, 2026.

[8] Cohn, Mike. *Succeeding with Agile: Software Development Using Scrum*. Addison-Wesley Professional, 2009.

[9] Fowler, Martin. "SubcutaneousTest." Martin Fowler's Bliki, February 14, 2011. https://martinfowler.com/bliki/SubcutaneousTest.html

[10] Robinson, Ian. "Consumer-Driven Contracts: A Service Evolution Pattern." ThoughtWorks, 2006. https://martinfowler.com/articles/consumerDrivenContracts.html

[11] Pact Foundation. "Pact — Consumer-Driven Contract Testing Framework." https://pact.io/

[12] Popper, Karl. *The Logic of Scientific Discovery*. Hutchinson, 1959.

[13] Nagappan, Nachiappan, E. Michael Maximilien, Thirumalesh Bhat, and Laurie Williams. "Realizing Quality Improvement Through Test Driven Development: Results and Experiences of Four Industrial Teams." *Empirical Software Engineering* 13:3 (2008), 289–302.

[14] Zhou, Yixiong. Personal communication. Senior software engineering practitioner with 20+ years of experience. 2026. *"The overhead of rigorous TDD practice typically ranges from 20% to 50% of initial implementation cost. Human engineers writing code without pre-written tests are not being negligent — they are reviewing and interpreting the specification during implementation, and post-hoc tests capture the design decisions made in that process. AI coding removes this implicit review entirely, fundamentally changing the risk profile of skipping pre-written tests."*

[15] Inflectra Corporation. "Operationalizing BDD Scenarios Through Generative AI." EuroSTAR Conference, May 2024. https://conference.eurostarsoftwaretesting.com/operationalizing-bdd-scenarios-through-generative-ai/

[16] Shah, Nat. "TDD and BDD in the Age of AI: Why AI Agents Demand 100% More Test-First Development." natshah.com, January 3, 2026. https://natshah.com/blog/tdd-bdd-age-ai-why-ai-agents-demand-100-more-test-first-development

[17] Foojay.io. "AI-Driven Testing Best Practices." Foojay — Friends of OpenJDK, May 26, 2025. https://foojay.io/today/ai-driven-testing-best-practices/

[18] Microsoft Engineering Fundamentals Playbook. "Smoke Testing." Microsoft GitHub, 2024. https://microsoft.github.io/code-with-engineering-playbook/automated-testing/smoke-testing/

[19] TestQuality. "How to Integrate Gherkin Testing in Your CI/CD Pipeline." TestQuality Blog, October 21, 2025. https://testquality.com/how-to-integrate-gherkin-testing-in-your-ci-cd-pipeline/

[20] Qodo. "2025 State of AI Code Quality." Qodo Research, June 2025. https://www.qodo.ai/reports/state-of-ai-code-quality/

[21] Beck, Kent. "TDD Isn't Design." *Tidy First? (Substack)*, December 7, 2023. https://tidyfirst.substack.com/p/tdd-isnt-design

[22] Beck, Kent. *Extreme Programming Explained: Embrace Change*, 2nd ed. Addison-Wesley, 2004.

[23] Agile Alliance. "Behavior-Driven Development (BDD)." Agile Alliance Glossary. https://agilealliance.org/glossary/bdd/

[24] Wikipedia. "Test-Driven Development — Acceptance Test-Driven Development." https://en.wikipedia.org/wiki/Test-driven_development

[25] Agile Alliance. "Acceptance Test-Driven Development (ATDD)." Agile Alliance Glossary. https://agilealliance.org/glossary/atdd/

[D1] Gödel, Kurt. "Über formal unentscheidbare Sätze der Principia Mathematica und verwandter Systeme I." *Monatshefte für Mathematik und Physik* 38 (1931), 173–198. English translation: "On Formally Undecidable Propositions of Principia Mathematica and Related Systems." Basic Books, 1962.

[D2] Gurevich, Yuri. "Sequential Abstract State Machines Capture Sequential Algorithms." *ACM Transactions on Computational Logic* 1:1 (July 2000), 77–111. https://dl.acm.org/doi/10.1145/343369.343384 *(also cited as [4] in the main text)*

[D3] Börger, Egon and Robert Stärk. *Abstract State Machines: A Method for High-Level System Design and Analysis*. Springer-Verlag, 2003. *(also cited as [5] in the main text)*

[D4] Pnueli, Amir. "The Temporal Logic of Programs." *Proceedings of the 18th Annual Symposium on Foundations of Computer Science (FOCS 1977)*, 46–57. https://doi.org/10.1109/SFCS.1977.32

[D5] Clarke, Edmund M., Orna Grumberg, and Doron A. Peled. *Model Checking*. MIT Press, 1999.

[D6] Börger, Egon. "The ASM Refinement Method." *Formal Aspects of Computing* 15:2–3 (November 2003), 237–257. https://doi.org/10.1007/s00165-003-0012-7

[D7] Lamport, Leslie. "Specifying Systems: The TLA+ Language and Tools for Hardware and Software Engineers." Addison-Wesley, 2002. *(TLA+ is a practical specification language built on temporal logic, widely used in industry for verifying distributed systems.)*

---

## Acknowledgements

The author thanks two reviewers whose comments materially improved this paper.

Fiona Xinghua Zheng, [title], [organization], reviewed all three papers in this series and provided comprehensive structured feedback. Her observations on this paper led to several specific improvements: her comment on adoption complexity prompted the Part 6 overview paragraph connecting the trust boundary to Agile's iterative cadence; her observation that the framework assumes good tests without explaining how to assess them led to the introduction of the Requirements Traceability Matrix and the Boundary Completeness section in Part 4; her comment that AI's role felt narrowly defined as a code generator produced the revised summary invariant now centered on the trust boundary as the foundation of trustable AI coding automation. Her cross-paper observations on boundaries, the unifying principle, and audience produced the orientation paragraph now appearing in each paper's preface.

Steven Offen, formerly Chief Technology Officer and Software Architect at MIRO Technologies and Tapestry Solutions, reviewed all three papers. His technical depth and architectural experience shaped the verification and governance arguments in important ways. His comments on this paper produced several specific improvements: his observation that TDD's cognitive inversion — from "what do I want this code to do" to "how will I know when it does not" — is genuinely unnatural for most practitioners led to the restructured opening of Part 1; his identification of the trust boundary diagnostic paragraph as the paper's strongest passage prompted its elevation into the Preface; his challenge to the "ground model" framing as an assertion rather than a design principle led to its removal; and his precise readings of the undefined behavior definition, the BDD suite failure bullet list, and the grounding claim each produced more accurate formulations.

Their collective feedback helped transform early drafts into clearer, more concise, and more readable papers.

---

*This document reflects analysis and practice current as of early 2026. The SBR framework is designed to be durable across near-term tool changes — the principles are stable; the specific AI tools and test frameworks are interchangeable.*

*A note on sources: Several citations in this paper reference industry survey reports, practitioner publications, and vendor documentation rather than peer-reviewed academic papers. This reflects the state of the field — empirical research on AI-assisted software development practices is only beginning to appear in peer-reviewed venues, and the most current quantitative data on AI coding velocity, code quality, and team adoption patterns is available primarily through industry research. Where peer-reviewed equivalents exist they have been used; where they do not, the cited sources have been selected for methodological transparency, sample size, and relevance to the specific assertion they support.*
