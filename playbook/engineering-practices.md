# Engineering Practices: What Makes an Engineer

This is a field guide for new engineers. It covers the behaviors, habits, technical practices, and mindset that separate professional software engineering from just writing code that happens to work.

None of this is about gatekeeping. It is about building the habits that let you ship software other people can trust, maintain, and run in production for years.

---

## The Core Difference

A vibe coder writes code until it appears to work. An engineer writes code they can defend — to reviewers, to ops, to the person maintaining it at 2 AM eighteen months from now.

The difference is not talent. It is discipline, accountability, and a bias toward evidence over intuition. An engineer thinks about the system, not just the feature. They think about what happens next week, not just what happens when they click the button right now.

---

## Mindset and Ownership

### Understand the Problem Before You Touch the Keyboard

Read the requirements. Read them again. Ask questions before you start coding. The most expensive bugs are the ones where you built the wrong thing correctly.

Senior engineers spend more time reading existing code and understanding constraints than they spend writing new code. That ratio is not a sign of slowness. It is a sign of experience.

### You Own the Outcome, Not Just the Code

Your job is not done when your code compiles. You own the PR, the review feedback, the test results, the deployment behavior, and the production impact. If your change breaks something, that is your problem to investigate — even if CI "passed."

Ownership means:

- You tested it, not just hoped it works
- You read the diff before pushing
- You responded to every review comment, not just the easy ones
- You checked production after deploy, not just walked away
- You updated the documentation when you changed the behavior

### Evidence Beats Confidence

"It should work" is not validation. "I ran the tests and here is the output" is.

Record what you checked, what you did not check, and what remains risky. If you skipped a validation step, say so directly. Reviewers and your future self will thank you.

Bad:
```
Tested locally, looks good.
```

Good:
```
Ran unit tests (all 47 pass). Ran integration tests against local Postgres.
Manually verified the new endpoint returns 404 for missing resources.
Did not test against staging — no staging environment available for this service yet.
Residual risk: error message format is only covered by unit test mocks.
```

### Know When Not to Build

Sometimes the best engineering decision is to not write code at all.

Before you start building, ask:

- Does this already exist in the codebase, in the framework, or in the standard library?
- Can we use an existing service, library, or vendor solution instead of building our own?
- Can we simplify the requirements to avoid needing this entirely?
- Is this feature worth the ongoing maintenance cost, or is it a nice-to-have that will become abandoned code in six months?

Every line of code you write is a line that needs to be tested, reviewed, maintained, debugged, and eventually migrated or deleted. Less code that solves the problem is almost always better than more code that also solves the problem.

The engineer who says "we do not need to build this" and explains why is contributing more than the one who builds an elegant solution to a problem no one actually has.

---

## Code Design and Structure

### Modularity and Separation of Concerns

A function should do one thing. A module should own one responsibility. A service should represent one bounded domain.

When everything lives in one file or one giant class, every change risks breaking something unrelated. Modular code lets you change, test, and reason about one piece without understanding the whole system.

Practical modularity:

- If a function is longer than your screen, it is probably doing too much — extract the logical steps into well-named helper functions
- If a class has fifteen methods that touch different parts of the system, it is doing too many jobs — split it by responsibility
- If you need to change three unrelated files to add a simple feature, your boundaries are in the wrong place
- If two modules need to know about each other's internals to work, they are too tightly coupled

The goal is not small files for the sake of small files. The goal is that a developer can open one module, understand its contract, and change its internals without fear.

### Don't Repeat Yourself — But Don't Over-Abstract Either

DRY is one of the most cited and most misapplied principles in software. The idea is sound: if the same logic exists in three places, a bug fix in one place leaves two places still broken.

When you see the same block of logic duplicated, extract it into a shared function, module, or configuration. But apply DRY to *logic*, not to *coincidence*. Two functions that happen to look similar today but serve different purposes and will evolve independently should stay separate. Premature abstraction creates coupling that is worse than the duplication it removed.

The test: if the two pieces of code would need to change for the *same reason*, unify them. If they would change for *different reasons*, let them be.

### Coupling and Cohesion

Good modules have high cohesion (everything inside them is closely related) and low coupling (they depend on as little outside themselves as possible).

Signs of problematic coupling:

- Changing one module forces changes in five others
- You cannot test a component without standing up half the system
- A "shared utilities" module that everything depends on and that changes constantly
- Circular dependencies between packages

Reduce coupling by depending on interfaces rather than implementations, passing dependencies in rather than reaching out for them, and keeping module APIs narrow.

### API and Interface Design

Every function signature, every REST endpoint, every class constructor is an API. Treat them that way.

- Make interfaces hard to misuse — if an argument order is easy to confuse, use named parameters or a config object
- Be consistent — if one endpoint returns `created_at` and another returns `createdAt`, someone will have a bad day
- Design for the caller, not the implementation — hide internal details behind a clean contract
- Think about backwards compatibility before you ship — changing a public interface after people depend on it is expensive

### Manage Your Dependencies

Every dependency you add is code you did not write, do not control, and must maintain compatibility with.

Before adding a library:

- Does it solve a problem substantial enough to justify the dependency?
- Is it actively maintained? Check commit history, issue tracker, release cadence
- What is its transitive dependency tree? Adding one package that brings in 200 others is a different decision
- Can you accomplish this with what the language or framework already provides?

Pin your dependency versions. Use lock files. Run dependency audits for known vulnerabilities. Update dependencies deliberately, not reactively when something breaks.

### Data Modeling

