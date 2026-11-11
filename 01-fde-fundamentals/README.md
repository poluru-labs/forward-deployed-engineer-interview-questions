# FDE fundamentals

What a Forward Deployed Engineer does, how the role differs from product engineering and solutions engineering, and the skills interviewers probe first: ambiguity, customer context, shipping in someone else's environment, and owning outcomes end to end.

This curated set of 100 foundational questions is a study guide, not a ranking or a list of verified company interview questions. Role boundaries vary by company. Use the answers as starting points, and support them with examples from your own experience.

## Quick cheatsheet

| Topic | Remember |
| --- | --- |
| FDE mission | Turn a customer's real workflow problem into a working, adopted solution with measurable value. |
| Delivery loop | Discover → define success → scope → build → validate → deploy → measure → improve. |
| Discovery | Ask who does the work, how it works today, what fails, how often, and what the failure costs. |
| Success | Establish a baseline, target, measurement method, owner, and review date. |
| Scope | Choose one useful workflow; state assumptions, dependencies, acceptance criteria, and exclusions. |
| Prioritization | Weigh customer impact, urgency, uncertainty, effort, and operational risk. |
| Technical judgment | Start with the simplest design that meets the requirements and has a credible growth path. |
| Integration | Verify identity, permissions, network access, schemas, rate limits, and failure behavior early. |
| Reliability | Design for timeouts, safe retries, idempotency, observability, recovery, and rollback. |
| Security | Use least privilege, approved data access, managed secrets, and tenant isolation. |
| Prototype vs. production | A demo proves a possibility; production also needs security, reliability, ownership, and support. |
| Customer communication | State the outcome, evidence, risk, decision needed, owner, and next checkpoint. |
| Escalation | Escalate early with impact, evidence, attempted fixes, options, and a recommendation. |
| Product feedback | Separate the underlying reusable need from a customer's requested implementation. |
| Behavioral answers | Use Situation → Task → Action → Result → Reflection; distinguish your contribution from the team's. |
| Strong interview habit | Clarify assumptions, explain tradeoffs, propose a small validation step, and define how you will measure success. |

**Useful measures:** adoption = active intended users / eligible intended users; task success rate = successful tasks / attempted tasks; time saved = comparable baseline time − new workflow time. Always define the population, time window, and quality threshold. Estimate net value after implementation, operating, and review costs; time saved alone is not proof of financial return.

## Questions and answers

### Role and mindset (1–10)

#### 1. What is a Forward Deployed Engineer?

An FDE is an engineer who works closely with customers to understand operational problems and deliver solutions in their actual environment. The work often spans discovery, integration, application development, deployment, and adoption. The defining responsibility is connecting technical delivery to a useful customer outcome.

#### 2. How does an FDE differ from a product engineer?

A product engineer typically builds capabilities for a broader product audience, while an FDE often starts with a specific customer's workflow and constraints. Both write production code and make engineering tradeoffs. FDE work can reveal recurring needs that become core product capabilities.

#### 3. How does an FDE differ from a solutions engineer?

Solutions engineers often emphasize technical evaluation, demonstrations, and helping customers assess a product. FDEs often take deeper responsibility for implementing and operating customer solutions. Titles overlap, so clarify who owns production code, deployment, and ongoing support at the company in question.

#### 4. How does an FDE differ from a consultant?

Consulting can include hands-on implementation, so there is no universal boundary. An FDE commonly works within a product company, building with its platform and feeding deployment lessons back into the product. Explain the actual ownership model rather than assuming consultants only provide advice.

#### 5. What does end-to-end ownership mean?

It means staying accountable from problem definition through a working, adopted solution and an agreed operational handoff. I track dependencies, resolve blockers, validate results, and ensure someone can support the system. Ownership includes coordinating work I cannot or should not do myself.

#### 6. What makes someone effective in an FDE role?

Strong candidates combine engineering ability, curiosity about customer workflows, clear communication, and judgment under uncertainty. They can narrow an ambiguous problem, ship a useful increment, and learn from evidence. They also recognize when security, product, or domain specialists need to be involved.

#### 7. Why are you interested in FDE work?

A strong answer connects your motivation to both engineering and customer outcomes. For example: “I enjoy understanding how people work, identifying the bottleneck, and building something they can use. My experience with [real project] showed me that deployment and adoption matter as much as the implementation.” Replace the placeholder with your own evidence.

