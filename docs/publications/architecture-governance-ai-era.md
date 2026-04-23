# Architecture Governance in the Era of AI Coding

**Author:** Yixiong Zhou
**Date:** 2026

---

## Abstract

AI generation makes structural decisions less visible, not less important. The governance response is to treat the architectural record as living context that shapes AI generation from the outset — not as documentation written after the fact. At AI generation speed, getting architectural constraints right before generation begins is more than a best practice — it is the primary defense against structural drift that is invisible at commit time and costly to reverse. This paper examines how that principle must be realized across two distinct domains — regulated and safety-critical systems, and software with uncertain or evolving requirements — as AI coding accelerates the pace at which structural decisions become embodied in code. In regulated domains, traceability gaps, safety case evidence requirements, and change control processes face new challenges from AI-generated code that embeds architectural choices without documentation. In commercial software, structural entropy accumulates at AI generation speed, and the Architecture Decision Record (ADR) model cannot keep pace with AI-assisted decision-making. The paper proposes a continuous, specification-anchored governance model that realizes this principle across both domains. Eight unresolved problems are identified and labeled for resolution in the companion papers.

---

## Preface

Architecture governance is the set of practices, decision rights, and review mechanisms that ensure a software system's structure remains coherent, intentional, and aligned with its long-term quality requirements. It operates continuously — not as a one-time activity — as the codebase grows and changes. It is distinct from architecture *design* — the act of making structural decisions — and from architecture *documentation* — the act of recording them. Governance is the ongoing discipline that keeps both of those activities connected to the living codebase.

AI code generation introduces a qualitatively new governance challenge. The challenge is not that AI makes bad structural decisions — it is that AI makes structural decisions silently. The speed of generation outpaces any governance model that depends on periodic review. When a developer writes code manually, the slow pace of the work is itself a forcing function — a constraint that creates space for reflection. Three days of implementation give three days to notice that a module is violating a layer boundary, that an abstraction is wrong, or that a design choice conflicts with a recorded architectural decision. A human developer implements and reviews simultaneously, drawing on tacit knowledge of the system's intent that never appears in any document. This is why human-paced teams can function even when specifications are incomplete — the gaps are filled by the developer's own judgment in the moment. That friction is gone with AI. A structurally problematic module can be generated in ten minutes — and because the code looks syntactically correct and locally coherent, the developer reviewing it may not recognize the structural problem at all. Architectural conformance requires architectural context: knowing not just what the code does, but how it relates to the decisions recorded elsewhere in the system. That context is rarely present when a developer reviews AI-generated code under the pressure of a fast-moving sprint. The result is a new failure mode: structural drift that is invisible at commit time, accumulates sprint by sprint, and surfaces only when the codebase has become difficult to extend, certify, or reason about.

Architecture governance in the AI era must therefore be **explicit** — structural decisions are recognized, stated, recorded, and communicated, not left as invisible defaults in generated code — and **ongoing** — woven continuously into the development cadence rather than treated as a phase, a milestone, or a one-time upfront activity.

How those two principles are realized, however, differs fundamentally between the two primary domains of software development: regulated and safety-critical systems, and systems with uncertain or evolving requirements. Each domain has different drivers, different risk profiles, and different governance traditions. AI reshapes both, but in different directions. In both domains, the governance frameworks that prove durable will be those that place architectural constraints in front of AI generation — not behind it. At AI generation speed, structural drift accumulates faster than any retrospective review process can catch it; getting architectural intent into the generation process the first time is the governing discipline the AI era demands.

This paper addresses the structural layer of AI-assisted development — how architectural decisions are made, recorded, and enforced as AI generation accelerates. Behavioral verification — whether generated code correctly implements specified behavior — is addressed in the companion paper *TDD, BDD, and the Stratified Behavioral Refinement Framework in the Era of AI Coding*.

