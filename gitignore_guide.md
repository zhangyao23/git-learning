# .gitignore 使用指南

`.gitignore` 文件用于告诉 Git 哪些文件或目录不需要被版本控制（即不需要被 `git add` 和 `git commit`）。

## 1. 为什么需要忽略文件？
- **临时文件**：编译产生的中间文件（如 `.o`, `.class`）、日志文件（`*.log`）。
- **敏感信息**：包含密码、API Key 的配置文件（如 `.env`, `config.js`）。
- **系统文件**：操作系统自动生成的文件（如 Mac 的 `.DS_Store`，Windows 的 `Thumbs.db`）。
- **依赖包**：如 Node.js 的 `node_modules`，体积大且可以通过 `package.json` 重新安装，不需要提交。
- **IDE 配置**：个人的编辑器配置（如 `.vscode`），避免覆盖队友的设置。

## 2. 语法规则
- **注释**：以 `#` 开头的行是注释。
- **忽略文件**：直接写文件名，如 `secret.txt`。
- **忽略目录**：在结尾加斜杠，如 `build/` 表示忽略 build 目录下的所有文件。
- **通配符**：
    - `*`：匹配零个或多个字符。例如 `*.log` 忽略所有后缀为 .log 的文件。
    - `?`：匹配一个字符。
    - `[]`：匹配括号内的任一字符。
- **否定规则**：以 `!` 开头，表示不忽略（即使前面有规则忽略了它）。
    ```gitignore
    *.a       # 忽略所有 .a 文件
    !lib.a    # 但不忽略 lib.a
    ```

## 3. 常见问题
**Q: 我添加了规则，但文件还是被 Git 追踪了？**
A: 如果文件已经被提交过（committed），`.gitignore` 对它无效。你需要先从 Git 缓存中删除它：
```bash
git rm --cached <filename>
git commit -m "stop tracking <filename>"
```

**Q: 如何检查某个文件被哪个规则忽略了？**
```bash
git check-ignore -v <filename>
```
