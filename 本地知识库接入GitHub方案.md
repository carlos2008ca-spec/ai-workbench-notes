---
layout: default
title: 本地知识库接入 GitHub 方案
---
# 本地知识库接入 GitHub 方案

> 给常胜本人看的操作说明：把「坚果云同步的本地知识库」用 Git 接到 GitHub，只维护本地一份，GitHub 做远程备份和版本管理。

---

## 一、背景与目标

### 现在的情况

- 你在 **Windows** 和 **Mac** 两台电脑上，已经用 **坚果云** 同步同一份知识库（历史文件、Markdown 笔记等）。
- 你已经有 GitHub 账号（`carlos2008ca-spec`），也装好了本机 Git，并创建过学习仓 `ai-workbench-notes`。
- 个人 **AI 工作台** 还在搭建；搭完后的下一步，就是和 GitHub 结合、链接。

### 目标

1. **不再维护两份知识库**：只保留本地那一份（知识库文件夹本身）。
2. 用 **Git** 把这份本地知识库和 **GitHub 仓库** 打通。
3. GitHub 充当：**远程备份** + **版本历史** + 两台电脑之间的**中转站**（逐步替代坚果云的自动同步）。
4. 为后续 AI 工作台「读本地笔记 / 推网页到仓库」打好基础。

### 一句话记住

> Git 长在知识库文件夹**里面**，文件不用搬家；改完要自己 `commit` + `push`，**不会**像坚果云那样自动同步。

---

## 二、为什么不要「新建 clone 文件夹再把知识库拖进去」

| 做法 | 结果 | 建议 |
|------|------|------|
| A. `git clone` 出一个空仓文件夹，再把知识库文件复制/拖进去 | 等于多了一套目录，容易和坚果云目录搞混，还要搬文件 | **不要** |
| B. 进入**已有知识库根目录**，在里面 `git init` | Git 直接「长」在知识库里，路径不变，坚果云目录可继续用到你决定停用为止 | **推荐** |

**正确思路：** 不是「把知识库搬进 Git」，而是「让 Git 住进知识库」。

---

## 三、分步操作（按顺序做）

下面命令在 **知识库根目录** 里执行。  
Windows 用 PowerShell；Mac 用「终端」。先 `cd` 进知识库文件夹。

示例（请换成你真实路径）：

```powershell
# Windows 示例
cd "D:\Nutstore\我的知识库"
```

```bash
# Mac 示例
cd ~/Nutstore/我的知识库
```

### 步骤 1：在知识库里初始化 Git（不要新建 clone 文件夹）

```bash
git init
```

可选：看当前状态

```bash
git status
```

此时该目录下会出现隐藏的 `.git` 文件夹（一般资源管理器不显示），说明 Git 已经接管这个目录的版本记录。

若默认分支不是 `main`，可统一设为 `main`：

```bash
git branch -M main
```

### 步骤 2：在 GitHub 上准备好远程仓库

二选一：