#### 8. What does customer empathy mean for an engineer?

It means understanding users' incentives, constraints, language, and consequences of failure. I observe their workflow and test my assumptions with them instead of treating a feature request as a complete problem statement. Empathy helps me build something usable without agreeing to every request.

#### 9. What does good judgment under ambiguity look like?

I separate known facts, assumptions, and unanswered questions, then identify which uncertainty could most change the plan. I choose a small experiment or conversation to resolve it. I make reversible decisions quickly and apply more scrutiny to expensive or hard-to-reverse commitments.

#### 10. How do you know an FDE engagement was successful?

The agreed workflow performs better, intended users adopt it, and the customer can sustain it with clear ownership. I compare results with the baseline using outcome and quality measures, not just shipped features. I also check operating costs, reliability, and unresolved risks.

### Customer understanding and discovery (11–20)

#### 11. What do you ask in a first customer meeting?

I ask what outcome they want, who performs the work, how the workflow operates today, and where it breaks down. I explore volume, frequency, business impact, existing tools, and constraints. I finish by agreeing on the next evidence-gathering step and who needs to participate.

#### 12. How do you turn a vague request into a clear problem statement?

I describe the user, task, current obstacle, and measurable consequence. “We need AI” might become “Support analysts spend too long locating approved answers across three systems.” I validate that statement with users before choosing a technical approach.

#### 13. How do you distinguish a symptom from a root problem?

I walk through a recent real example and ask what caused each delay or error. I compare interview statements with logs, process records, or direct observation. For instance, a slow approval screen may actually reflect missing information that forces repeated handoffs.

#### 14. Who are the key stakeholders in an engagement?

Common stakeholders include end users, the business sponsor, a customer champion, system and data owners, security, procurement, and operations. I identify who uses, approves, funds, and supports the solution. A stakeholder map helps expose dependencies and conflicting incentives early.

#### 15. What is the difference between a sponsor and a champion?

A sponsor typically provides organizational authority, funding, or strategic backing. A champion actively helps the project progress by connecting people, explaining internal processes, and supporting adoption. They may be the same person, but I verify both functions are covered.

#### 16. How do you validate what a customer tells you?

I triangulate interviews with workflow observation, representative examples, and available operational data. I ask users to demonstrate a recent task rather than describe only the ideal process. When evidence disagrees, I investigate the conditions behind the difference instead of choosing the most senior person's account.

#### 17. What do you do when customer stakeholders disagree?

I make the disagreement explicit: desired outcomes, constraints, priorities, or decision authority. I compare options against shared success criteria and identify the accountable decision maker. I document the decision and tradeoffs so the team can proceed with a common understanding.

#### 18. How do you avoid jumping to a solution too early?

I write down the problem, evidence, and constraints before discussing architectures. I consider a few approaches, including process changes or existing product features. A quick prototype can test a hypothesis, but I avoid treating the first prototype as a committed design.

#### 19. How do you handle a request for a feature that may not help?

I ask which task the feature would improve and what observable result the customer expects. I explain any concerns and offer a smaller test or alternative that targets the same need. The goal is a shared decision based on value and evidence.

#### 20. What should a discovery summary contain?

It should capture the problem, users, current workflow, baseline, target outcome, constraints, stakeholders, and open questions. It should also identify assumptions, dependencies, and the next validation step. I review it with the customer to catch misunderstandings before implementation.

### Scoping and prioritization (21–30)

#### 21. How do you define a good success metric?

I choose a measure tied to the customer's outcome, establish a baseline, and agree on a target and measurement window. I pair speed or cost metrics with quality and safety measures. For example, faster case resolution only counts if accuracy and escalation rates remain acceptable.

#### 22. What is the difference between an output and an outcome?

An output is something delivered, such as an integration or dashboard. An outcome is the resulting change, such as fewer failed handoffs or faster decision-making. Outputs matter because they enable outcomes, but completing them alone does not establish value.

#### 23. How do you choose an MVP?

I choose the smallest complete workflow that can test a valuable hypothesis with real users. I include the minimum necessary access controls, failure handling, and measurement for that context. I explicitly defer secondary features and agree on what the MVP will prove.

