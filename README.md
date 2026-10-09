# Dashboard estático — Suporte RAG

Dashboard interativo (HTML + Chart.js) alimentado por JSON exportado do Postgres.  
Pensado para publicação via **GitHub Pages** ou qualquer hospedagem estática.

## Estrutura

```
export/dashboard/
  index.html              # UI + gráficos
  README.md               # este arquivo
  metrics/
    latest.json           # gerado por scripts/export_metrics.py
```

## Gerar métricas

Na raiz do repositório (com `.env` e Postgres acessível):

```bash
python scripts/export_metrics.py
python scripts/export_metrics.py --days 30
```

Saída padrão:

`export/dashboard/metrics/latest.json`

## Visualizar localmente

Não abra o HTML via `file://` (fetch do JSON pode falhar). Use um servidor estático:

```bash
cd export/dashboard
python -m http.server 8080
```

Abra: http://localhost:8080/

## GitHub Pages

Opções comuns:

1. **Publicar só esta pasta** com Action que faz deploy de `export/dashboard` na branch `gh-pages`, **ou**
2. Copiar/sincronizar o conteúdo de `export/dashboard` para a pasta configurada no Pages (ex.: `/docs`).

O `index.html` carrega o JSON por caminho relativo:

```text
./metrics/latest.json
```

## Privacidade

O export contém **apenas agregados** (contagens, médias, taxas).  
Não inclui texto de mensagens, `query_preview` nem dados pessoais.

## Atualização

1. Rodar `export_metrics.py`
2. Commit de `export/dashboard/metrics/latest.json` (e HTML se mudou)
3. Push — Pages atualiza conforme o provedor