This paper is one of three companion papers addressing AI-assisted software development as a layered discipline. *Waterfall and Agile in the Era of AI Coding* addresses methodology — how development process and sprint structure must adapt when AI accelerates implementation. *Architecture Governance in the Era of AI Coding* addresses structure — how architectural decisions are recorded, enforced, and kept coherent at AI generation speed. *TDD, BDD, and the Stratified Behavioral Refinement Framework* addresses verification — how behavior is specified and how the trust boundary that makes AI generation trustable is built and maintained. The unifying principle across all three is that specification precedes generation, architecture constrains structure, and tests define the boundary of trust. This paper is written primarily for software architects, technical leads, and engineering managers responsible for structural coherence across a codebase.

---

## Domain 1: Regulated and Safety-Critical Systems

### The Governance Tradition

Regulated and safety-critical domains — medical devices, avionics, automotive control systems, defense, nuclear plant management, financial systems under statutory audit requirements — have long operated under the most rigorous architecture governance frameworks in the industry. The driving force is not engineering preference but legal and regulatory obligation. These domains are governed by standards such as DO-178C (airborne software), IEC 62304 (medical device software), ISO 26262 (automotive functional safety), and NIST SP 800-160 (systems security engineering) [8][9]. Each imposes specific requirements on how architectural decisions are made, documented, reviewed, and traced to requirements and test evidence.

In these domains, architecture governance has historically been:

- **Formal and documented.** Every significant structural decision is recorded in controlled artifacts — architecture description documents, interface control documents, safety cases — that are subject to configuration management and audit.
- **Traceable.** A line of code can be traced upward to the architectural component it belongs to, the requirement it implements, and the test that verifies it. Traceability is not optional; it is a certification prerequisite.
- **Review-gated.** Architecture changes pass through formal review boards — sometimes internal, sometimes with regulatory body involvement — before they are approved for implementation.
- **Change-controlled.** Deviations from the approved architecture are treated as defects unless they pass through a formal change request process. Unauthorized structural changes can invalidate certification evidence.

The overhead of this model has always been substantial. For many regulated organizations, the documentation and review burden represents a significant fraction of total development cost. The governance structure is defensible because the cost of a structural failure — a flight control system that fails under edge conditions, a medical device that delivers incorrect dosing — is catastrophic and potentially fatal.

### How AI Reshapes Governance in Regulated Domains

AI code generation enters regulated domains with a fundamental tension: it can dramatically accelerate the implementation phase, but regulatory frameworks were designed around human-authored code and human-reviewable artifacts. That tension resolves differently depending on whether the organization treats AI as a tool within existing governance or as a reason to evolve the governance framework itself.

**Traceability becomes the central challenge.** *(G1)* AI-generated code introduces a new class of traceability problem. When a human engineer writes a function, the decision to write it that way is at least nominally in the engineer's head and can be reconstructed through review. When an AI generates a function, the structural choices embedded in it — the abstraction boundary chosen, the error handling pattern applied, the interface designed — may reflect neither the system's intended architecture nor any documented decision. Regulators are watching this question with deliberate caution. The FAA's Roadmap for Artificial Intelligence Safety Assurance, published in August 2024, explicitly restated that responsibility for system design rests with the human designer, not the AI [16]. The implication is direct: accountability cannot be delegated to a generative tool regardless of how much of the implementation it produces.

The answer emerging from early adopters in regulated domains is a formalized **AI generation review checkpoint** inserted into the existing change control process. *(G8)* AI-generated code is not committed until a qualified engineer has reviewed it specifically for architectural conformance — not just functional correctness — and that review is recorded as a controlled artifact linked to the relevant architectural component and requirement.

**Architecture conformance tooling becomes essential.** Regulated organizations are beginning to invest in static analysis tools that can verify AI-generated code against the system's documented architectural rules — layer dependencies, interface contracts, module boundaries, naming conventions — automatically, before human review. This is an evolution of existing static analysis practice, but AI generation makes it more urgent because the volume of code requiring conformance checking increases substantially.

**Safety cases must account for AI generation methods.** *(G2)* A safety case is the structured argument, supported by evidence, that a system is acceptably safe for a given application. AI-generated code introduces a new dimension: the evidence base must now include not only test results but documentation of the AI generation process, the human review applied to generated outputs, and the governance controls that ensure generated code conforms to the safety architecture. Certification bodies have begun publishing governance frameworks that signal the regulatory direction for AI in safety-critical contexts. The FAA's 2024 roadmap [16] reinstates human accountability for AI-driven design decisions. EASA's NPA 2025-07 formalizes a comprehensive AI trustworthiness framework requiring human oversight and documented governance for high-risk AI systems in aviation [17]. The FDA's January 2025 draft guidance requires total product lifecycle documentation, change control, and performance monitoring for AI-enabled device software [18]. Taken together, these documents make clear that wherever AI produces outputs in regulated contexts, governance and human accountability are non-negotiable — a principle that extends directly to AI-generated code in safety-critical development.

