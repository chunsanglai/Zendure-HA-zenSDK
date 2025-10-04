# zenSDK (100% lokaal) voor één 2400AC

Gebaseerd op de [zenSDK van Zendure](https://github.com/Zendure/zenSDK). Deze setup maakt lokaal verbinding met je Zendure Solarflow 2400AC batterij zonder gebruik te maken van integraties maar werkt met **één automatisering**. 

---

## (1/3) Sensoren aanmaken via `configuration.yaml`

> ⚠️ Zorg ervoor dat **HEMS is uitgeschakeld** in de Zendure-app.

### Benodigde hardware

- Homewizard P1 (of een andere P1-meter die data per seconde levert)
- Zendure Solarflow 2400 AC  

### Vereiste gegevens

| Variabele            | Waar te vinden                                 |
|----------------------|------------------------------------------------|
| `<IP-BATTERIJ>`      | In de Zendure app onder **Device Information** |
| `<SERIAL-2400AC>`    | In de app of via `properties` link hierboven  |
| `<IP-HOMEWIZARD-P1>` | In de Homewizard app (lokale API **aanzetten**) |

---

### Configuratie en herstart

1. Maak eerst een **backup** van je `configuration.yaml`
2. Pas daarna je `configuration.yaml` aan door de vereiste gegevens in te vullen
3. **Herstart Home Assistant**

Na herstarten zie je onder integraties iets zoals dit verschijnen:

![Integraties screenshot](https://tweakers.net/i/E7bxQrCWwNpk4ZDFy1JQ9ITlLVE=/fit-in/4000x4000/filters:no_upscale():strip_exif()/f/image/UXu36CdUJ1KpqALNx5mSgJcP.png?f=user_large)

### Beschikbare entiteiten

![Beschikbare entiteiten](https://tweakers.net/i/kDfTMKTEIfmvs-lswXC8JOUIGYI=/800x/filters:strip_icc():strip_exif()/f/image/ybYjJIurZDrRxGMWhmq1Psby.jpg?f=fotoalbum_large)  

---

### Testen

Ga naar **Ontwikkelhulpmiddelen** in Home Assistant. Zoek onder "Acties" naar `Zendure`.

1. Voer **Snel ontladen** uit  
   > ⚠️ Let op: 2400 watt. Respecteert ingestelde limieten uit de app.

2. Daarna kun je **Stop met alles** uitvoeren

![Ontwikkel screenshot](https://tweakers.net/i/JxXs0t_MueIsdCrf_szwqhRYaHw=/800x/filters:strip_exif()/f/image/8Eh4Sb8T2h2sj23qvkg3NOHx.png?f=fotoalbum_large)  

#### YAML niet goed ingeladen?

Als je sensoren ziet maar geen `rest_commands`:  
> 🔁 Bug in Home Assistant – herstart meerdere keren totdat alles goed ingeladen is.

---

## (2/3) Zendure zenSDK (Gielz) automatisering

De motor van alles. Deze automatisering zorgt voor:

- Slim laden en ontladen
- Automatische interactie met zonneopbrengst en netverbruik

### Automatisering toevoegen

1. Maak een nieuwe automatisering aan
2. Klik rechtsboven op **Bewerken in YAML**
3. Plak de YAML-code (zie aparte sectie of bestand)
4. Geef het een naam, sla op, en start de automatisering

---

## (3/3) Batterij laten werken

Nu alles staat:

1. Voeg de entiteit **Zendure 2400 AC Modus Selecteren** toe aan je dashboard
2. Voeg eventueel andere entiteiten toe die je via `configuration.yaml` hebt aangemaakt
3. De modus zal op **Standby** staan
4. Kies hier je gewenste modus om de **Zendure zenSDK (Gielz) automatisering** te activeren

---

## ✅ Klaar!

Je hebt nu een lokaal werkende integratie van je Zendure 2400 AC batterij met Home Assistant via zenSDK.
