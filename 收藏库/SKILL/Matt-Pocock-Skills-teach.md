---
title: "Matt Pocock Skills - teach"
url: "https://github.com/mattpocock/skills/blob/main/skills/productivity/teach/SKILL.md"
source: "GitHub / mattpocock/skills"
published: "2026-05-27"
archived: "2026-10-07"
category: "SKILL"
tags: [Agent Skills, SKILL.md, 教学, 学习方法, Claude Code]
lang: en
status: ok
---

# Matt Pocock Skills - teach

> 一句话定位：一个「让 Agent 当老师」的有状态教学技能，把当前目录变成长期教学工作区，用文件承载学习状态，把知识、技能与智慧分层推进。

## 摘要

teach 是 Matt Pocock 的 skills 仓库里一个有状态教学 Skill（`skills/productivity/teach/SKILL.md`，MIT，需用户显式触发）。它把当前目录当作教学空间，用文件承载学习状态：`MISSION.md` 记录学习动机并锚定全部教学，`RESOURCES.md` 收集高可信资源，`learning-records/*.md` 像架构决策记录一样沉淀关键洞见，`lessons/*.html` 是核心产出（每课一个自包含 HTML 文件），`reference/*.html` 存速查表与术语表，`assets/*` 放跨课复用组件，`NOTES.md` 记用户偏好。方法论把学习分三层：知识（低难度获取，必须来自可信资源，明确不信任模型自身记忆）、技能（用合意困难、检索练习、间隔与交错练习建立长期记忆强度）、智慧（引导用户进入社区做真实检验）。课时要求短小、排版精美（对标 Tufte）、可打印、每次只给一个可积累的具体收获，并落在近侧发展区内。目录内另附 MISSION / RESOURCES / LEARNING-RECORD / GLOSSARY 四个格式模板。

## 要点

- **工作区即学习状态**：`MISSION.md`（学习动机，所有教学的锚点）、`RESOURCES.md`（可信资源清单）、`learning-records/*.md`（编号 `0001-` 递增，记录非显而易见、日后可能被修订的关键洞见）、`lessons/*.html`（核心产出）、`reference/*.html`（速查表、算法、术语表）、`assets/*`（跨课复用组件）、`NOTES.md`（用户偏好草稿本）。目的是让同一主题跨多次会话持续学习。
- **知识 → 技能 → 智慧三层模型**：资源库尚未充实前，首要任务不是讲课而是找高质量资源；知识获取阶段「难度是敌人」，会吃掉理解所需的工作记忆；技能阶段「难度是工具」；智慧只能来自真实世界互动，默认姿态是找到高声誉社区并引导用户加入（用户明确拒绝则尊重）。使命不清时，第一件事是追问用户「为什么想学这个」。
- **流利度 ≠ 存储强度**：流利度是当下的即时回忆，会制造虚假的掌握感；存储强度才是真目标，靠合意困难实现——检索练习（凭记忆回想）、间隔（把练习分散到不同时间）、交错（只用于技能练习，混合相关但不同的主题）。
- **课时规格**：每课一个自包含 HTML 文件，命名 `0001-<dash-case-name>.html` 递增编号；要求排版精美可复看、短到不超过学习者极小的工作记忆、与使命强绑定、落在近侧发展区，每次给出一个可积累的具体收获，并用 HTML 锚点互链其他课时与参考文档。参考文档比课时更常被回看，应做成压缩后的精华速查形态；术语表一旦建立，之后每课都必须遵守。
- **每课两条硬性要求**：推荐该主题最高质量、最可信的首要信源（供用户阅读或观看）；提示用户可就任何不清楚之处向作为「老师」的 Agent 追问。知识性论断应散布指向外部资源的引用链接以提升可信度。
- **测验与组件复用**：测验每个选项的字数（可能的话连字符数也）严格一致，避免格式泄露答案；写课时前先读 `./assets/` 并从已有组件搭建，新出现的可复用件写入 `assets/` 而非内联，共享样式表是每个工作区最先积累的组件。

## 关联

- `SKILL/Anthropic-Skills.md` — 同属 Agent Skills 生态：那个仓库给出 SKILL.md 规范与官方示例，本条是第三方按该规范编写的具体技能。
- `SKILL/Superpowers.md` — 同为交付给编码代理的技能，都主张用流程与文件系统承载状态，而不是依赖一次性提示。
- `AI应用/OpenMAIC.md` — 都面向「AI 做教育/课程生成」，一个偏技能化的教学工作流，一个偏多智能体课程生成产品。

## 注意

- 本次收录的是仓库中的**单个技能文件**；父仓库 `mattpocock/skills`（描述为「Skills for Real Engineers」）还含 `engineering`、`productivity`、`misc`、`deprecated` 等分类下的其他技能，未逐一收录。
- GitHub 页面抓取超时，正文经 `raw.githubusercontent.com` 上的原文取得；`published` 取该文件首次提交日期 2026-05-27，最新提交为 2026-08-19（移除全仓 em-dash）。
- 技能引用的 `MISSION-FORMAT.md`、`RESOURCES-FORMAT.md`、`LEARNING-RECORD-FORMAT.md`、`GLOSSARY-FORMAT.md` 与 `agents/` 目录仅确认存在，内容未展开。
- 仓库 API 快照：278,460 stars / 23,320 forks，MIT 许可，主页 https://aihero.dev/skills。
- 2026-10-07 复核：GitHub 页面与提交历史均重新核对，本文件最新提交仍是 2026-08-19（140 行 / 9.28 KB），正文相对首次收录无变化。
