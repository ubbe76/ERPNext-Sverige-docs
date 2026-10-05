# Installation

ERPNext Sverige installeras med [bench](https://github.com/frappe/bench) på en server där `frappe` och
`erpnext` (branchen `version-16`) redan finns. Redis (kö och cache) måste vara igång under installationen.

## Installera

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

Ska ni boka frakt installerar ni därefter [ERPNext Sverige Frakt](../frakt/index.md).

## Uppdatera

```bash
cd apps/erpnext_sverige && git pull && cd ../..
bench --site <site> migrate
bench compile-po-to-mo --app erpnext_sverige --locale sv
bench --site <site> clear-cache
```

!!! warning "Uppgradering till 0.3.0 med frakt"
    Frakten har flyttat till appen [ERPNext Sverige Frakt](../frakt/index.md#uppgradera-fran-erpnext-sverige-02).
    Använder ni frakt måste fraktappen installeras **innan** siten migreras. Den som inte använder frakt behöver
    inte göra något.

## Backup

ERPNext Sverige har ett script som varje natt tar en krypterad backup och laddar upp den till molnlagring
(till exempel Google Drive, OneDrive eller Backblaze B2). Dagliga kopior sparas i 30 dagar och månadskopior i
8 år. Inställningen beskrivs i appens README under **Backup och arkivering**. Se även [Bokslut och arkiv](bokslut-och-arkiv.md).
