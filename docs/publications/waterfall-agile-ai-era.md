# Waterfall and Agile in the Era of AI Coding

**Author:** Yixiong Zhou
**Date:** 2026

---

## Abstract

AI code generation shifts which parts of each methodology hold up — and which parts do not. This paper examines how AI changes the cost structure of software development in ways that challenge both Waterfall and Agile assumptions. Waterfall's long-dismissed emphasis on upfront specification gains new relevance when specification quality directly determines AI output quality. Agile's sprint mechanics, story points, and JBGE documentation practices face new pressures when implementation is no longer the expensive constraint. The paper argues that neither methodology ports cleanly into the AI era, and identifies the emerging hybrid pattern — Agile cadence, Waterfall-quality specification, AI implementation — as the practice that best-performing teams are converging on. The sprint must reorganize around specification and validation. Implementation, accelerated by AI, is no longer the bottleneck. Eight unresolved problems introduced by AI coding are identified and labeled for resolution in the companion papers.

---

## Preface

The AI era has reopened a debate the software industry largely considered settled. By the mid-2000s, Agile had been declared the winner over Waterfall — and for defensible reasons [2]. Waterfall's fatal flaw was its assumption that change was expensive and therefore had to be avoided through exhaustive upfront planning. Iterative development methods had already disproved that assumption decades before AI code generation entered the picture. By 2010, Agile had become the dominant answer for a strong majority of software teams, even if actual practice varied wildly from the Agile Manifesto's intent [2].

That verdict is now worth reopening — not because AI reverses the underlying logic, but because it changes the cost structure of software development in ways that shift which parts of each methodology hold up and which parts do not. The central shift this paper argues for is simple: when implementation is no longer the binding constraint, the sprint must reorganize around specification and validation — the activities that AI cannot replace.

This paper is one of three companion papers addressing AI-assisted software development as a layered discipline. *Waterfall and Agile in the Era of AI Coding* addresses methodology — how development process and sprint structure must adapt when AI accelerates implementation. *Architecture Governance in the Era of AI Coding* addresses structure — how architectural decisions are recorded, enforced, and kept coherent at AI generation speed. *TDD, BDD, and the Stratified Behavioral Refinement Framework* addresses verification — how behavior is specified and how the trust boundary that makes AI generation trustable is built and maintained. The unifying principle across all three is that specification precedes generation, architecture constrains structure, and tests define the boundary of trust. This paper is the entry point for the series and is written for anyone practicing or managing software development — engineers, leads, product managers, and architects alike.

---

## Part 1 — What AI Code Generation Actually Changes

Before examining either methodology, it is worth being precise about what changes when AI enters the picture, because not everything does.

**What changes:** The marginal cost of writing code drops dramatically with AI assistance — and continues to drop as AI coding capabilities advance at a pace that outstrips any fixed measurement. What took a skilled engineer days can now take hours, and the trajectory makes conservative estimates obsolete almost as soon as they are published.

**What does not change:** The cost of building the wrong thing. Misunderstood requirements, ambiguous specifications, and poor system design produce the same failed software whether a human or an AI wrote the implementation. If anything, AI sharpens that risk — the wrong thing can now be built faster than ever. *(W8)*

**What gets amplified:** The leverage of upstream decisions. If your specification is precise and your architecture is sound, AI multiplies your velocity. If your specification is vague and your architecture is muddled, AI multiplies your chaos.

This asymmetry — specification quality as the new velocity constraint — is the lens through which to examine both methodologies.

---

## Part 2 — The Pre-AI Baseline

### Why Waterfall Lost

Waterfall's canonical failure mode is the Big Requirements Document: six months of writing, six months of implementation, and then discovery upon delivery that the requirements were wrong. The assumption that requirements can be fully known and frozen upfront is almost never true in software. The cost of late-stage changes is catastrophic, and the model provides no mechanism for incorporating what is inevitably learned by building.

### Why Agile Won

Agile won because feedback loops matter. Short iterations, working software, and close customer collaboration allow teams to discover what they actually need through the process of building and using it. The sprint structure unitizes change effort into discrete, deliverable increments, making the cost of change measurable and the value delivered per sprint optimizable. Requirements are not fully knowable upfront; they emerge iteratively through use.

