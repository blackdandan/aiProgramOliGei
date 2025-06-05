# Workflows

Workflows允许您定义一系列步骤来指导Cline完成重复性任务，例如部署服务或提交PR。

要调用workflow，在聊天中输入`/[workflow-name.md]`。

## 如何创建和使用Workflows

Workflows与[Cline Rules](/features/cline-rules)并存。创建方法很简单：

<Frame>
  <img src="https://storage.googleapis.com/cline_public_images/docs/assets/workflows.png" alt="Cline中的Workflows标签页" />
</Frame>

1. 创建一个包含Cline应执行步骤清晰说明的markdown文件
2. 以`.md`扩展名保存在workflows目录中
3. 要触发workflow，只需输入`/`后跟workflow文件名
4. 在提示时提供任何必需的参数

真正的强大之处在于如何构建workflow文件。您可以：

* 利用Cline的[内置工具](/exploring-clines-tools/cline-tools-guide)如`ask_followup_question`、`read_file`、`search_files`和`new_task`
* 使用已安装的命令行工具如`gh`或`docker`
* 引用外部[MCP工具调用](/mcp/mcp-overview)如Slack或Whatsapp
* 将多个操作按特定顺序链接在一起

## 实际示例

我创建了一个PR Review workflow，已经节省了大量时间。

````md pr-review.md [可展开]
您可以访问`gh`终端命令。我已为您完成认证。请使用它来审查我要求您审查的PR。您已经在`cline`仓库中。

<detailed_sequence_of_steps>

# GitHub PR审查流程 - 详细步骤序列

## 1. 收集PR信息

1. 获取PR标题、描述和评论：

    ```bash
    gh pr view <PR-number> --json title,body,comments
    ```

2. 获取PR的完整差异：
    ```bash
    gh pr diff <PR-number>
    ```

## 2. 理解上下文

1. 识别PR中修改的文件：

    ```bash
    gh pr view <PR-number> --json files
    ```

2. 检查主分支中的原始文件以理解上下文：

    ```xml
    <read_file>
    <path>path/to/file</path>
    </read_file>
    ```

3. 对于文件的特定部分，可以使用search_files：
    ```xml
    <search_files>
    <path>path/to/directory</path>
    <regex>搜索词</regex>
    <file_pattern>*.ts</file_pattern>
    </search_files>
    ```

## 3. 分析变更

1. 对于每个修改的文件，理解：

    - 修改了什么
    - 为什么修改(基于PR描述)
    - 如何影响代码库
    - 潜在副作用

2. 检查：
    - 代码质量问题
    - 潜在错误
    - 性能影响
    - 安全问题
    - 测试覆盖率

## 4. 请求用户确认

1. 在做出决定前，询问用户是否应批准PR，提供您的评估和理由：

    ```xml
    <ask_followup_question>
    <question>基于我对PR #<PR-number>的审查，我建议[批准/请求更改]。这是我的理由：

    [关于PR质量、实现和任何问题的详细理由]

    您希望我继续执行此建议吗？</question>
    <options>["是，批准PR", "是，请求更改", "不，我想进一步讨论"]</options>
    </ask_followup_question>
    ```

## 5. 询问用户是否需要起草评论

1. 用户决定批准/拒绝后，询问是否需要起草评论：

    ```xml
    <ask_followup_question>
    <question>您需要我为这个PR起草一个评论供您复制粘贴吗？</question>
    <options>["是，请起草评论", "不，我自己处理评论"]</options>
    </ask_followup_question>
    ```

2. 如果用户需要评论，提供一个结构良好的评论供其复制：

    ```
    感谢您的PR！这是我的评估：

    [关于PR质量、实现和任何建议的详细评估]

    [包括关于代码质量、功能和测试的具体反馈]
    ```

## 6. 做出决定

1. 如果PR符合质量标准则批准：

    ```bash
    # 单行评论：
    gh pr review <PR-number> --approve --body "您的批准消息"

    # 带正确空白格式的多行评论：
    cat << EOF | gh pr review <PR-number> --approve --body-file -
    感谢@username的PR！实现看起来不错。

    我特别喜欢您处理X和Y的方式。

    干得好！
    EOF
    ```

2. 如果需要改进则请求更改：

    ```bash
    # 单行评论：
    gh pr review <PR-number> --request-changes --body "您的反馈消息"

    # 带正确空白格式的多行评论：
    cat << EOF | gh pr review <PR-number> --request-changes --body-file -
    感谢@username的PR！

    实现看起来有希望，但有几件事需要解决：

    1. 问题一
    2. 问题二

    请做出这些更改后我们可以合并。
    EOF
    ```

    注意：`cat << EOF | ... --body-file -`方法保留所有空白和格式而无需临时文件。`-`参数告诉命令从标准输入读取。
    </detailed_sequence_of_steps>

<example_review_process>

# 示例PR审查流程

