根据当前的 Git 变更生成符合 Conventional Commits 规范的提交消息。

步骤：
1. 运行 `git diff --staged` 查看暂存的变更
2. 分析变更类型：
   - `feat`: 新功能
   - `fix`: Bug 修复
   - `docs`: 文档变更
   - `style`: 代码格式（不影响功能）
   - `refactor`: 重构
   - `perf`: 性能优化
   - `test`: 测试相关
   - `chore`: 构建/工具链变更

3. 生成消息格式：
```
<type>(<scope>): <subject>

<body>

<footer>
```

示例：
```
feat(auth): add OAuth2 login support

- Implement Google and GitHub OAuth providers
- Add token refresh mechanism
- Update user model to store OAuth tokens

Closes #123
```

请生成 3 个候选消息，按推荐度排序。
