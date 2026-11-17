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

#### 36. How do you prevent CPU-heavy work from blocking a backend?

Tags: #BackendConcurrency #CompanyUnverified #AmazonRelevant

I measure whether the work is CPU-bound, I/O-bound, or waiting on a shared resource. CPU-heavy parsing, transformation, or computation may need worker threads, separate processes, or a background job service. I bound concurrency and memory rather than launching unlimited work outside the request handler.

An async function still blocks while running synchronous computation. For a large export, I might accept a job and return a status identifier instead of holding a request open. I include admission limits, progress, cancellation semantics, and operational ownership so moving work into the background does not merely hide overload.

#### 37. Why use streams and backpressure for large data transfers?

Tags: #Streaming #CompanyUnverified #AmazonRelevant

Streaming processes data incrementally instead of buffering the entire input or output. Backpressure lets a slower destination influence the producer so memory use remains bounded. I use interfaces that propagate errors and cleanup, and I define limits on size, duration, and simultaneous transfers.

For example, a large CSV export should not require materializing every row in application memory. I still consider database cursor behavior, slow clients, and connection lifetime. Once response bytes are sent, a later failure cannot always become a normal JSON error, so partial-output and retry behavior need a clear contract.

#### 38. How do database connection pools affect scalability?

Tags: #ConnectionPooling #CompanyUnverified #AmazonRelevant

A connection pool reuses a bounded number of database connections and controls concurrent access. I set pool size and acquisition timeouts based on workload and database capacity. I release connections reliably and keep transactions short so one request does not monopolize a scarce resource.

Adding application replicas multiplies potential connections, so autoscaling can overwhelm the database if each replica has a large pool. I estimate total concurrency and monitor wait time, active connections, and query duration. Pooling helps manage access; it does not make expensive queries or an overloaded database inherently faster.

#### 39. What does graceful shutdown require?

Tags: #GracefulShutdown #CompanyUnverified #AmazonRelevant

I stop accepting new work, signal unready status as appropriate, and allow in-flight operations to finish within a bounded drain period. I close connections and subscriptions and coordinate background jobs so unfinished work can be resumed or redelivered safely. The process must also tolerate forced termination.

For a worker, I distinguish completing the effect from acknowledging the message. If termination occurs between them, redelivery needs idempotency. I test the deployment platform's actual termination sequence because a shutdown handler alone is insufficient if traffic continues arriving or the platform's deadline is shorter than the assumed drain window.

#### 40. How do you separate configuration from code?

Tags: #Configuration #CompanyUnverified #AmazonRelevant

I make environment-specific values explicit, validate them at startup, and avoid scattered fallback assumptions. I distinguish ordinary configuration from secrets, which use an approved protected mechanism. I record enough versioned deployment context to reproduce behavior without exposing sensitive values.

Frontend configuration is usually observable by users once delivered, so an API secret does not become private because it originated in an environment variable. I also clarify whether a value is substituted at build time or read at runtime. That distinction affects promotion of one artifact across environments and emergency configuration changes.

### APIs and client-server integration (41–50)

#### 41. What makes a good HTTP API contract?

Tags: #APIContracts #CompanyUnverified #AmazonRelevant

I define operations, schemas, validation, authentication, authorization, status behavior, and compatibility expectations. I include pagination, rate limits, concurrency control, and idempotency where the workflow needs them. Clients should be able to determine what happened and what to do next without interpreting an undocumented string.

For example, a long-running import can return a job identifier and expose explicit terminal states. I describe whether partial success is possible and how rejected rows are retrieved. A contract covers failure and uncertainty as well as the successful response used in the first demo.

#### 42. How do you handle fetch responses correctly?

Tags: #FetchAPI #CompanyUnverified #AmazonRelevant

I distinguish network failure, HTTP failure, response-body parsing, and invalid application data. A fulfilled fetch promise can contain a `404` or `500` response, so I inspect status or `response.ok` before treating it as success. I parse the body according to the contract and validate its shape where needed.

