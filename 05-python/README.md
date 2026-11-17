# Python

Python questions common in FDE loops: data wrangling, scripting against APIs, packaging, typing, concurrency, and writing code another engineer can run inside a customer environment.

This curated set of 100 questions covers Python fundamentals and practical Forward Deployed Engineer work. “Top 100” is a broad study selection, not a measured frequency ranking. Examples target Python 3.11+ unless a later version is explicitly discussed. Distinguish Python language guarantees from CPython implementation details and confirm the customer's runtime and dependencies before relying on version-specific behavior.

## Company hashtags and attribution

Every question includes a topic and an explicit company-status label. **Relevance is not evidence that a company asked the question.** Unverified origins are not presented as company interview facts.

| Hashtags | Meaning |
| --- | --- |
| `#CompanyUnverified #AmazonRelevant` | Original practice question with unknown company origin. Relevance is an editorial inference from Amazon's broad [software-development interview topics](https://www.amazon.jobs/content/en/how-we-hire/interview-prep/software-development-topics), not attribution of this Python prompt to Amazon. |
| `#DellTechnologies #CandidateReportedAdapted` | Questions 5, 24, 37, and 82 adapt individual prompts from a [candidate-reported Dell Python Developer question list][dell-report]. |
| `#Reddit #CandidateReportedAdapted` | Question 27 adapts an iterator/generator prompt the author of a [first-person interview account][yield-report] attributes to Reddit, the company. |
| `#NVIDIA #CandidateReportedAdapted` | Question 28 adapts a lazy-evaluation prompt the same [first-person account][yield-report] attributes to an NVIDIA hiring manager. |
| `#AlphaGrepSecurities #CandidateReportedAdapted` | Question 51 adapts a GIL prompt attributed to an AlphaGrep Securities Python Developer interview on a [candidate-report page][gil-report]. |

Sources reviewed November 17, 2026. There are 93 questions with unverified company origins and seven candidate-reported attributions drawn from three reports. These are public reports, not independent verification, company-endorsed answers, or proof of use in an FDE interview. No individual interviewer identity is established. Added examples, follow-ups, and answers are original study material; technical references support concepts rather than interview provenance.

## Quick cheatsheet

| Area | Remember |
| --- | --- |
| Objects | Names refer to objects. Assignment does not copy; mutability and aliasing determine side effects. |
| Equality | `==` compares values through equality behavior; `is` compares identity. Use `is None` for the sentinel. |
| Defaults | Defaults are evaluated when the function definition executes. Use `None` or another sentinel for per-call mutable defaults. |
| Collections | List: ordered mutable sequence. Tuple: immutable sequence. Dict: insertion-ordered mapping. Set: distinct hashable elements without an ordering contract. |
| Complexity | Dict/set lookup is expected O(1), not guaranteed O(1) in every case. List membership is O(n); list append is amortized O(1). |
| Iteration | Iterable supplies an iterator; iterator supplies successive values; generators create resumable, single-pass iteration. |
| Functions | Know argument binding, closures, decorators, `functools.wraps`, and resource lifetime. |
| Types | Type hints support tools; they do not automatically validate runtime input. |
| Exceptions | Catch specific failures, preserve context, clean up with `with` or `finally`, and do not silently swallow errors. |
| Files | Use context managers, explicit encodings, streaming for large input, and clear partial-write behavior. |
| Numbers | Floating point is approximate. Use an agreed numeric model such as `Decimal` when exact decimal rules matter. |
| Time | Use timezone-aware timestamps for instants and a monotonic clock for elapsed durations. |
| Concurrency | Threads for suitable blocking I/O; asyncio for cooperative async I/O; processes for suitable CPU-bound work with GIL-enabled CPython. Benchmark the real workload. |
| GIL | GIL-enabled CPython limits simultaneous Python-bytecode execution within an interpreter; optional free-threaded builds change that assumption. Neither makes application invariants automatically thread-safe. |
| Async | Calling an async function creates a coroutine. Awaiting does not make blocking CPU work nonblocking. Bound tasks, connections, and queues separately. |
| APIs | Set deadlines, validate responses, respect quotas, retry selectively, and reconcile uncertain side effects. |
| Packaging | Declare runtime support and dependencies; isolate environments; validate reproducibility on the target platform. |
| Production | Include safe configuration, secret handling, structured diagnostics, meaningful tests, recovery, and an operational owner. |

**Useful standard-library tools:** `pathlib`, `csv`, `json`, `collections.Counter`, `collections.defaultdict`, `collections.deque`, `itertools`, `heapq`, `dataclasses`, `contextlib`, `argparse`, `logging`, `unittest`, `unittest.mock`, `time.perf_counter`, `tracemalloc`, `concurrent.futures`, and `asyncio`. Choose a tool because it fits the task, not to make code shorter at the expense of clarity.

**Interview answer pattern:** clarify input and constraints → explain the approach → show a small example → state complexity and limitations → describe verification.

## Study map

| Questions | Focus |
| --- | --- |
| 1–10 | Objects, values, scope, and language basics |
| 11–20 | Collections, algorithms, and complexity |
| 21–30 | Functions, decorators, and iteration |
| 31–40 | Classes, resource management, and exceptions |
| 41–50 | Typing, files, serialization, and numeric data |
| 51–60 | Threads, processes, and asyncio |
| 61–70 | APIs, databases, and customer data pipelines |
| 71–80 | Testing, debugging, and performance |
| 81–90 | Packaging, configuration, security, and delivery |
| 91–100 | Practical coding and FDE scenarios |

## Questions and detailed answers

### Objects, values, scope, and language basics (1–10)

#### 1. What does it mean that Python is dynamically typed?

Tags: #DynamicTyping #CompanyUnverified #AmazonRelevant

Objects have types, while names can be rebound to objects of different types at runtime. Operations are checked according to the objects involved rather than a permanently declared type for each variable. Dynamic typing does not mean Python freely accepts every combination of types or has no type rules.

I use clear interfaces, validation at external boundaries, and type annotations where they improve maintainability. For example, a JSON field that sometimes contains text and sometimes a number needs a deliberate normalization policy. An annotation alone does not make inconsistent customer input safe to use.

#### 2. How do assignment, aliasing, and mutability interact?

Tags: #ObjectModel #CompanyUnverified #AmazonRelevant

Assignment binds a name to an object rather than cloning it. If two names refer to the same mutable object, a mutation through either name can be observed through the other. Rebinding one name to a different object does not rebind the other name.

For example, `b = a` shares a list, while `b = a.copy()` creates a new outer list. I check whether a function mutates caller-owned data and document that behavior. Understanding this distinction prevents accidental changes in caches, default configuration, and intermediate data structures used by several processing steps.

#### 3. How do equality and identity differ?

Tags: #EqualityAndIdentity #CompanyUnverified #AmazonRelevant

`==` asks whether objects compare equal according to their equality behavior; `is` asks whether they are the same object. Equal lists can be distinct objects. I use identity for sentinels such as `None`, not for comparing strings or numbers whose implementation may reuse objects in some situations.

For custom classes, equality and hashing must agree if instances are used in hash-based collections. I do not base correctness on interning or small-object caching. A test that happens to pass with `is` on one literal can fail when an equivalent value arrives from a file or API.

#### 4. Why are mutable default arguments a common bug?

Tags: #DefaultArguments #CompanyUnverified #AmazonRelevant

Default expressions are evaluated when the function definition executes, not each time the function is called. A default list or dictionary can therefore be shared across calls. Mutating it may unintentionally carry one customer's or request's data into another invocation.

I use a sentinel such as `None` and create the object inside the function when a new object is required per call. If `None` itself is a meaningful input, I use a distinct sentinel. Deliberate shared caching should be explicit and bounded rather than hidden inside a default parameter.

#### 5. How do shallow and deep copying differ for nested objects?

Tags: #Copying #DellTechnologies #CandidateReportedAdapted

**Attribution:** Adapted from the copying prompt in the [Dell Technologies Python Developer candidate report][dell-report]. The answer and examples below are not supplied or endorsed by Dell.

A shallow copy creates a new outer container while retaining references to nested objects. A deep copy recursively copies supported contents, using bookkeeping to handle repeated references and cycles. Neither assignment nor a shallow copy makes every nested object independent.

