# GitHub / Git / Obsidian 学习笔记（2026-11-04）

> 常胜当日通话与切片练习要点整理，方便日后复习。仓库：`carlos2008ca-spec/ai-workbench-notes`。

---

## 1. GitHub 是什么

- **GitHub** = 云端代码/笔记仓库网站。
- 一个 **repository（仓库 / repo）** ≈ 一个项目文件夹 + **完整改动历史**。
- 它负责：**远程备份**、**版本记录**、可选协作（PR、Issue）。
- **不是**坚果云那种「一改就自动同步」：本地改完要自己 **commit + push** 才会上去。

---

## 2. clone / commit / push / pull（都要手动）

| 概念 | 白话 |
|------|------|
| **clone** | 把云端仓库**复制一份到本机**（新建本地副本）。知识库已有文件夹时，更推荐在文件夹里 `git init`，而不是再 clone 一套再拖文件。 |
| **commit** | 把当前改动**记成一版**，并写一句说明（谁、何时、改了什么）。 |
| **push** | 把本地已 commit 的版本**上传**到 GitHub。 |
| **pull** | 从 GitHub **拉回**最新版到本机。 |

心智模型：

```text
改文件 → add/暂存 → commit（记一版）→ push（上传）
另一台要最新 → pull
```

---

## 3. 本地知识库接入 GitHub（不搬文件）

目标：只保留本地一份知识库，用 Git 接到 GitHub。

**正确做法：**

1. 进入**已有知识库根目录**（不要新建空 clone 再拖文件进去）。
2. `git init`：让 Git「长」在知识库里面。
3. 在 GitHub 准备好远程仓（可复用 `ai-workbench-notes`，或新建专用仓）。
4. `git remote add origin <仓库HTTPS地址>`。
5. 首次：`git add .` → `git commit` → `git push -u origin main`（或 `master`，以实际分支为准）。
6. 日常：改 → commit → push；另一台：pull。

详细步骤见同仓文档：`本地知识库接入GitHub方案.md`。

---

## 4. Obsidian + Git 插件（作者 Vinzent）

### 环境（Windows）

- 知识库路径示例：`D:\AI_Workbench\KnowledgeHub`
- 本机 Git 已安装，可执行文件：

```text
C:\Program Files\Git\cmd\git.exe
```

### 插件设置

1. **设置 → 社区插件 → Git → Advanced**
2. 找到 **Custom Git binary path**
3. 填入上面的 `git.exe` 完整路径（要到文件，不是文件夹）
4. 可点 **Reload**，并**完全退出再打开** Obsidian

### 初始化

- 若提示 **No Git repository found**：在知识库里点 **Initialize repository**（= `git init`）。
- **不要**点 Clone（除非你明确要另拉一个空仓）。

### 日常用法（侧栏 Git）

- 看到 Changes：未暂存改动（如 `U` 未跟踪）。
- **Stage（暂存）** → 写 commit 说明 → **Commit** → **Push**。
- 另一台或远程有更新：**Pull**。
- 状态栏可看到分支名（当日练习为 `master`）。

---

## 5. PR 与 main 分支

- **main**（有的仓叫 **master**）：默认主分支，大家平时以它为准。
- **PR（Pull Request）**：把某条分支上的改动**申请合并**进目标分支（常是 main）。
- 当日例子：云端助手先开 PR #1 加入方案文档，再合并进 `main`；合并后网页上即可看到文件。

---

## 6. GitHub Pages（以后用）

- 仓库里放网页（如 `index.html`），在仓库 Settings 里打开 **Pages**。
- 可得到免费网址；改文件并 push 后，通常几十秒到几分钟更新。
- 适合：AI 工作台网页上线阶段（现在先记概念即可）。

---

## 7. 坚果云 vs GitHub

| | 坚果云 | GitHub + Git |
|--|--------|--------------|
| 强项 | 笔记/文件**自动同步** | **版本历史**、备份、协作（PR/Issue） |
| 同步方式 | 自动 | 手动 commit + push / pull |
| 适合 | 多端即时一致 | 可回溯、可协作、可接自动化 |

中期可逐步：以本地知识库 + GitHub 为准，减少坚果云与 Git 同时改同一目录打架。

---

## 8. Issue 当待办清单

- GitHub **Issue** 可当作仓库里的任务/待办：标题、描述、开关状态。
- 适合：学习清单、知识库改造任务、工作台下一步。
- 与 PR 配合：改完代码/文档用 PR，任务跟踪用 Issue。

---

## 9. 跨设备协同（Windows + Mac）

1. 两台都安装 Git，并配置 `user.name` / `user.email`。
2. 每台在知识库目录用同一远程 `origin`。
3. 改之前先 **pull**，改完 **commit + push**。
4. Obsidian 侧：Mac 上也装 Git 插件，并填对 Git 可执行文件路径（Mac 用 `which git` 查看）。
5. 避免两边同时大改同一文件再各自强推。

---

## 今日已完成（打卡）

- [x] GitHub 账号与仓库 `ai-workbench-notes`
- [x] Windows 安装 Git，配置身份
- [x] Cursor 连接 GitHub，验证可读仓库
- [x] 方案文档 `本地知识库接入GitHub方案.md` 合进 main
- [x] Obsidian Git：Custom Git binary path + Initialize repository
- [ ] 绑定远程并首次 push 知识库（下一步）
- [ ] Mac 侧同样打通

---

*整理日期：2026-11-04（通话日）· 教练：GitHub 切片教练*
