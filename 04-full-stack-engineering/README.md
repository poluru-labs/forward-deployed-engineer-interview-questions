# Full-stack engineering

End-to-end product questions that cross UI, API, data, and deployment. Focus is on shipping a working slice for a customer, not on a single layer in isolation.

This curated set of 100 questions covers full-stack engineering fundamentals and practical Forward Deployed Engineer scenarios. “Top 100” means a broad study selection, not a measured frequency ranking. Answers use JavaScript, TypeScript, React, Node.js, and relational databases as examples; the underlying reasoning applies to other stacks. Example workloads and outcomes are hypothetical, not claimed personal achievements.

## Company hashtags and attribution

Each question includes a topic and company-status hashtags. **A relevance tag is a preparation recommendation, not evidence that the company asked the question.** An unknown origin is explicitly labeled rather than assigned to a company without support.

| Hashtags | Meaning |
| --- | --- |
| `#CompanyUnverified #AmazonRelevant` | Original practice question with unverified company origin. Relevance is an editorial inference from Amazon's broad [software-development topics](https://www.amazon.jobs/content/en-gb/how-we-hire/interview-prep/software-development-topics) and [front-end preparation guide](https://www.amazon.jobs/content/en/how-we-hire/university/front-end-engineer), not attribution of this particular prompt. |
| `#Amazon #CandidateReportedAdapted` | Question 12 paraphrases a candidate-reported browser event-loop prompt attributed to an Amazon front-end interview. The report is linked beside the question. |
| `#Ubiquiti #CandidateReported` | Question 31 follows a reported Node.js event-loop prompt attributed to a Ubiquiti full-stack TypeScript interview. |
| `#AVBMarketing #CandidateReportedAdapted` | Question 91 adapts the table-to-detail API interaction from a reported AVB Marketing React TypeScript full-stack interview. The original entry also includes other prompts that are not attributed here. |

Sources reviewed November 15, 2026. There are 97 questions with unverified company origins and three candidate-reported company attributions. Candidate reports are not independent verification, and they do not establish that the questions appeared in FDE interviews. No individual interviewer identities are established. Technical references support concepts, not company provenance; these answers are original study material, not official company answers.

## Quick cheatsheet

| Area | Remember |
| --- | --- |
| End-to-end flow | User action → browser request → authentication and authorization → validation → business logic → data or dependency → response → UI state. |
| Build approach | Clarify one useful workflow, define acceptance criteria, build a thin working slice, and validate it in the target environment. |
| HTML and CSS | Prefer semantic elements, responsive layouts, visible focus, meaningful labels, and native interactions. |
| JavaScript | Understand scope, closures, references, promises, tasks, microtasks, cancellation, and bounded concurrency. |
| TypeScript | Static types help development; validate untrusted data at runtime. Prefer `unknown` plus narrowing over unchecked assertions. |
| React | State is a render snapshot. Keep state minimal, use stable keys, and synchronize external systems with correctly cleaned-up Effects. |
| Async UI | Model loading, success, empty, and error states. Prevent old responses from replacing current results. |
| HTTP | Use clear contracts and status semantics. `fetch` does not reject solely because a response has an HTTP error status. |
| API reliability | Define deadlines, idempotency, safe retries, pagination, compatibility, and dependency failure behavior. |
| Database | Enforce invariants with constraints and suitable transactions or conditional writes; index real query patterns. |
| Identity | Authentication identifies the caller; authorization checks the caller's action on the actual resource and tenant. |
| Browser security | CORS controls browser cross-origin response access; it is not authentication or a complete CSRF defense. |
| Session security | Use an appropriate session design, secure cookie settings, expiry, revocation, and CSRF defenses for cookie-authenticated actions. |
| Testing | Verify important behavior at the appropriate layer; include denied access, races, integration failures, and critical journeys. |
| Performance | Measure browser, network, API, query, and dependency timings before optimizing. |
| Deployment | Keep configuration explicit, secrets server-side, migrations compatible, and rollback or recovery steps reviewable. |
| Production ownership | Include task-success metrics, logs, traces, alerts, runbooks, support ownership, and recovery validation. |

