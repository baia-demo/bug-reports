# bug-reports

Repo coletor de relatos de bugs da **ShopFlow** (demo do BaIA).

Cada item no form web "Reportar bug" da ShopFlow vira uma issue **aqui**,
com label `needs-triage`. Um workflow de relay dispara o pipeline de triagem
no [`apresentacao-baia`](https://github.com/baia-demo/apresentacao-baia), que
analisa o código dos repos (`catalog-api`, `orders-api`, `storefront-web`),
classifica como bug ou não e roteia pro repo correto.

## Como funciona

```
ShopFlow form → API route Next.js
              → POST /repos/baia-demo/bug-reports/issues (label: needs-triage)
              → Workflow relay (issues.labeled)
              → repository_dispatch → apresentacao-baia
              → triage.py
              → cria issue no repo-alvo (catalog/orders/storefront)
              → comenta aqui com link
              → fecha esta issue
```

## Labels usadas

| Label | Quem aplica | Significado |
|---|---|---|
| `needs-triage` | Form web (na criação) | Aguardando análise |
| `triaged` | triage.py | Já analisado |
| `is-bug` | triage.py | Identificado como bug real |
| `not-a-bug` | triage.py | Uso incorreto, comportamento esperado, etc |
| `low-confidence` | triage.py | Análise inconclusiva (precisa humano) |
| `repo:<nome>` | triage.py | Repo identificado como dono do bug |
