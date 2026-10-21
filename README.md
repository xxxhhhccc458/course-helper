# course-helper

> 一份面向大学生课程学习的 Agent Skill：把讲义、课件、PDF 整理成复习大纲，解答作业题，维护跨会话的学习进度，并沉淀可复用的提示词库。

`course-helper` 不是一个程序，而是一份**给 AI Agent 看的方法论文档**（Markdown）。
任何能读取 Markdown 指令的 Agent 都能用：DeepSeek Harness、Claude Code、Codex CLI、Cursor、Cline……

---

## 它能做什么

| 场景 | 举例 |
|---|---|
| 整理复习大纲 | 「把这三章的 PDF 整理成能背的复习大纲，带公式速查表和易错点」 |
| 讲解课程概念 | 「用我课件的记号体系讲一下极大似然估计，给完整推导」 |
| 解答作业题 | 「HW2 第 3 题，先看我的思路对不对，再给完整答案」 |
| 维护学习进度 | 「记录一下今天学到哪、哪块还不熟」→ 生成可交接的进度文档 |
| 管理提示词库 | 「把这条提示词存进库里，抽成变量」 |

**不适用于**纯代码工程项目（更适合用工程类技能）。

---

## 核心设计：A 层 / B 层

这是本技能最关键的一点——**方法和项目事实分开存**：

```
A 层（本仓库，可跨项目复用）
  SKILL.md      通用方法：怎么取舍、怎么排版、怎么出题、怎么记录进度
  ├─ 不含任何具体课程的名字、页码、编号
  └─ 升级时所有项目一起受益

B 层（放在你的项目里，每个课程一份）
  <项目>/.dsh/skills/course-notes-profile/
  ├─ SKILL.md         索引 + 模块路由表
  ├─ 资料源.md         资料在哪、怎么刷新清单
  ├─ 定位.md           编号体系、页码口径、记号表
  ├─ 取舍.md           本课程不考什么、勘误登记
  ├─ 产物.md           大纲 / 进度文档写在哪、用什么模板
  └─ 提示词.md         本项目专用提示词
```

好处：换课程不用改技能本体；技能更新不会覆盖你的课程档案；B 层的「取舍特例」优先级高于 A 层通用判据。

---

## 目录结构

```
course-helper/
├── SKILL.md     ← 主文件：完整方法论（含 YAML frontmatter）
├── 提示词.md     ← 通用提示词库，P-NN 编号，变量用 {占位符}
├── AGENTS.md    ← 给 Codex 等读 AGENTS.md 的 Agent 的入口
├── README.md    ← 你在看的这份
└── LICENSE      ← MIT
```

---

## 安装与使用

### DeepSeek Harness（DSH）

把整个 `course-helper` 目录放到下面任一处，重启/刷新后即可在技能列表看到：

```bash
# 用户级：所有项目可用
~/.dsh/skills/course-helper/

# 项目级：只在该项目可用
<项目根目录>/.dsh/skills/course-helper/
```

目录名必须与 `SKILL.md` 里 frontmatter 的 `name` 完全一致（都是 `course-helper`），首行必须是 `---` 且无 BOM。

### Claude Code

```bash
git clone https://github.com/<你的用户名>/course-helper.git ~/.claude/skills/course-helper
```

### Codex CLI / Cursor / 其他通用 Agent

这类工具通常读 `AGENTS.md`、`.cursorrules` 或自定义指令：

- **Codex CLI**：把本仓库 `AGENTS.md` 的内容，或本目录放进项目根目录，Codex 会自动读取；也可在 `AGENTS.md` 里加一行 `Read course-helper/SKILL.md before course tasks.`
- **Cursor**：Settings → Rules → 把 `SKILL.md` 内容粘进去，或在项目里引用
- **其他**：直接把 `SKILL.md` 全文作为系统提示 / 自定义指令即可，它不依赖任何工具接口

### 手动（任何聊天窗口）

打开 `SKILL.md`，全文复制粘贴到对话开头，再说你的需求。

---

## 提示词库用法

`提示词.md` 里是编号 `P-01`…`P-06` 的现成提示词，每条都抽好了变量：

```text
我正在学习{课程名称}这门课。以下这些PPT是老师今天上课所使用的课件。……
```

用时把 `{课程名称}` 之类替换成实际值即可。当前收录：PPT 讲解、作业思路辅导（多轮）、例题提炼、代码逐题批改、作业代写（整份交付）、大纲整理（按 PDF 目录成文）。

---

## 常见问题

**Q：为什么一定要 B 层？直接问不行吗？**
A：一次性问当然行。但同一门课要持续一学期，B 层省的是「每次重新告诉 AI 资料在哪、编号怎么算、什么不考」的成本，也让下次换个会话能无损接手。

**Q：不想建 B 层能只用 A 层吗？**
A：能，`SKILL.md` 第 1 节有初始化指引；不初始化时它会按通用判据干活，只是拿不到课程特例。

**Q：SKILL.md 里为什么有那么多「不得」「必须」？**
A：技能是给模型执行的行为约束，模糊措辞会导致每次输出风格漂移。这些硬规则是踩坑后写下的。

---

## 贡献

欢迎提 Issue 或 PR：补充题型模板、改进取舍判据、修错别字都可以。
改动 `SKILL.md` 时请保持两点：① 只写可迁移的方法，不写具体课程事实；② 保持 frontmatter 的 `name` / `description` 有效。

## License

[MIT](LICENSE) © 2026 xxxhhhccc458
