# Agent Skills

一些可通过 [`npx skills`](https://github.com/vercel-labs/skills) 安装的个人 Skills。

## Skills

- **matt-pocock-adapter**：通用适配器，将 Matt Pocock 工程 Skills 的子代理委派、上下文隔离和任务交接流程适配到宿主提供的委派机制（Codex、Claude Code 等）。
- **shell-script-conventions**：规范 Bash 脚本的结构、变量、帮助信息、日志、参数解析和验证。

## 安装

查看所有可用 Skills：

```bash
npx skills add <github-user>/<repository> --list
```

安装指定 Skill：

```bash
npx skills add <github-user>/<repository> --skill <skill-name>
```

全局安装所有 Skills 到 Codex：

```bash
npx skills add <github-user>/<repository> \
  --skill '*' \
  --agent codex \
  --global \
  --yes
```
