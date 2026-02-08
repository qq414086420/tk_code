# OpenSpec Language Rules

## OpenSpec 工作规则

当使用 OpenSpec CLI 工具（如 /opsx-new-change、/opsx-continue-change、/opsx-apply-change 等命令）时：

- 所有 OpenSpec 工件内容必须使用中文
- 与用户交互的所有提示和响应必须使用中文
- proposal.md、design.md、specs/*.md、tasks.md 等文档必须用中文编写
- 场景描述使用中文（WHEN/THEN使用中文）

## Git 自动提交和推送规则

### 归档变更时

- **默认行为**：归档变更时自动提交到 git 并推送到远程仓库
- **命令选项**：可通过命令行选项控制
  - `--no-auto-commit`：禁用自动提交
  - `--no-auto-push`：禁用自动推送

### 前提条件

自动提交和推送需要：
- 项目是 git 仓库（可通过 `git init` 初始化）
- 有配置的远程仓库（可通过 `git remote add origin <url>` 配置）

### 示例

```bash
# 默认：自动提交和推送
openspec archive add-rules

# 禁用自动推送（仅提交）
openspec archive add-rules --no-auto-push

# 禁用自动提交和推送（仅归档）
openspec archive add-rules --no-auto-commit --no-auto-push
```

详细文档：[Auto Git Commit and Push](./docs/AUTO_GIT.md)