How you structure your data matters more than how you structure your code. Bad code can be refactored. Bad data models live forever because migrating production data is expensive and risky.

Think about:

- **Normalization vs. denormalization** — normalize by default to avoid data inconsistency; denormalize deliberately for read performance when you have measured the need
- **Indexes** — every query that runs in production should be backed by an appropriate index. An unindexed query on a growing table is a time bomb
- **Data types** — use the right type for the job. Do not store dates as strings. Do not store money as floating point. Do not store booleans as integers unless your database requires it
- **Nullability** — decide whether each field can be null and what null means. Ambiguous nulls (does null mean "unknown" or "not applicable" or "default"?) cause bugs that are hard to trace
- **Relationships and constraints** — use foreign keys, unique constraints, and check constraints to enforce data integrity at the database level, not just in application code. The application is not the only thing that touches the data
- **Schema evolution** — design with change in mind. Will you need to add fields? Split a table? Change a relationship? Flexible does not mean schemaless — it means thinking ahead

If the data model is wrong, no amount of clever application code will fix it cleanly.

### Never Trust New Data

Every new data source — a partner feed, a third-party API, a CSV from another team, a user upload, a migrated legacy table — will lie to you. Not maliciously, but reliably. The documentation will be wrong, the schema will be inconsistent, and the edge cases will be worse than anything you imagined.

**Before you integrate a new data source, verify it:**

- **Profile it.** Before writing any code, look at the actual data. Check row counts, null rates, distinct value counts, min/max ranges, and value distributions. Do not trust the spec — trust the data. A field documented as "never null" will have nulls. A field documented as "unique identifier" will have duplicates
- **Check for encoding and format consistency.** Does every row use the same date format? The same timezone? The same character encoding? Are phone numbers stored with or without country codes — or both? Is "New York" also "new york," "NY," "N.Y.," and "Nueva York"?
- **Validate against the contract.** If the provider says the file has 12 columns, check whether some rows have 11 or 13. If the API says it returns ISO 8601 dates, verify that it does not sometimes return epoch timestamps or locale-formatted strings
- **Look at the edges.** Check the first rows, the last rows, and random samples from the middle. Look at the shortest and longest string values. Look at the oldest and newest timestamps. The interesting bugs live at the extremes
- **Check for duplicates and conflicts.** Does the same entity appear more than once? Do two records that should match have contradictory values? Is the primary key actually unique?
- **Understand the update cadence and delivery guarantees.** Is this a one-time load or an ongoing feed? If ongoing, can records be updated or deleted in the source after you have received them? Can deliveries arrive out of order, be duplicated, or be missing?

**Common data risks to protect against:**

- **Schema drift.** The upstream source adds, removes, or renames fields without warning. Validate the schema on ingest. Fail loudly when it changes rather than silently ingesting garbage
- **Volume spikes.** A feed that normally sends 10,000 rows sends 10 million. Or zero. Both are problems. Set upper and lower bounds on expected volumes and alert when they are violated
- **Poison records.** A single malformed record should not take down your entire pipeline. Process records independently where possible. Quarantine failures rather than aborting the whole batch
- **Semantic drift.** The field names stay the same but the meaning changes. A `status` field that used to contain "active" and "inactive" now also contains "pending_review." Your code handles two values. The third one falls through to a default case that may not be correct
- **Late-arriving and out-of-order data.** A record timestamped Monday arrives on Wednesday. Your daily aggregation for Monday already ran. Do you reprocess? Ignore it? Append it? Decide this before it happens
- **PII and compliance surprises.** A data source you thought contained only anonymized data turns out to include email addresses, IP addresses, or government IDs. Audit new sources for personally identifiable information before you store or replicate them. Getting this wrong has legal consequences

Build an ingestion layer that validates, normalizes, and quarantines before data enters your system. Treating external data as clean is how you corrupt your own database.

---

## Coding Standards and Style

### Follow a Style Guide

Every language has community conventions: PEP 8 for Python, the Airbnb or Standard style for JavaScript, `gofmt` for Go, the Kotlin coding conventions, Google's style guides for C++ and Java. Your team may have its own.

Style guides exist to eliminate pointless debates and make code visually consistent across contributors. Follow the one your team uses. If your team does not have one, adopt a well-known community standard.

The specific style matters less than consistency. A codebase that mixes tabs and spaces, camelCase and snake_case, 80-column and 200-column lines is harder to read than one that picks any consistent convention.

### Automate Style Enforcement

Do not rely on humans to enforce formatting rules during code review. That is tedious, error-prone, and a waste of review bandwidth.

Use automated tools:

- **Linters** catch style violations, suspicious patterns, and potential bugs: ESLint, Pylint, Flake8, RuboCop, Clippy, golangci-lint
- **Formatters** apply consistent formatting automatically: Prettier, Black, `gofmt`, `rustfmt`, clang-format
- **Type checkers** catch type errors before runtime: mypy, TypeScript, Flow

Configure these in your project. Run them in CI so violations block merges. Run them locally on save or pre-commit so you get feedback immediately, not after you have pushed.

Formatter on save means you never think about formatting again. That is the goal.

### Comments and Documentation

Comments should explain *why*, not *what*. The code already says what it does. If the code is so unclear that it needs a comment explaining what it does, the code should be rewritten to be clearer.

Good comments:

```python
# We retry up to 3 times because the payment gateway occasionally returns
# transient 503s during their maintenance windows (confirmed with their
# support team, ticket #4521).
for attempt in range(3):
    ...
```

Bad comments:

