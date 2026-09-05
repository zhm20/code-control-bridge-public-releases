# Code Control Bridge

[中文](./README.md) · [English](./README.en.md)

**Finish the implementation plan. Let Controller carry it through to delivery.**

Code Control Bridge (CCB) connects ordinary ChatGPT Pro Web chat and local Codex
in one persistent delivery loop. The Controller in ChatGPT understands the project,
clarifies the request, produces the plan, and reviews the real result. Local Codex
acts as the Loop Engineer, carrying code changes, tests, rework, and authorized Git
delivery through in the same task context.

You do not have to keep copying long plans, chasing local progress, or reviewing a
change from a short summary. CCB connects planning, implementation, review, recovery,
and acceptance through 27 bounded tools, two Skills, and two execution modes, while
leaving the decisions that matter with you.

Plan and review in ChatGPT Pro, then save your Codex weekly allowance for hands-on
local implementation.

> CCB does not add, transfer, bypass, or guarantee any OpenAI quota. It is designed
> for a workflow that uses ordinary ChatGPT Pro Web chat for planning and review and
> Codex for local implementation. Actual allowances, reset periods, models,
> CCB plugin (MCP) capabilities, and availability follow your live account display
> and current OpenAI policy. ChatGPT Work should not be treated as an additional,
> independent allowance.

## One loop from plan to delivery

1. **The Controller understands the project:** inspect real project state in ChatGPT
   Web and align on scope, key decisions, and acceptance criteria.
2. **The accepted plan lands intact:** transfer a long plan in verified chunks and
   materialize it atomically in the local Plan Inbox, avoiding copy-and-paste loss.
3. **Local Codex implements continuously:** use the primary project directory or an
   isolated worktree and keep changes, tests, and rework in one Codex task.
4. **The result is saved before Review evidence:** when Codex finishes, CCB atomically
   saves the complete result, digest, and native thread/turn handles before freezing
   the Review diff. A temporary Git-evidence failure cannot discard the real result.
5. **Web review stays anchored to reality:** read back the complete result and locked
   diff in pages, then return feedback to the same implementation context.
6. **Delivery happens only with your authorization:** execute or reconcile an exact
   commit, normal push, and optional no-ff merge, then verify the branch, commit, and
   remote result. Acceptance never implies merge authorization.

## Recover without rerunning implementation

### Result-first Review evidence

- When an implementation turn completes, its result, result digest, and native handles
  are committed in one transaction first.
- If the Review diff is not ready, the turn becomes `RESULT_CAPTURED` and the
  Delivery becomes `AWAITING_REVIEW_EVIDENCE_RECOVERY`; the complete result remains
  pageable.
- `ccb_delivery_review_recover` completes evidence on the original turn without
  starting Codex, sending a prompt, or increasing the turn ordinal.
- An old result can be bound to recovered evidence only while the completion-time
  content fingerprint still matches the current bytes. Changed content returns to
  same-thread rework instead of presenting new bytes as the old result.

### External publish and no-ff merge reconciliation

- If the exact patch was reviewed externally and its feature commit was normally
  pushed, `ccb_delivery_external_publish_reconcile` verifies the target turn's
  `before_head_sha`, remote tip, single direct-child commit, complete raw binary
  patch SHA-256, changed paths, and clean workspace.
- Success records only `publication_source=external_attested`. It does not fabricate
  native Review evidence, claim that CCB assessed code quality, or replace final user
  acceptance.
- `ccb_merge_prepare` can recognize an externally completed no-ff merge. CCB verifies
  the unique first-parent merge, its two parents, and a recomputed merge tree before
  `ccb_delivery_finalize(action=external_merge)` revalidates the locked facts.
- External finalize does not commit, push, or delete the feature branch. It records
  `merge_source=external_verified` and releases only the workspace or lease owned
  by that Delivery.

## 27 tools

The tools are grouped into six jobs, with explicit boundaries around reading,
writing, recovery, and acceptance.

<details>
<summary><strong>Show the complete tool catalog</strong></summary>

### Skill discovery (2)

