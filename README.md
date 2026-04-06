# academic-research-assistant

学术科研 [Claude Code](https://docs.claude.com/en/docs/claude-code/overview) 技能包。覆盖文献检索、演讲逻辑梳理、幻灯片生成的完整流程。

## Skills 概览

| Skill | 用途 | 触发词 |
|-------|------|--------|
| `literature-review` | 文献检索 → 下载 → 精读 → 结构化笔记 | 查文献, 文献综述, 帮我找论文 |
| `presentation-logic` | 学术演讲逻辑梳理（做PPT前必须运行） | 帮我梳理逻辑, 演讲大纲, PPT逻辑 |
| `html-slides` | 从确认的 outline 生成 HTML 学术 slides | 做PPT, 生成slides |

## 工作流程

```
literature-review → presentation-logic → html-slides
   (查文献)          (梳理逻辑)           (生成PPT)
```

- 只查文献：直接用 `literature-review`
- 已有文献要做PPT：从 `presentation-logic` 开始
- 逻辑已确认：直接用 `html-slides`

## 安装

```bash
claude skill install --from hanzili/academic-research-assistant
```

或手动复制到 `~/.claude/skills/academic-research-assistant/`。

## 环境依赖

- Python 3.12+（需要 PyMuPDF, python-pptx, Pillow）
- Node.js + Playwright（用于 HTML slides 截图）
- LibreOffice（可选，用于 HTML 转 PDF）
- MCP servers: pubmed, paper-search (含 Sci-Hub), tavily

## 配套技能包

本技能包设计为配合以下技能包使用：
- [academic-research-skills](https://github.com/Imbad0202/academic-research-skills) — 深度研究、论文写作、同行评审
- [claude-office-skills](https://github.com/anthropics/skills) — Office 文档操作（PPTX/DOCX/XLSX/PDF）
