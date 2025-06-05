# Cline 功能说明

## 基本介绍
Cline是一个高度专业化的AI软件工程师助手，专注于通过系统化、分步骤的方式帮助用户完成技术任务。

## 核心能力
1. 代码编写与修改
2. 文件操作与管理
3. 系统命令执行
4. 项目分析与重构
5. 问题诊断与解决

## 工具系统
### 文件操作
- `read_file`: 读取文件内容
- `write_to_file`: 创建/覆盖文件
- `replace_in_file`: 精确修改文件内容
- `search_files`: 跨文件搜索

### 系统命令
- `execute_command`: 执行系统命令

### 项目分析
- `list_files`: 列出目录内容
- `list_code_definition_names`: 分析代码结构

### 其他工具
- `ask_followup_question`: 询问后续问题
- `attempt_completion`: 尝试完成任务  
- `new_task`: 创建新任务
- `plan_mode_respond`: 计划模式响应
- `load_mcp_documentation`: 加载MCP文档
- `use_mcp_tool`: 使用MCP工具
- `access_mcp_resource`: 访问MCP资源

## 工作流程
1. 任务分解
2. 逐步执行
3. 结果确认
4. 迭代优化

## 特殊功能
- 支持MCP服务器扩展
- 严格的XML格式工具调用
- 自动处理文件路径
- 精确的代码修改能力

## 聊天命令
- `/new`: 创建新任务(清空当前上下文)
- `/task [内容]`: 定义任务内容
- `/act`: 切换到执行模式(执行具体操作)
- `/plan`: 切换到计划模式(制定计划)
- `/help`: 获取帮助信息
- `/mode`: 显示当前模式(ACT/PLAN)
- `/reset`: 重置当前任务
- `/continue`: 继续上次未完成任务
- `/settings`: 查看当前设置
- `/smol`: 切换到简洁响应模式
- `/newrules`: 设置新的自定义规则

## 使用建议
1. 提供明确的任务描述
2. 分步骤确认执行结果
3. 利用工具组合完成复杂任务
