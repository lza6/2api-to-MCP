# 2api-to-MCP
这是一个可以把你2API项目（不管是docker还是cfwork等等工作流代码）来一键通过AI发送转成GitHub仓库的MCP工具，也就是你只需要按照AI说的那样去你的仓库创建对应的文件，接着用这个json快速导入你的MCP就完工了

## 下面这个代码是最终成品（也就是你上传完了仓库然后把对应的用户名和仓库名替换成你的仓库即可，记得项目名称换成你想要的MCP工具名称噢）
```
{
  "mcpServers": {
    "项目名称（你自己改一下定义一下）": {
      "command": "npx",
      "args": [
        "-y",
        "github:用户名/仓库名"
      ],
      "env": {}
    }
  }
}
```


## 生成指令（记得把你的项目代码追加到提示词的下面，这样可以让AI更清楚识别）：

```
角色扮演：
你是一位世界顶级的首席开发者体验架构师 (Principal Developer Experience Architect)，完全掌握、具备MCP等等工具的封装代码的生成转换以及使用和了解最新的MCP文档教程案例说明

核心任务：
我将提供一个完整的项目源代码（你可以根据我提供的项目自动转换自动识别哈，有可能是c++项目也有可能是docker等等那些项目）。你的任务是：
将我的项目完整、无损地迁移、转换为MCP工具，下面我会给出一些案例你方可查看，我是直接放在GitHub仓库上的，接着呢通过studio等等那样的调用，具体你可以看我下方提供的项目来自动学习自动深度了解一下我主要是怎样做的？
示例项目：
index.js文件：
#!/usr/bin/env node
import { Server } from "@modelcontextprotocol/sdk/server/index.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";
import { CallToolRequestSchema, ListToolsRequestSchema } from "@modelcontextprotocol/sdk/types.js";

const CONFIG = {
  UPSTREAM_URL: "https://perfectassistant.ai/ai/free",
  ORIGIN_URL: "https://perfectassistant.ai",
  USER_AGENT: "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/122.0.0.0 Safari/537.36"
};

const server = new Server(
  { name: "perfect-assistant-mcp", version: "1.0.0" },
  { capabilities: { tools: {} } }
);

server.setRequestHandler(ListToolsRequestSchema, async () => {
  return {
    tools: [{
      name: "ask_perfect_assistant",
      description: "【智能创作】调用 AI 进行写作。支持：周报、邮件、小红书文案、头脑风暴等。",
      inputSchema: {
        type: "object",
        properties: {
          prompt: { type: "string", description: "你的指令" }
        },
        required: ["prompt"]
      }
    }]
  };
});

server.setRequestHandler(CallToolRequestSchema, async (request) => {
  if (request.params.name === "ask_perfect_assistant") {
    const prompt = request.params.arguments.prompt;
    try {
      const aiResponse = await callUpstreamAI(prompt);
      return { content: [{ type: "text", text: aiResponse }] };
    } catch (error) {
      return { content: [{ type: "text", text: "Error: " + error.message }], isError: true };
    }
  }
  throw new Error("Tool not found");
});

async function callUpstreamAI(prompt) {
  const response = await fetch(CONFIG.UPSTREAM_URL, {
    method: "POST",
    headers: {
      "Content-Type": "text/plain;charset=UTF-8",
      "Origin": CONFIG.ORIGIN_URL,
      "Referer": `${CONFIG.ORIGIN_URL}/iframe/brainstorm-tool`,
      "User-Agent": CONFIG.USER_AGENT,
    },
    body: JSON.stringify({
      tone: "professional", language: "chinese", text: prompt,
      chatId: crypto.randomUUID(), id: "brainstorm-tool"
    }),
  });
  const data = await response.json();
  return data.response || (data.responses && data.responses[0]) || "无内容";
}

async function run() {
  const transport = new StdioServerTransport();
  await server.connect(transport);
}

run().catch(console.error);

package.json文件：
{
  "name": "perfect-assistant-mcp",
  "version": "1.0.0",
  "type": "module",
  "bin": {
    "perfect-assistant-mcp": "./index.js"
  },
  "dependencies": {
    "@modelcontextprotocol/sdk": "^1.0.1"
  }
}
看来这个项目就用了两个文件轻松解决

其中这个项目name等等你可以根据我下面提供的源项目来帮我进行自动分析自动命令给我，并且注释也要中文化哈，还有就是工具名称提示词啊资源啊等等那些都要中文化汉化，这样能极大帮助一些中文爱好者来更容易了解项目
下方是我需要转换的项目完整代码：






```
