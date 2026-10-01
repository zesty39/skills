# Agent Skills

一些可通过 [`npx skills`](https://github.com/vercel-labs/skills) 安装的个人 Skills。

## Skills

- **matt-pocock-adapter**：通用适配器，将 Matt Pocock 工程 Skills 的子代理委派、上下文隔离和任务交接流程适配到宿主提供的委派机制（Codex、Claude Code 等）。
- **shell-script-conventions**：规范 Bash 脚本的结构、变量、帮助信息、日志、参数解析和验证。

## 安装

查看所有可用 Skills：

```bash
npx skills add zesty39/skills --list
```

全局安装 Skills：

```bash
npx skills add zesty39/skills -g
```

仓库根目录的 `global-instructions.md` 是可选的全局指令片段（语言规范、拼写检查、提问方式、Git 提交），独立于各 Skill，不随 `skills add` 安装。如需启用，将其内容合并到全局指令文件：

- Codex：`~/.codex/AGENTS.md`
- Claude Code：`~/.claude/CLAUDE.md`
- 其他 agent：对应的全局指令文件