#### 24. How do you prioritize competing requests?

I compare expected impact, urgency, effort, uncertainty, dependencies, and risk. I make the tradeoffs visible and involve the accountable owner when priorities conflict. A scoring method can structure the discussion, but uncertain estimates should not create false precision.

#### 25. How do you estimate unfamiliar work?

I break it into components and identify what I understand versus what needs investigation. I time-box a spike for the highest uncertainty and provide a range with assumptions. I update the estimate as evidence improves and flag dependencies outside the team's control.

#### 26. How do you manage scope creep?

I compare new requests with the agreed outcome and acceptance criteria. I explain their impact on cost, timing, and risk, then propose adding capacity, moving the date, or replacing lower-priority work. I record the decision so scope changes are deliberate.

#### 27. What are acceptance criteria?

Acceptance criteria are observable conditions that determine whether a deliverable meets the agreed need. They describe expected behavior, relevant failure cases, and measurable thresholds. For example: authorized users can submit an order, duplicate submissions do not create duplicate orders, and failures produce actionable feedback.

#### 28. What do you do when the deadline is unrealistic?

I explain the gap using concrete work and dependencies, then offer options such as narrower scope, staged delivery, or a later date. I identify which quality requirements are essential for the deployment context. I seek an explicit tradeoff decision rather than quietly accepting an unachievable commitment.

#### 29. How do you manage project dependencies?

I track each dependency's owner, needed-by date, status, and impact if delayed. I validate critical items such as data access and security review early. Where possible, I use approved mocks or parallel work while preserving a clear distinction between simulated and validated behavior.

#### 30. When should you stop or change direction?

I revisit the plan when evidence undermines its value, feasibility, adoption, or risk assumptions. I compare the remaining investment with expected benefit and alternatives, ignoring sunk cost. I recommend a pivot or stop with supporting evidence and document what the team learned.

### Engineering and system design foundations (31–40)

#### 31. How do you approach designing a customer solution?

I clarify the workflow, users, data, scale, security constraints, and success criteria. I sketch the main components and data flow, identify failure modes, and compare a few viable options. I validate the riskiest assumption before expanding the implementation.

#### 32. How do you choose a technology stack?

I consider the requirements, existing customer systems, team expertise, deployment restrictions, and maintenance burden. I prefer proven components that the eventual owners can operate. I explain the tradeoffs and avoid introducing unfamiliar infrastructure without a clear benefit.

#### 33. When should you build versus buy?

I compare functional fit, integration effort, total cost, security, control, and long-term maintenance. Buying can accelerate common capabilities, while building may make sense for a differentiating workflow or unmet requirement. I test critical assumptions and include migration or exit costs in the decision.

#### 34. What is an API contract, and why does it matter?

An API contract defines inputs, outputs, authentication, errors, and behavior that callers can rely on. Clear contracts let teams work independently and reduce integration surprises. I also define compatibility expectations and validate both successful and unsuccessful interactions.

#### 35. When would you use synchronous versus asynchronous processing?

Synchronous processing suits work that can finish within a reasonable response time and must return an immediate result. Asynchronous processing suits longer tasks, bursts, or work that can complete later. It introduces job status, queue management, duplicate handling, and failure-recovery requirements.

#### 36. What is idempotency?

Idempotency means repeating an operation has the same intended effect as performing it once. It matters when a client retries after an uncertain response. For a create operation, I might use a scoped idempotency key, persist the outcome atomically, and return the previous result for matching retries.

#### 37. Why are retries dangerous without careful design?

Retries can duplicate side effects, overload a struggling dependency, or keep failing work alive indefinitely. I retry only appropriate transient failures, use timeouts, bounded attempts, exponential backoff, and jitter, and respect rate-limit guidance. Side-effecting operations also need deduplication or idempotency.

#### 38. How do you choose between a monolith and microservices?

I start with team size, deployment needs, domain boundaries, and scaling requirements. A modular monolith often reduces coordination and operational overhead for an initial solution. Separate services become useful when independent ownership, isolation, or scaling justifies the distributed-system complexity.

#### 39. What are nonfunctional requirements?

They describe qualities such as availability, latency, security, accessibility, maintainability, and recoverability. I translate vague terms like “fast” into measurable expectations for representative conditions. These requirements influence architecture, testing, and cost as much as functional features do.

