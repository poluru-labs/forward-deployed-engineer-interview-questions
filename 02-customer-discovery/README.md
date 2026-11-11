# Customer discovery

Questions on scoping a customer's problem before writing code: stakeholder mapping, requirements that change in the room, success metrics, and turning a vague workflow into a buildable first version.

This curated set of 100 questions prepares Forward Deployed Engineers for customer-discovery discussions. “Top 100” means a broad study selection, not a measured frequency ranking. Answers describe practical approaches; examples and numbers are hypothetical unless explicitly identified as your own experience.

## Company hashtags and attribution

Each question has a topic hashtag and an explicit company-attribution status. A company relevance tag is an editorial study recommendation, **not evidence that the company asked the question**. Public sources do not establish a company origin for every question; unverified questions are labeled accordingly.

| Hashtags | Meaning and evidence |
| --- | --- |
| `#CompanyUnverified #PalantirRelevant` | Original practice question; thematic relevance inferred from Palantir's description of discovering customer problems and working with data and users. [Role description](https://jobs.lever.co/palantir/e0ab8226-b928-4e3a-bf87-08fe7b1ea595). |
| `#CompanyUnverified #AmazonRelevant` | Original practice question; thematic relevance inferred from Amazon's Customer Obsession, Dive Deep, and Earn Trust principles. [Leadership Principles](https://www.amazon.jobs/content/en/our-workplace/leadership-principles). |
| `#Amazon #CandidateReported` | A public candidate-report page attributes the question to Amazon. This is not independent verification or a claim that it appeared in an FDE interview. Question 91 links its source and role. |
| `#Amazon #OfficialSampleAdapted` | A paraphrase of an official sample question, with the original program and source identified. Question 92 is from DSP program preparation, not an FDE question bank. |

Sources reviewed November 11, 2026. The answers are original study material, not official company answers. Question 91 has a reported “asked at” attribution; question 92 has official sample provenance; the other 98 have unverified company origins. No individual interviewer identity is established by these sources.

## Quick cheatsheet

| Discovery task | Questions or actions to remember |
| --- | --- |
| Establish the goal | Who needs to accomplish what, and why now? |
| Understand reality | “Walk me through the last time this happened.” Observe tools, handoffs, and exceptions. |
| Separate need from solution | Ask what the requested feature would change in the workflow. |
| Map stakeholders | Identify user, champion, sponsor, buyer, approver, system owner, and operator. |
| Gather evidence | Combine interviews, observation, artifacts, and operational data; record limitations. |
| Stay neutral | Ask about past behavior before future preferences; avoid pitching inside the question. |
| Quantify pain | Measure frequency, volume, time, errors, consequences, and affected groups. |
| Define success | Baseline + target + population + measurement window + owner + quality guardrails. |
| Check feasibility | Validate data access, quality, permissions, integrations, latency, and operating constraints. |
| Prioritize | Compare impact, urgency, confidence, effort, dependencies, and risk. |
| Select a pilot | One valuable workflow, representative users, explicit acceptance criteria, and a stop decision. |
| Close the meeting | Summarize facts, assumptions, open questions, decisions, owners, and dates. |
| Hand off | Share the problem brief, workflow map, evidence, scope, risks, and validation plan. |
| Answer an interview scenario | Clarify → gather evidence → compare options → recommend → validate. |
| Answer a behavioral question | Situation → Task → Action → Result → Reflection; use truthful personal evidence. |

**Useful calculations:**

- Workflow effort per period = task volume × average active handling time. Measure waiting time separately.
- Estimated gross time savings = eligible volume × expected adoption × time saved per adopted task. State assumptions and avoid counting the same savings twice.
- Task success rate = tasks meeting the agreed quality threshold / attempted tasks, over a defined window.
- Adoption rate = active intended users / eligible intended users, using an agreed definition of “active.”
- Estimated net benefit = estimated benefit − implementation, operation, review, and change-management costs. Freed capacity does not automatically become cash savings.

**Problem statement:** “[User group] needs to [job] in [context], but [obstacle] causes [measured consequence]. We will test whether [intervention hypothesis] improves [outcome] while preserving [guardrails].”

## Study map

| Questions | Focus |
| --- | --- |
| 1–10 | Discovery foundations and preparation |
| 11–20 | Interview technique and observation |
| 21–30 | Stakeholders and alignment |
| 31–40 | Workflow analysis and problem framing |
| 41–50 | Evidence, metrics, and value |
| 51–60 | Requirements and technical feasibility |
| 61–70 | Prioritization, scope, and pilots |
| 71–80 | Experiments, AI discovery, and validation |
| 81–90 | Communication, adoption, and handoff |
| 91–100 | Behavioral questions and practical cases |

## Questions and detailed answers

### Discovery foundations and preparation (1–10)

#### 1. What is customer discovery in an FDE engagement?

Tags: #CustomerDiscovery #CompanyUnverified #PalantirRelevant

Customer discovery is the process of establishing which customer problem is worth solving, for whom, and under what constraints. I examine actual workflows, consequences, stakeholders, and available evidence before committing to a solution. It continues during delivery as assumptions are tested and new information emerges.

The output is a shared problem definition, success criteria, and a plan to resolve important uncertainties. For example, a request for a dashboard may reveal that the real obstacle is inconsistent ownership of delayed orders; a visualization alone would not fix it.

#### 2. How is discovery different from requirements gathering?

Tags: #DiscoveryFoundations #CompanyUnverified #PalantirRelevant

Requirements gathering describes needed behavior and constraints. Discovery also tests whether the proposed problem and solution deserve investment. I investigate why a capability matters, what happens today, and which alternatives might improve the outcome before translating findings into requirements.

For example, “export a spreadsheet” is a requested capability. Discovery may show that users export data only to reconcile conflicting identifiers. The right requirement might involve reconciliation rather than another export format. The two activities overlap and inform each other throughout delivery.

#### 3. What should you learn before the first customer conversation?

Tags: #Preparation #CompanyUnverified #PalantirRelevant

I review the customer's business context, stated objectives, previous discussions, existing architecture, and known delivery commitments. I distinguish verified information from internal assumptions and prepare a short list of questions that could change the engagement's direction. I also confirm who will attend and what decisions the meeting should enable.

Preparation should improve my questions without locking me into a conclusion. If sales notes say “urgent automation,” I still ask what event created the urgency, which work needs automation, and what evidence supports the expected benefit.

#### 4. How would you structure a first discovery meeting?

Tags: #DiscoveryMeeting #CompanyUnverified #AmazonRelevant

I align on the meeting's purpose, then explore the desired outcome, a recent workflow example, pain points, and relevant constraints. I reserve time to summarize what I heard, check disagreements, and agree on next steps. I avoid using the whole meeting for a product demonstration.

For a hypothetical 45-minute meeting, I might use five minutes for alignment, twenty for the workflow, ten for impact and constraints, and ten for synthesis. The agenda remains flexible when a critical unanswered question deserves more attention.

#### 5. How do you decide what to investigate first?

Tags: #DiscoveryPlanning #CompanyUnverified #PalantirRelevant

I identify assumptions that combine high uncertainty with a large effect on value or feasibility. Then I choose the cheapest credible way to test the most consequential one. This may be a user observation, a data sample, a system-owner conversation, or a focused technical spike.

If a project depends on joining two systems, verifying a usable linking identifier can matter more than refining the interface. I record what result would support, weaken, or invalidate the assumption so the investigation informs an actual decision.

#### 6. What makes a strong discovery hypothesis?

Tags: #Hypotheses #CompanyUnverified #AmazonRelevant

A strong hypothesis names the user, problem, proposed cause or intervention, expected observable change, and relevant conditions. It should be possible to gather evidence against it. I keep problem hypotheses separate from solution hypotheses so enthusiasm for an implementation does not substitute for proof of need.

