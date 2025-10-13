# 🔋 Zendure - Home Assistant - zenSDK

Gebaseerd op de [zenSDK RESTful API](https://github.com/Zendure/zenSDK) voor Home Assistant. Deze setup maakt lokaal verbinding met **één Zendure Solarflow 2400 AC / Zendure Solarflow 800 Pro (zonder zonnepanelen)** zonder gebruik te maken van integraties maar werkt met **één automatisering**. Voor de gene die graag de thuisbatterij 100% lokaal in eigen beheer wilt zonder updates van derden en netjes in Home Assistant. Zoveel mogelijk plug-n-play om in 3 stappen de batterij werkend te krijgen in Home Assistant.

![Preview](Images/NOM.png)

Vind je dit project leuk en wil je mij steunen? Trakteer mij dan op een kopje koffie ☕️ – ik codeer beter met cafeïne!

<a href="https://www.buymeacoffee.com/gielz" target="_blank">
  <img src="https://github.com/Gielz1986/Zendure-zenSDK-HA/blob/main/Images/qr-code.png?raw=true" width="150" alt="Buy Me a Coffee">
</a><br><br>

## 1️⃣ Configuration.yaml

> ⚠️ Let op: Zorg ervoor dat **HEMS is uitgeschakeld** in de Zendure-app.

Daarna gaan wij alles aanmaken voor de restful integratie (zit standaard in HA). Hiervoor heb ik een bijna plug-n-play Configuration.yaml gemaakt.
#### Benodigde hardware

- Homewizard P1 (of een andere P1-meter die data per seconde levert)
- Zendure Solarflow 2400 AC  

#### Vereiste gegevens

| Variabele            | Waar te vinden                                 |
|----------------------|------------------------------------------------|
| `<IP-BATTERIJ>`      | In de Zendure app onder **Device Information** |
| `<SERIAL-2400AC>`    | In de Zendure app onder **Device Information**  |
| `<IP-HOMEWIZARD-P1>` | In de Homewizard app (lokale API **aanzetten**) |
| `<NORDPOOL>         ` | Optioneel de eniteit van Nordpool toevoegen |

---

### #️⃣ Configuratie en herstart

1. Maak eerst een **backup** van je `configuration.yaml`
2. Pas daarna je `configuration.yaml` aan door de vereiste gegevens in te vullen. [Klik hier](https://github.com/Gielz1986/Zendure-HA-zenSDK/blob/main/Configuration.yaml) voor de configuration.yaml van deze github.
3. **Herstart Home Assistant**

Na herstarten zie je onder integraties het onderstaande verschijnen.

![Preview](Images/Rest1.avif)

*Zelf toe te voegen entiteiten op een dashboard.
![Preview](Images/Rest2.png) 

---

### #️⃣ Testen

Ga naar **Ontwikkelhulpmiddelen** in Home Assistant. Zoek onder "Acties" naar `Zendure`.

1. Voer **Snel ontladen** uit  
   > ⚠️ Let op: 2400 watt. Respecteert ingestelde limieten uit de app.

2. Daarna kun je **Stop met alles** uitvoeren

![Preview](Images/Restcommand1.avif)

##### YAML niet goed ingeladen?

Als je sensoren ziet maar geen `rest_commands`:  
> 🔁 Bug in Home Assistant – herstart meerdere keren totdat alles goed ingeladen is.

<br><br>

## 2️⃣ Zendure zenSDK (Gielz) automatisering
De motor van alles. Deze zal slim opladen en slim ontladen en samen dansen tot één geheel. Heb je bij het bovenstaande geen namen aangepast dan is het een kwestie van een nieuwe automatisering aanmaken en rechtsboven op bewerken in YAML aan te klikken. Copy en paste en opslaan met deze naam en start daarna de automatisering;

#### Automatisering toevoegen

1. Maak een nieuwe automatisering aan
2. Klik rechtsboven op **Bewerken in YAML**
3. Plak de YAML-code (zie automatisering bestand)
4. Sla op, en start de automatisering

![Preview](Images/Automation1.gif)   

![Preview](Images/Automation2.gif) 

<br><br>

## 3️⃣ Batterij mag aan de slag

1. Voeg de entiteit **Zendure 2400 AC Modus Selecteren** toe aan je dashboard
2. Voeg eventueel andere entiteiten toe die je via `configuration.yaml` hebt aangemaakt
3. De modus zal op **Standby** staan
4. Kies hier je gewenste modus om de **Zendure zenSDK (Gielz) automatisering** te activeren
5. De batterij zal nu aan de slag gaan

![Preview](Images/Modus.gif) 

<br><br>

## 🔃 (Optioneel) Nordpool
Wanneer je ook het Dynamisch Nordpool gedeelte in gebruik gaat nemen moet je voor dat je deze in gebruik neemt bij input_text.dynamisch_handmatige_periode en
input_text.dynamisch_handmatige_periode_morgen even unknown weghalen. Hierna zal het dynamisch gedeelte werken. Alles wat in de forecast gezet word zal overgenomen worden om 00:00 via de automatisering.

Dynamisch Goedkoopste Periode en Dynamisch Duurste Periode geven JA en NEE aan. deze kun je vervolgens in je eigen automatisering gebruiken waarmee je de modus van de batterij veranderd.

#### ApexCharts vandaag
```yaml
type: custom:apexcharts-card
stacked: true
header:
  show: true
  title: Vandaag
experimental:
  color_threshold: true
graph_span: 24h
apex_config:
  plotOptions:
    bar:
      columnWidth: 90%
  tooltip:
    enabledOnSeries:
      - 0
    followCursor: true
    shared: true
    intersect: false
    enabled: true
    x:
      format: HH:mm
      show: false
  grid:
    show: true
    borderColor: "#332f2c"
    strokeDashArray: 0
  chart:
    height: 200px
  yaxis:
    - title:
        text: ""
      decimalsInFloat: 2
      min: 0
      max: 1
      tickAmount: 5
      forceNiceScale: false
      labels:
        style:
          colors: "#7b7c83"
  xaxis:
    labels:
      show: false
    axisTicks:
      show: false
    axisBorder:
      color: "#616269"
  legend:
    show: false
span:
  start: day
series:
  - entity: sensor.dynamisch_nordpool
    yaxis_id: Prijs
    type: column
    color: "#ebebeb"
    float_precision: 3
    color_threshold:
      - value: 0
        color: "#186ddc"
      - value: 0.155
        color: "#04822e"
      - value: 0.2
        color: "#12A141"
      - value: 0.25
        color: "#79B92C"
      - value: 0.3
        color: "#C4D81D"
      - value: 0.35
        color: "#F3DC0C"
      - value: 0.4
        color: "#EFA51E"
      - value: 0.45
        color: "#E76821"
      - value: 0.5
        color: "#DC182F"
    name: Vandaag
    show:
      in_header: false
      legend_value: false
      extremas: true
    data_generator: |
      return entity.attributes.raw_today.map((start, index) => {
        return [new Date(start["start"]).getTime(), entity.attributes.raw_today[index]["value"]];
      });
  - entity: sensor.dynamisch_goedkoopste_periode
    color: green
    curve: stepline
    opacity: 0.2
    type: column
    name: Goedkoop
    data_generator: |
      const data = entity.attributes["raw_today"];
      if (!data) return [];

      return data.map(item => {
        const tijd = new Date(item.start).getTime();
        const waarde = item.goedkoop === "ja" ? 1 : 0;
        return [tijd, waarde];
      });
  - entity: sensor.dynamisch_goedkoopste_periode
    color: red
    curve: stepline
    opacity: 0.2
    type: column
    name: Duur
    data_generator: |
      const data = entity.attributes["raw_today"];
      if (!data) return [];

      return data.map(item => {
        const tijd = new Date(item.start).getTime();
        const waarde = item.duur === "ja" ? 1 : 0;
        return [tijd, waarde];
      });
```

#### ApexCharts morgen

```yaml
type: custom:apexcharts-card
stacked: true
header:
  show: true
  title: Morgen
experimental:
  color_threshold: true
graph_span: 24h
apex_config:
  noData:
    text: Vanaf 14:00 zijn de prijzen van morgen bekend.
  plotOptions:
    bar:
      columnWidth: 90%
  tooltip:
    enabledOnSeries:
      - 0
    followCursor: true
    shared: true
    intersect: false
    enabled: true
    x:
      format: HH:mm
      show: false
  grid:
    show: true
    borderColor: "#332f2c"
    strokeDashArray: 0
  chart:
    height: 200px
  yaxis:
    - title:
        text: ""
      decimalsInFloat: 2
      min: 0
      max: 1
      tickAmount: 5
      forceNiceScale: false
      labels:
        style:
          colors: "#7b7c83"
  xaxis:
    labels:
      show: false
    axisTicks:
      show: false
    axisBorder:
      color: "#616269"
  legend:
    show: false
span:
  start: day
  offset: +1d
series:
  - entity: sensor.dynamisch_nordpool
    yaxis_id: Prijs
    type: column
    color: "#ebebeb"
    float_precision: 3
    color_threshold:
      - value: 0
        color: "#186ddc"
      - value: 0.155
        color: "#04822e"
      - value: 0.2
        color: "#12A141"
      - value: 0.25
        color: "#79B92C"
      - value: 0.3
        color: "#C4D81D"
      - value: 0.35
        color: "#F3DC0C"
      - value: 0.4
        color: "#EFA51E"
      - value: 0.45
        color: "#E76821"
      - value: 0.5
        color: "#DC182F"
    name: Morgen
    show:
      in_header: false
      legend_value: false
      extremas: true
    data_generator: |
      return entity.attributes.raw_tomorrow.map((start, index) => {
        return [new Date(start["start"]).getTime(), entity.attributes.raw_tomorrow[index]["value"]];
      });
  - entity: sensor.dynamisch_goedkoopste_periode
    color: green
    curve: stepline
    opacity: 0.2
    type: column
    name: Goedkoop
    data_generator: |
      const data = entity.attributes["raw_tomorrow"];
      if (!data) return [];

      return data.map(item => {
        const tijd = new Date(item.start).getTime();
        const waarde = item.goedkoop === "ja" ? 1 : 0;
        return [tijd, waarde];
      });
  - entity: sensor.dynamisch_goedkoopste_periode
    color: red
    curve: stepline
    opacity: 0.2
    type: column
    name: Duur
    data_generator: |
      const data = entity.attributes["raw_tomorrow"];
      if (!data) return [];

      return data.map(item => {
        const tijd = new Date(item.start).getTime();
        const waarde = item.duur === "ja" ? 1 : 0;
        return [tijd, waarde];
      });


```