#### 40. How do you make a solution maintainable?

I favor clear interfaces, readable code, automated checks around important behavior, and configuration separated from implementation. I document operational assumptions and provide useful logs and runbooks. I also identify an owner and reduce avoidable custom behavior before handoff.

### Data and integration fundamentals (41–50)

#### 41. What do you check before integrating with a customer system?

I verify approved access, authentication, network connectivity, API capabilities, schemas, quotas, and representative data. I ask about sandbox availability, change processes, and system ownership. I also establish failure handling and support contacts before making the integration a critical dependency.

#### 42. What is the difference between authentication and authorization?

Authentication establishes who or what is making a request. Authorization determines which actions that identity may perform on particular resources. A successfully authenticated user must still be checked for tenant, resource, and action permissions.

#### 43. How do you handle poor data quality?

I profile the data for missing values, duplicates, invalid formats, and inconsistent meanings. I agree on validation and remediation rules with the data owner, preserving provenance and reporting rejected records. I avoid silent corrections that could change business meaning.

#### 44. What is a source of truth?

It is the authoritative system or rule for a particular piece of information. Different fields may have different authoritative owners. I document how updates propagate and how conflicts are resolved so multiple copies do not produce contradictory decisions.

#### 45. How do you handle schema changes?

I define compatibility expectations, validate incoming data, and monitor for unexpected changes. Where possible, I add fields compatibly and coordinate migrations before removing old ones. Versioning, contract checks, and replayable processing help limit disruption when breaking changes are unavoidable.

#### 46. When would you use batch versus streaming data processing?

Batch processing works when periodic updates meet the business need and often simplifies operations. Streaming is useful when fresher data materially changes the outcome. I weigh freshness against cost and complexity, including ordering, late events, and recovery.

#### 47. How do you choose between webhooks and polling?

Webhooks can provide timely updates with fewer repeated requests when the provider supports reliable delivery. Polling can be simpler or necessary when events are unavailable. With either method, I address missed updates, duplicates, rate limits, and reconciliation; webhook handlers also need authenticity checks.

#### 48. How do you work when production data access is unavailable?

I request approved synthetic, masked, or representative test data and document the limitations. I build against agreed contracts while pursuing the required access process. Before rollout, I validate assumptions in an approved environment because test fixtures may miss real data edge cases.

#### 49. How do you reconcile data between systems?

I compare records using stable identifiers, totals, and business invariants over a defined time window. I account for expected delays, deletions, and transformations. I investigate mismatches and use a controlled, auditable repair process rather than blindly overwriting one system.

#### 50. How do you migrate customer data safely?

I map and validate transformations, rehearse on representative data, and agree on cutover and recovery plans. I verify counts and business-level correctness, not just successful file transfer. I also plan for writes during migration, access controls, reconciliation, and any irreversible changes.

### Delivery, deployment, and reliability (51–60)

#### 51. What separates a prototype from a production system?

A prototype tests feasibility or usability within a limited context. Production requires appropriate security, reliability, monitoring, deployment controls, recovery procedures, and ownership. I explicitly list the gaps so a convincing demo is not mistaken for a deployment-ready system.

#### 52. How do you define “done”?

Done means meeting agreed acceptance criteria and the operational needs of the release. Depending on scope, that includes validation, deployment, monitoring, documentation, training, and an accountable support owner. I confirm that the customer can complete the intended workflow.

#### 53. What tests should an FDE prioritize?

I prioritize important business behavior and the highest-risk failure modes: permissions, integration contracts, data transformations, and critical user journeys. I include representative unhappy paths and a deployment smoke check. The depth depends on impact and complexity, not a target test count.

#### 54. How do you deploy a change safely?

I verify prerequisites, use a repeatable deployment process, and choose a rollout size appropriate to the risk. I monitor predefined health and business measures and have explicit stop conditions. I prepare rollback or recovery steps, including treatment of any data changes.

#### 55. What is the difference between rollback and roll-forward?

Rollback restores an earlier version or configuration; roll-forward applies a new change to fix the problem. I choose based on recovery time, compatibility, and data effects. A code rollback alone may not undo database changes or external side effects.

#### 56. What should you monitor after launch?

