# Systemdokumentation

Bokföringslagen (5 kap. 11 §) kräver att det finns en **systemdokumentation** som beskriver hur
bokföringssystemet är organiserat och används, och en **behandlingshistorik** som visar hur uppgifterna har
behandlats. Den här sidan beskriver hur ERPNext med ERPNext Sverige uppfyller kraven. Uppgifter som är
specifika för ert företag fyller ni i en egen, intern kopia (se [Företagets uppgifter](#foretagets-uppgifter)).

!!! note "Granska med revisor"
    Sidan beskriver systemets funktioner. Hur de används i ert företag, och om det räcker för er
    verksamhet, bör stämmas av med er revisor eller redovisningskonsult.

## Systemet

| Del | Beskrivning |
|---|---|
| Programvara | [ERPNext](https://erpnext.com) version 16 (Frappe Framework) med apparna ERPNext Sverige och, för personal, HRMS Sverige |
| Databas | MariaDB. All räkenskapsinformation lagras i databasen; bilagor som filer på servern |
| Åtkomst | Via webbläsare med personlig inloggning. Varje användare har roller som styr behörigheten |
| Källkod och ändringar | Apparna utvecklas i git med versionshistorik på GitHub. Varje ändring görs i en egen pull request |

## Bokföringens organisation

**Kontoplan.** BAS 2024 (mallen *BAS 2024 med Nummer*). Konton som saknas i ERPNext:s mall, till exempel 3308,
läggs till av grunduppsättningen.

**Räkenskapsår.** Ett *Fiscal Year* per räkenskapsår i ERPNext.

**Verifikationer och nummerserier.** Varje affärshändelse registreras som ett dokument i ERPNext, och dokumentet
är verifikationen:

| Affärshändelse | Dokument | Nummerserie |
|---|---|---|
| Kundfaktura, kreditfaktura | Försäljningsfaktura | **Fakturanummer** ÅÅÅÅ-NNNN utan luckor, sätts vid bokföring (internt id ACC-SINV-…) |
| Leverantörsfaktura | Inköpsfaktura | ACC-PINV-ÅÅÅÅ-… |
| Betalningar | Betalningspost | ACC-PAY-ÅÅÅÅ-… |
| Övriga verifikationer | Journalpost | ACC-JV-ÅÅÅÅ-… |
| Lagerhändelser | Lagerpost, följesedel, inköpskvitto | MAT-… |

ERPNext ger interna id redan till utkast. Ett raderat utkast lämnar därför en lucka i det interna id:t men
aldrig i fakturanumret. Utkast är inte bokförda och ingår inte i bokföringen. I SIE-exporten numreras
verifikationerna utan luckor i serierna A (journalposter), B (kundfakturor), C (leverantörsfakturor),
D (betalningar), E (lager) och F (övrigt).

**Verifikationens innehåll.** Varje dokument har datum för affärshändelsen (bokföringsdatum), datum och tid för
registreringen, vem som registrerade, belopp, motpart och vad den avser. Underlag (kvitton, inskannade fakturor)
bifogas dokumentet.

## Löpande bokföring

- **Grundbok** (registreringsordning): rapporten [Grundbok](bokforing/index.md) under *Svensk bokföring*.
- **Huvudbok** (systematisk ordning): ERPNext:s rapporter *Bokföringsregister* (huvudboken, per konto) och
  *Provsaldo*.
- **Kontoval och moms** sker automatiskt utifrån kundens eller leverantörens momskategori och artikelns
  momssats, se [Bokföring och moms](bokforing/index.md).

## Rättelser och skydd mot ändringar

- **Immutable Ledger** är påslaget. En bokförd verifikation kan inte ändras eller raderas. Den rättas genom
  makulering, som bokför en motverifikation; originalet finns kvar.
- **Bilagor** på bokförda och makulerade verifikationer kan inte tas bort eller flyttas.
- **Låsta perioder**: när momsdeklarationen är inlämnad låses perioden med *Lås perioden* i rapporten
  Momsdeklaration. Därefter går det inte att bokföra, ändra eller makulera något med datum i perioden.

## Behandlingshistorik

| Vad | Var |
|---|---|
| Vem som registrerade en verifikation och när | Fälten *Skapad av* och *Skapad* på dokumentet, samt rapporten Grundbok |
| Ändringar av dokument och inställningar | Dokumentets *Aktivitet* (ändringshistorik) |
| Makuleringar | Motverifikationer i Grundbok och Bokföringsregister, och dokumentets status |
| Inloggningar | ERPNext:s *Aktivitetslogg* |
| Ändringar i programvaran | Versionshistoriken på GitHub |

## Arkivering och backup

Räkenskapsinformationen sparas i **7 år** efter räkenskapsårets slut (bokföringslagen 7 kap.).

- Varje natt tas en krypterad backup av databas och bilagor som laddas upp till molnlagring utanför servern.
  Dagliga kopior sparas i 30 dagar och en kopia per månad i 8 år.
- Efter bokslutet sparas ett **årsarkiv** med backup och SIE 4-fil, som aldrig raderas
  (se [Årsrutiner](arsrutiner.md)).
- Backupen krypteras med en lösenfras som förvaras i en lösenordshanterare.
- Återställning beskrivs i ERPNext Sveriges README under *Backup och arkivering*.

## Kända begränsningar

- ERPNext:s kassa är **inte** ett certifierat kassaregister. Kontant försäljning till kunder på plats kräver
  ett certifierat kassaregister med kontrollenhet.
- Omvänd skattskyldighet för inköp från EU med 12 eller 6 % moms redovisas än så länge med 25 %.
- Den som har direkt åtkomst till servern och databasen kan ändra uppgifter utanför ERPNext. Åtkomsten till
  servern ska därför vara begränsad, och backupen i molnet utgör ett oberoende skydd.

## Företagets uppgifter

Fyll i en intern kopia av följande och uppdatera den när något ändras:

| Uppgift | Ert svar |
|---|---|
| Företag, organisationsnummer | |
| Räkenskapsår | |
| Ansvarig för bokföringen | |
| Användare och roller (vem är System Manager, Accounts Manager, Accounts User) | |
| Var servern står och vem som har åtkomst till den | |
| Molnlagring för backup och var lösenfrasen förvaras | |
| Rutin för bokföring av kontanta betalningar (senast nästa arbetsdag) och övriga affärshändelser | |
| Rutin för momsdeklaration och låsning av perioden | |
| Revisor eller redovisningskonsult | |