1. **复用** 已有公开仓 [`ai-workbench-notes`](https://github.com/carlos2008ca-spec/ai-workbench-notes)（适合：知识库和 AI 工作台笔记先放一起学流程）。
2. **新建** 一个专用仓，例如 `local-knowledge-base`（适合：知识库体积大、想和练习仓分开）。

在仓库页面点绿色 **Code**，复制 HTTPS 地址，形如：

```text
https://github.com/carlos2008ca-spec/仓库名.git
```

记下这个地址，下一步要用。

> 若远程仓里已经有 README（例如 `ai-workbench-notes`），首次推送时可能提示历史不一致，见文末「常见问题」。

### 步骤 3：把 GitHub 设为远程（origin）

仍在知识库根目录：

```bash
git remote add origin https://github.com/carlos2008ca-spec/仓库名.git
```

检查是否加对：

```bash
git remote -v
```

应能看到 `origin` 指向你刚填的地址。

若加错了，可删掉重加：

```bash
git remote remove origin
git remote add origin https://github.com/carlos2008ca-spec/仓库名.git
```

### 步骤 4：首次提交并推送（体积可能较大）

**强烈建议先加 `.gitignore`（见第六节），再 `add`，避免把大附件、临时文件、密钥推上去。**

然后：

```bash
git add .
git status
git commit -m "首次：本地知识库接入 Git"
```

首次推送（默认分支按你实际为准，一般是 `main`）：

```bash
git push -u origin main
```

若本地是 `master`：

```bash
git push -u origin master
```

**注意：**

- 知识库若有几百 MB～几 GB，首次推送可能要 **几分钟到十几分钟**。
- 网络不稳可能中断；断了可以再执行一次同样的 `git push`（通常会续传或重试未完成部分）。
- 第一次向 GitHub 推送时，浏览器/本机会要求登录或授权（Personal Access Token 或 Git Credential Manager），按提示完成即可。

`-u origin main` 的意思：以后在这个分支上可以直接 `git push` / `git pull`，不用每次写远程名和分支名。

### 步骤 5：确认网页上能看到文件

打开 GitHub 仓库页面，刷新后应能看到知识库里的文件夹和 Markdown。  
能看见 = 远程备份已经立住。

---

## 四、日常使用流程（替代「自动同步」的手动节奏）

Git **不会**定时自动推。习惯可以很简单：

### 在这台电脑改完一批笔记后

```bash
cd "你的知识库根目录"
git add .
git status
git commit -m "简述今天改了什么，例如：补制造业客户拜访笔记"
git push
```

### 另一台电脑要最新版时

```bash
cd "你的知识库根目录"
git pull
```

### 最小心智模型

```text
改文件 → add（暂存）→ commit（记一版说明）→ push（上传 GitHub）
需要别人/另一台的最新版 → pull（拉下来）
```

建议：一天结束或做完一块内容就 push 一次，比攒很久再推更安全。

---

## 五、两台电脑（Windows / Mac）协同注意点

1. **每台电脑**都要在各自的知识库根目录里完成「有 `.git` + 配好 `origin`」。  
   - 第一台：按第三节 `init` → `remote` → `push`。  
   - 第二台：不要再 `git init` 覆盖；更干净的做法是：  
     - 若第二台已有坚果云同步下来的同一份文件夹且还没有 `.git`：可复制第一台的 `.git` 思路较绕，**更推荐**在第二台对空目录 `git clone` 一次到临时位置，确认无误后，再决定是否逐步停用坚果云、统一以 Git 目录为准；  
     - 或在第二台已有文件夹里：`git init` → `remote add` → `git fetch` → 按 Git 提示对齐分支（不熟时先只在一台练通，再配置第二台）。
2. **不要两边同时大改同一文件再各自强推**，容易冲突。习惯：改之前先 `git pull`，改完再 `push`。
3. 出现冲突时，Git 会标出文件里的冲突段；解决后 `add` → `commit` → `push`。初期可找教练一起看一次。
4. 坚果云与 Git **短期可以并存**，但最终目标是：以本地知识库 + GitHub 为准，逐步关掉坚果云对该文件夹的同步，避免「坚果云自动改」和「Git 手动版本」互相打架。
5. 两台电脑的 `user.name` / `user.email` 建议都配成你自己（你已在 Windows 配过：`常胜` / `carlos2008ca@gmail.com`），Mac 上也同样配一次。

---

## 六、进阶选项（可选，不阻塞主路径）

### 1. `.gitignore`：别把不该上传的东西推上去

在知识库根目录新建文件 `.gitignore`，按行写规则，例如：

```gitignore
# 系统与编辑器垃圾文件
.DS_Store
Thumbs.db
*.tmp
~$*

# 大附件 / 导出（按你实际情况改）
*.zip
*.rar
*.7z
**/附件过大/

# 敏感与本地配置
.env
*.pem
secrets/
```

改完 `.gitignore` 后：

```bash
git add .gitignore
git commit -m "添加 gitignore，排除临时与敏感文件"
git push
```

若某些大文件**已经被**提交过，只加 ignore 不够，需要另做「从历史里拿掉」的操作（以后单独学）。

### 2. GitHub Actions：定时检查或备份脚本

仓库里可加工作流（`.github/workflows/...`），例如定时跑检查、生成备份说明。  
**现在不必做**；等日常 `push` 习惯稳定后再加。

### 3. GitHub Pages：以后 AI 工作台网页上线用

若仓库里有 `index.html`（或指定 `docs/`），在仓库 **Settings → Pages** 开启，可得到免费网址；改文件并 `push` 后通常几十秒到几分钟更新。  
这是「网页项目」阶段的事，和知识库备份可同一账号、不同仓库，也可以后期再拆。

---

## 七、和 AI 工作台的衔接（下一步）

| 阶段 | 做什么 |
|------|--------|
| 现在 | 本地知识库 ↔ GitHub 打通，养成 `commit` + `push` |
| 工作台搭好后 | 工作台读写本地知识库路径；需要协作/备份时走同一套 Git 远程 |
| 再往后 | 网页/小工具进仓库；可用 Pages 发布；Issue/PR 管任务；需要时再请主 Bot / 方案架构师用云端改代码 |

**原则（和教练约定一致）：** 学习用切片动手；正式往你仓库大批量改代码时，说明交接并交给主 Bot 或方案架构师，本教练不擅自推业务代码。

---

## 八、常见问题（首次对接时）

### Q1：远程已有 README，本地也有文件，`push` 被拒？

可先拉再推（理解含义后再做）：

```bash
git pull origin main --allow-unrelated-histories
# 如有冲突，解决后
git add .
git commit -m "合并远程初始 README 与本地知识库"
git push -u origin main
```

或新建一个**空的**专用知识库仓库（不要勾选 README），再 `push`，冲突更少。

### Q2：推送太大失败？

- 检查是否误传视频、安装包；用 `.gitignore` 排除后重新提交策略（大文件已进历史时需单独处理）。
- 网络稳定时段再 `git push`。
- 单文件过大（GitHub 对单文件有约 100MB 限制）需用 Git LFS 或不要纳入版本库。

### Q3：和坚果云一起用会怎样？

短期可以，但坚果云可能同步 `.git` 或产生冲突副本。更稳妥：Git 流程跑通后，对该文件夹**停止坚果云同步**，改由 GitHub 中转两台电脑。

---

## 九、最小检查清单（做完打勾）

- [ ] 已在知识库根目录 `git init`（没有另建 clone 空文件夹再拖文件）
- [ ] GitHub 上有目标仓库，并复制了 HTTPS 地址
- [ ] `git remote add origin ...` 成功（`git remote -v` 能看到）
- [ ] 已按需写好 `.gitignore`
- [ ] 首次 `commit` + `push` 成功，网页上能看到文件
- [ ] 记住日常：`add` → `commit` → `push`；另一台：`pull`
- [ ] （可选）Mac 上也配好 `user.name` / `user.email`
- [ ] （中期）决定何时停用坚果云对该目录的同步

---

*文档版本：2026-11-04 · 仓库：`ai-workbench-notes` · 作者备注：随练习可继续增补截图与真实路径。*
