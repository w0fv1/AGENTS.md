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

在项目根目录执行：

~~~shell
gh release download --repo w0fv1/AGENTS.md --pattern AGENTS.md --clobber
~~~

Windows PowerShell：

~~~powershell
Invoke-WebRequest "https://github.com/w0fv1/AGENTS.md/releases/latest/download/AGENTS.md" -OutFile "AGENTS.md"
~~~

Windows CMD：

~~~batch
curl.exe -fL "https://github.com/w0fv1/AGENTS.md/releases/latest/download/AGENTS.md" -o "AGENTS.md"
~~~

macOS/Linux：

~~~shell
curl -fL "https://github.com/w0fv1/AGENTS.md/releases/latest/download/AGENTS.md" -o AGENTS.md
~~~

### 2. 逐个确认更新直接子文件夹

在父文件夹执行，只检查一层。每个已有 AGENTS.md 的子文件夹单独询问：输入 Y/y 才更新，其他输入跳过并继续。若子文件夹是 Git 仓库，更新后还会分别询问是否提交、是否推送。

Windows PowerShell：

~~~powershell
$uri = "https://github.com/w0fv1/AGENTS.md/releases/latest/download/AGENTS.md"
Get-ChildItem -Directory | ForEach-Object {
    $repo = $_.FullName
    $file = Join-Path $repo "AGENTS.md"
    if (Test-Path -LiteralPath $file) {
        if ((Read-Host "更新 $repo？Y=继续") -notmatch "^[Yy]$") { return }
        Invoke-WebRequest $uri -OutFile $file
        if (Test-Path (Join-Path $repo ".git")) {
            if ((Read-Host "提交 $repo？Y=继续") -notmatch "^[Yy]$") { return }
            git -C $repo add AGENTS.md
            git -C $repo commit -m "Update AGENTS.md"
            if ((Read-Host "推送 $repo？Y=继续") -match "^[Yy]$") { git -C $repo push }
        }
    }
}
~~~

macOS/Linux：

~~~shell
for repo in */; do
  file="${repo}AGENTS.md"
  if [ -f "$file" ]; then
    read -r -p "更新 $repo？Y=继续：" ok
    case "$ok" in
      [Yy]) curl -fL "https://github.com/w0fv1/AGENTS.md/releases/latest/download/AGENTS.md" -o "$file" ;;
      *) continue ;;
    esac
    if [ -d "${repo}.git" ]; then
      read -r -p "提交 $repo？Y=继续：" ok
      case "$ok" in [Yy]) git -C "$repo" add AGENTS.md && git -C "$repo" commit -m "Update AGENTS.md" ;; *) continue ;; esac
      read -r -p "推送 $repo？Y=继续：" ok
      case "$ok" in [Yy]) git -C "$repo" push ;; esac
    fi
  fi
done
~~~

Windows CMD 请在每个目录内依次使用 set /p 或 choice 询问更新、提交、推送；任一步不是 Y 都跳过当前目录。

## 发布

版本由发布工作流自动生成，每个正式版本都可以独立下载和追溯。
