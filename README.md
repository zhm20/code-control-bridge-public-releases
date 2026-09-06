# Code Control Bridge

[中文](./README.md) · [English](./README.en.md)

**聊完实施计划，放心交给 Controller 持续推进到交付。**

Code Control Bridge（CCB）把普通 ChatGPT Pro Web 聊天与本地 Codex
连接成一条持续推进的交付 Loop。ChatGPT 中的 Controller 负责读懂项目、
澄清需求、形成计划和审查真实结果；本地 Codex 作为 Loop Engineer，在同一个
任务上下文中完成代码、测试、返工和经授权的 Git 交付。

你不必反复复制长计划、追问本地任务进度，或只凭一段摘要审查改动。CCB 通过
27 个边界明确的工具、两份 Skill 和两种执行方式，把计划、实施、审查、恢复与
验收连接起来，同时把真正需要判断的决定保留给你。

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
4. **结果先保存，再生成审查证据**：Codex 完成后，CCB 先原子保存完整 result、
   摘要和原生 thread/turn handle，再冻结 Review diff。即使 Git evidence
   暂时失败，真实结果也不会丢失。
5. **Web 对准真实结果审查**：分页读回完整结果和锁定 diff，把反馈继续送回原任务，
   不重新建立实施上下文。
6. **经你授权后交付**：精确执行或对账 commit、普通 push 和可选 no-ff merge，
   并核对分支、提交与远端结果。接受交付不等于自动获得合并授权。

## 恢复不再重跑实现

### Result-first Review evidence

- 实施 turn 完成后，result、result digest 和原生 handle 在一个事务中先落盘。
- Review diff 暂未生成时，turn 为 `RESULT_CAPTURED`，Delivery 为
  `AWAITING_REVIEW_EVIDENCE_RECOVERY`；完整 result 仍可分页读取。
- `ccb_delivery_review_recover` 在原 turn 上补齐证据，不启动 Codex、
  不发送 prompt，也不增加 turn ordinal。
- 只有完成时保存的内容指纹与当前 bytes 一致，旧 result 才能绑定恢复后的 diff；
  内容变化时回到同一线程返工，不把新 bytes 冒充旧结果。

### 外部发布与 no-ff merge 对账

- 如果精确 patch 已在外部完成 Review，并且对应 feature commit 已普通推送，
  `ccb_delivery_external_publish_reconcile` 会核对目标 turn 的
  `before_head_sha`、远端 tip、单一直接子提交、完整 raw binary patch SHA-256、
  changed paths 和 clean workspace。
- 成功只记录 `publication_source=external_attested`。它不伪造原生 Review，
  不代表 CCB 审查了代码质量，也不替代最终用户验收。
- 已由外部完成的 no-ff merge 可通过 `ccb_merge_prepare` 识别。CCB 验证唯一
  first-parent merge、两个 parent 和重算后的 merge tree，再由
  `ccb_delivery_finalize(action=external_merge)` 复核锁定事实。
- external finalize 不 commit、不 push、不删除 feature branch；成功记录
  `merge_source=external_verified`，并只释放当前 Delivery 拥有的 workspace
  或 lease。

## 27 个工具

工具按职责分为六组。读取、写入、恢复和验收各有明确边界。

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

### 实施、审查与恢复（8）

- `ccb_delivery_preflight`
- `ccb_delivery_start`
- `ccb_delivery_turn_start`
- `ccb_delivery_status`
- `ccb_delivery_turn_result_page`
- `ccb_delivery_diff_page`
- `ccb_delivery_review_recover`
- `ccb_delivery_external_publish_reconcile`

### 验收与合并（4）

- `ccb_delivery_acceptance_decide`
- `ccb_merge_prepare`
- `ccb_merge_execute`
- `ccb_delivery_finalize`

</details>

当前版本的 MCP surface 为
`tool_surface_ref:ccb_transparent_web_delivery_v3`。工具能力可能随账户状态、
灰度发布和 OpenAI 政策变化；发布证据以
[独立信任记录](https://trust.zhm20.com/ccb/)为准。

## 两份 Skill，两种执行方式

- `ccb-web-plan-authoring` **v6**：从真实项目出发澄清需求，形成决策完整、
  可审阅、可校验的 HTML 实施计划。
- `ccb-web-delivery-loop` **v8**：组织 Controller 与本地 Codex 的持续交付，
  覆盖 result-first、Review evidence 恢复、外部发布与 merge 对账、验收以及
  经授权的 Git 操作。旧版交付 ref 仍会读取当前 v8 正文。
- `primary_checkout`：直接在永久项目目录中工作，适合小项目或当前只有一项主要任务。
- `linked_worktree`：为并行任务使用独立 worktree 和分支，减少工作区互相干扰。

## 一次授权后持续推进

CCB 会把能够安全连续执行的动作放在同一轮任务里。确认实施范围、执行方式、分支、
模型、reasoning effort 和 Codex 完整权限后，实施、测试、状态轮询、证据恢复以及
已明确要求的返工可以持续推进，不反复询问同一授权。

只有在真正需要你决定时才暂停，例如最终验收、是否合并、外部 Review attestation、
external merge finalize、新范围、凭据或 MFA，以及真实外部漂移。已经确认的范围
不会被拆成多轮重复授权。

## 升级后刷新 ChatGPT 工具

本地 `/usr/local/bin/ccb service restart` 只会更新 CCB runtime，不会自动改写
ChatGPT MCP App 已保存的工具快照。安装 beta41 后：

1. 重启 CCB service。
2. 在 ChatGPT MCP App 的 **Action control** 或 **Configure Actions** 中执行
   **Refresh**；如果当前界面没有 Refresh，则按平台当前流程重新保存或发布，并使用
   **Scan Tools**。
3. 审查工具 diff，显式启用新增的 `ccb_delivery_review_recover` 和
   `ccb_delivery_external_publish_reconcile`。
4. 保存或重新发布 App，在新建 ChatGPT Web 会话中确认 27 个工具以及 Skill v6/v8
   可见后再开始交付。

## beta41 当前公开版本

当前版本：**0.1.0-beta.41**。

从 [beta41 Release](https://github.com/zhm20/code-control-bridge-public-releases/releases/tag/v0.1.0-beta.41) 下载 macOS Apple Silicon 安装包、签名与校验文件、SBOM 和用户手册。


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
- GitHub 自动生成的 `Source code (zip)` 和 `Source code (tar.gz)` 只是本发布
  入口仓库的快照，不是产品源码，也不是安装资产。安装时应下载并验证对应的 PKG。
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
