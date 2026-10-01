# ERPNext Sverige – manual

Användarmanual för [ERPNext Sverige](https://github.com/ubbe76/ERPNext-Sverige) och
[HRMS Sverige](https://github.com/ubbe76/HRMS-Sverige), byggd med
[Material for MkDocs](https://squidfunk.github.io/mkdocs-material/).

Publicerad på <https://ubbe76.github.io/ERPNext-Sverige-docs/>. Varje push till `main` bygger och publicerar
sidan med GitHub Actions.

## Förhandsgranska lokalt

```bash
python3 -m venv .venv
.venv/bin/pip install -r requirements.txt
.venv/bin/mkdocs serve
```

Öppna sedan <http://127.0.0.1:8000>. Sidorna ligger i `docs/` och menyn i `mkdocs.yml`.

Ta skärmbilder från en testsite, aldrig från en site med riktiga data.
