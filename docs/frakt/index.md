# Frakt

ERPNext Sverige kan boka frakt via [Sendify](https://www.sendify.se). Inställningarna finns under
**Fraktinställningar**.

![Fraktinställningar](../assets/skarmbilder/frakt/fraktinstallningar.png)

- **Miljö**: börja med **Sandlåda** för att prova utan riktiga bokningar.
- **API-nyckel**: skapas i Sendify under Settings → API. Nyckeln sparas krypterad.
- **Avsändare och upphämtning**: bolag, avsändaradress, kontaktperson och **Upphämtningstider**.
- **Fraktpris till kund**: påslag i procent eller kronor, och den artikel (**Frakt**) som frakten faktureras som.

**Upphämtningstider** har en rad per veckodag med upphämtning, med *Från* och *Till*. Har ni kortare dag på
fredagar anger du det på fredagsraden. En dag utan rad har ingen upphämtning; lägg till en rad för lördag om ni
har upphämtning då. Röda dagar i bolagets helglista (till exempel julafton och midsommarafton) har aldrig
upphämtning.

En ny försändelse får nästa dag med upphämtning och den dagens tider. Ändrar du upphämtningsdagen på
försändelsen byts tiderna till den nya dagens. Väljer du en dag utan upphämtning visas ett meddelande, och
försändelsen kan inte bokas förrän du har valt en annan dag.

!!! note "Avsnittet är inte färdigskrivet"
    Bokning, fraktsedlar och spårning beskrivs senare.
