# user-feedback

Coletor de **feedback do usuário** da **ShopFlow** (demo do BaIA).

Cada item submetido no widget "Central de ajuda" da ShopFlow vira uma issue
**aqui**, com label `needs-triage`. Um workflow de relay dispara o pipeline
de triagem no [`apresentacao-baia`](https://github.com/baia-demo/apresentacao-baia),
que analisa o código dos repos (`catalog-api`, `orders-api`, `storefront-web`),
**classifica** o que o usuário escreveu (bug / improvement / question / unclear)
e roteia pro repo correto quando aplicável.

## Como funciona

```
ShopFlow widget → Next.js /api/feedback
              → POST /repos/baia-demo/user-feedback/issues (label: needs-triage)
              → Workflow relay (issues.labeled, neste repo)
              → repository_dispatch (event_type: feedback-labeled)
              → apresentacao-baia / triage.yml
              → scripts/triage/triage.py
              → cria issue no repo-alvo (catalog/orders/storefront)
                com label "bug" ou "enhancement" conforme o kind
              → comenta aqui com o veredito + link
              → fecha esta issue (se houve issue técnica criada)
```

## Labels usadas

| Label | Quem aplica | Significado |
|---|---|---|
| `needs-triage` | Form web (na criação) | Aguardando análise |
| `triaged` | triage.py | Já analisado |
| `is-bug` | triage.py | Identificado como bug real |
| `is-improvement` | triage.py | Sugestão de melhoria validada |
| `is-question` | triage.py | Dúvida do usuário (sem código pra ajustar) |
| `needs-info` | triage.py | Não dá pra entender o relato |
| `low-confidence` | triage.py | Análise inconclusiva |
| `repo:<nome>` | triage.py | Repo identificado como dono do código relacionado |
