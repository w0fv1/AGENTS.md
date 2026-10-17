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

在父文件夹执行，只检查一层。每个已有 AGENTS.md 的子文件夹单独询问：输入 Y/y 才更新，其他输入跳过并继续。

Windows PowerShell：

~~~powershell
$uri = "https://github.com/w0fv1/AGENTS.md/releases/latest/download/AGENTS.md"
Get-ChildItem -Directory | ForEach-Object {
    $file = Join-Path $_.FullName "AGENTS.md"
    if (Test-Path -LiteralPath $file) {
        $ok = Read-Host "更新 $file？输入 Y 继续，其他输入跳过"
        if ($ok -match "^[Yy]$") { Invoke-WebRequest $uri -OutFile $file }
    }
}
~~~

macOS/Linux：

~~~shell
for dir in */; do
  file="${dir}AGENTS.md"
  if [ -f "$file" ]; then
    read -r -p "更新 $file？输入 Y 继续，其他输入跳过：" ok
    case "$ok" in [Yy]) curl -fL "https://github.com/w0fv1/AGENTS.md/releases/latest/download/AGENTS.md" -o "$file" ;; esac
  fi
done
~~~

Windows CMD 请对每个匹配目录使用 set /p 或 choice 单独询问；只有输入 Y 才更新当前目录，其他输入跳过。

## 发布

版本由发布工作流自动生成，每个正式版本都可以独立下载和追溯。
