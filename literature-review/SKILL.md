---
name: literature-review
description: "实用文献检索与管理技能。搜索PubMed→Sci-Hub下载→PyMuPDF提取文本→subagent并行精读→结构化笔记。专为有PubMed/Paper-Search/Tavily MCP的环境设计。触发词: 查文献, 文献综述, 帮我找论文, literature review, 帮我搜一下"
metadata:
  version: "1.0"
  last_updated: "2026-04-05"
---

# Literature Review — 实用文献检索与管理

从搜索到精读的完整pipeline。核心原则：**每篇论文用独立subagent读，产出结构化笔记存盘，主context只读笔记不读原文。**

## 触发条件

用户提到: 查文献, 文献综述, 帮我找论文, 搜一下XX方面的研究, literature review, search papers

## 工作流程

### Phase 1: 确认范围

向用户确认：
1. **研究主题**（具体到什么层面）
2. **目的**（写论文？做PPT？基金申请？了解现状？）
3. **需要多深**（快速了解5-10篇？还是系统性综述30+篇？）
4. **有没有已知的核心文献**（用户可能已经知道几篇重要的）

### Phase 2: 搜索

使用可用的MCP工具搜索：

```
优先级：
1. mcp__pubmed__pubmed_search_articles — 最权威，有PMID
2. mcp__paper-search__search_semantic_scholar — 覆盖面广
3. mcp__paper-search__search_google_scholar — 补充搜索
4. mcp__tavily__tavily_search — 兜底，搜网页/PDF
```

**搜索策略**：
- 用3-5组不同关键词搜索，每组取top 5-10
- 去重
- 按相关性和影响力排序
- 输出参考文献列表（DOI, PMID, 标题, 作者, 年份, 期刊）

### Phase 3: 下载

```
下载优先级：
1. mcp__paper-search__download_paper (platform: "scihub") — 主力
2. mcp__pubmed__pubmed_fetch_fulltext (PMC全文) — OA论文
3. 直接curl BMC/SpringerOpen等OA出版商
4. mcp__tavily__tavily_search 搜 "论文标题 filetype:pdf" — 兜底
```

**文件组织**：
```
项目目录/
├── papers/
│   ├── downloads/    ← 原始PDF
│   ├── text/         ← PyMuPDF提取的文本文件
│   ├── notes/        ← 结构化笔记（每组2-3篇一个文件）
│   └── figures/      ← 提取的关键图表（如需要）
├── reference_list.md ← 完整参考文献列表
└── synthesis.md      ← 跨论文综合分析
```

### Phase 4: 文本提取

下载的PDF必须提取为.txt文件才能被subagent读取：

```python
# 使用venv Python + PyMuPDF
"C:/Users/86138/.claude/skills/claude-office-skills/venv/Scripts/python.exe"

import fitz
doc = fitz.open(pdf_path)
text = ''
for page in doc:
    text += page.get_text() + '\n---PAGE BREAK---\n'
```

PMC全文（JSON格式）需要解析sections:
```python
for sec in article.get('sections', []):
    # sec has 'title', 'text', 'subsections'
```

### Phase 5: 并行精读

**关键原则：用subagent读论文，保护主context。**

每个subagent负责2-3篇论文，产出结构化笔记：

```markdown
# [第一作者 年份] 论文标题

## 基本信息
- DOI / PMID / 期刊 / 年份

## 研究设计
- 类型（RCT/meta/cohort/etc）
- 人群（纳入排除、样本量、设置）
- 干预/对照

## 核心结果（必须有具体数字）
- 主要结局：XX vs XX, P = XX
- AUC / OR / HR / RR（含95% CI）
- 敏感性 / 特异性 / cutoff值

## 关键图表描述
- Figure X: 描述内容

## 临床意义
## 局限性
## 适合放在slide上的关键句
```

### Phase 6: 综合分析

读完所有笔记后，写 `synthesis.md`：
- **跨论文对比**（同一指标在不同研究中的结果差异及原因）
- **证据链**（研究A的发现如何被研究B验证/挑战）
- **知识缺口**（还有什么不知道）
- **独立洞见**（从数据中发现的非显而易见的联系）

## 质量检查

完成后自查：
- [ ] 所有PMID/DOI真实可验证
- [ ] 具体数字与原文一致
- [ ] 没有编造数据
- [ ] 综合分析包含跨论文对比
- [ ] 文件组织清晰，方便后续使用

## 注意事项

- **永远不要编造PMID或DOI** — 如果找不到，标注"需验证"
- **图片提取后必须压缩** — PIL压缩到≤1500px再读取
- **中文输出，英文保留论文标题和技术术语**
