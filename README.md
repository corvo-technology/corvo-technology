<p align="center">
  <img src="https://assets.greenflow.solutions/brand/corvo-faceted-green.png" alt="Corvo" width="480">
</p>

<p align="center">
  <strong>API de NFS-e (Nota Fiscal de Serviços Eletrônica) self-hosted para o Brasil.</strong><br>
  Emita, liste, baixe e reconcilie NFS-e direto da sua infraestrutura.
</p>

<p align="center">
  <a href="https://corvo.tech">Site</a> ·
  <a href="https://github.com/corvo-technology/corvo-nfse">API</a> ·
  <a href="https://github.com/corvo-technology/corvo-nfse-node">SDK Node.js</a>
</p>

---

## O que entregamos

- **Corvo** ([`corvo-technology/corvo-nfse`](https://github.com/corvo-technology/corvo-nfse)) — API self-hosted (NestJS + Postgres). Você roda na sua infra, sob seu domínio.
- **[`corvo-nfse-node`](https://github.com/corvo-technology/corvo-nfse-node)** — SDK + CLI oficiais para Node.js / TypeScript. Zero dependências em runtime, paginação por cursor, erros que nunca lançam, verificação de assinatura HMAC de webhooks.

## Pra começar

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
    customer: { name: "Cliente", taxId: "..." },
    service: { code: "010101" },
    amount: 150,
    competenceDate: "2026-10-12",
  },
  { idempotencyKey: "order-1" },
);
```

## Contato

- Site: [corvo.tech](https://corvo.tech)
- Issues: abra no repo correspondente (API ou SDK)
