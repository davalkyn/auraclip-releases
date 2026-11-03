# Auraclip MCP: setup instructions for AI agents

You are an AI agent. The user wants you connected to **Auraclip**, a Windows app that turns videos into
short clips. Follow these steps, then report back.

## Facts
- Server name to use: `auraclip`
- Transport: Streamable HTTP
- URL: `http://127.0.0.1:8765/mcp`. If port 8765 is busy, Auraclip uses the next free port; the
  user can see the address in Auraclip → Settings → AI scoring → Connected tools.
- Auth: none by default. If the user set `MCP_TOKEN` in `%LOCALAPPDATA%\Auraclip\clipper.env`,
  send the header `Authorization: Bearer <token>`.
- Same PC only: the server accepts only 127.0.0.1, localhost and [::1].
- Check Auraclip is running: `GET http://127.0.0.1:8765/api/v1/health` returns `{"status":"ok",...}`.

## Add the server (pick the one matching you)
- Claude Code: `claude mcp add --transport http auraclip http://127.0.0.1:8765/mcp --scope user`
- Codex (CLI and app share `~/.codex/config.toml`): `codex mcp add auraclip --url http://127.0.0.1:8765/mcp`,
  or add to config.toml:
  ```toml
  [mcp_servers.auraclip]
  url = "http://127.0.0.1:8765/mcp"
  default_tools_approval_mode = "approve"
  ```
- OpenClaw: `openclaw mcp set auraclip '{"url":"http://127.0.0.1:8765/mcp","transport":"streamable-http"}'`.
  In Windows PowerShell 5.1, escape the inner quotes as `\"`. Or write
  `mcp.servers.auraclip = {"url": "...", "transport": "streamable-http"}` in `~/.openclaw/openclaw.json`.
- Hermes: `hermes mcp add auraclip --url http://127.0.0.1:8765/mcp`, then `hermes mcp test auraclip`.
  Better: the user presses Auraclip → Settings → AI scoring → Connected tools → Hermes → Connect,
  which also installs Auraclip's Hermes skills.
- LM Studio: add `"auraclip": {"url": "http://127.0.0.1:8765/mcp"}` under `mcpServers` in
  `~/.lmstudio/mcp.json`, or press Connect in Auraclip's Connected tools.
- Generic `mcpServers` JSON (Cursor, Windsurf, Cline and others):
  `{"mcpServers": {"auraclip": {"type": "http", "url": "http://127.0.0.1:8765/mcp"}}}`

You may need to restart or reload for new MCP servers to load.

## Verify
Call the tool `health_check`. A good answer has `"state": "ok"`. Tell the user the result and any
`problems` it lists.

## How to use the tools (default "agent" profile)
1. `clipper_setup(url=...)` for a link, or `clipper_setup(path=...)` for a file on this PC. It returns
   `questions`: ask the user exactly those, and never pick an answer yourself. Then call
   `clipper_setup(setup_id=..., answer="<the user's words>")`. "Change settings" returns a second form;
   ask it the same way. `started: true` means the run began.
2. `clipper_status(job_id)` until `done` is true. It waits for the stage to change. When done, send the
   user its `reply` exactly as written.
3. On `failed`, explain `error.code` and offer `clipper_retry(job_id)`. `clipper_cancel(job_id)` stops a run.
4. `clipper_post(job_id, answer)` only with the user's own reply to the "post these clips?" message.
   Posts are public.

Only clip videos the user owns or has the rights to use.
