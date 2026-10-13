<p align="center">
  <img src="https://assets.greenflow.solutions/brand/corvo-faceted-green.png" alt="Corvo" width="480">
</p>

<p align="center">
  <strong>Self-hosted NFS-e (Brazilian electronic service invoice) API.</strong><br>
  Issue, list, download and reconcile NFS-e from your own infrastructure.
</p>

<p align="center">
  <a href="https://corvo.tech">Website</a> ·
  <a href="https://github.com/corvo-technology/corvo-nfse">API</a> ·
  <a href="https://github.com/corvo-technology/corvo-nfse-node">Node.js SDK</a>
</p>

---

## What we ship

- **Corvo** ([`corvo-technology/corvo-nfse`](https://github.com/corvo-technology/corvo-nfse)) — Self-hosted API (NestJS + Postgres). Run it on your infra, under your own domain.
- **[`corvo-nfse-node`](https://github.com/corvo-technology/corvo-nfse-node)** — Official Node.js / TypeScript SDK and CLI. Zero runtime dependencies, cursor pagination, errors that never throw, HMAC webhook signature verification.

## Get started

```bash
# SDK
yarn add corvo-nfse-node

# CLI
yarn global add corvo-nfse-node
corvo config set apiKey corvo_live_xxx --base-url https://corvo.example.com
corvo companies list
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

## Contact

- Website: [corvo.tech](https://corvo.tech)
- Issues: open in the relevant repository (API or SDK)
