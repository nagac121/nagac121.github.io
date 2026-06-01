---
title: "Create a simple MCP server"
pubDate: 2026-05-29
description: "A Step by step guide to create a simple mcp server"
author: "Naga"
---

# Learn step-by-step how to create an MCP tool using typescript

## Learning Plan (Step-by-step)

### 1. Understand MCP basics first

- MCP server = program that exposes tools/resources/prompts
- Client (like Cursor/Claude Desktop/etc.) connects to it
- Client can call your tools
  Pick a runtime

### 2. Easiest: Node.js + TypeScript (good docs, fast setup)

### 3. Create the smallest project

- initialize npm
- install MCP SDK
- create one file server entrypoint

### 4.Implement one tool only

- Example idea: `echo_message(message: string) -> string`
- Keep schema tiny so you understand how input/output works

### 5. Run it locally

- start the server from terminal
- verify no startup errors

### 6. Connect from a client

- configure client to launch your local MCP server
- call the tool once and confirm response

### 7. Iterate

- Add 2nd tool (e.g., `sum_numbers`)
- Add basic validation and better error messages

# Create a project

- Create a folder at any path with the name `simple-mcp`
- This is your project name

## Tools we are going to create are as follows

`echo_message` → returns same message  
`sum_numbers` → returns sum of two numbers

## Step 1: Initialize the project with required dependencies

```
npm init -y
npm install @modelcontextprotocol/sdk zod
npm install -D typescript tsx @types/node
npx tsc --init
```

### Why these commands

`npm init -y` → creates package.json  
`@modelcontextprotocol/sdk` → MCP server SDK  
`zod` → clean input validation/schema  
`typescript, tsx, @types/node` → run TypeScript easily  
`npx tsc --init` → creates tsconfig.json

## Step 2: Create project files

### Create `src/server.ts`

```ts
import { McpServer } from "@modelcontextprotocol/sdk/server/mcp.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";
import { z } from "zod";

// 1) Create MCP server instance
const server = new McpServer({
  name: "simple-mcp",
  version: "1.0.0",
});

// 2) Register one simple tool: echo_message
server.registerTool(
  "echo_message",
  {
    title: "Echo Message",
    description: "Returns the same message back",
    inputSchema: z.object({
      message: z.string().min(1, "message is required"),
    }),
  },
  async ({ message }) => {
    return {
      content: [
        {
          type: "text",
          text: `Echo: ${message}`,
        },
      ],
    };
  },
);

// 3) connect transport and start listening for messages
async function main() {
  const transport = new StdioServerTransport();
  await server.connect(transport);
  console.error("simple-mcp server is running on stdio");
}

main().catch((error) => {
  console.error("Fatal error in MCP server:", error);
  process.exit(1);
});
```

### Create Package.json

And in package.json, add scripts like below:

```json
"scripts": {
    ...
    "dev": "tsx src/server.ts",
    "build": "tsc",
    "start": "node dist/server.js"
},
```

> TypeScript treats server.ts as CommonJS and rejects import syntax.
> In package.json, change:
> `"type": "commonjs"`  
> to:  
> `"type": "module"`

### tsconfig.json

```ts
  "compilerOptions": {
    "rootDir": "./src",
    "outDir": "./dist",
...
    "types": [
      "node"
    ],
```

in tsconfig.json make sure these options exist.

## Step 3: Run application

`npm run dev`

output:
`simple-mcp server is running on stdio`

## Step 3: connect MCP server to client

Now connect this server to a client so you can actually call echo_message.

`npm run build`
\.cursor\mcp.json

```
{
    "mcpServers": {
        "simple-mcp": {
            "command": "npx",
            "args": [
                "tsx",
                "src/server.ts"
            ]
        }
    }
}
```

- create .cursor folder under root folder
- create mcp.json inside .cursor
- paste above config

Alternative (global for all projects)
C:\Users\<username>\.cursor\mcp.json
Project file is better while learning, because it stays tied to this repo.

What to put in it

```
{
  "mcpServers": {
    "simple-mcp": {
      "command": "npx",
      "args": ["tsx", "src/server.ts"]
    }
  }
}
```

### In Cursor IDE

- Open settings: Ctrl + Shift + J
- Go to Tools & MCP
  - you can find "tools & mcp" option on LHS sidebar
- under "installed MCP Servers" you can see "simple-MCP", just click enable to make the button green color
  - you see simple-mcp because of mcp.json we configured inside .cursor folder in previously
- After saving:
  - Restart Cursor (alt + f4) or
  - reload window (ctrl+shift+p) and Run command: Developer: Reload Window

- Open chat
  - put switch agent mode as "agent" (ctrl+.) and switch model (ctrl+/) as "auto"
  - copy paste this exacctly `Use echo_message with {"message":"hello"}`
  - you will see the echo `Echo: hello`

## step 4: Add another to tool

```ts
// 2b) Register second tool: sum_numbers
server.registerTool(
  "sum_numbers",
  {
    title: "Sum Numbers",
    description: "Adds two numbers and returns the result",
    inputSchema: z.object({
      a: z.number(),
      b: z.number(),
    }),
  },
  async ({ a, b }) => {
    const sum = a + b;
    return {
      content: [
        {
          type: "text",
          text: `Sum: ${sum}`,
        },
      ],
    };
  },
);
```

add this just below echo_message fn

> code explanation

- "sum_numbers": Tool name the client calls
- title / description: Shown in MCP tool list
- inputSchema: z.object({ a, b }): Client must send two numbers
- z.number(): Validates type (rejects strings)
- async ({ a, b }) =>: Handler receives validated args
- a + b: Your logic
- content: [{ type: "text", text: ... }]: Same response shape as echo_message

follow restart cursor procedure above and ping this in the chat
`Use sum_numbers with {"a": 3, "b": 7}`  
result: sum result

> Full code

src\server.ts

```ts
import { McpServer } from "@modelcontextprotocol/sdk/server/mcp.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";
import { z } from "zod";

// 1) Create MCP server instance
const server = new McpServer({
  name: "simple-mcp",
  version: "1.0.0",
});

// 2) Register one simple tool: echo_message
server.registerTool(
  "echo_message",
  {
    title: "Echo Message",
    description: "Returns the same message back",
    inputSchema: z.object({
      message: z.string().min(1, "message is required"),
    }),
  },
  async ({ message }) => {
    return {
      content: [
        {
          type: "text",
          text: `Echo: ${message}`,
        },
      ],
    };
  },
);

// 2b) Register second tool: sum_numbers
server.registerTool(
  "sum_numbers",
  {
    title: "Sum Numbers",
    description: "Adds two numbers and returns the result",
    inputSchema: z.object({
      a: z.number(),
      b: z.number(),
    }),
  },
  async ({ a, b }) => {
    const sum = a + b;
    return {
      content: [
        {
          type: "text",
          text: `Sum: ${sum}`,
        },
      ],
    };
  },
);

// 3) connect transport and start listening for messages
async function main() {
  const transport = new StdioServerTransport();
  await server.connect(transport);
  console.error("simple-mcp server is running on stdio");
}

main().catch((error) => {
  console.error("Fatal error in MCP server:", error);
  process.exit(1);
});
```