```python
# Loop through the list
for item in items:
    # Check if the item is valid
    if item.is_valid():
        # Add it to results
        results.append(item)
```

For public APIs, libraries, and shared modules, write proper documentation: what it does, what the parameters mean, what it returns, what exceptions it raises, and an example of typical usage. Use your language's standard documentation format — docstrings in Python, JSDoc in JavaScript, Javadoc in Java, XML doc comments in C#.

Write READMEs for your repositories and services. A new developer should be able to read the README and know: what this thing is, how to set it up locally, how to run the tests, and how to deploy it.

### Naming Is Design

Variable names, function names, class names, file names — these are the primary documentation of your code. A function called `processData()` tells the next reader nothing. A function called `validateAndNormalizeEmailAddress()` tells them exactly what it does.

Spend time on names. Rename things when you find better words. This is not cosmetic. Unclear names cause real bugs because people assume wrong behavior.

Conventions to follow:

- Boolean variables should read like a question: `isValid`, `hasPermission`, `shouldRetry`
- Functions that return something should describe what: `getUser`, `calculateTotal`, `findMatchingOrders`
- Functions that perform side effects should describe the action: `sendNotification`, `deleteExpiredSessions`
- Avoid abbreviations that are not universally understood — `ctx` is fine, `cstmrAcctMgr` is not

---

## Error Handling and Resilience

### Error Handling Is Not Optional

Happy path code is the easy part. What happens when the database is down? When the input is malformed? When the upstream service returns a 500? When the disk is full?

Engineers think about failure modes. They do not add `catch (e) { /* ignore */ }` and move on. They decide, deliberately, what each error means and how the system should respond.

At minimum:

- Validate inputs at system boundaries — never trust data from outside your service
- Fail fast and fail loud — do not swallow errors silently
- Return useful error messages that help the caller fix the problem
- Log enough context to diagnose issues without logging sensitive data
- Distinguish between retryable and permanent failures

### Defensive Programming at Boundaries

Trust nothing that crosses a system boundary: user input, API responses, file contents, environment variables, message queue payloads. Validate and sanitize at the edges so the core logic can assume clean data.

This does not mean adding null checks on every line inside your own code. Validate at the perimeter; trust your own internals.

### Design for Partial Failure

In distributed systems, things fail independently. A good engineer asks: what happens if the cache is down but the database is up? What if one of three downstream services is slow? What if a message is delivered twice?

Common resilience patterns:

- **Timeouts** — never make an unbounded network call; always set a deadline
- **Retries with backoff** — transient failures often resolve on retry, but retry floods make outages worse
- **Circuit breakers** — stop calling a service that is clearly down instead of piling on
- **Graceful degradation** — serve stale data, disable a non-critical feature, show a helpful error instead of crashing

### Concurrency and Shared State

Concurrency bugs are among the hardest to find because they depend on timing. They do not reproduce reliably, they do not show up in unit tests, and they appear under load at the worst possible moment.

Core principles:

- **Avoid shared mutable state.** If two threads, processes, or requests can read and write the same data, you have a race condition waiting to happen. Prefer immutable data, message passing, or clearly owned mutable state
- **Understand your concurrency model.** Threads, async/await, actors, goroutines, and event loops all have different guarantees. Know which one your language and framework use and what the rules are
- **Use locks and synchronization correctly.** If you do need shared state, protect it. But know that locks introduce their own problems: deadlocks, contention, and priority inversion
- **Design for idempotency.** In distributed systems, messages get delivered more than once, requests get retried, and jobs run twice. If processing the same operation twice produces a different result than processing it once, you have a data integrity problem. Design operations so that repeating them is safe
- **Watch for time-of-check to time-of-use (TOCTOU) bugs.** Checking a condition and then acting on it is not safe if something can change the condition in between. This applies to file systems, databases, and API calls

If you think you do not have concurrency issues, check whether your system handles simultaneous requests from multiple users. If it does, you have concurrency.

---

## Testing Strategy

### The Testing Pyramid

Not all tests are equal. A healthy test suite has:

- **Many unit tests** — fast, isolated, focused on individual functions and logic branches. These are your first line of defense and your fastest feedback loop
- **Fewer integration tests** — verify that modules, databases, APIs, and services work together correctly. Slower but catch a class of bugs unit tests miss entirely
- **Few end-to-end tests** — simulate real user workflows through the full stack. Expensive to write and maintain, brittle, but validate that the whole system hangs together

A codebase with 500 unit tests that run in seconds, 50 integration tests that run in a minute, and 10 critical-path e2e tests is in good shape. A codebase with zero unit tests and 200 flaky Selenium tests is in pain.

### What to Test

Focus testing effort where it matters:

- Business logic and calculations — the rules that determine what your system does
- Edge cases and boundary conditions — empty lists, null inputs, max values, off-by-one scenarios
- Error paths — verify the system behaves correctly when things go wrong
- Integration points — verify your code talks to databases, APIs, and queues correctly
- Regressions — when you fix a bug, add a test that would have caught it

Do not waste time testing trivial getters/setters, framework boilerplate, or generated code. Test the logic you wrote and the contracts you depend on.

### Test Isolation

Each test should be independent. It should not depend on the order it runs in, the state left behind by a previous test, or shared mutable data.

Tests that pass locally but fail in CI — or pass individually but fail when run together — almost always have an isolation problem.

### Write Tests You Trust

Flaky tests erode confidence. If a test fails intermittently and the team's reaction is "oh, just re-run it," that test is worse than no test — it trains people to ignore failures.

