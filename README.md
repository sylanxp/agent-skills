# Agent Skills

个人常用的 Hermes Agent Skills 合集。

## Skills 列表

| Skill 名称 | 安装方式 | 简要说明 |
|-----------|---------|---------|
| [rtk-token-proxy](devops/rtk-token-proxy/) | `hermes skills tap add https://github.com/sylanxp/agent-skills` | RTK (Rust Token Killer) — 一个 CLI 代理，压缩终端输出以减少 LLM token 消耗 60-90% |
| [darwin-skill](darwin-skill/) | `hermes skills tap add https://github.com/sylanxp/agent-skills` | Darwin Skill 2.0 — 自主 skill 优化器，对 SKILL.md 进行 9 维评分 +  hill-climbing 优化 + 人在回路验证 |

## 快速开始

### 安装所有 Skills

```bash
hermes skills tap add https://github.com/sylanxp/agent-skills
```

安装后重新加载 skills：

```bash
hermes skills reload
```

### 查看某个 Skill

```bash
hermes skills inspect rtk-token-proxy
hermes skills inspect darwin-skill
```

## 项目结构

```
agent-skills/
├── devops/
│   └── rtk-token-proxy/    # RTK: CLI 输出压缩代理，减少 LLM token 消耗
└── darwin-skill/           # 自主 skill 优化器（9维评分 + hill-climbing + 人在回路）
```

## License

MIT