I also use appropriate cancellation and avoid expecting a JSON body from a successful `204` response. Errors should retain useful context without leaking secrets. See [MDN's Fetch guidance](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch) for response handling, credentials, and cancellation behavior.

#### 43. What is CORS, and why can a call work outside the browser but fail inside it?

Tags: #CORS #CompanyUnverified #AmazonRelevant

CORS is a browser mechanism that allows a server to specify which cross-origin requests may expose their responses to script. Some requests require an OPTIONS preflight. A command-line client is not subject to the browser's same-origin response-access enforcement, which explains many apparent differences.

I inspect the actual origin, requested headers and methods, credentials, and preflight response. Credentialed access requires an appropriate explicit allowed origin rather than a wildcard response origin. CORS is not authentication, and some cross-origin requests can still reach the server without readable responses. See [MDN's CORS guide](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS).

#### 44. How do you implement pagination, filtering, and sorting consistently?

Tags: #CollectionAPIs #CompanyUnverified #AmazonRelevant

I define allowed filters and sorts, validate values, and apply them before pagination in the authoritative query. The ordering needs a stable tie-breaker, and cursor or offset semantics should fit expected mutations and data size. I bound page size and ensure the query matches available indexes.

The interface can encode these inputs in the URL for navigation and sharing. Changing a filter may require resetting the current page or cursor. I do not sort only the currently loaded page while presenting it as a globally sorted result set, and I enforce authorization before returning records or counts.

#### 45. What makes a mutation safe to retry?

Tags: #Idempotency #CompanyUnverified #AmazonRelevant

I identify whether repeating the request could produce another effect. For operations such as creating an order, I can accept a scoped idempotency key and persist a request fingerprint and result atomically with the authoritative change. Matching retries return the recorded outcome, while conflicting payloads are rejected.

I define concurrent-request behavior and retention. External effects may need downstream idempotency or reconciliation because a local database transaction does not cover them automatically. A disabled submit button improves interaction but cannot protect against network retries, multiple tabs, or a client that directly repeats the API call.

#### 46. How do you design a long-running asynchronous operation?

Tags: #AsyncJobs #CompanyUnverified #AmazonRelevant

I durably accept the work, return a job identifier, and expose status with clear progress and terminal states. Workers process bounded tasks, record outcomes, and handle retries safely. The client can poll or subscribe to updates depending on freshness requirements and infrastructure constraints.

I include ownership checks for status and result downloads, expiration, partial failures, and cancellation semantics. “Cancel requested” may differ from “all effects stopped.” If acceptance and enqueueing are separate writes, I use a reliable handoff such as a transactional outbox or an equivalent mechanism so accepted jobs cannot silently disappear.

#### 47. How do you integrate with an unreliable third-party API?

Tags: #ExternalIntegrations #CompanyUnverified #AmazonRelevant

I define timeouts, bounded retries for suitable failures, rate-limit handling, and an explicit unavailable or degraded state. I validate responses, isolate dependency-specific logic behind an adapter, and keep diagnostics that identify the failing operation. I use backoff and jitter rather than synchronized immediate retries.

For writes, a timeout can mean the remote change succeeded but the response was lost. I use provider-supported idempotency or reconcile through a stable external identifier. If the API lacks necessary guarantees, I document the remaining uncertainty and provide an operator workflow instead of claiming perfect automatic recovery.

#### 48. How do you process webhooks reliably?

Tags: #Webhooks #CompanyUnverified #AmazonRelevant

I verify the provider's authenticity mechanism using the required raw body and metadata, validate the event, and durably accept it before acknowledging. I deduplicate by a suitable event identity and process expensive work asynchronously when appropriate. I enforce payload limits and avoid logging secrets or unnecessary sensitive content.

I assume events may repeat, arrive late, or arrive out of order unless the provider guarantees otherwise. State transitions use versions or authoritative lookups where needed. A reconciliation process covers missed events, and replay remains safe. A public endpoint URL alone does not establish that the sender is trusted.

#### 49. How do you keep frontend and backend contracts aligned?

Tags: #ContractEvolution #CompanyUnverified #AmazonRelevant

I maintain a shared contract or generated client when it reduces duplication and validate compatibility through appropriate checks. I distinguish public transport schemas from internal persistence models so a database change does not unintentionally expose fields. Both sides still validate their trust boundaries.

I prefer compatible staged changes because frontend and backend versions may coexist during deployment or browser caching. For example, making a new field mandatory immediately can break older clients. I measure usage before retiring old behavior and define a migration path rather than relying on a coordinated release occurring perfectly everywhere.

#### 50. How do you choose polling, server-sent events, or WebSockets?

Tags: #RealtimeUpdates #CompanyUnverified #AmazonRelevant

I choose based on required freshness, direction of communication, update frequency, and operating constraints. Polling is straightforward for occasional updates. Server-sent events fit server-to-client streaming, while WebSockets support sustained bidirectional interactions. I verify proxy support, connection limits, and authentication behavior in the actual environment.

All approaches need reconnect and missed-update semantics where data matters. A resume cursor or subsequent authoritative fetch can restore state after disconnection. I distinguish ephemeral signals from durable records and avoid treating an open socket as proof of message delivery or current authorization.

### Databases and data correctness (51–60)

#### 51. How do you choose a relational database versus a document or key-value store?

Tags: #DatabaseChoice #CompanyUnverified #AmazonRelevant

I compare access patterns, relationships, invariants, query flexibility, growth, and operating constraints. A relational database often fits transactions and related entities well. Document or key-value models may fit particular access patterns, but their exact transaction, indexing, and consistency capabilities depend on the chosen system.

I avoid treating “SQL versus NoSQL” as a universal scale comparison. A customer workflow with modest volume and complex updates may benefit most from a familiar relational design. I include backups, migration, tooling, and the receiving team's skills in the decision, not only peak throughput claims.

#### 52. How do you model a business workflow in a database?

Tags: #DataModeling #CompanyUnverified #AmazonRelevant

I identify entities, stable identifiers, relationships, lifecycle states, and invariants before designing tables or documents. I distinguish authoritative facts from derived summaries and define ownership, timestamps, and deletion behavior. I test the model against realistic transitions and failure cases.

For an approval process, I might separate the request, approval attempts, and audit events rather than repeatedly overwriting one text field. The model should explain who changed what and which transitions are valid. I also consider tenant-scoped keys and constraints so correctness does not depend on every application path remembering the same informal rule.

#### 53. When do you normalize or denormalize data?

Tags: #Normalization #CompanyUnverified #AmazonRelevant

Normalization reduces duplicated facts and can simplify consistent updates. Denormalization stores repeated or precomputed data to support a particular read pattern. I start from the domain and actual queries, then make duplication deliberate with a defined update and repair policy.

For example, an order may preserve the shipping address used at purchase even after the customer's profile changes. That is a historical business fact, not simply an accidental duplicate. A cached customer display name has different freshness requirements. I distinguish these meanings before deciding which source should update which copy.

#### 54. How do you decide which indexes to create?

Tags: #DatabaseIndexes #CompanyUnverified #AmazonRelevant

I inspect important filters, joins, ordering, cardinality, and query plans using representative data. I choose indexes that support those patterns and evaluate their effect on read latency, write cost, and storage. Composite key order matters, and a large number of indexes can make writes and maintenance expensive.

For a tenant-scoped recent-record list, an index aligned with tenant and ordering may help. I verify the actual plan and avoid assuming the database will always select an available index. I also check whether unnecessary columns, broad scans, or poor pagination are the real problem.

#### 55. What is an N+1 query problem?

Tags: #QueryEfficiency #CompanyUnverified #AmazonRelevant

An N+1 pattern performs one query for a collection and then an additional query for each item or related entity. It can create excessive round trips and load even when individual queries are fast. I detect it through traces, query logs, or request-level query counts.

I may use a join, explicit eager loading, or batching depending on the relationship and response shape. Joining every relationship can create a huge multiplied result, so I measure both round trips and returned data. I also ensure batching preserves tenant and resource authorization rather than fetching broadly and hoping the interface filters correctly.

#### 56. What do transactions guarantee, and what still needs design?

Tags: #Transactions #CompanyUnverified #AmazonRelevant

A transaction groups database operations according to the database's atomicity, durability, and isolation guarantees. I choose a boundary around the business invariant and verify the isolation behavior required for concurrent operations. Merely placing two statements in a transaction does not make every read-modify-write sequence safe.

I keep transactions appropriately short and handle conflicts or serialization failures. External effects such as an email are not automatically rolled back with database state, so they need a reliable handoff or reconciliation. See [PostgreSQL's transaction-isolation documentation](https://www.postgresql.org/docs/current/transaction-iso.html) for the distinction between supported isolation levels and concurrent anomalies.

#### 57. How do you prevent lost updates from two editors?

Tags: #OptimisticConcurrency #CompanyUnverified #AmazonRelevant

I can include a version in the record and require that an update match the version the editor originally read. The authoritative update checks and increments the version atomically. If no record matches, the API reports a conflict instead of silently overwriting someone else's work.

The interface can offer a refresh, comparison, or domain-appropriate merge. Blindly retrying with the new version may defeat the protection by applying an old edit to new state. Some workflows need locking or more specific merge rules, so I choose the policy based on what the data means.

#### 58. How do you migrate a schema while old and new application versions coexist?

Tags: #DatabaseMigrations #CompanyUnverified #AmazonRelevant

I use compatible stages: add the new representation, deploy code that can tolerate the transition, backfill carefully, switch authoritative usage, and remove obsolete fields after validation. I assess locking, data volume, transaction size, and the deployment platform's rollout behavior.

Renaming or dropping a required column immediately can break still-running instances or background jobs. I verify both versions during the transition and reconcile migrated values. Recovery planning includes data effects because reverting the application does not automatically reverse a completed transformation or restore deleted information.

#### 59. When should a full-stack application cache database results?

Tags: #ApplicationCaching #CompanyUnverified #AmazonRelevant

I cache when repeated expensive reads have sufficient reuse and the workflow permits a clear freshness policy. I define the key, tenant and user context, lifetime, invalidation, capacity, and miss behavior. I first verify whether query optimization or reducing unnecessary requests would solve the problem more simply.

A cache must not become an accidental authority for a critical write decision. I plan for cold starts, simultaneous refreshes, and complete cache loss so the backend remains protected. If permissions change, stale cached data needs an appropriate enforcement strategy rather than a promise that expiration will eventually fix access.

#### 60. How do you validate data imported from customer spreadsheets or files?

Tags: #DataImports #CompanyUnverified #AmazonRelevant

I define the expected schema and business meaning, then validate formats, required values, identifiers, duplicates, and references. I preserve provenance and produce actionable row-level errors using approved data-handling practices. I distinguish a completely rejected file from a partially accepted import with explicit semantics.

I provide a preview or dry run where helpful and use stable identities or idempotency to avoid duplicate effects on retries. Silent correction can change business meaning, so normalization rules need agreement. Reconciliation checks counts and important values after import rather than treating successful parsing as proof of correct data.

### Authentication and application security (61–70)

#### 61. What is the difference between authentication and authorization?

Tags: #AccessControl #CompanyUnverified #AmazonRelevant

Authentication identifies the caller. Authorization checks whether that caller may perform a specific action on a specific resource in the current context. I enforce authorization on the server for every relevant path, including exports, downloads, background jobs, and administrative operations.

For example, knowing a record identifier or hiding a button does not determine permission to edit the record. I check ownership, tenant, role, and other applicable policy using trusted context. I test denial paths as deliberately as success paths so a valid session cannot access another customer's information by modifying a request.

#### 62. How do server-side sessions and JWT-based access tokens differ?

Tags: #SessionsAndTokens #CompanyUnverified #AmazonRelevant

A server-side session commonly uses an opaque identifier whose associated state is stored on the server. A signed JWT can carry claims that a receiver verifies under a defined token-validation policy. These are representation and validation choices; either may be delivered through a cookie or another suitable mechanism.

I compare revocation, expiry, audience, storage exposure, and operational complexity. Signed does not mean encrypted, and a token's claims may become stale as permissions change. JWTs do not automatically eliminate server state or make an application more secure. I choose a complete session lifecycle rather than choosing only a format.

#### 63. What does secure session management require?

Tags: #SessionSecurity #CompanyUnverified #AmazonRelevant

I use unpredictable session identifiers, protected transport, appropriate cookie attributes where cookies are used, and clear idle and absolute expiration policies. I rotate session identifiers when appropriate, support server-side invalidation, and avoid exposing credentials in URLs or logs. I consider account changes and concurrent sessions.

`HttpOnly` limits script access to a cookie, while `Secure` restricts its transmission to secure contexts under browser rules; neither is a complete defense against malicious script actions or CSRF. I validate the whole lifecycle, including login, logout, renewal, and revocation. See the [OWASP session-management guidance](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html).

#### 64. What is CSRF, and how do you defend against it?

Tags: #CSRF #CompanyUnverified #AmazonRelevant

Cross-site request forgery abuses credentials that a browser attaches automatically to induce an unwanted state-changing request. For cookie-authenticated operations, I use appropriate CSRF tokens or a suitable equivalent pattern, origin checks where applicable, and carefully configured cookie policy. I do not make state changes through ordinary safe navigation endpoints.

SameSite cookies help in many situations but need to match the application's cross-site flows and threat model. CORS alone is not a general CSRF defense because some requests can be sent even when their responses are unreadable. Server-side authorization and input validation remain necessary alongside CSRF protection. See [OWASP's CSRF prevention guidance](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html).

#### 65. What is XSS, and how do you prevent it?

Tags: #XSS #CompanyUnverified #AmazonRelevant

Cross-site scripting occurs when untrusted content executes as script in a trusted page context. I use context-appropriate output handling, avoid unsafe DOM or template insertion, and sanitize approved rich HTML with a maintained approach when HTML must be supported. A restrictive content policy can add defense in depth.

Framework escaping helps common text rendering but does not make arbitrary HTML, URLs, or third-party scripts safe. I inspect every trust boundary and avoid trusting stored content merely because it came from our database. I also limit the credentials and sensitive data available to compromised client code.

#### 66. How do you prevent SQL injection and unsafe mass assignment?

Tags: #InjectionPrevention #CompanyUnverified #AmazonRelevant

I use parameterized database operations for values and explicitly allowlist dynamic structures such as sort fields or identifiers. I do not concatenate untrusted input into query text. For updates, I map approved input fields to the operation rather than passing the entire request body into an object or persistence API.

These address different problems: parameterization protects query interpretation, while field allowlisting prevents users from setting properties such as owner or privilege level. An ORM does not guarantee every custom query or update is safe. I also apply authorization and database constraints so valid syntax cannot perform an unauthorized change.

#### 67. How do you implement tenant isolation across the stack?

Tags: #TenantIsolation #CompanyUnverified #AmazonRelevant

I derive tenant context from validated identity and authorized membership, then propagate it through queries, caches, files, jobs, and integration calls. Data-access boundaries enforce tenant scoping consistently, with additional database protections where appropriate. I do not trust an arbitrary client-supplied tenant ID as authority.

I test cross-tenant reads, writes, exports, searches, and background results, including guessed identifiers and cache-key collisions. Operational tooling needs equivalent care because administrators can bypass normal application paths. Isolation is an end-to-end property; a filtered table in the browser does not establish it.

#### 68. How do you handle secrets and credentials in a full-stack project?

Tags: #SecretsManagement #CompanyUnverified #AmazonRelevant

I keep service credentials in an approved server-side secret mechanism, scope access, and plan rotation and revocation. I avoid committing them, embedding them in frontend bundles, or exposing them in logs and error responses. Public configuration and secret configuration have different handling requirements.

If a browser needs access to an external operation, I use an authorized backend path or a narrowly scoped, short-lived capability when the service supports it. I include expiration and permitted actions in that design. Moving a secret into a frontend environment variable does not keep it hidden from the application user.

#### 69. What makes a file-upload feature secure?

Tags: #UploadSecurity #CompanyUnverified #AmazonRelevant

I authorize the upload, bound size and resource usage, and validate supported content independently of the filename or client-declared type. I store objects with controlled names and access and isolate untrusted parsing or scanning. Files remain unavailable to ordinary consumers until required checks complete.

I also secure result downloads and clean up abandoned uploads. A signed upload URL needs limited scope and lifetime, and accepting an object does not prove its contents are safe. I consider archive expansion, parser failures, and indirect processing costs so a small request cannot trigger unlimited computation or storage.

#### 70. How do you handle sensitive customer data during development and debugging?

Tags: #DataHandling #CompanyUnverified #AmazonRelevant

I use approved environments and minimize copying, collection, and retention. Synthetic or appropriately de-identified examples can support development when representative enough. I confirm permitted use with responsible owners and protect logs, screenshots, traces, test fixtures, and third-party diagnostic tools from unnecessary exposure.

When debugging requires real examples, I use the narrowest approved access and record only what is needed. I include cleanup and access revocation in the workflow. Technical ability to retrieve a record does not establish permission to move it into another environment or send it to an external service.

### Testing, debugging, and performance (71–80)

#### 71. How do you choose unit, integration, and end-to-end tests?

Tags: #TestingStrategy #CompanyUnverified #AmazonRelevant

I choose the lowest-cost layer that can credibly verify the important behavior. Unit tests suit isolated rules and transformations. Integration tests verify boundaries such as database queries, authorization, or dependency adapters. A smaller set of end-to-end tests exercises critical user journeys through the assembled system.

I prioritize impact and failure likelihood rather than pursuing a fixed test count. For an approval flow, useful coverage includes allowed and denied transitions, concurrent updates, and one browser journey. Testing a mock's behavior repeatedly is weak evidence if the production boundary it replaces remains unverified.

#### 72. What makes a useful frontend component test?

Tags: #ComponentTesting #CompanyUnverified #AmazonRelevant

I render the component in a representative context, interact through user-observable controls, and assert meaningful behavior. I prefer queries based on roles, labels, or visible text when they fit the interface. I cover important states such as pending submission, validation feedback, and recovery from an API error.

I avoid assertions tied only to private state or incidental implementation details. A test that survives an internal refactor while detecting a broken user action provides better evidence. The [Testing Library guiding principles](https://testing-library.com/docs/guiding-principles/) explain this emphasis on testing components in ways that resemble their use.

#### 73. How do you make browser tests reliable?

Tags: #BrowserTesting #CompanyUnverified #AmazonRelevant

I isolate test data and browser state, control dependencies as appropriate, and wait for observable conditions instead of fixed sleeps. I use stable, meaningful locators and assertions that account for asynchronous behavior. Failure artifacts such as traces and screenshots help distinguish application defects from setup problems.

I keep a few critical journeys using real integrations where practical while using controlled responses for targeted failure cases. Retries can expose intermittent behavior but should not conceal a recurring defect. See [Playwright's best practices](https://playwright.dev/docs/best-practices) for isolation, resilient locators, and user-visible verification.

#### 74. When should you mock a dependency?

Tags: #TestDoubles #CompanyUnverified #AmazonRelevant

I mock a dependency when controlling it makes a focused test faster, deterministic, or able to exercise an otherwise difficult failure. I preserve the relevant contract and separately verify important real boundaries. A mock should not be the only evidence that authentication, persistence, or a customer integration works.

For example, a component test can simulate a timeout while an integration test verifies the real client's response handling. I avoid duplicating the implementation inside the mock, which can make both share the same mistake. I also review fixtures when contracts change so stale examples do not create false confidence.

#### 75. How would you debug a bug that crosses frontend and backend boundaries?

Tags: #EndToEndDebugging #CompanyUnverified #AmazonRelevant

I reproduce a specific user action and collect the request inputs, browser behavior, response, and correlation identifier. I trace the operation through API logs, business logic, database activity, and dependencies. I compare expected and actual behavior at each boundary to narrow the first point of divergence.

For example, a blank table might result from an incorrect filter, denied access, an unexpected response schema, or a rendering error. I avoid changing several layers at once. After fixing the cause, I add or run a focused check at the boundary that would have exposed it.

#### 76. How do you measure frontend performance?

Tags: #WebPerformance #CompanyUnverified #AmazonRelevant

I measure real user tasks and inspect network, main-thread, rendering, and interaction behavior under representative conditions. Current Core Web Vitals include Largest Contentful Paint for loading, Interaction to Next Paint for responsiveness, and Cumulative Layout Shift for visual stability. These complement workflow-specific measures rather than replacing them.

I distinguish controlled laboratory measurements from field data across actual devices and networks. A fast initial page can still have a slow search or save action. I identify the bottleneck before optimizing and verify improvement in the affected experience. See [Google's Web Vitals overview](https://web.dev/articles/vitals).

#### 77. How do you optimize a table with thousands of rows?

Tags: #LargeLists #CompanyUnverified #AmazonRelevant

I first ask whether users need all rows at once and whether server-side filtering or pagination better fits the task. Virtualization can reduce rendered elements for large interactive lists, but it introduces focus, measurement, accessibility, and scrolling considerations. I avoid fetching unnecessary fields or repeating expensive cell calculations.

I profile rendering separately from data loading and query time. A virtualized table does not fix a slow unindexed query or an oversized download. I verify sorting and selection across pages or windows, stable row identities, and the behavior of screen readers and keyboard navigation for the chosen component.

#### 78. How do you diagnose a backend performance bottleneck?

Tags: #BackendPerformance #CompanyUnverified #AmazonRelevant

I define the workload and expected service behavior, then inspect request traces, latency distributions, errors, CPU, memory, connection waits, and dependency timings. I examine query plans and request amplification where database work dominates. I compare affected and unaffected requests and recent changes.

If a shared database is saturated, adding application replicas may increase pressure. If a handler blocks the event loop, a query index will not address it. I select an intervention based on evidence, then retest comparable load and failure conditions while watching correctness, tail latency, and resource consumption.

#### 79. How do you use HTTP caching safely?

Tags: #HTTPCaching #CompanyUnverified #AmazonRelevant

I classify responses by freshness and sharing requirements, then set an explicit cache policy. Versioned public assets can often use long-lived caching, while personalized responses need carefully controlled handling. Validators such as ETags can support revalidation when appropriate.

I distinguish `no-cache`, which permits storage with required revalidation, from `no-store`, which instructs caches not to store the response. I verify variation and shared-cache boundaries so one user's content cannot be served to another. Cache behavior also affects rollout and revocation. See [MDN's HTTP caching guide](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Caching).

#### 80. What causes memory leaks in full-stack applications?

Tags: #MemoryDiagnostics #CompanyUnverified #AmazonRelevant

Long-lived references can retain objects that are no longer needed. Common causes include unremoved listeners, timers, subscriptions, unbounded caches, pending work, or retained request context. I reproduce sustained activity and inspect memory trends and appropriate heap diagnostics rather than labeling every temporary allocation a leak.

In the browser, an unmounted view may remain referenced by a listener; on the server, an ever-growing map may retain every user or job. I define lifecycle cleanup and size bounds, then verify behavior over repeated operations. Garbage collection cannot free reachable objects merely because the application no longer intends to use them.

### Delivery, deployment, and operations (81–90)

#### 81. How do you structure a full-stack repository?

Tags: #RepositoryStructure #CompanyUnverified #AmazonRelevant

I organize code around clear application and domain boundaries, with explicit ownership of client, server, contracts, and shared utilities. A monorepo can simplify coordinated changes, while separate repositories may fit independent teams and release cycles. I choose based on workflow rather than assuming either structure guarantees modularity.

I keep browser-safe code separate from server-only dependencies and credentials. Shared packages should have intentional interfaces rather than exposing all internal models. I also provide straightforward setup, validation, and build commands so another engineer can reproduce a working slice without undocumented local steps.

#### 82. What should a full-stack CI pipeline verify?

Tags: #ContinuousIntegration #CompanyUnverified #AmazonRelevant

I run appropriate static checks, type checking, tests for important behavior, and production builds. I include contract or migration checks where the change can break a boundary. The pipeline should provide actionable failures and use reproducible dependencies and configuration.

I distinguish fast checks for every change from expensive validation triggered by risk or release scope. A passing frontend build does not prove the backend contract remains compatible. I protect secrets and test data in CI and ensure generated artifacts correspond to the version actually reviewed and promoted.

#### 83. What does a container provide, and what does it not solve?

Tags: #Containers #CompanyUnverified #AmazonRelevant

A container packages an application and its user-space dependencies into a reproducible execution unit with configured isolation and resource behavior. It can reduce environment drift, but it does not automatically provide secure configuration, persistent data, high availability, or correct networking.

I define the startup command, runtime user, resource limits, configuration, health behavior, and shutdown sequence. I keep secrets out of image layers and plan image updates. A container that starts locally can still fail in production because identity, network access, permissions, or storage assumptions differ in the customer's environment.

#### 84. How do readiness, liveness, and startup checks differ?

Tags: #HealthChecks #CompanyUnverified #AmazonRelevant

A readiness check indicates whether an instance should receive work. A liveness check helps identify a process that should be restarted. A startup check can give initialization its own allowance before other checks apply, depending on the platform. I choose behavior based on failure modes rather than reusing one expensive endpoint for every purpose.

A temporary dependency outage should not necessarily trigger a fleet-wide restart loop. Conversely, returning healthy because the process exists may route traffic to an unusable instance. I test startup, draining, and dependency failures and document what each signal means to the deployment system.

#### 85. How do you deploy frontend and backend changes without downtime?

Tags: #CompatibleDeployment #CompanyUnverified #AmazonRelevant

I make the contract and schema compatible across the versions that can coexist. I deploy additive server behavior before depending on it from the client, then remove obsolete paths only after usage evidence supports it. I use staged rollout and clear health or business stop conditions.

Old browser tabs, cached assets, and background workers may outlive a server release. I test those transitions and maintain a recovery plan for application and data changes. “Deploy both together” is not a complete compatibility strategy when distributed clients cannot all update at the same instant.

#### 86. What should you observe after launch?

Tags: #ProductionObservability #CompanyUnverified #AmazonRelevant

I monitor successful user tasks, errors, latency, request volume, resource saturation, and dependency health. For asynchronous workflows, I include job age, failures, and completion outcomes. Correlation identifiers and appropriate logs or traces connect a user's report to the relevant operations.

Alerts need an owner and a useful response procedure. I avoid collecting excessive sensitive content or unbounded high-cardinality labels. A low API error rate can coexist with incorrect data or abandoned workflows, so operational health includes business-level checks rather than only process uptime and successful HTTP responses.

#### 87. How do feature flags help, and what risks do they introduce?

Tags: #FeatureFlags #CompanyUnverified #AmazonRelevant

Flags can separate code deployment from activation and support gradual rollout or a bounded pilot. I define ownership, targeting, defaults, and how behavior changes when the flag service is unavailable. I ensure the activation boundary includes any necessary backend enforcement.

Flags add configuration states and can become permanent complexity if never removed. I test important combinations and track cleanup after the rollout. Hiding a feature through a client flag does not restrict the underlying API, and disabling a flag cannot automatically reverse data changes or external effects already produced.

#### 88. How do you handle a production incident involving a recent release?

Tags: #IncidentResponse #CompanyUnverified #AmazonRelevant

I assess impact, coordinate ownership, and prioritize safe mitigation while preserving useful evidence. A rollback, feature disablement, or capacity action may restore service faster than root-cause analysis. I communicate verified facts, uncertainty, and the next update time through the agreed process.

I check whether rollback is safe for the current data and mixed-version state. After stabilization, I investigate the cause and implement proportionate prevention or detection improvements. I verify customer workflow recovery rather than stopping when a dashboard turns green, especially if failed jobs or uncertain side effects need reconciliation.

#### 89. How do you deliver into a restricted enterprise environment?

Tags: #EnterpriseDeployment #CompanyUnverified #AmazonRelevant

I verify approved identity, network routes, proxies, certificates, deployment tools, data access, and external dependencies early. I test a thin end-to-end slice from the actual target environment. I include artifact delivery, updates, monitoring, and support access in the plan.

If an external service is not permitted, I adjust the architecture or scope with the customer. I do not assume bypassing a control is part of implementation. A successful local demo proves little about production connectivity, so the pilot should expose important environment constraints before the team invests heavily in unsupported assumptions.

#### 90. What belongs in a full-stack operational handoff?

Tags: #OperationalHandoff #CompanyUnverified #AmazonRelevant

I provide architecture context, setup and deployment procedures, configuration ownership, monitoring, runbooks, and recovery steps. I identify support contacts, known limitations, data flows, and responsibility for credentials and dependency changes. I confirm who owns the service after the initial delivery team leaves.

I walk the receiving team through a routine update and a representative failure. This tests whether documentation and access are sufficient in practice. Handoff also includes user onboarding and feedback paths, because operating the infrastructure does not guarantee the customer can complete the business workflow or adopt the application.

### Practical full-stack and FDE scenarios (91–100)

These answers are proposed approaches, not claims of completed projects. Clarify assumptions and choose a scope appropriate to the interview before implementing or drawing the design.

#### 91. How would you load a detail table from an API when a user selects a row?

Tags: #MasterDetailUI #AVBMarketing #CandidateReportedAdapted

**Attribution:** Adapted from the table-to-detail interaction in the June 30, 2021 AVB Marketing React TypeScript Full Stack Developer entry on [Glassdoor's question listing](https://www.glassdoor.com/Interview/typescript-full-stack-developer-interview-questions-SRCH_KO0%2C31.htm). The entry also discusses sessions and a dialog; this question attributes only the relevant interaction. The report is not independently verified.

I give rows stable identifiers and expose an accessible selection control or appropriate table interaction. Selection updates the active identifier and triggers an authorized detail request. The detail area represents loading, error, empty, and success states and clearly identifies which record it describes.

I cancel or ignore outdated requests so a slow earlier selection cannot replace the current result. The backend verifies access to the requested record regardless of what the master table showed. I define caching and refresh behavior and test rapid selection, denied access, and deletion between selection and retrieval. If the design uses a dialog, focus entry, keyboard interaction, and focus restoration also need deliberate handling.

#### 92. Build a searchable, paginated customer table. What do you implement first?

Tags: #SearchableTable #CompanyUnverified #AmazonRelevant

I clarify searchable fields, sort order, expected volume, permissions, and the definition of a useful result. I build a complete path from accessible controls to a validated API query and indexed database access. The response includes stable record identities and the agreed pagination information.

I preserve query state in the URL where useful, reset pagination when filters change, and protect against stale responses. I debounce input only if it improves the interaction and respect cancellation and quotas. I test empty results, invalid filters, authorization, rapid searches, and navigating backward. Server-side filtering ensures a search does not silently examine only the page currently loaded in the browser.

#### 93. Design an asynchronous CSV import with row-level errors.

Tags: #ImportWorkflow #CompanyUnverified #AmazonRelevant

I create an authorized upload session, validate file constraints, and durably register an import job. A worker parses records with bounded memory, validates the agreed schema and business rules, and records progress and errors. I define whether acceptance is atomic for the whole file or supports explicit partial success.

The interface shows job status and an authorized error report that lets users correct rejected rows. Stable job and record identities prevent retrying the import from duplicating valid changes. I reconcile counts and important values, isolate untrusted processing, and clean up files under the approved retention policy. A preview can catch mapping mistakes before effects are committed.

#### 94. Design an approval workflow with multiple reviewers.

Tags: #ApprovalWorkflow #CompanyUnverified #AmazonRelevant

I clarify whether reviews are sequential or parallel, how many approvals are required, who may act, and which changes invalidate earlier decisions. I model requests, reviewer decisions, versions, and allowed transitions. The backend enforces these rules in the authoritative state change rather than relying on disabled interface controls.

I use concurrency protection so simultaneous decisions cannot create an invalid final state. Notifications follow a reliable handoff and are safe under retries. The interface explains pending reviewers, conflicts, and rejected transitions. I include an audit trail, delegation or expiry rules where required, and tests for changed permissions, repeated submissions, and edits after approval has started.

#### 95. How would you build a chat interface with streamed AI responses?

Tags: #StreamingChat #CompanyUnverified #AmazonRelevant

I clarify history, identity, data permissions, cancellation, and what users expect during partial output. The backend authorizes the conversation, controls model or tool access, and streams events with a defined format. The client distinguishes pending, streaming, completed, stopped, and failed responses and renders content safely.

I decide what is durably stored and how reconnection or retry avoids duplicating messages and side effects. Stopping the display does not prove every remote operation stopped. I bound context, execution, and cost, handle backpressure, and protect credentials server-side. Evaluation includes answer quality and unauthorized tool behavior as well as smooth rendering and time to first useful output.

#### 96. How do you prevent duplicate orders when a customer clicks Submit twice?

Tags: #DuplicateSubmission #CompanyUnverified #AmazonRelevant

I improve the interface with clear pending status and appropriate submission controls, but enforce uniqueness at the authoritative backend boundary. The client associates a stable idempotency key with the intended operation and reuses it for retries. The server atomically records the resulting order and matching operation identity, with request-payload checks.

I handle simultaneous requests and a lost response after successful creation. If payment is external, its idempotency and reconciliation must align with the order workflow. I test double clicks, separate tabs, replayed requests, and crashes around persistence. A browser-only debounce cannot guarantee one order because callers and networks can repeat the request independently.

#### 97. The customer says a screen is slow, but the API is fast. What do you investigate?

Tags: #CrossLayerPerformance #CompanyUnverified #AmazonRelevant

I reproduce the actual user task on a representative device and network and align the measurement boundaries. The API metric may exclude connection setup, multiple sequential calls, large downloads, parsing, rendering, or waiting for another interaction. I inspect a browser trace and network waterfall and connect requests to backend evidence.

If the screen fetches unnecessary data or renders thousands of complex rows, I address that bottleneck rather than tuning an already-fast handler. I also check caching and environment-specific proxies. I verify the same task after the change using user-visible completion and responsiveness, not just one component's isolated timing.

#### 98. How would you build a field application for unreliable connectivity?

Tags: #OfflineWorkflow #CompanyUnverified #AmazonRelevant

I identify which reads and actions may work offline and which require current server authority. I store only approved local data, queue permitted operations durably with stable identifiers, and display sync state clearly. Reconnection uses authenticated synchronization, idempotency, and a defined conflict policy.

Offline draft capture may be safe, while reserving a scarce resource may need an online decision. I include local security, data cleanup, device loss, stale permissions, and storage limits. I test disconnection during each transition and ensure users can distinguish saved locally from accepted by the server. Offline support should name its guarantees rather than imply the full online workflow remains unchanged.

#### 99. You have one week to deliver a full-stack customer pilot. How do you scope it?

Tags: #PilotDelivery #CompanyUnverified #AmazonRelevant

I agree on one valuable workflow, representative users, acceptance criteria, and the decision the pilot should enable. I validate access, data, and deployment constraints immediately, then build a thin end-to-end slice. I defer secondary features and keep limitations explicit while preserving controls appropriate to the data and actions involved.

I reserve time for real user feedback, failure handling, deployment, and a basic operational handoff. Mocks can accelerate isolated development but do not validate the integration they replace. At the end, I compare evidence with the pilot criteria and state what must change before broader production use rather than treating an attractive demo as proof of readiness.

#### 100. What does “done” mean for a full-stack feature?

Tags: #DefinitionOfDone #CompanyUnverified #AmazonRelevant

Done means the agreed user workflow meets its acceptance criteria across the relevant layers and can be delivered and supported in its intended environment. Depending on risk and scope, that includes access control, data correctness, error handling, appropriate tests, performance validation, deployment, monitoring, and documentation.

I verify the outcome with users and confirm operational ownership and known limitations. For a pilot, the completion standard may be narrower than a broad production release, but that distinction must be explicit. A merged change or successful build is evidence of progress; successful, sustainable customer use is the broader delivery goal.

## Practice and review checklist

Practice ten questions at a time. Start with a direct answer, then add a concrete example, a tradeoff, a failure path, and verification. For behavioral follow-ups, use truthful examples and distinguish your contribution from the team's work.

For a practical exercise, check:

1. Can the intended user complete the workflow with keyboard-accessible controls and clear feedback?
2. Are validation, authorization, and tenant boundaries enforced server-side?
3. Can stale responses, duplicate requests, or concurrent edits corrupt the displayed or stored result?
4. Are database and external side effects coordinated or reconciled deliberately?
5. Are failures visible and recoverable without exposing secrets or sensitive customer data?
6. Does the deployment work in the target environment with compatible application and schema versions?
7. Do verification and monitoring measure the user outcome as well as individual component health?
8. Is there an owner for support, access, dependency updates, and recovery?

Continue with [system design](../03-system-design/README.md), [JavaScript and TypeScript](../06-javascript-typescript/README.md), [React frontend](../07-react-frontend/README.md), [backend APIs](../08-backend-apis/README.md), [databases](../09-databases/README.md), and [customer deployment](../17-customer-deployment/README.md).
