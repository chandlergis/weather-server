# Weather Server - MCP天气查询服务

一个基于Model Context Protocol (MCP)的天气查询服务器，提供实时天气信息查询功能。

## 功能特性

- 🌤️ 实时天气信息查询
- 🌍 支持全球城市天气查询
- 🔧 基于MCP协议，易于集成
- 📊 提供详细的天气数据（温度、湿度、风速等）
- 🚀 TypeScript开发，类型安全

## 前置要求

- Node.js 18.0.0 或更高版本
- npm 或 yarn 包管理器
- OpenWeatherMap API密钥

## 安装步骤

### 1. 克隆项目

```bash
git clone https://github.com/chandlergis/weather-server.git
cd weather-server
```

### 2. 安装依赖

```bash
npm install
```

### 3. 配置环境变量

复制环境变量示例文件：

```bash
cp .env.example .env
```

编辑 `.env` 文件，添加您的OpenWeatherMap API密钥：

```env
OPENWEATHER_API_KEY=your_actual_api_key_here
```

### 4. 获取OpenWeatherMap API密钥

1. 访问 [OpenWeatherMap](https://openweathermap.org/api)
2. 注册账户并登录
3. 在API keys页面生成新的API密钥
4. 将密钥复制到 `.env` 文件中

### 5. 构建项目

```bash
npm run build
```

## 使用方法

### 启动服务器

```bash
node build/index.js
```

### MCP客户端配置

在您的MCP客户端配置中添加以下配置：

```json
{
  "mcpServers": {
    "weather": {
      "command": "node",
      "args": ["path/to/weather-server/build/index.js"]
    }
  }
}
```

### 使用示例

服务器提供以下工具：

#### get_weather

获取指定城市的天气信息。

**参数：**
- `city` (string): 城市名称

**示例调用：**
```json
{
  "name": "get_weather",
  "arguments": {
    "city": "北京"
  }
}
```

**返回结果：**
```json
{
  "city": "Beijing",
  "temperature": 25.6,
  "conditions": "晴朗",
  "humidity": 45,
  "wind_speed": 3.2
}
```

## 支持的城市格式

- 中文城市名：北京、上海、广州
- 英文城市名：Beijing、Shanghai、Guangzhou
- 城市,国家：London,UK
- 城市,州,国家：New York,NY,US

## 开发

### 项目结构

```
weather-server/
├── src/
│   └── index.ts          # 主服务器文件
├── build/                # 编译输出目录
├── package.json          # 项目配置
├── tsconfig.json         # TypeScript配置
├── .env.example          # 环境变量示例
└── README.md            # 项目说明
```

### 开发模式

```bash
# 监听文件变化并自动重新编译
npx tsc --watch

# 在另一个终端运行服务器
node build/index.js
```

### 代码结构说明

- `WeatherServer` 类：主服务器类，处理MCP协议通信
- `setupToolHandlers()` 方法：设置工具处理器
- `get_weather` 工具：查询天气信息的核心功能

## API参考

### 环境变量

| 变量名 | 必需 | 描述 |
|--------|------|------|
| `OPENWEATHER_API_KEY` | 是 | OpenWeatherMap API密钥 |

### 工具列表

| 工具名 | 描述 | 参数 |
|--------|------|------|
| `get_weather` | 获取城市天气信息 | `city`: 城市名称 |

## 故障排除

### 常见问题

**Q: 提示"OPENWEATHER_API_KEY environment variable is required"**
A: 请确保已正确设置环境变量，检查 `.env` 文件是否存在且包含有效的API密钥。

**Q: 查询天气时返回"城市未找到"错误**
A: 请检查城市名称拼写，尝试使用英文城市名或添加国家代码。

**Q: API请求失败**
A: 请检查网络连接和API密钥是否有效，确认API密钥未超出使用限制。

### 调试模式

启动服务器时会在stderr输出调试信息：

```bash
node build/index.js 2>debug.log
```

## 贡献指南

1. Fork 本仓库
2. 创建特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 开启 Pull Request

## 许可证

本项目采用 MIT 许可证 - 查看 [LICENSE](LICENSE) 文件了解详情。

## 更新日志

### v1.0.0
- 初始版本发布
- 支持基本天气查询功能
- 集成OpenWeatherMap API
- 完整的MCP协议支持

## 相关链接

- [Model Context Protocol](https://modelcontextprotocol.io/)
- [OpenWeatherMap API](https://openweathermap.org/api)
- [TypeScript](https://www.typescriptlang.org/)

## 支持

如果您遇到问题或有建议，请：

1. 查看[故障排除](#故障排除)部分
2. 搜索现有的[Issues](https://github.com/chandlergis/weather-server/issues)
3. 创建新的Issue描述您的问题

---

**注意：** 请将README中的 `chandlergis` 替换为您的实际GitHub用户名（如果需要）。
