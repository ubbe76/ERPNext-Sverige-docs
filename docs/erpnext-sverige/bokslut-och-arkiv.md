# Bokslut och arkiv

## Årsarkiv

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

Personalens årsrutiner (nästa års helgdagslista och frånvaroperiod) finns under
[HRMS Sverige → Helgdagar och semester varje år](../hrms-sverige/helglistor.md).
