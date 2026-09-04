# Code Control Bridge

[中文](./README.md) · [English](./README.en.md)

**Finish the implementation plan. Let Controller carry it through to delivery.**

Code Control Bridge (CCB) connects ordinary ChatGPT Pro Web chat and local Codex
in one persistent delivery loop. The Controller in ChatGPT understands the project,
clarifies the request, produces the plan, and reviews the real result. Local Codex
acts as the Loop Engineer, carrying code changes, tests, rework, and authorized Git
delivery through in the same task context.

You do not have to keep copying long plans, chasing local progress, or reviewing a
change from a short summary. CCB connects planning, implementation, review, and
acceptance through 25 bounded tools, two Skills, and two execution modes, while
leaving the key decisions with you.

Plan and review in ChatGPT Pro, then save your Codex weekly allowance for hands-on
local implementation.

> CCB does not add, transfer, bypass, or guarantee any OpenAI quota. It is designed
> for a workflow that uses ordinary ChatGPT Pro Web chat for planning and review and
> Codex for local implementation. Actual allowances, reset periods, models,
> CCB plugin (MCP) capabilities, and availability follow your live account display and
> current OpenAI policy. ChatGPT Work should not be treated as an additional,
> independent allowance.

## One loop from plan to delivery

1. **The Controller understands the project:** inspect real project state in ChatGPT
   Web and align on scope, key decisions, and acceptance criteria.
2. **The accepted plan lands intact:** transfer a long plan in verified chunks and
   materialize it atomically in the local Plan Inbox, avoiding copy-and-paste loss.
3. **Local Codex implements continuously:** use the primary project directory or an
   isolated worktree and keep changes, tests, and rework in one Codex task.
4. **Web review stays anchored to reality:** read back the complete result and locked
   diff in pages, then return feedback to the same implementation context.
5. **Delivery happens only with your authorization:** perform an exact commit, normal
   push, and optional merge, then verify the branch, commit, and remote result.
   Accepting a delivery does not automatically authorize a merge.

## 25 tools

The tools are grouped into six jobs, with explicit boundaries around reading,
writing, waiting, and acceptance.

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

### Implementation delivery (6)

- `ccb_delivery_preflight`
- `ccb_delivery_start`
- `ccb_delivery_turn_start`
- `ccb_delivery_status`
- `ccb_delivery_turn_result_page`
- `ccb_delivery_diff_page`

### Acceptance and merge (4)

- `ccb_delivery_acceptance_decide`
- `ccb_merge_prepare`
- `ccb_merge_execute`
- `ccb_delivery_finalize`

</details>

Tool availability can vary with account state, staged rollout, and OpenAI policy.
See the [independent trust record](https://trust.zhm20.com/ccb/) for the current
public evidence.

## Two Skills and two execution modes

- `ccb-web-plan-authoring` **v6**: starts from the real project, clarifies the request,
  and produces a decision-complete, reviewable, digest-verified HTML implementation
  plan.
- `ccb-web-delivery-loop` **v7**: coordinates the Controller and local Codex across
  preflight, same-task rework, full result and diff review, acceptance, and authorized
  Git operations.
- `primary_checkout`: works directly in the permanent project directory, suited to a
  small project or one active primary task.
- `linked_worktree`: uses a separate worktree and branch for parallel tasks, keeping
  their files and Git state isolated.

## Current public prerelease

| Item | Exact value |
| --- | --- |
| Version | `0.1.0-beta.35` |
| Immutable tag | [`v0.1.0-beta.35`](https://github.com/zhm20/code-control-bridge-public-releases/releases/tag/v0.1.0-beta.35) |
| GitHub Release state | `prerelease`, `draft=false` |
| Release date | `2026-09-04` |
| Package | `0.1.0-beta.35.pkg` |
| Package SHA-256 | `9814494c93629e6c25a787213968f904e3e7fcda501aacd0b5e7cde374d3bd11` |
| Buyer verifier SHA-256 | `b7ceb7c5e9b6700ce89638cedf2dfd387de765bc8aa53de505a0816e0c9044fc` |
| Release public key SPKI DER SHA-256 | `796d69af8f8ef6b74551573eded3c54474b02e6f1e464889dd068c1b116a8dec` |
| SBOM SHA-256 | `454d6122b0256d859f30103c81cc6e27db0f265b768ba5800188d2e51c47ed71` |

Get the package, SHA-256 file, release statement, detached signature, buyer
verifier, SBOM, compatibility notice, and user manual from the
[beta35 Release](https://github.com/zhm20/code-control-bridge-public-releases/releases/tag/v0.1.0-beta.35).
Published assets are immutable. Any correction is released as a new successor
version.

## Download and verification

1. Read the pre-purchase disclosures on the [product website](https://ccb.zhm20.com/en/)
   and the [installation guide](https://ccb.zhm20.com/en/guide/install/).
2. Enter the immutable Release through the
   [official download page](https://downloads.zhm20.com/ccb).
3. Check the buyer-verifier and release-public-key digests against the
   [independent trust record](https://trust.zhm20.com/ccb/).
4. Verify the package with the Release's `verify-ccb-release.sh`, release statement,
   and detached signature before installing it.

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
