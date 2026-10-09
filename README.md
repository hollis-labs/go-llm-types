# go-llm-types

## Moved to substrate

This standalone repository is deprecated. New development lives in the
[`github.com/hollis-labs/substrate/llm-core`](https://github.com/hollis-labs/substrate/tree/llm-core/v0.1.0/llm-core)
module, released as **`llm-core/v0.1.0`**.

```sh
go get github.com/hollis-labs/substrate/llm-core@v0.1.0
```

Follow the [package and API migration guide](https://github.com/hollis-labs/substrate/blob/llm-core/v0.1.0/llm-core/llmtypes/MIGRATION.md) when updating imports;
the consolidation can include API changes. Existing standalone tags and history
are preserved. The documentation below describes the standalone releases and
is retained for historical reference. Applications migrate separately; this
redirect does not deploy or update any consumer.

Transport-agnostic data structures for LLM chat/completion workflows. This
module defines the shared request, response, tool, and stream-event types
used across the Hollis Labs LLM toolchain. It deliberately holds **data
types only** — no transport, no provider interfaces, no retry logic.

## Status

Pre-1.0 (`v0.x`). The API may evolve; breaking changes will be called out in
[CHANGELOG.md](./CHANGELOG.md) with a minor-version bump.

## Install

```sh
go get github.com/hollis-labs/go-llm-types
```

## Quickstart

```go
package main

import (
	"fmt"

	llmtypes "github.com/hollis-labs/go-llm-types"
)

func main() {
	req := llmtypes.ChatRequest{
		Model:        "claude-3-5-sonnet",
		SystemPrompt: "You are a helpful assistant.",
		Messages: []llmtypes.ChatMessage{
			{Role: "user", Content: "Hello!"},
		},
		MaxTokens: 1024,
	}

	fmt.Println(req.EffectiveSystemPrompt())
}
```

See [`examples/`](./examples) for runnable demos covering `ChatRequest`,
`StreamEvent`, and slot composition.

## Documentation

Full API reference: [pkg.go.dev/github.com/hollis-labs/go-llm-types](https://pkg.go.dev/github.com/hollis-labs/go-llm-types).

## What's Here

- Request and conversation types: `ChatRequest`, `ChatMessage`, `SlotBlock`
- Tool and content types: `ToolDefinition`, `ToolUseBlock`, `ContentBlock`
- Streaming / event types: `StreamEvent` (with `BlockID` and `Phase`), `EventType`, `ThinkingBlock`; the shared stop-reason vocabulary and `NormalizeStopReason`
- Metadata types: `Usage` (token counts and a per-event `CostUSD` delta), `CompleteResult`, `ProviderCapabilities`
- Helpers: `IsTurnComplete`, `ChatRequest.EffectiveSystemPrompt`

## What's Not Here

`go-llm-types` intentionally contains no provider interfaces, no transport
implementations, and no rate-budget or retry helpers. Those belong in
companion modules and provider-specific adapters.

## Companion Modules

- `github.com/hollis-labs/go-llm-contracts` — provider interfaces and shared
  rate-budget primitives
- `github.com/hollis-labs/go-providers` — PTY / CLI / subprocess provider
  adapters

## License

MIT — see [LICENSE](./LICENSE).