Fix or delete flaky tests. Do not let them linger.

---

## Debugging

Debugging is a skill, not a talent. Good debuggers are not luckier — they are more systematic.

### Reproduce First

Before you try to fix a bug, reproduce it reliably. If you cannot reproduce it, you cannot confirm your fix works. Get the exact inputs, the exact environment, and the exact sequence of steps. "It sometimes crashes" is not a reproduction — it is a starting point.

### Form a Hypothesis, Then Test It

Do not change things at random hoping the bug goes away. That is how you introduce new bugs while "fixing" the original one.

1. Observe the symptoms. Read the error message, the logs, the stack trace
2. Form a hypothesis about what is going wrong and where
3. Design an experiment that would confirm or disprove your hypothesis — add a log line, set a breakpoint, write a failing test
4. If disproved, form a new hypothesis based on what you learned
5. If confirmed, write a test that reproduces the bug, then fix it

### Narrow the Search Space

`git bisect` is criminally underused. If something worked last week and is broken now, bisect finds the exact commit that broke it in O(log n) steps. Learn it.

For logic bugs, binary search through the code: comment out half the logic, see if the bug persists, narrow from there. This is faster than reading every line trying to spot the problem.

### Read the Error Message

This sounds obvious. It is not. Engineers routinely glance at an error, assume they know what it means, and spend an hour debugging the wrong thing. Read the full message. Read the stack trace. Google the exact error string. Check whether the line number points to the actual problem or a downstream symptom.

### Debug the System, Not Your Mental Model

The most common debugging mistake is assuming you know what the code does instead of observing what it actually does. Add logging. Use a debugger. Inspect the actual values at runtime. The code is not doing what you think — that is literally why there is a bug.

---

## Version Control and Collaboration

### Commit Hygiene

Commits are a communication tool. Each commit should represent a single logical change with a clear message explaining what and why.

Good commit messages:

```
Fix order total calculation for discount codes

The discount was applied after tax instead of before,
causing overcharges of 1-3% on discounted orders.
Fixes #1234.
```

Bad commit messages:

```
fix stuff
wip
asdfasdf
```

Small, well-described commits make `git log`, `git blame`, and `git bisect` useful. Giant commits with vague messages make them useless.

### Branching and Merging

Keep branches short-lived. A branch that lives for two weeks accumulates merge conflicts and diverges from what the rest of the team is building against.

- Branch from the latest `main`
- Merge (or rebase) `main` into your branch regularly if it lives more than a day or two
- Delete branches after they merge

### Small, Reviewable Pull Requests

A 50-line PR gets reviewed carefully. A 500-line PR gets skimmed. A 2000-line PR gets approved with a prayer.

Break work into the smallest change that is independently correct and deployable. This is harder than writing one giant commit, but it results in faster reviews, easier bisection, lower merge conflict risk, and clearer git history.

If you cannot explain what your PR does in one sentence, it is probably too big.

### Read the Diff Before You Push

Look at every line of your diff before you create a PR. You will find:

- Debug logging you forgot to remove
- Commented-out code that should be deleted
- Files you did not intend to change
- Formatting changes mixed in with logic changes
- Hardcoded values that should be configuration
- Credentials or secrets that should never be committed

This takes five minutes and catches problems that waste hours in review.

### Code Review Is a Core Practice

Code review is not a chore. It is one of the fastest ways to learn a codebase, learn what good code looks like, and develop judgment about design tradeoffs.

When reviewing:

- Actually read the code — do not just look for syntax errors
- Ask yourself if you understand what the change does and why
- Check for things automated tools miss: wrong abstractions, missing edge cases, unclear intent, performance traps, security gaps
- Be specific in feedback — "this is confusing" is less useful than "I expected this function to return early when the list is empty, but it falls through to the loop"

When receiving review feedback, assume good intent. The reviewer is trying to make the code better, not criticize you personally.

---

## Technical Communication

Writing code is only part of the job. Engineers also need to communicate decisions, proposals, and knowledge clearly.

### Design Documents

For non-trivial work — anything that involves multiple components, introduces a new pattern, changes a public API, or carries meaningful risk — write a short design document *before* you write code.

A design doc does not need to be a novel. It should cover:

- **Problem** — what are we solving and why?
- **Constraints** — what are the requirements, limits, and non-negotiable properties?
- **Proposed approach** — how will you solve it? What are the key design decisions?
- **Alternatives considered** — what else did you think about and why did you reject it?
- **Risks and open questions** — what could go wrong? What are you not sure about?

The point is not bureaucracy. The point is that writing forces you to think clearly, and sharing the doc catches problems before you have built the wrong thing. A one-page doc that saves a week of rework is a good trade.

### Architecture Decision Records (ADRs)

When you make a significant technical decision — choosing a framework, picking a database, deciding on an API style, adopting a new pattern — record it. An ADR captures what was decided, why, and what the alternatives were.

Six months later, when someone asks "why did we use RabbitMQ instead of Kafka?" the answer should not be "I think Sarah knew, but she left." The answer should be a document anyone can find.

### Status Updates and Technical Writing

Be clear, be concise, lead with the conclusion. Engineers who can explain a complex problem in a paragraph are more effective than those who need thirty minutes and a whiteboard.

When reporting status, lead with what matters: what is done, what is blocked, what is at risk. Save the details for people who ask for them.

---

## Environments, Deployment, and Release

### Environment Separation

Production code should never be the first place you see your change running. A professional setup has distinct environments:

- **Local development** — your machine, fast iteration, mocked or containerized dependencies
- **CI / test** — automated build and test on every push, clean environment, no manual setup
- **Staging / pre-production** — a production-like environment where you validate behavior with real-ish data, integrations, and infrastructure before it reaches users
- **Production** — the real thing, where real users are affected by every change

Each environment should be as close to production as practically possible. "Works on my machine" is not evidence of correctness. Differences between environments — different OS versions, different database engines, missing environment variables — are a top source of deployment surprises.

Use configuration (environment variables, config files, secrets managers) to handle per-environment differences. Never hardcode connection strings, API keys, or feature behavior to a specific environment.

### Configuration Belongs Outside Code

Anything that changes between environments — database URLs, API keys, feature thresholds, log levels, external service endpoints — should be configuration, not code.

The twelve-factor app principle is clear: store config in the environment. This means:

- No credentials in source code, ever
- No `if (environment === 'production')` scattered through business logic
- Use environment variables, config files loaded at startup, or a configuration service
- Secrets should live in a secrets manager (Vault, AWS Secrets Manager, Azure Key Vault), not in `.env` files checked into the repo

### CI/CD Pipelines

Continuous integration means every push triggers an automated build and test. Continuous delivery means every change that passes CI *can* be deployed at any time. Continuous deployment means it *is* deployed automatically.

At minimum, your CI pipeline should:

- Install dependencies from a lock file (reproducible builds)
- Run linters and formatters (catch style issues before review)
- Run the full test suite (catch regressions before merge)
- Build the artifact (catch compilation or bundling errors)
- Run security scans (dependency vulnerabilities, static analysis)

CI should be fast enough that developers get feedback in minutes, not hours. If CI takes too long, people stop waiting for it and merge without it — which defeats the purpose.

### Feature Flags and Gradual Rollout

Shipping code to production does not have to mean exposing it to all users at once. Feature flags let you decouple deployment from release:

- **Deploy** the code behind a flag, turned off
- **Enable** for internal users or a small percentage of traffic
- **Ramp** gradually — 1%, 5%, 25%, 100% — while watching metrics
- **Roll back** by flipping the flag, without a code deployment

This pattern dramatically reduces the blast radius of problems. A bug that affects 1% of users for ten minutes is a very different incident than one that affects 100% of users for an hour while you scramble to deploy a fix.

Feature flags are also useful for:

- A/B testing and experimentation
- Trunk-based development — merge incomplete features to main without exposing them
- Kill switches for expensive or risky operations
- Regional or customer-specific behavior

Clean up old flags. A codebase full of stale flags with dead branches is its own kind of technical debt.

### Backwards Compatibility

When you change something that other people or systems depend on — an API, a message format, a library interface, a database schema — you need a migration path, not a breaking change.

The pattern:

1. **Add** the new way alongside the old way
2. **Migrate** consumers to the new way
3. **Remove** the old way once nothing depends on it

This takes discipline because step 3 is boring and easy to skip. But carrying both versions forever is its own cost.

For APIs, version them explicitly. For database columns, add the new column before removing the old one. For message formats, make new fields optional so old producers and consumers keep working.

The cardinal sin is deploying a breaking change to production and finding out what depends on it by watching things fail.

### Database Migrations

Schema changes are among the most dangerous things you can do in production. They can lock tables, corrupt data, or make rollback impossible.

Treat migrations as first-class code:

- Write them as versioned, sequential scripts — never modify a migration that has already been applied
- Make them backwards-compatible when possible — add columns before removing the old ones, make new columns nullable at first
- Test migrations against a copy of production-sized data, not just your empty dev database
- Have a rollback plan before you run the migration forward
- Never run DDL changes by hand in a production console

---

## Security as a Default

Security is not a feature you add at the end. It is a property of how you build things from the start.

### The Basics That Are Non-Negotiable

- **Never store passwords in plain text.** Use bcrypt, scrypt, or argon2. This is a solved problem
- **Never commit secrets to source control.** Use `.gitignore`, pre-commit hooks, and secrets scanning. If a secret is committed, rotate it immediately — deleting the commit is not enough
- **Validate and sanitize all external input.** SQL injection, XSS, command injection, and path traversal are all caused by trusting user input. Use parameterized queries, not string concatenation
- **Use HTTPS everywhere.** There is no reason not to in 2026
- **Apply the principle of least privilege.** Services, users, and API keys should have the minimum permissions they need and no more
- **Keep dependencies patched.** Known vulnerabilities in outdated packages are the easiest attack vector there is

### Authentication and Authorization

Know the difference. Authentication is *who are you*. Authorization is *what are you allowed to do*. Getting either one wrong is a security incident.

Do not build your own authentication system unless you genuinely know what you are doing. Use established libraries and providers (OAuth, OIDC, SAML). The security surface area of auth is enormous and the consequences of getting it wrong are severe.

### Think Like an Attacker

When you write an API endpoint, ask yourself: what happens if someone sends unexpected input? What if they call it a thousand times per second? What if they manipulate the request to access another user's data? What if they send a 10 GB payload?

You do not need to be a security expert. You need to be suspicious of your own assumptions.

### Protecting Customer Data

Customer data is not yours. You are a custodian. Treat every piece of personal information — names, emails, addresses, payment details, health records, usage patterns — as something that could end up in a headline if you get it wrong.

**Collect only what you need.** The safest way to protect data is to not have it. Before adding a field to a form, a column to a table, or a parameter to an API call, ask: do we actually need this to provide the service? If the answer is no, do not collect it. Data you do not store cannot be leaked.