I monitor user task success, errors, latency, traffic, resource limits, and dependency health. I also watch workflow-specific indicators such as stuck jobs or reconciliation failures. Alerts should signal actionable customer impact and have a clear owner and response procedure.

#### 57. What is the difference between an SLI, SLO, and SLA?

An SLI is a measured service indicator, such as the proportion of successful requests. An SLO is the target for that indicator over a defined window. An SLA is an agreement that may include service commitments and consequences; I check the actual agreement before making promises.

#### 58. What do you do first during a production incident?

I assess impact, coordinate an owner, and prioritize safe mitigation such as disabling a faulty feature or reverting a change. I preserve relevant evidence and provide clear updates. Root-cause analysis follows stabilization; it should not delay a known safe recovery action.

#### 59. What belongs in a useful runbook?

A runbook explains the system's purpose, dependencies, dashboards, common symptoms, diagnostic steps, and recovery procedures. It includes escalation contacts and the conditions under which a procedure is safe. I test it with someone who did not build the system.

#### 60. How do you conduct an operational handoff?

I transfer architecture context, deployment procedures, access ownership, monitoring, runbooks, and known limitations. I walk the receiving team through routine tasks and a failure scenario. I confirm support responsibilities and escalation paths, with a defined transition period when needed.

### Communication and stakeholder management (61–70)

#### 61. How do you explain a technical tradeoff to a nontechnical stakeholder?

I describe the options in terms of business effects such as speed, cost, risk, and flexibility. I give a recommendation with the key assumption behind it and use a concrete example. I add implementation detail when it helps the stakeholder make the decision.

#### 62. What makes a good project status update?

It states progress toward the agreed outcome, evidence of what works, current risks or blockers, and the next milestone. Each required action has an owner and date. I highlight changes to the forecast early, instead of relying on a list of completed tasks.

#### 63. How do you communicate bad news?

I share verified facts promptly, explain the customer impact, and separate known information from uncertainty. I present mitigation options, a recommendation, and the next update time. I take responsibility for my part without assigning blame or promising an unsupported recovery date.

#### 64. How do you build trust with a customer?

I listen carefully, keep commitments, explain constraints honestly, and follow up with evidence. I make progress visible and surface risks while there is still time to respond. Trust grows through consistent behavior, including how I handle mistakes and disagreements.

#### 65. What do you do when you do not know the answer?

I say what I know, identify what needs verification, and explain how I will find out. I consult documentation, test the behavior, or involve the right expert. I agree on a follow-up and close the loop with a verified answer.

#### 66. When should an FDE escalate a problem?

I escalate when impact, timing, required authority, or specialist knowledge exceeds what I can resolve safely. I include the problem, affected users, evidence, attempted fixes, options, and my recommendation. I continue coordinating the work after escalation rather than treating it as a transfer of responsibility.

#### 67. How do you push back on a customer request?

I acknowledge the intended outcome, explain the concrete constraint or downside, and propose a viable alternative. I make tradeoffs visible and involve the decision owner where necessary. Clear boundaries are easier to accept when they come with a practical path forward.

#### 68. How do you run an effective customer demo?

I demonstrate a realistic workflow tied to the agreed success criteria using approved data. I explain limitations and identify mocked components. I ask users to assess whether the workflow fits their work, capture feedback, and end with specific decisions or next steps.

#### 69. How do you work with product and sales teams?

I share customer context, technical evidence, constraints, and realistic delivery expectations. I coordinate commitments so the customer receives consistent information. With product, I distinguish reusable needs from local implementation details; with sales, I clarify what is supported, validated, and still exploratory.

#### 70. How do you document decisions without creating excessive overhead?

I record the decision, context, considered options, tradeoffs, owner, and conditions that would trigger reconsideration. Short decision records are usually enough. I keep them near the relevant work and update them when new evidence changes the decision.

### Product judgment, value, and adoption (71–80)

#### 71. How do you translate a customer request into product feedback?

I describe the underlying workflow need, affected users, frequency, business impact, and current workaround. I include evidence and distinguish the need from the requested implementation. I also identify whether other customers share it and what a general solution would need to support.

#### 72. How do you avoid creating a separate product for every customer?

I look for shared capabilities and use configuration or stable extension points for legitimate variations. I review customizations for long-term cost and ownership. When a requirement is unique, I isolate it behind clear interfaces instead of spreading customer-specific branches throughout the codebase.

