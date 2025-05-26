# 使用示例 - Weather Server

本文档提供weather-server的详细使用示例和集成方法。

## 基本使用

### 1. 启动服务器

```bash
# 确保已构建项目
npm run build

# 启动服务器
npm start
```

服务器启动后会显示：
```
天气MCP服务器已启动
```

### 2. MCP客户端配置

#### Claude Desktop配置示例

在Claude Desktop的配置文件中添加：

```json
{
  "mcpServers": {
    "weather": {
      "command": "node",
      "args": ["C:/Users/chandler/Documents/Cline/MCP/weather-server/build/index.js"],
      "env": {
        "OPENWEATHER_API_KEY": "your_api_key_here"
      }
    }
  }
}
```

#### 其他MCP客户端配置

```json
{
  "servers": {
    "weather-server": {
      "command": "node",
      "args": ["path/to/weather-server/build/index.js"],
      "cwd": "path/to/weather-server"
    }
  }
}
```

## API使用示例

### get_weather 工具

#### 基本查询

**输入：**
```json
{
  "name": "get_weather",
  "arguments": {
    "city": "北京"
  }
}
```

**输出：**
```json
{
  "city": "Beijing",
  "temperature": 15.2,
  "conditions": "多云",
  "humidity": 65,
  "wind_speed": 2.1
}
```

#### 国际城市查询

**查询伦敦天气：**
```json
{
  "name": "get_weather",
  "arguments": {
    "city": "London,UK"
  }
}
```

**查询纽约天气：**
```json
{
  "name": "get_weather",
  "arguments": {
    "city": "New York,NY,US"
  }
}
```

#### 中文城市查询

```json
{
  "name": "get_weather",
  "arguments": {
    "city": "上海"
  }
}
```

```json
{
  "name": "get_weather",
  "arguments": {
    "city": "广州"
  }
}
```

## 编程集成示例

### Node.js客户端示例

```javascript
import { Client } from '@modelcontextprotocol/sdk/client/index.js';
import { StdioClientTransport } from '@modelcontextprotocol/sdk/client/stdio.js';
import { spawn } from 'child_process';

async function createWeatherClient() {
  // 启动weather server进程
  const serverProcess = spawn('node', ['build/index.js'], {
    cwd: '/path/to/weather-server',
    env: {
      ...process.env,
      OPENWEATHER_API_KEY: 'your_api_key'
    }
  });

  // 创建MCP客户端
  const transport = new StdioClientTransport({
    stdin: serverProcess.stdin,
    stdout: serverProcess.stdout
  });

  const client = new Client({
    name: 'weather-client',
    version: '1.0.0'
  }, {
    capabilities: {}
  });

  await client.connect(transport);
  return client;
}

async function getWeather(city) {
  const client = await createWeatherClient();
  
  try {
    const result = await client.request({
      method: 'tools/call',
      params: {
        name: 'get_weather',
        arguments: { city }
      }
    });
    
    return JSON.parse(result.content[0].text);
  } finally {
    await client.close();
  }
}

// 使用示例
getWeather('北京').then(weather => {
  console.log(`${weather.city}的天气：`);
  console.log(`温度：${weather.temperature}°C`);
  console.log(`天气：${weather.conditions}`);
  console.log(`湿度：${weather.humidity}%`);
  console.log(`风速：${weather.wind_speed} m/s`);
});
```

### Python客户端示例

```python
import json
import subprocess
import asyncio
from mcp import ClientSession, StdioServerParameters
from mcp.client.stdio import stdio_client

async def get_weather(city: str):
    # 启动weather server
    server_params = StdioServerParameters(
        command="node",
        args=["build/index.js"],
        env={"OPENWEATHER_API_KEY": "your_api_key"}
    )
    
    async with stdio_client(server_params) as (read, write):
        async with ClientSession(read, write) as session:
            # 初始化
            await session.initialize()
            
            # 调用get_weather工具
            result = await session.call_tool("get_weather", {"city": city})
            
            # 解析结果
            weather_data = json.loads(result.content[0].text)
            return weather_data

# 使用示例
async def main():
    weather = await get_weather("上海")
    print(f"{weather['city']}的天气：")
    print(f"温度：{weather['temperature']}°C")
    print(f"天气：{weather['conditions']}")
    print(f"湿度：{weather['humidity']}%")
    print(f"风速：{weather['wind_speed']} m/s")

if __name__ == "__main__":
    asyncio.run(main())
```