Two related practices extended this cost optimization further. Just Barely Good Enough (JBGE) documentation [12] and code-as-documentation [13] eliminated the ongoing cost of synchronizing documentation with code — a chronic drain in the pre-Agile era where comprehensive upfront documents became stale as implementation progressed, requiring constant maintenance that teams rarely had the discipline to sustain. By treating tests and code as the primary documentation — always accurate because they execute against the actual system — Agile teams removed an entire category of overhead while also eliminating the confusion caused by documents that said one thing while the code did another. This was a genuine engineering advance, not mere laziness.

---

## Part 3 — Waterfall in the AI Era

### The Partial Rehabilitation

AI code generation somewhat restores viability to a disciplined upfront specification approach for a specific and important reason: if the cost of implementation is low, the cost of getting the specification right is proportionally higher than the cost of writing code. The classic Waterfall critique — "you are spending too much time on docs and not enough building" — loses force when building is cheap.

**Specification becomes a first-class AI input.** A detailed, well-structured requirements document is in practice a very effective prompt. Teams that invest in precise functional specifications before AI generation get dramatically better output than teams that describe features loosely [6]. This is structurally similar to what Waterfall was trying to achieve with its requirements phase — clarity of intent before construction begins.

**Upfront architecture investment pays higher returns.** AI generates code well; it designs systems poorly without strong direction. As Addy Osmani, VP of Engineering at Google Chrome, observes: agents can rapidly reach 80% code completion, but the remaining work requires deep knowledge of context, architecture, and trade-offs that AI cannot supply on its own [7]. Waterfall's emphasis on upfront architecture and design is more defensible than it used to be because the implementation phase is no longer where most of the time is spent. When AI compresses a three-month implementation phase into weeks, the architecture and design phases grow as a proportion of total project time. A week of upfront architectural thinking that once felt like overhead against months of building now represents a substantial and justified share of the total effort [11].

**Regulated and safety-critical domains benefit.** In domains where requirements traceability, formal change control, and documented design decisions are required — medical devices, avionics, defense, financial systems — AI code generation makes Waterfall more attractive, not less. You still need the documentation; now the implementation is faster and cheaper. The overhead that made Waterfall impractical in commercial software becomes proportionally smaller.

### Where Waterfall Still Fails

The fundamental problem remains: requirements still cannot be fully known upfront for most software. AI does not fix human uncertainty about what we want. *(W1)* A product manager who could not write a complete specification in 2015 cannot write a complete specification today. If anything, AI's speed makes the gap between specification and what users actually wanted more visible, not less — you surface the mismatch faster.

Additionally, Waterfall's change control processes are built for expensive implementation. When a change takes a week, a formal change request process is proportionate. When a change takes an hour with AI assistance, a heavyweight change management overhead becomes the bottleneck. A change management process calibrated to expensive implementation becomes a bottleneck when implementation is cheap.

The right model is not lighter governance but faster specification refinement — revising and validating the BDD scenario first, then regenerating the implementation against the updated specification. The measure of an effective change process is the ratio of specification improvement to labor hours spent, not the absence of process [14].

### Waterfall: Pros and Cons in the AI Era

**Pros:**
- Upfront specification quality maps directly to AI output quality — the better the requirements document, the better the generated code
- Architecture and design phases are relatively more valuable as implementation becomes cheaper
- Traceability and documentation fit regulated and safety-critical environments well
- Clear, concise requirements produce superior and more consistent AI generation results

**Cons:**
- Requirements uncertainty does not disappear with AI — the wrong-thing problem remains
- Change control processes do not scale down to match AI-accelerated implementation costs
- Still fails when users cannot articulate what they need until they see and interact with working software — no amount of upfront analysis can substitute for that discovery process
- Big-bang delivery still risks delivering the wrong thing faster

---

## Part 4 — Agile in the AI Era

### How AI Complicates Agile

Agile was designed for a world where implementation was the expensive constraint. Its rituals — sprint planning, story pointing, velocity tracking, two-week cycles — are all calibrated to the assumption that building takes meaningful time. When AI can generate a feature in hours rather than days, several Agile conventions stop making sense.

**Story points lose meaning as specification becomes the constraint.** If a five-point story now takes four hours of AI-assisted implementation, what does the sprint capacity model represent? Teams are discovering that velocity metrics become noise when AI collapses implementation time [3]. The constraint shifts from "how much can we build" to "how much can we specify, review, and validate." *(W2)*