#### 73. When should a customer-specific capability become a core feature?

It becomes a candidate when multiple customers share the underlying need and a generalized solution fits the product strategy. I provide evidence about demand, value, technical feasibility, and maintenance cost. Product owners then weigh it against other priorities; one urgent deployment alone does not establish broad demand.

#### 74. How do you measure customer value?

I compare the agreed business outcome with its baseline, accounting for adoption, quality, and ongoing costs. Possible measures include cycle time, error reduction, throughput, or revenue impact. I distinguish observed improvements from estimates and avoid claiming causality without enough evidence.

#### 75. Why might a technically correct solution fail?

It may solve the wrong problem, add workflow friction, lack trust, or have no clear owner. Users may lack access, training, incentives, or confidence in its outputs. I investigate real usage and organizational constraints alongside technical correctness.

#### 76. How do you improve adoption?

I involve representative users early, fit the solution into their workflow, and make the first useful action easy. I provide role-appropriate training and a feedback channel. I track usage and successful task completion, then investigate where users abandon the workflow.

#### 77. What do you do when users keep using the old process?

I observe when and why they switch back: missing features, slower performance, access problems, or lack of confidence. I fix the highest-impact obstacle and validate the improvement with users. Any retirement of the old process needs a coordinated transition and a recovery path.

#### 78. How do you balance speed and quality?

I adjust scope and sequencing while preserving requirements essential to the release's risk level. A limited pilot can accelerate learning with fewer users and controlled data. I make remaining limitations explicit and assign owners to production-readiness work before broader rollout.

#### 79. How do you manage technical debt?

I record the shortcut, why it was taken, its consequences, and the condition or date for revisiting it. I prioritize repayment based on operational risk and the work it blocks. Temporary choices should have owners and should not silently become permanent dependencies.

#### 80. What should happen after a successful pilot?

I review evidence against the pilot's criteria and identify what changes at broader scale. I plan capacity, permissions, support, onboarding, costs, and rollout checkpoints. A pilot demonstrates value within its tested conditions; expanding it requires validating the assumptions that change.

### Security, risk, and AI judgment (81–90)

#### 81. What does least privilege mean in practice?

Users and services receive only the access needed for their tasks, for only as long as needed. I scope identities to appropriate resources and actions, separate environments, and review access over time. I avoid shared administrator credentials as an integration shortcut.

#### 82. How should application secrets be handled?

I use an approved secrets manager or platform mechanism, restrict access, and support rotation and revocation. Secrets should not be committed to source control, embedded in client code, or written to logs. If exposed, I revoke or rotate them and investigate the impact through the incident process.

#### 83. What is tenant isolation?

Tenant isolation prevents one customer's users or workloads from accessing another customer's data or resources. I enforce tenant boundaries in authorization, queries, storage, caches, and background jobs as applicable. I validate cross-tenant denial paths rather than relying only on interface filtering.

#### 84. How do you handle sensitive customer data during development?

I use approved environments and minimize the data collected or copied. I prefer synthetic or properly de-identified fixtures where suitable and confirm access, retention, and deletion requirements. I also check logs, analytics, backups, and third-party services for unintended exposure.

#### 85. What do you do if a customer asks you to bypass a security control?

I explain the impact and work with the responsible security or system owner on an approved alternative. Options might include scoped temporary access, an isolated test environment, or a revised workflow. I do not bypass controls simply to meet a deadline.

#### 86. When is AI appropriate for a customer workflow?

AI is a candidate when its capabilities fit the task, suitable data is available, and quality can be evaluated against a baseline. I compare it with simpler rules, search, or process changes. I account for error consequences, latency, cost, privacy, and human review requirements.

#### 87. What is a hallucination, and how do you reduce its impact?

A hallucination is generated content that is unsupported or incorrect despite appearing plausible. I use task-appropriate grounding, constrained outputs, verification, and abstention or escalation paths. I evaluate failures on representative tasks; retrieval and prompting can help but do not guarantee correctness.

#### 88. How do you evaluate an AI proof of concept?

I define representative tasks, a baseline, explicit scoring criteria, and acceptable failure rates before tuning. I measure quality, latency, cost, and the human effort needed to review results. I reserve evaluation cases for checking changes and inspect failure categories, not just an average score.

