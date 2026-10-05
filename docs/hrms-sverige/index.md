# HRMS Sverige

![HRMS Sverige](../assets/logo/hrms-sverige.svg#only-light){ width="320" }
![HRMS Sverige](../assets/logo/hrms-sverige-dark.svg#only-dark){ width="320" }

Appen **HRMS Sverige** anpassar [Frappe HRMS](https://github.com/frappe/hrms) för svenska förhållanden: personal,
frånvaro, närvaro, skift och löneunderlag till lönesystemet. Lön räknas inte i ERPNext. Källkoden finns på
[GitHub](https://github.com/ubbe76/HRMS-Sverige) under GPL-3.0.

| Sida | Innehåll |
|---|---|
| [Personal](personal.md) | Anställda och personnummer, helgdagslistor, semester och frånvaro |
| [Skift och arbetstid](skift.md) | Skifttyper med obetalda raster, scheman per veckodag och arbetstidsförkortning |
| [Löneunderlag till Crona](loneunderlag.md) | Frånvaro, arbetad tid, övertid, mertid och OB som PAXml-fil |
| [Stämpling](stampling.md) | Stämplingssidan för en gemensam surfplatta eller dator |
| [Helgdagar och semester varje år](helglistor.md) | Nästa års helgdagslista, frånvaroperiod och semester |

## Installation

HRMS Sverige kräver Frappe HRMS och ska installeras **efter** den, så att de svenska översättningarna vinner. Hela
ordningen, med installationsguiden och kontrollen efteråt, finns under [Installation](../installation.md).

```bash
bench get-app hrms --branch version-16
bench get-app https://github.com/ubbe76/HRMS-Sverige --branch version-16
bench --site <site> install-app hrms
bench --site <site> install-app hrms_sverige
bench compile-po-to-mo --app hrms_sverige --locale sv --force
bench --site <site> clear-cache
```

Helgdagslistor ("Sverige ÅÅÅÅ") och frånvaroperioder för i år och nästa år skapas automatiskt när
installationsguiden är klar. Fanns företaget redan när appen installerades skapas de direkt. Se annars
[Helgdagar och semester varje år](helglistor.md).
