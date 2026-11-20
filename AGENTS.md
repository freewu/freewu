# AGENTS.md

本文件的约定对所有在本仓库工作的 AI Agent / 协作者生效。

## 项目说明

- 本仓库是 GitHub Profile 仓库（`freewu/freewu`），`README.md` 就是 GitHub 个人主页内容。
- 图片资源目录：
  - `images/*.png` —— `### Projects` 区块的应用截图（平铺）
  - `images/book/*.png` —— `### Books` 区块的书籍封面
  - `images/app/*.png|jpg` —— `### Apps` 区块的 App 截图

## 排版约定

- 文件换行统一使用 **LF**（不要提交 CRLF，避免整文件 diff）。
- 表格内层级缩进沿用现有风格（`<tr>` 用 2 个 tab，`<td>` 用 3 个 tab）。
- `### Books` / `### Apps` 区块结构固定为三段：
  1. 图片行：`<img src="images/app/xxx.png" width="90%" title="xxx" />`
  2. 名称链接行：`<a href="https://freewu.github.io/xxx/">xxx</a>`
  3. 描述行：`使用 <技术栈> 开发的 <功能>`
- 新增 App 时：截图放到 `images/app/<repo>.png`，在 `### Apps` 表格中按 2 列（`width="48%"`）追加。
- 描述文案优先取对应 GitHub 仓库的 `description`。

## 交付流程（开发完成后必须自动执行）

开发/修改完成后，不需要再询问，直接按顺序执行以下步骤：

1. **自检**：`git status --short`、`git diff`，确认没有多余文件、图片路径正确、换行是 LF。
2. **暂存**：`git add -A`
3. **提交**：`git commit -m "<type>: <简述>"`（如 `feat: add apps section`、`docs: update apps`）
4. **推送**：`git push origin main`
5. **触发 GitHub Action 的 Pages 部署**，任选其一：
   - 已登录 gh 时：
     ```bash
     gh workflow run main.yml --ref main
     ```
   - 使用 Token 走 API（`GITHUB_TOKEN` 具备 `workflow` 权限）：
     ```bash
     curl -sS -X POST \
       -H "Authorization: Bearer $GITHUB_TOKEN" \
       -H "Accept: application/vnd.github+json" \
       https://api.github.com/repos/freewu/freewu/actions/workflows/main.yml/dispatches \
       -d '{"ref":"main"}'
     ```
   - 若两者都不可用（未登录 / 无 Token），明确告知用户，并给出可手动点击的地址：
     `https://github.com/freewu/freewu/actions/workflows/main.yml`
6. **验证**：`gh run list --limit 3`（或提示用户查看 Actions 页面），确认本次 run 成功；Pages 站点的产物在 `output` 分支。

> 环境备注：本机 `gh` 位于 `/mnt/c/Program Files/GitHub CLI/gh.exe`（默认未登录，需要时先 `gh auth login`）；
> git 凭证由 `git-credential-manager` 提供，`git push` 可直接使用。

## 提交信息规范

- `feat:` 新增内容 / 区块
- `docs:` 文档、图片、文案调整
- `fix:` 修正错误链接、错别字、图片路径等
