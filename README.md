# Prompt context to help our lovely LLMs


Includes:

- [Custom CLI agents](https://docs.aws.amazon.com/amazonq/latest/qdeveloper-ug/command-line-custom-agents-defining.html) for Amazon Q CLI
- Prebaked context for those agents
- Useful MCPs, with read-only tools trusted, for each agent.

For example, a dev agent will import `prompts/my-coding-style.md` to set context, while an SA agent could import `prompts/sfdc.md`.

# Q CLI cli-agents

Put or symlink configs into `~/.aws/amazonq/cli-agents/`.

You can then use them by passing an agent config flag, for example:

```bash
q chat --agent dev
```

## dev.json

Used for developing software on AWS, especially in CDK.

Uses:

- context7 mcp - https://github.com/upstash/context7
- From https://github.com/awslabs/mcp
    - awslabs.aws-documentation-mcp-server
    - awslabs.cdk-mcp-server
- Context from `prompts/my-coding-style.md`.


## Usage Tips


- *¡Remember!* - You can type `--resume` in any Q chat session to pick up your conversation where you left off. This functionality is keyed by working directory.
  - For example, if I am working on some code in `~/code/my-cool-project`, I can `cd` back to that directory _at any time_ and restart my chat session where I left off with `q chat --resume` (or, usually, in my case, `q chat --agent dev --resume`). When I restart the session, Q will summarise our previous conversation and have all the same context from our previous conversation.
  - I use this _extensively_, to the point where I have empty directories in my `/code/` folder just to use as keys for remembering context (eg `camerontod.com-r53-management`). I `cd` into that directoy and `q chat --resume` so that I don't need to re-explain everything.
  - I *think* this conversation is kept in `~/.aws/amazonq/history`, for reference.

- I tend to set up read-only AWS creds for when I want to muck around with AWS resources directly. Q is really good at debugging, for example checking a chain of services like cloudwatch->sns->sqs->lambda->chatbot to make sure permissions and events are in the right format. This is a lot nicer than doing that all manually and trying to keep it in your head.


- You can load special context (for example, your source code) with `/knowledge add <path>`.

- You can see your usage, and possibly compact it to make the most of your token context window, with `/usage`.

- You can explicitly save or load conversation history with `/save` or `/load`. I prefer the directory-based memory I spoke about above and don't really use this.
