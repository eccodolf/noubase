# Noubase Search

`noubase-search` is an [Agent Skill](skills/noubase-search/SKILL.md) for finding and verifying evidence in any project indexed by Noubase. It includes workflows and call examples for all 18 Noubase MCP tools.

Install the skill in your client and connect that client to your Noubase MCP instance. Use the endpoint and authentication method supplied by your Noubase administrator. In the examples below, `NOUBASE_MCP_URL` holds that endpoint.

## Install

### Claude Code

```sh
claude plugin marketplace add eccodolf/noubase
claude plugin install noubase@eccodolf-noubase
```

Invoke with `/noubase:noubase-search`. For a manual install, copy the skill into `~/.claude/skills/` or the project's `.claude/skills/`. [Claude Code plugins](https://code.claude.com/docs/en/plugins) · [Skill paths](https://code.claude.com/docs/en/skills#choose-where-skills-load)

For Claude's custom skill upload, clone the repository and run `(cd skills && zip -r ../noubase-search.zip noubase-search)` from its root. Upload the resulting ZIP under **Customize → Skills**. Use a Noubase connector reachable from that Claude environment. [Custom skill uploads](https://support.claude.com/en/articles/12512198-how-to-create-custom-skills)

### Codex

```sh
codex plugin marketplace add eccodolf/noubase
codex plugin add noubase@eccodolf-noubase
```

Select the skill or mention `$noubase-search`. To install it as a standalone skill, give the built-in `$skill-installer` the [skill directory URL](https://github.com/eccodolf/noubase/tree/main/skills/noubase-search), or copy the directory to `~/.agents/skills/` or the project's `.agents/skills/`. [Codex skills](https://developers.openai.com/codex/skills) · [Plugin package format](https://developers.openai.com/plugins/build/plugins)

### Kimi Code CLI

In the current Kimi Code CLI, enter these commands in order.

```text
/plugins install https://github.com/eccodolf/noubase
/reload
/skill:noubase:noubase-search
```

For a manual install, use `${KIMI_CODE_HOME:-$HOME/.kimi-code}/skills/` or the project's `.kimi-code/skills/`. The older Python Kimi CLI uses `~/.kimi/skills/` or the project's `.kimi/skills/` and invokes the standalone skill with `/skill:noubase-search`. [Kimi Code plugins](https://github.com/MoonshotAI/kimi-code/blob/main/docs/en/customization/plugins.md) · [Python Kimi CLI skills](https://github.com/MoonshotAI/kimi-cli/blob/main/docs/en/customization/skills.md)

### DeepSeek Harness (DSH)

Run these commands from a local clone to install the skill into `${DSH_HOME:-$HOME/.dsh}/skills/`.

```sh
dsh_skills="${DSH_HOME:-$HOME/.dsh}/skills"
mkdir -p "$dsh_skills"
cp -R skills/noubase-search "$dsh_skills/"
```

For a project install, use its `.dsh/skills/` directory. The filesystem provider also discovers skills under `.agents/skills/`. Invoke with `/noubase-search`. [DSH skill directories](https://github.com/deepseek-ai/deepseek-harness/blob/master/packages/skill/skill-filesystem/README.md#roots-and-priority)

### Qwen Code

```sh
qwen extensions install eccodolf/noubase
```

Invoke with `/noubase:noubase-search`. For a standalone install, copy the directory to `~/.qwen/skills/`, the project's `.qwen/skills/`, or a shared `.agents/skills/` directory. Invoke the standalone skill with `/noubase-search`. [Qwen Agent Plugins](https://qwenlm.github.io/qwen-code-docs/en/users/extension/agent-plugins/) · [Skill paths](https://qwenlm.github.io/qwen-code-docs/en/users/features/skills/)

### OpenClaw

Run these commands from a local clone.

```sh
openclaw skills install ./skills/noubase-search
openclaw skills info noubase-search
```

You can also copy the directory into the OpenClaw workspace's `skills/` folder. Ask the agent to use `noubase-search` for a Noubase task. [Skills CLI](https://docs.openclaw.ai/cli/skills) · [Skill paths](https://docs.openclaw.ai/tools/skills)

### Shared installer

Run the following from your project to copy the skill to the locations used by the selected clients.

```sh
npx skills add eccodolf/noubase --skill noubase-search \
  -a claude-code codex kimi-code-cli qwen-code openclaw --copy
```

DSH also discovers the resulting skill under the project's `.agents/skills/` directory. [Skills CLI](https://github.com/vercel-labs/skills)

### Clone for manual installs

```sh
git clone https://github.com/eccodolf/noubase.git
cd noubase
```

Copy the entire [`skills/noubase-search/`](skills/noubase-search/) directory to the chosen client skill root. The target path ends in `noubase-search/SKILL.md`.

## Connect Noubase MCP

Register the server as `noubase`. Adjust authentication to match the deployment. Configure secrets locally through the client or its supported environment variables.

| Client | HTTP MCP setup |
| --- | --- |
| Claude Code | `claude mcp add --scope user --transport http noubase "$NOUBASE_MCP_URL"`. Use the deployment's authentication options if required. [Claude Code MCP](https://code.claude.com/docs/en/mcp) |
| Codex | `codex mcp add noubase --url "$NOUBASE_MCP_URL"`. For bearer authentication, add `--bearer-token-env-var NOUBASE_MCP_TOKEN`. [Codex MCP](https://developers.openai.com/codex/mcp) |
| Kimi Code | Use `/mcp-config` to add the HTTP server or configure it in `~/.kimi-code/mcp.json`. Verify with `/mcp`. [Kimi Code MCP](https://github.com/MoonshotAI/kimi-code/blob/main/docs/en/customization/mcp.md) |
| DSH | Add Noubase in MCP settings on the Web or Desktop profile. For profile config, use `@deepseek-ai/dsh-mcp-client` with `serverName: noubase`, `transport: streamable-http`, and the instance URL. [DSH MCP configuration](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/subsystems/mcp.md#configuration) |
| Qwen Code | `qwen mcp add --transport http noubase "$NOUBASE_MCP_URL"`. Configure the required header if applicable and verify with `/mcp`. [Qwen Code MCP](https://qwenlm.github.io/qwen-code-docs/en/users/features/mcp/) |
| OpenClaw | `openclaw mcp add noubase --url "$NOUBASE_MCP_URL" --transport streamable-http` and `openclaw mcp probe noubase`. Use a tool profile that exposes MCP tools. [OpenClaw MCP registry](https://docs.openclaw.ai/cli/mcp/registry) |

Once the skill and server are available, try this read-only request.

> Use noubase-search to list the projects I can search in Noubase.
