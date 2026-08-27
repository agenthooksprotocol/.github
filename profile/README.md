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
  <a href="https://github.com/agenthooksprotocol/agent-hooks-protocol">Specification</a> ·
  <a href="https://github.com/agenthooksprotocol/typescript-sdk">TypeScript SDK</a> ·
  <a href="https://github.com/orgs/agenthooksprotocol/discussions">Discussions</a>
</p>

The Agent Hooks Protocol (AHP) is a proposed open protocol that seeks to define a standard set of lifecycle events in the agent loop, and a standard way for external systems to observe and act on them.

A common hooks interface will allow for the development of a robust ecosystem of tooling that works across agents.  AHP is designed to complement existing protocols such as MCP and ACP. MCP connects agents to tools and context; ACP connects coding agents to editors; AHP standardizes where external controls can observe or restrict agent actions.

## Current status

AHP is an early **Working Draft**. The v0.1 portability floor is intentionally narrow:

- intercept `tool.before` before external side effects;
- return no effect or an explicit `deny`;
- use JSON-RPC over UTF-8 newline-delimited stdio;
- make deadlines and fail-open or fail-closed behavior explicit; and
- validate behavior with shared schemas, fixtures, and conformance tests.

The TypeScript implementation is official but non-normative. The language-neutral specification remains authoritative.
