# Årsrutiner

## Helgdagslista och frånvaroperiod (HRMS Sverige)

Inför varje nytt år skapar du nästa års helgdagslista och frånvaroperiod:

```bash
bench --site <site> execute hrms_sverige.setup.install.setup_all
```

Kommandot skapar bara det som saknas. Befintliga frånvarotyper, helgdagslistor och frånvaroperioder rörs inte,
så egna ändringar som klämdagar finns kvar.

Skapa sedan årets semestertilldelning: öppna frånvaropolicyn **Semester 25 dagar** och koppla den till
de anställda med årets frånvaroperiod.

!!! tip "Standardhelgdagslista på företaget"
    Vissa delar av ERPNext (projekt, arbetsstationer, underhållsscheman) använder företagets
    *standardhelgdagslista*. Byt den till det nya årets lista under **Företag** vid årsskiftet.

## Årsarkiv (ERPNext Sverige)

Bokföringslagen kräver att räkenskapsinformationen sparas i **7 år** efter räkenskapsårets slut. När bokslutet
för året är klart sparar du ett årsarkiv:

```bash
apps/erpnext_sverige/backup/erpnext-backup.sh arkiv 2026
```

Arkivet innehåller en fullständig backup och en SIE 4-fil per bolag. Det krypteras och laddas upp till samma
molnlagring som den dagliga backupen, i mappen `arkiv/2026/`, och raderas aldrig.

!!! warning "Lösenfrasen"
    Backuperna går bara att läsa med lösenfrasen i `~/.config/erpnext-backup/passphrase`. Förvara en kopia
    i en lösenordshanterare.
