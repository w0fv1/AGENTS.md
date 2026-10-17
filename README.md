# AGENTS.md

> 让 Agent 写下的每一行代码，都服务于项目的下一年。

一套面向真实项目的 Agent 工程准则，强调长期主义、架构一致性、单一真相源、测试、真实环境验证和可回滚交付。

## 它带来的价值

- 项目优先解决根因，持续删除无效代码和历史包袱。
- 统一结构、类型和写入口，维护单一真相源。
- 先测试再实现，在真实环境中验证，并保持可编译、可运行、可回滚。
- 让 Agent 先理解项目，再设计和开发。

完整规则见 AGENTS.md。

## 模型使用建议

| 模型规模 | 常见模型示例 | 建议 |
| --- | --- | --- |
| 小模型 | GPT-6 Luna、Claude Haiku、Gemini Flash、DeepSeek Flash、Qwen Flash、GLM Flash | 可选，适合分类、抽取、格式转换等边界清晰的任务。 |
| 中模型 | GPT-6.1 Sol、Claude Sonnet、Gemini Pro、DeepSeek V4 Pro、Qwen Max、GLM | **十分建议使用这份 AGENTS.md，作为默认选择。** 能稳定理解抽象原则并执行工程流程。 |
| 大模型 | GPT-6 Astra、Claude Opus、Gemini Argon、Grok、Llama 大型版本 | 按需使用，适合最复杂的推理、架构设计和代码改造。 |

模型名称会变化；选择时确认具体版本、上下文、工具调用能力和数据策略。

## 安装与更新

### 1. 更新当前文件夹

PowerShell：

```powershell
$uri = "https://github.com/w0fv1/AGENTS.md/releases/latest/download/AGENTS.md"
Invoke-WebRequest $uri -OutFile AGENTS.md
```

macOS/Linux：

```bash
curl -fsSL https://github.com/w0fv1/AGENTS.md/releases/latest/download/AGENTS.md -o AGENTS.md
```

### 2. 逐个确认更新直接子文件夹

只检查当前文件夹的直接子文件夹。先判断子文件夹是否为 Git 仓库；只有存在 `.git` 且已有 `AGENTS.md` 的子文件夹才会询问。每个仓库分别确认更新、提交和 Push，输入不是 Y/y 就跳过当前仓库并继续。

PowerShell：

```powershell
$uri = "https://github.com/w0fv1/AGENTS.md/releases/latest/download/AGENTS.md"
Get-ChildItem -Directory | ForEach-Object {
    $repo = $_.FullName
    if (Test-Path (Join-Path $repo ".git")) {
        $file = Join-Path $repo "AGENTS.md"
        if (Test-Path -LiteralPath $file) {
            if ((Read-Host "更新 $repo？Y=继续") -notmatch "^[Yy]$") { return }
            Invoke-WebRequest $uri -OutFile $file
            if ((Read-Host "提交 $repo？Y=继续") -notmatch "^[Yy]$") { return }
            git -C $repo add AGENTS.md
            git -C $repo commit -m "Update AGENTS.md"
            if ((Read-Host "Push $repo？Y=继续") -match "^[Yy]$") { git -C $repo push }
        }
    }
}
```

macOS/Linux：

```bash
uri="https://github.com/w0fv1/AGENTS.md/releases/latest/download/AGENTS.md"
for repo in */; do
    if [ -d "${repo}.git" ] && [ -f "${repo}AGENTS.md" ]; then
        read -r -p "更新 ${repo}? Y=继续：" ok
        [[ "$ok" =~ ^[Yy]$ ]] || continue
        curl -fsSL "$uri" -o "${repo}AGENTS.md"
        read -r -p "提交 ${repo}? Y=继续：" ok
        [[ "$ok" =~ ^[Yy]$ ]] || continue
        git -C "$repo" add AGENTS.md && git -C "$repo" commit -m "Update AGENTS.md"
        read -r -p "Push ${repo}? Y=继续：" ok
        [[ "$ok" =~ ^[Yy]$ ]] && git -C "$repo" push
    fi
done
```