**The opportunity: AI-assisted traceability generation.** The same AI capability that introduces governance risk also offers a governance opportunity. AI tools can be used to generate draft traceability links between requirements, architectural components, and code — producing the documentation scaffold that human engineers then review and approve rather than author from scratch. The vendor tooling landscape is responding to this challenge with specific, documented steps. LDRA's tool suite automates bidirectional traceability across DO-178C, IEC 62304, ISO 26262, and related standards, integrating conformance checks directly into the commit pipeline [1]. Parasoft has addressed AI-generated code at the standards level, supporting MISRA C:2025 — which formally requires AI-generated code to meet the same compliance obligations as handwritten code — and has extended this into agentic AI workflows that allow AI agents to reason over test results and compliance artifacts under full engineer oversight [2][3]. The governance principle is unchanged: humans are responsible for the accuracy of every traceability artifact. AI accelerates the conformance checking; humans own the record.

**Architectural drift detection at AI speed.** Because AI-generated code can introduce structural inconsistencies faster than periodic architecture reviews can catch them, regulated organizations are beginning to adopt continuous architectural conformance monitoring — automated checks that run on every commit and flag deviations from the documented architecture for immediate human review rather than waiting for the next formal review gate [1][15]. This shifts governance from episodic to continuous without reducing its rigor.

### The Net Effect in Regulated Domains

AI does not reduce the governance burden in regulated domains — it redistributes it. The implementation phase becomes faster and cheaper. The governance, review, and documentation phases do not shrink proportionally; in some respects they grow, because AI-generated code requires additional review disciplines that human-authored code did not. The organizations that manage this well will achieve genuine velocity gains while maintaining certification integrity. The organizations that treat AI as a way to skip governance steps will face certification failures, audit findings, and in the worst cases, safety incidents.

The governance framework that emerges is recognizably continuous with the pre-AI regulated model — formal, traceable, change-controlled — but augmented with AI-specific review checkpoints, conformance tooling, and AI-assisted documentation drafting. The human authority over every governed artifact remains absolute.

---

## Domain 2: Software with Uncertain or Evolving Requirements

### The Governance Tradition

Commercial software, enterprise applications, consumer products, and internal tooling typically operate in an environment where requirements are genuinely uncertain at the outset and evolve continuously in response to user feedback, market conditions, and competitive dynamics. This is the domain Agile was designed for, and architecture governance in this domain has historically been far lighter than in regulated systems.

The dominant governance practice is the **Architecture Decision Record (ADR)**: a short, version-controlled document that records a significant architectural decision, its context, the options considered, and the rationale for the choice made [10][10a]. ADRs live in the repository alongside the code they describe and evolve with the codebase. Supplementary practices include regular architecture review sessions, designated technical leads with structural authority, and code review norms that include architectural conformance as a review criterion alongside functional correctness.

The weakness of this model — even before AI — is that it depends heavily on individual discipline and organizational culture. ADRs are written when engineers recognize that a decision has been made and take the time to record it. Architecture reviews happen when teams prioritize them against sprint delivery pressure. Code review catches structural problems when reviewers have the context and authority to raise them. In practice, all three of these conditions are inconsistently met, and most commercial codebases accumulate structural entropy over time as a result [12][13].

### How AI Reshapes Governance in Evolving Requirements Domains

The AI era intensifies the existing governance weakness in this domain while simultaneously offering new tools to address it.

**Structural entropy accelerates.** *(G3)* The most immediate effect of AI-assisted development in an under-governed codebase is that structural entropy accumulates at the same speed as feature velocity. An engineering team using AI generation ships features substantially faster than a pre-AI team. But each feature may embed architectural decisions that were never deliberately made — invisible precisely because they emerged from generation rather than human judgment. The volume of those implicit decisions per sprint grows in direct proportion to AI-assisted velocity [12][13]. Without governance mechanisms that operate at the same cadence as generation, the codebase becomes architecturally incoherent faster than any previous development model allowed.

