---
name: harness-syncer
description: 搜索并同步本设备中的harness，即：用户自定义skill、用户自定义agent和各类自定义指令文件（如CLAUDE.md, copilot-instructions.md, AGENTS.md）），并将他们汇集到本仓库的对应目录下，最终提交github仓库。
---

## 仓库地址

本地仓库：/Users/user/ucloud/ai-harness
远端仓库：git@github.com:brezQuill/ai-harness.git

## 必须同步

- 指令文件
  - 各级目录下的CLAUDE.md
  - 各级目录下的copilot-instructions.md
  - 各级目录下的AGENTS.md
- skill
  - 用户自定义skill（如，~/.claude/skills/目录下的skill）
- agent
  - 用户自定义agent

## 存放位置

- 指令文件
  - 不同编程智能体存放在不同目录下，如
    - CLAUDE.md存放在**claude-code**目录下，项目级别的CLAUDE.md创建同名子目录存放（如./claude-code/SecGroupFE/CLAUDE.md）
    - copilot-instructions.md存放在copilot目录下
    - AGENTS.md存放在opencode目录下
- skill
  - 统一存放在**skills**目录下
- agent
  - 统一存放在**agents**目录下

  ## 同步流程

  - WebSearch获取**CLAUDE、Copilot CLI、Open Code**的指令文件和用户自定义的skill、agent的存放位置
  - 从上述位置获取用户自定义的**指令文件（CLAUDE.md、copilot-instructions.md、AGENTS.md）、skill和agent**
  - 和本地仓库文件对比后，将新增或更新的文件复制到本地仓库的对应目录下
  - 提交并推送到远端仓库

  ## 注意事项

  - 不要全局搜索整个设备，避免不必要的性能问题。可以限定搜索范围在特定目录下，如用户主目录。
