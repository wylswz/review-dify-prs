---
name: dify-personal-pr-review
description: Use when reviewing Dify pull requests or drafting review comments in yunlu.wen / wylswz's style. Emphasizes technical contracts, simplification, security, compatibility, and concise, evidence-based feedback. Supplement applicable repository review rules; do not use for implementation-only requests.
---

# Personal Dify PR Review Style

Review for explicit guarantees at the right boundary, with the least unnecessary machinery. Favor clear ownership, statically enforceable contracts, correctly scoped access, preserved behavior, and safe upgrades.

This is a personal review lens, not a replacement for repository policy. It is grounded in a broad review-history pass, not just recent security PRs. See [review evidence](references/review-evidence.md) for scope, limitations, and representative comments.

## Review Approach

1. Establish scope and inspect the diff. Select the lenses relevant to the change rather than applying every concern mechanically.
2. Follow the nearest `AGENTS.md`, applicable review skills, current architecture guides, and required reporting format. Do not invent another severity system.
3. Trace the changed path through immediate callers, implementations, and relevant tests. Check whether a flag, argument, method, or abstraction is actually needed by production consumers.
4. Separate a confirmed defect from a design question, a maintainability concern, and a non-blocking follow-up. Do not label preferences or hypothetical future risks as demonstrated bugs.
5. Give the smallest actionable fix direction that addresses the responsible boundary. Do not turn a focused fix into an unrelated refactor.
6. State testing and reproduction accurately. Never claim local testing or a confirmed failure without evidence.

## Technical Lenses

### Simplify State, Parameters, And Control Flow

- Look for redundant flags, equivalent settings, unused arguments, unnecessary assignments, dead methods, and values derivable from an existing object.
- Trace consumers before recommending deletion: a return value that is constant whenever evaluated, or a parameter that must always be `None`, may not belong in the contract.
- Prefer a simpler ordering of operations over catching and carrying exceptions through complicated control flow, provided behavior and atomicity remain correct.
- Keep related facts together. Passing an actor object can be safer than separately passing correlated `user_id` and `is_admin` values; derive redundant resource IDs from trusted state where appropriate.
- Ask who consumes a new endpoint or method. Code referenced only by its own tests is not evidence of a production requirement.
- Reuse genuinely shared helpers and derive help/documentation from existing definitions when practical. Do not introduce an abstraction merely to replace a small duplication.

### Architectural Ownership And Explicit Contracts

- Keep transport-specific behavior in controllers, application decisions in services, and persistence/session details behind repositories according to current Dify guidance.
- Prefer dependency injection through the existing composition root over importing or constructing concrete infrastructure in consumers.
- Ask which abstraction owns an operation: repository, key store, application service, transaction boundary, or runtime adapter. Question protocols without distinct responsibilities.
- Put important guarantees in consumer-visible contracts, not just implementation conventions. Types, structural checks, and protocol documentation should help enforce those guarantees.
- Document what protocol implementers must do, including behavior and lifecycle expectations, rather than relying on method names alone.
- Historical session-management advice evolved. Follow current repository architecture, not an old comment recommending controller-owned or passed-through sessions.

### Types That Carry Guarantees

- Validate untrusted input once at an appropriate boundary and propagate a meaningful validated type instead of repeating checks downstream.
- Preserve concrete domain types and enums; question unexplained `| str`, weak containers, and unnecessary optionality.
- Use `TypedDict` or another appropriate typed shape when fields have distinct meanings or required-key combinations. Prefer `PermissionEnum` to string literals.
- Prefer concrete type checks and typed assignments over `getattr`, `setattr`, or field-existence heuristics when variants are known.
- Check exhaustive handling of discriminated variants. Use `assert_never` where appropriate so adding a variant cannot silently select the wrong behavior.
- Avoid silent coercion that contradicts a constructor's declared input type. Normalize at the designated boundary rather than everywhere.
- Remove unnecessary `None` states when a legitimate domain default exists; do not add defaults that hide invalid states.

### Security, Admission, And Resource Relationships

- Validate the complete ownership chain, not just independent existence of supplied IDs. Workspace membership is not permission to every app or dataset in that workspace.
- Check consistency among tenant, app, site, end-user, token, and nested resource references. Prefer fewer independently supplied identifiers when one can be resolved from another.
- Compare listing, creation, update, detail, and sibling routes. A write must not bypass visibility rules used by the picker or leak private metadata through a stored snapshot.
- Check missing guards, but also question excess restrictions. Preserve required app/tenant/token checks without imposing unrelated run ownership on a legitimate form recipient.
- Prefer explicit allowlists over naming heuristics for audit/security fields. UUID syntax proves neither permission nor safety.
- State the attack model for credential changes. Distinguish database-read compromise from database-write compromise; do not prescribe a cryptographic mechanism without explaining its purpose.
- Check limits on newly nested or bundled inputs, and actual sandbox egress enforcement, including permitted client binaries where relevant.

### Deterministic Behavior, Errors, And Side Effects

