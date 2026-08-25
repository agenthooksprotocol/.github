# Agent Hooks Protocol

<p align="center">
  <strong>Portable control points for agent actions</strong>
</p>

<p align="center">
  <a href="https://github.com/agenthooksprotocol/agent-hooks-protocol">Specification</a> ·
  <a href="https://github.com/agenthooksprotocol/typescript-sdk">TypeScript SDK</a> ·
  <a href="https://github.com/orgs/agenthooksprotocol/discussions">Discussions</a>
</p>

The Agent Hooks Protocol (AHP) defines a vendor-neutral boundary between agent harnesses and policy, security, approval, and runtime middleware. It lets a harness expose agent actions to portable backends before side effects occur, without replacing the harness's own permissions, approvals, or sandbox.

AHP is designed to complement protocols such as MCP and ACP. MCP connects agents to tools and context; ACP connects coding agents to editors; AHP standardizes where external controls can observe or restrict agent actions.

## Current status

AHP is an early **Working Draft**. The v0.1 portability floor is intentionally narrow:

- intercept `tool.before` before external side effects;
- return no effect or an explicit `deny`;
- use JSON-RPC over UTF-8 newline-delimited stdio;
- make deadlines and fail-open or fail-closed behavior explicit; and
- validate behavior with shared schemas, fixtures, and conformance tests.

The TypeScript implementation is official but non-normative. The language-neutral specification remains authoritative.

## Project structure

- [`agent-hooks-protocol`](https://github.com/agenthooksprotocol/agent-hooks-protocol) — canonical Working Draft, schemas, proposals, fixtures, governance, and conformance assets.
- [`typescript-sdk`](https://github.com/agenthooksprotocol/typescript-sdk) — non-normative TypeScript SDK, stdio reference implementation, fake backend, and conformance CLI.
- [`.github`](https://github.com/agenthooksprotocol/.github) — organization profile and shared contribution templates.

## Get involved

Start with the [Working Draft](https://github.com/agenthooksprotocol/agent-hooks-protocol/blob/main/spec/working-draft.md), review [AHP-0001](https://github.com/agenthooksprotocol/agent-hooks-protocol/blob/main/proposals/AHP-0001.md), or join the [organization discussions](https://github.com/orgs/agenthooksprotocol/discussions). Material protocol changes follow the public [AHP proposal process](https://github.com/agenthooksprotocol/agent-hooks-protocol/blob/main/governance/AHP-PROCESS.md).

Before contributing, read the guidance in the repository that owns the work. Security reports must follow that repository's security policy and must not be posted publicly.
