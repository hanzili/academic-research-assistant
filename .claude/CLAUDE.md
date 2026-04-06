# Academic Research Assistant

学术科研工作技能包。

## Skills 概览

| Skill | 用途 | 触发词 |
|-------|------|--------|
| `literature-review` | 文献检索→下载→精读→结构化笔记 | 查文献, 文献综述, 帮我找论文, literature review |
| `presentation-logic` | 学术演讲逻辑梳理（必须在做PPT前运行） | 帮我梳理逻辑, 演讲大纲, PPT逻辑, presentation outline |
| `html-slides` | 从确认的outline生成HTML学术slides | 做PPT, 生成slides, make slides |

## 路由规则

1. **做PPT的完整流程**: literature-review → presentation-logic → html-slides（三步必须按顺序）
2. **只查文献**: 直接用 literature-review
3. **已有文献要做PPT**: 跳过 literature-review，从 presentation-logic 开始
4. **逻辑已确认要生成slides**: 直接用 html-slides

## 环境依赖

- Python 3.12+（需要 PyMuPDF, python-pptx, Pillow）
- Node.js + Playwright（用于 HTML slides 截图）
- LibreOffice（可选，用于转 PDF）
- MCP servers: pubmed, paper-search (含Sci-Hub下载), tavily

## 输出目录

所有输出默认存放在工作目录下。
