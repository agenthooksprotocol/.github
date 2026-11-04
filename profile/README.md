# Agent Hooks Protocol

<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="assets/mark-dark.svg">
    <source media="(prefers-color-scheme: light)" srcset="assets/mark-light.svg">
    <img src="assets/mark-light.svg" alt="Agent Hooks Protocol" width="128">
  </picture>
</p>

<p align="center">
  <strong>Portable control points for agent actions</strong>
</p>

<p align="center">
  <a href="https://agenthooksprotocol.com">Website</a> ·
  <a href="https://agenthooksprotocol.com/docs/">Docs</a> ·
  <a href="https://agenthooksprotocol.com/docs/spec/draft/">Specification</a> ·
  <a href="https://agenthooksprotocol.com/roadmap/">Roadmap</a> ·
  <a href="https://github.com/orgs/agenthooksprotocol/discussions">Discussions</a>
</p>

Every coding agent has its own hooks: different events, different payloads, different ways to say no. A policy engine, DLP scanner or audit pipeline has to be rebuilt for each one.

The Agent Hooks Protocol (AHP) is an open protocol that gives them one interface. An agent harness reports what is happening in the agent loop, and external backends can observe it or change the outcome. The harness stays in charge: it runs the tools, enforces the result, and keeps its own permissions and sandbox.

AHP complements the protocols around it. MCP connects agents to tools and context, ACP connects coding agents to editors, and AHP standardizes where external systems can observe and control what agents do.

## Get started

