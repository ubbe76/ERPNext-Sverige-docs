# Kom igång

Den här manualen beskriver tre tillägg som anpassar [ERPNext](https://erpnext.com) version 16 för svenska
företag. Varje app har ett eget kapitel.

| App | Vad den gör | Kräver |
|---|---|---|
| [**ERPNext Sverige**](erpnext-sverige/index.md) | Bokföring och moms enligt BAS-kontoplanen, svenska fakturamallar, bankfiler, momsdeklaration, SIE-export, systemdokumentation och rättade svenska översättningar. | ERPNext |
| [**ERPNext Sverige Frakt**](frakt/index.md) | Transportbokning via Sendify: priser, bokning, fraktsedel, spårning och frakt på fakturan. | ERPNext Sverige |
| [**HRMS Sverige**](hrms-sverige/index.md) | Personal: personnummer, svenska helgdagar, semester och frånvaro, skift och raster, stämpling och löneunderlag till Crona. Ingen lönehantering. | Frappe HRMS |

HRMS Sverige är fristående från de andra två. Frakten bygger på ERPNext Sverige och installeras efter den.

!!! note "Manualen är under uppbyggnad"
    Avsnitt som ännu inte är skrivna är markerade. Hittar du fel eller saknar något kan du föreslå en
    ändring via pennan uppe till höger på varje sida.

## Innan du börjar

- Välj språket **Svenska (sv)** under dina användarinställningar. Annars syns inte de svenska texterna.
- Ladda om sidan utan webbläsarens cache efter att apparna har installerats eller uppdaterats. Annars
  kan webbläsaren visa gamla menyer och texter.
    - **Windows:** ++ctrl+f5++ eller ++ctrl+shift+r++ (Chrome, Edge och Firefox).
    - **Mac:** ++cmd+shift+r++ i Chrome, Edge och Firefox, och ++cmd+option+r++ i Safari.

Apparna är senast testade med frappe 16.36.1, erpnext 16.37.0 och hrms 16.20.1 (2026-10-04).

## Ordlista

ERPNext Sverige använder svensk bokförings- och affärsterminologi i stället för ERPNext:s egna översättningar.
De viktigaste begreppen:

| Engelska | ERPNext | ERPNext Sverige |
|---|---|---|
| Cost Center | Resultat Enheter | Resultatenhet |
| Party | Parti (batch) | Part |
| Journal Entry | Journalpost | Verifikation |
| General Ledger | Bokföringsregister | Huvudbok |
| Balance Sheet / Profit and Loss | Balansrapport / Resultatrapport | Balansräkning / Resultaträkning |
| Accounts Receivable / Payable | Fordringar / Skulder | Kundreskontra / Leverantörsreskontra |
| Sales / Purchase Invoice | Försäljningsfaktura / Inköpsfaktura | Kundfaktura / Leverantörsfaktura |
| Tax Withholding | Momsavdrag | Källskatt |
| Purchase Receipt | Inköpsföljesedel | Inleverans |
| Work Order / Job Card | Arbetsorder / Jobbkort | Tillverkningsorder / Operationskort |
| Operation / Workstation | Åtgärd / Arbetsplats | Operation / Arbetsstation |
| Stores (standardlager) | Butiker | Lager |