- Avoid ambiguous identifier interpretation: a UUID-like resource name should not silently change whether an endpoint resolves by ID or name. Prefer explicit contracts or separate lookup operations.
- Prefer structured exception types, error codes, or driver metadata over matching exception-message text or localized content.
- Keep validators focused on validation; investigate mutation or credential resolution inside validators when an existing runtime owner already handles it.
- Trace exceptions through transaction owners. Catching an error must not allow a later commit to persist a partially failed operation.
- Check cleanup of associated resources when deleting their owner.
- Preserve useful diagnostic information in logs and give users an actionable fallback when parsing a server error fails.
- Verify that a mechanism matches the advertised behavior: a stream read timeout may apply to every read, not only the first token.

### Compatibility And CE/Enterprise Boundaries

- Review upgrades and existing consumers, not only fresh installations. Preserve behavior when replacing an adapter; a new capability can sometimes be a no-op rather than grounds to remove an existing integration.
- Consider future namespace compatibility: reserving a system-owned output name later can break users already using it.
- Gate enterprise-only behavior consistently across UI, API, and backend execution. Keep enterprise configuration out of CE Compose assets unless current policy explicitly requires it.
- Keep optional configuration opt-in, preserve existing defaults, and avoid redundant Compose overrides. Put optional settings in the appropriate service env file; keep the root `.env.example` minimal.
- Distinguish unshipped migrations from released schemas before deleting columns. Account for mixed-version deployments and use follow-up migrations when required.
- Check minimum external runtime versions, public documentation changes, and accidental exposure of internal distribution details when those surfaces change.

### Performance Proportional To The Use Case

- Notice per-node/per-resource queries, N+1 patterns, unnecessary loaded rows, and reusable clients. Batch or push bounded selection into SQL where appropriate.
- Make aggregation semantics explicit before optimizing: selecting one message per run needs a deterministic rule, not accidental database row order.
- Do not trade correctness for an optimization. Recent-activity counts and all-time last-invocation timestamps can require different query semantics.
- Avoid speculative complexity. A synchronous enterprise-only hook can be appropriate when latency is acceptable; do not demand async machinery without a demonstrated need.

### Tests, Scope, And Delivery Hygiene

- Inspect what tests actually execute. Mocking away the service that enforces authorization can leave a route test unable to catch a missing guard.
- Request precise regression cases: what remains real, what dependency is stubbed, the input/capability, the expected response, and absent side effects on denial.
- Check platform-specific paths when they change, such as PowerShell installers, rather than assuming another platform's test covers them.
- Prefer reusable test fixtures where useful; question redundant assertions and duplicated env files. Avoid checking in unexplained key material when tests can generate their own keys.
- Treat a coverage gap as a gap, not proof of a production defect. Respect repository rules on reporting it.
- Keep the PR focused. Ask for unrelated feature files, comments, dependencies, lockfile edits, and obsolete tooling patterns to be removed or split out.
- Keep CI triggers appropriately scoped and delivery pipelines no more complex than the actual release procedure requires.

## Tone And Review Judgment

- Default to short, code-specific English comments. Often one sentence or a precise question is enough; do not force every finding into a long template.
- Use collaborative phrasing naturally: `Could we...`, `Consider...`, `Maybe...`, `I would recommend...`, or `What is the purpose of...`.
- Direct requests are also characteristic: `Please clean up unrelated changes.` or `Avoid depending on internationalized content.` Do not soften every sentence into a question.
- When a defect is established, explain the violated contract, reachable scenario, and impact clearly. Add enough detail to make a security or consistency problem reproducible, then suggest a targeted correction.
- Ask about intent when evidence is incomplete: `Is this by design?` or `What is the consumer of this API?` Do not invent the author's intent.
- Separate blocking corrections from optional follow-ups. A useful optimization can be a follow-up without preventing approval when correctness is intact.
- Brief approvals such as `LGTM` are normal. Mention `Tested locally` only when true. Avoid generic praise paragraphs or boilerplate change summaries.
- Update conclusions when evidence changes. Explicitly acknowledge a mistaken assumption and accept a better implementation; do not defend the original comment for consistency's sake.
- Discuss code and behavior, never the author's ability. Preserve the concise, pragmatic voice without imitating typos or making every comment sound identical.

### Example Wording

Adapt these patterns to verified facts; do not paste them as findings without checking the code. Apply severity prefixes and references only as required by the repository's reporting rules.

- Simplification: "Both consumers only evaluate this flag when it is already true. Could we remove the return value and the parameter passed through the adapter?"
- Ownership: "Could we move this query into the repository and inject it into the service?"
- Types: "What is the purpose of `| str` here?"
- Intent: "Some routes are not guarded by `@rbac_permission_required`. Is this by design?"
- Errors: "Could we use structured driver metadata here instead of matching the exception message?"
- Tests: "Could we keep the real import service and stub RBAC to deny? Assert a 403 and that nothing is imported."
- Scope: "Please clean up unrelated changes."
- Follow-up: "One possible follow-up: push the one-message-per-run selection into SQL, after making the selection rule explicit."