**HTTP reminders:** `200` successful response; `201` resource created; `202` accepted for processing; `204` success without a response body; `400` invalid request; `401` valid authentication required; `403` access refused; `404` resource not found or intentionally undisclosed; `409` state conflict; `429` rate limit; `5xx` server-side failure. Choose precise semantics for the contract rather than treating every failure as `500`.

**Useful calculations:** average request rate = requests/day ÷ 86,400; payload bandwidth = requests/second × average bytes/response; estimated storage = records/period × bytes/record × retention periods, plus indexes and copies. In a stable workload, approximate in-flight work = arrival rate × average time in the system. Treat these as estimates to validate with realistic measurements.

**Answer pattern:** state the requirement → propose an approach → explain a tradeoff → walk through a failure case → describe verification.

## Study map

| Questions | Focus |
| --- | --- |
| 1–10 | End-to-end web and browser foundations |
| 11–20 | JavaScript, TypeScript, and asynchronous work |
| 21–30 | React and frontend state |
| 31–40 | Backend services and runtime behavior |
| 41–50 | APIs and client-server integration |
| 51–60 | Databases and data correctness |
| 61–70 | Authentication and application security |
| 71–80 | Testing, debugging, and performance |
| 81–90 | Delivery, deployment, and operations |
| 91–100 | Practical full-stack and FDE scenarios |

## Questions and detailed answers

### End-to-end web and browser foundations (1–10)

#### 1. What does full-stack ownership mean in an FDE role?

Tags: #FullStackOwnership #CompanyUnverified #AmazonRelevant

It means connecting a customer's workflow to a functioning application across interface, API, data, integration, and deployment boundaries. I clarify the outcome, implement a useful slice, validate it with users, and ensure someone can operate it. Ownership includes coordinating specialists rather than doing every task personally.

For example, a working approval screen is incomplete if the API ignores permissions or the customer cannot access it through their network. I define acceptance criteria across the whole journey and measure successful user tasks, not just whether individual components compile or return responses.

#### 2. What happens when a user enters a URL in a browser?

Tags: #RequestLifecycle #CompanyUnverified #AmazonRelevant

Depending on cached state and the protocol, the browser resolves the host, establishes or reuses a connection, negotiates secure transport, and sends an HTTP request. Servers or intermediaries return a response. The browser parses HTML, discovers resources, builds document and style representations, and performs layout and rendering as needed.

JavaScript and additional requests may update the page afterward. I trace both the initial document and subsequent application calls when debugging. A slow experience might come from connection setup, resource loading, script execution, or the backend; naming the full path helps identify which layer to measure.

#### 3. How do you choose between client rendering, server rendering, and static generation?

Tags: #RenderingStrategies #CompanyUnverified #AmazonRelevant

I compare initial content needs, interactivity, personalization, freshness, search visibility, and deployment complexity. Client rendering relies on browser execution to build the interface. Server rendering generates content per request or through an appropriate cache. Static generation prepares content ahead of time and needs a refresh strategy when data changes.

Hybrid approaches can suit different routes in one application. An authenticated operational tool and a public documentation page have different priorities. I also account for server cost, hydration, caching, and permissions instead of assuming one rendering strategy automatically provides better performance or security for every workflow.

#### 4. Why does semantic HTML matter?

Tags: #SemanticHTML #CompanyUnverified #AmazonRelevant

Semantic elements communicate purpose to browsers, assistive technology, and developers. A button has interaction behavior that a styled generic element does not supply automatically. Headings, labels, landmarks, and properly structured tables help users understand and navigate the page.

I begin with native elements and add custom behavior only when required. For example, a clickable card may need a real link for navigation and a separate button for an action. I verify keyboard operation and accessible names because visual similarity alone does not establish equivalent behavior or usability.

