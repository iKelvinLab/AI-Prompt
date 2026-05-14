检查项目的环境配置是否完整且正确：$1

检查内容：
1. **依赖项**：
   - 读取 `package.json`/`requirements.txt`/`Cargo.toml` 等
   - 检查是否有过时的依赖（有安全漏洞）
   - 检查版本冲突

2. **环境变量**：
   - 读取 `.env.example` 或代码中引用的环境变量
   - 检查当前环境是否缺少必需的变量
   - 敏感信息是否正确加密/隐藏

3. **配置文件**：
   - `tsconfig.json`/`babel.config.js` 等是否合理
   - 是否启用了必要的编译选项（如 TypeScript strict mode）

4. **开发工具**：
   - Linter/Formatter 配置是否存在
   - Git hooks 是否配置
   - CI/CD 配置是否完整

输出格式：
### ✅ 已正确配置
- [列表]

### ⚠️ 需要注意
- [列表]

### ❌ 缺少配置
- [列表 + 修复建议]