让我们通过审查修复Claude 3.7模型思考模式计算的PR #3627的真实示例。

## 步骤1: 收集PR信息

```bash
# 获取PR详情
gh pr view 3627 --json title,body,comments

# 获取完整差异
gh pr diff 3627
```

## 步骤2: 理解上下文

```xml
# 检查原始文件以理解修改内容
<read_file>
<path>src/shared/api.ts</path>
</read_file>

# 查看ThinkingBudgetSlider组件实现
<read_file>
<path>webview-ui/src/components/settings/ThinkingBudgetSlider.tsx</path>
</read_file>

# 检查API提供者中当前如何处理思考模式
<search_files>
<path>src/api/providers</path>
<regex>reasoningOn</regex>
<file_pattern>*.ts</file_pattern>
</search_files>
```

## 步骤3: 分析变更

从PR信息和代码检查中，我们可以看到：

1. PR修复了Claude 3.7模型思考模式预算计算的问题
2. 当前思考预算错误地计算为maxTokens(8192)的80%，结果为6553个token
3. Claude 3.7实际上支持更大的思考预算(高达64000个token)
4. PR添加了带有`maxBudget: 64000`的`thinkingConfig`属性到模型定义中
5. 更新API处理程序在启用推理模式时使用此值
6. 增强滑块组件基于模型特定百分比计算最大值
7. 添加了全面测试来验证计算

## 步骤4: 请求用户确认

```xml
<ask_followup_question>
<question>基于我对PR #3627的审查，我建议批准此PR。这是我的理由：

1. PR正确修复了Claude 3.7模型的思考模式预算计算
2. 它正确实现了Claude 3.7模型的thinkingConfig.maxBudget属性(64000)
3. 它为滑块计算应用了适当的百分比(50%)
4. 变更经过全面单元测试
5. 实现干净并遵循项目编码标准
6. PR包含展示修复的有用截图

您希望我继续批准此PR吗？</question>
<options>["是，批准PR", "不，我想进一步讨论", "让我先自己审查"]</options>
</ask_followup_question>
```

## 步骤5: 询问用户是否需要起草评论

```xml
<ask_followup_question>
<question>您需要我为这个PR起草一个评论供您复制粘贴吗？</question>
<options>["是，请起草评论", "不，我自己处理评论"]</options>
</ask_followup_question>
```

## 步骤6: 做出决定

```bash
# 选项1: 简单单行评论
gh pr review 3627 --approve --body "这个PR看起来不错！它正确修复了Claude 3.7模型的思考模式预算计算。"

# 选项2: 带正确空白格式的多行评论
cat << EOF | gh pr review 3627 --approve --body-file -
这个PR看起来不错！它正确修复了Claude 3.7模型的思考模式预算计算。

我特别喜欢：
1. thinkingConfig.maxBudget属性(64000)的正确实现
2. 滑块计算的适当百分比(50%)
3. 全面的单元测试
4. 遵循项目编码标准的干净实现

干得好！
EOF
```

</example_review_process>

<common_gh_commands>

# PR审查常用GitHub CLI命令

## 基本PR命令

```bash
# 列出打开的PR
gh pr list

# 查看特定PR
gh pr view <PR-number>

# 查看带特定字段的PR
gh pr view <PR-number> --json title,body,comments,files,commits

# 检查PR状态
gh pr status
```

## 差异和文件命令

```bash
# 获取PR的完整差异
gh pr diff <PR-number>

# 列出PR中更改的文件
gh pr view <PR-number> --json files

# 本地检出PR
gh pr checkout <PR-number>
```

## 审查命令

```bash
# 批准PR(单行评论)
gh pr review <PR-number> --approve --body "您的批准消息"

# 批准PR(带正确空白格式的多行评论)
cat << EOF | gh pr review <PR-number> --approve --body-file -
您的多行
批准消息带

正确的空白格式
EOF

# 请求更改PR(单行评论)
gh pr review <PR-number> --request-changes --body "您的反馈消息"

# 请求更改PR(带正确空白格式的多行评论)
cat << EOF | gh pr review <PR-number> --request-changes --body-file -
您的多行
更改请求带

正确的空白格式
EOF

# 添加评论审查(无批准/拒绝)
gh pr review <PR-number> --comment --body "您的评论消息"

# 添加带正确空白格式的评论
cat << EOF | gh pr review <PR-number> --comment --body-file -
您的多行
评论带

正确的空白格式
EOF
```

## 附加命令

```bash
# 查看PR检查状态
gh pr checks <PR-number>

# 查看PR提交
gh pr view <PR-number> --json commits

# 合并PR(如果有权限)
gh pr merge <PR-number> --merge
```

</common_gh_commands>

<general_guidelines_for_commenting>
审查PR时，请像友好的审查者一样正常交谈。保持简短，首先感谢PR作者并@提及他们。

