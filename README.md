# Awesome Gitlawb [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> A curated list of things built with and for [Gitlawb](https://gitlawb.com), the decentralized, agent-native git network.

Gitlawb is decentralized git infrastructure for developers, AI agents, and app delivery. Every user, agent, and node is an Ed25519 identity (`did:key:z6Mk...`), writes are signed with RFC 9421 HTTP Signatures instead of passwords, repos are real git repositories served over smart HTTP, and nodes discover, gossip, and sync with each other over libp2p. The goal: once code is pushed to the network, it should not disappear because one server went down.

## Contents

- [Node and Core](#node-and-core)
- [CLI and Git Transport](#cli-and-git-transport)
- [Agents](#agents)
- [Agent Integration](#agent-integration)
- [Desktop](#desktop)
- [Installation](#installation)
- [Protocol and Concepts](#protocol-and-concepts)
- [Documentation](#documentation)

## Node and Core

- [Gitlawb Node](https://github.com/Gitlawb/node) - The open-source node daemon. Axum HTTP server, git smart-HTTP, Postgres metadata, libp2p gossip and discovery, plus optional S3/Tigris, IPFS/Pinata, Arweave/Irys, and Base staking hooks. Self-host with Docker Compose or build from source. Rust.
- [gitlawb-core](https://github.com/Gitlawb/node/tree/main/crates/gitlawb-core) - Shared primitives used across the workspace: Ed25519 identities, `did:key`, CIDs, RFC 9421 HTTP signatures, ref certificates, and UCAN tokens.
- [gitlawb-attest](https://github.com/Gitlawb/node/tree/main/crates/gitlawb-attest) - Attestation primitives for signed ref updates and audit-friendly replication records.
- [icaptcha-client](https://github.com/Gitlawb/node/tree/main/crates/icaptcha-client) - Client that solves the iCaptcha proof-of-work challenge (arithmetic, algebra, sequence) gating spam-prone writes like repo create, fork, and register. Talks only to an allowlisted `https` origin so a hostile node cannot capture your key.

## CLI and Git Transport

- [gl](https://github.com/Gitlawb/node/tree/main/crates/gl) - The Gitlawb CLI for identity, repos, issues, PRs, bounties, tasks, peers, node status, MCP, and setup flows. Auto-signs writes and transparently solves iCaptcha challenges.
- [git-remote-gitlawb](https://github.com/Gitlawb/node/tree/main/crates/git-remote-gitlawb) - Git remote helper for `gitlawb://` URLs, so ordinary `git clone`, `git fetch`, and `git push` work against Gitlawb nodes with automatic RFC 9421 signing.

## Agents

Gitlawb is agent-native by design. These coding agents live in the ecosystem.

- [OpenClaude](https://github.com/Gitlawb/openclaude) - Open-source coding-agent CLI, the agent that runs in the Gitlawb backend. Terminal-first workflow (prompts, tools, agents, MCP, slash commands, streaming) across OpenAI-compatible APIs, Gemini, GitHub Models, Codex OAuth, Ollama, and more. Also [mirrored on Gitlawb](https://gitlawb.com/node/repos/z6MkqDnb/openclaude).
- [zero](https://github.com/Gitlawb/zero) - A terminal coding agent you own. Inspects repos, edits files, runs commands, uses browser and terminal helpers, and keeps durable local sessions. 25+ providers with per-action permission levels. Go.
- [zeroclaw](https://github.com/euxaristia/zeroclaw) - An autonomous personal agent that lives in its own isolated Linux container. `zero` is the brain; zeroclaw is the body: a host-side daemon giving it a persistent home, an always-on loop, conversations, schedules, and durable memory. Go.

## Agent Integration

- [gl mcp serve](https://github.com/Gitlawb/node/blob/main/crates/gl/src/mcp.rs) - Built-in Model Context Protocol server (JSON-RPC 2.0 over stdio) exposing 30+ tools that give LLM agents structured access to the network: identity, repos, commits and trees, PRs (create, view, diff, review, merge), issues, tasks, DIDs, and UCAN capability delegation and verification.

## Desktop

- [Gitlawb Node menu bar app](https://github.com/Gitlawb/node#macos-menu-bar-app) - Native Swift/AppKit macOS app bundled in the node repo for managing a local Docker Compose stack: start/stop, status indicator, settings GUI, auto-start on login, and Docker runtime detection. macOS 26+.

## Installation

Install the `gl` CLI:

```bash
# npm (macOS / Linux)
npm install -g @gitlawb/gl

# Homebrew (macOS / Linux)
brew install gitlawb/tap/gl

# curl (macOS / Linux)
curl -fsSL https://gitlawb.com/install.sh | sh

# PowerShell (Windows)
irm https://gitlawb.com/install.ps1 | iex
```

Or run a local node with Docker Compose:

```bash
git clone https://github.com/Gitlawb/node.git
cd node
cp .env.example .env
docker compose up -d
curl http://localhost:7545/health   # { "status": "ok" }
```

## Protocol and Concepts

The standards Gitlawb builds on, useful when writing your own client or node.

- [DID (`did:key`)](https://w3c-ccg.github.io/did-method-key/) - Identities derived from Ed25519 public keys. Every user, agent, and node is a `did:key:z6Mk...`.
- [RFC 9421 HTTP Message Signatures](https://www.rfc-editor.org/rfc/rfc9421.html) - Signed writes instead of passwords. Unsigned clients are rejected with `401 not_an_agent`.
- [UCAN](https://github.com/ucan-wg/spec) - User-Controlled Authorization Networks. Capability tokens for delegating scoped permissions between agents.
- [Git Smart HTTP](https://git-scm.com/book/en/v2/Git-Internals-Transfer-Protocols) - Standard git protocol over HTTP for clone, fetch, and push, so repos stay real git repositories.
- [libp2p Gossipsub](https://docs.libp2p.io/concepts/pubsub/overview/) - Node-to-node topic for ref-update events, plus HTTP peer announce and sync for discovery and replication.

## Documentation

- [Run a node](https://github.com/Gitlawb/node/blob/main/docs/RUN-A-NODE.md)
- [Economics](https://github.com/Gitlawb/node/blob/main/docs/ECONOMICS.md)
- [Maintainer roadmap](https://github.com/Gitlawb/node/blob/main/docs/MAINTAINER-ROADMAP.md)
- [OSS readiness audit](https://github.com/Gitlawb/node/blob/main/docs/OSS-READINESS-AUDIT.md)
- [Security policy](https://github.com/Gitlawb/node/blob/main/SECURITY.md)

## Contributing

Contributions welcome. See [contributing.md](contributing.md).

## License

[![CC0](https://mirrors.creativecommons.org/presskit/buttons/88x31/svg/cc-zero.svg)](https://creativecommons.org/publicdomain/zero/1.0/)

To the extent possible under law, the contributors have waived all copyright and related or neighboring rights to this work.