**Sprint length becomes arbitrary.** The two-week sprint made sense when it represented a coherent unit of implementable work. As AI compresses implementation time, the two-week sprint no longer represents a coherent unit of buildable work — the sprint fills instead with specification, review, and stakeholder validation that AI cannot accelerate [3]. The right iteration cadence is an open question, but its direction is visible: the sprint reorganizes around specification completeness rather than implementation volume, and "done" shifts from "code committed and tested" to "behavior specified, generated, and verified." Review cycles replace implementation cycles as the pacing constraint. The *TDD, BDD, and SBR* companion paper addresses the specification-completeness standard that a replacement "done" definition requires [14]. *(W2)*

**"Working software over comprehensive documentation" needs revision.** The Agile Manifesto's famous value statement was written against Waterfall's over-documentation pathology [1]. But AI generation requires precise specification, which is a form of documentation. Teams that rely on JBGE documentation [12] and code-as-documentation practices [13] find that AI produces mediocre output from vague prompts [6]. AI generation requires precise behavioral specification before code exists — the exact artifact that JBGE and code-as-documentation treat as unnecessary overhead. The pendulum swings back toward documentation — not all the way to Waterfall, but meaningfully. The shift is not toward more documentation but toward more precise specification. A concise BDD scenario set that captures a feature's behavioral contract is more useful as an AI input than a lengthy requirements document that remains vague about outcomes. Volume is not the goal; precision and behavioral completeness are. *(W3)*

**Technical debt accelerates.** AI-generated code in a rapid Agile cadence accumulates architectural debt faster than human review can detect it [4][5]. Architectural debt here means concrete structural problems: violated layer boundaries, inconsistent service patterns, and implicit design decisions that were never recorded. The deeper problem is asymmetric speed — AI generates faster than any human reviewer can evaluate structural correctness. A reviewer catching functional bugs in AI-generated code is not the same as a reviewer catching architectural drift. The former is visible in the output; the latter requires understanding the system's intended structure and comparing every generated decision against it. Without a mechanism that operates at generation speed, debt accumulates invisibly sprint by sprint. The *TDD, BDD, and SBR* companion paper addresses how the trust boundary and pre-specified tests shift debt detection from human review to automated verification [14]. The structural dimension of this problem — how fitness functions and continuous conformance monitoring prevent architectural drift from accumulating at AI speed — is addressed in the companion paper *Architecture Governance in the Era of AI Coding* [11]. *(W6)*

### Where Agile Still Wins

The core insight — that requirements are discovered iteratively — remains correct and AI may not change it. What AI does is reduce the implementation cost of each iteration. Specification work per issue may also reduce over time — AI can assist with drafting, scenario generation, and surfacing ambiguity, improving both speed and quality without removing the human from the loop. The key distinction is that human judgment remains required for specification, while implementation can be delegated to AI. The gain shows up as more issues delivered per sprint, fewer developers needed per issue, or shorter sprints for the same scope.

**The testing investment argument.** Agile's emphasis on continuous integration, automated testing, and working software at the end of every sprint is more important with AI generation, not less. AI-generated code that is not continuously tested is a liability that compounds with each sprint. Agile's quality infrastructure is the right container for AI-generated code, provided the validation framework is rigorous enough to verify AI-generated behavior at every level [14].

**Customer collaboration becomes more valuable.** When implementation is cheap, the bottleneck becomes knowing what to build. Agile's emphasis on product owner involvement and regular stakeholder review of working software is even more critical when you can build the wrong thing in a quarter of the time.

This raises a tension the paper has not yet addressed directly. User uncertainty and specification validity pull in opposite directions. Users cannot fully specify what they need before they see working software — that is Agile's founding insight. Yet AI generation requires precise specification to produce correct output. BDD Gherkin scenarios are the practical bridge: they are lightweight enough to write iteratively, precise enough to drive AI generation, and readable enough for stakeholders to validate before any code exists. The cost of being wrong also drops with AI — a misunderstood requirement discovered at sprint review costs less to correct when AI can regenerate the implementation quickly. The *TDD, BDD, and SBR* companion paper addresses how iterative BDD scenario refinement keeps specification validity aligned with evolving user understanding [14].