For example: “Analysts recheck invoices because vendor identifiers disagree across systems” is a cause hypothesis. “A reconciliation step will reduce rechecks without increasing missed errors” is an intervention hypothesis. Each requires different evidence.

#### 7. How do you conduct discovery in an unfamiliar industry?

Tags: #DomainLearning #CompanyUnverified #PalantirRelevant

I learn enough vocabulary and business structure to ask useful questions, then work with domain experts and frontline users to validate my understanding. I ask them to explain a recent case, important exceptions, and the consequences of a wrong decision. I maintain a glossary and mark unresolved concepts.

I do not treat unfamiliar terminology as an excuse to avoid foundational questions. Asking what “approved” means may uncover several approval states with different owners. I confirm the workflow interpretation before translating it into software behavior.

#### 8. How do you time-box discovery without rushing it?

Tags: #Timeboxing #CompanyUnverified #AmazonRelevant

I define the decisions discovery needs to enable and assign time to the highest-impact unknowns. I set review points and stop conditions rather than promising that every uncertainty will disappear. Unresolved low-risk questions can become monitored assumptions, while critical gaps need further investigation or a narrower commitment.

For example, a short discovery period might establish one workflow and a feasible data path while leaving organization-wide rollout questions open. I explain exactly what the evidence supports and what still needs validation during the pilot.

#### 9. What discovery artifacts are most useful?

Tags: #DiscoveryArtifacts #CompanyUnverified #PalantirRelevant

I use a concise problem brief, stakeholder map, current workflow map, evidence log, and assumptions or risks list. A success-measurement plan and a scoped pilot proposal connect those findings to delivery. Each artifact should answer a decision-relevant question rather than exist as a documentation exercise.

For example, the workflow map shows where work waits; the evidence log records how we know; the pilot proposal explains what we will change. I keep them consistent and review important interpretations with the customer.

#### 10. When is discovery sufficient to start building?

Tags: #DiscoveryExitCriteria #CompanyUnverified #PalantirRelevant

I start when the team understands the target user, valuable problem, acceptance criteria, and major constraints well enough to make a bounded investment. Critical feasibility assumptions should be validated or explicitly tested by the first increment. An accountable customer owner must agree on what the initial build will establish.

This is a risk decision, not a document-count threshold. A reversible prototype can begin with uncertainty; a production migration requires stronger evidence. I keep discovery active and revisit scope when new findings contradict the original assumptions.

### Interview technique and observation (11–20)

#### 11. What are good opening questions for a customer interview?

Tags: #InterviewTechnique #CompanyUnverified #AmazonRelevant

I start with context: “What are you responsible for?” and “Walk me through the last time you completed this task.” I follow the example through triggers, tools, decisions, handoffs, and outcomes. This establishes concrete behavior before I ask for opinions or proposed improvements.

If the user says a process is frustrating, I ask what happened in that recent case and what they did next. The objective is to understand the work in their terms, including what already works well and should be preserved.

#### 12. How do you avoid leading questions?

Tags: #NeutralQuestions #CompanyUnverified #AmazonRelevant

I remove assumptions about the problem, solution, and desired answer. Instead of asking whether an AI assistant would save time, I ask how the task is performed, where time is spent, and what users have tried. I keep my reactions neutral so agreement does not become the easiest social response.

When testing a concept, I ask users to perform a task and explain their interpretation before sharing the intended design. Questions about observed behavior and specific examples generally provide stronger evidence than compliments about a pitch.

#### 13. Why ask about past behavior instead of future intent?

Tags: #BehavioralEvidence #CompanyUnverified #AmazonRelevant

Past examples reveal actual constraints, actions, and tradeoffs. Future statements such as “I would use this daily” can reflect politeness or an idealized workflow. I ask when the problem last occurred, what the user did, and what time or money they already spend addressing it.

Future intent still helps explore expectations, but I label it as a prediction. If users express strong interest but cannot recall a relevant situation, I investigate whether the problem is rare, emerging, or less urgent than initially suggested.

#### 14. How do you use follow-up questions effectively?

Tags: #FollowUpQuestions #CompanyUnverified #PalantirRelevant

I follow the user's evidence rather than mechanically reading a script. Useful prompts include “What happened next?”, “Can you show me an example?”, and “How did you know it was complete?” I ask one question at a time and allow pauses so the user can think.

When the answer is ambiguous, I reflect it back in plain language and ask for correction. I avoid repeatedly asking “why” in a way that feels accusatory; asking about conditions and decisions often reveals the same underlying information.

#### 15. How do you interview a user who gives only short answers?

Tags: #InterviewFacilitation #CompanyUnverified #AmazonRelevant

I check whether the topic, timing, or presence of other people makes the conversation difficult. I switch from abstract questions to a concrete recent task and invite a walkthrough using approved artifacts. I explain that understanding the process is the goal, not evaluating the individual's performance.

If the person lacks relevant experience, I ask who performs the work and seek an introduction. I do not fill silence with suggested answers or treat a short interview as evidence that no problem exists.

#### 16. How do you manage a participant who dominates the discussion?

Tags: #InterviewFacilitation #CompanyUnverified #AmazonRelevant

I acknowledge their contribution, summarize it, and redirect toward the discovery objective. In group sessions, I explicitly invite quieter participants to describe their own workflow and use structured turns when needed. I separate strategic commentary from direct accounts of the work.

If seniority prevents candid answers, I schedule individual conversations with appropriate consent. The final synthesis distinguishes whose perspective each finding reflects, so the most confident voice does not silently become the assumed experience of every user.

#### 17. How do you observe a workflow without disrupting it?

Tags: #WorkflowObservation #CompanyUnverified #PalantirRelevant

I agree on permission, timing, confidentiality, and what can be viewed or recorded. I ask the user to perform a normal task and explain key decisions, while saving nonessential questions for a natural pause. I note interruptions, workarounds, handoffs, and places where the observed process differs from documentation.

I avoid copying sensitive information into notes and do not intervene in consequential actions. Afterward, I validate my interpretation with the user, including whether the observed case was representative or unusually simple or difficult.

#### 18. How do you recruit a representative set of users?

Tags: #ResearchSampling #CompanyUnverified #PalantirRelevant

I identify meaningful differences such as role, experience, location, workflow volume, accessibility needs, and system access. I recruit across those groups instead of relying only on the sponsor's preferred users. I include frustrated users, non-adopters, or people using workarounds when relevant.

The sample is designed for learning, not automatically for statistical inference. I record missing perspectives and avoid claiming that a small convenience sample establishes population-wide prevalence. Quantitative validation may be needed for decisions that depend on the size of a pattern.

#### 19. How do you take useful discovery notes?

Tags: #ResearchNotes #CompanyUnverified #AmazonRelevant

I separate direct observations, participant statements, my interpretation, and follow-up questions. I capture context such as role, task, date, and the conditions of the example. I use approved recording and storage practices and minimize personal or sensitive information.

After the interview, I summarize decision-relevant findings while the context is fresh. For instance, “user reopened three records” is an observation; “search is untrustworthy” is an interpretation that still needs evidence. Keeping them separate makes later synthesis more reliable.

#### 20. How do you reduce confirmation bias in discovery?

Tags: #ResearchBias #CompanyUnverified #AmazonRelevant

I state assumptions before research and deliberately ask what evidence would disprove them. I seek contrasting user groups, investigate counterexamples, and review interpretations with another team member where practical. I preserve conflicting evidence rather than selecting only supportive quotes.

For example, if I believe approval delays come from software, I also examine staffing, policy, and missing inputs. A finding that most delay occurs before data entry should change the proposed intervention, even if the team already prefers an application redesign.

