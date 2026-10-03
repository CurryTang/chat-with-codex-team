# Chat with Codex Team

A reusable Codex skill for coordinating a recoverable group of chats: **A** receives user requests, **B** coordinates and checks evidence, and each **C** delivers one specific task. Instructions are primarily in Chinese. Model choices are configurable; existing chat settings are preserved unless the user requests an override.

This is a skill package, not a standalone program or server. It needs a Codex desktop environment exposing chat-management tools. Goal and heartbeat support are optional and depend on the live tools available in your environment. Local scheduled follow-ups require the computer and application to remain running.

B is the main working conversation; A stays a stable user entry. The current defaults use Luna low for experiments and Sol low for implementation, with Sol xhigh available for genuinely difficult work and no Astra. Routine acknowledgments and duplicate monitoring are avoided. Slow exploratory runs are stopped when early evidence makes them uncompetitive, then investigated with bounded experiments. Code should stay readable and simple, with only necessary tests.

An optional local code-graph MCP, such as [GitNexus](https://github.com/nxpatterns/gitnexus), can support cross-chat symbol and dependency navigation. One owner maintains a shared source-only index; chats exchange concise source links rather than graph dumps. Installing this skill does not install GitNexus, upload code, or enable hooks.

## Install

The package uses the [official skill format](https://developers.openai.com/plugins/build/skills): `SKILL.md`, supporting references and optional UI metadata.

This Bash example installs only the skill and refuses to overwrite an existing clone or installed skill:

```bash
set -eu
repo_dir="$HOME/chat-with-codex-team"
skill_home="${CODEX_HOME:-$HOME/.codex}"
skill_dest="$skill_home/skills/codex-chat-teams"

if [ -e "$repo_dir" ] || [ -L "$repo_dir" ]; then
  printf '%s\n' "Clone directory already exists: $repo_dir" >&2
  exit 1
fi
if [ -e "$skill_dest" ] || [ -L "$skill_dest" ]; then
  printf '%s\n' "Skill already exists; no files changed: $skill_dest" >&2
  exit 1
fi
git clone https://github.com/CurryTang/chat-with-codex-team.git "$repo_dir"
mkdir -p "$skill_home/skills"
if ! mkdir "$skill_dest"; then
  printf '%s\n' "Cannot create skill directory; refusing to overwrite it." >&2
  exit 1
fi
cp -R "$repo_dir/skills/codex-chat-teams/." "$skill_dest/"
```

The default above is `~/.codex/skills`; verify the loaded skill location in your environment. Invoke it as `$codex-chat-teams` after Codex discovers it. Installing the files does not create chats or authorize communication. A failed installation may leave the clone or a partial installation to inspect before retrying.

## 使用方式

在用户入口聊天 A 中给出目标、项目、修改范围和验收条件。B 负责协调，每个 C 对应具体交付任务。普通单聊天任务不需要建立组。

```text
用 $codex-chat-teams 建立包含新聊天的 website-release 组，本聊天作为 A。
新建 B 协调两个 C：一个修复网站前端的键盘导航，一个验证 API 回归。
项目、可修改范围和验收条件：填写实际范围与可验证标准。
授权 A→B、B→C、C→B、B→A 在此任务内派发工作、报告证据和阻塞。
不需要 Goal 或定时检查；模型沿用当前设置。
```

恢复已有组：

```text
用 $codex-chat-teams 恢复 website-release 组，核实登记和真实聊天进度，
继续已有授权范围内的任务，复用已有聊天。
```

需要监控时明确请求：

```text
为这个组的 B 设置每 15 分钟一次的 heartbeat，复用已有监控。
只在有意义的变化、完成、失败或需要我决定时通知，无变化时保持安静。
```

创建新聊天、组内通信、子代理、Goal、监控和发布等动作分别遵守用户授权范围。只有明确请求创建 Goal 才会调用相应工具。模型分工可由用户配置，不绑定固定模型 ID。

运行登记默认在 `${CODEX_HOME:-$HOME/.codex}/chat-teams/groups/`，由单一写入者维护。真实登记属于私有运行状态，不要提交到公开仓库。

## Package contents

```text
skills/codex-chat-teams/
├── SKILL.md
├── agents/openai.yaml
└── references/
    ├── role-prompts.md
    └── registry-template.json
```

Only reusable instructions and an empty template are included. See [security and privacy](SECURITY.md). Distributed under the [MIT License](LICENSE).