- `ccb_web_skills_list`
- `ccb_web_skill_read`

### Project discovery (6)

- `ccb_project_initialize`
- `ccb_repository_inspect`
- `ccb_repository_tree_page`
- `ccb_repository_file_read_page`
- `ccb_local_project_search`
- `ccb_local_project_file_read_page`

### Plan materialization (4)

- `ccb_plan_content_begin`
- `ccb_plan_content_append`
- `ccb_plan_content_status`
- `ccb_plan_content_finalize`

### Plan publication (3)

- `ccb_plan_publish_prepare`
- `ccb_plan_publish_execute`
- `ccb_plan_publish_status`

### Implementation, Review, and recovery (8)

- `ccb_delivery_preflight`
- `ccb_delivery_start`
- `ccb_delivery_turn_start`
- `ccb_delivery_status`
- `ccb_delivery_turn_result_page`
- `ccb_delivery_diff_page`
- `ccb_delivery_review_recover`
- `ccb_delivery_external_publish_reconcile`

### Acceptance and merge (4)

- `ccb_delivery_acceptance_decide`
- `ccb_merge_prepare`
- `ccb_merge_execute`
- `ccb_delivery_finalize`

</details>

The beta40 current release retains MCP surface
`tool_surface_ref:ccb_transparent_web_delivery_v3`. Tool availability can vary
with account state, staged rollout, and OpenAI policy. Use the
[independent trust record](https://trust.zhm20.com/ccb/) for published evidence.

## Two Skills and two execution modes

- `ccb-web-plan-authoring` **v6**: starts from the real project, clarifies the
  request, and produces a decision-complete, reviewable, digest-verified HTML
  implementation plan.
- `ccb-web-delivery-loop` **v8**: coordinates the Controller and local Codex across
  result-first capture, Review-evidence recovery, external publish and merge
  reconciliation, acceptance, and authorized Git operations. Cached older delivery
  refs remain compatibility aliases for the current v8 body.
- `primary_checkout`: works directly in the permanent project directory, suited to
  a small project or one active primary task.
- `linked_worktree`: uses a separate worktree and branch for parallel tasks, keeping
  their files and Git state isolated.

## Keep moving after one authorization

CCB keeps actions that can safely run together in one continuous task round. After
you confirm the implementation scope, execution mode, branch, model, reasoning
effort, and Codex full-access contract, implementation, tests, status polling,
evidence recovery, and requested rework can continue without asking for the same
permission again.

It pauses only for a decision that still belongs to you: final acceptance, whether
to merge, external Review attestation, external-merge finalization, new scope,
credentials or MFA, or real external drift. An already confirmed scope is not split
into repetitive authorization prompts.

## Refresh ChatGPT tools after upgrading

Local `/usr/local/bin/ccb service restart` updates the CCB runtime, but it does not
rewrite the tool snapshot already saved by a ChatGPT MCP App. After installing
beta40:

1. Restart the CCB service.
2. In the ChatGPT MCP App's **Action control** or **Configure Actions**, use
   **Refresh**. If Refresh is unavailable in the current UI, follow the platform's
   current save or publish flow and use **Scan Tools**.
3. Review the tool diff and explicitly enable `ccb_delivery_review_recover` and
   `ccb_delivery_external_publish_reconcile`.
4. Save or republish the App. Start a new ChatGPT Web conversation and verify that
   all 27 tools and Skills v6/v8 are visible before beginning delivery.

## beta40 current public release

Beta40 uses a new tag, Formal Candidate, and complete 14-attachment set. The product
runtime, 27 tools, Skill v6/v8, dual-root CA bundle, and readers are unchanged.
Functional evidence comes from beta38 Web validation and beta39 installed validation;
no beta40 device installation or ChatGPT Web Refresh is claimed.

| Item | Current value |
| --- | --- |
| Version | `0.1.0-beta.40` |
| Tool surface | `tool_surface_ref:ccb_transparent_web_delivery_v3` |
| Skills | plan v6 / delivery v8 |
| Immutable tag | `v0.1.0-beta.40` |
| Source commit | `75f3fc73424d3b127a1e8379f797adfcea153497` |
| Formal Candidate ID | `sha256:37893532c2fab0a3722add6ee88ab6b9fa52626f6a506dec51a02998e4ebc924` |
| GitHub Release state | `published immutable prerelease; 14/14 assets` |
| Release date | `2026-09-05` |
| Package | `0.1.0-beta.40.pkg` |
| Package SHA-256 | `8ae7b4f54cbb9bdfd1a884615be724b0a44e766b441bace88d41c71943565019` |
| Buyer verifier SHA-256 | `b7ceb7c5e9b6700ce89638cedf2dfd387de765bc8aa53de505a0816e0c9044fc` |
| Release public key SPKI DER SHA-256 | `796d69af8f8ef6b74551573eded3c54474b02e6f1e464889dd068c1b116a8dec` |
| Control-plane CA bundle SHA-256 | `709ce419f95333d64de6086948e2e76dc9c9367a6e0d99a62dbbf12e73939007` |
| Reader generation | `beta38-v1`; beta40 does not rotate it again |
| Third-party notices | `THIRD_PARTY_NOTICES-0.1.0-beta.40.md` |
| SBOM SHA-256 | `a65c37bd08703f89c3c9943277a22130cf96692f0caee42fa013ccf7a3a620cf` |

Get the package, statement, signature, verifier, SBOM, compatibility notice, Release
Notes, notices, and user manual from the [beta40 Release](https://github.com/zhm20/code-control-bridge-public-releases/releases/tag/v0.1.0-beta.40).

## Download and verification

1. Read the pre-purchase disclosures on the
   [product website](https://ccb.zhm20.com/en/) and the
   [installation guide](https://ccb.zhm20.com/en/guide/install/).
2. Enter the immutable Release through the
   [official download page](https://downloads.zhm20.com/ccb).
3. Check the buyer-verifier and release-public-key digests against the
   [independent trust record](https://trust.zhm20.com/ccb/).
4. Verify the package with the Release's `verify-ccb-release.sh`, release
   statement, and detached signature before installing it.

Do not substitute search results, third-party mirrors, or a checksum copied from a
chat message for the official download and independent trust record.

## Important disclosures

- **This repository is a public binary-release entry point, not the product source
  repository.** The Git checkout contains public documentation only. Binary install
  and verification files are published as GitHub Release attachments. This repository
  does not contain the Code Control Bridge source, private development repository,
  internal operational evidence, account credentials, private configuration values,
  or signing private keys.
- GitHub's automatically generated `Source code (zip)` and `Source code (tar.gz)`
  files are snapshots of this release-entry repository. They are neither the product
  source nor installation assets. Download and verify the corresponding package for
  installation.
- The current package is **not signed with an Apple Developer ID, is not notarized,
  and is not stapled**. A macOS warning that the developer cannot be verified is
  expected for this release method. It is not a claim of Apple review,
  certification, or endorsement.
- Integrity is provided by the publisher signature, published SHA-256 values, and
  the independent trust record. Do not disable Gatekeeper or use an untrusted bypass
  command.
- The current release targets Apple Silicon Macs. Company- or school-managed MDM
  devices are unsupported. See the Release compatibility notice for exact OS scope
  and other limitations.
- The tunnel client, Git, and Codex CLI are installed separately from their own
  trusted sources and are not included in the CCB package.

## Official links

- Product and purchase: [ccb.zhm20.com/en](https://ccb.zhm20.com/en/)
- Official downloads: [downloads.zhm20.com/ccb](https://downloads.zhm20.com/ccb)
- Independent trust record: [trust.zhm20.com/ccb](https://trust.zhm20.com/ccb/)
- Buyer guides: [ccb.zhm20.com/en/guide](https://ccb.zhm20.com/en/guide/)
- Support center: [ccb.zhm20.com/en/support](https://ccb.zhm20.com/en/support/)
- Support email: [eric.zhm2018@gmail.com](mailto:eric.zhm2018@gmail.com)