## 错误处理示例

### 常见错误及处理

#### 1. 城市未找到

**输入：**
```json
{
  "name": "get_weather",
  "arguments": {
    "city": "不存在的城市"
  }
}
```

**错误响应：**
```json
{
  "error": {
    "code": -32603,
    "message": "天气API错误: city not found"
  }
}
```

#### 2. API密钥无效

**错误响应：**
```json
{
  "error": {
    "code": -32603,
    "message": "天气API错误: Invalid API key"
  }
}
```

#### 3. 网络错误

**错误响应：**
```json
{
  "error": {
    "code": -32603,
    "message": "天气API错误: Network Error"
  }
}
```

### 错误处理最佳实践

```javascript
async function safeGetWeather(city) {
  try {
    const weather = await getWeather(city);
    return { success: true, data: weather };
  } catch (error) {
    console.error('获取天气失败:', error.message);
    
    if (error.message.includes('city not found')) {
      return { 
        success: false, 
        error: '城市未找到，请检查城市名称拼写' 
      };
    } else if (error.message.includes('Invalid API key')) {
      return { 
        success: false, 
        error: 'API密钥无效，请检查配置' 
      };
    } else {
      return { 
        success: false, 
        error: '网络错误，请稍后重试' 
      };
    }
  }
}
```

## 高级用法

### 批量查询多个城市

```javascript
async function getMultipleCityWeather(cities) {
  const results = await Promise.allSettled(
    cities.map(city => safeGetWeather(city))
  );
  
  return results.map((result, index) => ({
    city: cities[index],
    ...result.value
  }));
}

// 使用示例
const cities = ['北京', '上海', '广州', '深圳'];
const weatherData = await getMultipleCityWeather(cities);

weatherData.forEach(({ city, success, data, error }) => {
  if (success) {
    console.log(`${city}: ${data.temperature}°C, ${data.conditions}`);
  } else {
    console.log(`${city}: ${error}`);
  }
});
```

### 天气数据缓存

```javascript
class WeatherCache {
  constructor(ttl = 300000) { // 5分钟缓存
    this.cache = new Map();
    this.ttl = ttl;
  }
  
  async getWeather(city) {
    const cacheKey = city.toLowerCase();
    const cached = this.cache.get(cacheKey);
    
    if (cached && Date.now() - cached.timestamp < this.ttl) {
      return cached.data;
    }
    
    const weather = await getWeather(city);
    this.cache.set(cacheKey, {
      data: weather,
      timestamp: Date.now()
    });
    
    return weather;
  }
}

const weatherCache = new WeatherCache();
```

## 测试示例

### 单元测试

```javascript
import { describe, it, expect, beforeAll, afterAll } from '@jest/globals';

describe('Weather Server', () => {
  let client;
  
  beforeAll(async () => {
    client = await createWeatherClient();
  });
  
  afterAll(async () => {
    await client.close();
  });
  
  it('should get weather for Beijing', async () => {
    const weather = await getWeather('Beijing');
    
    expect(weather).toHaveProperty('city');
    expect(weather).toHaveProperty('temperature');
    expect(weather).toHaveProperty('conditions');
    expect(weather).toHaveProperty('humidity');
    expect(weather).toHaveProperty('wind_speed');
    
    expect(typeof weather.temperature).toBe('number');
    expect(typeof weather.humidity).toBe('number');
    expect(typeof weather.wind_speed).toBe('number');
  });
  
  it('should handle invalid city', async () => {
    await expect(getWeather('InvalidCity123')).rejects.toThrow();
  });
});
```

## 性能优化建议

1. **使用连接池**：避免频繁创建/销毁MCP连接
2. **实现缓存**：避免重复查询相同城市
3. **批量处理**：合并多个请求
4. **错误重试**：实现指数退避重试机制
5. **监控日志**：记录API调用次数和响应时间

---

这些示例应该能帮助您快速上手使用weather-server！
