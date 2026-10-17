# AGENTS.md

## 模型使用建议

| 模型规模 | 常见模型示例 | 建议 |
| --- | --- | --- |
| 小模型 | GPT-6 Luna、Claude Haiku、Gemini Flash、DeepSeek Flash、Qwen Flash、GLM Flash | 适合分类、抽取、格式转换。 |
| 中模型 | GPT-6.1 Sol、Claude Sonnet、Gemini Pro、DeepSeek V4 Pro、Qwen Max、GLM | **十分建议使用这份 AGENTS.md，作为默认选择。** |
| 大模型 | GPT-6 Astra、Claude Opus、Gemini Argon、Grok、Llama 大型版本 | 适合复杂推理、架构设计和代码改造。 |

## 安装与更新

### 1. 更新当前文件夹

```powershell
$uri = "https://github.com/w0fv1/AGENTS.md/releases/latest/download/AGENTS.md"
Invoke-WebRequest $uri -OutFile AGENTS.md
```

### 2. 逐个确认更新直接子文件夹

只检查一层。先判断是否为 Git 仓库，只有存在 `.git` 且已有 `AGENTS.md` 才询问。每个仓库分别确认更新、提交和 Push，非 Y/y 跳过当前仓库。

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