**Classify it.** Not all data carries the same risk. A display name and a Social Security number are different categories of problem if exposed. Establish clear tiers — public, internal, confidential, restricted — and handle each accordingly. Know which regulations apply to which data: GDPR for EU personal data, HIPAA for health information, PCI DSS for payment card data, CCPA/CPRA for California consumers, and whatever applies in your market.

**Encrypt it.** Encrypt data at rest (disk, database, backups) and in transit (TLS/HTTPS). Use your cloud provider's encryption services or well-known libraries — do not roll your own cryptography. Encryption at rest means a stolen hard drive or database snapshot is not automatically a data breach.

**Control access.** Limit who and what can read customer data:

- Application code should access only the fields it needs, not `SELECT *` from a table with PII columns it will never use
- Admin tools and dashboards should mask or redact sensitive fields by default. An engineer debugging a pagination bug does not need to see real email addresses
- Database access in production should be audited and restricted. Not every developer needs a direct connection to the production customer table
- Third-party integrations and vendors should receive the minimum data they need. If the analytics service only needs event counts, do not send it user profiles

**Isolate it.** Keep PII out of places it does not belong:

- Do not log customer data. Log a request ID, not the request body that contains an address. Log a hashed or tokenized user identifier, not an email
- Do not put PII in URLs, query strings, or error messages — these end up in browser history, server logs, and monitoring dashboards
- Do not copy production data to development or staging environments without anonymizing or synthesizing it first. A developer laptop with a full copy of the customer database is an unencrypted, unmonitored, unaudited liability
- Do not store PII in caches, search indexes, or analytics pipelines unless those systems have the same access controls and retention policies as your primary data store

**Delete it when you should.** Data retention is not just a storage cost problem — it is a risk and compliance obligation. Define how long you keep different categories of data and enforce it. Users who delete their accounts should not still have their data sitting in a backup six years later. Regulations like GDPR give users the right to request deletion, and you need to be able to actually do it across every system that holds their data.

**Plan for breach.** Assume that despite your best efforts, a breach is possible. Know your incident response plan: who is notified, what authorities and regulators must be contacted, what the timeline requirements are, and how affected users will be informed. Discovering you need a breach notification plan during a breach is too late.

None of this is optional or nice-to-have. A data breach damages customer trust in ways that no feature launch can repair. Engineers who handle customer data carelessly are not just writing bad code — they are putting real people at risk.

---

## Observability and Operations

### Logging

Logs are how you understand what your system is actually doing in production. Write them for the person debugging an incident at 3 AM.

- Log at appropriate levels: DEBUG for development, INFO for normal operations, WARN for recoverable problems, ERROR for things that need attention
- Include context: request IDs, user identifiers (not PII), operation names, relevant parameters
- Use structured logging (JSON) so logs can be searched and aggregated
- Never log passwords, tokens, credit card numbers, or personal data

### Monitoring and Alerting

If you do not measure it, you do not know if it is working.

At minimum, instrument:

- Request rate, latency, and error rate for your APIs
- Queue depth and processing time for async jobs
- Resource utilization: CPU, memory, disk, connections
- Business metrics that matter: signups, orders, payments

Set up alerts for conditions that require human attention. Alert on symptoms (elevated error rate) rather than causes (high CPU), because symptoms tell you users are affected.

Alert fatigue is real. If your team ignores alerts because most of them are noise, your alerting is worse than useless — it is a liability. Tune aggressively.

### Runbooks and Incident Response

When something breaks in production, clarity matters more than cleverness.

- Write runbooks for known failure modes: what the symptoms look like, what to check, how to mitigate
- Define who gets paged and what the escalation path is
- After an incident, write a blameless post-mortem: what happened, what the impact was, what the root cause was, and what you are doing to prevent recurrence
- Track follow-up items and actually complete them

### Distributed Tracing

In a system with multiple services, a single user request may traverse five or ten services before a response is returned. When that request is slow or fails, logs from individual services do not tell you the story.

Distributed tracing (OpenTelemetry, Jaeger, Zipkin) propagates a trace ID across service boundaries so you can see the full journey of a request: which services were called, in what order, how long each took, and where the failure or bottleneck occurred.

If you operate multiple services, tracing is not optional — it is how you debug production.

---

## Performance Awareness

### Do Not Optimize Prematurely, But Do Not Be Negligent

You do not need to micro-optimize every loop. But you do need to understand the performance characteristics of what you write.

Know the difference between O(n) and O(n²). Know that an N+1 query in a loop will take down your database when the dataset grows. Know that loading a 500 MB file into memory on a 512 MB container will crash. Know that an unindexed database query that works fine with 1,000 rows will time out with 10 million.

Performance problems that are obvious in hindsight are often invisible in development because dev data sets are small.

### Measure Before You Optimize

When performance is actually a problem, measure first. Profile. Look at the data. The bottleneck is almost never where you think it is.

Optimization without measurement is guesswork that often makes the code harder to read without making it meaningfully faster.

### Caching

Caching is powerful and dangerous. It solves performance problems but introduces consistency problems.

Before adding a cache, ask:

- What happens when the cache serves stale data?
- How will the cache be invalidated?
- What happens when the cache is cold or evicted?
- Can you tolerate the inconsistency window?

"There are only two hard things in computer science: cache invalidation and naming things." This joke persists because it is true.

---

## Common Sources of Bugs

Some categories of bugs recur across every language, framework, and decade. Knowing them saves you from learning the hard way.