**Refactoring becomes faster and safer.** Agile's "embrace change" philosophy is better supported when AI can assist with large-scale refactoring. With a comprehensive test suite defining the behavioral contract, AI can execute the mechanical transformation while the tests catch any deviation immediately. A database migration from ArangoDB to PostgreSQL combined with a data model restructuring from three entities to one completed successfully in one week under AI assistance with a full test suite in place [15]. The trust boundary concept that makes this safety guarantee precise is developed in the companion paper [14].

### Agile: Pros and Cons in the AI Era

**Pros:**
- Iterative feedback loops remain the right model for uncertain requirements — the most durable insight in software development
- CI/CD and automated testing infrastructure is essential for AI-generated code quality, and Agile teams already have it
- Customer collaboration becomes more, not less, important when building is cheap and building the wrong thing is a greater risk
- Refactoring under AI assistance becomes faster and less painful
- Short cycles surface wrong-thing problems faster. Working software is what triggers stakeholder recognition that the wrong thing is being built — users cannot identify the mismatch between what they asked for and what they need until they interact with something real. A two-week cycle produces that interaction point fourteen days into the work. A six-month cycle produces it six months in, after far more has been built in the wrong direction.

**Cons:**
- Sprint mechanics calibrated to human implementation velocity become awkward when AI collapses build time
- JBGE and code-as-documentation practices produce insufficient AI generation inputs, degrading output quality
- Technical debt can accumulate faster than sprints can absorb and address it
- Story pointing and velocity tracking lose precision as planning tools. AI generation reduces average implementation duration but increases variability across tasks — a well-specified feature generates in hours, while a poorly specified one requires days of iterative refinement. This variability is not a property of AI itself but of the team's specification skill. Before AI, implementation speed varied with coding skill. With AI, delivery speed varies with specification skill. The binding constraint shifts, but the human skill dependency does not disappear — it relocates.
- AI generation is only as good as the specification it receives — vague or incomplete requirements produce vague or incorrect code, regardless of how fast the iteration cycle runs

---

## Part 5 — The Synthesis: What Actually Fits the AI Era

Neither methodology ports cleanly into the AI era. What the AI era is pushing toward is a hybrid that borrows deliberately from both — not as a compromise, but as a purposeful integration that takes the best insight from each.

### From Waterfall: What Is Worth Keeping

- Upfront architecture and design investment (more valuable, not less, as implementation becomes cheaper)
- Precise, written specifications as first-class project artifacts *(W4)*
- Formal acceptance criteria before implementation begins *(W5)*

Formal acceptance criteria formalize user needs into verifiable statements that align the team on what success looks like before implementation begins. Acceptance criteria written without genuine user involvement do not capture actual user needs. Acceptance criteria treated as fixed once written cannot adapt as user understanding evolves. Both conditions produce the same result: the criteria measure specification conformance rather than user value. BDD Gherkin scenarios are the practice that keeps acceptance criteria honest: written in stakeholder-readable language, validated before implementation, and revisable as user understanding evolves [14].
- Traceability between requirements and implemented behavior

### From Agile: What Is Worth Keeping

- Iterative delivery and feedback loops — these are irreplaceable regardless of AI capability
- Continuous integration and automated testing — now critical infrastructure, not optional practice
- Close customer collaboration throughout the development cycle
- Willingness to change direction based on working software and real user feedback

### What Needs to Evolve in Both

- Sprint structure should reorganize around specification and validation cycles, not implementation cycles, since implementation is no longer the constraint. The planning atom should measure specification completeness. The definition of done should mean verified behavior. The *TDD, BDD, and SBR* companion paper's twelve-step sequence provides the workflow that makes this concrete [14]. *(W2)*
- Documentation should be valued and constructed as an AI input, not merely as a compliance artifact or legacy overhead. The measure is not volume but precision — a well-written BDD scenario set is more valuable than a comprehensive requirements document that is vague about behavioral outcomes.
- Technical review frequency must increase to match the pace at which AI generates code. Human review alone cannot achieve this — AI generates faster than any reviewer can evaluate structural correctness. The answer is automated verification at commit time: pre-specified tests verify that AI-generated code satisfies the behavioral contract [14], while architecture fitness functions and conformance tooling verify structural correctness continuously [11]. Human review focuses on specification quality and design decisions — the judgments that automation cannot replace. *(W7)*
- Architecture governance needs to be explicit and ongoing, not a one-time upfront activity [11]

