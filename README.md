# Code Control Bridge

[中文](./README.md) · [English](./README.en.md)

**聊完实施计划，放心交给Controller持续推进到交付。**

Code Control Bridge（CCB）把普通 ChatGPT Pro Web 聊天与本地 Codex
连接成一条可持续推进的交付 Loop。ChatGPT 中的 Controller 负责读懂项目、
澄清需求、形成计划和审查真实结果；本地 Codex 作为 Loop Engineer，在同一个
任务上下文中完成代码、测试、返工和经授权的 Git 交付。

你不必反复复制长计划、追问本地任务进度，或只凭一段摘要审查改动。CCB 通过
25 个边界明确的工具、两份 Skill 和两种执行方式，把计划、实施、审查与验收
连接起来，同时把关键决定保留给你。

让 ChatGPT Pro 做规划与审查，让 Codex 把周额度留给真正的本地实现。

> CCB 不会增加、转移、绕过或保证任何 OpenAI 额度。本产品面向使用普通
> ChatGPT Pro Web 聊天进行规划与审查、使用 Codex 进行本地实施的场景。
> 实际额度、重置周期、模型、CCB 插件（MCP）能力和可用性以你的账户实时显示及
> OpenAI 当前政策为准；ChatGPT Work 不应被理解为额外的独立额度。

## 一条从计划到交付的 Loop

1. **Controller 读懂项目**：在 ChatGPT Web 中读取真实项目状态，集中确认目标、
   范围、关键决定和验收标准。
2. **计划可靠落地**：将已接受的长计划分块传输、校验并原子写入本机 Plan Inbox，
   减少复制粘贴造成的截断与遗漏。
3. **本地 Codex 持续实施**：选择项目主目录或隔离 worktree，让同一个 Codex
   任务完成修改、测试和返工。
4. **Web 对准真实结果审查**：分页读回完整结果和锁定 diff，把反馈继续送回原任务，
   不重新建立实施上下文。
5. **经你授权后交付**：精确执行 commit、普通 push 和可选 merge，并核对分支、
   提交与远端结果。接受交付不等于自动获得合并授权。

## 25 个工具

工具按职责分为六组，每项读取、写入、等待和验收动作都有清晰边界。

<details>
<summary><strong>展开完整工具清单</strong></summary>

### Skill 发现（2）

- `ccb_web_skills_list`
- `ccb_web_skill_read`

### 项目发现（6）

- `ccb_project_initialize`
- `ccb_repository_inspect`
- `ccb_repository_tree_page`
- `ccb_repository_file_read_page`
- `ccb_local_project_search`
- `ccb_local_project_file_read_page`

### 计划物化（4）

- `ccb_plan_content_begin`
- `ccb_plan_content_append`
- `ccb_plan_content_status`
- `ccb_plan_content_finalize`

### 计划发布（3）

- `ccb_plan_publish_prepare`
- `ccb_plan_publish_execute`
- `ccb_plan_publish_status`

### 实施交付（6）

- `ccb_delivery_preflight`
- `ccb_delivery_start`
- `ccb_delivery_turn_start`
- `ccb_delivery_status`
- `ccb_delivery_turn_result_page`
- `ccb_delivery_diff_page`

### 验收与合并（4）

- `ccb_delivery_acceptance_decide`
- `ccb_merge_prepare`
- `ccb_merge_execute`
- `ccb_delivery_finalize`

</details>