**Inconsistent pattern application.** *(G4)* AI tools generate locally coherent code. A module in isolation looks reasonable. But AI has no awareness of the system's intended architectural patterns unless those patterns are explicitly provided in context at generation time. Across a sprint, the same AI tool will generate data access code in three different styles, error handling in two different patterns, and service boundaries at inconsistent levels of granularity — because each generation prompt was isolated from the architectural decisions made in the others [14]. The result is a codebase that is syntactically correct and functionally working but structurally incoherent.

**The specification document as an architecture governance anchor.** The spec-driven development methodologies emerging from the AI era — Amazon Kiro's `design.md` [5], GitHub Spec Kit [6], Thoughtworks' SDD framework [7] — are, in architectural governance terms, a lightweight but explicit architecture record that is present at generation time rather than written after the fact. When a specification includes architectural constraints — layer boundaries, dependency rules, pattern conventions, naming standards — those constraints shape what AI generates. Governance moves upstream — into the specification and design, before implementation begins — rather than being applied retroactively in review.

**ADR generation at AI speed.** *(G5)* The ADR model is compatible with AI-assisted development, but the cadence of decision-making must change. An engineering team using AI generation may make more consequential architectural decisions in a single sprint than a pre-AI team made in a month. The governance practice that emerges in disciplined teams is an **AI generation review protocol** embedded in the pull request process: before a PR is approved, the reviewer explicitly asks whether any architectural decision is embedded in the generated code that should be recorded as an ADR. AI tools can assist by analyzing the diff and flagging potential architectural decisions for human review — identifying new dependencies, new abstractions, new cross-cutting patterns — but the decision to record and the content of the record remain human responsibilities.

ADRs are not replaced by this shift — the format remains the right instrument for recording structural decisions. What changes is how they are produced. At AI generation speed, asking engineers to author ADRs from scratch after each PR is not realistic. The same AI capability that generates the code can draft the ADR: given the PR diff, the review conversation, and the specification context, AI produces a structured decision record that the human reviewer then reviews, amends, and approves before committing. This is directly parallel to AI-assisted traceability generation in regulated domains — AI accelerates the production of the record; the human owns its accuracy. The result is ADR authorship that can match the cadence of AI-assisted decision-making without adding a disproportionate documentation burden to each sprint.

**Architecture fitness functions.** *(G6)* Borrowed from evolutionary architecture thinking [4], fitness functions are automated tests that verify architectural properties — dependency direction, module coupling metrics, layer boundary enforcement, performance characteristics — on every build. In a pre-AI codebase, fitness functions were a best practice adopted by sophisticated teams. In an AI-accelerated codebase, they become a governance necessity, because the volume of generated code that could violate architectural constraints in a single sprint is too large for manual review to catch reliably. Fitness functions are the automated governance layer that scales with AI generation velocity.

**Architecture as context for AI generation.** *(G7)* The most forward-looking governance practice in this domain is the treatment of the architectural record as living context that is explicitly provided to AI tools at generation time. The architectural record includes the ADRs, the design specification, and the structural constraints that govern the codebase — whether those constraints are expressed as fitness functions written in code or as configuration files for code scan tools such as SonarQube or ESLint. An AI generation session that begins with this context will produce architecturally conformant code far more reliably than one that starts from a blank slate. This reframes governance documentation from a record of past decisions into an active input that shapes future generation — the governance artifact and the generation context become one and the same.

### The Net Effect in Evolving Requirements Domains

AI does not eliminate the need for architecture governance in commercial software — it makes governance more urgent and more valuable while also providing new mechanisms to practice it more effectively. The lightweight governance tradition of this domain — ADRs, technical leads, code review norms — remains the most defensible starting point available, though more effective governance models may emerge as specification-anchored development practices mature. What is clear is that the existing practices must be executed with more consistency and more automation than was sufficient in a human-paced development environment.

