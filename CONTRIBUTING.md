# 开源仓库贡献流程规范（每轮一个完整闭环）

> 适用场景：向他人管理的仓库贡献代码/文档（哪怕只是一个 txt），重点考察**流程规范**。
> 一轮 = 一个 Issue + 一个 PR + 一次合并，产出一组干净的 git 成果。

## 0. 全景链路（管理人提示的补全版）

```
发现缺陷/改进点 → 提 Issue → （维护者确认）→ 建分支 → 改动 + 规范提交
→ Push → 发 Pull Request → Code Review → 按意见追加提交 → 维护者合并
→ Issue 自动关闭 → 删除远端分支 → 本轮结束
```

## 1. 铁律（每轮自查）

1. **一个 PR 只解决一个 Issue**（原子性；两个问题就开两个 Issue 两个 PR）。
2. **先 Issue 后代码**：未经 Issue 确认就甩 PR 是大忌（尤其文档类，可能和维护者规划冲突）。
3. **不动别人的历史**：review 意见用**追加 commit** 响应，禁止 force-push 改写已被 review 的提交。
4. **提交历史干净**：PR 内每个 commit 都应能独立编译/自洽；合并前若维护者要求 squash 则配合。
5. **每轮结束留存 git 成果**：`git log` + PR 链接 + 合并截图，即"本轮 git 成果"。

## 2. Issue 规范

- 标题带类型前缀：`[Bug]`、`[Docs]`、`[Feature]`、`[Question]`。
- 正文四段式（Bug 类）：
  1. **现象**：一句话说清错在哪。
  2. **复现步骤**：1、2、3 步可复现。
  3. **期望行为** vs **实际行为**。
  4. **环境**：版本/分支/commit 号、系统。
- 文档类可简化为：缺什么、为什么需要、建议放哪。

## 3. 分支与提交规范

- 分支命名：`fix/#Issue号-短横线简述` 或 `docs/#Issue号-简述`（例：`docs/#3-readme-usage`）。
- Commit 信息用 Conventional Commits 格式：`类型: 描述 (#Issue号)`
  - 可用类型：`feat`（新功能）/ `fix`（修缺陷）/ `docs`（文档）/ `refactor` / `chore` / `test`
  - 例：`docs: 补充构建环境说明 (#3)`
- 一个改动一个 commit；不要把无关的格式化、空白改动混进来。

```bash
# 从最新主分支切出（每次开工先同步）
git checkout main && git pull
git checkout -b docs/#3-readme-usage
# ...编辑文件...
git add <文件>
git commit -m "docs: 补充构建环境说明 (#3)"
git push -u origin docs/#3-readme-usage
```

## 4. Pull Request 规范

- 标题：与 commit 主题一致（`docs: 补充构建环境说明`）。
- 描述必含三块：
  1. **Closes #3**（让合并自动关 Issue；多个写多行）。
  2. **改动说明**：做了什么、为什么（关联 Issue 里的讨论）。
  3. **自查清单**：☐ 只解决该 Issue ☐ 本地验证过 ☐ 无无关改动 ☐ commit 信息合规。
- PR 发出后若 CI 失败，先自己修到绿再 @ 维护者。

## 5. Code Review 礼仪

- 维护者评论后**逐条回复**：改了就回复 "done + 新 commit 号"；不认同就摆理由讨论，不沉默。
- 每轮修改 = 追加一个 commit（`fix: 按 review 意见调整 XX`），让维护者能 diff 增量。
- 合并方式（squash / merge / rebase）听维护者的，不在 PR 里自作主张。

## 6. 合并后收尾

- 确认 Issue 已自动关闭（没关就手动关并附 PR 链接）。
- 删除远端分支（GitHub 合并按钮旁有 Delete branch；本地 `git branch -d` + `git remote prune origin`）。
- 归档本轮成果：`git log --oneline main..分支名` 输出 + PR 地址 + Issue 地址。

## 7. 演示样例（"随便写一个 txt" 的规范做法）

1. 阅读 README，发现没写本地构建的前置依赖说明 → 开 Issue：
   `[Docs] README 缺少本地构建前置依赖说明`
2. 维护者回复认领 → `git checkout -b docs/#1-build-deps`
3. 新增 `docs/build-notes.txt`（内容：依赖清单 + 一行说明），或按维护者指定位置放。
4. `git commit -m "docs: 新增本地构建前置依赖说明 (#1)"` → push → PR（Closes #1）。
5. review → 若被要求挪位置/补内容 → 追加 commit `docs: 按 review 意见移至根目录并补充版本号`。
6. 合并 → Issue #1 自动关闭 → 删分支 → 提交本轮 git 成果。

## 8. 常见扣分点（反向自查）

- ❌ 不开 Issue 直接发 PR
- ❌ PR 里混多个不相关改动 / 顺手格式化全文件
- ❌ commit 信息是 "update"、"fix bug"、"111" 这类无信息量文案
- ❌ 分支叫 `patch-1`、`dev` 这种无关联名
- ❌ review 意见不回复或 force-push 抹掉讨论上下文
- ❌ 合并后不删分支、Issue 挂着不关