### Stakeholders and alignment (21–30)

#### 21. How do you identify all relevant stakeholders?

Tags: #StakeholderMapping #CompanyUnverified #PalantirRelevant

I trace the workflow from trigger to outcome and ask who performs, funds, approves, supports, and is affected by each step. I include system owners, data owners, security, procurement, and downstream teams when their involvement could change delivery. I ask each participant who else sees the problem differently.

I record influence, interests, responsibilities, and engagement needs. This catches hidden dependencies: a sponsor may approve the business case while a separate platform owner controls the only integration path needed for the pilot.

#### 22. How do you distinguish users, buyers, champions, and sponsors?

Tags: #StakeholderRoles #CompanyUnverified #AmazonRelevant

Users perform the work, buyers control the commercial decision, champions actively help the project progress, and sponsors provide authority or strategic backing. One person can fill several roles, but I verify the responsibilities rather than inferring them from a title.

I gather each perspective separately: usability from users, organizational value from sponsors, and purchasing constraints from buyers. A champion's enthusiasm helps access and adoption, but it does not establish budget approval or prove that the broader user population has the same needs.

#### 23. What do you do when executives and frontline users describe different problems?

Tags: #StakeholderAlignment #CompanyUnverified #PalantirRelevant

I treat both descriptions as hypotheses at different levels. I trace how frontline activities contribute to the executive's outcome and look for evidence connecting them. I use examples and process data to distinguish incompatible priorities from descriptions of the same issue at different scales.

For example, an executive may want lower cost while users want fewer repeated checks. If repeated checks drive substantial effort, the goals may align. If they do not, I make the conflict explicit and ask the accountable owner to choose the priority.

#### 24. How do you find the actual decision maker?

Tags: #DecisionAuthority #CompanyUnverified #AmazonRelevant

I ask how similar projects were approved, who controls budget and access, and who can accept delivery or stop a launch. I map decision authority separately for business scope, technical architecture, security, and operations. These decisions often belong to different people.

I confirm the approval path directly with the relevant owners and record it in the engagement plan. This prevents a common failure: obtaining enthusiastic feedback from the working group while discovering late that an absent approver has different acceptance criteria.

#### 25. How do you handle conflicting stakeholder priorities?

Tags: #ConflictResolution #CompanyUnverified #AmazonRelevant

I write down each priority, its underlying outcome, supporting evidence, and consequences of delay. I compare options against the shared goal and identify which tradeoffs require authority beyond the working team. I look for a staged plan when needs can be met sequentially.

If the conflict remains, I ask the accountable decision maker for a choice and document the implications. I avoid averaging incompatible requirements into an expensive solution that satisfies neither group. The decision should also identify when new evidence would justify revisiting it.

#### 26. What if you cannot speak directly to end users?

Tags: #UserAccess #CompanyUnverified #PalantirRelevant

I explain which decisions depend on firsthand workflow evidence and request a bounded access option, such as a short observation session or anonymized walkthrough. Meanwhile, I use approved artifacts, support records, and proxy interviews, labeling their limitations. I do useful work without treating indirect evidence as equivalent to user validation.

If access remains unavailable, I narrow the claims and scope of the pilot. A proposed interface may remain a hypothesis until actual users can test it; sponsor approval alone cannot establish usability.

#### 27. How do you work with a customer champion without becoming biased?

Tags: #CustomerChampion #CompanyUnverified #AmazonRelevant

I use the champion's context and relationships to improve access, but independently validate important claims. I ask to meet users with different roles and levels of enthusiasm and review evidence beyond the champion's examples. I make clear that disagreement helps identify delivery risks.

For example, a power user may consider a manual workaround easy because they created it. New users may struggle with it daily. Including both perspectives prevents the pilot from optimizing only for the person most invested in its success.

#### 28. How do you discover internal incentives and resistance?

Tags: #OrganizationalContext #CompanyUnverified #PalantirRelevant

I ask how each group is measured, what changes the proposed solution would introduce, and who gains or absorbs work. I listen for concerns about accountability, control, job impact, and support burden without assuming resistance is irrational. Private conversations may reveal issues that a group workshop misses.

If automation saves one team's time but creates exception handling for another, I include that cost and ownership in the plan. Adoption requires a workable arrangement across the full process, not just a favorable demo.

#### 29. How do you align a cross-functional discovery team?

Tags: #CrossFunctionalWork #CompanyUnverified #PalantirRelevant

I establish a shared problem statement, learning agenda, decision owner, and responsibilities. Engineering, design, product, and customer-facing teams review the same evidence and distinguish facts from assumptions. Short synthesis sessions help resolve conflicting interpretations before they become incompatible implementation plans.

I assign an owner and due date to each critical follow-up. For example, engineering validates data access while design observes the task and the sponsor confirms the business baseline. Their findings are combined into one decision about pilot scope.

#### 30. What do you do when the sponsor changes mid-engagement?

Tags: #SponsorChange #CompanyUnverified #AmazonRelevant

I brief the new sponsor on the original problem, evidence, commitments, current results, and unresolved decisions. I ask whether strategic priorities and success criteria have changed, then reconfirm ownership and decision authority. I do not assume the earlier mandate transfers automatically.

If the new priorities invalidate the scope, I present the cost and value of continuing, adjusting, or stopping. I preserve continuity for users while the decision is made and update the plan so the delivery team is not working toward an obsolete outcome.

### Workflow analysis and problem framing (31–40)

#### 31. How do you map a current workflow?

Tags: #WorkflowMapping #CompanyUnverified #PalantirRelevant

I identify the trigger, actors, inputs, steps, decisions, systems, handoffs, and end state. I capture normal paths and important exceptions, including where work waits or loops back. I validate the map with people who perform different parts of the process.

For each step, I ask what information is needed, how completion is recognized, and what happens on failure. A useful map distinguishes active work from waiting, because improving a short processing step may barely affect a workflow dominated by approval queues.

#### 32. How do you find the real bottleneck?

Tags: #BottleneckAnalysis #CompanyUnverified #PalantirRelevant

I compare volumes, queue lengths, handling times, wait times, and rework across the workflow. I look for the constraint limiting overall throughput or completion time, then confirm it through representative examples. The most visibly frustrating screen is not always the limiting step.

If cases wait two days for approval but take minutes to enter, faster entry may increase the queue without improving completion. I would test an intervention at the approval step and monitor whether the constraint moves elsewhere after the change.

#### 33. How do you separate symptoms from root causes?

Tags: #RootCauseAnalysis #CompanyUnverified #AmazonRelevant

I start from a specific failure and trace the conditions and decisions that produced it. I examine alternative explanations using logs, artifacts, and interviews rather than relying on a chain of guesses. Multiple contributing causes may be more accurate than a single root cause.

For example, duplicate entry might reflect missing integration, incompatible identifiers, or a required independent check. Each implies a different intervention. I test the causal explanation before removing a step that could serve a legitimate control function.

#### 34. How do you write an actionable problem statement?

Tags: #ProblemFraming #CompanyUnverified #PalantirRelevant

I name the user, job, context, obstacle, and consequence without prescribing an implementation. I include the supporting evidence and distinguish measured facts from estimates. I review the statement with affected users and the business owner to confirm that it captures a worthwhile problem.

For example: “Regional planners cannot identify delayed shipments before the daily dispatch cutoff because status updates arrive in separate systems.” That creates room to compare process, integration, and interface changes rather than assuming a new dashboard is the answer.

#### 35. How do you turn a feature request into a discovery conversation?

Tags: #FeatureRequests #CompanyUnverified #AmazonRelevant

I ask when the requested feature would be used, what the user does today, and what outcome they expect to change. I request a recent example and explore why existing alternatives fail. I acknowledge the idea while keeping the underlying need open to other solutions.

