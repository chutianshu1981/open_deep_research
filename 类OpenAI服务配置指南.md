# 类 OpenAI 服务配置指南

## 为什么需要多个 API 密钥？

### 🎯 简单回答：其实你可以只用一个！

Open Deep Research 支持多个 API 密钥是为了：
1. **成本优化**：不同任务用不同价格的模型
2. **性能优化**：不同模型擅长不同任务  
3. **容错备份**：一个服务挂了可以切换
4. **功能互补**：某些功能只有特定提供商支持

**但是，你完全可以只配置一个 OpenAI API 密钥来运行整个系统！**

## 🚀 最简配置（只用一个 API 密钥）

### 方案一：只用 OpenAI
```bash
# .env 文件
OPENAI_API_KEY=sk-your-openai-key-here
TAVILY_API_KEY=tvly-your-tavily-key-here  # 搜索用
LANGSMITH_TRACING=false
GET_API_KEYS_FROM_CONFIG=false

# 其他的都可以留空
ANTHROPIC_API_KEY=
GOOGLE_API_KEY=
LANGSMITH_API_KEY=
LANGSMITH_PROJECT=
```

系统会自动使用 OpenAI 的模型来处理所有任务：
- 总结模型：`openai:gpt-4.1-nano`
- 研究模型：`openai:gpt-4.1`
- 压缩模型：`openai:gpt-4.1-mini`
- 最终报告模型：`openai:gpt-4.1`

## 🔧 配置类 OpenAI 兼容服务

### 支持的类 OpenAI 服务
- **OpenRouter**：聚合多个模型提供商
- **Together AI**：开源模型托管
- **Groq**：超快推理速度
- **DeepSeek**：中国的 AI 服务
- **Ollama**：本地模型服务
- **vLLM**：自部署的模型服务
- **LocalAI**：本地 AI 服务

### 配置方法

#### 方法一：通过环境变量配置 BASE_URL

```bash
# .env 文件
OPENAI_API_KEY=your-service-api-key
OPENAI_BASE_URL=https://your-service-endpoint.com/v1  # 关键配置

# 例如 OpenRouter
OPENAI_API_KEY=sk-or-v1-your-openrouter-key
OPENAI_BASE_URL=https://openrouter.ai/api/v1

# 例如 Together AI
OPENAI_API_KEY=your-together-key
OPENAI_BASE_URL=https://api.together.xyz/v1

# 例如本地 Ollama
OPENAI_API_KEY=ollama  # 可以是任意值
OPENAI_BASE_URL=http://localhost:11434/v1
```

#### 方法二：通过模型字符串配置

Open Deep Research 使用 `init_chat_model()` API，支持多种配置方式：

```python
# 在配置中可以这样指定
"openai:gpt-4"  # 标准 OpenAI
"openrouter:anthropic/claude-3.5-sonnet"  # OpenRouter
"together:meta-llama/Llama-3-8b-chat-hf"  # Together AI
"groq:llama3-8b-8192"  # Groq
"ollama:llama3"  # 本地 Ollama
```

### 具体服务配置示例

#### 1. OpenRouter 配置
```bash
# .env
OPENAI_API_KEY=sk-or-v1-your-openrouter-key
OPENAI_BASE_URL=https://openrouter.ai/api/v1

# 或者在 UI 中配置模型为：
# research_model: "openrouter:anthropic/claude-3.5-sonnet"
# compression_model: "openrouter:openai/gpt-4o-mini"
```

#### 2. Together AI 配置
```bash
# .env
OPENAI_API_KEY=your-together-api-key
OPENAI_BASE_URL=https://api.together.xyz/v1

# 模型配置：
# research_model: "together:meta-llama/Llama-3-70b-chat-hf"
```

#### 3. 本地 Ollama 配置
```bash
# .env
OPENAI_API_KEY=ollama
OPENAI_BASE_URL=http://localhost:11434/v1

# 模型配置：
# research_model: "ollama:llama3"
# compression_model: "ollama:llama3:8b"
```

#### 4. DeepSeek 配置
```bash
# .env
OPENAI_API_KEY=your-deepseek-api-key
OPENAI_BASE_URL=https://api.deepseek.com/v1

# 模型配置：
# research_model: "deepseek:deepseek-chat"
```

## 🎛️ 在 LangGraph Studio 中配置

启动服务后，在 LangGraph Studio 的配置面板中：

1. **General Settings**：
   - Max Concurrent Research Units: 5
   - Allow Clarification: true

2. **Research Configuration**：
   - Search API: Tavily（推荐，兼容所有模型）

3. **Models**：
   - Summarization Model: `your-service:model-name`
   - Research Model: `your-service:model-name`
   - Compression Model: `your-service:model-name`
   - Final Report Model: `your-service:model-name`

## ⚠️ 重要注意事项

### 模型要求
所有模型必须支持：
1. **结构化输出**：能够输出 JSON 格式
2. **工具调用**：支持 Function Calling
3. **足够的上下文长度**：至少 8K tokens

### 兼容性检查
不是所有类 OpenAI 服务都完全兼容，建议测试：
- 结构化输出是否正常
- 工具调用是否工作
- 错误处理是否正确

### 成本考虑
- **OpenAI**：质量最高，成本较高
- **OpenRouter**：多模型选择，价格灵活
- **Together AI**：开源模型，成本较低
- **Ollama**：本地运行，免费但需要硬件

## 🔍 搜索 API 说明

### 为什么需要搜索 API？
研究智能体需要获取最新信息，搜索 API 提供：
- 实时网络搜索
- 学术论文搜索
- 新闻和资讯获取

### 搜索选项
1. **Tavily**（推荐）：
   - 兼容所有模型
   - 专为 AI 优化
   - 结果质量高

2. **OpenAI/Anthropic 原生搜索**：
   - 需要对应模型支持
   - 集成度更高
   - 可能有使用限制

3. **无搜索**：
   - 只能基于模型训练数据
   - 信息可能过时
   - 不推荐用于研究任务

## 🚀 推荐配置

### 预算充足
```bash
OPENAI_API_KEY=sk-your-openai-key
TAVILY_API_KEY=tvly-your-tavily-key
LANGSMITH_TRACING=false
```

### 预算有限
```bash
OPENAI_API_KEY=sk-or-v1-your-openrouter-key
OPENAI_BASE_URL=https://openrouter.ai/api/v1
TAVILY_API_KEY=tvly-your-tavily-key
LANGSMITH_TRACING=false

# 在 UI 中配置便宜的模型：
# research_model: "openrouter:openai/gpt-4o-mini"
# compression_model: "openrouter:openai/gpt-4o-mini"
```

### 本地部署
```bash
OPENAI_API_KEY=ollama
OPENAI_BASE_URL=http://localhost:11434/v1
TAVILY_API_KEY=tvly-your-tavily-key
LANGSMITH_TRACING=false

# 确保 Ollama 已安装并运行：
# ollama serve
# ollama pull llama3
```

现在你可以根据自己的需求选择合适的配置方案了！