#### 5. How do you build a responsive layout?

Tags: #ResponsiveCSS #CompanyUnverified #AmazonRelevant

I start with content priorities and use flexible layout primitives such as grid or flexbox, appropriate sizing, and breakpoints based on where the content needs to change. I consider long labels, localization, zoom, touch targets, and tables rather than checking only a few device widths.

For an enterprise dashboard, secondary panels may stack while important actions remain reachable. I avoid hiding essential functionality just to make a screenshot fit. I verify overflow, focus visibility, and real content at different viewport sizes, including errors and loading states that can alter the layout.

#### 6. How do you make a form accessible and usable?

Tags: #AccessibleForms #CompanyUnverified #AmazonRelevant

I provide visible labels, appropriate input types, clear required-field instructions, and errors associated with the relevant controls. I support keyboard submission and logical focus movement. I preserve entered values when validation fails and communicate submission status without relying only on color or a spinner.

For a failed multi-field submission, an error summary can help users find problems while inline messages explain corrections. Client validation improves feedback, but the server still validates authoritative inputs and business rules. I test successful submission, invalid data, server rejection, and recovery with keyboard and assistive-technology considerations.

#### 7. What is the browser's critical rendering path?

Tags: #BrowserRendering #CompanyUnverified #AmazonRelevant

The browser must obtain and interpret resources needed to display content, calculate styles and layout, and paint the result. Blocking styles or scripts, large resources, and main-thread work can delay meaningful content or interaction. I inspect a performance trace and network waterfall to see which work actually blocks the experience.

I might reduce unnecessary JavaScript, defer nonessential work, or prioritize an important image after measurement. I avoid treating all resources as equally urgent. Optimizations should improve the target user's experience under representative network and device conditions, not just move work out of one benchmark's window.

#### 8. What is progressive enhancement?

Tags: #ProgressiveEnhancement #CompanyUnverified #AmazonRelevant

Progressive enhancement starts from a useful baseline and adds capabilities when the environment supports them. For suitable workflows, semantic navigation and forms can remain functional while JavaScript improves feedback and responsiveness. The baseline depends on the product; some advanced interactions legitimately need substantial client execution.

I identify what users should still be able to do if a script fails or a connection is slow. For example, an enhanced submission flow can preserve ordinary server validation and clear error responses. The objective is resilience and reach, not an inflexible ban on richer frontend behavior.

#### 9. How do browser storage mechanisms differ?

Tags: #BrowserStorage #CompanyUnverified #AmazonRelevant

Cookies can accompany matching HTTP requests under their configured rules. Web Storage provides synchronous string storage with different lifetime behavior for local and session storage. IndexedDB supports richer asynchronous local persistence. I choose based on data size, lifetime, access needs, and security boundaries.

None of these should be treated as a trusted authority for business permissions or payment state. JavaScript-readable storage can be exposed by script injection. I minimize sensitive local data and define logout, expiration, and offline cleanup behavior. Session credential choices require a threat model rather than a blanket rule to put every token in one storage mechanism.

#### 10. How should a frontend represent loading, empty, error, and success states?

Tags: #UIStates #CompanyUnverified #AmazonRelevant

I model states explicitly so users can distinguish work in progress, a valid empty result, a failed request, and completed content. I provide meaningful next actions and preserve useful context where appropriate. Initial loading and background refresh may need different behavior so existing content does not disappear unnecessarily.

For a search screen, “no matching records” is different from “the search service is unavailable.” I avoid showing an empty table as a generic failure fallback. State transitions should also handle cancellation, retries, stale results, and accessibility announcements without producing confusing flicker or duplicate actions.

### JavaScript, TypeScript, and asynchronous work (11–20)

#### 11. What are scope and closures, and where do they cause bugs?

Tags: #Closures #CompanyUnverified #AmazonRelevant

Scope determines where a binding is accessible. A closure lets a function retain access to its lexical environment after the outer execution has finished. This supports callbacks, encapsulation, and factory functions, but can also keep data alive or capture values from an earlier operation.