For “add a download button,” I might discover a regulatory submission, a recurring manual calculation, or a desire to move data into another tool. Those needs have different access, formatting, reliability, and integration requirements that the button request leaves unstated.

#### 36. How do you investigate workarounds and shadow systems?

Tags: #Workarounds #CompanyUnverified #PalantirRelevant

I ask users to show approved examples of spreadsheets, personal checklists, scripts, or manual communications that keep the process functioning. I explore why they exist, who maintains them, and what breaks when they are unavailable. I avoid framing the conversation as an audit of individual behavior.

These workarounds often reveal unmet needs and hidden business rules. Before replacing one, I identify which capabilities it supplies, such as exception notes or local reconciliation. Removing the workaround without preserving those functions can make the official process worse.

#### 37. How do you discover edge cases without over-expanding scope?

Tags: #EdgeCases #CompanyUnverified #PalantirRelevant

I ask for recent exceptions, their frequency, their consequences, and how they are resolved. I group them into categories and distinguish rare high-impact cases from low-risk variations. The pilot needs a safe treatment for critical exceptions, but it does not need to automate every possible path.

For a rare unsupported case, a clear manual escalation may be sufficient initially. I document the boundary and measure how often the fallback occurs, so the team can decide later whether automation is justified by real demand.

#### 38. How do you distinguish a process problem from a software problem?

Tags: #ProcessImprovement #CompanyUnverified #AmazonRelevant

I examine whether the obstacle comes from missing capability, unclear ownership, policy, incentives, training, or inconsistent inputs. I compare technical and organizational interventions and look for evidence about which would change the outcome. Several causes may require a combined solution.

If employees cannot act because approval authority is undefined, a faster interface will not resolve the delay. I would clarify ownership and test the revised process, adding software only where it improves execution, visibility, or enforcement of the agreed workflow.

#### 39. What do you do when different users follow different workflows?

Tags: #WorkflowVariants #CompanyUnverified #PalantirRelevant

I map the variations and investigate whether they reflect legitimate differences, inconsistent training, local policy, or adaptation to broken tools. I identify shared steps and the conditions requiring distinct paths. I involve the business owner before recommending standardization.

For example, regional workflows may differ because of different approval obligations. A configurable rule could preserve necessary differences, whereas accidental variations might be simplified. I avoid forcing one process on all users until I understand why the variants exist and what standardization would change.

#### 40. How do you define the boundaries of the problem?

Tags: #ProblemBoundaries #CompanyUnverified #PalantirRelevant

I state the workflow's start and end, target users, systems involved, and outcomes the engagement can influence. I identify adjacent processes and external dependencies without automatically including them in scope. I also record exclusions and any assumptions about upstream or downstream behavior.

For example, improving claim intake may include validating submitted information but exclude redesigning adjudication policy. If the excluded process prevents meaningful benefit, I surface that dependency and revisit the boundary rather than claiming success from an isolated step improvement.

### Evidence, metrics, and value (41–50)

#### 41. How do you establish a baseline when the customer has no metrics?

Tags: #Baselines #CompanyUnverified #AmazonRelevant

I define the task and outcome first, then gather a practical sample through observation, timestamps, or approved records. I include representative variation and distinguish handling time from elapsed time. I document sample size, period, missing data, and any measurement effects.

If direct measurement is initially impossible, I use a clearly labeled estimate and plan to replace it. A rough baseline can guide a small experiment, but I would avoid presenting it as a precise financial forecast or using it to claim proven improvement.

#### 42. What makes a good discovery success metric?

Tags: #SuccessMetrics #CompanyUnverified #AmazonRelevant

A good metric reflects the customer's desired change and has a clear definition, baseline, target, owner, population, and time window. I pair it with guardrails for quality, reliability, and unintended effects. Users and sponsors should understand how it will be measured before the pilot starts.

For example, “reduce median case handling time” needs a comparable case mix and a quality threshold. I would also inspect slower cases or a high percentile so an improved median does not hide a worse experience for difficult cases.

#### 43. How do you quantify customer pain?

Tags: #PainQuantification #CompanyUnverified #PalantirRelevant

I examine frequency, affected population, task volume, time, error rate, and consequences. I distinguish an annoying inconvenience from a repeated obstacle with meaningful operational impact. I ask who bears the cost and what they already do to reduce it.

For example, five minutes of rework repeated across thousands of tasks may justify investment, while an infrequent issue may matter because its failures are severe. I keep consequence and prevalence separate so neither a loud complaint nor a simple average determines priority alone.

#### 44. How do you estimate return on investment responsibly?

Tags: #ValueEstimation #CompanyUnverified #AmazonRelevant

I model expected benefit using explicit assumptions about volume, adoption, improvement, and the value of that improvement. I include implementation, maintenance, infrastructure, review, and organizational change costs. I use ranges or sensitivity analysis for uncertain inputs and validate the model with the business owner.

Time savings represent freed capacity unless there is a credible plan to realize financial value. I avoid double-counting benefits, such as counting both reduced handling effort and the same effort again as increased throughput without accounting for the shared source.

#### 45. How do you combine qualitative and quantitative evidence?

Tags: #EvidenceSynthesis #CompanyUnverified #AmazonRelevant

Qualitative evidence explains mechanisms and context; quantitative evidence helps estimate scale and patterns. I use interviews to form hypotheses, then seek operational data to test prevalence or impact where feasible. I also use unusual data patterns to choose follow-up interviews.

If users report frequent failures but dashboards show few errors, the system may be recording technical success while the task still fails. I investigate definitions and untracked manual work rather than assuming either source is wrong. The strongest conclusion explains both observations.

#### 46. What is a vanity metric in a discovery or pilot context?

Tags: #MetricQuality #CompanyUnverified #AmazonRelevant

A vanity metric looks impressive but provides weak evidence for the decision at hand. Total logins or generated answers may rise without improving task completion, quality, or cost. I ask how movement in the metric relates to the desired customer outcome and what alternative explanations exist.

Usage can still be diagnostically useful. For example, increasing logins combined with declining task success might indicate repeated failed attempts. I interpret activity alongside outcome measures instead of treating it as proof of value by itself.

#### 47. How do you handle contradictory customer evidence?

Tags: #ContradictoryEvidence #CompanyUnverified #PalantirRelevant

I check whether sources refer to different populations, periods, definitions, or workflow variants. I preserve the disagreement and design a focused follow-up to distinguish explanations. I assess source quality and incentives without automatically favoring quantitative data or senior stakeholders.

For example, managers may report quick resolution because their clock stops at assignment, while users include days awaiting confirmation. Aligning the measurement boundary can explain the discrepancy. If it remains unresolved, I communicate the uncertainty and limit commitments that depend on it.

#### 48. How do you know whether you have interviewed enough people?

Tags: #ResearchCoverage #CompanyUnverified #PalantirRelevant

I assess whether important user segments and workflow variants are covered and whether new interviews still change the decision-relevant understanding. Repetition within one narrow group does not establish coverage across the customer. I use the risk of the decision to determine how much evidence is enough.

There is no universal interview count that proves discovery is complete. I document remaining gaps and choose additional research when it could materially change scope, feasibility, or value. Claims about population percentages require an appropriate quantitative design, not interview saturation alone.

#### 49. How do you distinguish correlation from causation when evaluating value?

Tags: #CausalReasoning #CompanyUnverified #AmazonRelevant

An improvement after launch may reflect seasonality, staffing, case mix, or another concurrent change. I identify plausible confounders and use a suitable comparison, such as a randomized rollout where practical or a carefully matched group. I compare similar populations and measurement windows.