### Time and Timezones

Time handling is one of the most consistently underestimated sources of bugs in production software.

- **Store times in UTC.** Convert to local time only for display. A database full of local times in mixed timezones is a nightmare to query and reason about
- **Use your language's date/time library.** Do not do date math with string manipulation or raw arithmetic. Daylight saving transitions, leap years, leap seconds, and month-length differences will bite you
- **Be explicit about what a timestamp means.** Is it the time the event happened, the time it was recorded, or the time it was processed? Different systems may have different clocks
- **Beware of time comparisons.** "Today" depends on whose timezone you mean. "Last 24 hours" and "yesterday" are different things

### Floating Point and Money

Never use floating point arithmetic for money. `0.1 + 0.2 !== 0.3` in most languages. Use integer cents, a decimal type, or a money library. A rounding error of one cent per transaction adds up to real money at scale — and real audit findings.

### Character Encoding

Use UTF-8. Everywhere. In your source files, your database, your APIs, your file I/O. Most encoding bugs come from mixing encodings or assuming ASCII. If you see garbled characters, mojibake, or mysterious question marks, it is almost always an encoding mismatch.

### Off-by-One Errors

Is the range inclusive or exclusive? Is the index zero-based or one-based? Does "the last 7 days" include today? Off-by-one errors are trivial individually and relentless collectively. Be explicit about boundaries and write tests for the edges.

### Null and Missing Data

Null means different things in different contexts and languages. Decide what null represents in your system and handle it consistently. The worst bugs come from code that treats null as an empty string in one place, as false in another, and as "use the default" in a third.

Use your language's null-safety features if available: Optional in Java, Option in Rust, strict null checks in TypeScript. The compiler catching a null dereference is cheaper than a customer finding it.

---

## Working Habits

### Learn Your Tools

Know your editor, your debugger, your version control system, your build system, and your CI pipeline. You do not need to be an expert on day one, but you should be getting better at these every month.

An engineer who understands `git log`, `git bisect`, and `git rebase` solves problems faster than one who only knows commit and push. An engineer who knows how to use a debugger and set conditional breakpoints wastes less time than one who sprinkles `console.log` everywhere.

Learn your language's package manager, its standard library, its profiler, its testing framework. The upfront investment pays for itself many times over.

### Ask Questions Early

New engineers sometimes avoid asking questions because they do not want to seem incompetent. This is backwards. Asking questions early is the single best way to avoid wasting days on the wrong approach.

Good questions to ask before starting work:

- Is there existing code or a pattern I should follow?
- Are there constraints or decisions already made that I should know about?
- What does the reviewer care most about for this change?
- How will I test this? Is there existing test infrastructure I should use?
- Who should I talk to if I get stuck on the [database / infrastructure / auth] part?

### Write Things Down

If you spent thirty minutes figuring out why something works a certain way, write it down. A comment in the code, a note in the README, a doc in the wiki, an ADR (Architecture Decision Record) for significant choices — anywhere someone else will find it when they have the same question.

Institutional knowledge that exists only in people's heads is a liability. People leave, forget, and are unavailable. The documentation survives.

### Manage Technical Debt Deliberately

Every codebase has technical debt. The question is whether you manage it or let it manage you.

- Acknowledge it. Pretending shortcuts do not exist does not make them disappear
- Track it. File issues, add `TODO` comments with ticket numbers, keep a tech debt backlog
- Pay it down incrementally. The boy scout rule — leave the code a little better than you found it — works if applied consistently
- Be honest about when you are taking on debt deliberately (shipping faster with a known shortcut) versus accidentally (not realizing you are making a mess)

### Manage Your Own Work

Senior engineers do not wait to be told what to do next. They:

- Break down their assigned work into concrete steps before starting
- Identify blockers early and raise them before they cause delays
- Communicate status without being asked
- Estimate honestly, including uncertainty and risk
- Push back on unrealistic timelines with evidence, not complaints
- Know when to ask for help and when to keep pushing

---

## In the Age of AI Assistants

AI coding tools — including agents like Claude Code and GitHub Copilot — make it faster to generate code. This makes engineering discipline *more* important, not less.

When code is cheap to produce, the bottleneck shifts to:

- **Understanding what to build** — AI can write code, but it cannot decide what the product should do or whether the requirements make sense
- **Reviewing and validating** — more generated code means more code that needs human judgment
- **Maintaining quality** — it is easy to accept AI suggestions without understanding them; do not do this
- **System design** — AI can implement a design; deciding the right design, the right boundaries, the right tradeoffs still requires engineering judgment and domain knowledge
- **Operational responsibility** — AI does not get paged at 2 AM when production breaks

Use AI tools to move faster, but do not outsource your judgment. If you cannot explain what your code does and why it is correct, it does not matter who or what wrote it — you have a problem.

The engineer who uses AI to write boilerplate faster while spending the saved time on design, edge cases, testing, and validation is ahead. The one who uses AI to produce more code without more understanding is building a house of cards.

The practices in this document — modularity, testing, code review, observability, safe deployment, security hygiene — are not overhead that AI eliminates. They are the practices that determine whether AI-assisted velocity turns into shipped products or shipped liabilities.

---

## The Don't-Do List: Common Newbie Pitfalls

Every experienced engineer has made most of these mistakes. The goal is not to shame anyone — it is to save you a few painful lessons by naming them up front.

### Coding Anti-Patterns

