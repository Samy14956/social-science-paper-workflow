# social-science-paper-workflow

# 社科论文全流程技能包

面向 WorkBuddy / Claude Code 等支持 Skill + MCP 的智能体环境，提供从选题、文献、实证方法到写作、评审、排版的社会科学论文全流程能力。技能包**全部文件来自本仓库自身**，安装后不依赖外部私有服务；文献检索类技能所需的 API 密钥由使用者自行配置。

## 一、包含内容

| 组件 | 类型 | 作用 |
|---|---|---|
| 社科量化研究方法库 | Skill | 量表选择与改编、信效度、EFA/CFA、共同方法偏差、测量不变性、SEM、中介调节、因果推断审计、DID/PSM/IV/RDD、统计结果表、问卷报告模板（10 个方法分册） |
| deep-research | Skill | 多智能体深度调研：问题拆解、来源核验、系统综述、APA 7 排版、交接论文写作 |
| zotero | Skill | Zotero 文献库检索与管理（需自备 API 密钥） |
| literature-review | Skill | 多引擎文献检索与综述（需 `requests`） |
| paper-tools-mcp | MCP Server | 24 个工具：字数/引用/结构校验、DOCX 版式五件套、AIGC 风险评估、文献卡、Excel 校验、pandoc 排版链 |

> 说明：写作编排主干（生成器—评估器、多视角评审、修订门禁）由本地既有的 `academic-paper` / `academic-pipeline` / `academic-paper-reviewer` 提供，本包与之互补，不重复内置。

## 二、前置条件

- Python ≥ 3.11
- `uv`（包管理器，https://docs.astral.sh/uv/）
- `pandoc`（排版链需要，https://pandoc.org/）

## 三、一键安装

在仓库根目录执行（跨平台，Windows / macOS / Linux 均可）：

```bash
python install.py --with-deps
```

安装器会：
1. 将 4 个技能复制到 `~/.workbuddy/skills/`（同名目录先备份，不覆盖历史内容）；
2. 将 MCP 源码复制到 `~/.workbuddy/mcp-servers/paper-tools-mcp/`；
3. **合并** `~/.workbuddy/mcp.json`，仅新增 `paper-tools-mcp` 条目，保留全部既有条目；
4. 执行 `uv sync` 安装运行依赖。

自定义路径：

```bash
python install.py --skills-dir DIR --mcp-dir DIR --mcp-json FILE --with-deps
```

预览模式（不落盘）：

```bash
python install.py --dry-run
```

安装后**重启 WorkBuddy**，并在连接器管理页信任 `paper-tools-mcp`（新 MCP 不会自动激活）。

## 四、可选：启用语义检索

`search_blocks` 默认使用关键词匹配。如需向量语义检索：

```bash
cd ~/.workbuddy/mcp-servers/paper-tools-mcp
uv sync --extra semantic
```

此操作会下载 `sentence-transformers` 及 torch 模型（约数百 MB）。未启用不影响其余 23 项工具。

## 五、专有配置

- **zotero**：在技能目录按 `SKILL.md` 说明配置 `ZOTERO_API_KEY`、`ZOTERO_USER_ID`。
- **paper-tools-mcp 沙箱**：编辑 MCP 配置中的 `PAPER_TOOLS_BASE_DIR`（允许读写的根目录）、`PAPER_TOOLS_TEMPLATE_DIR`（模板目录）、`PAPER_TOOLS_SKILLS_DIR`（minimax 系列技能根目录）。详见 `mcp/paper-tools-mcp/README.md`。

## 六、合规与使用红线

各组件来源、许可与适配修改详见 `NOTICE.md`。要点：

- `paper-tools-mcp` 采用 Apache-2.0 许可。
- `社科量化研究方法库` 改编自 Drchronx/ai-agent-research-starter-kit，原许可 PolyForm Noncommercial，**限非商业的学习研究使用**，禁止商用。
- `deep-research` / `zotero` / `literature-review` 来自 WorkBuddy 用户导出包，请遵循其原始许可。
- 全文据插件运行环境离线使用；文献、数据、引用须人工复核，不得把模型输出直接作为可回溯事实。

## 七、目录结构

```
.
├── install.py                       # 跨平台安装器
├── README.md                        # 本文件
├── skills/                          # 四个 Skill
│   ├── 社科量化研究方法库/
│   ├── deep-research/
│   ├── zotero/
│   └── literature-review/
├── mcp/
│   └── paper-tools-mcp/             # MCP Server 源码
└── config/
    └── mcp-snippet.example.json     # mcpServers 配置片段样例
```
