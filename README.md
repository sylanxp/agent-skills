# Agent Skills

个人常用的 Hermes Agent Skills 合集。

## Skills 列表

| Skill 名称 | 安装方式 | 简要说明 |
|-----------|---------|---------|
| [rtk-token-proxy](devops/rtk-token-proxy/) | `hermes skills tap add https://github.com/sylanxp/agent-skills` | RTK (Rust Token Killer) — 一个 CLI 代理，压缩终端输出以减少 LLM token 消耗 60-90% |

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
```

## 项目结构

```
agent-skills/
└── devops/
    └── rtk-token-proxy/    # RTK: CLI 输出压缩代理，减少 LLM token 消耗
```

## License

MIT
