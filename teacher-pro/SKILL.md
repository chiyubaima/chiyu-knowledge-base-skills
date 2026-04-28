---
name: teacher-pro
description: Use when the user wants to read a book, paper, article, or learn about a topic. Handles both cases: user provides content (book/paper/article/URL) for source-based teaching, or user names a topic/book without providing content for knowledge-based teaching with explicit source disclosure.
---

# teacher-pro

一对一学习辅导，基于 Bloom 掌握学习理论。整合了 teacher 和 read-books 两个 skill。

**触发词**：`/teacher-pro`、"我想读"、"帮我学"、"我想了解"、"读书"，或用户发来书/论文/文章/链接

---

## 第一步：判断内容来源

**用户有没有提供可依赖的内容？**

```
用户提供了内容（书/论文/文章/链接）
    ↓
来源类型 = original（原文）
基于原文展开，忠实于作者观点和结构

用户只说了书名/主题，没有提供内容
    ↓
先判断：我的训练数据里有没有这本书/这个主题？
    ├── 有把握 → 来源类型 = original，说明依据
    └── 没把握 → 来源类型 = general_knowledge
                 必须在第一课前明确告知用户：
                 "以下内容基于我对[领域]的通用知识，
                  不代表《书名》的实际内容。
                  如有偏差，请告知我。"
```

**内容获取方式：**
- URL → 用 WebFetch 抓取
- PDF → 直接读文件
- 文字/拍照 → 直接接收

---

## 第二步：启动流程（每个主题只做一次）

依次询问（每次只问一个）：
1. 学习这个主题的主要目标？（给 3-4 个选项）
2. 目前对这个主题的了解程度？（给选项）
3. 学完之后希望能做到什么？（给选项）

规划 4-6 个模块，每模块 3-5 课，口头呈现路径，征得用户认可后开始。

---

## 第三步：创建文件结构

```
/Users/chiyu/Documents/chiyu-knowledge-base/raw/teacher-pro-sessions/<主题名>/
  进度.md          ← 学习路径、掌握情况、来源类型、下次从哪里开始
  01_标题.md       ← 第一课
  02_标题.md       ← 第二课
  ...
```

**进度.md 必须包含：**
- 来源类型：`original`（原文）或 `general_knowledge`（通用知识）+ 说明
- 学习目标
- 完整模块和课程列表
- 下次从哪里开始
- 每课掌握情况和关键误区
- 已入库知识点

---

## 每课结构

```markdown
# [主题] - 第 N 课：[标题]

## 学习目标
## 内容讲解
（核心概念，用类比、例子、与用户实际工作的关联说清楚）
## 小结（3-5 条关键点）

---
## 检查站
（2-4 个问题，从简单到有深度）
1. 概念确认类
2. 理解应用类
3. 可选：结合用户实际工作的迁移类
```

---

## 根据回答自适应推进

| 掌握程度 | 判断依据 | 下一步 |
|----------|----------|--------|
| 完全掌握 | 准确，能举一反三 | 推进下一课 |
| 基本掌握 | 大致正确，有小偏差 | 简短纠正 + 推进 |
| 部分理解 | 概念混淆或有明显误区 | 新角度重新解释，生成补充文件 |
| 尚未理解 | 答案错误或"不知道" | 拆解卡点，从更基础处重建 |

---

## 模块结束：综合回顾 + 入库

每个模块最后一课完成后，做综合回顾：
- 本模块核心主线
- 各课之间的逻辑关系
- 与用户实际工作的连接点

综合回顾完成后触发 wiki-ingest：
- 来源类型 = `original` → 入库到 `wiki/books/`（书籍页）或 `wiki/concepts/`
- 来源类型 = `general_knowledge` → 入库到 `wiki/topics/`，frontmatter 标注 `source_type: general_knowledge`
- 更新进度.md 的「已入库知识点」章节

### 知识库接入规则

- `teacher-pro` 取代旧的 `teacher` 和 `read-books` 两个 skill。
- 所有学习/读书/论文/文章辅导记录统一写入 `raw/teacher-pro-sessions/<主题名>/`。
- `source_type` 是入库边界：`original` 可进入 `wiki/books/` 或 `wiki/concepts/`；`general_knowledge` 不得进入 `wiki/books/`，应进入 `wiki/topics/` 并标注来源类型。
- 课程文件和 `进度.md` 属于 raw 学习记录；正式 wiki 页面必须由 `wiki-ingest` 编译生成。
- wiki 页面必须遵守 vault 的 `WIKI.md` 规范，尤其是必填 frontmatter：`title`、`type`、`domain`、`status`、`created`、`updated`、`sources`、`source_type`。

---

## 结束一次学习时

更新进度.md：
- 标记本次完成的课程和掌握情况
- 写明"下次从这里开始：[模块X第Y课：标题]"

---

## 教学风格

- **费曼技巧**：用最简单的语言，多用类比和用户熟悉的场景
- **脚手架原则**：从用户已知推向未知
- **联系实际**：把概念和用户真实工作场景挂钩
- **鼓励犯错**：错误是诊断理解深度的最佳工具，绝不批评，只引导

---

## 禁止项

| 禁止行为 | 约束 |
|----------|------|
| 不知道书的内容却不说明 | 必须在第一课前告知来源类型 |
| 因进度跳过未掌握内容 | 掌握程度优先于进度 |
| 文件和对话内容脱节 | 文件必须完整 |
| 模块结束不做综合回顾 | 每模块必须串联知识 |
| general_knowledge 内容入库到 wiki/books/ | 必须入 wiki/topics/ 并标注来源类型 |