#### 89. When should a human remain in the loop?

Human review is valuable when errors have significant consequences, uncertainty is high, or actions are difficult to reverse. I define what the reviewer must check and give them enough evidence and authority to intervene. I also measure review quality and workload so approval does not become a rubber stamp.

#### 90. What is prompt injection, and why should an FDE care?

Prompt injection occurs when untrusted content attempts to redirect an AI system's behavior. It matters when customer documents, web pages, or tool outputs enter the model's context. I treat those inputs as data and enforce permissions, tool restrictions, and consequential-action checks outside the model.

### Behavioral and practical scenarios (91–100)

For experience-based questions, use real examples and measured results where available. For hypothetical scenarios, explain your assumptions and decision process; do not present invented outcomes as past achievements.

#### 91. Tell me about a time you solved an ambiguous problem.

A strong answer explains the unclear starting point, how you gathered evidence, and how you narrowed the scope. Describe the assumption you tested, your own actions, and the resulting change. Finish with what you learned and what you would do differently.

#### 92. Tell me about a project that failed.

Choose a real example and state what failed against the original goal. Explain your contribution, when you recognized the problem, and how you responded. Show how the lesson changed a later decision or practice rather than ending with a generic claim about learning.

#### 93. Describe a disagreement with a customer or teammate.

Explain the competing goals or evidence fairly, then describe how you made the decision constructive. Show how you listened, tested assumptions, and involved the appropriate owner. Include the outcome and any remaining tradeoff without portraying the other person as unreasonable.

#### 94. You have one week to deliver a pilot. What do you do?

I confirm the outcome, audience, environment, and acceptance criteria immediately. I choose one complete workflow, validate access and data early, and build a thin end-to-end path before adding polish. I schedule user feedback and reserve time for validation, deployment, and a clear limitations review.

#### 95. A customer says the application is slow. How do you respond?

I clarify which task is slow, who is affected, when it happens, and what response time they expect. I reproduce the issue and trace browser, network, application, and dependency timings to locate the bottleneck. I validate any fix against the same workload and check for regressions.

#### 96. An integration works in testing but fails in production. What do you check?

I first assess impact and choose a safe mitigation. I compare environment configuration, permissions, network rules, dependency versions, quotas, and real data shapes using logs and request identifiers. I avoid exposing secrets during diagnosis and add a check for the discovered failure before redeploying.

#### 97. A sponsor wants to launch, but users say the workflow is not ready. What do you do?

I gather specific examples and compare them with the agreed acceptance criteria and launch risks. I explain the impact to the sponsor and propose options such as fixing a blocker, narrowing the rollout, or extending the pilot. The accountable owner makes an informed decision with the remaining risks recorded.

#### 98. Two customers have urgent issues at the same time. How do you prioritize?

I assess severity, affected users, time sensitivity, commitments, and available workarounds. I coordinate ownership and specialist help so work can proceed in parallel where possible. I communicate priorities and update times to both customers, escalating capacity conflicts instead of silently neglecting one.

#### 99. What would your first 30, 60, and 90 days look like?

In the first 30 days, I would learn the product, deployment process, customer workflows, and team expectations while contributing to a scoped task. By 60 days, I would aim to own a bounded deliverable with feedback. By 90 days, I would aim to own an agreed customer outcome and turn recurring lessons into team improvements, adapting the plan to the actual role.

#### 100. What questions should you ask the interviewer about the FDE role?

Ask how the company defines success, how much work is production engineering, and who owns solutions after launch. Ask how engagements are scoped, how customer needs influence the product, and how engineering quality is maintained across deployments. Request a concrete example of a recent engagement to understand the actual work and tradeoffs.

## How to practice

1. Practice ten questions at a time, answering aloud before reading the sample.
2. Keep a direct answer to roughly one minute, then add an example and tradeoffs when asked.
3. Prepare real stories about ambiguity, delivery, conflict, failure, and measurable customer impact.
4. For scenarios, clarify the outcome and constraints, explain your next action, and define how you would verify success.

Continue with [customer discovery](../02-customer-discovery/README.md), [system design](../03-system-design/README.md), [customer deployment](../17-customer-deployment/README.md), and [behavioral leadership](../19-behavioral-leadership/README.md) for deeper preparation.