When a strong causal design is unavailable, I triangulate evidence and state the limitation. I can report that performance improved during the pilot without claiming the software caused the entire change. That distinction matters when estimating the benefit of expanding it.

#### 50. What evidence would make you recommend stopping an engagement?

Tags: #StopCriteria #CompanyUnverified #AmazonRelevant

I would recommend stopping or changing direction if the problem has insufficient value, required access is infeasible, risks are unacceptable, or users cannot adopt the workflow under realistic conditions. I compare remaining investment with alternatives instead of allowing sunk cost to dominate.

I present the evidence, uncertainty, and options to the decision owner. A failed solution hypothesis may still leave a valuable problem worth addressing differently. I distinguish stopping this implementation from concluding that the customer has no worthwhile need.

### Requirements and technical feasibility (51–60)

#### 51. What technical constraints should you discover early?

Tags: #TechnicalDiscovery #CompanyUnverified #PalantirRelevant

I investigate deployment environments, identity, network access, approved technologies, integration capabilities, data residency expectations, and operational ownership. I ask about scale, latency, availability, and change-control requirements in terms of the actual workflow. Critical constraints should have an authoritative owner and evidence, not just secondhand descriptions.

For example, an assumed outbound API connection may be blocked in the target environment. Testing the approved connectivity path early can prevent building a pilot that works only on a developer's machine. I prioritize constraints that could invalidate the proposed approach.

#### 52. How do you discover data availability and ownership?

Tags: #DataDiscovery #CompanyUnverified #PalantirRelevant

I identify each required data element, its authoritative source, owner, access process, update frequency, and intended use. I request an approved sample to verify schema, coverage, history, and linking identifiers. “The data exists” is not the same as “the application can use it reliably.”

I also investigate missing records, deletions, and the meaning of important fields. If the proposed workflow requires a status that is not recorded anywhere, the solution may first need a data-capture change rather than an integration alone.

#### 53. What should you ask about integrations during discovery?

Tags: #IntegrationDiscovery #CompanyUnverified #PalantirRelevant

I ask which systems must exchange data, which operations are supported, and who controls each endpoint. I confirm authentication, permissions, quotas, schemas, event or polling support, and sandbox access. I also ask about failures, reconciliation, changes, and support ownership.

A small approved end-to-end check can validate the most consequential assumptions. If an API can read records but cannot perform the required write, I make that limitation visible before promising automation. A documented endpoint alone is insufficient evidence that the full workflow is feasible.

#### 54. How do you discover nonfunctional requirements?

Tags: #NonfunctionalRequirements #CompanyUnverified #PalantirRelevant

I ask what happens if the system is slow, unavailable, incorrect, or difficult to operate, then translate the answers into measurable requirements. I explore expected volumes, peak conditions, recovery expectations, accessibility, and support capacity. I distinguish essential constraints from preferences.

For “near real time,” I ask what decision becomes impossible after a delay and what delay is acceptable. A requirement tied to that decision helps compare architectures and cost. I confirm thresholds with the responsible owner instead of selecting arbitrary performance targets.

#### 55. How do you discover security and privacy constraints without giving legal advice?

Tags: #SecurityDiscovery #CompanyUnverified #PalantirRelevant

I identify data classifications, intended users, access boundaries, approved environments, retention expectations, and required review processes. I bring the customer's responsible security, privacy, or legal owners into questions that require their interpretation. I translate confirmed requirements into implementation and acceptance criteria.

For example, I ask whether approved test data may leave the customer environment and who can authorize that use. I record the answer and source of authority rather than inferring permission from technical access or claiming a design guarantees compliance.

#### 56. How do you write testable requirements from discovery findings?

Tags: #AcceptanceCriteria #CompanyUnverified #PalantirRelevant

I connect each requirement to the user need and describe observable behavior, preconditions, outputs, and important failure cases. I agree on acceptance criteria with the business and technical owners. I keep solution choices separate unless a real constraint requires a particular implementation.

For example, “the system must be reliable” becomes specific behavior for a duplicate submission, an unavailable dependency, and an unsuccessful operation. The requirement should tell a reviewer how to determine success without needing to guess what the original conversation meant.

#### 57. What should you do when customer requirements change during discovery?

Tags: #ChangingRequirements #CompanyUnverified #AmazonRelevant

I ask what new evidence or priority caused the change and update the problem statement if needed. I distinguish genuine learning from additional preferences or an unannounced change of objective. I assess the impact on success criteria, dependencies, scope, and timeline before revising commitments.

Discovery is expected to change understanding, so a change is not automatically a failure. However, the team needs an explicit decision record. If the new goal replaces the old one, I retire obsolete assumptions and artifacts instead of letting contradictory requirements accumulate.

#### 58. How do you uncover hidden business rules?

Tags: #BusinessRules #CompanyUnverified #PalantirRelevant

I ask users to explain recent exceptions, approval decisions, thresholds, and cases where they override the normal process. I compare actual behavior with policy documents and confirm which rule is authoritative. I record who can change or override each important rule.

For instance, “urgent orders go first” may conceal regional cutoffs, contractual priorities, and stock constraints. I use examples to resolve ambiguities before encoding the rule. If policy and practice disagree, the business owner must clarify the intended behavior; engineering should not silently choose policy.

#### 59. How do you run a technical feasibility spike?

Tags: #FeasibilitySpike #CompanyUnverified #PalantirRelevant

I define one question, a time limit, the evidence needed, and a decision the result will inform. I build the smallest approved experiment that tests the critical constraint under relevant conditions. I document what was validated and what remains untested.

For example, I might test whether representative records can be retrieved and joined within an agreed latency budget. A successful spike supports that data path, not production readiness for the whole application. I avoid allowing exploratory code to become an unreviewed production dependency.

#### 60. How do you capture operational ownership during discovery?

Tags: #OperationalOwnership #CompanyUnverified #PalantirRelevant

I ask who will deploy, monitor, support, fund, and change the solution after the initial team leaves. I assess the receiving team's skills, access, support hours, and escalation expectations. Their operating capacity influences technology choices and acceptable complexity.

If no owner exists, I raise it as a delivery risk and seek an agreed ownership model before broader rollout. A system with no one responsible for failed jobs or expiring credentials is incomplete even when the main workflow works during a demo.

### Prioritization, scope, and pilots (61–70)

#### 61. How do you prioritize several discovered problems?

Tags: #Prioritization #CompanyUnverified #AmazonRelevant

I compare customer impact, frequency, urgency, confidence in the evidence, delivery effort, dependencies, and risk. I make assumptions visible and use a scoring framework only as a discussion aid. I also consider whether solving one problem unlocks other valuable work.

For example, a shared data-quality issue may deserve priority over multiple interface requests that depend on that data. I ask the accountable owner to confirm the tradeoffs and explain why lower-priority issues are deferred. Uncertain inputs should not produce a falsely precise ranking.

#### 62. How do you define an MVP from discovery?

Tags: #MVP #CompanyUnverified #PalantirRelevant

I select the smallest complete workflow that can test a meaningful value hypothesis with real users. I include the access controls, failure handling, and measurement needed for that context. I state what the MVP excludes and which questions it should answer.

An MVP for invoice review might support one document type and a manual exception path. It still needs to show whether reviewers can complete useful work at acceptable quality. A collection of disconnected screens may be small, but it cannot validate the workflow's value.

#### 63. How do you choose the first pilot users?

Tags: #PilotUsers #CompanyUnverified #PalantirRelevant

I choose users who perform the target workflow frequently enough to generate evidence and can provide timely feedback. I include meaningful variation without expanding beyond the pilot's intended scope. I confirm their access, availability, manager support, and understanding of the pilot's limitations.

Enthusiastic users help initial learning, but testing only with experts can hide onboarding problems. I plan when to include less experienced or skeptical users and label the conditions under which the pilot results are valid before recommending broader adoption.