For example, a delayed callback may refer to a value from the render or request in which it was created. I inspect which binding is captured and how it changes rather than assuming callbacks always read the newest application state. Correct cleanup also matters when a long-lived listener retains unnecessary objects.

#### 12. How do browser tasks and microtasks affect promise callbacks, timers, and rendering?

Tags: #BrowserEventLoop #Amazon #CandidateReportedAdapted

**Attribution:** Adapted from the March 25, 2026 candidate report on [Glassdoor's Amazon front-end interview page](https://www.glassdoor.com/Interview/Amazon-Interview-Questions-E6036.htm?filter.jobTitleExact=Software+Engineer+-+Front+End). The report describes a front-end interview, not an independently verified FDE interview.

Synchronous JavaScript runs on the current execution stack. At a microtask checkpoint, queued promise reactions and other microtasks run until that queue is drained. Timer callbacks become eligible tasks after their delay and scheduling conditions; a zero delay does not mean immediate execution. Browsers get rendering opportunities according to the event loop rather than a guaranteed paint after every callback.

For a basic script, a resolved-promise callback runs before a subsequently processed timer callback. Excessive microtask chaining can delay rendering and input handling. I would split long work or move suitable computation to a worker instead of assuming `await` always gives the browser a paint opportunity. See [MDN's microtask guide](https://developer.mozilla.org/en-US/docs/Web/API/HTML_DOM_API/Microtask_guide).

#### 13. What is the difference between promises and async/await?

Tags: #AsyncAwait #CompanyUnverified #AmazonRelevant

A promise represents eventual completion or failure. An async function returns a promise, and `await` provides a convenient way to express dependent asynchronous steps. It does not make synchronous computation nonblocking or automatically run independent operations concurrently.

I use sequential awaits when one result depends on another. For independent work, I can initiate operations together and choose an appropriate aggregation strategy. I also handle rejected promises at a meaningful boundary. A `try` block around an unawaited asynchronous call does not automatically catch a rejection that occurs after control has left that block.

#### 14. When should you use Promise.all or Promise.allSettled?

Tags: #PromiseConcurrency #CompanyUnverified #AmazonRelevant

I use `Promise.all` when every result is needed for the combined operation and failure should reject the aggregate. I use `Promise.allSettled` when I need to inspect every outcome, such as independent validation or reporting tasks. I decide how partial success appears to the caller.

Neither construct inherently limits concurrency or cancels work already started. For hundreds of calls to a customer API, I use a bounded pool or batches that respect quotas. If early failure makes remaining work unnecessary, cancellation needs explicit support and cleanup rather than an assumption that aggregate rejection stopped everything.

#### 15. What is the difference between debounce and throttle?

Tags: #DebounceAndThrottle #CompanyUnverified #AmazonRelevant

Debouncing delays an operation until an input pattern has been quiet for a chosen period, depending on leading and trailing options. Throttling limits how often an operation runs over time while activity continues. I select the behavior based on the interaction rather than treating the functions as interchangeable optimizations.

Search suggestions often benefit from debouncing, while scroll-related updates may need throttling or frame-aligned scheduling. I define cancellation, cleanup, and final-call behavior. Debouncing requests does not prevent an older in-flight response from replacing a newer result, so response ordering still needs protection.

#### 16. How do you handle cancellation and stale asynchronous results?

Tags: #Cancellation #CompanyUnverified #AmazonRelevant

I cancel supported work when it is no longer needed and separately guard state updates using request identity or current inputs. Abort signals can stop supported client operations, but cancellation does not prove the server never received or completed a request. I treat interrupted mutations as potentially uncertain outcomes.

For autocomplete, a slow response for an earlier query must not replace a later query's results. I scope loading and error updates to the active request too. For a submitted payment or job, I query authoritative status or retry idempotently rather than assuming abort means nothing happened.

#### 17. What does TypeScript catch, and what does it not guarantee?

Tags: #TypeScriptBoundaries #CompanyUnverified #AmazonRelevant

TypeScript can catch many inconsistencies during development, such as incompatible arguments or unsafe use of optional values under suitable compiler settings. It does not prove external JSON matches a declared interface or that a business rule is correct. Type annotations do not substitute for runtime validation at trust boundaries.

I validate request bodies, environment configuration, and dependency responses before relying on their shape. I avoid using a type assertion to silence uncertainty about untrusted data. Shared types improve coordination, but backend authorization and database constraints still enforce the application rules that static checking cannot establish.

#### 18. How do unknown, any, and narrowing differ?

Tags: #TypeNarrowing #CompanyUnverified #AmazonRelevant

`any` permits unchecked operations that bypass much of the type system's protection. `unknown` allows a value to exist without claiming its usable shape; code must narrow or validate it before performing type-specific operations. Narrowing uses evidence such as checks or a discriminant to refine the understood type.

For a parsed response, I prefer an explicit validator over casting the value directly to an application model. I also ensure custom type guards actually verify the properties they claim. The [TypeScript narrowing handbook](https://www.typescriptlang.org/docs/handbook/2/narrowing.html) explains control-flow analysis and the checks the compiler understands.

#### 19. How can discriminated unions improve application state modeling?

Tags: #StateModeling #CompanyUnverified #AmazonRelevant

A discriminated union represents alternatives with a shared distinguishing field, such as `status: 'loading'`, `'success'`, or `'error'`. Each variant carries only the data appropriate to that state. This can eliminate combinations such as claiming success while also requiring an error message.

I use exhaustive handling where practical so adding a new state highlights missing cases. The same technique can model API results or job transitions. It still needs runtime validation for external input, and the model should allow real cases such as displaying previous data during a background refresh instead of oversimplifying the workflow.

#### 20. Why do object identity, mutation, and shallow copies matter?

Tags: #Immutability #CompanyUnverified #AmazonRelevant

Objects can be shared through references, so changing one can affect code that holds the same object. A shallow copy creates a new outer object while retaining references to nested values. I inspect ownership before mutating data and use explicit updates where callers expect immutable inputs.

For example, copying an array and then changing a nested item still mutates that item's shared object. This can confuse state comparison, caching, and debugging. Deep copying everything is not a universal solution because it has cost and may mishandle special values; I update only the necessary structure with clear ownership.

### React and frontend state (21–30)

#### 21. What is the difference between props and state?

Tags: #ReactDataFlow #CompanyUnverified #AmazonRelevant

Props are inputs passed to a component; state is information managed across its renders. Both should be treated as immutable within a render. I keep the minimal source of truth and derive values that can be computed cheaply instead of storing redundant copies.

If several components need coordinated selection, I place that state at the appropriate shared owner. Copying a prop into local state without a defined synchronization policy can create disagreement between parent and child. I first decide which component owns the value and which events are allowed to change it.

#### 22. Why does reading state immediately after setting it show an older value?

Tags: #StateSnapshots #CompanyUnverified #AmazonRelevant

A render and its event handlers observe a snapshot of state. Calling a setter requests another render; it does not rewrite the variable captured by the current handler. React may queue and batch updates according to its execution model.

When the next value depends on the previous one, I use an updater such as `setCount(current => current + 1)` rather than repeatedly calculating from the same captured count. Updaters should be pure. I verify the intended transition instead of relying on console output from the old closure. See [React's state-as-a-snapshot explanation](https://react.dev/learn/state-as-a-snapshot).

#### 23. When should you use an Effect?

Tags: #ReactEffects #CompanyUnverified #AmazonRelevant

An Effect synchronizes a component with an external system, such as a subscription, connection, or imperative browser API. I include the reactive dependencies it uses and return cleanup that reverses the setup. I do not use an Effect merely to derive a value that can be calculated during rendering.

For example, changing a selected room should disconnect the old subscription and connect the new one. Development checks can exercise an extra setup-and-cleanup cycle, so the code must tolerate it. Suppressing dependencies can conceal stale values rather than fix the design. See [React's useEffect reference](https://react.dev/reference/react/useEffect).

#### 24. How do you choose between local state, context, and an external store?

Tags: #StateOwnership #CompanyUnverified #AmazonRelevant

I keep state close to the components that use it unless sharing or lifecycle requirements justify moving it. Context can distribute a value through a subtree, while an external store may help with complex shared state, subscription control, or cross-cutting behavior. I separate remote server data from purely local interaction state.

For example, a modal's open state rarely needs to be global, while authenticated-user context may be shared. I consider debugging, update frequency, persistence, and team conventions. Introducing a store should solve an ownership problem rather than turn every temporary input into application-wide state.

#### 25. Why do list keys matter?

Tags: #ReactKeys #CompanyUnverified #AmazonRelevant

Keys help React identify items among siblings as a list changes. Stable identifiers preserve the intended component identity through insertions, removals, and reordering. An array index can associate local state with the wrong record when positions change, and a random key can force unnecessary remounting.

For editable rows, I use a stable record or locally assigned draft identifier. I test reordering while a row contains unsaved input. Keys help reconciliation; they do not automatically authorize access, provide a business identifier to the child, or make expensive row rendering disappear.

#### 26. What are controlled and uncontrolled form inputs?

Tags: #ReactForms #CompanyUnverified #AmazonRelevant

A controlled input has its displayed value driven by application state and updated through events. An uncontrolled input keeps its current value in the DOM, which code reads through a suitable form or reference mechanism. I choose based on validation, dynamic behavior, performance, and the surrounding form approach.

Controlled inputs can make interdependent fields straightforward, but I avoid unnecessary rerendering of a large form. In either model, I handle defaults, resets, accessibility, and server validation. I also avoid accidentally switching an input between controlled and uncontrolled behavior during its lifetime.

#### 27. How do you fetch and cache server data in a React application?

Tags: #ServerState #CompanyUnverified #AmazonRelevant

I define request keys from the relevant user, tenant, filters, and pagination inputs. I handle loading, errors, cancellation, freshness, and invalidation after mutations. A framework data layer or query library can manage these concerns, but its defaults still need to match the workflow.

For example, changing tenants must not reuse a previous tenant's result under the same key. I distinguish stale-but-displayable data from unauthorized data that must be removed. If fetching manually, I explicitly manage race conditions and cleanup rather than assuming mounting an Effect establishes a complete caching strategy.

#### 28. How do you optimize a React interface without premature memoization?

Tags: #ReactPerformance #CompanyUnverified #AmazonRelevant

I profile the actual interaction and identify expensive rendering, computation, or browser layout work. I first consider moving state closer to its owner, reducing unnecessary work, and rendering fewer items. Memoization can help specific expensive calculations or identity-sensitive boundaries when its inputs and cost justify it.

I avoid assuming every function needs a cached identity or every component needs memoization. Unstable dependencies can defeat it, and stale dependencies can create bugs. I verify improvements with representative data and devices, including interaction responsiveness, instead of using a lower render count as the only success criterion.

#### 29. How do you design optimistic UI updates?

Tags: #OptimisticUI #CompanyUnverified #AmazonRelevant

I show a predicted outcome before the server confirms it only when the action's semantics make that experience appropriate. I track the pending mutation, reconcile the authoritative result, and provide a clear correction or rollback on failure. Concurrent edits and out-of-order responses need explicit handling.

For example, optimistically toggling a preference is different from claiming a payment succeeded. I avoid a rollback that overwrites newer user changes after an older request fails. Idempotency protects retried server effects, while mutation identity and version checks help keep the interface aligned with the final state.

#### 30. What causes hydration mismatches, and how do you address them?

Tags: #Hydration #CompanyUnverified #AmazonRelevant

Hydration connects client behavior to server-rendered markup. A mismatch can occur when the initial client output differs from the server output because of time, random values, browser-only state, invalid markup, or inconsistent data. I inspect the first render on both sides and make shared inputs deterministic.

Browser-dependent behavior may need a deliberate post-mount transition or an appropriately isolated client component. I do not hide all warnings without understanding the cause. I also check that personalized server output is cached with correct boundaries, because a rendering problem can coexist with a more serious data-isolation error.

### Backend services and runtime behavior (31–40)

#### 31. Explain the Node.js event loop.

Tags: #NodeEventLoop #Ubiquiti #CandidateReported

**Attribution:** The [Glassdoor TypeScript full-stack question listing](https://www.glassdoor.com/Interview/typescript-full-stack-developer-interview-questions-SRCH_KO0%2C31.htm) attributes this prompt to a Ubiquiti Full-stack TypeScript Developer interview, dated December 12, 2023. This is a candidate report, not independent verification.

Node.js coordinates asynchronous work through an event loop with phases for different callback categories. JavaScript callbacks in one ordinary event-loop thread execute sequentially, while the runtime can rely on operating-system facilities, a worker pool, or explicitly created workers for other work. Asynchronous I/O does not imply every operation executes on the same single thread.

Long synchronous computation blocks progress for unrelated callbacks on that loop. I keep handlers bounded and move suitable CPU-heavy work to workers or background processing. I avoid memorized claims about all timer ordering without context, because phase behavior and module execution details matter. See [Node.js's event-loop documentation](https://nodejs.org/en/learn/asynchronous-work/event-loop-timers-and-nexttick).

#### 32. How do you separate controllers, business logic, and data access?

Tags: #BackendLayers #CompanyUnverified #AmazonRelevant

I keep transport handling focused on parsing, authentication context, validation, and response mapping. Business logic expresses use cases and invariants, while data-access code handles persistence details. The boundaries should make important behavior testable without requiring every test to start an HTTP server.

For example, an approval rule should not be duplicated across several route handlers. I also define where authorization and transaction boundaries belong so a helper cannot accidentally bypass them. A small application may use fewer files, but clear responsibilities matter more than reproducing a large framework's folder structure.

#### 33. What is middleware, and why does ordering matter?

Tags: #Middleware #CompanyUnverified #AmazonRelevant

Middleware participates in a request pipeline, often adding context, checking access, transforming data, or handling failures. Ordering determines which assumptions are valid at each stage. A route that expects authenticated context must not execute before authentication, and resource-intensive parsing should respect size and content constraints.

I make error propagation and asynchronous failures consistent with the framework's behavior. I also distinguish global policies from resource-specific authorization that needs loaded data. Logging should avoid secrets, and request identifiers should exist early enough to connect failures across the remaining pipeline.

#### 34. How should a backend validate incoming data?

Tags: #InputValidation #CompanyUnverified #AmazonRelevant

I validate structure, types, bounds, allowed values, and relevant cross-field rules at the trust boundary. I then apply business rules using authoritative state, such as whether the user may edit the target record. I reject or explicitly handle unknown fields and do not pass client input directly into persistence updates.

For example, a client must not assign itself an administrative role through an extra JSON property. Validation errors should identify correctable problems without exposing internal implementation details. Shared schemas can help client feedback, but server enforcement remains necessary because callers can bypass the interface.

#### 35. How do you design consistent backend error handling?

Tags: #ErrorHandling #CompanyUnverified #AmazonRelevant

I classify expected validation, authorization, conflict, dependency, and unexpected internal failures. I map them to stable response semantics with a useful error code, safe message, and correlation identifier where appropriate. I preserve detailed diagnostics in approved server-side records rather than leaking stack traces to clients.

I also explain retry behavior and uncertainty. A dependency timeout during a mutation may require status reconciliation instead of telling the user to repeat the action blindly. Central handling improves consistency, but domain errors still need meaningful context so every failure does not collapse into an indistinguishable generic message.
