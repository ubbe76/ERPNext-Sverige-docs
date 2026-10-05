# ERPNext Sverige – manual

Användarmanual för [ERPNext Sverige](https://github.com/ubbe76/ERPNext-Sverige),
[ERPNext Sverige Frakt](https://github.com/ubbe76/ERPNext-Sverige-Frakt) och
[HRMS Sverige](https://github.com/ubbe76/HRMS-Sverige), med ett kapitel per app, byggd med
[Material for MkDocs](https://squidfunk.github.io/mkdocs-material/).

Publicerad på <https://ubbe76.github.io/ERPNext-Sverige-docs/>. Varje push till `main` bygger och publicerar
sidan med GitHub Actions.

## Förhandsgranska lokalt

```bash
python3 -m venv .venv
.venv/bin/pip install -r requirements.txt
.venv/bin/mkdocs serve
```

Öppna sedan <http://127.0.0.1:8000>. Sidorna ligger i `docs/`, en mapp per app (`erpnext-sverige/`, `frakt/`,
`hrms-sverige/`), och menyn i `mkdocs.yml`. Flyttas en sida läggs den gamla adressen in under `redirects` i
`mkdocs.yml`, så att publicerade länkar fortsätter att fungera.

Ta skärmbilder från en testsite, aldrig från en site med riktiga data.