#### 64. What should a pilot plan contain?

Tags: #PilotPlanning #CompanyUnverified #PalantirRelevant

A pilot plan names the problem, users, hypothesis, scope, baseline, success thresholds, guardrails, duration, and owners. It describes the data and environment, support model, feedback process, and stop or recovery conditions. It also states how the expand, revise, or stop decision will be made.

I include dependencies and prerequisites so the pilot period is not consumed by missing access. For example, intended users should be onboarded and instrumentation verified before the measurement window starts. Otherwise, the results may reflect setup problems rather than workflow performance.

#### 65. How do you manage scope creep while preserving customer trust?

Tags: #ScopeManagement #CompanyUnverified #AmazonRelevant

I acknowledge the new need and assess how it relates to the agreed outcome. I explain the additional effort, dependencies, and risk, then offer options: replace lower-priority work, stage the request, add capacity where useful, or change the timeline. I record the customer's decision.

The conversation should make the tradeoff concrete. If a new integration adds an external approval dependency, I explain that dependency instead of merely saying it is “out of scope.” This helps the customer choose deliberately while keeping commitments credible.

#### 66. What do you do when a deadline is fixed but scope is uncertain?

Tags: #DeadlineTradeoffs #CompanyUnverified #AmazonRelevant

I clarify what must be true by the deadline: a decision, demonstration, pilot, or production capability. I identify a minimum useful outcome and resolve the riskiest dependencies first. I distinguish committed scope from optional work and report changes to confidence early.

If production readiness cannot fit the date, I explain viable alternatives such as a controlled demonstration or narrower release. I do not relabel an unvalidated prototype as production simply to meet the calendar. The customer needs a clear statement of what the milestone actually establishes.

#### 67. How do you balance one customer's urgent request with reusable product needs?

Tags: #ProductTradeoffs #CompanyUnverified #PalantirRelevant

I identify the underlying need and compare a configuration, an extension, a temporary workaround, and a core product change. I assess customer urgency alongside maintenance cost, broader demand, and roadmap fit. I involve product and engineering owners before making commitments that affect shared capabilities.

If a custom extension is justified, I isolate it behind clear interfaces and assign ownership. I also record evidence that might support later generalization. Reuse is valuable, but speculative abstraction can delay learning just as uncontrolled customization can create long-term cost.

#### 68. What should you do when a discovery dependency is blocked?

Tags: #DependencyManagement #CompanyUnverified #PalantirRelevant

I identify the owner, reason, needed-by date, and consequence of the block. I pursue an approved alternative such as synthetic data or a contract mock while continuing independent work. I make clear which assumptions remain unvalidated and escalate with options if the critical path is threatened.

For example, mock integration can support interface development but cannot establish real permissions or data quality. If access remains blocked, I adjust the pilot claim and schedule rather than presenting simulated behavior as proof that production integration will work.

#### 69. How do you decide which exceptions to include in a pilot?

Tags: #PilotScope #CompanyUnverified #PalantirRelevant

I classify exceptions by frequency, consequence, detectability, and available fallback. Common cases that determine value and rare cases with severe consequences need appropriate coverage. Other cases can be explicitly excluded if users can recognize them and follow a safe alternative process.

For example, an unusual document format may be routed to manual review while the pilot focuses on standard forms. I measure fallback volume and effort. If most real work falls outside the supported path, the pilot's nominal scope is too narrow to demonstrate meaningful value.

#### 70. How do you set pilot exit criteria?

Tags: #PilotExitCriteria #CompanyUnverified #AmazonRelevant

I agree in advance on the evidence required to expand, revise, or stop. Criteria include outcome improvement, quality thresholds, user participation, operational readiness, and acceptable cost. I define the measurement window and how insufficient or inconclusive evidence will be handled.

For example, low usage may mean the pilot cannot evaluate the value hypothesis at all. I would investigate access and workflow fit rather than declare success from a few favorable examples. Meeting a demo milestone should not automatically trigger organization-wide deployment.

### Experiments, AI discovery, and validation (71–80)

#### 71. How do you test a solution idea before building the full system?

Tags: #ExperimentDesign #CompanyUnverified #PalantirRelevant

I choose the uncertainty first, then the experiment. A workflow walkthrough can test process fit, a clickable prototype can test comprehension, and a technical spike can test feasibility. I define an observable result and a decision threshold before running the exercise.

For example, users can attempt a realistic approval task in a prototype while I observe mistakes and uncertainty. That tests usability, not production latency or financial return. I avoid using evidence from one kind of experiment to claim that all major risks are resolved.

#### 72. When is a manual or concierge test useful?

Tags: #ConciergeTesting #CompanyUnverified #AmazonRelevant

A manual test is useful when the team can deliver the proposed outcome with human effort and learn whether users value it before automating. I disclose the setup appropriately, use approved data handling, and record the manual work needed. The test should have a clear scope and endpoint.

For instance, manually assembling a daily exception list may reveal whether it changes decisions. It does not prove the process can scale economically. I measure demand and usefulness separately from automation feasibility and include the human effort in later cost estimates.

#### 73. How do you distinguish usability testing from value validation?

Tags: #ValidationMethods #CompanyUnverified #AmazonRelevant

Usability testing checks whether people can understand and operate a solution. Value validation asks whether using it improves a meaningful outcome enough to justify its costs. A smooth interface can solve an unimportant problem, while a valuable workflow can still need usability improvements.

I evaluate both through appropriate tasks and measures. Successfully finding an answer in a prototype supports usability; reducing resolution time at acceptable accuracy during real work supports value. I avoid treating positive comments or task completion alone as proof of business impact.

#### 74. What do you ask when a customer says, “We need AI”?

Tags: #AIDiscovery #CompanyUnverified #PalantirRelevant

I ask which user task should improve, what happens today, and why existing approaches are insufficient. I explore available inputs, acceptable errors, review requirements, privacy constraints, latency, and operating cost. I compare AI with rules, search, integration, or a process change.

For example, the customer may need reliable routing rather than conversational generation. I would test the simplest credible baseline and use AI only if it provides a meaningful advantage under representative conditions. The technology request is a starting hypothesis, not the success criterion.

#### 75. How do you discover whether a RAG solution is appropriate?

Tags: #RAGDiscovery #CompanyUnverified #PalantirRelevant

I ask which questions users need answered, what sources contain authoritative information, how current those sources are, and which permissions apply. I investigate whether retrieval can find sufficient evidence and whether users need citations or exact structured results. I compare the approach with ordinary search or direct database queries.

For example, policy interpretation may benefit from grounded answers, while an exact account balance should usually come from the authoritative system. I evaluate retrieval and answer correctness separately, including cases where the available evidence cannot support an answer.

#### 76. How do you discover the right level of AI autonomy?

Tags: #AIAutonomy #CompanyUnverified #PalantirRelevant

I examine each proposed action's consequence, reversibility, required authority, and detectability of errors. I ask whether the system should suggest, draft, request approval, or execute. I confirm who is accountable and what evidence a reviewer needs to make an informed decision.

A draft response and a financial transaction require different controls. I would start with bounded capabilities, enforce permissions outside the model, and use explicit approval for consequential actions where needed. Autonomy should expand only when evaluation and operational evidence justify it.

#### 77. What do you need before evaluating an AI pilot?

Tags: #AIEvaluation #CompanyUnverified #PalantirRelevant

I need representative tasks, an agreed baseline, a scoring rubric, and a definition of unacceptable failures. I confirm who can judge correctness and how disagreements will be resolved. I measure latency, cost, review effort, and workflow outcomes alongside model-level quality.

