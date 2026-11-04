# Review Evidence

## Coverage And Method

- Account: `wylswz` (local Git identity: `yunlu.wen`). Repository: `langgenius/dify`.
- Retrieved on 2026-11-04 using the authenticated GitHub GraphQL API.
- Paginated all results of `repo:langgenius/dify is:pr reviewed-by:wylswz sort:created-asc`: 577 matched PRs, below GitHub search's 1,000-result ceiling.
- Fetched reviews filtered by author `wylswz`, including review bodies and inline comments. Checked pagination for both review and comment connections.
- Excluded 33 self-authored PRs. Of the remaining 544 PRs, 369 had submitted reviews; these contained 502 submitted review records, 134 inline comments, and 28 nonempty review bodies. Read all exported nonempty feedback, separating questions, recommendations, approvals, replies, and corrections.
- Excluded 178 pending review records on other authors' PRs, including two draft inline comments. Submitted review dates range from 2025-09-26 to 2026-11-04. Dismissed but submitted reviews remain historical evidence, not current approval status.
- This covers the review records accessible through this search at retrieval time, not deleted/inaccessible history or all general PR conversation comments. Inline replies are interpreted in context, not automatically treated as independent review demands.
- Empty approvals provide no evidence of a particular technical preference. Repeated themes receive more emphasis than isolated suggestions; examples are not universal mandates.
- Historical architecture advice changed: June/August 2026 comments allowed passing sessions, while September/October comments move session ownership into repositories. The skill defers to current repository policy rather than combining conflicting historical rules.

## Representative Sources

These links support the skill's lenses; they are a selection from the larger corpus, not the sampling boundary. The wording examples in `SKILL.md` are adapted patterns, not all verbatim quotations.

### Simplification And Ownership

- Simpler exception/operation ordering: https://github.com/langgenius/dify/pull/32938#discussion_r2881627277
- Equivalent semantics with fewer flags: https://github.com/langgenius/dify/pull/33788#discussion_r2964280637
- Correlated actor fields: https://github.com/langgenius/dify/pull/35468#discussion_r3322988342
- Always-None parameter: https://github.com/langgenius/dify/pull/35891#discussion_r3206576782
- Derivable identifiers and enforceable reference contracts: https://github.com/langgenius/dify/pull/38177#discussion_r3496547296
- Consumers and unreachable methods: https://github.com/langgenius/dify/pull/39675#discussion_r3800831733
- Current session ownership: https://github.com/langgenius/dify/pull/41420#discussion_r3966622228
- Protocol responsibilities: https://github.com/langgenius/dify/pull/43671#discussion_r4236827635
- Repository queries and injection: https://github.com/langgenius/dify/pull/43803#discussion_r4246094437
- Redundant flag traced through consumers: https://github.com/langgenius/dify/pull/44795#discussion_r4374893376

### Types, Determinism, And Errors

- Ambiguous UUID-like names: https://github.com/langgenius/dify/pull/37212#discussion_r3377965296
- Avoid localized-content dependencies: https://github.com/langgenius/dify/pull/37107#discussion_r3378572924
- Exception detail in logs: https://github.com/langgenius/dify/pull/38192#discussion_r3510640569
- Timeout semantics in the underlying stream reader: https://github.com/langgenius/dify/pull/38586#discussion_r3600190789
- Validated types instead of repeated checks: https://github.com/langgenius/dify/pull/39033#discussion_r3592603631
- Protocol implementer guidance: https://github.com/langgenius/dify/pull/39480#discussion_r3649045045
- Side effects inside validators: https://github.com/langgenius/dify/pull/41625#discussion_r3912476397
- Exhaustive variant handling: https://github.com/langgenius/dify/pull/41685#discussion_r3920701094
- Typed return shapes: https://github.com/langgenius/dify/pull/41685#discussion_r3920800860
- Structured errors instead of message matching: https://github.com/langgenius/dify/pull/41687#discussion_r3954243861

### Security, Compatibility, And Performance

- Exception handling can permit an inconsistent commit: https://github.com/langgenius/dify/pull/32938#discussion_r2881691495
- Synchronous operation when latency does not justify complexity: https://github.com/langgenius/dify/pull/32308#discussion_r2807286712
- Enterprise gating: https://github.com/langgenius/dify/pull/36839#discussion_r3355495430
- SQL selection and explicit aggregation semantics: https://github.com/langgenius/dify/pull/38359#pullrequestreview-4654603729
- Preserve enterprise adapter behavior: https://github.com/langgenius/dify/pull/39480#discussion_r3648979535
- Breaking changes from future reserved names: https://github.com/langgenius/dify/pull/41062#discussion_r3829397585
- Related resource authorization: https://github.com/langgenius/dify/pull/41318#discussion_r4093349970
- Opt-in deployment defaults: https://github.com/langgenius/dify/pull/42481#discussion_r4045392060
- Per-node query batching: https://github.com/langgenius/dify/pull/44673#discussion_r4424036499
- Limits on bundled workflow packages: https://github.com/langgenius/dify/pull/44592#discussion_r4385670810
- Scope-write visibility checks: https://github.com/langgenius/dify/pull/45540#discussion_r4465945531
- Avoid excessive ownership restrictions: https://github.com/langgenius/dify/pull/45540#discussion_r4466339115
- Shipped versus unshipped migration cleanup: https://github.com/langgenius/dify/pull/45540#discussion_r4466468234

### Testing, Delivery, And Tone

- Platform-specific installer tests: https://github.com/langgenius/dify/pull/37036#discussion_r3361865745
- Generate test keys rather than keep unexplained key material: https://github.com/langgenius/dify/pull/38531#discussion_r3542145946
- Correcting an earlier conclusion: https://github.com/langgenius/dify/pull/39453#discussion_r3662022317
- Accepting an alternative after testing: https://github.com/langgenius/dify/pull/39453#discussion_r3662080449
- Keep the release pipeline simple: https://github.com/langgenius/dify/pull/41355#discussion_r3869361047
- Direct cleanup request: https://github.com/langgenius/dify/pull/41835#discussion_r3946027426
- Split unrelated changes: https://github.com/langgenius/dify/pull/41890#discussion_r3946036657
- Precise API-level authorization test request: https://github.com/langgenius/dify/pull/45105#discussion_r4451162436

## Interpretation Limits

This skill describes observed review preferences, not a personality assessment or a claim that every concern appeared frequently. Detailed PAT security and migration examples come from a recent feature review; use them as applicable boundary checks, not requirements for every PR. Findings still require evidence in the code under review. An older comment, a future-hardening suggestion, or a missing test does not establish a current production bug.
