# Cline Rules 系统指南

## 规则概述
Cline Rules 允许为项目或全局对话提供系统级指导，是一种持久化的上下文和偏好设置方式。

## 创建规则
1. 点击Rules标签页的"+"按钮
2. 在IDE中编辑规则文件
3. 保存位置：
   - 项目规则：`.clinerules/`目录
   - 全局规则：`Documents/Cline/Rules`目录
4. 也可使用`/newrule`命令创建

## 规则文件示例
```markdown
# Project Guidelines

### Documentation Requirements
- 更新相关文档在/docs目录
- 保持README.md同步
- 维护CHANGELOG.md

### Architecture Decision Records
在/docs/adr创建ADRs记录：
- 主要依赖变更
- 架构模式变更
- 新集成模式
- 数据库模式变更

### Code Style & Patterns
- 使用OpenAPI Generator生成API客户端
- 采用TypeScript axios模板
- 生成代码放在/src/generated
- 优先使用组合而非继承
- 数据访问使用仓库模式
```

## 关键优势
1. 版本控制：`.clinerules`成为项目代码一部分
2. 团队一致性：确保所有成员行为一致
3. 项目特定：为每个项目定制规则
4. 知识传承：维护项目标准和实践

## 文件夹系统
```
your-project/
├── .clinerules/       # 活动规则
│   ├── 01-coding.md   # 编码标准
│   └── current-sprint.md
├── clinerules-bank/   # 规则库
│   ├── clients/       # 客户特定规则
│   └── frameworks/    # 框架特定规则
└── ...
```

### 文件夹系统优势
1. 上下文激活：从规则库复制相关规则
2. 易于维护：单独更新规则文件
3. 团队灵活性：按需激活规则
4. 减少干扰：保持活动规则集专注

## 使用建议
1. 清晰简洁：使用简单明确的语言
2. 关注结果：描述期望结果而非具体步骤
3. 测试迭代：实验找到最佳工作流

## 规则管理UI (v3.13+)
1. 查看活动规则
2. 快速切换规则
3. 添加/管理规则