The governance framework that emerges is continuous and specification-anchored. Architectural intent is explicit in the specification and design before generation begins. Fitness functions and code scan tools enforce conformance automatically on every commit. ADRs are recorded at the cadence of AI-assisted decision-making rather than on a human-pace schedule. The entire architectural record is treated as live context for AI generation — not archived documentation.

---

## Comparative Summary

| Governance Dimension | Regulated / Safety-Critical | Uncertain / Evolving Requirements |
|---|---|---|
| Pre-AI governance tradition | Formal, traceable, change-controlled | Lightweight, ADR-based, culture-dependent |
| Primary AI governance risk | Traceability gaps in generated code | Structural entropy at AI generation speed |
| Governance anchor artifact | Architecture description documents, safety cases | Specification files, ADRs, fitness functions |
| AI's primary governance opportunity | Assisted traceability drafting, conformance tooling | Specification-as-generation-context, automated fitness functions |
| Human authority requirement | Absolute — every governed artifact requires human approval | Absolute — every ADR and conformance decision requires human judgment |
| Change to governance cadence | Continuous conformance monitoring added to existing periodic reviews | Governance must match generation cadence; episodic review is insufficient |
| Net governance burden after AI | Redistributed — implementation faster, review and documentation do not shrink proportionally | Increased urgency — more decisions per sprint require more governance discipline |
| Certification / audit implication | AI generation process must be documented as part of the evidence base | No formal certification, but architectural coherence directly impacts maintainability and technical debt |

---

## The Invariant Across Both Domains

Despite the substantial differences between these two domains, one principle holds universally: **AI generation does not make architectural decisions less important — it makes them less visible and more vulnerable to violation.** The appropriate response is not more retrospective review, but earlier and more precise delivery of architectural constraints into the AI generation process itself. At AI generation speed, the cost of structural drift discovered after the fact rises sharply; the governance frameworks that prove durable will be those that make architectural intent available to AI before generation begins, not after.

Explicit governance surfaces those decisions. Ongoing governance ensures they are caught at the speed of generation rather than discovered months later in the form of accumulated technical debt, certification failures, or structural collapse.

The architecture governance frameworks that will prove durable in the AI era are those that treat the architectural record not as documentation written after the fact but as living context that shapes generation from the outset — whether that context takes the form of a regulated system's architecture control document or a commercial product's specification file and ADR library.

---

## Unresolved Problems Summary

The following eight problems arise specifically from AI coding adoption and are identified and labeled at the point where they first appear in the body of this paper. Full descriptions are in the Unresolved Issues Glossary after the references.

| Label | Domain | Problem |
|---|---|---|
| G1 | Regulated | Traceability gaps — AI-generated structural choices may not be traceable to documented decisions |
| G2 | Regulated | Safety cases must now document the AI generation process and governance controls |
| G3 | Evolving requirements | Structural entropy accumulates invisibly at AI generation speed |
| G4 | Evolving requirements | Inconsistent pattern application across isolated AI generation prompts |
| G5 | Evolving requirements | ADR authorship cannot keep pace with AI-assisted decision-making cadence |
| G6 | Both | Fitness functions verify structure but not behavioral correctness at each layer |
| G7 | Both | No runtime verification when generated code diverges from specification |
| G8 | Regulated | AI generation review checkpoint lacks a structured workflow and evidence standard |

---

*This document should be read alongside* Waterfall and Agile in the Era of AI Coding, *which addresses the broader methodology context within which architecture governance operates.*

---

## References

[1] LDRA. "CI/CD Pipeline: Safety Critical Continuous Integration Tools." LDRA Technology, September 11, 2024. https://ldra.com/capabilities/continuous-integration/

[2] Parasoft. "Parasoft Expedites Support for New MISRA C:2025 Compliance Standard, Reinforcing Commitment to Advance Safety-Critical Software Development." Parasoft Corporation, March 10, 2025. https://www.parasoft.com/news/parasoft-expedites-support-for-new-misra-c2025-compliance-standard-reinforcing-commitment-to-advance-safety-critical-software-development/

[3] Parasoft. "Parasoft Bridges the AI and Compliance Gap With Certified C/C++test CT Featuring GoogleTest." Parasoft Corporation, November 5, 2025. https://www.parasoft.com/news/parasoft-bridges-ai-and-compliance-gap/