| | Install | Repository |
| --- | --- | --- |
| **TypeScript** | `npm install agenthooksprotocol` | [typescript-sdk](https://github.com/agenthooksprotocol/typescript-sdk) |
| **Python** | `pip install 'agenthooksprotocol[http]'` | [python-sdk](https://github.com/agenthooksprotocol/python-sdk) |
| **Go** | `go get github.com/agenthooksprotocol/go-sdk@latest` | [go-sdk](https://github.com/agenthooksprotocol/go-sdk) |
| **Rust** | `cargo add agenthooksprotocol` | [rust-sdk](https://github.com/agenthooksprotocol/rust-sdk) |

The SDKs cover both sides of the protocol: harnesses sending events and backends answering them.

The language-neutral [specification, schemas, fixtures and conformance profiles](https://github.com/agenthooksprotocol/agent-hooks-protocol) define the protocol. SDKs and adapters implement it and are not themselves normative.

## Third-party harnesses

Adapters put AHP inside harnesses that do not speak it natively, so their events reach any AHP backend with no code. Each one advertises only what it can enforce and lists the rest as gaps in its manifest.

<table>
  <tr>
    <td width="33%" align="center" valign="top">
      <br>
      <a href="https://github.com/agenthooksprotocol/adapter-claude">
        <picture>
          <source media="(prefers-color-scheme: dark)" srcset="assets/harnesses/claude-code-name-dark.svg">
          <source media="(prefers-color-scheme: light)" srcset="assets/harnesses/claude-code-name-light.svg">
          <img src="assets/harnesses/claude-code-name-light.svg" alt="Claude Code" height="28">
        </picture>
      </a>
      <br><br>
      <strong>28 of 32 events</strong><br>
      A Claude Code mod<br><br>
      <a href="https://github.com/agenthooksprotocol/adapter-claude">adapter-claude</a>
      <br><br>
    </td>
    <td width="33%" align="center" valign="top">
      <br>
      <a href="https://github.com/agenthooksprotocol/adapter-opencode">
        <picture>
          <source media="(prefers-color-scheme: dark)" srcset="assets/harnesses/opencode-wordmark-dark.svg">
          <source media="(prefers-color-scheme: light)" srcset="assets/harnesses/opencode-wordmark-light.svg">
          <img src="assets/harnesses/opencode-wordmark-light.svg" alt="OpenCode" height="28">
        </picture>
      </a>
      <br><br>
      <strong>23 of 32 events</strong><br>
      24 on OpenCode 1<br><br>
      <a href="https://github.com/agenthooksprotocol/adapter-opencode">adapter-opencode</a>
      <br><br>
    </td>
    <td width="33%" align="center" valign="top">
      <br>
      <strong>Your harness</strong>
      <br><br>
      <strong>Want it next?</strong><br>
      Or building one?<br><br>
      <a href="https://github.com/orgs/agenthooksprotocol/discussions">Start a discussion</a>
      <br><br>
    </td>
  </tr>
</table>

| Harness | Install |
| --- | --- |
| **Claude Code** | `/plugin install agent-hooks-protocol --marketplace agenthooksprotocol/adapter-claude` |
| **OpenCode 2** | `"plugins": ["@agenthooksprotocol/adapter-opencode"]` in `opencode.json` |
| **OpenCode 1** | `"plugin": ["@agenthooksprotocol/adapter-opencode"]` in `opencode.json` |

<sub>Names and logos belong to their owners. These adapters are built by the Agent Hooks Protocol project, and their presence here does not mean those owners endorse or sponsor it.</sub>

## How it works

A harness registers its backends in `hooks.json`, choosing what each one receives and what happens if it fails:

```json
{
  "protocolVersion": "draft",
  "hooks": [{
    "id": "com.example.policy",
    "transport": { "type": "http", "url": "https://hooks.example/ahp" },
    "subscriptions": [{
      "mode": "intercept",
      "events": ["tool.before"],
      "timeoutMs": 2000,
      "failurePolicy": "fail-closed",
      "content": { "default": "metadata" }
    }]
  }]
}
```

Before a tool runs, the backend gets a `hooks/intercept` request and answers with the effects it wants:

```json
{ "protocolVersion": "draft", "effects": [{ "type": "deny", "reason": "Production writes need approval" }] }
```

Leave `subscriptions` out and the backend supplies its own defaults through server-driven configuration.

## What the protocol covers

**32 events**, any of which a harness may support:

| Family | Events |
| --- | --- |
| Session | `session.start` `session.end` |
| Turn | `turn.start` `turn.progress` `turn.finish.before` `turn.end` |
| Model | `model.request.before` `model.response.after` `model.error` `model.switch.before` `model.switch.after` |
| Tool | `tool.before` `tool.after` `tool.progress` `tool.batch.after` `tool.permission.request` `tool.permission.resolved` |
| Context | `context.compact.before` `context.compact.after` |
| User | `user.message.inbound` `user.message.outbound` `user.elicitation.request` `user.elicitation.result` `user.attention` |
| Environment | `config.change.before` `config.change.after` `workspace.change.before` `workspace.change.after` `task.change.before` `task.change.after` `file.changed` |
| Delivery | `hook.failure` |

**Effects** a backend can return, where the event allows them: `deny`, `allow`, `ask`, `modify`, `inject`, `return`, `flow` (stop or continue), and `message`.

- **Honest capabilities.** A harness advertises only what it can actually enforce, per event and per request, and declares its gaps in a manifest.
- **Explicit failure.** Every intercept subscription sets a deadline and `fail-open` or `fail-closed`. An explicit deny is never confused with an outage, and no effect does not mean allow.
- **Durable identity.** Events, sessions, turns, calls and content items carry stable IDs, so retries and correlation work across backends.
- **Content under control.** Each subscription selects `body`, `metadata` or `omit` per content category. Images and files upload separately, by reference.
- **Transports and auth.** JSON-RPC over stdio or HTTP, with bearer tokens or OAuth.
- **Verifiable.** Stable requirement IDs, JSON Schemas, golden fixtures and conformance profiles, with the SDKs tested against each other in an interop matrix.

## Status

AHP is a **Working Draft**. Everything uses the wire version `draft`, and breaking changes are still landing, so pin your SDK version. Frozen, dated snapshots will mark stable releases. See the [roadmap](https://agenthooksprotocol.com/roadmap/).

## Get involved

Ask questions and share ideas in [Discussions](https://github.com/orgs/agenthooksprotocol/discussions). Protocol changes go through the [proposal process](https://github.com/agenthooksprotocol/agent-hooks-protocol/blob/main/governance/AHP-PROCESS.md), and [CONTRIBUTING](https://github.com/agenthooksprotocol/.github/blob/main/CONTRIBUTING.md) covers the rest.