无论是否批准PR，都应快速总结变更而不过于冗长或绝对，保持谦逊态度说明这是您对变更的理解。就像我现在与您交谈的方式一样。

如果有任何建议或需要更改的内容，请请求更改而非批准PR。

在代码中留下内联评论很好，但仅当您对代码有具体意见时才这样做。确保先留下这些评论，然后在PR中用简短评论解释您要求更改的总体主题。
</general_guidelines_for_commenting>

<example_comments_that_i_have_written_before>
<brief_approve_comment>
看起来不错，不过我们应该在某个时候为所有提供者和模型使其通用
</brief_approve_comment>
<brief_approve_comment>
这对可能不匹配OR/Gemini的模型有效吗？比如思考模型？
</brief_approve_comment>
<approve_comment>
这太棒了！我喜欢您处理全局端点支持的方式 - 将其添加到ModelInfo接口完全合理，因为它只是另一个能力标志，类似于我们处理其他模型功能的方式。

过滤模型列表的方法很干净，比硬编码哪些模型适用于全局端点更易于维护。显然需要升级genai库才能工作。

感谢添加关于限制的文档 - 用户知道他们不能将上下文缓存与全局端点一起使用但可能减少429错误是好事。
</approve_comment>
<requesst_changes_comment>
这很棒。感谢@scottsus。

不过我主要担心 - 这对所有可能的VS Code主题都有效吗？我们最初为此挣扎过，这就是为什么目前没有太多样式。请在合并前测试并分享不同主题的截图以确保
</request_changes_comment>
<request_changes_comment>
嘿，PR总体看起来不错，但我担心移除那些超时。它们可能是有原因的 - VSCode的UI在时间上可能很挑剔。

您能在聚焦侧边栏后重新添加超时吗？比如：

```typescript
await vscode.commands.executeCommand("claude-dev.SidebarProvider.focus")
await setTimeoutPromise(100) // 给UI更新时间
visibleWebview = WebviewProvider.getSidebarInstance()
```

</request_changes_comment>
<request_changes_comment>
嗨@alejandropta 感谢您的工作！

几点说明：
1 - 向环境变量添加额外信息相当有问题，因为环境变量会附加到每条消息。我不认为这对于一个相对小众的用例是合理的。
2 - 在设置中添加此选项可能是一个选择，但我们希望我们的选项对新用户来说简单明了
3 - 我们正在重新设计设置页面的显示/组织方式，一旦完成，这可能可以协调，我们的设置页面会更清晰地划分。

所以在设置页面更新前，并且以不混淆新用户的干净方式添加到设置中之前，我认为我们不能合并这个。请耐心等待。
</request_changes_comment>
<request_changes_comment>
另外，不要忘记添加变更集，因为这修复了一个面向用户的错误。

架构变更很扎实 - 将焦点逻辑移动到命令处理程序很有意义。只是不想通过移除那些超时引入微妙的时间问题。
</request_changes_comment>
</example_comments_that_i_have_written_before>
````

当我收到新PR审查时，过去我手动收集上下文：检查PR描述，检查差异，查看周围文件，最后形成意见。现在我只需：

1. 在聊天中输入`/pr-review.md`
2. 粘贴PR编号
3. 让Cline处理其他一切

我的workflow使用`gh`命令行工具和Cline内置的`ask_followup_question`来：

* 拉取PR描述和评论
* 检查差异
* 检查周围文件获取上下文
* 分析潜在问题
* 如果一切看起来不错，询问我是否可以批准，并提供批准理由
* 如果我回答"是"，Cline自动用`gh`命令批准PR

这使我的PR审查流程从手动多步操作变成了一个命令，为我提供做出明智决策所需的一切。

> 这只是workflow文件的一个示例。您可以在我们的[prompts仓库](https://github.com/cline/prompts)中找到更多灵感。

## 构建您自己的Workflows

Workflows的美妙之处在于它们完全可定制以满足您的需求。您可以为各种重复性任务创建workflows：

* 对于发布，您可以有一个workflow获取所有合并的PR，构建变更日志并处理版本更新。
* 新项目设置非常适合workflows。只需运行一个命令创建文件夹结构，安装依赖项并设置配置。
* 需要创建报告？创建一个从不同来源获取统计信息并按您喜欢的方式格式化的workflow。您甚至可以使用[slidev](https://sli.dev/)等库将它们可视化并制作演示文稿。
* 您甚至可以使用workflows在提交PR后通过Slack或Whatsapp等MCP服务器起草消息给您的团队。

使用Workflows，您的想象力是唯一的限制。真正的潜力来自于发现那些您经常做的烦人重复性任务。

如果您能将某件事描述为"首先我做X，然后Y，然后Z" - 那就是一个完美的workflow候选。

从困扰您的小事开始，将其变成workflow并不断改进。您会惊讶于这种方式可以自动化您的一天中的多少内容。
