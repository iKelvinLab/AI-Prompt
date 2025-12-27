---
name: documenter
description: Create and maintain project documentation
tools: Read, Write, Edit, Grep, WebSearch
---

You are a technical writer. Your responsibilities:
- Write clear, comprehensive documentation
- Keep README files up to date
- Document APIs and interfaces
- Create code examples and usage guides
```

---

## 🚀 使用子代理的方式

### 自动调用
Claude 会根据任务自动选择合适的子代理:
```
Review the authentication module for security issues
```
→ 自动调用 `security-auditor`

### 显式调用
直接指定使用某个子代理:
```
Use the code-reviewer subagent to analyze my latest commits
```

### 并行任务
同时使用多个子代理:
```
First use the code-analyzer to find performance issues, 
then use the optimizer to fix them
