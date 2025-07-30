# Open Deep Research

<img width="1388" height="298" alt="full_diagram" src="https://github.com/user-attachments/assets/12a2371b-8be2-4219-9b48-90503eb43c69" />

深度研究已经成为最受欢迎的智能体应用之一。这是一个简单、可配置、完全开源的深度研究智能体，支持多种模型提供商、搜索工具和 MCP 服务器。

* 阅读我们的[博客](https://blog.langchain.com/open-deep-research/)了解更多信息
* 观看我们的[视频](https://www.youtube.com/watch?v=agGiWUpxkhg)快速了解

### 🚀 快速开始

1. 克隆仓库并激活虚拟环境：
```bash
git clone https://github.com/langchain-ai/open_deep_research.git
cd open_deep_research
uv venv
source .venv/bin/activate  # Windows 系统: .venv\Scripts\activate
```

2. 安装依赖：
```bash
uv pip install -r pyproject.toml
```

3. 设置 `.env` 文件来自定义环境变量（用于模型选择、搜索工具和其他配置设置）：
```bash
cp .env.example .env
```

4. 在本地启动 LangGraph 服务器以在浏览器中打开 LangGraph Studio：

```bash
# 安装依赖并启动 LangGraph 服务器
uvx --refresh --from "langgraph-cli[inmem]" --with-editable . --python 3.11 langgraph dev --allow-blocking
```

使用以下链接打开 Studio UI：
```
- 🚀 API: http://127.0.0.1:2024
- 🎨 Studio UI: https://smith.langchain.com/studio/?baseUrl=http://127.0.0.1:2024
- 📚 API 文档: http://127.0.0.1:2024/docs
```
<img width="817" height="666" alt="Screenshot 2025-07-13 at 11 21 12 PM" src="https://github.com/user-attachments/assets/052f2ed3-c664-4a4f-8ec2-074349dcaa3f" />

在 `messages` 输入框中提出问题并点击 `Submit`。

### 配置选项

Open Deep Research 提供了广泛的配置选项来自定义研究过程和模型行为。所有配置都可以通过 Web UI、环境变量或直接修改配置来设置。

#### 通用设置

- **最大结构化输出重试次数** (默认: 3)：当解析失败时，模型结构化输出调用的最大重试次数
- **允许澄清** (默认: true)：是否允许研究者在开始研究前询问澄清问题
- **最大并发研究单元** (默认: 5)：使用子智能体并发运行的最大研究单元数量。更高的值可以实现更快的研究，但可能会触及速率限制

#### 研究配置

- **搜索 API** (默认: Tavily)：可选择 Tavily（适用于所有模型）、OpenAI 原生网络搜索、Anthropic 原生网络搜索或无
- **最大研究者迭代次数** (默认: 3)：研究监督者反思研究并提出后续问题的次数
- **最大 React 工具调用次数** (默认: 5)：单个研究者步骤中工具调用迭代的最大次数

#### 模型

Open Deep Research 使用多个专门的模型来处理不同的研究任务：

- **总结模型** (默认: `openai:gpt-4.1-nano`)：总结来自搜索 API 的研究结果
- **研究模型** (默认: `openai:gpt-4.1`)：进行研究和分析
- **压缩模型** (默认: `openai:gpt-4.1-mini`)：压缩来自子智能体的研究发现
- **最终报告模型** (默认: `openai:gpt-4.1`)：编写最终的综合报告

所有模型都使用 [init_chat_model() API](https://python.langchain.com/docs/how_to/chat_models_universal_init/) 进行配置，支持 OpenAI、Anthropic、Google Vertex AI 等提供商。

**重要的模型要求：**

1. **结构化输出**：所有模型都必须支持结构化输出。在[这里](https://python.langchain.com/docs/integrations/chat/)检查支持情况。

2. **搜索 API 兼容性**：研究和压缩模型必须支持您选择的搜索 API：
   - Anthropic 搜索需要具有网络搜索功能的 Anthropic 模型
   - OpenAI 搜索需要具有网络搜索功能的 OpenAI 模型
   - Tavily 适用于所有模型

3. **工具调用**：所有模型都必须支持工具调用功能

4. **特殊配置**：
   - 对于 OpenRouter：遵循[此指南](https://github.com/langchain-ai/open_deep_research/issues/75#issuecomment-2811472408)
   - 对于通过 Ollama 的本地模型：查看[设置说明](https://github.com/langchain-ai/open_deep_research/issues/65#issuecomment-2743586318)

#### MCP (模型上下文协议) 服务器示例

Open Deep Research 支持 MCP 服务器来扩展研究能力。

#### 本地 MCP 服务器

**文件系统 MCP 服务器** 提供具有强大访问控制的安全文件系统操作：
- 读取、写入和管理文件和目录
- 执行读取文件内容、创建目录、移动文件和搜索等操作
- 为了安全起见，将操作限制在预定义的目录中
- 支持命令行配置和动态 MCP 根目录

使用示例：
```bash
mcp-server-filesystem /path/to/allowed/dir1 /path/to/allowed/dir2
```

#### 远程 MCP 服务器

**远程 MCP 服务器** 支持分布式智能体协调和可流式传输的 HTTP 请求。与本地服务器不同，它们可以是多租户的，需要更复杂的身份验证。

**Arcade MCP 服务器示例**：
```json
{
  "url": "https://api.arcade.dev/v1/mcps/ms_0ujssxh0cECutqzMgbtXSGnjorm",
  "tools": ["Search_SearchHotels", "Search_SearchOneWayFlights", "Search_SearchRoundtripFlights"]
}
```

远程服务器可以配置为需要身份验证或不需要身份验证，并通过 OAuth 端点支持基于 JWT 的身份验证。

### 评估

为详细分析和比较研究设计的综合批量评估系统。

#### **功能特性：**
- **多维度评分**：具有 0-1 量表评级的专门评估器
- **数据集驱动评估**：跨多个测试用例的批量处理

#### **使用方法：**
```bash
# 在 LangSmith 数据集上运行综合评估
python tests/run_evaluate.py
```
#### **关键文件：**
- `tests/run_evaluate.py`：主要评估脚本
- `tests/evaluators.py`：专门的评估器函数
- `tests/prompts.py`：每个维度的评估提示词

### 部署和使用

#### LangGraph Studio

按照[快速开始](#-快速开始)在本地启动 LangGraph 服务器，并在 LangGraph Studio 上测试智能体。

#### 托管部署

您可以轻松部署到 [LangGraph Platform](https://langchain-ai.github.io/langgraph/concepts/#deployment-options)。

#### Open Agent Platform

Open Agent Platform (OAP) 是一个 UI，非技术用户可以从中构建和配置自己的智能体。OAP 非常适合让用户配置深度研究者，使用最适合他们需求和要解决问题的不同 MCP 工具和搜索 API。

我们已经将 Open Deep Research 部署到我们的 OAP 公共演示实例。您只需要添加您的 API 密钥，就可以自己测试深度研究者！在[这里](https://oap.langchain.com)试用。

您也可以部署自己的 OAP 实例，并将您自己的自定义智能体（如深度研究者）提供给您的用户。
1. [部署 Open Agent Platform](https://docs.oap.langchain.com/quickstart)
2. [将深度研究者添加到 OAP](https://docs.oap.langchain.com/setup/agents)

### 更新 🔥

### 遗留实现 🏛️

`src/legacy/` 文件夹包含两个早期实现，提供了自动化研究的替代方法：

#### 1. 工作流实现 (`legacy/graph.py`)
- **计划和执行**：具有人工参与规划的结构化工作流
- **顺序处理**：通过反思逐个创建章节
- **交互控制**：允许对报告计划进行反馈和批准
- **质量导向**：通过迭代改进强调准确性

#### 2. 多智能体实现 (`legacy/multi_agent.py`)
- **监督者-研究者架构**：协调的多智能体系统
- **并行处理**：多个研究者同时工作
- **速度优化**：通过并发实现更快的报告生成
- **MCP 支持**：广泛的模型上下文协议集成

有关两种遗留实现的详细文档、配置选项和使用示例，请参阅 `src/legacy/legacy.md`。
