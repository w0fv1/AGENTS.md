# AGENTS.md

<p align="center">
  <strong>让 Agent 写下的每一行代码，都服务于项目的下一年。</strong>
</p>

<p align="center">一套面向真实项目的 Agent 工程准则：长期主义、架构一致性、单一真相源、测试、真实环境验证，以及可回滚交付。</p>

## 核心理念

这份准则要求 Agent 先理解项目，再设计和开发；先解决根因，再修补表象；先建立验证，再交付结果。它把一次性的代码修改，放回项目长期演进的上下文中。

| 原则 | 关注点 |
| --- | --- |
| **解决根因** | 删除无效代码和历史包袱，避免反复修补同一问题。 |
| **单一真相源** | 统一结构、类型和写入口，减少重复定义和行为分叉。 |
| **验证优先** | 先测试，再实现；在真实环境中验证可编译、可运行和可回滚。 |
| **理解项目** | 先阅读现有结构、约束和约定，再做设计与开发。 |

完整规则见 [`AGENTS.md`](AGENTS.md)。

## 模型选择

模型名称和能力会持续变化。选择时还应确认具体版本、上下文长度、工具调用能力和数据策略。

| 规模 | 常见模型示例 | 适合任务 | 建议 |
| --- | --- | --- | --- |
| 小 | GPT-6 Luna、Claude Haiku、Gemini Flash、DeepSeek Flash、Qwen Flash、GLM Flash | 分类、抽取、格式转换等边界清晰的任务 | 可选 |
| 中 | GPT-6.1 Sol、Claude Sonnet、Gemini Pro、DeepSeek V4 Pro、Qwen Max、GLM | 日常开发、调试、重构和文档维护 | **十分建议使用，作为默认选择** |
| 大 | GPT-6 Astra、Claude Opus、Gemini Argon、Grok、Llama 大型版本 | 复杂推理、架构设计和大型代码改造 | 按需使用 |

## 安装与更新

### 1. 更新当前文件夹

在目标项目根目录执行。

**PowerShell**

```powershell
$uri = "https://raw.githubusercontent.com/w0fv1/AGENTS.md/main/AGENTS.md"
Invoke-WebRequest -Uri $uri -OutFile AGENTS.md
```

**macOS / Linux**

```bash
curl -fsSL https://raw.githubusercontent.com/w0fv1/AGENTS.md/main/AGENTS.md -o AGENTS.md
```

### 2. 逐个确认更新直接子文件夹

在父文件夹执行，只检查一层：

- 先用 Git 验证子文件夹本身是有效仓库，且 Git 根目录就是该子文件夹；
- 仓库根目录存在 `AGENTS.md`，才进入确认流程；
- 更新、提交、Push 分别确认；
- 输入不是 `Y/y`，跳过当前步骤并继续处理后面的仓库。

**PowerShell**

```powershell
$uri = "https://raw.githubusercontent.com/w0fv1/AGENTS.md/main/AGENTS.md"

foreach ($dir in Get-ChildItem -Directory) {
    $repo = $dir.FullName
    $file = Join-Path $repo "AGENTS.md"
    $gitRootRaw = (& git -C $repo rev-parse --show-toplevel 2>$null | Select-Object -First 1)

    if ($LASTEXITCODE -ne 0 -or [string]::IsNullOrWhiteSpace($gitRootRaw) -or -not (Test-Path -LiteralPath $file -PathType Leaf)) {
        continue
    }

    $gitRoot = [IO.Path]::GetFullPath(($gitRootRaw.Trim() -replace "/", "\")).TrimEnd("\")
    $repoPath = [IO.Path]::GetFullPath($repo).TrimEnd("\")
    if (-not [StringComparer]::OrdinalIgnoreCase.Equals($gitRoot, $repoPath)) {
        continue
    }

    $ok = Read-Host "更新 $repo？Y=继续"
    if ($ok -notmatch "^[Yy]$") {
        continue
    }

    Invoke-WebRequest -Uri $uri -OutFile $file
    & git -C $repo add -- AGENTS.md
    if ($LASTEXITCODE -ne 0) {
        Write-Warning "git add 失败：$repo"
        continue
    }

    & git -C $repo diff --cached --quiet -- AGENTS.md
    $diffExit = $LASTEXITCODE
    if ($diffExit -eq 0) {
        Write-Host "没有变化：$repo"
        continue
    }
    if ($diffExit -ne 1) {
        Write-Warning "无法检查暂存区：$repo"
        continue
    }

    $ok = Read-Host "提交 $repo？Y=继续"
    if ($ok -notmatch "^[Yy]$") {
        & git -C $repo reset -- AGENTS.md
        continue
    }

    & git -C $repo commit -m "Update AGENTS.md"
    if ($LASTEXITCODE -ne 0) {
        Write-Warning "git commit 失败：$repo"
        continue
    }

    $ok = Read-Host "Push $repo？Y=继续"
    if ($ok -notmatch "^[Yy]$") {
        continue
    }

    & git -C $repo push
    if ($LASTEXITCODE -ne 0) {
        Write-Warning "git push 失败：$repo"
    }
}
```

**macOS / Linux**

```bash
uri="https://raw.githubusercontent.com/w0fv1/AGENTS.md/main/AGENTS.md"

for repo in */; do
    git -C "$repo" rev-parse --is-inside-work-tree >/dev/null 2>&1 || continue
    [ "$(git -C "$repo" rev-parse --show-toplevel 2>/dev/null)" = "$(cd "$repo" && pwd)" ] || continue
    file="$repo/AGENTS.md"
    [ -f "$file" ] || continue

    read -r -p "更新 $repo？Y=继续：" ok
    [[ "$ok" =~ ^[Yy]$ ]] || continue
    curl -fsSL "$uri" -o "$file"

    git -C "$repo" add -- AGENTS.md
    git -C "$repo" diff --cached --quiet -- AGENTS.md
    diff_status=$?
    if [ "$diff_status" -eq 0 ]; then
        echo "没有变化：$repo"
        git -C "$repo" reset -- AGENTS.md
        continue
    fi
    if [ "$diff_status" -ne 1 ]; then
        echo "无法检查暂存区：$repo" >&2
        continue
    fi

    read -r -p "提交 $repo？Y=继续：" ok
    if [[ ! "$ok" =~ ^[Yy]$ ]]; then
        git -C "$repo" reset -- AGENTS.md
        continue
    fi
    git -C "$repo" commit -m "Update AGENTS.md"

    read -r -p "Push $repo？Y=继续：" ok
    [[ "$ok" =~ ^[Yy]$ ]] && git -C "$repo" push
done
```
