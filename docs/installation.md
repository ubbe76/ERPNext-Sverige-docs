# Installation

Båda apparna installeras med [bench](https://github.com/frappe/bench) på en server där `frappe` och
`erpnext` (branchen `version-16`) redan finns. Redis (kö och cache) måste vara igång under installationen.

## ERPNext Sverige

```bash
bench get-app https://github.com/ubbe76/ERPNext-Sverige --branch version-16
bench --site <site> install-app erpnext_sverige
bench compile-po-to-mo --app erpnext_sverige --locale sv
bench --site <site> clear-cache
```

Sätt sedan upp bolaget med svensk kontoplan, moms och brevhuvud. Det går att köra flera gånger utan att något
dubbleras:

```bash
bench --site <site> execute erpnext_sverige.setup.company.setup_swedish_company --kwargs "{'company': '<bolag>'}"
```

Källkoden finns på GitHub: [ERPNext Sverige](https://github.com/ubbe76/ERPNext-Sverige) och
[HRMS Sverige](https://github.com/ubbe76/HRMS-Sverige), båda under GPL-3.0.

## HRMS Sverige

HRMS Sverige kräver Frappe HRMS och ska installeras **efter** den, så att de svenska översättningarna vinner.

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
[Årsrutiner](arsrutiner.md).

## Backup

ERPNext Sverige har ett script som varje natt tar en krypterad backup och laddar upp den till molnlagring
(till exempel Google Drive, OneDrive eller Backblaze B2). Dagliga kopior sparas i 30 dagar och månadskopior i
8 år. Inställningen beskrivs i appens README under **Backup och arkivering**. Se även [Årsrutiner](arsrutiner.md).