---

## Part 6 — The Emerging Pattern

The teams performing best today follow a model that did not exist as a named methodology before 2024: **Agile cadence, Waterfall-quality specification, AI implementation.** This is the foundational pattern described across the spec-driven development literature — Thoughtworks' SDD framework, GitHub's Spec Kit, and Amazon's Kiro all converge on the same structure: short iterative cycles, but with rigorous specification front-loaded within each cycle as the input that drives AI generation [8][9][10].

Short iterations, but with specification work front-loaded within each iteration rather than treated as an afterthought. The sprint planning session becomes the most important meeting because it is where the AI prompts are effectively written. Working software at the end of every sprint remains the goal, but the work of the sprint is now specification and validation, not primarily construction.

A sprint in this model follows a different shape from a traditional Agile sprint:

| Sprint phase | Activity | Primary constraint |
|---|---|---|
| Planning | Requirements analyzed; BDD scenarios drafted and reviewed with stakeholders | Specification clarity — can we design against this? |
| Early sprint | Architecture design, implementation skeleton, tests written to red | Design completeness — is the trust boundary fully established? |
| Mid sprint | AI generates implementations within the trust boundary | Speed — bounded and fast |
| Late sprint | BDD grounding proof runs; stakeholder demo | Validation — does the system realize the specification? |

The definition of done shifts from "code committed and tested" to "behavior specified, generated, and verified against the BDD scenarios." The sprint's pacing constraint is no longer implementation — it is the time required to arrive at requirements precise enough to design against and stakeholders available to validate the result.

This shift redefines the developer role. Writing code is no longer the primary activity — specifying behavior, making design decisions, and validating that generated code realizes the specification are. Coding skill remains essential: reviewing AI-generated code for correctness, recognizing structural drift, and making architectural judgments all require deep technical knowledge. What changes is where that knowledge is applied. Before AI, the scarce skill was implementation speed. With AI, the scarce skill is specification clarity and design judgment — the ability to express intent precisely enough that AI generation produces trustworthy results, and to recognize when it has not.

The teams that are struggling are those who imported AI tools into existing Agile workflows without changing anything else — treating AI as a faster keyboard rather than as a force that realigns which parts of the process are now the binding constraint [3].

The teams that will maintain software quality over a three-to-five-year horizon are those that recognize this realignment explicitly: specification quality is the new velocity, human judgment is irreplaceable for architecture and intent, and the workflows that win will formalize that constraint rather than hoping AI generates the right thing from vague direction.

---

## Summary Comparison Table

| Dimension | Waterfall in AI Era | Agile in AI Era |
|---|---|---|
| Specification quality | Strength — maps directly to AI output quality | Weakness — JBGE and code-as-documentation practices degrade AI generation input |
| Requirements uncertainty | Still a fatal flaw for most software | Still the core strength |
| Architecture investment | More defensible as build cost drops | Needs explicit governance added |
| Change management | Still too heavyweight for AI-speed iteration | Remains a strength |
| Testing and CI/CD | Underemphasized | Essential and well-established |
| Documentation | Valued but can become a bottleneck | Undervalued but needed for AI generation |
| Technical debt risk | Moderate — big-bang but slower | Higher — AI-speed sprint debt accumulation |
| Regulated domains | Advantage — traceability requirements align | Disadvantage — compliance overhead conflicts |
| Best use case | Stable requirements, regulated, safety-critical systems [11] | Most commercial software with uncertain or evolving requirements [11] |

---

## Unresolved Issues Glossary

The following issues arise specifically from the adoption of AI coding technology in software development workflows. Each issue is discussed in the body of this paper and marked with its corresponding label (W1–W8) at the point where it first appears. They are referenced in the *TDD, BDD, and the Stratified Behavioral Refinement Framework in the Era of AI Coding* companion document, which assesses how the SBR testing framework addresses each issue.

**W1 — Requirements uncertainty is not resolved by AI.**
Users cannot fully articulate what they need until they see and interact with working software. AI does not resolve this uncertainty. The tension is that AI generation requires precise specification, yet precise specification requires user knowledge that only emerges through iteration. BDD Gherkin scenarios reduce this tension — they are cheap to write, readable by stakeholders, and precise enough to drive generation. A wrong scenario discovered at sprint review costs less to correct when AI can regenerate the implementation quickly. The uncertainty is not eliminated; the cost of discovering it earlier is reduced [14].

