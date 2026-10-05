# Installation

Installera apparna i ordningen nedan, och gör det **innan** installationsguiden i webbläsaren körs. Då blir
guiden svensk från början, och HRMS Sverige skapar helgdagslistorna automatiskt när bolaget läggs upp.

| Steg | Vad | Behövs |
|---|---|---|
| 1 | [Server och bench](#1-server-och-bench) | Alltid |
| 2 | [Site med ERPNext](#2-site-med-erpnext) | Alltid |
| 3 | [ERPNext Sverige](#3-erpnext-sverige) | Alltid |
| 4 | [ERPNext Sverige Frakt](#4-erpnext-sverige-frakt) | Om ni bokar frakt via Sendify |
| 5 | [HRMS och HRMS Sverige](#5-hrms-och-hrms-sverige) | Om ni hanterar personal i ERPNext |
| 6 | [Starta om och töm cachen](#6-starta-om-och-tom-cachen) | Alltid |
| 7 | [Installationsguiden](#7-installationsguiden) | Ny site |
| 8 | [Svensk grunduppsättning](#8-svensk-grunduppsattning) | Alltid |
| 9 | [Kontrollera](#9-kontrollera) | Alltid |

Har ni redan en site med ERPNext och ett bolag går ni direkt till steg 3 och hoppar över steg 7.

Alla `bench`-kommandon körs i bench-katalogen (till exempel `~/frappe-bench`), inte inne i en app. Byt
`<site>` mot sitens namn, till exempel `erp.foretaget.se`.

## 1. Server och bench

Apparna är testade på Linux med:

| Program | Version |
|---|---|
| frappe och erpnext | 16 (branchen `version-16`) |
| Python | 3.14 |
| Node.js | 24 eller senare |
| MariaDB | 11.8 |
| Redis | Det som följer med Linuxdistributionen |

Följ Frappes [installationsinstruktioner](https://github.com/frappe/bench) för att installera systempaketen och
`bench`. Skapa sedan en bench med frappe version 16:

```bash
bench init frappe-bench --frappe-branch version-16 --python python3.14
cd frappe-bench
```

## 2. Site med ERPNext

Starta bench i en **egen terminal** och låt den vara igång under hela installationen. Den startar Redis (kö och
cache), som installationerna behöver:

```bash
bench start
```

Skapa siten och installera ERPNext i den första terminalen:

```bash
bench new-site <site>
bench get-app erpnext --branch version-16
bench --site <site> install-app erpnext
```

`new-site` frågar efter ett lösenord för användaren Administrator. Det används för den första inloggningen.

!!! warning "Redis måste vara igång"
    Avbryts en installation för att Redis inte svarar körs aldrig appens sista steg, och fält och
    inställningar saknas efteråt. Kontrollera att `bench start` är igång innan varje `install-app`.

## 3. ERPNext Sverige

```bash
bench get-app https://github.com/ubbe76/ERPNext-Sverige --branch version-16
bench --site <site> install-app erpnext_sverige
bench compile-po-to-mo --app erpnext_sverige --locale sv --force
```

- `get-app` hämtar koden till `apps/erpnext_sverige` och installerar Pythonpaketet.
- `install-app` skapar appens egna fält på bland annat kunder, leverantörer och fakturor.
- `compile-po-to-mo` bygger de svenska översättningarna. Utan det steget visas ERPNext:s egna översättningar.

## 4. ERPNext Sverige Frakt

Hoppa över steget om ni inte bokar frakt. Fraktappen kräver ERPNext Sverige och installeras efter den.

```bash
bench get-app https://github.com/ubbe76/ERPNext-Sverige-Frakt --branch version-16
bench --site <site> install-app erpnext_sverige_frakt
```

Appen skapar fraktartikeln **Frakt** och lägger fraktmenyn under ERPNext. API-nyckel och avsändare ställer ni in
efter installationen, se [Frakt via Sendify](frakt/sendify.md).

## 5. HRMS och HRMS Sverige

Hoppa över steget om ni inte hanterar personal i ERPNext. HRMS Sverige installeras **efter** Frappe HRMS, så att
de svenska översättningarna vinner.

```bash
bench get-app hrms --branch version-16
bench get-app https://github.com/ubbe76/HRMS-Sverige --branch version-16
bench --site <site> install-app hrms
bench --site <site> install-app hrms_sverige
bench compile-po-to-mo --app hrms_sverige --locale sv --force
```

## 6. Starta om och töm cachen

Processerna som redan körde känner inte till de nya apparna. Stoppa `bench start` med ++ctrl+c++ i dess
terminal och starta den igen. På en produktionsserver med supervisor kör ni i stället `bench restart`.

Töm sedan sitens cache:

```bash
bench --site <site> clear-cache
```

## 7. Installationsguiden

Öppna siten i webbläsaren och logga in som **Administrator** med lösenordet från steg 2. Installationsguiden
startar av sig själv på en ny site. Fyll i:

| Fält | Värde |
|---|---|
| Språk | **Svenska** |
| Land | **Sverige** (sätter tidszonen Europe/Stockholm och valutan SEK) |
| Bolagsnamn | Bolagets registrerade namn, till exempel *Exempel AB* |
| Bolagsförkortning | Föreslås av guiden och syns i slutet av kontonamnen. Den går inte att ändra senare. |
| Kontoplan | **BAS 2024 med Nummer** |
| Räkenskapsårets startdatum | 1 januari, eller första dagen i ert brutna räkenskapsår |
| Skapa demodata för att utforska | Nej, på en site med riktiga data |

Välj **BAS 2024 med Nummer**. Den svenska grunduppsättningen i nästa steg bygger på BAS-kontonumren, och
kontoplanen går inte att byta när det finns bokföring.

Med HRMS Sverige installerat skapas helglistor ("Sverige ÅÅÅÅ") och frånvaroperioder för i år och nästa år
när guiden är klar.

## 8. Svensk grunduppsättning

Kör uppsättningen för bolaget. Ange bolagsnamnet exakt som i guiden:

```bash
bench --site <site> execute erpnext_sverige.setup.company.setup_swedish_company --kwargs "{'company': 'Exempel AB'}"
```

Den lägger in svenskt talformat, standardkonton enligt BAS, momsmallar och momsregler, låst huvudbok och
brevhuvudet **Brevhuvud Sverige**. Vad som ställs in beskrivs under
[Bokföring och moms](erpnext-sverige/bokforing.md#grunduppsattning). Kommandot går att köra igen utan att något
dubbleras, till exempel för ett andra bolag.

## 9. Kontrollera

Lista apparna på siten:

```bash
bench --site <site> list-apps
```

`erpnext_sverige` och de övriga apparna ni installerat ska finnas med. Kontrollera sedan i webbläsaren:

1. Ladda om sidan utan cache: ++ctrl+shift+r++ i Windows och Linux, ++cmd+shift+r++ på Mac.
2. Språket under dina användarinställningar ska vara **Svenska**. Varje användare väljer sitt eget språk.
3. Menyerna ska säga **Kundfaktura** och **Verifikation**, inte "Försäljningsfaktura" och "Journalpost".
4. Kontoplanen ska ha BAS-nummer, till exempel **1510 Kundfordringar**.
5. Med HRMS Sverige: helglistan **Sverige ÅÅÅÅ** med årets årtal ska finnas under **Helglista**.

## Felsökning

| Problem | Lösning |
|---|---|
| Texterna är på engelska eller ERPNext:s egen svenska ("Försäljningsfaktura") | Välj språket Svenska för användaren, kör `compile-po-to-mo` från steg 3 (och 5) och `clear-cache`, och ladda om sidan utan cache. |
| `install-app` avbröts halvvägs | Starta `bench start` och kör `bench --site <site> migrate`. Då skapas fälten från ERPNext Sverige och Frakt igen. |
| Helglistorna saknas | Bolaget fanns inte när HRMS Sverige installerades. Kör `bench --site <site> execute hrms_sverige.setup.install.setup_all`, se [Helgdagar och semester varje år](hrms-sverige/helglistor.md). |
| `setup_swedish_company` svarar att bolaget inte finns | Bolagsnamnet stavas inte exakt som i ERPNext. Kopiera namnet från listan **Bolag**. |
| En ny app syns inte, eller schemaläggaren stannar | Processerna startades före installationen. Starta om enligt steg 6. |

## Uppdatera

Ta en backup och uppdatera en app i taget. Här ERPNext Sverige:

```bash
bench --site <site> backup
git -C apps/erpnext_sverige pull
bench --site <site> migrate
bench compile-po-to-mo --app erpnext_sverige --locale sv --force
bench --site <site> clear-cache
```

Gör likadant för `erpnext_sverige_frakt` och `hrms_sverige` (med `--app hrms_sverige` i `compile-po-to-mo`).
Starta sedan om enligt steg 6. Kör `migrate` på varje site om benchen har flera.

!!! warning "Uppgradering av ERPNext Sverige till 0.3.0 med frakt"
    Frakten har flyttat till appen ERPNext Sverige Frakt. Använder ni frakt måste fraktappen installeras
    **innan** siten migreras, annars tar `migrate` bort fraktens doctyper. Följ
    [Uppgradera från ERPNext Sverige 0.2](frakt/index.md#uppgradera-fran-erpnext-sverige-02). Den som inte använder
    frakt behöver inte göra något.
