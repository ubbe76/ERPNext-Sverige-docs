# Löneunderlag till Crona Lön

Frånvaron i ERPNext kan föras över till Crona Lön som en PAXml-fil, så att den inte behöver skrivas in två gånger.
Filen innehåller den godkända frånvaron för en månad. Arbetad tid och tillägg ingår inte än.

## Förberedelser i ERPNext

- **Anställningsnummer:** fyll i fältet *Anställningsnummer* på varje anställd. Det måste vara samma nummer som i
  Crona, annars hittar Crona inte den anställde.
- **Tidkoder:** varje frånvarotyp har en *PAXml-tidkod*. De svenska frånvarotyperna har koderna från början:

| Frånvarotyp | PAXml-tidkod |
|---|---|
| Semester | SEM |
| Sjukfrånvaro | SJK |
| VAB | VAB |
| Föräldraledighet | FPE |
| Tjänstledighet | TJL |
| Kompledighet | KOM |

## Förberedelser i Crona Lön

1. Under **Import > Försystem**, skapa ett nytt försystem med importformat **PAXml** och filändelse **XML**.
2. Under **Register > Löneartsstyrning**, koppla varje tidkod ovan till rätt löneart. Kontrollera om
   kopplingen gäller månadsavlönade eller timavlönade.
3. Alla anställda behöver ett schema i Crona. Frånvaron skickas som procent av schemat, och Crona räknar
   timmarna och karensavdraget.

## Timavlönade

Sätt **Löneform** till *Timlön* på anställda med timlön. För dem skickas:

- **arbetad tid** som tidkod **ARB**, en rad per dag med de timmar som närvaron visar. Närvaron räknas fram av
  HRMS från in- och utstämplingarna mot skiftet;
- **frånvaro** i timmar per dag de var schemalagda (skifttilldelning eller standardskift). Helgdagar och dagar
  utan skift räknas inte, och en halvdag ger halva skiftet.

Timavlönade behöver alltså inget schema i Crona. Koppla **ARB** till lönearten för timlön under
**Register > Löneartsstyrning**, och kontrollera att frånvarokoderna har lönearter för timavlönade.

Godkännandet stoppas om en timavlönad har stämplingar som inte blivit närvaro, till exempel en saknad
utstämpling. Rätta stämplingen eller markera närvaron för dagen, och hämta sedan frånvaro och tid igen.

## Övertid, mertid och OB

Övertid, mertid och OB räknas ur närvarons in- och utstämplingstid, för alla som stämplar.

**Ställ in reglerna** under **Löneinställningar**:

- **Heltid per dag** (standard 8 timmar) är gränsen för mertid.
- **Tidsregler**: varje rad har typ (OB eller Övertid), nivå 1–5, dagar och klockslag. Till före Från går över
  midnatt, och Från = Till betyder hela dygnet. *Helgdag* är röda dagar i helglistan. Där regler överlappar gäller
  den högsta nivån.

| Typ | Nivå | Dagar | Från | Till |
|---|---|---|---|---|
| OB | 1 | mån–fre | 18:00 | 22:00 |
| OB | 2 | alla dagar | 22:00 | 06:00 |
| OB | 3 | lör, sön, helgdag | 00:00 | 00:00 |
| Övertid | 1 | mån–fre | 06:00 | 20:00 |
| Övertid | 2 | alla dagar och helgdag | 20:00 | 06:00 |

**Så räknas det per dag:**

- Tid utanför det planerade skiftet är extra tid. Raster mellan stämplingarna räknas inte.
- **Pass på helger och röda dagar** (lediga dagar i helglistan) är extra tid hela passet, för den som har ett
  skift. Ett extrapass på en lördag ger alltså både övertid och OB.
- **Den som saknar skift** (ingen skifttilldelning och inget standardskift) har inget schema och får ingen
  övertid, bara OB. Det gäller till exempel timanställda som rings in vid behov.
- Deltidsanställda får **MER** tills dagens arbetade tid når heltid; resten blir övertid.
- Övertiden delas efter övertidsreglerna och blir **ÖT** (pengar) eller **ÖK** (komptid). Valet görs vid
  utstämplingen (*Övertidsersättning*), annars gäller den anställdes *Övertid som*.
- **OB** räknas på hela passet.

HR kan ändra koden på en rad i löneunderlaget (till exempel ÖT1 till ÖK1) eller ta bort övertid som inte var
beordrad, innan underlaget godkänns. Närvaro utan stämplingstider ger en varning, eftersom där inte går att räkna
övertid eller OB.

**I Crona:** koppla MER, ÖT1–ÖT5, ÖK1–ÖK5 och OB1–OB5 under **Register > Löneartsstyrning**. För timavlönade
dras MER- och övertidstimmarna från ARB, så lönearterna för MER och ÖT ska omfatta hela timlönen plus tillägget.
OB är ett rent tillägg.

## Varje månad

1. Sök efter **Löneunderlag** i sökfältet och skapa ett nytt. Förra månaden är förvald; välj bolag.
2. Klicka på **Hämta frånvaro och tid**. Godkänd frånvaro och timavlönades arbetade tid i månaden blir rader.
   För månadsavlönade blir en halvdag en egen rad med 50 %.
3. Granska raderna och **godkänn** löneunderlaget.
4. Klicka på **Ladda ner PAXml** och läs in filen i Crona under **Lön > Importera löneunderlag**.

Godkännandet stoppas om en anställd saknar anställningsnummer, en frånvarotyp saknar tidkod, en ledighetsansökan
har makulerats efter att raderna hämtades eller månaden redan har ett godkänt löneunderlag.

!!! tip "Prova med en anställd först"
    Första gången: läs in filen för en anställd och kontrollera i Cronas kalendarium att frånvaron hamnade på
    rätt dagar innan du läser in hela personalen.

!!! warning "Ändringar efter exporten"
    Godkänns eller makuleras en ledighet i en månad som redan är exporterad visas en varning. Rätta då
    frånvaron för hand i Crona, eller makulera löneunderlaget och gör om det.