**W2 — Sprint mechanics become incoherent under AI velocity.**
Story points lose meaning, sprint length becomes arbitrary, and velocity metrics become noise when AI collapses implementation time. A replacement paradigm requires new answers on three fronts. The planning atom should measure specification completeness. The sprint cadence should be defined around specification and validation cycles. "Done" should mean verified behavior. The *TDD, BDD, and SBR* companion paper's twelve-step sequence and trust boundary model provide the specification-completeness framework [14], but their translation into team-level planning and measurement discipline is an open research question.

**W3 — Agile's JBGE and code-as-documentation practices conflict with AI's need for precise behavioral specification.**
JBGE treats external documentation as unnecessary overhead when tests and code serve the same purpose. Code-as-documentation works when code already exists. Neither practice produces the precise behavioral specification that AI generation requires before any code exists. No practice bridges this gap.

**W4 — Specification quality has no verification mechanism.**
Specification quality determines AI output quality. Specification completeness is partially verifiable — BDD scenario coverage mapped against requirements can identify gaps. Specification correctness is harder. A specification is correct when it captures actual user intent, not merely when it is internally consistent. No automated mechanism verifies this. The available mechanisms are human stakeholder validation of BDD scenarios before implementation begins and iterative feedback from working software. This remains a fundamental constraint, not a solvable engineering problem with current methods [14].

**W5 — Formal acceptance criteria are named but not operationalized.**
Formal acceptance criteria formalize user needs into verifiable statements that align the team on what success looks like. Acceptance criteria written without genuine user involvement do not capture actual user needs. Acceptance criteria treated as fixed once written cannot adapt as user understanding evolves. Both conditions produce the same result: the criteria measure specification conformance rather than user value. BDD Gherkin scenarios are the operationalization that preserves their intent: readable by stakeholders, validated before implementation begins, and revisable as understanding evolves [14].

**W6 — Technical debt accumulates faster than sprints can absorb it.**
AI-generated code produces architectural debt at AI speed. The feedback loop between debt accumulation and sprint work is not closed.

**W7 — Technical review cadence needs to increase but no mechanism is specified.**
Technical review cadence needs to increase to match the pace at which AI generates code, but what that review examines or how it is triggered is not specified.

---

## Acknowledgements

The author thanks three reviewers whose comments materially improved this paper.

Fiona Xinghua Zheng, [title], [organization], reviewed all three papers in this series and provided comprehensive structured feedback. Her observations on this paper led to several specific improvements: her comment that the hybrid model felt high-level prompted the addition of the sprint structure table in Part 6; her observation that the documentation comeback point could be misread led to the clarification that the shift is toward precision, not volume; her note that the technical debt discussion felt disconnected from the governance paper produced the explicit cross-reference to the companion paper; and her comment on the developer role led to the new paragraph in Part 6 describing how the developer's primary activity shifts from implementation to specification and validation. Her cross-paper observations on boundaries, the unifying principle, and audience produced the orientation paragraph now appearing in each paper's preface.

Steven Offen, formerly Chief Technology Officer and Software Architect at MIRO Technologies and Tapestry Solutions, reviewed all three papers. His technical depth and architectural experience produced several specific improvements to this paper. His observation that "open question each team must answer" was insufficient prompted the substantive expansion of W2 into a three-part replacement paradigm with a forward pointer to the SBR companion paper. His distinction between implementation speed and iteration speed as a whole produced the corrected treatment of AI's effect on sprint economics. His challenge to the refactoring claim — that reduced catastrophic risk requires qualification — led to the trust boundary framing and the introduction of the ArangoDB-to-PostgreSQL personal example as direct evidence. His observation that acceptance criteria can become a contractual shield prompted the rewrite of W5 around the true purpose of acceptance criteria and the role of BDD in preserving that purpose. His question about specification correctness as a verification problem led to the completeness-versus-correctness distinction now stated in W4. His challenge to the change management framing produced the specification refinement efficiency metric. His observation on review cadence ambiguity led to the automated verification paragraph that now closes W7.