工具能力可能随账户状态、灰度发布和 OpenAI 政策变化。当前公开证据说明见
[独立信任记录](https://trust.zhm20.com/ccb/)。

## 两份 Skill，两种执行方式

- `ccb-web-plan-authoring` **v6**：从真实项目出发澄清需求，形成决策完整、可审阅、
  可校验的 HTML 实施计划。
- `ccb-web-delivery-loop` **v7**：组织 Controller 与本地 Codex 的持续交付，覆盖
  实施前检查、同任务返工、结果与 diff 审查、验收，以及经授权的 Git 操作。
- `primary_checkout`：直接在永久项目目录中工作，适合小项目或当前只有一项主要任务。
- `linked_worktree`：为并行任务使用独立 worktree 和分支，减少工作区互相干扰。

## 当前公开测试版

| 项目 | 精确值 |
| --- | --- |
| Version | `0.1.0-beta.35` |
| Immutable tag | [`v0.1.0-beta.35`](https://github.com/zhm20/code-control-bridge-public-releases/releases/tag/v0.1.0-beta.35) |
| GitHub Release 状态 | `prerelease`，`draft=false` |
| Release date | `2026-09-04` |
| Package | `0.1.0-beta.35.pkg` |
| Package SHA-256 | `9814494c93629e6c25a787213968f904e3e7fcda501aacd0b5e7cde374d3bd11` |
| Buyer verifier SHA-256 | `b7ceb7c5e9b6700ce89638cedf2dfd387de765bc8aa53de505a0816e0c9044fc` |
| Release public key SPKI DER SHA-256 | `796d69af8f8ef6b74551573eded3c54474b02e6f1e464889dd068c1b116a8dec` |
| SBOM SHA-256 | `454d6122b0256d859f30103c81cc6e27db0f265b768ba5800188d2e51c47ed71` |

请从 [beta35 Release](https://github.com/zhm20/code-control-bridge-public-releases/releases/tag/v0.1.0-beta.35)
取得安装包、SHA-256 文件、release statement、detached signature、买家校验脚本、
SBOM、兼容性说明和用户手册。已发布资产不可覆盖；任何修复都使用新的 successor
版本。

## 下载与验真

1. 阅读[产品官网](https://ccb.zhm20.com/)中的购买前披露和
   [安装指南](https://ccb.zhm20.com/guide/install/)。
2. 从[官方下载入口](https://downloads.zhm20.com/ccb)进入对应的 immutable Release。
3. 先从[独立信任记录](https://trust.zhm20.com/ccb/)核对买家校验脚本和发布公钥摘要。
4. 使用 Release 中的 `verify-ccb-release.sh`、release statement 和 detached
   signature 验证 PKG；校验通过后再安装。

不要用搜索结果、第三方镜像或聊天消息中的摘要替代官方下载与独立信任记录。

## 重要披露

- **本仓库是公开的二进制发布入口，不是产品源码仓库。** Git checkout 只包含
  公开说明，二进制安装与验真文件通过 GitHub Release attachments 发布；这里不包含
  Code Control Bridge 源码、私有开发仓库、内部运行证据、账户凭据、私有配置值或
  签名私钥。
- GitHub 自动生成的 `Source code (zip)` 和 `Source code (tar.gz)` 只是本发布入口
  仓库的快照，不是产品源码，也不是安装资产。安装时应下载并验证对应的 PKG。
- 当前 PKG **未使用 Apple Developer ID 签名、未经 Apple 公证，也没有 staple**。
  安装时出现 macOS 无法验证开发者的提示是当前发布方式的预期表现，不代表 Apple
  审核、认证或背书。
- 完整性由发布方签名、公开 SHA-256 和独立信任记录共同提供。请勿关闭 Gatekeeper
  或使用来源不明的绕过命令。
- 当前版本面向 Apple Silicon Mac；公司或学校统一管理的 MDM 设备不在支持范围内。
  准确系统版本和其他限制以该 Release 的兼容性说明为准。
- tunnel client、Git 和 Codex CLI 需按买家指南从各自可信来源单独安装，不包含在
  CCB PKG 中。

## 官方入口

- 产品与购买：[ccb.zhm20.com](https://ccb.zhm20.com/)
- 官方下载：[downloads.zhm20.com/ccb](https://downloads.zhm20.com/ccb)
- 独立信任记录：[trust.zhm20.com/ccb](https://trust.zhm20.com/ccb/)
- 买家指南：[ccb.zhm20.com/guide](https://ccb.zhm20.com/guide/)
- 支持中心：[ccb.zhm20.com/support](https://ccb.zhm20.com/support/)
- 支持邮箱：[eric.zhm2018@gmail.com](mailto:eric.zhm2018@gmail.com)
