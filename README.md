# Prompt Context

Files to use to set context for LLMs, generally included on demand in configuration.

For example, in Q CLI, you can add a reference to a file in your configuration JSON:

```
"resources": [
    "file:///path/to/my-coding-style.md"
],
```

The file path should be absolute, and start with a `/`. the `file://` part is the URI scheme, so a path will start `file:///`.

I use [agents](https://docs.aws.amazon.com/amazonq/latest/qdeveloper-ug/command-line-custom-agents.html) to store different configs. These are the agents I have setup:

```bash
❯ q agent list
writer          /Users/myusername/.aws/amazonq/cli-agents
dev             /Users/myusername/.aws/amazonq/cli-agents
sa              /Users/myusername/.aws/amazonq/cli-agents
❯ cat /Users/ctod/.aws/amazonq/cli-agents/dev.json     
```
Each agent has its own config. I also import some [MCP servers](https://github.com/awslabs/mcp) for given profiles (for example):

```json
{
  "name": "dev",
  "description": "",
  "prompt": null,
  "mcpServers": {
  "awslabs.aws-documentation-mcp-server": {
      "command": "uvx",
      "args": ["awslabs.aws-documentation-mcp-server"],
      "env": {
        "FASTMCP_LOG_LEVEL": "ERROR",
        "AWS_DOCUMENTATION_PARTITION": "aws"
      },
      "disabled": false,
      "autoApprove": []
    },
    "awslabs.cdk-mcp-server": {
      "command": "uvx",
      "args": ["awslabs.cdk-mcp-server"],
      "env": {
        "FASTMCP_LOG_LEVEL": "ERROR"
      },
      "disabled": false,
      "autoApprove": []
    }
  },
  "tools": [
    "*"
  ],
  "toolAliases": {},
  "allowedTools": [
    "fs_read"
  ],
  "resources": [
    "file:///Users/ctod/.aws/amazonq/prompts/my-coding-style.md"
  ],
  "hooks": {},
  "toolsSettings": {},
  "useLegacyMcpJson": true
}
```

I can invoke them like:

```bash
> q agent --agent dev
```

I alias them to make it a little easier to type :)

```bash
alias q-dev="q chat --agent dev"
```
