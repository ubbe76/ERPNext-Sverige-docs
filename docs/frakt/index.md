# ERPNext Sverige Frakt

![ERPNext Sverige Frakt](../assets/logo/erpnext-sverige-frakt.svg#only-light){ width="420" }
![ERPNext Sverige Frakt](../assets/logo/erpnext-sverige-frakt-dark.svg#only-dark){ width="420" }

Appen **ERPNext Sverige Frakt** bokar transporter via [Sendify](https://www.sendify.se), som förmedlar bland
annat DHL, UPS, DSV, PostNord och Schenker. Den bygger på [ERPNext Sverige](../erpnext-sverige/index.md), som
måste vara installerad. Källkoden finns på [GitHub](https://github.com/ubbe76/ERPNext-Sverige-Frakt) under
GPL-3.0.

Hur frakten ställs in och används beskrivs under [Frakt via Sendify](sendify.md).

## Installera

```bash
bench get-app https://github.com/ubbe76/ERPNext-Sverige-Frakt --branch version-16
bench --site <site> install-app erpnext_sverige_frakt
bench --site <site> clear-cache
```

Appen skapar fraktartikeln **Frakt** (konto 3520 för svenska kunder) och lägger fraktmenyn under ERPNext.

## Uppgradera från ERPNext Sverige 0.2

Till och med version 0.2.0 låg frakten i ERPNext Sverige. Inställningar, fraktprodukter och försändelser följer
med till fraktappen, men den måste installeras **innan** siten migreras till ERPNext Sverige 0.3.0:

```bash
bench --site <site> backup
bench get-app https://github.com/ubbe76/ERPNext-Sverige-Frakt --branch version-16
cd apps/erpnext_sverige && git pull && cd ../..        # 0.3.0, utan frakt
bench --site <site> install-app erpnext_sverige_frakt   # före migrate
bench --site <site> migrate
bench build --app erpnext_sverige_frakt
```

!!! danger "Installera före migrate"
    Migreras siten först tar ERPNext bort fraktens doctyper som föräldralösa. Tabellerna finns kvar, men fältet
    med spårningshändelser på försändelsen och fraktmenyn försvinner.