For example, modifying a nested list can affect both shallow copies. I choose a copy policy based on ownership rather than deep-copying everything, because some data should remain shared and resources such as open files are not ordinary copyable values. See the [standard-library copy documentation](https://docs.python.org/3/library/copy.html) for supported behavior and customization.

#### 6. What does hashability require?

Tags: #Hashability #CompanyUnverified #AmazonRelevant

A hashable object supplies a stable hash over its lifetime, with equal objects producing equal hashes. Dictionaries and sets rely on that relationship. A tuple is hashable only when its elements are hashable, so an immutable outer container does not automatically make an arbitrary nested structure suitable as a key.

For custom classes, I avoid basing hashes on mutable fields that can change after insertion. Identity-based objects can still be mutable and hashable, so “hashable means immutable” is an oversimplification. I choose keys that express stable domain identity and test equality behavior where custom objects are involved.

#### 7. How do truthiness and None checks differ?

Tags: #Truthiness #CompanyUnverified #AmazonRelevant

Truthiness supports concise conditions for values such as empty collections and zero, as well as custom objects with truth-value behavior. `value is None` specifically detects the `None` sentinel. The choice depends on whether empty, zero, false, and missing mean the same thing in the domain.

For example, `timeout = supplied or default` replaces a valid zero with the default. I use an explicit missing-value check when zero has meaning. External tabular values can also have special missing-data behavior, so I do not assume a generic truthiness test correctly validates every data type.

#### 8. How does Python resolve names in nested scopes?

Tags: #Scope #CompanyUnverified #AmazonRelevant

For ordinary function code, name lookup follows local, enclosing function, module-global, and built-in scopes. Assignment normally makes a name local to that function unless declared otherwise. `nonlocal` targets an enclosing function binding, while `global` refers to a module-level binding.

A function that reads a name before assigning to its local binding can raise `UnboundLocalError`, even if a global value exists. I prefer explicit inputs and returned values over hidden global mutation in customer scripts. Class bodies and comprehensions have additional scope details, so the simple lookup mnemonic should not replace understanding the actual construct.

#### 9. What is the difference between a Python language guarantee and a CPython detail?

Tags: #PythonImplementations #CompanyUnverified #AmazonRelevant

The language specifies observable semantics that compatible implementations should follow, while CPython chooses particular execution and memory-management mechanisms. Bytecode format, reference counting, and many low-level performance properties are implementation-specific. I identify which kind of claim I am making in an interview.

For example, I use a context manager to close a file instead of depending on the moment a reference disappears. I also confirm interpreter version and build before explaining concurrency behavior. This matters when moving scripts between a developer laptop, a customer platform, and an alternative runtime with different implementation characteristics.

#### 10. How are str and bytes different?

Tags: #TextAndBytes #CompanyUnverified #AmazonRelevant

`str` represents Unicode text, while `bytes` represents a sequence of byte values. Encoding converts text to bytes, and decoding converts bytes to text using an agreed encoding and error policy. They should not be mixed implicitly when processing network data or files.

I decode at an appropriate boundary and preserve raw data when needed for investigation under approved handling rules. Ignoring decoding errors can silently change identifiers or customer content. I also distinguish code points from user-perceived characters; slicing a string is not a complete solution for every language-sensitive display or normalization task.

### Collections, algorithms, and complexity (11–20)

#### 11. When do you choose a list or a tuple?

Tags: #Sequences #CompanyUnverified #AmazonRelevant

A list is a mutable ordered sequence suited to an evolving collection. A tuple is an immutable sequence often useful for a fixed grouping or return value. Tuple immutability applies to its references, not necessarily to the objects those references point to.

I choose based on the meaning and operations, not only a presumed speed advantage. For a record with named fields, a dataclass or named tuple may communicate intent better than positions. A tuple containing a list can still expose mutable contents and cannot serve as a hashable key merely because its outer structure is fixed.

#### 12. What ordering guarantees do dictionaries and sets provide?

Tags: #MappingOrder #CompanyUnverified #AmazonRelevant

Python dictionaries preserve insertion order, while sets do not provide a general ordering contract for application logic. Updating an existing dictionary value does not move its key, and removing then reinserting a key changes its insertion position. Sorting is a separate operation with an explicit ordering rule.

I use dictionaries when I need key-based lookup and intentional traversal order, but I do not confuse that with sorted keys. For reproducible reports, I specify a sort or another stable policy. Set traversal that looks consistent locally is not an appropriate basis for customer-visible ordering.

#### 13. How do you discuss collection-operation complexity accurately?

Tags: #Complexity #CompanyUnverified #AmazonRelevant

I state both the operation and the assumptions. List indexing is constant time in typical Python list implementations, membership scans are linear, and append is amortized constant time. Hash-table lookup in dictionaries and sets is expected constant time under suitable conditions, with possible worse behavior.

I include costs of hashing, equality, copying, and returned output when they matter. An operation on a large string key is not free simply because a lookup uses a dictionary. I also distinguish asymptotic complexity from measured performance and memory use on the actual workload.

#### 14. When is a comprehension better than an explicit loop?

Tags: #Comprehensions #CompanyUnverified #AmazonRelevant

A comprehension expresses a straightforward transformation or filter compactly. I use one when the operation remains readable and the output should be materialized. An explicit loop is clearer when processing includes several branches, validation errors, logging, or state transitions.

I avoid a deeply nested expression merely to fit an answer on one line. For large or single-pass input, a generator expression may avoid allocating the entire result. I also consider when side effects and exceptions occur, because changing materialization into lazy iteration changes the timing of work and failure.

#### 15. How do sorted and list.sort differ?

Tags: #Sorting #CompanyUnverified #AmazonRelevant

`sorted` consumes an iterable and returns a new list. `list.sort` changes that list in place and returns `None`. Python's sorting is stable, so equal-key items retain their input order, which is useful when a secondary order already exists.

I define a key function that captures the required comparison and handle missing or mixed-type values deliberately. For deterministic customer output, I add a tie-breaker when input order is not itself meaningful. Sorting all records costs time and memory, so a streaming top-k approach may be better when only a few results are needed.

#### 16. When should you use deque instead of a list?

Tags: #Deque #CompanyUnverified #AmazonRelevant

`collections.deque` supports efficient appends and removals at both ends, making it useful for queues and sliding windows. Removing the first element of a list shifts the remaining elements. Lists remain appropriate when frequent indexed access and contiguous sequence behavior are important.

I choose a bounded deque when retaining only recent items is the intended policy. That bound silently discards old items as new ones arrive, so it is unsuitable for work that must never be lost without additional handling. For concurrent worker coordination, I use an appropriate queue abstraction rather than assuming a deque solves synchronization and backpressure.

#### 17. When is a heap useful?

Tags: #Heaps #CompanyUnverified #AmazonRelevant

A heap is useful for repeatedly retrieving the smallest-priority item or maintaining a bounded candidate set. The standard `heapq` interface supports a min-heap, with logarithmic push and pop operations and efficient bulk heap construction. I explain the priority and tie-breaking policy explicitly.

For example, a priority queue can use a sequence number to avoid comparing unrelated payload objects when priorities tie. A heap does not keep all elements in globally sorted traversal order. If an item's priority changes, I need a deliberate update or invalidation strategy rather than mutating its priority in place and assuming the heap repairs itself.

#### 18. How do Counter and defaultdict simplify data processing?

Tags: #CollectionsUtilities #CompanyUnverified #AmazonRelevant

`Counter` models counts of hashable values, while `defaultdict` supplies a value through a factory when a missing key is accessed through indexing. They can simplify aggregation or grouping without repeated initialization branches. I still define treatment of invalid keys and missing values.

For example, grouping records with `defaultdict(list)` is concise, but the result grows with the data. I do not assume convenience makes the approach memory bounded. I also remember that a missing-key read through indexing can mutate a defaultdict, which may surprise code that intended only to inspect the mapping.

#### 19. How do you remove duplicates while preserving order?

Tags: #Deduplication #CompanyUnverified #AmazonRelevant

For hashable values, I can track a set of seen keys and yield the first occurrence, or materialize using insertion-ordered dictionary keys when that fits. For records, I define a stable business key and whether the first or latest occurrence should win.

I account for memory proportional to distinct keys and clarify what counts as equality. A streaming generator avoids storing the output list but still retains the seen set. For unbounded inputs or durable deduplication across restarts, I need a bounded policy or external state; the in-memory pattern alone does not provide that guarantee.

#### 20. How do you choose between a linear scan, an index, and sorting?

Tags: #AlgorithmSelection #CompanyUnverified #AmazonRelevant

I compare the input size, number of lookups, mutation pattern, memory budget, and output requirements. One lookup may justify a simple linear scan; many lookups can justify building a dictionary. Sorting helps ordered output or repeated range-style processing when its cost is worthwhile.

For example, joining two large collections through nested loops is often avoidable by indexing one side. I include index construction and duplicate-key policy in the explanation. An optimization should improve the actual workload without silently changing semantics, such as dropping records because a dictionary comprehension overwrote repeated keys.

### Functions, decorators, and iteration (21–30)

#### 21. How do positional-only, keyword-only, args, and kwargs work?

Tags: #FunctionSignatures #CompanyUnverified #AmazonRelevant

A function signature controls how callers bind arguments. `/` marks positional-only parameters; parameters after `*` can be keyword-only. `*args` collects extra positional arguments, and `**kwargs` collects extra keyword arguments. I choose a signature that communicates which inputs need explicit names.

For example, `def export(rows, *, overwrite=False)` makes a consequential option visible at the call site. Accepting arbitrary keywords can hide misspellings unless they are validated or forwarded through a defined contract. Flexible wrappers should preserve documented behavior instead of turning a clear interface into an opaque bag of options.

#### 22. How are arguments passed to Python functions?

Tags: #ArgumentPassing #CompanyUnverified #AmazonRelevant

Parameters are local names bound to the objects supplied by the caller. Reassigning a parameter does not reassign the caller's variable, but mutating a shared mutable object can affect the caller. Describing this as object sharing is more precise than implying ordinary call-by-reference variable reassignment.

For example, appending to an input list changes that list, while assigning the parameter to a new list does not replace the caller's binding. I make mutation part of the function contract and avoid depending on callers to infer it from implementation details. Tests should check both returned results and relevant input preservation.

#### 23. What is late binding in closures?

Tags: #Closures #CompanyUnverified #AmazonRelevant

A closure retains access to an enclosing binding rather than necessarily freezing its current value. Functions created in a loop can therefore all observe the binding's later value when they execute. This matters for callbacks, deferred tasks, and dynamically constructed handlers.

I use a factory function or an intentional default binding when each callable needs its own captured value. I distinguish that technique from accidentally sharing a mutable default. A useful interview demonstration creates several callbacks, executes them after the loop, and explains exactly which environment each one reads rather than memorizing only the surprising output.

#### 24. How does a decorator change callable behavior?

Tags: #Decorators #DellTechnologies #CandidateReportedAdapted

**Attribution:** Adapted from the decorator prompts in the [Dell Technologies candidate-reported question list][dell-report]. This guide adds its own production considerations.

A decorator receives an object, often a function, and returns the replacement bound to the decorated name. It can wrap behavior such as measurement, validation, or authorization. A decorator factory adds a configuration stage, and multiple decorators compose in a defined nesting order.

I preserve metadata with `functools.wraps` when wrapping a function and keep the wrapper appropriate to synchronous or asynchronous execution. Timing an async function without awaiting its work measures coroutine creation rather than completion. I also avoid generic retry decorators that repeat unsafe side effects. Clear signatures and documented behavior matter more than clever wrapping.

#### 25. When should you cache a function result?

Tags: #Memoization #CompanyUnverified #AmazonRelevant

Caching helps when repeated calls have reusable results and the function's inputs describe the relevant context. I consider key hashability, memory limits, invalidation, tenant context, and mutable return values. A cache around impure or time-sensitive work can return incorrect or unauthorized results.

`lru_cache` can bound entry count, but that is not a byte-size or freshness guarantee. Concurrent callers may still execute the wrapped function more than once before a result is stored, so it is not an exactly-once mechanism. See the [functools documentation](https://docs.python.org/3/library/functools.html) for these semantics and supported utilities.

#### 26. When do you use lambda, partial, or a named function?

Tags: #Callables #CompanyUnverified #AmazonRelevant

A lambda is useful for a short expression such as a sort key. `functools.partial` creates a callable with some arguments already bound. A named function communicates intent and supports clearer documentation, debugging, and more complex logic.

I choose readability and the execution environment over compactness. For example, process-based execution may require a suitably importable callable rather than a local lambda. I also inspect closure capture and mutable bound arguments, because partial application does not automatically copy an object or make it safe to share between concurrent operations.

#### 27. How does a generator relate to an iterator?

Tags: #Generators #Reddit #CandidateReportedAdapted

**Attribution:** Adapted from the iterator/generator prompt attributed to Reddit, the company, in a [first-person interview account][yield-report]. This is the author's report, not company confirmation.

An iterator produces successive values through `next` and signals exhaustion with `StopIteration`. A generator function creates an iterator whose execution suspends at `yield` and resumes on demand. All generators are iterators, but iterators can also be implemented by classes or other mechanisms.

A generator is normally single-pass; once exhausted, it does not restart merely because a loop uses it again. Laziness can reduce materialization but also defer errors and keep resources alive. I define who owns iteration and cleanup, especially when yielding records from an open file or database cursor.

#### 28. What is lazy evaluation, and what tradeoffs does it introduce?

Tags: #LazyEvaluation #NVIDIA #CandidateReportedAdapted

**Attribution:** Adapted from a lazy-evaluation prompt attributed to an NVIDIA hiring manager in the same [first-person interview account][yield-report]. No individual interviewer identity or independent verification is established.

Lazy processing postpones work until a consumer requests a result. Generator expressions and iterators can avoid computing or storing unused values, which helps large pipelines and early termination. This is a property of particular constructs, not a statement that Python evaluates all expressions lazily.

The tradeoffs include delayed exceptions, single-pass consumption, and resource lifetime tied to iteration. A lazy pipeline that later sorts everything still materializes data at that stage. I explain where memory is retained and when side effects happen rather than claiming that replacing a list with a generator makes the whole workflow constant space.

#### 29. What does yield from do?

Tags: #GeneratorDelegation #CompanyUnverified #AmazonRelevant

`yield from` delegates iteration to another iterable and, for generators, participates in forwarding the generator protocol. It can simplify composition compared with manually relaying values. Delegation does not recursively flatten arbitrary structures unless the code explicitly defines that behavior.

I use it when one generator naturally contains another sequence of results. I still consider errors and cleanup, including what happens if the consumer stops early. For nested customer data, I define which types are containers; treating strings as generic iterables during recursive flattening can produce unintended character-level results or recursive mistakes.

#### 30. How do you process an iterable in batches without loading everything?

Tags: #Batching #CompanyUnverified #AmazonRelevant

I take one iterator and repeatedly collect at most the chosen batch size, stopping when no items remain. `itertools.islice` supports a portable implementation for the stated Python baseline. I validate that the size is positive and define whether the last incomplete batch is accepted.

Memory for the batching layer is proportional to batch size, but downstream results or upstream buffers can still grow. I also distinguish bounded data batches from bounded concurrency: scheduling every batch at once defeats a memory limit. For retries, I need stable batch or record identities and a defined partial-success policy.

### Classes, resource management, and exceptions (31–40)

#### 31. How do instance attributes and class attributes differ?

Tags: #ClassState #CompanyUnverified #AmazonRelevant

Instance attributes belong to a particular object, while class attributes participate in lookup through the class and can supply shared values. A mutable list placed on the class can therefore be shared by instances that have not overridden it. I create per-instance mutable state during initialization when sharing is not intended.

For example, a customer connector should not accidentally share a pending-record list with every other connector. Class-level constants can be appropriate, but shared mutable state needs clear ownership and synchronization. I also account for descriptors and properties when explaining attribute lookup rather than assuming every read simply checks two dictionaries.

#### 32. What do __new__ and __init__ do?

Tags: #ObjectConstruction #CompanyUnverified #AmazonRelevant

`__new__` creates or returns the object, while `__init__` initializes an instance after creation under the normal construction path. `__init__` must return `None`; it is not the function that returns the new instance. Most ordinary classes need only a clear initializer.

I customize construction only when the object's semantics require it, such as certain immutable subclasses. I avoid hiding network calls or large, failure-prone operations in basic object initialization without a clear API. Separating configuration from connection or loading can make resource ownership, retries, and tests easier to understand.

#### 33. How do instance methods, class methods, and static methods differ?

Tags: #MethodBinding #CompanyUnverified #AmazonRelevant

An instance method receives the instance through binding, a class method receives the class, and a static method receives no automatic instance or class argument. A class method is useful for an alternative constructor that respects subclasses. A static method can group a related helper when no object state is needed.

I do not use these decorators merely to avoid passing arguments. For example, `from_config` can construct an instance with validated settings, while a pure parsing helper may belong at module level. The interface should make dependencies explicit and avoid mixing global behavior with per-instance state unexpectedly.

#### 34. When should you use a dataclass?

Tags: #Dataclasses #CompanyUnverified #AmazonRelevant

A dataclass reduces boilerplate for classes that primarily hold fields by generating methods according to its configuration. I choose equality, representation, mutability, and defaults intentionally. Per-instance mutable fields should use a suitable `default_factory` rather than a shared object.

Annotations do not make a dataclass validate incoming JSON, and a frozen instance does not recursively freeze every nested value. I use explicit parsing or validation for untrusted input. A dataclass is useful for internal domain values or configuration, but it should not be confused with a complete database model, authorization layer, or schema-validation system.

#### 35. How do inheritance, composition, and super work together?

Tags: #ObjectOrientedDesign #CompanyUnverified #AmazonRelevant

Inheritance models a substitutable relationship and participates in method resolution. Composition assembles behavior by holding collaborators, often making dependencies and testing simpler. `super` follows the method-resolution order rather than meaning only “call this one immediate parent.”

I use inheritance when the contract and extension points are clear, and composition when combining independent responsibilities such as storage, HTTP access, and reporting. Cooperative multiple inheritance requires compatible methods and consistent forwarding. For a customer integration, a small adapter with injected collaborators may be easier to maintain than a deep hierarchy that couples unrelated systems.

#### 36. When do you use properties or special methods?

Tags: #PythonProtocols #CompanyUnverified #AmazonRelevant

A property can provide controlled attribute-like access while preserving a clear interface. Special methods let objects participate in protocols such as iteration, context management, comparison, and representation. I implement a protocol when its expected behavior matches the domain rather than surprising callers.

For example, `__repr__` should help diagnostics without revealing credentials. A property that silently performs an expensive remote call may violate user expectations and complicate failure handling. I also avoid inventing arbitrary double-underscore names; the established protocols have specific semantics, and a conventional named method is often clearer for an explicit operation.

#### 37. How do you structure exception handling and preserve useful error context?

Tags: #Exceptions #DellTechnologies #CandidateReportedAdapted

**Attribution:** Adapted from the exception-control-flow prompt in the [Dell Technologies Python Developer candidate report][dell-report]. Additional guidance here is original preparation material.

I catch specific exceptions at a boundary where I can recover, translate the failure, or add meaningful context. `else` can hold work that should run only after the protected block succeeds; `finally` handles required cleanup. I use a bare `raise` to re-raise the current exception or explicit chaining when introducing a domain-specific failure.

I avoid broad suppression that reports success after processing failed. I also avoid returning from `finally`, which can obscure an exception or earlier result. Error messages should identify the failed operation without disclosing secrets, and resources should normally use context managers for predictable cleanup.

#### 38. How do context managers manage resources?

Tags: #ContextManagers #CompanyUnverified #AmazonRelevant

The `with` statement uses a context-management protocol to enter a scope and then exit it on normal completion or an exception. It is useful for files, locks, transactions, and temporary resources. A generator-based manager from `contextlib.contextmanager` can express acquisition and cleanup with `try/finally` around its yield.

I acquire resources carefully because an error during entry may require cleanup inside the acquisition code. An exit method can suppress an exception, so that behavior must be intentional. I do not rely on object destruction timing to release a customer database connection or a lock promptly.

#### 39. How do you define useful custom exceptions?

Tags: #ExceptionDesign #CompanyUnverified #AmazonRelevant

I define exceptions around failures callers need to distinguish, such as invalid input, unavailable dependencies, and rejected domain transitions. I normally subclass `Exception` and preserve the underlying cause when translating a lower-level failure. Structured fields can carry safe operation context without forcing callers to parse a message.

A separate class for every line of code creates noise, while one generic exception can hide recovery choices. I design the hierarchy around handling needs. I also keep cancellation and process-exit behavior separate rather than catching all `BaseException` instances and unintentionally preventing orderly shutdown.

#### 40. How does Python manage memory, and why can memory still grow unexpectedly?

Tags: #MemoryManagement #CompanyUnverified #AmazonRelevant

CPython commonly combines reference counting with machinery for collecting reference cycles, but applications should not depend on exact destruction timing. Objects remain live while reachable, including through caches, closures, globals, tasks, or exception tracebacks. Unbounded retention can look like a leak even when the collector is working correctly.

I inspect allocation and retention under a reproducible workload and bound structures that grow with requests. Process memory may not fall immediately when objects are freed because allocators and native libraries have their own behavior. I distinguish Python-object growth from native allocations before choosing a fix.

### Typing, files, serialization, and numeric data (41–50)

#### 41. What do type hints provide, and what do they not enforce?

Tags: #TypeHints #CompanyUnverified #AmazonRelevant

Type hints document expected interfaces and help static tools detect incompatible operations or missing cases. Ordinary Python does not automatically enforce them on function calls or variable assignment. A value read from a customer file still requires runtime validation before business logic relies on it.

I annotate useful boundaries and models while avoiding assertions or casts that simply hide uncertainty. A typed internal representation can follow explicit parsing of external data. I verify tool configuration and the supported runtime because typing syntax and features evolve. See the [typing documentation](https://docs.python.org/3/library/typing.html) for the runtime distinction and supported constructs.

#### 42. How do Any, object, and Optional differ?

Tags: #TypingSemantics #CompanyUnverified #AmazonRelevant

`Any` permits operations that largely bypass static checking, while `object` describes a value without granting arbitrary type-specific operations. `Optional[T]` or `T | None` means a value may be `None`; it does not mean a parameter has a default or may be omitted.

I narrow values using validated checks and keep missing, null, and empty distinct where the API requires it. For example, an omitted update field may mean “leave unchanged,” while explicit null means “clear.” Using one optional annotation without a parsing policy can collapse those different business actions into an ambiguous interface.

#### 43. When would you use a Protocol or an abstract base class?

Tags: #Interfaces #CompanyUnverified #AmazonRelevant

A Protocol supports structural typing: a collaborator can satisfy the required interface without inheriting from a particular base class. An abstract base class can express an explicit hierarchy and abstract methods, with its own runtime mechanisms. I choose based on the contract and whether nominal membership or shared implementation matters.

For an integration adapter, a small protocol for fetching records can make tests and alternate providers straightforward. Static conformance does not verify business behavior. Runtime-checkable protocols also do not provide comprehensive validation of signatures or semantics, so I still test important contracts against actual implementations.

#### 44. How do you process a large text file safely?

Tags: #FileIO #CompanyUnverified #AmazonRelevant

I open the file with a context manager and an explicit encoding, then iterate or read bounded chunks according to the format. I validate records and track line or record context for useful errors. I avoid reading everything into memory unless the size and operations justify it.

Line iteration does not guarantee a tiny memory bound if individual lines can be huge, so I consider record-size limits for untrusted input. I define whether processing stops or quarantines bad records. The caller should also understand when a generator still owns an open resource and how to close it on early termination.

#### 45. Why should you use a CSV parser instead of splitting on commas?

Tags: #CSV #CompanyUnverified #AmazonRelevant

CSV fields can contain delimiters, quotes, and embedded newlines under the chosen dialect. A proper parser handles those rules, while a simple split silently misinterprets valid data. I specify encoding, dialect, headers, and newline handling and validate the expected schema separately.

`csv.DictReader` provides named fields but does not enforce types or business requirements. I inspect missing or extra columns, duplicate headers, and row-level errors. If exported data will be opened in spreadsheet software, I also consider formula interpretation as an output-specific risk rather than assuming correctly escaped CSV is safe in every consumer.

#### 46. What should you validate when reading JSON?

Tags: #JSON #CompanyUnverified #AmazonRelevant

Parsing JSON establishes syntactic structure, not the expected application schema. I validate field presence, types, ranges, nesting, and cross-field rules and bound input size and resource consumption. I distinguish absent fields from explicit null when the workflow needs that difference.

Python's JSON tooling can have options and defaults around non-finite numbers or repeated object names that may not match a strict interoperability requirement. I configure and test those behaviors where relevant. I also avoid logging full rejected payloads if they contain sensitive customer information; useful diagnostics can identify the failing path without copying the entire record.

#### 47. How do you handle timestamps and time zones?

Tags: #Datetime #CompanyUnverified #AmazonRelevant

I use timezone-aware values to represent instants and preserve the relevant zone or offset information needed by the business. I define parsing, storage, comparison, and presentation rules rather than mixing naive local timestamps with UTC assumptions. Local scheduling needs explicit treatment of ambiguous or nonexistent times around clock changes.

For elapsed durations, I use a monotonic clock such as `time.perf_counter` rather than wall time that can be adjusted. A daily business cutoff is not always equivalent to adding exactly 24 hours. I test time-zone transitions and clarify the customer's intended policy before encoding it.

#### 48. When should you use Decimal instead of float?

Tags: #NumericPrecision #CompanyUnverified #AmazonRelevant

Binary floating point approximates many decimal fractions and is appropriate for many numerical tasks. `Decimal` supports decimal arithmetic under an explicit precision and rounding context. I choose based on the required numeric model, such as financial rounding rules, not a claim that one representation is universally more accurate.

I construct decimal values from agreed text or another exact representation rather than first introducing a binary approximation. I define quantization, rounding, units, and invalid-value behavior with the domain owner. Exact representation alone does not establish the correct business formula or prevent inconsistent treatment across systems.

#### 49. Why is untrusted pickle data dangerous?

Tags: #SerializationSecurity #CompanyUnverified #AmazonRelevant

Unpickling can execute code as part of reconstructing objects, so it is unsuitable for untrusted inputs. I choose a safer interchange format plus explicit validation when crossing a customer or service trust boundary. Signing or restricting a transport needs its own threat model and does not make arbitrary producers safe.

I also consider compatibility and long-term storage. Serialized Python objects can depend on importable classes and environment details, making them brittle as public contracts. For an integration, a versioned schema with clear field meaning is usually easier to inspect, validate, migrate, and support than an opaque object snapshot.

#### 50. How do you write output without leaving a misleading partial file?

Tags: #AtomicOutput #CompanyUnverified #AmazonRelevant

Where the filesystem supports the required semantics, I write to a temporary file in the destination filesystem, finish and close it, then replace the destination. I clean up failed temporary output and define whether an existing destination may be overwritten. I avoid treating rename behavior across filesystems as automatically atomic.

Atomic visibility and crash durability are different requirements. Stronger durability may need appropriate flushing and synchronization of both file and directory under platform-specific rules. For customer exports, I also verify permissions and metadata and avoid announcing completion before the final output is valid and reachable.

### Threads, processes, and asyncio (51–60)

#### 51. What does the GIL mean for Python concurrency?

Tags: #GIL #AlphaGrepSecurities #CandidateReportedAdapted

**Attribution:** Adapted from the GIL prompt attributed to an AlphaGrep Securities Python Developer interview on a [candidate-report page][gil-report]. The runtime caveats below are additional study guidance.

In GIL-enabled CPython, the interpreter lock limits simultaneous Python-bytecode execution by threads within that interpreter. Threads can still be useful for blocking I/O, and some native libraries release the lock during computation. The GIL does not make a compound business operation or shared application state automatically safe.

Optional free-threaded CPython builds change the parallel-execution assumption, and package compatibility can affect actual GIL behavior. I confirm the build and workload before recommending threads or processes and use proper synchronization either way. See [CPython's free-threading guidance](https://docs.python.org/3.14/howto/free-threading-python.html) rather than claiming that all Python runtimes have one identical threading model.

#### 52. When do you choose threads, processes, or asyncio?

Tags: #ConcurrencyChoices #CompanyUnverified #AmazonRelevant

I classify the work as waiting, CPU computation, or a mixture and inspect the libraries it uses. Threads often fit blocking I/O, asyncio fits cooperative asynchronous I/O, and processes can provide CPU parallelism for suitable GIL-enabled CPython workloads. Native code or a free-threaded build can change the performance picture.

I compare startup cost, memory, serialization, cancellation, and operational complexity. A download-and-parse pipeline may use different mechanisms for each stage with bounded handoffs. I benchmark realistic inputs rather than choosing based only on request count or the slogan that one mechanism is always fastest.

#### 53. How do you protect shared state between threads?

Tags: #ThreadSafety #CompanyUnverified #AmazonRelevant

I define the invariant and protect the entire relevant read-modify-write sequence with appropriate synchronization, or avoid shared mutation through ownership and message passing. I do not infer application safety from one built-in operation's implementation behavior or from the presence of a GIL.

For example, checking a key and then inserting a resource can race even if individual mapping operations are internally safe. Locks must have a clear scope and consistent acquisition order to avoid deadlock. I keep slow external operations outside critical sections where the invariant allows it and test realistic interleavings.

#### 54. What must you consider before using multiprocessing?

Tags: #Multiprocessing #CompanyUnverified #AmazonRelevant

Processes have separate ordinary memory and require deliberate communication or supported shared state. Worker functions and arguments need to fit the chosen start method and serialization requirements. I use an import-safe entry point and avoid assuming inherited connections or locks are usable in a child.

Defaults differ by operating system and Python version, so I verify or explicitly choose an appropriate context. I include startup and data-transfer costs and keep tasks large enough to justify them. See the [multiprocessing documentation](https://docs.python.org/3.14/library/multiprocessing.html) for start-method behavior and restrictions; portable code should not silently depend on historical fork defaults.

#### 55. What happens when you call an async function?

Tags: #Coroutines #CompanyUnverified #AmazonRelevant

Calling an async function produces a coroutine object; it does not by itself ensure the operation is run to completion. The coroutine must be awaited or scheduled through an appropriate event-loop mechanism. In an ordinary single-loop thread, tasks progress cooperatively when execution yields through suitable await points.

An `await` expression does not guarantee a useful scheduling pause if the awaited work completes immediately. Synchronous CPU work or a blocking client inside a coroutine can stall unrelated tasks. I choose truly asynchronous libraries or offload suitable blocking operations and make lifecycle ownership explicit.

#### 56. How do TaskGroup and gather differ in failure handling?

Tags: #StructuredConcurrency #CompanyUnverified #AmazonRelevant

`TaskGroup`, available from Python 3.11, owns a group of tasks and waits for them on exit. An ordinary child failure triggers cancellation of remaining tasks, with failures reported according to its exception-group semantics. `gather` has different default propagation behavior and does not simply provide the same sibling-cancellation policy.

I choose fail-together or independent-result semantics deliberately and handle cancellation cleanly. Neither abstraction inherently bounds how much work is created. For a customer import, one malformed record may be an expected result rather than a reason to fail every operation. See the [asyncio task documentation](https://docs.python.org/3.14/library/asyncio-task.html).

#### 57. How do you bound asynchronous concurrency and memory?

Tags: #BoundedConcurrency #CompanyUnverified #AmazonRelevant

A semaphore can limit active operations, while a bounded queue and fixed worker count can also limit pending work. Creating a task for every item and placing a semaphore inside each one still retains all those tasks. I bound producer buffers, connections, active work, and accumulated results according to the workload.

For large input, I stream records into a bounded queue and handle results incrementally or in explicit batches. I define worker-failure propagation so a dead worker cannot leave the producer blocked forever. Backpressure is an end-to-end property, not simply the presence of one concurrency parameter.

#### 58. How do you handle cancellation and timeouts correctly?

Tags: #AsyncCancellation #CompanyUnverified #AmazonRelevant

I put cleanup in `finally` or asynchronous context managers and normally allow cancellation to propagate after cleanup. I avoid broadly swallowing it, which can interfere with structured-concurrency and timeout behavior. A timeout limits waiting according to the API's semantics; it is not proof that every external effect stopped.

For example, a server may complete a write after the caller stops waiting. I reconcile state or retry through an idempotent contract rather than assuming the operation never happened. I also distinguish connection, per-operation, and total workflow deadlines so retries cannot exceed the intended overall budget.

#### 59. How do you offload blocking work from an async application?

Tags: #AsyncOffloading #CompanyUnverified #AmazonRelevant

I identify whether the operation is blocking I/O or CPU-bound and select an appropriate thread or process execution mechanism. `asyncio.to_thread` can keep suitable blocking I/O from occupying the loop thread. It does not automatically make pure Python CPU work scale across cores on a GIL-enabled interpreter.

I bound the executor backlog and confirm library thread-safety and context requirements. Cancelling the awaiter does not reliably kill an already-running thread operation, so resource lifetime and external effects still need a policy. For process work, I include serialization and shutdown behavior rather than treating offloading as a transparent function-call replacement.

#### 60. How do queues and backpressure improve a concurrent pipeline?

Tags: #ProducerConsumer #CompanyUnverified #AmazonRelevant

A bounded queue decouples stages while limiting how far a faster producer can outrun a slower consumer. I define item ownership, completion acknowledgement, termination, and failure propagation. Queue capacity should reflect memory and acceptable waiting time rather than be an arbitrary large number.

For example, downloaders can feed a limited parsing stage without retaining every document in memory. I decide whether overloaded work should wait, be rejected, or be persisted elsewhere. A local in-memory queue is not durable across a process crash, so work requiring recovery needs an appropriate external store or replayable source.

### APIs, databases, and customer data pipelines (61–70)

#### 61. What makes a Python HTTP client production-ready?

Tags: #HTTPClients #CompanyUnverified #AmazonRelevant

I reuse an appropriately scoped client for connection pooling, configure timeouts and limits, validate status and response data, and close resources reliably. I handle authentication, network policy, TLS verification, and redacted diagnostics according to the customer's environment. Response streaming needs explicit body and connection cleanup.

I distinguish connect, read, write, pool-acquisition, and total workflow budgets where the library exposes them. A per-read timeout is not necessarily a total request deadline. See [HTTPX's timeout documentation](https://www.python-httpx.org/advanced/timeouts/) for an example of these distinctions. The same design questions apply when using another client library.

#### 62. How do you retry API calls safely?

Tags: #Retries #CompanyUnverified #AmazonRelevant

I classify failures and retry only suitable transient conditions with bounded attempts, delay, jitter, and an overall deadline. I respect provider guidance such as rate-limit responses and avoid repeating validation or authorization failures. The operation's semantics determine whether repeating it is safe.

A timeout after a remote write can mean success with a lost response. I use provider-supported idempotency or reconcile with a stable identifier before creating another effect. I make the final failure actionable and preserve enough safe context for support. A generic decorator that retries every exception can duplicate work or amplify an outage.

#### 63. How do you consume a paginated API without missing records?

Tags: #Pagination #CompanyUnverified #AmazonRelevant

I follow the provider's cursor or continuation contract, validate each page, and define termination explicitly. I keep request filters stable and understand whether the API offers a snapshot or a changing collection. Offset pagination over concurrent changes can skip or repeat records unless the contract addresses it.

I checkpoint after durable processing at the intended boundary and tolerate duplicates where necessary. I also detect repeated cursors or unexpected empty pages according to the provider's rules. Pagination alone does not guarantee exactly-once processing, so record identity, reconciliation, and replay behavior remain part of the pipeline design.

#### 64. How do you make a data import restartable and idempotent?

Tags: #RestartableImports #CompanyUnverified #AmazonRelevant

I assign stable input and record identities, preserve processing progress durably, and define the transaction boundary for each effect. Repeating completed work either returns the prior result or applies an equivalent safe update. I record rejected data separately from pending work and make partial-success semantics explicit.

The checkpoint must reflect committed effects, not merely fetched input. If effects and progress cannot be atomic, I use a replay-safe design or reconciliation to cover the gap. I test crashes before and after persistence so a restart neither silently loses records nor creates duplicate business actions.

#### 65. When do you use pandas instead of standard-library iteration?

Tags: #DataWrangling #CompanyUnverified #AmazonRelevant

I use pandas when labeled tabular operations, grouping, joins, or vectorized transformations provide a clear benefit and the workload fits the available resources. Standard-library iteration may be simpler for streaming records, small utilities, or strict dependency constraints. I choose based on data shape and operations, not the mere presence of a CSV file.

I validate column types, missing values, and business meaning before transformation. I also account for intermediate allocations and conversion costs. A concise DataFrame expression can still consume excessive memory, while chunking helps only if the required computation can be performed or combined correctly across chunks.

#### 66. How do you avoid incorrect joins when combining customer datasets?

Tags: #DataJoins #CompanyUnverified #AmazonRelevant

I define key normalization, expected cardinality, missing-key behavior, and the intended join type before combining data. Duplicate keys on both sides can multiply rows, so I validate uniqueness or explicitly support many-to-many results. I compare unmatched counts and important totals afterward.

For pandas, merge validation options can check expected cardinality, and null-key matching needs attention because it differs from ordinary SQL expectations. I do not hide missing matches by immediately filling every null with a plausible value. See the [pandas merging guide](https://pandas.pydata.org/docs/user_guide/merging.html) for validation and join behavior.

#### 67. How do you write database code safely?

Tags: #DatabaseAccess #CompanyUnverified #AmazonRelevant

I parameterize values, allowlist any dynamic query structure, and use appropriate transactions and database constraints for invariants. I manage connections and cursors reliably and verify the driver's actual transaction behavior. I keep sensitive values out of logs and avoid passing untrusted records directly into unrestricted updates.

For example, checking stock and later decrementing it needs concurrency protection at the authoritative boundary. An ORM does not automatically make every custom query safe or every read-modify-write sequence correct. I test both successful writes and rollback, conflict, and connection-failure behavior under the chosen database semantics.

#### 68. How do you validate customer records without silently changing their meaning?

Tags: #DataValidation #CompanyUnverified #AmazonRelevant

I separate parsing, normalization, schema validation, and domain validation. I agree on rules for missing values, identifiers, units, and permissible corrections with the data owner. Invalid records receive actionable error information and provenance rather than being silently discarded or converted into a success-shaped default.

For example, leading zeros may be essential in an account identifier even if the field looks numeric. I preserve raw approved evidence where useful and make transformations traceable. I also distinguish a quarantined record from an unprocessed one so operators know whether to correct input, retry a dependency, or investigate a rule.

#### 69. How do you monitor a Python data pipeline?

Tags: #PipelineObservability #CompanyUnverified #AmazonRelevant

I track input volume, processed records, rejected records, duplicates, lag, duration, and meaningful business totals. Correlation and batch identifiers connect stages without copying sensitive content into every log. I include alerts for stuck or missing work, not just exceptions thrown by running code.

A job that exits successfully may still have skipped most data. I reconcile outputs against authoritative expectations and account for legitimate filtering and delay. Each alert needs an owner and recovery instructions, including how to resume safely. Observability should support correct customer outcomes rather than merely confirm that the process remains alive.

#### 70. How do you manage temporary files and partial output in a pipeline?

Tags: #PipelineResources #CompanyUnverified #AmazonRelevant

I use scoped temporary resources, appropriate permissions, bounded disk usage, and cleanup on normal and exceptional exits. I distinguish working files from completed artifacts and publish a result only after validation. Resumable work may deliberately retain a checkpointed artifact, with an explicit lifecycle and owner.

I also consider disk exhaustion, interrupted processes, and a cleanup task racing with active work. Temporary storage is not automatically suitable for sensitive data or durable progress. Recovery should identify which files are safe to resume, which need removal, and which completed output has already been delivered.

### Testing, debugging, and performance (71–80)

#### 71. What should you test in a Python customer integration?

Tags: #TestingStrategy #CompanyUnverified #AmazonRelevant

I prioritize transformations, business invariants, permissions, dependency contracts, and the critical end-to-end workflow. Unit tests isolate clear rules, while integration tests validate database or client behavior against appropriate controlled environments. I include representative malformed data and failure paths, not only the sample that worked during development.

For an importer, important checks include duplicate input, partial failure, retries, and restart after a crash boundary. I avoid measuring confidence only through test count or line coverage. The useful question is which customer-impacting regression each check can detect and whether important real boundaries remain unverified.

#### 72. How do pytest fixtures help, and what can go wrong with their scope?

Tags: #PytestFixtures #CompanyUnverified #AmazonRelevant

Fixtures define reusable setup and cleanup with an explicit lifetime. I choose scope based on resource cost and isolation, using teardown that reliably releases what was acquired. Shared mutable state in a broad-scoped fixture can make tests order-dependent or allow one case to contaminate another.

For example, a shared service may be reasonable while each test still needs isolated records or a reset policy. Cleanup must account for setup failures and partially acquired resources. See [pytest's fixture documentation](https://docs.pytest.org/en/stable/how-to/fixtures.html) for scope and teardown behavior; convenience should not obscure the resource lifecycle being tested.

#### 73. Where should you patch a dependency in a unit test?

Tags: #Mocking #CompanyUnverified #AmazonRelevant

I patch the name where the code under test looks it up, which may differ from where the original object was defined. If a module imported a callable directly, replacing the callable in its original module may not replace the local imported binding. Dependency injection can make this boundary more explicit.

I use specifications or suitable fakes to reduce impossible mock behavior and separately validate important real contracts. A mock returning the same incorrect shape the application expects creates false confidence. See [unittest.mock's guidance on where to patch](https://docs.python.org/3.11/library/unittest.mock.html#where-to-patch).

#### 74. How do parameterized and property-based tests differ?

Tags: #TestCases #CompanyUnverified #AmazonRelevant

Parameterized tests run explicit examples through the same behavior, making known edge cases and expected outcomes readable. Property-based testing generates inputs to explore general invariants under a defined strategy. The two approaches complement each other rather than one replacing all hand-chosen examples.

For batching, a property might assert that flattening the output recreates the input while no batch exceeds the bound. I still test specific failures such as an invalid size. Generated tests need meaningful properties and realistic input constraints; a tautology or a property that reproduces the implementation is weak evidence.

#### 75. How do you test asynchronous and time-dependent code deterministically?

Tags: #AsyncTesting #CompanyUnverified #AmazonRelevant

I coordinate work through observable events, controlled dependencies, and explicit completion rather than relying on long arbitrary sleeps. I inject clocks or delay behavior when timing policy is the subject of the test. I ensure pending tasks are awaited or cleaned up so failures do not leak into later tests.

I test cancellation and exceptions as well as successful concurrency. A timeout assertion should verify the intended semantic boundary without depending on precise scheduler timing on a busy machine. For external I/O, I combine controlled failure tests with appropriate real-client integration checks instead of assuming mocks verify the network stack.

#### 76. How do you design useful Python logging?

Tags: #Logging #CompanyUnverified #AmazonRelevant

I use meaningful levels, consistent event context, and identifiers that connect operations across a workflow. Libraries generally obtain named loggers while application configuration owns handlers and output policy. I preserve exceptions where useful and avoid logging the same failure repeatedly at every layer.

I redact secrets and unnecessary customer content and bound log volume. Lazy argument formatting can avoid constructing some messages when the level is disabled. Logs should answer what failed, which operation was affected, and what to investigate next; a giant payload dump is often both less useful and more sensitive than a structured summary.

#### 77. How do you find a performance bottleneck before optimizing?

Tags: #Profiling #CompanyUnverified #AmazonRelevant

I define a representative workload and target, reproduce the issue, and measure where time is spent. I separate CPU execution, waiting, network transfer, database work, and allocation. Profiling or targeted tracing helps identify whether the bottleneck is an algorithm, a repeated call, or an external constraint.

I change one important factor and compare comparable runs while checking correctness and memory. Optimizing a small loop is irrelevant if most time is spent on thousands of avoidable network round trips. I also consider startup and warm-cache effects so a benchmark does not accidentally measure a different workflow from the customer's.

#### 78. How do timeit, cProfile, and tracemalloc serve different purposes?

Tags: #DiagnosticTools #CompanyUnverified #AmazonRelevant

`timeit` helps measure small code fragments under controlled repetition. `cProfile` attributes execution time and call activity within its profiling model. `tracemalloc` tracks Python-managed allocations to help investigate where memory is allocated and retained. None is a complete view of all operating-system and native-library behavior.

I choose based on the suspected problem and account for tool overhead. A microbenchmark does not automatically predict end-to-end performance, and an allocation snapshot does not identify every native leak. I compare evidence with process metrics and realistic requests instead of treating one tool's output as the entire diagnosis.

#### 79. When can vectorization or native libraries improve performance?

Tags: #Vectorization #CompanyUnverified #AmazonRelevant

Moving suitable work into efficient array or native operations can reduce Python-level iteration overhead. The benefit depends on data representation, operation support, conversion cost, and memory behavior. Some native computation can also release the GIL, which changes how threads perform for that workload.

I verify that the transformation preserves types, missing-value semantics, precision, and ordering. A nominally vectorized expression can still allocate large intermediates or execute slow object-level operations. I benchmark realistic sizes and compare a simpler algorithm first; a better asymptotic approach may matter more than replacing syntax around an inefficient computation.

#### 80. How do you investigate steadily increasing memory use?

Tags: #MemoryDebugging #CompanyUnverified #AmazonRelevant

I reproduce growth over repeated operations and distinguish stable caching, temporary peaks, and genuinely unbounded retention. I inspect object counts, allocation snapshots, task lifecycles, and process memory under comparable conditions. Likely causes include retained results, exception tracebacks, listeners, global caches, and unclosed resources.

I bound or release the responsible ownership path and verify that continued workload stabilizes. Calling garbage collection repeatedly is not a substitute for removing reachable references. I also investigate native allocations or allocator behavior when Python-level evidence does not explain the process footprint, especially in data and numerical libraries.

### Packaging, configuration, security, and delivery (81–90)

#### 81. How do modules, packages, and imports affect application structure?

Tags: #Imports #CompanyUnverified #AmazonRelevant

Modules provide namespaces and are loaded through Python's import machinery; packages organize importable code, with regular and namespace-package forms. I keep imports understandable and avoid surprising work such as network calls during module import. Circular dependencies often signal that responsibilities or shared interfaces need reconsideration.

I use an appropriate entry point for executable behavior and test the installed package rather than depending solely on the current working directory. Naming a local file after a standard-library or dependency module can cause confusing shadowing. Packaging structure should make execution reproducible on the customer's system, not only inside one IDE session.

#### 82. Why isolate a project's Python environment?

Tags: #VirtualEnvironments #DellTechnologies #CandidateReportedAdapted

**Attribution:** Adapted from the environment-isolation prompt in the [Dell Technologies Python Developer candidate report][dell-report]. The operational limitations below are additional guidance.

A virtual environment provides a project-specific interpreter environment and package installation location, reducing conflicts between application dependencies. I create it from an intended interpreter and invoke installation through that interpreter, such as `python -m pip`, to reduce ambiguity about which environment receives packages.

Isolation does not lock versions, reproduce operating-system libraries, or provide a security sandbox. I still declare runtime support, capture dependency resolution, and verify the target platform. Copying an existing environment directory between machines is not a general deployment strategy because paths, native dependencies, and interpreter compatibility can differ.

#### 83. What belongs in pyproject.toml?

Tags: #ProjectMetadata #CompanyUnverified #AmazonRelevant

Depending on the project and tooling, `pyproject.toml` declares build-system requirements, project metadata, supported Python versions, dependencies, entry points, and tool configuration. I keep ownership of those values clear and avoid contradictory version or dependency declarations scattered across files.

I distinguish metadata describing a distributable package from the fully resolved environment needed to run an application. A clean install and build in a fresh environment verifies that undeclared local dependencies are not required. See the [Python Packaging User Guide](https://packaging.python.org/en/latest/tutorials/packaging-projects/) for the relationship between project metadata, building, and distribution.

#### 84. How do you make dependency installation reproducible?

Tags: #DependencyManagement #CompanyUnverified #AmazonRelevant

I record the supported interpreter and platform, resolve transitive dependencies through the selected workflow, and use an appropriate lock or fully pinned installation specification. Integrity checking and approved package sources add assurance about artifacts. I verify the result in a clean target-like environment.

Pinning only direct dependencies leaves room for transitive changes. A snapshot of installed packages can also include unrelated local tools and may not express intended cross-platform resolution. I distinguish library compatibility ranges from application deployment constraints. See [pip's repeatable-install guidance](https://pip.pypa.io/en/stable/topics/repeatable-installs/) for pinning, hashes, and artifact-oriented approaches.

#### 85. How do wheels and source distributions differ operationally?

Tags: #DistributionFormats #CompanyUnverified #AmazonRelevant

A wheel is a built distribution format that can often be installed without rebuilding the project. A source distribution supplies source material that may require a build step and its declared build dependencies. Compatibility depends on the artifact, including interpreter, ABI, platform, and native-library assumptions when applicable.

In a restricted customer environment, unavailable compilers or network access can make source builds impractical. I prepare approved compatible artifacts and verify installation and runtime behavior on the target. A successful install alone does not prove all native shared libraries or runtime capabilities are present.

#### 86. What makes a Python command-line tool usable?

Tags: #CLI #CompanyUnverified #AmazonRelevant

I provide clear arguments, help, input validation, meaningful exit status, and predictable output. I distinguish machine-readable results on standard output from diagnostics on standard error where appropriate. Consequential actions need explicit scope, and a dry-run option can help users review intended effects when the operation supports it.

I define configuration precedence and avoid prompting unexpectedly inside unattended automation. I test invalid arguments, missing files, unavailable dependencies, and interrupted execution. A script another engineer can run should explain what it needs and how to recover from failure instead of depending on undocumented working-directory assumptions.

#### 87. How do you handle configuration and secrets?

Tags: #ConfigurationSecurity #CompanyUnverified #AmazonRelevant

I load configuration from approved sources, validate it before starting important work, and keep secrets out of code, output, and committed fixtures. I define precedence between command-line flags, environment, and files and avoid logging a complete configuration object that includes credentials.

Environment values are strings and need explicit parsing; for example, `bool('false')` is true because the string is nonempty. Secret storage also requires scoped access, rotation, and revocation. I make startup failures actionable without exposing values and include configuration ownership in the customer's operational handoff.

#### 88. How do you invoke a subprocess safely?

Tags: #Subprocesses #CompanyUnverified #AmazonRelevant

I use an argument sequence without a shell when the command does not require shell features, validate allowable operations, and set an appropriate timeout and working context. I check exit status and control captured output so errors do not disappear or consume unbounded memory. Credentials need careful handling in arguments and logs.

Avoiding a shell prevents many shell-injection risks but does not stop an external program from interpreting dangerous options or inputs. I constrain the executable and its contract too. Platform-specific behavior matters, so I verify the [subprocess documentation](https://docs.python.org/3.11/library/subprocess.html) rather than assuming identical process semantics everywhere.

#### 89. How do you deliver Python into an offline or restricted environment?

Tags: #RestrictedDeployment #CompanyUnverified #AmazonRelevant

I identify the interpreter, operating system, architecture, approved package sources, certificates, network routes, and native dependencies. I prepare approved artifacts and verify a clean installation under the actual restrictions. I include updates, diagnostics, and recovery rather than assuming deployment ends once files are copied.

If an external API or model is unavailable from that environment, I adjust the design or scope with the customer. I do not disable TLS verification or bypass network policy as a routine fix. A thin end-to-end validation early in the engagement exposes constraints before the team depends on an unsupported environment assumption.

#### 90. How do you make a Python service operationally maintainable?

Tags: #ProductionOwnership #CompanyUnverified #AmazonRelevant

I provide explicit configuration, resource limits, meaningful health and business indicators, graceful shutdown, and tested recovery behavior. I document deployment, access, dependencies, known limitations, and support ownership. Observability should connect a user-impacting failure to a diagnosable operation without exposing sensitive content.

Before handoff, another engineer should be able to install the intended artifact, perform a normal task, and recover from a representative failure. I distinguish a pilot's bounded assumptions from broader production guarantees. Readable code matters, but maintainability also includes the customer's ability to operate and update the system after the original author leaves.

### Practical coding and FDE scenarios (91–100)

The snippets below are independent examples using the standard library and Python 3.11+. They illustrate stated contracts rather than complete production applications. Validate customer-specific schemas, permissions, limits, and recovery requirements before adapting them.

#### 91. Implement stable deduplication of records by a chosen key.

Tags: #DeduplicationExercise #CompanyUnverified #AmazonRelevant

I define whether first or latest wins and what constitutes record identity. The example yields the first record for each hashable key without mutating the records. Expected processing time is linear in input count under ordinary hash-table assumptions, with memory proportional to distinct keys.

I test empty input, repeated keys, and records with different payloads but the same identity. A generator avoids an output list but not the growing seen set. Missing or unhashable keys are errors under this small contract; a real import needs an agreed rejection policy and possibly durable deduplication across restarts.

```python
def unique_by(records, key):
    seen = set()
    for record in records:
        identity = key(record)
        if identity not in seen:
            seen.add(identity)
            yield record
```

#### 92. Implement batches for a single-pass iterable.

Tags: #BatchingExercise #CompanyUnverified #AmazonRelevant

I create one iterator and consume at most `size` items for each tuple. The final batch may be shorter, and nonpositive sizes are rejected. The batching layer holds O(size) items and processes each input once, excluding work done by the producer or consumer.

Because this function is a generator, its validation runs when iteration starts, not merely when the generator object is created. I test that distinction, empty input, exact multiples, and a short final batch. The implementation supports Python 3.11 without relying on newer batching conveniences, and it does not repeatedly restart the source iterable.

```python
from itertools import islice


def batches(items, size):
    if size <= 0:
        raise ValueError("size must be positive")
    iterator = iter(items)
    while True:
        batch = tuple(islice(iterator, size))
        if not batch:
            return
        yield batch
```

#### 93. Read JSON Lines while reporting the failing line.

Tags: #JSONLinesExercise #CompanyUnverified #AmazonRelevant

I accept an already-open text stream so the caller owns resource lifetime and encoding. The example skips blank lines, requires a JSON object per nonblank line, and adds line context without including the potentially sensitive payload in the error. It fails fast rather than quietly dropping invalid records.

I test malformed JSON, scalar values, blank lines, and an empty stream. Production code also needs schema validation, record-size limits, and an explicit policy for nonstandard numeric values if strict JSON is required. Streaming is bounded by the current line and decoded object, not an unconditional constant-memory guarantee for arbitrarily large records.

```python
import json


def json_objects(lines):
    for number, line in enumerate(lines, start=1):
        if not line.strip():
            continue
        try:
            value = json.loads(line)
        except json.JSONDecodeError as exc:
            raise ValueError(f"invalid JSON on line {number}") from exc
        if not isinstance(value, dict):
            raise ValueError(f"expected object on line {number}")
        yield value
```

#### 94. Count frequent error codes in an incoming record stream.

Tags: #AggregationExercise #CompanyUnverified #AmazonRelevant

I clarify whether missing codes are invalid, should be excluded, or need their own category. Under this example's contract, every record has a hashable `code`, and `limit` is a nonnegative integer. `Counter` aggregates one pass, and `most_common` returns the selected counts with its documented tie behavior.

Counting takes expected O(n) time and O(u) state for u distinct codes; selecting the output has additional cost depending on the number requested. This is not bounded memory for unbounded distinct values. I test ties, zero limit, duplicates, and missing fields and define a different aggregation strategy if cardinality can exceed the budget.

```python
from collections import Counter


def frequent_codes(records, limit):
    if limit < 0:
        raise ValueError("limit must be nonnegative")
    counts = Counter(record["code"] for record in records)
    return counts.most_common(limit)
```

#### 95. Parse a port from configuration without confusing missing and invalid input.

Tags: #ConfigurationExercise #CompanyUnverified #AmazonRelevant

The input contract is a string or `None`. Missing input uses a documented default, while an empty, malformed, or out-of-range value is rejected. I convert explicitly and validate the resulting range. The example uses a generic error message so callers do not need to log raw configuration values.

I test zero, boundaries, whitespace, valid digits, empty text, and missing input. Other fields need their own semantics rather than sharing one generic truthiness conversion. For boolean configuration, I would recognize explicit accepted tokens and reject unknown ones instead of using `bool` on a nonempty string.

```python
def parse_port(raw: str | None) -> int:
    if raw is None:
        return 8080
    try:
        port = int(raw)
    except ValueError as exc:
        raise ValueError("port must be an integer") from exc
    if not 1 <= port <= 65535:
        raise ValueError("port must be between 1 and 65535")
    return port
```

#### 96. Implement a context manager that closes a text resource after failure.

Tags: #ResourceExercise #CompanyUnverified #AmazonRelevant

The example demonstrates acquisition, yielding the resource, and unconditional cleanup. For a plain file, the built-in `open` context manager is normally the simpler choice; the pattern becomes useful when a custom resource needs a similarly explicit lifecycle. Acquisition failure propagates before entering the managed body.

I test both successful use and an exception inside the body and verify that the resource is closed in either case. I do not suppress the body's exception accidentally. More complicated acquisition involving several resources needs cleanup for partial setup too, potentially through `ExitStack` or another appropriate composition mechanism.

```python
from contextlib import contextmanager


@contextmanager
def managed_text(path):
    stream = open(path, encoding="utf-8")
    try:
        yield stream
    finally:
        stream.close()
```

#### 97. Design a timing decorator without changing failure behavior.

Tags: #DecoratorExercise #CompanyUnverified #AmazonRelevant

I measure elapsed duration with a monotonic performance clock, preserve wrapped metadata, and return the original result unchanged. Cleanup-style timing belongs in `finally` if failed calls must also be measured. I decide whether timing-recording failure may affect the operation rather than letting a diagnostic callback unexpectedly mask the original error.

An async wrapper must await the original operation before recording completion, while a generator wrapper needs a policy for iteration duration rather than creation time alone. I test result preservation, exception propagation, metadata, and measurement failures. The interview goal is explaining these semantic boundaries, not merely surrounding a call with two timestamps.

#### 98. Join orders to customers while rejecting duplicate customer identifiers.

Tags: #JoinExercise #CompanyUnverified #AmazonRelevant

I build an index on the smaller customer side and explicitly reject duplicate identities instead of silently overwriting them. This example performs a left-style join, yielding each order with its matching customer or `None`. Identifiers must be present and hashable; missing references are preserved for the caller to handle.

Expected processing time is O(customers + orders), and index space is O(customers), excluding retained output. I test duplicates, unmatched orders, and empty collections. Real customer matching also needs normalization, tenant scoping, and a deliberate null-key policy; matching a raw identifier does not establish authorization or semantic equivalence between systems.

```python
def join_orders(orders, customers):
    index = {}
    for customer in customers:
        identity = customer["id"]
        if identity in index:
            raise ValueError("duplicate customer identifier")
        index[identity] = customer
    for order in orders:
        yield order, index.get(order["customer_id"])
```

#### 99. Implement bounded async workers without creating one task per input item.

Tags: #AsyncWorkerExercise #CompanyUnverified #AmazonRelevant

I use a bounded queue, one producer task, and a fixed worker count under `TaskGroup`. The operation must be an async callable, and the ordinary iterable should not block the event loop while producing items. Worker failure propagates through the group and cancels sibling work instead of leaving a producer waiting indefinitely.

The example returns results in input order and deliberately retains all results, so total result memory remains O(n). It bounds pending queue entries and active worker tasks, not the whole output footprint. I test worker limits, ordering, empty input, invalid options, operation failure, and cancellation. Production code also needs deadlines and safe external-effect semantics.

```python
import asyncio


async def bounded_map(items, operation, *, workers=4, buffer=8):
    if workers <= 0 or buffer <= 0:
        raise ValueError("workers and buffer must be positive")
    queue = asyncio.Queue(maxsize=buffer)
    stop = object()
    results = []

    async def produce():
        for index, item in enumerate(items):
            await queue.put((index, item))
        for _ in range(workers):
            await queue.put(stop)

    async def consume():
        while True:
            entry = await queue.get()
            try:
                if entry is stop:
                    return
                index, item = entry
                result = await operation(item)
                results.append((index, result))
            finally:
                queue.task_done()

    async with asyncio.TaskGroup() as group:
        for _ in range(workers):
            group.create_task(consume())
        group.create_task(produce())
    results.sort(key=lambda pair: pair[0])
    return [result for _, result in results]
```

#### 100. How would you deliver a Python integration pilot in one week?

Tags: #FDEPilot #CompanyUnverified #AmazonRelevant

I agree on one valuable workflow, an acceptance threshold, representative input, and the intended deployment environment. I validate approved access and the riskiest integration assumption immediately, then build a thin end-to-end path. I prioritize clear errors, safe retries, data validation, and basic observation over secondary features.

I reserve time for real-user feedback, target-environment installation, and a recoverable handoff. Tests cover critical transformation and failure boundaries, while documentation identifies limitations and ownership. At the end, I compare evidence with the pilot goal and state what must change before broader production use; a script that only works on my laptop is not a completed customer outcome.

## Practice and review checklist

Practice ten questions at a time. Explain the behavior before writing code, state your runtime assumptions, and test a normal case plus a meaningful failure. For experience-based follow-ups, use real examples and distinguish your contribution from the team's.

Before handing over a Python solution, check:

1. Are input schema, missing values, duplicate keys, and output semantics explicit?
2. Are mutation, resource ownership, and cleanup understandable?
3. Are memory, concurrency, queue, connection, and retry limits actually bounded where claimed?
4. Can a timeout, crash, duplicate, or restart produce an incorrect business effect?
5. Do logs and diagnostics protect secrets and customer data?
6. Can another engineer install and run the intended artifact in the target environment?
7. Do tests cover meaningful contracts and failure boundaries, including real integrations where needed?
8. Is there an owner and procedure for updates, support, reconciliation, and recovery?

Continue with [full-stack engineering](../04-full-stack-engineering/README.md), [backend APIs](../08-backend-apis/README.md), [databases](../09-databases/README.md), [customer deployment](../17-customer-deployment/README.md), and [production troubleshooting](../18-production-troubleshooting/README.md).

[dell-report]: https://www.glassdoor.com/Interview/What-is-Python-and-why-is-it-popular-in-data-processing-and-analysis-Explain-the-differences-between-Python-2-and-Python-QTN_6681000.htm
[yield-report]: https://jonolson.substack.com/p/yield-the-most-asked-interview-question
[gil-report]: https://www.glassdoor.sg/Interview/They-asked-me-about-the-Global-Interpreter-Lock-GIL-in-Python-QTN_8641427.htm