[4] Ford, Neal, Rebecca Parsons, and Patrick Kua. *Building Evolutionary Architectures: Automated Software Governance*. 2nd ed. O'Reilly Media, 2022.

[5] Amazon Web Services. "Specs." Kiro Documentation. kiro.dev, 2025. https://kiro.dev/docs/specs/

[6] Delimarsky, Den. "Spec-Driven Development with AI: Get Started with a New Open Source Toolkit." The GitHub Blog, September 2, 2025. https://github.blog/ai-and-ml/generative-ai/spec-driven-development-with-ai-get-started-with-a-new-open-source-toolkit/

[7] Shangqi, Liu. "Spec-Driven Development: Unpacking One of 2025's Key New AI-Assisted Engineering Practices." Thoughtworks Insights, December 4, 2025. https://www.thoughtworks.com/en-us/insights/blog/agile-engineering-practices/spec-driven-development-unpacking-2025-new-engineering-practices

[8] MathWorks. "Requirements Traceability." MATLAB & Simulink Documentation. MathWorks, Inc. https://www.mathworks.com/discovery/requirements-traceability.html

[9] Mndwrk. "The Role of Standards in Safety-Critical QA: Navigating ISO 26262, DO-178C, and IEC 62304." Mndwrk Blog, January 2024. https://www.mndwrk.com/blog/the-role-of-standards-in-safety-critical-qa-navigating-iso-26262-do-178c-and-iec-62304

[10] Nygard, Michael. "Documenting Architecture Decisions." Cognitect Blog, November 15, 2011. https://www.cognitect.com/blog/2011/11/15/documenting-architecture-decisions

[10a] Tyree, Jeff and Art Akerman. "Architecture Decisions: Demystifying Architecture." *IEEE Software* 22:2 (March 2005), 19–27. https://doi.org/10.1109/MS.2005.27

[11] Ejakait, Emmanuel. "Why You Should Be Using Architecture Decision Records to Document Your Project." Red Hat Blog, November 21, 2025. https://www.redhat.com/en/blog/architecture-decision-records

[12] Harding, Bill. "AI Copilot Code Quality: 2025 Data Suggests 4x Growth in Code Clones." GitClear Research, 2025. https://www.gitclear.com/ai_assistant_code_quality_2025_research

[13] Sonar. "2026 State of Code Developer Survey Report." Sonar Source, January 2026. https://www.sonarsource.com/resources/developer-survey-report/

[14] Qodo. "2025 State of AI Code Quality." Qodo Research, June 2025. https://www.qodo.ai/reports/state-of-ai-code-quality/

[15] Bucaioni, Alessio, Amleto Di Salle, Ludovico Iovino, Leonardo Mariani, and Patrizio Pelliccione. "Continuous Conformance of Software Architectures." *21st IEEE International Conference on Software Architecture (ICSA 2024)*, Hyderabad, India, June 7, 2024. https://conf.researchr.org/details/icsa-2024/icsa-2024-papers/3/Continuous-conformance-of-software-architectures

[16] Federal Aviation Administration. "Roadmap for Artificial Intelligence Safety Assurance." U.S. Department of Transportation, August 2024. https://www.faa.gov/aircraft/air_cert/step/roadmap_for_AI_safety_assurance

[17] European Union Aviation Safety Agency. "NPA 2025-07 — AI Trustworthiness: Detailed Specifications and Associated Acceptable Means of Compliance and Guidance Material." EASA, November 2025. https://www.easa.europa.eu/en/document-library/notices-of-proposed-amendment/npa-2025-07

[18] U.S. Food and Drug Administration. "Artificial Intelligence-Enabled Device Software Functions: Lifecycle Management and Marketing Submission Recommendations." Draft Guidance, Docket No. FDA-2024-D-4488, January 7, 2025. Comment period closed April 7, 2025; final version pending as of early 2026. https://www.fda.gov/media/184856/download

[19] Zhou, Yixiong. "Waterfall and Agile in the Era of AI Coding." Companion document to this paper, 2026.

---

## Unresolved Issues Glossary

