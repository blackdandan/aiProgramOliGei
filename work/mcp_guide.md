# MCP (Model Context Protocol) 指南

## MCP 功能概述
MCP (Model Context Protocol) 是一种扩展协议，允许AI助手与本地运行的MCP服务器通信，从而扩展AI的能力。主要功能包括：
- 提供额外的工具和资源
- 连接外部API和服务
- 执行自定义操作
- 访问专用数据源

## 基本用法
### 可用工具
1. `use_mcp_tool`: 使用MCP服务器提供的工具
2. `access_mcp_resource`: 访问MCP服务器提供的资源

### 使用示例
```xml
<use_mcp_tool>
<server_name>weather-server</server_name>
<tool_name>get_forecast</tool_name>
<arguments>
{
  "city": "Beijing",
  "days": 3
}
</arguments>
</use_mcp_tool>
```

## 自定义MCP服务器开发指南

### 1. 准备工作
- 安装Rust工具链
- 克隆MCP服务器模板仓库
- 准备开发环境

### 2. 创建基础服务器
```bash
cargo new my_mcp_server --lib
cd my_mcp_server
```

### 3. 添加MCP依赖
在Cargo.toml中添加：
```toml
[dependencies]
model_context_protocol = "0.1"
tokio = { version = "1.0", features = ["full"] }
```

### 4. 实现核心功能
```rust
use model_context_protocol::{McpServer, Tool, Resource};

struct MyServer;

#[async_trait]
impl McpServer for MyServer {
    async fn get_tools(&self) -> Vec<Tool> {
        vec![
            Tool::new("example_tool", "示例工具")
        ]
    }

    async fn execute_tool(&self, name: &str, args: Value) -> Result<Value> {
        match name {
            "example_tool" => Ok(json!({"result": "success"})),
            _ => Err(anyhow!("未知工具"))
        }
    }
}
```

### 5. 运行服务器
```rust
#[tokio::main]
async fn main() -> Result<()> {
    let server = MyServer;
    server.run("127.0.0.1:8080").await
}
```

### 6. 连接AI助手
1. 启动MCP服务器
2. 在AI助手配置中添加服务器地址
3. 通过工具名称调用自定义功能

## 最佳实践
1. 保持工具接口简单明确
2. 实现完善的错误处理
3. 添加详细的文档说明
4. 进行充分的测试
