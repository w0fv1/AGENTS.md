# AGENTS.md

> 让 Agent 写下的每一行代码，都服务于项目的下一年。

一套面向真实项目的 Agent 工程准则。它把长期主义、架构一致性、单一真相源、TDD、真实环境验证和可回滚交付带进每一次开发，让 Agent 不只完成眼前的任务，更能主动控制复杂度、消除架构分叉并守住长期质量。

## 它带来的价值

- **项目越改越干净**：优先解决根因，持续删除无效代码和历史包袱，避免用补丁换取短期完成。
- **架构始终只有一条主线**：统一结构、类型和写入口，用复用与派生维护单一真相源。
- **交付经得起真实使用**：先测试再实现，在真实环境中验证，并保持每次变更可编译、可运行、可回滚。
- **Agent 真正理解项目**：先阅读现有代码和文档，再设计、确认和开发，把已有能力变成新功能的地基。
- **一套标准覆盖所有仓库**：同一个版本化规则文件可以快速引入任何项目，团队与 Agent 始终遵循一致的工程判断。

完整规则见 [`AGENTS.md`](AGENTS.md)。

## 编写理念

这份规则由人类高级工程专家持续编辑和审定。每一条内容都来自对真实工程问题的抽象，追求用最少、最精炼的语言表达最具迁移价值的思想，让有限的上下文承载尽可能高的决策密度。

整体设计遵循“道、术、器”三个层次：

- **道**：决定什么是长期正确的事，建立稳定的工程价值观。
- **术**：定义可复用的判断方法和工作方式，把价值观落实到开发过程。
- **器**：承载具体工具、框架和实现，它们应由项目按需选择并随时代演进。

`AGENTS.md` 的重心放在“道”和“术”。它不试图收录所有工具用法，而是用最低的规则成本撬动最大的工程收益，让同一套原则能够跨项目、跨技术栈长期生效。

## 使用注意事项

这套规则的收益取决于模型对抽象工程原则的理解能力和指令遵循能力。

| 模型/系列 | 使用建议 | 适合场景 |
| --- | --- | --- |
| OpenAI GPT-6 Astra、GPT-6.1 Sol、GPT-6 Luna | Astra 用于最难任务；Sol 作为默认均衡选择；Luna 用于低成本高并发 | 编码、复杂推理、Agent、通用工作 |
| Anthropic Claude Opus 5.5、Sonnet 5.5、Haiku 5.5 | Opus 处理复杂任务；Sonnet 兼顾质量与成本；Haiku 适合速度和规模 | 长文档、代码审查、写作、工具调用 |
| Google Gemini 4 Argon、Gemini 3.8 Flash | Argon 用于旗舰推理；Flash 用于快速、低成本、多模态任务 | 超长上下文、图片/音频/视频、代码 |
| xAI Grok 4.7 | 适合需要实时信息、联网和强风格表达的任务 | 搜索辅助、分析、开放式问答 |
| DeepSeek V4 Pro、V4 Flash | 适合成本敏感的推理和代码任务；优先通过官方渠道确认具体版本 | 中文、数学、代码、批量调用 |
| Qwen 3.8 Max、Qwen 3.8 Flash | Max 追求质量；Flash 追求速度和成本 | 中文、多语言、代码、企业私有化 |
| GLM-5.3、GLM-5.3-Flash | 适合中文任务和较低成本部署 | 中文写作、代码、知识问答 |
| Meta Llama 系列 | 适合自部署、微调和可控的开源权重场景 | 私有化、边缘部署、定制模型 |
| Mistral Large、Mixtral 系列 | 适合欧洲生态、开放部署和高性价比推理 | 多语言、代码、私有化 |
| Kimi、豆包等中文模型 | 按具体产品和版本选择，重点核对上下文、工具和数据策略 | 中文长文档、办公、搜索和内容生成 |
| 小模型、蒸馏模型 | 只有在任务边界清晰且验收标准明确时使用 | 分类、抽取、格式转换、简单自动化 |
它最适合作为具备一定推理能力和良好指令遵循能力的 Agent 的工程放大器。小模型如果需要稳定产出，应优先补充面向具体任务的明确步骤和验收标准。

## 安装与更新

在项目根目录执行：

```shell
gh release download --repo w0fv1/AGENTS.md --pattern AGENTS.md --clobber
```

这条命令同时用于首次安装和后续更新，会将项目根目录的 `AGENTS.md` 替换为最新正式版本。

不使用 GitHub CLI 时，可以直接下载覆盖。

如果要在一个父文件夹中批量更新其**直接子文件夹**里的 `AGENTS.md`（不递归更深层目录），请在该父文件夹执行对应命令。命令只处理已经存在 `AGENTS.md` 的子文件夹，不会新建文件。

### Windows PowerShell：批量更新一层子文件夹

```powershell
$uri = "https://github.com/w0fv1/AGENTS.md/releases/latest/download/AGENTS.md"
Get-ChildItem -Directory | ForEach-Object {
    $target = Join-Path $_.FullName "AGENTS.md"
    if (Test-Path -LiteralPath $target) {
        Invoke-WebRequest $uri -OutFile $target
        Write-Host "Updated $target"
    }
}
```

### Windows CMD：批量更新一层子文件夹

```batch
for /d %D in (*) do @if exist "%D\AGENTS.md" curl.exe -fL "https://github.com/w0fv1/AGENTS.md/releases/latest/download/AGENTS.md" -o "%D\AGENTS.md"
```

### macOS/Linux：批量更新一层子文件夹

```shell
for dir in */; do
  [ -f "${dir}AGENTS.md" ] && curl -fL "https://github.com/w0fv1/AGENTS.md/releases/latest/download/AGENTS.md" -o "${dir}AGENTS.md"
done
```
### Windows PowerShell

```powershell
Invoke-WebRequest "https://github.com/w0fv1/AGENTS.md/releases/latest/download/AGENTS.md" -OutFile "AGENTS.md"
```

### Windows CMD

```batch
curl.exe -fL "https://github.com/w0fv1/AGENTS.md/releases/latest/download/AGENTS.md" -o "AGENTS.md"
```

### macOS/Linux

```shell
curl -fL "https://github.com/w0fv1/AGENTS.md/releases/latest/download/AGENTS.md" -o AGENTS.md
```

也可以直接[下载最新版本](https://github.com/w0fv1/AGENTS.md/releases/latest/download/AGENTS.md)。

## 发布

版本由[发布工作流](.github/workflows/release.yml)自动生成，每个正式版本都可以独立下载和追溯。


> **使用建议：** 对具备推理能力和良好指令遵循能力的模型，十分建议使用这份 `AGENTS.md`。
>
> **批量更新确认：** 批量更新命令会覆盖直接子文件夹中的 `AGENTS.md`。执行命令前必须先确认目标目录和文件，只有明确输入 `Y`/`y` 后才允许继续；输入其他内容应立即取消。