The evaluation should include difficult, ambiguous, and unsupported cases rather than only demonstrations that worked. I separate development examples from held-out checks where practical and examine performance across relevant user or task groups. One average score can conceal failures that make deployment unsuitable.

#### 78. How do you gather useful feedback on an AI prototype?

Tags: #AIFeedback #CompanyUnverified #AmazonRelevant

I capture the task, approved input context, output, user action, and reason the result helped or failed. I distinguish factual errors, missing evidence, incorrect workflow assumptions, and interface confusion. I use structured categories with space for explanation instead of relying only on a satisfaction score.

For example, a rejected answer may be correct but too late to be useful, or well written but based on an obsolete policy. Those failures require different fixes. I protect sensitive content in feedback logs and verify that changes improve representative tasks.

#### 79. What do you do when customers like the demo but will not commit to a pilot?

Tags: #DemandValidation #CompanyUnverified #AmazonRelevant

I investigate the gap between positive reaction and a concrete commitment. Possible barriers include insufficient urgency, unclear ownership, access constraints, competing priorities, or an unconvincing business case. I ask what would need to be true for them to allocate users, data access, and time.

I propose a bounded next step only if it resolves a real uncertainty. Repeated praise without participation is weak evidence of demand. If the opportunity remains low priority, I record that conclusion rather than repeatedly polishing the demonstration.

#### 80. How do you respond when an experiment disproves your preferred solution?

Tags: #LearningFromExperiments #CompanyUnverified #AmazonRelevant

I check whether the experiment tested the intended hypothesis under credible conditions. If it did, I update the recommendation and explain what the result means for the problem, solution, and remaining options. I preserve the learning so the team does not repeat the same assumption later.

For example, if users already know which cases need attention but lack authority to act, better recommendations will not resolve the issue. I would revisit the intervention with the customer rather than changing success criteria to make the original idea appear successful.

### Communication, adoption, and handoff (81–90)

#### 81. How do you summarize discovery for executives?

Tags: #ExecutiveCommunication #CompanyUnverified #AmazonRelevant

I lead with the problem, business impact, recommendation, and decision required. I summarize the strongest evidence, important uncertainties, cost or timing implications, and the next validation step. Detailed workflow and technical material remains available for follow-up rather than obscuring the decision.

For example, I might recommend a narrow pilot because the need is established but data quality remains uncertain. The executive summary should make that distinction obvious, including what resources are requested and what evidence will determine whether further investment is justified.

#### 82. How do you communicate uncertainty without appearing unprepared?

Tags: #UncertaintyCommunication #CompanyUnverified #AmazonRelevant

I separate verified facts, assumptions, and unknowns, then explain how each important unknown affects the decision. I give a concrete validation plan, owner, and expected checkpoint. Preparation is demonstrated by knowing where evidence is weak and how to strengthen it.

Instead of saying “the integration should work,” I state which operations were tested and which permissions remain unconfirmed. Where useful, I provide a range with assumptions. I avoid invented confidence percentages that suggest more precision than the evidence supports.

#### 83. How do you tell a customer that their preferred solution is unlikely to work?

Tags: #CustomerPushback #CompanyUnverified #AmazonRelevant

I acknowledge the intended outcome, explain the evidence and specific failure mode, and offer alternatives that address the same need. I distinguish a hard constraint from a concern that could be tested. I invite correction if my understanding of the workflow is incomplete.

For example, if the requested automation depends on unavailable data, I can propose a data-capture change or a smaller assisted workflow. The conversation should end with a decision or validation step, not simply a rejection of the customer's idea.

#### 84. How do you close a discovery meeting effectively?

Tags: #MeetingClosure #CompanyUnverified #AmazonRelevant

I summarize the problem, evidence, decisions, and unresolved questions in plain language and ask participants to correct misunderstandings. I confirm actions with named owners and dates, including any approval or access needed before the next step. I send a concise record through the agreed channel.

I distinguish a decision from a suggestion that still needs review. For example, discussing a pilot does not mean the sponsor approved it. Explicitly confirming the status prevents the delivery team from treating an exploratory conversation as a commitment.

#### 85. How do you discover training and change-management needs?

Tags: #ChangeManagement #CompanyUnverified #AmazonRelevant

I ask how users learn current tasks, what skills and access they have, and how the new workflow changes responsibilities. I identify affected managers, support teams, and downstream users. I observe where users need help instead of assuming a written guide will be enough.

For example, an automated recommendation may require users to learn how to challenge an output and handle exceptions. I include that training, time commitment, and support burden in pilot planning and value estimates. Adoption work is part of delivery scope, not an afterthought.

#### 86. How do you investigate low adoption after launch?

Tags: #AdoptionDiscovery #CompanyUnverified #AmazonRelevant

I define the eligible user group and examine the path from access to first use to successful repeated use. I combine usage evidence with interviews of users and non-users, checking for availability, trust, workflow fit, incentives, and performance problems. I avoid assuming that more training is always the answer.

If users try the tool once and return to spreadsheets, I observe the switching point. A missing exception path may explain the pattern better than awareness. I test a focused improvement and verify whether task success and repeat use change.

#### 87. How do you hand discovery findings to engineering?

Tags: #EngineeringHandoff #CompanyUnverified #PalantirRelevant

I provide the problem statement, users, workflow, supporting evidence, acceptance criteria, constraints, and prioritized scope. I include assumptions, unresolved questions, and links to approved examples. I explain why decisions were made so engineers can assess alternatives without losing the customer context.

A walkthrough helps the team challenge ambiguity before implementation. I keep a customer feedback path open and identify who can answer domain questions. Handoff should transfer understanding and decision authority, not merely deliver a list of features with no rationale.

#### 88. How do you keep discovery findings current during delivery?

Tags: #ContinuousDiscovery #CompanyUnverified #PalantirRelevant

I maintain regular contact with representative users, review feedback from working increments, and monitor the assumptions that shaped scope. I update the problem brief and decision records when new evidence changes the plan. Important changes are reviewed with the accountable owners.

For example, real data may reveal a workflow variant that interviews missed. I assess its frequency and impact before expanding scope. Continuous discovery should improve delivery decisions while preserving a clear record of what the team has committed to build.

#### 89. What do you do when the customer disputes your findings?

Tags: #FindingsReview #CompanyUnverified #AmazonRelevant

I ask which specific observation or interpretation they disagree with and review the evidence together. I check whether the sample, definitions, or context missed something important. I correct errors openly and propose targeted follow-up when the disagreement cannot be resolved from existing evidence.

If a sponsor dislikes a valid result, I explain its implications respectfully and distinguish their preference from the observed finding. I document uncertainty and the final decision without rewriting the evidence to support a more convenient conclusion.

#### 90. How do you turn one engagement's discoveries into reusable learning?

Tags: #ReusableLearning #CompanyUnverified #PalantirRelevant

I extract recurring workflow needs, integration patterns, failure modes, and questions that would improve future discovery. I separate general lessons from customer-specific details and follow confidentiality requirements when sharing them. I provide product and engineering teams with evidence and limits, not just a requested feature.

For example, repeated difficulty reconciling identifiers might motivate shared integration tooling. One customer's custom naming convention would remain local. I track whether the lesson appears elsewhere before treating it as a broad market requirement or committing it to the roadmap.

### Behavioral questions and practical cases (91–100)

For behavioral questions, use a real example and distinguish your actions from the team's work. The answer guidance below is a structure, not a fictional personal story. Scenario numbers are illustrative assumptions, not reported customer results.

#### 91. Tell me about a time you had to deal with a difficult customer, how did you handle it?

Tags: #CustomerConflict #Amazon #CandidateReported