The following issues arise specifically from the adoption of AI coding technology in software development workflows. Each issue is discussed in the body of this paper and marked with its corresponding label (G1–G8) at the point where it first appears. They are referenced in the *TDD, BDD, and the Stratified Behavioral Refinement Framework in the Era of AI Coding* companion document, which assesses how the SBR testing framework addresses each issue.

**G1 — Traceability gaps in AI-generated code — regulated domains.**
AI-generated structural choices — abstraction boundaries, error handling patterns, interface designs — may not be traceable to documented architectural decisions. Certification requires a continuous traceability chain from requirements through design to code to test.

**G2 — Safety cases must account for AI generation methods.**
The evidence base for a safety case must now include documentation of the AI generation process, the human review applied to generated outputs, and the governance controls that ensure generated code conforms to the safety architecture.

**G3 — Structural entropy accumulates invisibly at AI speed.**
Each AI-generated feature embeds implicit architectural decisions that are not visible in the generated code. The volume of those invisible decisions grows in direct proportion to AI generation velocity.

**G4 — Inconsistent pattern application across AI-generated code.**
AI tools generate different styles and patterns across isolated prompts because each prompt lacks cross-prompt architectural context. Two different implementations of the same behavior may both pass all tests while being structurally inconsistent.

**G5 — ADR generation cannot keep pace with AI-assisted decision-making.**
An AI-accelerated team makes more consequential architectural decisions per sprint than a human-paced team made per month. The ADR model, which depends on engineers recognizing that a decision has been made, cannot operate at AI generation speed.

**G6 — Fitness functions cover structure, not behavior.**
Architecture fitness functions verify structural properties — dependency direction, coupling metrics, layer boundaries — but do not verify whether the code correctly implements the intended behavior at each architectural layer.

**G7 — No downstream verification mechanism when generated code diverges from specification at runtime.**
The specification document as governance anchor works upstream — shaping what AI generates — but does not verify what the generated code actually does at runtime.

**G8 — AI generation review checkpoint in regulated domains lacks a structured workflow.**
The proposed AI generation review checkpoint does not specify what that review examines, what evidence it produces, or how it integrates with existing change control processes.

---

## Acknowledgements

The author thanks three reviewers whose comments materially improved this paper.

Fiona Xinghua Zheng, [title], [organization], reviewed all three papers in this series and provided comprehensive structured feedback. Her observations on this paper led to several specific improvements: her comment on overlap with the SBR paper prompted the addition of the scope boundary statement in the preface; her observation that the ADR discussion felt incomplete led to the extended G5 section explaining AI-assisted ADR authorship; her comment that the unresolved problems were referenced but not collected produced the Unresolved Problems Summary table now appearing before the references. Her cross-paper observations on boundaries, the unifying principle, and audience produced the orientation paragraph now appearing in each paper's preface.

Steven Offen, formerly Chief Technology Officer and Software Architect at MIRO Technologies and Tapestry Solutions, reviewed all three papers. His technical depth and architectural experience shaped the governance arguments in important ways. His observation that the treatment of the architectural record as living generation context is the central point of the paper prompted its elevation into the Abstract and Preface, where it now leads the argument. His challenge to the invariant section produced the more precise formulation that AI makes architectural decisions not only less visible but more vulnerable to violation, and his insistence on the importance of getting architectural constraints right before generation begins — rather than correcting drift after the fact — sharpened the paper's governing prescription. His observation that the ADR model as the right foundation was asserted rather than argued led to the more qualified framing that it remains the most defensible starting point while acknowledging that specification-anchored practices may yield more effective models.


Their collective feedback helped transform early drafts into clearer, more concise, and more readable papers.

---

*Reflects analysis current as of early 2026.*

*A note on sources: Several citations in this paper reference industry survey reports, practitioner publications, and vendor documentation rather than peer-reviewed academic papers. This reflects the state of the field — empirical research on AI-assisted software development practices is only beginning to appear in peer-reviewed venues, and the most current quantitative data on AI coding velocity, code quality, and team adoption patterns is available primarily through industry research. Where peer-reviewed equivalents exist they have been used; where they do not, the cited sources have been selected for methodological transparency, sample size, and relevance to the specific assertion they support.*
