# Helgdagar och semester varje år

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
