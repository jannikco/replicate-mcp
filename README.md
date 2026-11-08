# Replicate MCP server

Inspect the chosen model schema, approve prediction creation, then use the returned prediction ID to read status. Do not infer model success from the request transport result.

A direct-provider MCP server with useful local execution. No PipMCP account is required for local use. Provider API access and any provider charges are your responsibility.

## Quick start

Requires Node.js22+. Clone this repository, then run:

```sh
npm ci --ignore-scripts
npm test
npm start
```

The default command inspects the tools offline. Configure your own credentials in your environment:

- `REPLICATE_API_TOKEN`



Credential help: https://api.replicate.com/openapi.json

## Execute a real read

Replace example record IDs with records you are authorized to access.

```sh
npm start -- --call get_model '{"model_owner":"replicate","model_name":"hello-world"}'
```

## Use with an MCP client

Launch `node /absolute/path/replicate-mcp/server.mjs` using stdio transport; pass credentials through the client environment. Stdout is reserved for JSON-RPC.

## Tools

| Tool | Effect | Purpose |
| --- | --- | --- |
| get_model | Read | Inspect model metadata and latest_version.openapi_schema before constructing input. |
| create_prediction | Explicit approval required | Start a billable asynchronous prediction after explicit approval. Read status/output with get_prediction; model-specific input must match its schema. |
| get_prediction | Read | Read prediction status, error and output metadata; starting/processing are not terminal success. |

## Concrete workflow

Inspect the chosen model schema, approve prediction creation, then use the returned prediction ID to read status. Do not infer model success from the request transport result.

1. get_model
2. create_prediction
3. get_prediction

## Optional writes

Writes are disabled by default. Set CONNECTOR_ALLOW_WRITES=true and supply `_approval: {approved:true,idempotencyKey:"unique-request-id"}` on the specific call. Do not reuse a key for a different request. Uncertain outcomes are never automatically replayed; inspect the provider before another attempt.

## Capability evidence

Local: **Tested — HTTP fixtures and real stdio tools/call**. Managed: **Documented**. Provider authentication and live operations have not been established by these fixture tests. Evidence applies per operation and execution mode; setup discovery does not imply provider execution.

## Managed production

[Import with PipMCP](https://pipmcp.com/github/start?integration=openapi-to-mcp&utm_source=github&utm_campaign=github-replicate-mcp&utm_content=readme) when you want the hosted service. This provider is not currently listed as a managed template. Import selected-openapi.json (REST) or the official GraphQL schema linked in provenance, configure authentication, and review the tools before execution. Local credentials are not uploaded by this code. Review provider permissions in the managed onboarding. [Directory and related projects](https://jannikco.github.io/pipmcp-connectors/integrations/replicate/).

## Provenance and maintenance

- https://api.replicate.com/openapi.json

Maintainer:jannikco. MIT license for this code. See connector.json for selected operations and integration.json for evidence. Runtime dependencies retain their own licenses. Report sensitive issues through https://pipmcp.com/contact; do not post credentials or customer data in public issues.
