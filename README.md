# Code Control Bridge

**ChatGPT Web × local Codex：从计划到交付的一条透明工作流。**

Code Control Bridge 让 ChatGPT Web 负责需求澄清、计划制定、进度总控和
diff review，再把经过确认的实施任务交给本地 Codex。每一步都有明确状态、
可核对摘要和独立授权点，适合希望持续推进真实项目、又不想在多个窗口之间
手工搬运上下文的个人开发者。

## 它能做什么

- 使用 Skill v6 在 ChatGPT Web 中形成决策完整的 HTML 实施计划。
- 通过分块 SHA-256 校验，把已接受的计划可靠地写入本机 Plan Inbox。
- 选择直接使用主 checkout，或为任务建立隔离的 linked worktree。
- 由本地 Codex 实施和验证，再回到 Web 完成 diff review、返工与接受决策。
- 只有在用户明确授权后，才执行 commit、普通 push 或 merge。
- beta35 提供 25 个工具、delivery ledger v8、双 execution mode，以及
  Skill v6/v7 的可配置 Markdown 正文。

当本地 Codex 周度用量紧张时，可以把适合 Web 完成的规划、审查和流程控制
放到 ChatGPT Web，让本地 Codex 更专注于实现。CCB 不会增加、转移、绕过或
保证任何 OpenAI 配额；可用模型和上限取决于账户、灰度发布与 OpenAI 政策。

## 当前版本

- Version: `0.1.0-beta.35`
- Release: <https://github.com/zhm20/code-control-bridge-public-releases/releases/tag/v0.1.0-beta.35>
- Package: `0.1.0-beta.35.pkg`
- Package SHA-256: `9814494c93629e6c25a787213968f904e3e7fcda501aacd0b5e7cde374d3bd11`
- Buyer verifier SHA-256: `b7ceb7c5e9b6700ce89638cedf2dfd387de765bc8aa53de505a0816e0c9044fc`
- Release public-key SHA-256: `796d69af8f8ef6b74551573eded3c54474b02e6f1e464889dd068c1b116a8dec`

## 官方入口

- 产品与购买：<https://ccb.zhm20.com>
- 下载：<https://downloads.zhm20.com/ccb>
- 独立信任记录：<https://trust.zhm20.com/ccb>
- 支持：`eric.zhm2018@gmail.com`

## 下载前须知

本仓库只包含公开说明和二进制 Release 资产，不包含产品源码、内部证据、
凭据或签名材料。

当前 PKG 未经 Apple 签名、公证或 staple。安装前请从独立信任渠道核对
verifier 与发布公钥指纹，再使用 Release 中的 `verify-ccb-release.sh`、
release statement 和 detached signature 验证 PKG。

已发布资产不可覆盖；任何修复都使用新版本。