**Copy-paste-modify.** You find code that almost does what you need, copy it, change two lines, and move on. Now the same logic exists in two places with slight differences, and neither has a test. Next month someone fixes a bug in one copy and does not know the other exists. Extract shared logic into a function instead.

**The God function.** A single function that is 300 lines long, does input validation, business logic, database queries, error handling, and response formatting. It is untestable, unreadable, and every change to it is a risk. Break it apart.

**Stringly typed programming.** Using raw strings for things that should be enums, constants, or types: `if (status === "actve")` — notice the typo? The compiler did not, because it is just a string. Use enums or constants so the compiler catches mistakes for you.

**Boolean blindsill.** `processOrder(order, true, false, true)` — what do those booleans mean? Nobody knows without reading the function signature. Use named parameters, option objects, or descriptive enums instead.

**Catch and ignore.** `try { ... } catch (e) { }` — swallowing exceptions silently is how bugs become invisible. If you catch an exception, log it, handle it, or rethrow it. Empty catch blocks are never acceptable.

**Magic numbers and hardcoded values.** `if (retries > 3)` or `setTimeout(fn, 86400000)`. What is 3? Why that number? What is 86400000 milliseconds? Use named constants: `MAX_RETRIES = 3`, `ONE_DAY_MS = 86_400_000`. The next reader should not need a calculator.

**Premature cleverness.** Writing a generic, configurable, plugin-based abstraction for something you need to do once. You are not building a framework — you are solving a problem. Write the straightforward version first. Generalize only when you have three concrete cases that need it.

**Mega-PR.** You worked for a week on a branch, touched 40 files, and opened one massive pull request. Nobody can review it effectively. It will either sit for days or get rubber-stamped. Break the work into a sequence of small, reviewable PRs from the start.

### Process Anti-Patterns

**Works on my machine.** You tested locally and it passed. But you have a different OS, a different database version, extra environment variables, or stale cached data. If it does not pass in CI on a clean build, it is not proven.

**Committing to main.** Pushing directly to the main branch without a PR, review, or CI check. Even for "small fixes." Especially for small fixes — those are the ones most likely to contain a typo that takes down production.

**The fix-forward spiral.** Your change broke something, so you quickly push another change to fix it, which introduces a new bug, so you push another fix. Stop. Revert the original change, understand what went wrong, and fix it properly.

**Not reading the error.** The stack trace says exactly what went wrong and where. The compiler error tells you what it expected. Read it. Read all of it. Do not glance at the first line and start guessing.

**Suffering in silence.** You have been stuck for four hours and have not asked anyone for help because you do not want to seem incompetent. This is the most expensive anti-pattern on this list. A five-minute conversation with a teammate could save you a day. Set a timebox — if you are stuck for 30 to 60 minutes, ask.

**Skipping the requirements.** You started coding before you finished reading the ticket. Now you have built something that almost matches what was asked for, and the rework takes longer than doing it right the first time would have.

**Merging without understanding.** CI passed, the reviewer approved, but you do not actually understand why your fix works. If you cannot explain it, you do not own it. The next related bug will find you again.

### Code Smell Shortcuts

**TODO without a ticket.** `// TODO: fix this later` is a promise to no one. If it matters, file an issue and reference the ticket number. If it does not matter, delete the comment. Untracked TODOs are where good intentions go to die.

**Commented-out code.** Blocks of code that are commented out "just in case we need them." You will not need them. That is what version control is for. Delete them. They clutter the file and confuse anyone reading it.

**Console.log debugging left in production.** Development debugging output that made it into the final commit. Review your diff before pushing. Better yet, use a proper logging framework with log levels so debug output is never emitted in production.

**Ignoring warnings.** Compiler warnings, linter warnings, deprecation notices — these are the codebase trying to tell you something. Fix them or explicitly suppress them with a comment explaining why. Do not train yourself to ignore yellow text.

**Testing only the happy path.** Your test verifies that the function works when given perfect input. What about empty input? Null? A string where a number is expected? A list with one million items? The happy path is the easy part. The edge cases are where bugs live.

Most of these are not moral failings. They are habits. The engineers you admire made all of these mistakes — they just stopped making them.

---

## The Short Version

- Understand the problem before you implement
- Know when *not* to build — less code is often the better answer
- Own the outcome end to end — code, tests, deploy, production
- Prove it works with evidence, not confidence
- Modularize: one function, one job; one module, one responsibility
- Model your data carefully — bad schemas outlast bad code
- Never trust new data — profile, validate, and quarantine before it enters your system
- DRY for shared logic, but do not over-abstract
- Follow a style guide and enforce it with automated tooling
- Write comments that explain *why*, document APIs properly, maintain READMEs
- Handle errors deliberately — validate inputs, fail loud, design for partial failure
- Design for concurrency and idempotency — things run twice, messages arrive out of order
- Test at multiple levels: unit, integration, end-to-end
- Debug systematically — reproduce, hypothesize, narrow, verify
- Keep changes small, diffs clean, commit messages clear
- Write design docs for non-trivial work; record architectural decisions
- Separate your environments: local, CI, staging, production
- Use feature flags to decouple deploy from release, ramp gradually
- Maintain backwards compatibility — add, migrate, then remove
- Treat security as a default, not an afterthought
- Instrument your systems: logging, metrics, tracing, alerting
- Watch for the perennials: timezones, floating point money, encoding, nulls
- Manage dependencies, configuration, and technical debt deliberately
- Learn your tools, ask questions early, write things down
- Use AI to go faster, not to stop thinking

Engineering is not about writing code. It is about solving problems reliably, in a way that lasts, in a team.
