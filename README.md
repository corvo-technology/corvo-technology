<p align="center">
  <img src="https://assets.greenflow.solutions/brand/corvo-faceted-green.png" alt="Corvo" width="240">
</p>

<p align="center">
  <strong>Self-hosted NFS-e (Brazilian electronic service invoice) API.</strong><br>
  Issue, list, download and reconcile NFS-e from your own infrastructure — with official SDKs in nine languages.
</p>

<p align="center">
  <a href="https://corvo.tech">Website</a> ·
  <a href="https://github.com/corvo-technology/corvo-nfse">API</a> ·
  <a href="#sdks">SDKs</a>
</p>

---

## What we ship

- **Corvo** ([`corvo-technology/corvo-nfse`](https://github.com/corvo-technology/corvo-nfse)) — Self-hosted API (NestJS + Postgres). Run it on your infra, under your own domain.
- **Official SDKs** in nine languages (below) — one consistent contract, idiomatic in each.

## SDKs

Every SDK ships the same 28 operations + 2 health checks, `X-API-Key` auth, cursor pagination, result-style errors that never throw, and HMAC webhook-signature verification.

| Language | Package | Repo |
| --- | --- | --- |
| Node.js / TypeScript | `corvo-nfse-node` (npm) | [`corvo-nfse-node`](https://github.com/corvo-technology/corvo-nfse-node) |
| Go | `corvo-nfse-go` (Go module) | [`corvo-nfse-go`](https://github.com/corvo-technology/corvo-nfse-go) |
| Python | `corvo-nfse` (PyPI) | [`corvo-nfse-python`](https://github.com/corvo-technology/corvo-nfse-python) |
| PHP | `corvo-technology/corvo-nfse` (Packagist) | [`corvo-nfse-php`](https://github.com/corvo-technology/corvo-nfse-php) |
| Ruby | `corvo-nfse` (RubyGems) | [`corvo-nfse-ruby`](https://github.com/corvo-technology/corvo-nfse-ruby) |
| Rust | `corvo-nfse` (crates.io) | [`corvo-nfse-rust`](https://github.com/corvo-technology/corvo-nfse-rust) |
| Java | `technology.corvo:corvo-nfse` (Maven) | [`corvo-nfse-java`](https://github.com/corvo-technology/corvo-nfse-java) |
| Elixir | `corvo_nfse` (Hex) | [`corvo-nfse-elixir`](https://github.com/corvo-technology/corvo-nfse-elixir) |
| .NET | `Corvo.Nfse` (NuGet) | [`corvo-nfse-dotnet`](https://github.com/corvo-technology/corvo-nfse-dotnet) |

## The contract (in-depth)

Nine SDKs, one behavior. This is the analysis each one implements, mirrored from the canonical Node SDK.

**Auth.** Every request carries `X-API-Key` (never `Authorization: Bearer`). Keys are scoped per tenant; the base URL defaults to `https://api.corvo.tech` and is overridable for self-hosted deployments. All routes live under the `v1` prefix.

**Response envelope.** Endpoints return one of three shapes, and each SDK unwraps them idiomatically: `{ "data": ... }` (single resource), `{ "data": { "props": ... } }` (resource behind a `props` wrapper), or no envelope (health checks, raw downloads). List endpoints return `{ "data": [...], "pagination": { "limit", "hasMore", "nextCursor" } }`. A `204 No Content` maps to an empty result.

**Errors never throw.** Network failures, timeouts, non-2xx responses, and malformed JSON all resolve to a typed error carrying `status`, `code`, `message`, `details`, `requestId`, and `retryAfter` — returned alongside (or as) the result, never raised. `code` falls back to `http_<status>` when the body doesn't carry one; transport-level failures use `network_error` / `timeout` / `invalid_json`. The only operation that raises is the `iterate` helper, and only on the first page's failure.

**Pagination.** List endpoints are cursor-paginated. Each SDK exposes both a `list` (single page) and an `iterate`/stream helper that walks `nextCursor` until `hasMore` is false.

**Idempotency.** Invoice create and send-email accept an idempotency key, sent as the `Idempotency-Key` header so retries are safe.

**Webhooks.** Deliveries are signed with HMAC-SHA256 over the raw body. Each SDK exposes a timing-safe `verifySignature(rawBody, header, secret)`.

**Surface.** Exactly 28 operations across companies, certificates, invoices, webhooks (incl. nested deliveries), credits, and billing — plus `health/live` and `health/ready`.

## Get started

```bash
npm install corvo-nfse-node
```

```ts
import { Corvo } from "corvo-nfse-node";

const corvo = new Corvo({ apiKey: process.env.CORVO_API_KEY });
const { data, error } = await corvo.invoices.create(
  {
    companyId: "...",
    customer: { name: "Customer", taxId: "..." },
    service: { code: "010101" },
    amount: 150,
    competenceDate: "2026-10-12",
  },
  { idempotencyKey: "order-1" },
);
```

Every other SDK mirrors this API in its own idiom — see its README for install and a quickstart.

## Contact

- Website: [corvo.tech](https://corvo.tech)
- Issues: open in the relevant repository (API or SDK)