**Attribution:** This wording appears on a [Glassdoor Amazon interview-question page](https://www.glassdoor.com/Interview/Tell-me-about-a-time-you-had-to-deal-with-a-difficult-customer-how-did-you-handle-it-QTN_8196271.htm), labeled Customer Service Associate. It is a candidate-reported attribution, not an independently verified FDE interview question. The page does not establish an individual interviewer identity.

Choose a real situation and explain the customer's concern without blaming them. Describe how you clarified the underlying need, gathered evidence, set expectations, and agreed on a practical next step. Be specific about your own actions and any boundary you needed to maintain.

Close with the observed result and what you changed afterward. A strong discovery connection is showing how an apparently difficult request revealed a legitimate unmet need. Do not invent a satisfaction score or imply the customer accepted every decision if that did not happen.

#### 92. Describe a time you communicated a major change in direction and expected stakeholder concerns.

Tags: #DirectionChange #Amazon #OfficialSampleAdapted

**Attribution:** Adapted from a sample in Amazon's [2022 Delivery Service Partner interview preparation document, page 3](https://m.media-amazon.com/images/G/01/DSP2022/assets/mobile/DSP_Interview_Prep_2022.pdf#page=3). This is official sample provenance for the DSP program; it does not establish use in an FDE interview or an actual candidate interview.

Use a real example in which evidence required changing the plan. Explain the original goal, the new finding, affected stakeholders, and alternatives you considered. Describe how you communicated the reason, acknowledged concerns, and made the implications for scope, timing, and ownership concrete.

Then explain how you collected feedback, obtained the needed decision, and verified that people understood the next steps. Include the result and any remaining tradeoff. For discovery interviews, emphasize how you updated the recommendation while preserving trust and the underlying customer objective.

#### 93. Tell me about a time discovery changed your initial assumption.

Tags: #BehavioralDiscovery #CompanyUnverified #AmazonRelevant

Choose a real example with a clear initial belief and evidence that challenged it. Explain how you gathered that evidence, why it was credible, and which decision changed as a result. Describe your role in updating the team and customer rather than presenting the shift as something that happened automatically.

The strongest answer shows a concrete consequence: a different scope, avoided work, or a better outcome. Finish with how you changed your discovery practice afterward. An example in which you searched only for supporting evidence does not demonstrate willingness to learn.

#### 94. A customer asks for a dashboard to fix late deliveries. How do you discover the real need?

Tags: #DeliveryCase #CompanyUnverified #PalantirRelevant

I clarify who needs to act, what “late” means, when intervention is possible, and what decisions a dashboard would support. I trace recent late deliveries through planning, inventory, dispatch, and communication. I compare on-time cases and measure where delays or missing information arise.

If the issue is stale status data, I test whether fresher information leads to useful action. If users already know about delays but cannot change dispatch decisions, the intervention may involve authority or process changes. I define a pilot around improved delivery outcomes, not dashboard completion alone.

#### 95. A support team wants an AI assistant. What do you discover before proposing one?

Tags: #SupportAICase #CompanyUnverified #PalantirRelevant

I segment support tasks by volume, difficulty, consequence, and available knowledge. I observe how agents find answers, verify them, and escalate uncertain cases. I investigate source freshness, permissions, sensitive content, and whether the need is retrieval, drafting, routing, or execution.

I choose a bounded task and compare an AI approach with the current process or a simpler improvement. The pilot measures successful resolution, quality, review effort, latency, and cost. I include unsupported questions and escalation paths so a fluent but incorrect answer is not counted as success.

#### 96. You have two days for discovery before a pilot commitment. What do you do?

Tags: #RapidDiscoveryCase #CompanyUnverified #AmazonRelevant

I clarify which commitment is needed and identify the few unknowns most likely to invalidate it. I prioritize a sponsor discussion, a representative user walkthrough, and validation of the critical data or integration path. I use existing approved evidence to avoid duplicating work.

At the end, I present a narrow pilot proposal with explicit assumptions, prerequisites, and stop conditions. If critical access remains unverified, I commit only to a stage that resolves it. I explain what two days of evidence can support rather than implying the full deployment risk has disappeared.

#### 97. The customer says a task takes ten minutes, but logs show one minute. How do you investigate?

Tags: #MeasurementCase #CompanyUnverified #AmazonRelevant

I align the measurement boundaries before deciding either account is wrong. Logs may capture only a server operation while the user includes searching, preparation, waiting, and rework. I observe representative tasks and compare timestamps, task definitions, case complexity, and untracked actions.

I then report active handling time, system time, and elapsed time separately as appropriate. If the difference remains, I gather another sample and investigate instrumentation gaps. A shared definition becomes the baseline for evaluating improvement so later comparisons do not repeat the same ambiguity.

#### 98. The sponsor wants automation, but users fear losing control. How do you proceed?

Tags: #AutomationCase #CompanyUnverified #AmazonRelevant

I explore which decisions users believe require judgment and what happens when automation is wrong. I distinguish concerns about accountability, job changes, missing context, and poor prior experiences. I map which steps can safely be assisted and which require review or a manual path.

I propose a bounded test with transparent outputs, appropriate override options, and clear ownership. I measure both outcome improvement and exception-handling effort. The sponsor needs an honest account of adoption risks; user participation should inform the design rather than serve as a ceremonial approval step.

#### 99. Several departments want different versions of the same solution. How do you scope discovery?

Tags: #MultiDepartmentCase #CompanyUnverified #PalantirRelevant

I map the common job and the reasons for each variant, including policy, data, ownership, and operating conditions. I compare the value and cost of a shared workflow with configurable extensions versus separate implementations. I identify an owner for cross-department decisions.

I choose a pilot that validates the common core while testing the most consequential variation. I do not assume the largest department's process should become everyone's default. Before expansion, I confirm which differences are necessary and who will maintain the resulting configuration or custom behavior.

#### 100. What questions should you ask an interviewer about customer discovery at their company?

Tags: #InterviewerQuestions #CompanyUnverified #PalantirRelevant

Ask who owns discovery, how FDEs access users, and how evidence changes scope or product priorities. Ask how success is measured and what happens when an engagement's original hypothesis is wrong. Request a recent example of a customer request that led to a different solution after investigation.

Also ask how discovery time is balanced with delivery commitments and who approves changes to those commitments. These questions reveal actual incentives and decision authority. Follow up on concrete examples to understand whether FDEs can challenge assumptions or are mainly expected to implement already-promised features.

## Practice and reusable discovery brief

Practice one group of ten questions at a time. Start with a direct answer, then explain evidence, tradeoffs, and how you would validate the result. Prepare truthful examples of learning from users, changing scope, resolving conflict, and measuring value.

Use this brief to turn a discovery conversation into a reviewable plan:

| Field | Capture |
| --- | --- |
| Problem and users | Who needs to do what, in which context? |
| Current workflow | Trigger, steps, systems, handoffs, exceptions, and completion condition. |
| Evidence | Observations, approved artifacts, data, dates, sample limitations, and contradictory findings. |
| Impact | Frequency, volume, time, errors, and business consequences. |
| Stakeholders | User, champion, sponsor, decision maker, approvers, and operational owner. |
| Desired outcome | Baseline, target, quality guardrails, measurement window, and owner. |
| Constraints | Data, access, integrations, environment, security, budget, and timeline. |
| Hypotheses and risks | What must be true, what remains unknown, and how to test it. |
| Pilot scope | Included workflow, exclusions, users, prerequisites, and fallback. |
| Decision | Expand, revise, or stop criteria; accountable owner and review date. |
| Next actions | Named owner and date for every critical follow-up. |

Continue with [FDE fundamentals](../01-fde-fundamentals/README.md), [system design](../03-system-design/README.md), [customer deployment](../17-customer-deployment/README.md), and [behavioral leadership](../19-behavioral-leadership/README.md).
