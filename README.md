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

```powershell
$uri = "https://github.com/w0fv1/AGENTS.md/releases/latest/download/AGENTS.md"
Invoke-WebRequest $uri -OutFile AGENTS.md
```

### 2. 逐个确认更新直接子文件夹

只检查一层。只有子文件夹直属存在 `.git`，且已有 `AGENTS.md`，才会询问；不会把上级 Git 仓库中的普通子文件夹当成仓库。每个仓库分别确认更新、提交和 Push，输入不是 Y/y 就跳过对应步骤并继续。

```powershell
$uri = "https://github.com/w0fv1/AGENTS.md/releases/latest/download/AGENTS.md"
Get-ChildItem -Directory | ForEach-Object {
    $repo = $_.FullName
    if (Test-Path -LiteralPath (Join-Path $repo ".git")) {
        $file = Join-Path $repo "AGENTS.md"
        if (Test-Path -LiteralPath $file) {
            $ok = Read-Host "更新 $repo？Y=继续"
            if ($ok -match "^[Yy]$") {
                Invoke-WebRequest $uri -OutFile $file
                $ok = Read-Host "提交 $repo？Y=继续"
                if ($ok -match "^[Yy]$") {
                    git -C $repo add AGENTS.md
                    git -C $repo commit -m "Update AGENTS.md"
                    $ok = Read-Host "Push $repo？Y=继续"
                    if ($ok -match "^[Yy]$") { git -C $repo push }
                }
            }
        }
    }
}
```

macOS/Linux：

```bash
uri="https://github.com/w0fv1/AGENTS.md/releases/latest/download/AGENTS.md"
for repo in */; do
    git -C "$repo" rev-parse --show-toplevel >/dev/null 2>&1 || continue
    file="${repo}AGENTS.md"
    [ -f "$file" ] || continue
    read -r -p "更新 ${repo}? Y=继续：" ok
    [[ "$ok" =~ ^[Yy]$ ]] || continue
    curl -fsSL "$uri" -o "$file"
    read -r -p "提交 ${repo}? Y=继续：" ok
    [[ "$ok" =~ ^[Yy]$ ]] || continue
    git -C "$repo" add AGENTS.md && git -C "$repo" commit -m "Update AGENTS.md"
    read -r -p "Push ${repo}? Y=继续：" ok
    [[ "$ok" =~ ^[Yy]$ ]] && git -C "$repo" push
done
```
