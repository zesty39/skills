# 语言规范

- 与用户交流时使用中文。
- 编写面向用户的文档时使用中文，包括 Markdown、Word 等格式。
- 编写代码及代码注释时使用英文。

# 拼写检查

- 编写或修改代码、代码注释及文档时，检查拼写并修正发现的错误。
- 无法确认某个词语或名称的正确拼写时，先向用户询问，确认后再继续。

# 提问方式

- 需要向用户澄清需求、询问偏好或提供选项时，优先使用交互式提问工具（Codex 为 `request_user_input`，Claude Code 为 `AskUserQuestion`，其他 agent 使用等价能力）。
- 禁止使用任何异步提问工具（Codex 为 `request_user_input_async`，其他 agent 如有同类工具同样禁止）。

# Git 提交

- 提交代码时，禁止附带 AI 署名（Claude Code 会默认添加 `Co-Authored-By: Claude Code <noreply@anthropic.com>`）。