Bob Lowell, formerly Senior Director of Engineering Data Platform and Senior Distinguished Architect at Walmart Labs, reviewed this paper. His perspective as a disciplined Agile practitioner with large-scale enterprise experience led to several specific improvements. His observation that architecture investment and documentation are standard practice in disciplined Agile teams prompted the reframing of the architecture section away from a misleading Agile-versus-Waterfall contrast and toward the more precise argument about explicit architectural intent before AI code generation begins. His challenge to the story points framing produced the more accurate treatment of story points as a constraint-measurement tool that needs recalibration rather than a practice that loses validity. His question about how sprint mechanics are solved prompted the forward pointer to the SBR companion paper. His observation on prompt engineering as documentation prompted the clarifying passage that distinguishes a well-engineered prompt from an executable specification — one of the paper's sharper arguments.

Their collective feedback helped transform early drafts into clearer, more concise, and more readable papers.

---

*Reflects analysis current as of early 2026. The tooling landscape for AI-assisted development is evolving rapidly; the methodological conclusions are intended to be durable across near-term tool changes.*

*A note on sources: Several citations in this paper reference industry survey reports, practitioner publications, and vendor documentation rather than peer-reviewed academic papers. This reflects the state of the field — empirical research on AI-assisted software development practices is only beginning to appear in peer-reviewed venues, and the most current quantitative data on AI coding velocity, code quality, and team adoption patterns is available primarily through industry research. Where peer-reviewed equivalents exist they have been used; where they do not, the cited sources have been selected for methodological transparency, sample size, and relevance to the specific assertion they support.*

---

## References

[1] Beck, Kent, Mike Beedle, Arie van Bennekum, et al. *Manifesto for Agile Software Development*. Agile Alliance, 2001. https://agilemanifesto.org/

[2] VersionOne. "10th Annual State of Agile Report." VersionOne Inc., 2016. https://www.slideshare.net/slideshow/version-one-10thannualstateofagilereport/169924197

[3] Faros AI. "AI Productivity Paradox Report 2025." Faros AI, June 2025. https://www.faros.ai/ai-productivity-paradox

[4] Harding, Bill. "AI Copilot Code Quality: 2025 Data Suggests 4x Growth in Code Clones." GitClear Research, 2025. https://www.gitclear.com/ai_assistant_code_quality_2025_research

[5] Sonar. "2026 State of Code Developer Survey Report." Sonar Source, January 2026. https://www.sonarsource.com/resources/developer-survey-report/

[6] Qodo. "2025 State of AI Code Quality." Qodo Research, June 2025. https://www.qodo.ai/reports/state-of-ai-code-quality/

[7] Osmani, Addy. "The 80% Problem in Agentic Coding." Elevate by Addy Osmani, January 28, 2026. https://addyo.substack.com/p/the-80-problem-in-agentic-coding

[8] Shangqi, Liu. "Spec-Driven Development: Unpacking One of 2025's Key New AI-Assisted Engineering Practices." Thoughtworks Insights, December 4, 2025. https://www.thoughtworks.com/en-us/insights/blog/agile-engineering-practices/spec-driven-development-unpacking-2025-new-engineering-practices

[9] Delimarsky, Den. "Spec-Driven Development with AI: Get Started with a New Open Source Toolkit." The GitHub Blog, September 2, 2025. https://github.blog/ai-and-ml/generative-ai/spec-driven-development-with-ai-get-started-with-a-new-open-source-toolkit/

[10] Amazon Web Services. "Specs." Kiro Documentation. kiro.dev, 2025. https://kiro.dev/docs/specs/

[11] Zhou, Yixiong. "Architecture Governance in the Era of AI Coding." Companion document to this paper, 2026.

[12] Ambler, Scott. "Just Barely Good Enough Models and Documents: An Agile Best Practice." Agile Modeling, 2023. https://agilemodeling.com/essays/barelygoodenough.htm

[13] Fowler, Martin. "Code As Documentation." Martin Fowler's Bliki, 2005. https://martinfowler.com/bliki/CodeAsDocumentation.html

[14] Zhou, Yixiong. "TDD, BDD, and the Stratified Behavioral Refinement Framework in the Era of AI Coding." Companion document to this paper, 2026.

[15] Zhou, Yixiong. Personal communication. Founder and software architect with 20+ years of experience. 2026. "A database migration from ArangoDB to PostgreSQL combined with a data model restructuring from three entities representing item, container, and location into a single Item entity with a type column completed successfully in one week under AI assistance, with a full test suite maintaining behavioral correctness throughout."
