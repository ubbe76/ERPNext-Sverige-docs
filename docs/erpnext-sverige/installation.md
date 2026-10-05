# Installation och backup

Hela installationen av alla tre apparna, från ny server till kontrollerad site, beskrivs steg för steg under
[Installation](../installation.md). Där finns också uppdatering och felsökning.

## Befintlig site

Har ni redan en site med ERPNext och ett bolag med kontoplanen **BAS 2024 med Nummer** räcker det här. Redis
(`bench start`) måste vara igång:

```bash
bench get-app https://github.com/ubbe76/ERPNext-Sverige --branch version-16
bench --site <site> install-app erpnext_sverige
bench compile-po-to-mo --app erpnext_sverige --locale sv --force
bench --site <site> clear-cache
bench --site <site> execute erpnext_sverige.setup.company.setup_swedish_company --kwargs "{'company': '<bolag>'}"
```

Starta sedan om bench, se [steg 6](../installation.md#6-starta-om-och-tom-cachen). Vad grunduppsättningen
ställer in beskrivs under [Bokföring och moms](bokforing.md#grunduppsattning).

## Backup

ERPNext Sverige har ett script som varje natt tar en krypterad backup och laddar upp den till molnlagring
(till exempel Google Drive, OneDrive eller Backblaze B2). Dagliga kopior sparas i 30 dagar och månadskopior i
8 år. Inställningen beskrivs i appens README under **Backup och arkivering**. Se även [Bokslut och arkiv](bokslut-och-arkiv.md).
