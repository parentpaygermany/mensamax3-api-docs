---
title: MensaMax API-Dokumentation

language_tabs: # must be one of https://git.io/vQNgJ
  - shell: cURL
  - json--request: JSON Body-Anfrage
  - json--response: JSON Body-Antwort
  - html: HTTP-Statuscode

title: MensaMax API-Dokumentation

toc_footers:
  - <a href='https://mensamax.de'>MensaMax</a>
  - "<style>.toc-wrapper .logo { padding: 20px; margin-left: auto; margin-right: auto; } .http-status-code { padding: 5px; background-color: white; }</style>"

search: true
---

# MensaMax API
### Dokumentation Version 3.22.0811.01

Herzlich willkommen zur MensaMax-API. Mithilfe dieser Schnittstelle können folgende Daten abgerufen oder Aktionen ausgeführt werden:

- Abrufen von Wochenspeiseplänen für einen gewissen Zeitraum
- Setzen des Wochenspeiseplans für eine Woche
- Abrufen der Bestellzahlen von Menüs für ein Datum oder einen Zeitraum
- Erstellen, Suchen, Löschen und Inaktivsetzen von Personen
- Abrufen, Erstellen, Ändern und Löschen von Klassensufen und Klassen
- Zuweisen und Zurückgeben von Identifikationsmedien

# Authentifizierung

MensaMax verwendet API-Keys, um den Zugriff auf die API zu gewähren. Im Folgenden ist beschrieben, wie ein API-Key zur Authentifizierung genutzt, erstellt und wiederrufen werden kann.

MensaMax erwartet, dass der API-Key in allen Anfragen an den Server in einem Header mitgesendet wird:

`Authorization: Bearer [API-KEY]`

<aside class="notice">
Der Platzhalter <code>[API-KEY]</code> muss mit dem persönlichen API-Key ersetzt werden.

Alle Projekt spezifischen Werte sind in dieser Dokumentation <code>[zwischen eckigen Klammern]</code> dargestellt. Diese Werte dienen als Platzhalter und müssen entsprechend ersetzt werden.
</aside>

<aside class="warning">
cURL Anfragen werden ohne Zeilenümbrüche ausgeführt. Es gibt Sonderzeichen um Zeilenumbrüche in Shells zu erzwingen. Diese sind allerdings Shell abhängig. In dieser Dokumentation sind die cURL Beispiele zur besseren Übersichtlichkeit mit Zeilenumbrüchen dargestellt.
</aside>

## Erstellung eines API-Keys

Der API-Key sollte immer an einem sicheren Ort aufbewahrt werden. Der API-Key verliert nach 30 Minuten seine Gültigkeit und muss neu angefragt werden.

```shell
curl "https://[SERVER]/api/createToken"
  --request POST
  --header "Content-Type: application/json"
  --data '{"projekt":"[PROJEKT]","einrichtung":"[EINRICHTUNG]","benutzername":"[BENUTZERNAME]","passwort":"[PASSWORT]"}'
```

```json--request
{
  "projekt": "[PROJEKT]",
  "einrichtung": "[EINRICHTUNG]",
  "benutzername": "[BENUTZERNAME]",
  "passwort": "[PASSWORT]"
}
````

```json--response
"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9 ... yMnzUs5mqLRbv0_k1HgsVQ95tinByUw1ktFn7nm_T4I"
```

```html
200 OK
```

### HTTP-Anfrage

`POST https://[SERVER]/api/createToken`

### Body-Parameter

Parameter | Erforderlich | Datentyp | Beschreibung 
--------- | :----------: | -------- | ------------
projekt | &#x2611; | string | Name des Projekts.
einrichtung | &#x2611; | string | Einrichtung des Nutzers.
benutzername | &#x2611; | string | Benutzername eines Nutzers, der zur API-Nutzung berechtigt ist.
passwort | &#x2611; | string | Passwort des Benutzers.

### Body-Antwort

Parameter | Datentyp | Beschreibung 
--------- | -------- | ------------
 | string | API-Key

# API Status

## API Zustand

Diese Anfrage gibt den Zustand und die Version der API zurück. So kann sicher gestellt werden, dass die API bereit für Anfragen ist.

```shell
curl --request GET "https://[SERVER]/api/health"
```

```json--response
{
  "date": "2022-04-20T14:34:16.8782331+02:00",
  "version": "3.22.1904.1"
}
```

```html
200 OK
```

### HTTP-Anfrage

`GET https://[SERVER]/api/health`

### Body-Antwort

Parameter | Datentyp | Beschreibung
--------- | -------- | ------------
date | string | Aktueller Zeitpunkt der Anfrage.
version | string | Version der API.

# Wochenspeiseplan

## Wochenspeisepläne abrufen

Dieser Endpunkt liefert eine Liste an Wochenspeiseplänen für den angegebenen Zeitraum. Wird das Bis-Datum nicht gesetzt, enthält die Liste nur einen Wochenspeiseplan des Von-Datums.

```shell
curl "https://[SERVER]/api/wochenspeiseplan?mandantName=[MANDANTNAME]&von=[VON]&bis=[BIS]"
  --header "Authorization: Bearer [API-KEY]"
```

```json--response
[
  {
    "textOben": "",
    "textUnten": "Täglich Abwechslung vom frischen Salat-Buffet mit Knabbergemüse (Möhren, Kohlrabi, Gurke) und Blattsalaten (Eisbergsalat, Lollo Rosso) mit wechselndem Dressing und Dip ",
    "startDatum": "2019-08-12T00:00:00",
    "endDatum": "2019-08-18T23:59:59",
    "tagesspeiseplaene": [
      {
        "datum": "2019-08-12T00:00:00",
        "positionen": [
          {
            "menuegruppe": {
              "bezeichnung": "Menü 1"
            },
            "menue": {
              "externeId": null,
              "bezeichnung": null,
              "speisen": [
                {
                  "bezeichnung": "Cevapcici (Geflügelhackröllchen)",
                  "vegetarisch": false,
                  "menuefolge": 1,
                  "piktogramm": 6,
                  "zusatzstoffe": [],
                  "allergene": [
                    {
                      "bezeichnung": "glutenhaltiges Getreide Weizen",
                      "kennung": "20"
                    },
                    {
                      "bezeichnung": "Eier und Eierzeugnisse",
                      "kennung": "22"
                    },
                    {
                      "bezeichnung": "Senf oder Senferzeugnisse",
                      "kennung": "29"
                    }
                  ],
                  "naehrwerte": [
                    {
                      "bezeichnung": "Brennwert",
                      "menge": 143,
                      "einheit": "kJ",
                      "bezugsmenge": 100,
                      "bezugseinheit": "g"
                    },
                    {
                      "bezeichnung": "Fett",
                      "menge": 1.7,
                      "einheit": "g",
                      "bezugsmenge": 100,
                      "bezugseinheit": "g"
                    }
                  ]
                },
                {
                  "bezeichnung": "Tzatziki",
                  "vegetarisch": false,
                  "menuefolge": 1,
                  "zusatzstoffe": [],
                  "allergene": [
                    {
                      "bezeichnung": "Milch oder Milcherzeugnisse",
                      "kennung": "26"
                    }
                  ],
                  "naehrwerte": [
                    {
                      "bezeichnung": "Brennwert",
                      "menge": 143,
                      "einheit": "kJ",
                      "bezugsmenge": 100,
                      "bezugseinheit": "g"
                    },
                    {
                      "bezeichnung": "Fett",
                      "menge": 1.7,
                      "einheit": "g",
                      "bezugsmenge": 100,
                      "bezugseinheit": "g"
                    }
                  ]
                },
                {
                  "bezeichnung": "Röstkartoffeln",
                  "vegetarisch": false,
                  "menuefolge": 1,
                  "zusatzstoffe": [],
                  "allergene": [],
                  "naehrwerte": []
                }
              ]
            },
            "mahlzeit": null
          },
          {
            "menuegruppe": {
              "bezeichnung": "Menü 2"
            },
            "menue": {
              "externeId": null,
              "bezeichnung": null,
              "speisen": [
                {
                  "bezeichnung": "Ravioli mit Käsefüllung in bunter Rahmsoße (Möhren, Sellerie, Lauch)",
                  "zusatzstoffe": [
                    {
                      "bezeichnung": "mit Antioxidationsmitteln",
                      "kennung": "c"
                    }
                  ],
                  "allergene": [
                    {
                      "bezeichnung": "glutenhaltiges Getreide Weizen",
                      "kennung": "20"
                    },
                    {
                      "bezeichnung": "Eier und Eierzeugnisse",
                      "kennung": "22"
                    },
                    {
                      "bezeichnung": "Milch oder Milcherzeugnisse",
                      "kennung": "26"
                    }
                  ],
                  "vegetarisch": true,
                  "menuefolge": 1,
                  "piktogramm": 1
                }
              ]
            },
            "mahlzeit": null
          },
          {
            "menuegruppe": {
              "bezeichnung": "Nachtisch"
            },
            "menue": {
              "externeId": null,
              "bezeichnung": null,
              "speisen": [
                {
                  "bezeichnung": "Griesbrei",
                  "vegetarisch": true,
                  "menuefolge": 2,
                  "piktogramm": 1,
                  "allergene": [
                    {
                      "bezeichnung": "glutenhaltiges Getreide Weizen",
                      "kennung": "20"
                    },
                    {
                      "bezeichnung": "Milch oder Milcherzeugnisse",
                      "kennung": "26"
                    }
                  ]
                }
              ]
            },
            "mahlzeit": null
          }
        ],
        "zusatztext": "Täglicher Zusatztext"
      },
      {
        "datum": "2019-08-13T00:00:00",
        "positionen": [
          {
            "menuegruppe": {
              "bezeichnung": "Menü 1"
            },
            "menue": {
              "externeId": null,
              "bezeichnung": null,
              "speisen": [
                {
                  "bezeichnung": "Cevapcici (Geflügelhackröllchen)",
                  "vegetarisch": false,
                  "menuefolge": 1,
                  "zusatzstoffe": [],
                  "allergene": [
                    {
                      "bezeichnung": "glutenhaltiges Getreide Weizen",
                      "kennung": "20"
                    },
                    {
                      "bezeichnung": "Eier und Eierzeugnisse",
                      "kennung": "22"
                    },
                    {
                      "bezeichnung": "Senf oder Senferzeugnisse",
                      "kennung": "29"
                    }
                  ]
                },
                {
                  "bezeichnung": "Tzatziki",
                  "vegetarisch": false,
                  "menuefolge": 1,
                  "zusatzstoffe": [],
                  "allergene": [
                    {
                      "bezeichnung": "Milch oder Milcherzeugnisse",
                      "kennung": "26"
                    }
                  ]
                },
                {
                  "bezeichnung": "Röstkartoffeln",
                  "vegetarisch": false,
                  "menuefolge": 1,
                  "zusatzstoffe": [],
                  "allergene": []
                },
                {
                  "bezeichnung": "Griesbrei",
                  "vegetarisch": true,
                  "menuefolge": 2,
                  "allergene": [
                    {
                      "bezeichnung": "glutenhaltiges Getreide Weizen",
                      "kennung": "20"
                    },
                    {
                      "bezeichnung": "Milch oder Milcherzeugnisse",
                      "kennung": "26"
                    }
                  ]
                }
              ]
            },
            "mahlzeit": null
          },
          {
            "menuegruppe": {
              "bezeichnung": "Menü 2"
            },
            "menue": {
              "externeId": null,
              "bezeichnung": null,
              "speisen": [
                {
                  "bezeichnung": "Ravioli mit Käsefüllung",
                  "zusatzstoffe": [
                    {
                      "bezeichnung": "mit Antioxidationsmitteln",
                      "kennung": "c"
                    }
                  ],
                  "allergene": [
                    {
                      "bezeichnung": "glutenhaltiges Getreide Weizen",
                      "kennung": "20"
                    },
                    {
                      "bezeichnung": "Eier und Eierzeugnisse",
                      "kennung": "22"
                    },
                    {
                      "bezeichnung": "Milch oder Milcherzeugnisse",
                      "kennung": "26"
                    }
                  ],
                  "vegetarisch": true,
                  "menuefolge": 1
                },
                {
                  "bezeichnung": "bunte Rahmsoße (Möhren, Sellerie, Lauch)",
                  "zusatzstoffe": [],
                  "allergene": [
                    {
                      "bezeichnung": "Sellerie oder Sellerieerzeugnisse",
                      "kennung": "28"
                    }
                  ]
                },
                {
                  "bezeichnung": "Griesbrei",
                  "vegetarisch": true,
                  "menuefolge": 2,
                  "allergene": [
                    {
                      "bezeichnung": "glutenhaltiges Getreide Weizen",
                      "kennung": "20"
                    },
                    {
                      "bezeichnung": "Milch oder Milcherzeugnisse",
                      "kennung": "26"
                    }
                  ]
                }
              ]
            },
            "mahlzeit": null
          }
        ],
        "zusatztext": "Täglicher Zusatztext"
      },
      {
        "datum": "2019-08-14T00:00:00",
        "positionen": [
          {
            "menuegruppe": {
              "bezeichnung": "Menü 1"
            },
            "menue": {
              "externeId": null,
              "bezeichnung": null,
              "speisen": [
                {
                  "bezeichnung": "Cevapcici (Geflügelhackröllchen) mit Tzatziki und Röstkartoffeln",
                  "vegetarisch": false,
                  "menuefolge": 1,
                  "zusatzstoffe": [],
                  "allergene": [
                    {
                      "bezeichnung": "glutenhaltiges Getreide Weizen",
                      "kennung": "20"
                    },
                    {
                      "bezeichnung": "Eier und Eierzeugnisse",
                      "kennung": "22"
                    },
                    {
                      "bezeichnung": "Milch oder Milcherzeugnisse",
                      "kennung": "26"
                    },
                    {
                      "bezeichnung": "Senf oder Senferzeugnisse",
                      "kennung": "29"
                    }
                  ]
                },
                {
                  "bezeichnung": "Griesbrei",
                  "vegetarisch": true,
                  "menuefolge": 2,
                  "allergene": [
                    {
                      "bezeichnung": "glutenhaltiges Getreide Weizen",
                      "kennung": "20"
                    },
                    {
                      "bezeichnung": "Milch oder Milcherzeugnisse",
                      "kennung": "26"
                    }
                  ]
                }
              ]
            },
            "mahlzeit": null
          },
          {
            "menuegruppe": {
              "bezeichnung": "Menü 2"
            },
            "menue": {
              "externeId": null,
              "bezeichnung": null,
              "speisen": [
                {
                  "bezeichnung": "Ravioli mit Käsefüllung in bunter Rahmsoße (Möhren, Sellerie, Lauch)",
                  "zusatzstoffe": [
                    {
                      "bezeichnung": "mit Antioxidationsmitteln",
                      "kennung": "c"
                    }
                  ],
                  "allergene": [
                    {
                      "bezeichnung": "glutenhaltiges Getreide Weizen",
                      "kennung": "20"
                    },
                    {
                      "bezeichnung": "Eier und Eierzeugnisse",
                      "kennung": "22"
                    },
                    {
                      "bezeichnung": "Milch oder Milcherzeugnisse",
                      "kennung": "26"
                    },
                    {
                      "bezeichnung": "Sellerie oder Sellerieerzeugnisse",
                      "kennung": "28"
                    }
                  ],
                  "naehrwerte": [],
                  "vegetarisch": true,
                  "menuefolge": 1
                },
                {
                  "bezeichnung": "Griesbrei",
                  "vegetarisch": true,
                  "menuefolge": 2,
                  "allergene": [
                    {
                      "bezeichnung": "glutenhaltiges Getreide Weizen",
                      "kennung": "20"
                    },
                    {
                      "bezeichnung": "Milch oder Milcherzeugnisse",
                      "kennung": "26"
                    }
                  ]
                }
              ]
            },
            "mahlzeit": null
          }
        ],
        "zusatztext": "Täglicher Zusatztext"
      }
    ]
  }
]
```

```html
200 OK
```

### HTTP-Anfrage

`GET https://[SERVER]/api/wochenspeiseplan?mandantName=[MANDANTNAME]&von=[VON]&bis=[BIS]`

### Query-Parameter

Parameter | Erforderlich | Datentyp | Beschreibung
--------- | :----------: | -------- | ------------
von | &#x2611; | string | Datum, ab dem Wochenspeisepläne abgerufen werden sollen.
bis | | string | Datum, bis zu dem Wochenspeisepläne abgerufen werden sollen.
mandantName | &#x2611; | string | Mandantname für dessen Mandantenverbund der Wochenspeiseplan abgerufen werden soll.

### Body-Antwort

array of [Wochenspeiseplan](#wochenspeiseplan-2)

### Wochenspeiseplan

Parameter | Erforderlich | Datentyp | Beschreibung
--------- | :------------------------: | -------- | ------------
textOben | | string | Text, der oberhalb des Speiseplans angezeigt wird.
textUnten | | string | Text, der unterhalb des Speiseplans angezeigt wird.
startDatum | &#x2611; | string | Datum des ersten Tages in der Woche.
endDatum | | string | Datum des letzten Tages in der Woche.
tagesspeiseplaene | &#x2611; | array of [Tagesspeiseplan](#tagesspeiseplan) | Liste an Tagesspeiseplänen.

### Tagesspeiseplan

Parameter | Erforderlich | Datentyp | Beschreibung
----------| :------------------------: | -------- | ------------
datum | &#x2611; | string | Datum des Wochentags.
positionen | &#x2611; | array of [Tagesspeiseplanposition](#tagesspeiseplanposition) | Liste der Tagesspeiseplanpositionen.
zusatztext | | string | Täglicher Zusatztext.

### Tagesspeiseplanposition

Parameter | Erforderlich | Datentyp | Beschreibung
--------- | :------------------------: | -------- | ------------
menuegruppe | &#x2611; | [Menuegruppe](#menuegruppe) | Menügruppe der Tagesspeiseplanposition.
menue | &#x2611; | [Menue](#menue) | Menü der Tagesspeiseplanpostition.
mahlzeit | | [Mahlzeit](#mahlzeit) | Mahlzeit der Tagesspeiseplanpostion.

### Menuegruppe

Parameter | Erforderlich | Datentyp | Beschreibung
--------- | :------------------------: | -------- | ------------
bezeichnung | &#x2611; | string | Name der Menügruppe. Darüber wird die Menügruppe eindeutig identifiziert.

<aside class="notice">
Die Menügruppe kann nicht neu gesetzt werden und wird über die Bezeichnung abgeglichen.
</aside>

### Menue

Parameter | Erforderlich | Datentyp | Beschreibung
--------- | :------------------------: | -------- | ------------
externeId | | string | Externe ID des Menüs.
bezeichnung | &#x2611; | string | Bezeichnung des Menüs (aktuell immer null und wird beim setzen ignoriert, kann aber in der Zukunft evtl. gesetzt werden).
speisen | &#x2611; | array of [Speise](#speise) | Liste von Speisen des Menüs.
zusatzstoffe | | array of [Zusatzstoff](#zusatzstoff) | Liste von Zusatzstoffen eines Menüs. Diese werden anhand der in den Speisen angegebenen Zusatzstoffen zusammengestellt und können **nicht** extra gesetzt werden.
allergene | | array of [Allergen](#allergen) | Liste von Allergenen eines Menüs. Diese werden anhand der in den Speisen angegebenen Allergenen zusammengestellt und können **nicht** extra gesetzt werden.

### Mahlzeit

Parameter | Erforderlich | Datentyp | Beschreibung
--------- | :------------------------: | -------- | ------------
bezeichnung | &#x2611; | string | Bezeichnung der Mahlzeit (Frühstück, Mittagessen, ...)

<aside class="notice">
Mahlzeit ist aktuell immer NULL, kann aber in der Zukunft evtl. gesetzt werden.
</aside>

### Speise

Parameter | Erforderlich | Datentyp | Beschreibung
--------- | :------------------------: | -------- | ------------
bezeichnung | &#x2611; | string | Bezeichnung der Speise, anhand derer die Identifikation erfolgt.
zusatzstoffe | | array of [Zusatzstoff](#zusatzstoff) | Liste von Zusatzstoffen.
allergene | | array of [Allergen](#allergen) | Liste von Allergenen.
naehrwert | | array of [Naehrwert](#naehrwert) | Liste von Nährwerten.
vegetarisch | | boolean | True = vergetarisch, false = nicht vegetarisch.
menuefolge | | number | Menüfolge (0 .. 2) Standardwert: Hauptspeise.
piktogramm | | number | Piktogramm der Speise (1 .. 12).

<aside class="notice">
Menüfolgen:
  <ol start="0">
    <li>Vorspeise</li>
    <li>Hauptspeise</li>
    <li>Nachspeise</li>
  </ol>
</aside>

<aside class="notice">
Auswahl Piktogramme:
  <ol>
    <li>Aehre</li>
    <li>DGE</li>
    <li>Essen</li>
    <li>Fisch</li>
    <li>Gemüse</li>
    <li>Huhn</li>
    <li>Rind</li>
    <li>Pastateller</li>
    <li>Salat</li>
    <li>Schaf</li>
    <li>Schwein</li>
    <li>Suppenteller</li>
  </ol>
</aside>

### Zusatzstoff

Parameter | Erforderlich | Datentyp | Beschreibung
--------- | :------------------------: | -------- | ------------
bezeichnung | &#x2611; | string | Bezeichnung des Zusatzstoffs, anhand derer die Identifikation erfolgt.
kennung | &#x2611; | string | Kennung des Zusatzstoffes.

<aside class="notice">
Ein Zusatzstoff wird durch die Bezeichnung und Kennung identifiziert. Wenn es keinen Zusatzstoff mit dieser Bezeichnung und Kennung gibt, dann wird die Kennung überprüft. Wenn es diese Kennung mit einer anderen Bezeichnung noch nicht gibt, wird der Zusatzstoff neu angelegt.
</aside>

### Allergen

Parameter | Erforderlich | Datentyp | Beschreibung
--------- | :------------------------: | -------- | ------------
bezeichnung | &#x2611; | string | Bezeichnung des Allergens, anhand derer die Identifikation erfolgt.
kennung | &#x2611; | string | Kennung des Allergens.


<aside class="notice">
Ein Allergen wird durch die Bezeichnung und Kennung identifiziert. Wenn es kein Allergen mit dieser Bezeichnung und Kennung gibt, dann wird die Kennung überprüft. Wenn es diese Kennung mit einer anderen Bezeichnung noch nicht gibt, wird das Allergen neu angelegt.
Zusatzstoffe und Allergene mit der selben Kennung sind nicht zulässig.
</aside>

### Naehrwert

Parameter | Erforderlich | Datentyp | Beschreibung
--------- | :------------------------: | -------- | ------------
bezeichnung | &#x2611; | string | Bezeichnung des Nährwerts, anhand derer abgeglichen wird.
menge | &#x2611; | number | Menge des Nährwerts (z.B. 13 bei 13g Kohlenhydrate pro 1kg).
einheit | &#x2611; | string | Einheit des Nährwerts (z.B. g bei 13g Kohlenhydrate pro 1kg).
bezugsmenge | &#x2611; | number | Bezugsmenge (z.B. 1 bei 13g Kohlenhydrate pro 1kg).
bezugseinheit | &#x2611; | string | Bezugseinheit (z.B. kg bei 13g Kohlenhydrate pro 1kg).

## Wochenspeiseplan setzen

Dieser Endpunkt setzt den Wochenspeiseplan für die Woche, die im übergebenen Wochenspeiseplan enthalten ist. Der Wochenspeiseplan wird für die Mandantengruppe gesetzt, in dem der angegebene Mandant enthalten ist.

```shell
curl "https://[SERVER]/api/wochenspeiseplan"
  --request POST
  --header "Authorization: Bearer [API-KEY]"
  --header "Content-Type: application/json"
  --data '{"textOben": "An apple a day keeps the doctor away", ...}'
```

```json--request
{
  "textOben": "An apple a day keeps the doctor away.",
  "textUnten": "",
  "startDatum": "2022-04-18T00:00:00+01:00",
  "endDatum": "2022-04-22T00:00:00+01:00",
  "tagesspeiseplaene": [...]
}
````

```json--response
"Wochenspeiseplan der Kalenderwoche von 18.04.2022 bis 22.04.2022 ist erfolgreich in Projekt in MensaMax hinterlegt."
```

```html
200 OK
```

### HTTP-Anfrage

`POST https://[SERVER]/api/wochenspeiseplan?mandantName=[MANDANTNAME]`

### Query-Parameter

Parameter | Erforderlich | Beschreibung
--------- | :----------: | ------------
mandantName | &#x2611; | Mandantname, für dessen Mandantenverbund der Wochenspeiseplan gesetzt werden soll.

### Body-Parameter

Im Body muss der **[Wochenspeiseplan](#wochenspeiseplan-2)** gesetzt werden.

### Body-Antwort

Parameter | Datentyp | Beschreibung
--------- | -------- | ------------
 | string | Status der Anfrage.

# Bestellzahlen

## Bestellzahlen abrufen

Dieser Endpunkt liefert die Menü-Bestellzahlen für den angegebenen Zeitraum. Wird das Bis-Datum nicht angegeben, enthält die Liste nur die Bestellzahlen für das Von-Datum.
Es können die Bestellzahlen für maximal eine Woche abgerufen werden.

<aside class="notice">
Die Bezeichnungen für Ausgabeschichten, -orte und Zubereitungsmerkmale können von den Kunden selbst gesetzt werden und können daher ungewöhnliche Bezeichnungen enthalten. Ausgabeorte und Ausgabeschichten enhalten nun IDs.
</aside>

```shell
curl "https://[SERVER]/api/bestellzahlen?von=[VON]&bis=[BIS]"
  --header "Authorization: Bearer [API-KEY]"
```

```json--response
[
  {
    "datum": "2018-10-01T00:00:00",
    "mandanten": [
      {
        "bezeichnung": "Grundschule",
        "menuegruppen": [
          {
            "bezeichnung": "Vollwertkost",
            "menues": [
              {
                "bezeichnung": null,
                "anzahl": 233,
                "speisenamen": ["Rinder-Schmorfleisch", "Buntes Gemüse", "Kartoffelspalten", "Blattsalate"],
                "ausgabeorte": [
                  {
                    "bezeichnung": null,
                    "anzahl": 233,
                    "ausgabeschichten": [
                      {
                        "bezeichnung": "ab 12.45 Uhr",
                        "anzahl": 233,
                        "zubereitungsmerkmale": [
                          {
                            "bezeichnung": "Nuss-Allergie",
                            "anzahl": 1
                          },
                          {
                            "bezeichnung": "Passiert",
                            "anzahl": 2
                          }
                        ]
                      }
                    ]
                  }
                ]
              },
              {
                "bezeichnung": null,
                "anzahl": 39,
                "speisenamen": ["Tomatencremesuppe", "Milchreis mit Apfelmus"],
                "ausgabeorte": [
                  {
                    "bezeichnung": null,
                    "anzahl": 39,
                    "ausgabeschichten": [
                      {
                        "bezeichnung": "ab 12.45 Uhr",
                        "anzahl": 39,
                        "zubereitungsmerkmale": null
                      }
                    ]
                  }
                ]
              }
            ],
            "final": true
          },
          {
            "bezeichnung": "Vegetarisch",
            "menues": [
              {
                "bezeichnung": null,
                "anzahl": 233,
                "speisenamen": ["Rinder-Schmorfleisch", "Buntes Gemüse", "Kartoffelspalten", "Blattsalate"],
                "ausgabeorte": [
                  {
                    "id": 1,
                    "bezeichnung": null,
                    "anzahl": 233,
                    "ausgabeschichten": [
                      {
                        "id": 1,
                        "bezeichnung": "ab 12.45 Uhr",
                        "anzahl": 233,
                        "zubereitungsmerkmale": [
                          {
                            "bezeichnung": "Nuss-Allergie",
                            "anzahl": 1
                          },
                          {
                            "bezeichnung": "Passiert",
                            "anzahl": 2
                          }
                        ]
                      }
                    ]
                  }
                ]
              },
              {
                "bezeichnung": null,
                "anzahl": 39,
                "speisenamen": ["Tomatencremesuppe", "Milchreis mit Apfelmus"],
                "ausgabeorte": [
                  {
                    "id": 1,
                    "bezeichnung": null,
                    "anzahl": 39,
                    "ausgabeschichten": [
                      {
                        "id": 1,
                        "bezeichnung": "ab 12.45 Uhr",
                        "anzahl": 39,
                        "zubereitungsmerkmale": null
                      }
                    ]
                  }
                ]
              }
            ],
            "final": true
          }
        ]
      },
      {
        "bezeichnung": "Realschule",
        "menuegruppen": [
          {
            "bezeichnung": "Vollwertkost",
            "menues": [
              {
                "bezeichnung": null,
                "anzahl": 405,
                "speisenamen": ["Rinder-Schmorfleisch", "Buntes Gemüse", "Kartoffelspalten", "Blattsalate"],
                "ausgabeorte": [
                  {
                    "id": 1,
                    "bezeichnung": null,
                    "anzahl": 405,
                    "ausgabeschichten": [
                      {
                        "id": 1,
                        "bezeichnung": "ab 12.00 Uhr",
                        "anzahl": 114,
                        "zubereitungsmerkmale": [
                          {
                            "bezeichnung": "Koscher/Halāl",
                            "anzahl": 3
                          },
                          {
                            "bezeichnung": "Nuss-Allergie, Koscher/Halāl",
                            "anzahl": 1
                          }
                        ]
                      },
                      {
                        "id": 2,
                        "bezeichnung": "ab 12.45 Uhr",
                        "anzahl": 291,
                        "zubereitungsmerkmale": [
                          {
                            "bezeichnung": "Koscher/Halāl",
                            "anzahl": 12
                          }
                        ]
                      }
                    ]
                  }
                ]
              }
            ],
            "final": true
          }
        ]
      }
    ]
  },
  {
    "datum": "2018-10-02T00:00:00",
    "mandanten": [
      {
        "bezeichnung": "Grundschule",
        "menuegruppen": []
      },
      {
        "bezeichnung": "Realschule",
        "menuegruppen": []
      }
    ]
  }
]
```

```html
200 OK
```

### HTTP-Anfrage

`GET https://[SERVER]/api/bestellzahlen?von=[VON]&bis=[BIS]`

### Query-Parameter

Parameter | Erforderlich | Datentyp | Beschreibung
--------- | :----------: | -------- | ------------
von | &#x2611; | string | Format '2099-01-31'
bis | | string | Format '2099-12-31' 

### Body-Antwort

array of [Bestellzahlen](#bestellzahlen-2)

### Bestellzahlen

Parameter | Datentyp | Beschreibung
--------- | ---------| ------------
datum | string | Datum der Tagesbestellzahlen.
mandanten | array of [Mandant](#mandant) | Liste an Mandanten und aller Bestellungen.

### Mandant

Parameter | Datentyp | Beschreibung
--------- | -------- | ------------
bezeichnung | string | Name der Einrichtung.
menuegruppen | array of [Menuegruppe](#menuegruppe-2) | Menuegruppen, die bestellt wurden. |

### Menuegruppe

Parameter | Datentyp | Beschreibung
| ----------- | -------- | ------------------------------------------------------------ |
bezeichnung | string | Bezeichnung der Menuegruppe.
menues | array of [Menue](#menue-2) | Menüs einer Menügruppe, die bestellt wurden.
final | boolean | Gibt an, ob die Bestellzahlen final sind.

### Menue

Parameter | Datentyp | Beschreibung
----------| -------- | ------------
exterenId | string | Externe ID des Menüs, falls vorhanden.
bezeichnung | string | Bezeichnung des Menüs (erst in nächster MensaMax Version gesetzt, aktuell immer NULL.
anzahl | number | Anzahl der bestellten Menüs.
speisenamen | array of string | Namen der Speisen dieses Menüs.
ausgabeorte | array of [Ausgabeort](#ausgabeort) | Ausgabeorte der Menüs.

### Ausgabeort

Parameter| Datentyp | Beschreibung
-------- | -------- | ------------
bezeichnung | string | Name des Ausgabeortes.
anzahl | number | Anzahl der an diesem Ausgabeort bestellten Menüs.
ausgabeschichten | array of [Ausgabeschicht](#ausgabeschicht) | Ausgabeschichten der Ausgabeorte.

### Ausgabeschicht

Parameter | Datentyp | Beschreibung
--------- | -------- | ------------
bezeichnung | string | Bezeichnung der Ausgabeschicht.
anzahl | number | Anzahl der in dieser Schicht ausgegebenen Menüs.
zubereitungsmerkmale | array of [Zubereitungsmerkmal](#zubereitungsmerkmal) | Zubereitungsmerkmale (z.B. Allergie, ...).

### Zubereitungsmerkmal

Parameter | Datentyp | Beschreibung
--------- | -------- | ------------
bezeichnung | string | Bezeichnung des Zubereitungsmerkmals.
anzahl | number | Anzahl der Menüs in dieser Schicht mit diesem Zubereitungsmerkmal.

# Person

## Person erstellen

Dieser Endpunkt erstellt eine Person für den angegebenen Mandanten und gibt die ID der erstellten Person zurück.
  
```shell
curl "https://[SERVER]/api/person?mandantName=[MANDANTNAME]"
  --request POST
  --header "Authorization: Bearer [API-KEY]"
  --header "Content-Type: application/json"
  --data '{"vorname":"MensaMax","name":"Benutzer"}'
```

```json--request
{
  "vorname": "MensaMax",
  "name": "Benutzer"
}
```

```json--response
1234
```

```html
201 CREATED
```

### HTTP-Anfrage

`POST https://[SERVER]/api/person?mandantName=[MANDANTNAME]`

### Query-Parameter

Parameter | Erforderlich | Beschreibung
--------- | :----------: | ------------
mandantName | &#x2611; | Mandantname, für den die Person erstellt werden soll.

### Body-Parameter

### Person

Parameter | Erforderlich | Datentyp | Beschreibung
--------- | :----------: | -------- | ------------
| id | &#x2611; (beim Ändern) | number | ID der Person. Wird beim Erstellen einer Person weggelassen. Muss beim Ändern einer Person angegeben werden.
| vorname | &#x2611; | string | Vorname der Person.
| name | &#x2611; | string | Name der Person.
| geburtsdatum | | string | Geburtsdatum der Person.
| loginname | | string | Benutzername. Wenn NULL oder nicht angegeben, dann wird der Loginname automatisch generiert.
| telefon | | string | Telefonnummer der Person.
| handy | | string | Handynummer der Person.
| fax | | string | Faxnummer der Person.
| email | | string | E-Mail Adresse der Person.
| standardausgabeort | | string | Bezeichnung des Ausgabeortes. Es wird keine Änderung vorgenommen, wenn der Ausgabeort nicht existiert.
| bestellregeln | | array of [Bestellregel](#bestellregel) | Bei Angabe von Bestellregeln muss ein Standardausgabeort übergeben werden, der in Regeln definiert ist.
| anrede | | [Anrede](#anrede) | Anrede der Person.
| preiskategorien | | array of [Preiskategorie](#preiskategorie) | Beim Weglassen werden Standardpreiskategorien angelegt.
| adresse | | [Adresse](#adresse) | Adresse der Person.
| nummern | | [Nummer](#nummer) | Sammlung an Nummern.
| klasse | | [Klasse](#klasse) | Klasse oder Abteilung der Person.
| einrichtung | | [Einrichtung](#einrichtung) | Einrichtung der Person.
| identifikation | | array of [Identifikation](#identifikation) | Liste aller Identifikationen.
| setInitialPasswort | | boolean | Gibt an, ob ein Initialpasswort für die Person erzeugt werden soll. Standardwert: True.
| aktivAb | | string | Das Datum ab wann die Person aktiv ist und Bestellungen vornehmen darf. Bei nicht Angabe aktiv ab heute.

### Bestellregel

Parameter | Erforderlich | Datentyp | Beschreibung
--------- | :----------: | -------- | ------------
regelId | &#x2611; | string | Regel ID.
value | &#x2611; | string | Regel Wert.

### Preiskategorie

Parameter | Erforderlich | Datentyp | Beschreibung
--------- | :----------: | -------- | ------------
day | &#x2611; | number | Wochentag (0 .. 7)
bezeichnung | | string | Wenn die Preiskategorie nicht existiert, wird die Standardpreiskategorie verwendet.
von | | string | Gültigkeitsbeginn. Bei nicht Angabe gültig ab heute.

<aside class="notice">
Wochentage:
  <ol start="0">
    <li>Sonntag</li>
    <li>Montag</li>
    <li>Dienstag</li>
    <li>Mittwoch</li>
    <li>Donnerstag</li>
    <li>Freitag</li>
    <li>Samstag</li>
  </ol>
</aside>

### Anrede

Parameter | Erforderlich | Datentyp | Beschreibung
--------- | :----------: | -------- | ------------
bezeichnung | &#x2611; | string | Bezeichnung der Anrede.
briefanrede | &#x2611; | string | Briefanrede, zur verwendung in Serienbriefen.
geschlecht | &#x2611; | number | Geschlecht der Person (0..3, 100).
schluessel | | string | Schlüssel der Anrede.

<aside class="notice">
Geschlecht:
  <ul style="list-style: none;">
    <li>0. Neutral</li>
    <li>1. Männlich</li>
    <li>2. Weiblich</li>
    <li>3. Divers</li>
    <li>100. Juristisch</li>
  <ul>
</aside>

### Adresse

Parameter | Erfoderlich | Datentyp | Beschreibung
--------- | :---------: | -------- | ------------
strasse | | string | Straße
hausNr | | string | Hausnummer
hausNrZusatz | | string | Hausnummernzusatz
plz | | string | Postleitzahl
ort | | string | Ort
ortsteil | | string | Ortsteil

### Nummer

Parameter | Erforderlich | Datentyp | Beschreibung
--------- | :----------: | -------- | ------------
personalnummer | | string | Personalnummer
kundennummer | | string | Kundennummer
objektnummer | | string | Objektnummer
steuernummer | | string | Steuernummer
kennziffer | | string | Kennziffer

### Klasse

Parameter | Erforderlich | Datentyp | Beschreibung
--------- | :----------: | -------- | ------------
bezeichnung | | string | Bezeichnung der Klasse.

### Einrichtung

Parameter | Erforderlich | Datentyp | Beschreibung
--------- | :----------: | -------- | ------------
bezeichnung | | string | Bezeichnung der Einrichtung. Wenn die Einrichtung mit dieser Bezeichnung in MensaMax nicht vorhanden ist, wird sie für den Mandanten angelegt.

### Identifikation

Parameter | Erforderlich | Datentyp | Beschreibung
--------- | :----------: | -------- | ------------
typ | &#x2611; | number | Typ der Identifikation (1..5).
value | &#x2611; | string | Wert der Identifikation.

<aside class="notice" id="identifikation-typ">
Identifikation Typ:
  <ol>
    <li>Barcode</li>
    <li>RFID</li>
    <li>PAN</li>
    <li>IPERS</li>
    <li>UID</li>
  </ol>
</aside>

### Body-Antwort

Parameter | Datentyp | Beschreibung
--------- | -------- | ------------
 | number | ID der Person.

## Person suchen

Über diesen Endpunkt kann eine Suche der Personen durchgeführt werden.

```shell
curl "https://[SERVER]/api/person/search"
  --request POST
  --header "Authorization: Bearer [API-KEY]"
  --header "Content-Type: application/json"
  --data '{"name": "Mustermann", ...}'
```

```json--request
{
  "name": "Mustermann",
  "vorname": "Max",
  "geburtsdatum": "1990-12-31T00:00:00+01:00",
  "loginname": "MusMa0007",
  "personalnummer": "123456789",
  "kundennummer": "123456789",
  "objektnummer": "123456789",
  "steuernummer": "123456789",
  "kennziffer": "123456789",
  "klasse": "1a"
}
```

```json--response
[
    {
      "id": 1,
      "active": true,
      "name": "Mustermann",
      "vorname": "Max",
      "geburtsdatum": "1990-12-31T00:00:00+01:00",
      "loginname": "MusMa0007",
      "telefon": "07123 / 1234",
      "handy": "0151 / 987654321",
      "fax": "07123 / 1235",
      "email": "max.mustermann@maxdorf.de",
      "standardausgabeort": "Lounge",
      "bestellregeln": [
        {
          "regelId": "Ausgabort",
          "value": "Lounge"
        }
      ],
      "preiskategorien": [
        {
          "day": 0,
          "bezeichnung": "Erwachsener"
        },
        {
          "day": 1,
          "bezeichnung": "Erwachsener"
        },
        {
          "day": 2,
          "bezeichnung": "Erwachsener"
        },
        {
          "day": 3,
          "bezeichnung": "Erwachsener"
        },
        {
          "day": 4,
          "bezeichnung": "Schüler"
        },
        {
          "day": 5,
          "bezeichnung": "Schüler"
        },
        {
          "day": 6,
          "bezeichnung": "Schüler"
        }
      ],
      "anrede": {
        "bezeichnung": "Herr",
        "briefanrede": "Sehr geehrter Herr",
        "geschlecht": 1,
        "schluessel": "0001"
      },
      "adresse": {
        "strasse": "Lindenstr.",
        "hausNr": "1",
        "hausNrZusatz": "a",
        "plz": "71234",
        "ort": "Maxdorf",
        "ortsteil": ""
      },
      "nummern": {
        "personalnummer": "123456789",
        "kundennummer": "123456789",
        "objektnummer": "123456789",
        "steuernummer": "123456789",
        "kennziffer": "123456789"
      },
      "klasse": {
        "bezeichnung": "1a"
      },
      "einrichtung": {
        "bezeichnung": "Grundschule"
      },
      "identifikation": [
        {
          "typ": 2,
          "value": "010BE120F5"
        }
      ],
      "aktivAb": "2021-09-01T00:00:00"
    },
    ...
]

```

```html
200 OK
```

### HTTP-Anfrage

`POST https://[SERVER]/api/person/search?mandantName=[MANDANTNAME]`

### Query-Parameter

Parameter | Erforderlich | Beschreibung
--------- | :----------: | ------------
mandantName | &#x2611; | Einschränkung der Suche auf Personen, die diesem Mandanten zugeordnet sind.

### Body-Parameter

Parameter | Erforderlich | Datentyp | Beschreibung
--------- | :----------: | -------- | ------------
name | | string | Nachname der Person.
vorname | | Vorname der Person.
geburtsdatum | | string | Geburtsdatum der Person.
loginname | | string | Login der Person.
personalnummer | | string | Personalnummer der Person.
kundennummer | | string | Kundennummer der Person.
objektnummer | | string | Objektnummer der Person.
steuernummer | | string | Steuernummer der Person.
kennziffer | | string | Kennziffer der Person.
klasse | | string | Klasse der Person.

<aside class="notice">
Alle Body-Parameter sind optional und können kombiniert werden.
<span style="font-weight: bold;">Ein Parameter wird ignoriert (nicht erkannt), wenn er falsch geschrieben ist.</span>
</aside>

### Body-Antwort

array of [Person](#person-2)

## Person ändern

Dieser Endpunkt ändert eine Person für den angegebenen Mandanten.
Wichtig hierbei ist die ID der Person.

```shell
curl "https://[SERVER]/api/person?mandantName=[MANDANTNAME]"
  --request POST
  --header "Authorization: Bearer [API-KEY]"
  --header "Content-Type: application/json"
  --data '{"id": 1234, ...}'
```

```json--request
{
  "id": 1234,
  "vorname": "Max edit",
  "name": "Mustermann edit",
  ...
}
```

```json--response
1234
```

```html
200 OK
```

### HTTP-Anfrage

`POST https://[SERVER]/api/person?mandantName=[MANDANTNAME]`

### Query-Parameter

Parameter | Erforderlich | Beschreibung
--------- | :----------: | ------------
mandantName | &#x2611; | Mandantname, für den die Person geändert werden soll.

### Body-Parameter

Datenstruktur einer [Person](#person-2).

### Body-Antwort

Parameter | Datentyp | Beschreibung
--------- | -------- | ------------
 | number | ID der Person.

## Person inaktiv setzen

Setzt eine Person in MensaMax zu einem bestimmten Datum auf inaktiv. Das Datum muss im vorgegebenen Format übergeben werden.

```shell
curl "https://[SERVER]/api/person/[ID]/deactivate"
  --request POST
  --header "Authorization: Bearer [API-KEY]"
  --header "Content-Type: application/json"
```

```json--request
{
  "deactivationDate": "2099-1-1"
}
```

```html
200 OK
```

### HTTP-Anfrage

`POST https://[SERVER]/api/person/[ID]/deactivate?mandantName=[MANDANTNAME]`

### Query-Parameter

Parameter | Erforderlich | Beschreibung
--------- | :----------: | ------------
id | &#x2611; | ID der Person.
mandantName | &#x2611; | Mandantname, für den die Person inaktiv gesetzt werden soll.

### Body-Parameter

Parameter | Erforderlich | Datentyp | Beschreibung
--------- | :----------: | -------- | ------------
deactivationDate | | string | Das Datum, zu dem die Person auf inaktiv gesetzt werden soll.

<aside class="notice">
Das Datum darf nicht in der Vergangenheit liegen, eine Person kann also nicht rückwirkend inaktiv gesetzt werden.
Ansonsten wird die Person zum heutigen Datum inaktiv gesetzt.
</aside>

## Person aktiv setzen

Setzt eine Person in MensaMax aktiv.

```shell
curl "https://[SERVER]/api/person/[ID]/activate?mandantName=[MANDANTNAME]"
  --request POST
  --header "Authorization: Bearer [API-KEY]"
```

```html
200 OK
```

### HTTP-Anfrage

`POST https://[SERVER]/api/person/[ID]/activate?mandantName=[MANDANTNAME]`

### Query-Parameter

Parameter | Erforderlich | Beschreibung
--------- | :----------: | ------------
id | &#x2611; | ID der Person.
mandantName | &#x2611; | Mandantname, für den die Person aktiv gesetzt werden soll.

## Person löschen

Über diesen Endpunkt kann eine Person gelöscht werden.

```shell
curl "https://[SERVER]/api/person/[ID]"
  --request DELETE
  --header "Authorization: Bearer [API-KEY]"
```

```html
200 OK
```

### HTTP-Anfrage

`DELETE https://[SERVER]/api/person/[ID]`

### Query-Parameter

Parameter | Erforderlich | Beschreibung
--------- | :----------: | ------------
id | &#x2611; | ID der Person, die gelöscht werden soll.

## Ein Bild zu einer Person hochladen

Dieser Request läd ein Bild hoch und weist dieses der angegebenen Person zu.

```shell
curl --request POST 'https://[SERVER]/api/upload?personId=[ID]'
--header "Authorization: Bearer [API-KEY]"
--form "=@/C:/Pictures/1234.jpg"

# PowerShell:
$headers = New-Object "System.Collections.Generic.Dictionary[[String],[String]]"
$headers.Add("Authorization", "Bearer [API-KEY]")

$multipartContent = [System.Net.Http.MultipartFormDataContent]::new()
$multipartFile = '/C:/Pictures/1234.jpg'
$FileStream = [System.IO.FileStream]::new($multipartFile, [System.IO.FileMode]::Open)
$fileHeader = [System.Net.Http.Headers.ContentDispositionHeaderValue]::new("form-data")
$fileHeader.Name = ""
$fileHeader.FileName = "/C:Pictures/1234.jpg"
$fileContent = [System.Net.Http.StreamContent]::new($FileStream)
$fileContent.Headers.ContentDisposition = $fileHeader
$multipartContent.Add($fileContent)

$body = $multipartContent

$response = Invoke-RestMethod 'https://[SERVER]/api/upload?personId=[ID]' -Method 'POST' -Headers $headers -Body $body
$response | ConvertTo-Json
```

```html
200 OK
```

### HTTP-Anfrage

`POST https://[SERVER]/api/upload?personId=[ID]`

### Query-Parameter

Parameter | Erforderlich | Beschreibung
--------- | :----------: | ------------
personId | &#x2611; | ID der Person.

## Personenbestellungen

Liefert eine Liste der Personen mit ihren Bestellungen an einem bestimmten Tag.

```shell
curl "https://[SERVER]/api/person/bestellungen?mandantName=[MANDANTNAME]&datum=[DATUM]"
  --request GET
  --header "Authorization: Bearer [API-KEY]"
  --header "Content-Type: application/json"
```

```json--request
```

```json--response
{
    "anzahlPersonen": 1,
    "personen": [
        {
            "person": {
                "id": 1,
                "vorname": "Max",
                "nachname": "Mustermann",
                "loginName": "MusMa007",
                "einrichtung": "Realschule"
            },
            "anzahlBestellungen": 1,
            "bestellungen": [
                {
                    "bezeichnung": "Vollwertkost",
                    "bestelldatum": "2022-11-07T00:00:00",
                    "preiskategorie": "Preis1",
                    "anzahl": 1,
                    "kommentar": null
                }
            ]
        }
    ]
}
```

```html
200 OK
```

### HTTP-Anfrage

`GET https://[SERVER]/api/person/bestellungen?mandantName=[MANDANTNAME]&datum=[DATUM]`

### Query-Parameter

Parameter | Erforderlich | Beschreibung
--------- | :----------: | ------------
mandantName | &#x2611; | Mandantname.
datum | &#x2611; | Tag der Bestellungen.

### Body-Antwort

Parameter | Datentyp | Beschreibung
--------- | -------- | ------------
anzahlPersonen | number | Anzahl der Personen mit Bestellungen an diesem Tag.
personen | array of [Personenbestellung] (#personenbestellung) | Bestellungdetails einer Person.

### Personenbestellung

Parameter | Datentyp | Beschreibung
--------- | -------- | ------------
person | [Person](#person-3) | Details zur Person im Bestellkontext.
anzahlBestellungen | number | Anzahl der Bestellungen dieser Person.
bestellungen | array of [Bestellung](#bestellung) | Liste der Bestellungen dieser Person.

### Person (Bestellkontext)

Parameter | Datentyp | Beschreibung
--------- | -------- | ------------
id | number | ID der Person.
vorname | string | Vorname der Person.
nachname | string | Nachname der Person.
loginname | string | Loginname der Person.
einrichtung | string | Einrichtung der Person.

### Bestellung

Parameter | Datentyp | Beschreibung
--------- | -------- | ------------
bezeichnung | string | Bezeichnung der Bestellung.
bestelldatum | string | Datum der Bestellung.
preiskategorie | string | Preiskategorie der Bestellung.
anzahl | number | Bestellanzahl.
kommentar | string | Ein Kommentar zur Bestellung.

## Personen ohne Bestellung

Liefert eine Liste der Personen ohne Bestellung an einem bestimmten Tag.

```shell
curl "https://[SERVER]/api/person/ohneBestellung?mandantName=[MANDANTNAME]&datum=[DATUM]"
  --request GET
  --header "Authorization: Bearer [API-KEY]"
  --header "Content-Type: application/json"
```

```json--request
```

```json--response
{
    "anzahlPersonen": 1,
    "personen": [
        {
            "id": 1,
            "vorname": "Max",
            "nachname": "Mustermann",
            "loginName": "MusMa007",
            "einrichtung": "Realschule"
        }
    ]
}
```

```html
200 OK
```

### HTTP-Anfrage

`GET https://[SERVER]/api/person/ohneBestellung?mandantName=[MANDANTNAME]&datum=[DATUM]`

### Query-Parameter

Parameter | Erforderlich | Beschreibung
--------- | :----------: | ------------
mandantName | &#x2611; | Mandantname.
datum | &#x2611; | Tag der Bestellungen.

### Body-Antwort

Parameter | Datentyp | Beschreibung
--------- | -------- | ------------
anzahl | number | Anzahl der Personen ohne Bestellung an diesem Tag.
personen | array of [Person] (#person-3) | Details zur Person im Bestellkontext.

# Klassenstufe

## Klassenstufe erstellen

Über diesen Endpunkt kann eine Klassenstufe erstellt werden.

```shell
curl "https://[SERVER]/api/klassenstufe"
  --request POST 
  --header "Authorization: Bearer [API-KEY]" 
  --header "Content-Type: application/json" 
  --data '{"bezeichnung":"Neue Klassenstufe","reihenfolge":1}'
```

```json--request
{
  "bezeichnung": "Neue Klassenstufe",
  "reihenfolge": 1
}
```

```json--response
{
  "id": 1,
  "bezeichnung": "Neue Klassenstufe",
  "reihenfolge": 1
}
```

```html
201 CREATED
```

### HTTP-Anfrage

`POST https://[SERVER]/api/klassenstufe`

### Body-Parameter

Parameter | Erforderlich | Datentyp | Beschreibung
--------- | :----------: |--------- | ------------
bezeichnung |  &#x2611; | string | Bezeichnung für die Klassenstufe.
reihenfolge |  &#x2611; | number | Reihenfolge für die Klassenstufe.

### Body-Antwort

### Klassenstufe

Parameter | Datentyp | Beschreibung
--------- | -------- | ------------
id | number | ID der neu erstellten Klassenstufe.
bezeichnung | string | Bezeichnung der Klassenstufe.
reihenfolge | string | Reihenfolge der Klassenstufe.

## Klassenstufen abrufen

Liefert die Anzahl und eine Liste der Klassenstufen.
Über einen zweiten Endpunkt kann eine Klassenstufe mit der entsprechenden ID abgefragt werden.

```shell
curl "https://[SERVER]/api/klassenstufen"
 --request GET
 --header "Authorization: Bearer [API-KEY]"
```

```json--response
{
  "anzahl": 1,
  [
    {
      "id": 1,
      "bezeichnung": "Neue Klassenstufe",
      "reihenfolge": 1
    }
  ]
}
```

```html
200 OK
```

### HTTP-Anfrage

`GET https://[SERVER]/api/klassenstufen`

### Body-Antwort

Parameter | Datentyp | Beschreibung
--------- | -------- | ------------
anzahl | number | Anzahl der Klassenstufen.
 | array of [Klassenstufe](#klassenstufe-2) | Liste der Klassenstufen.

```shell
curl "https://[SERVER]/api/klassenstufe/1"
 --request GET
 --header "Authorization: Bearer [API-KEY]"
```

```json--response
{
  "id": 1,
  "bezeichnung": "Neue Klassenstufe",
  "reihenfolge": 1
}
```

```html
200 OK
```

### HTTP-Anfrage

`GET https://[SERVER]/api/klassenstufe/[ID]`

### Query-Parameter

Parameter | Erforderlich | Beschreibung
--------- | :----------: | ------------
id | &#x2611; | ID der Klassenstufe.

### Body-Antwort

Parameter | Datentyp | Beschreibung
--------- | -------- | ------------
 | [Klassenstufe](#klassenstufe-2) | Eine bestimmte Klassenstufe.

## Klassenstufe ändern

```shell
curl "https://[SERVER]/api/klassenstufe"
  --request PUT
  --header "Authorization: Bearer [API-KEY]"
  --header "Content-Type: application/json"
  --data '{"id":1,"bezeichnung":"Klassenstufe geändert","reihenfolge":2}'
```

```json--request
{
  "id": 1,
  "bezeichnung": "Klassenstufe geändert",
  "reihenfolge": 2
}
```

```html
204 NO CONTENT
```

### HTTP-Anfrage

`PUT https://[SERVER]/api/klassenstufe`

### Body-Parameter

Parameter | Erforderlich | Datentyp | Beschreibung
--------- | :----------: | -------- | ---------
id |  &#x2611; | number | ID der Klassenstufe.
bezeichnung | &#x2611; | Bezeichnung der Klassenstufe.
reihenfolge |  &#x2611; | Reihenfolge der Klassenstufe.

## Klassenstufe löschen

Dieser Endpunkt löscht eine Klassenstufe. Die Klassenstufe kann nur gelöscht werden, wenn dieser keine Klasse zugeordnet ist.

```shell
curl "https://[SERVER]/api/klassenstufe/[ID]"
  --request DELETE
  --header "Authorization: Bearer [API-KEY]"
```

```html
204 NO CONTENT
```

### HTTP-Anfrage

`DELETE https://[SERVER]/api/klassenstufe/[ID]`

### Query-Parameter

Parameter | Erforderlich | Beschreibung
--------- | :----------: | ------------
id | &#x2611; | ID der Klassenstufe.

# Klasse

## Klasse erstellen

Diser Endpunkt legt eine neue Klasse an. Eine Klasse kann eine Folgeklasse haben. Eine Klasse muss einer Klassenstufe zugeordnet sein.

```shell
curl "https://[SERVER]/api/klasse"
  --request POST
  --header "Authorization: Bearer [API-KEY]"
  --header "Content-Type: application/json"
  --data '{"bezeichnung":"Neue Klasse","rang":1,"folgeklasseId":2,"klassenstufeId":1}'
```

```json--request
{
  "bezeichnung": "Neue Klasse",
  "rang": 1,
  "folgeklasseId": 2,
  "klassenstufeId": 1
}
```

```json--response
{
  "id": 1,
  "bezeichnung": "Neue Klasse",
  "rang": 1,
  "folgeklasseId": 2,
  "klassenstufeId": 1
}
```

```html
201 CREATED
```

### HTTP-Anfrage

`POST https://[SERVER]/api/klasse`

### Body-Parameter

Parameter | Erforderlich | Datentyp | Beschreibung
--------- | :----------: | -------- | ---------
bezeichnung |  &#x2611; | string | Bezeichnung der Klasse.
rang | &#x2611; | number | Rang der Klasse
folgeklasseId | | number | ID der Folgeklasse.
klassenstufeId | &#x2611; | number | ID der Klassenstufe.

### Body-Antwort

### Klasse

Parameter | Datentyp | Beschreibung
--------- | -------- | ------------
id | number | ID der Klasse.
bezeichnung | string | Bezeichnung der Klasse.
rang | number | Rang der Klasse
folgeklasseId | number | ID der Folgeklasse.
klassenstufeId | number | ID der Klassenstufe.

## Klassen abrufen

Dieser Enpunkt ruft eine Klassenliste ab.
Ein weiterer Endpunkt ruft eine bestimmte Klasse ab.

```shell
curl "https://[SERVER]/api/klassen"
  --request GET
  --header "Authorization: Bearer [API-KEY]"
```

```json--response
{
  "anzahl": 1,
  [
    {
      "id": 1,
      "bezeichnung": "Neue Klasse",
      "rang": 1,
      "folgeklasseId": 2,
      "klassenstufeId": 1
    }
  ]
}
```

```html
200 OK
```

### HTTP-Anfrage

`GET https://[SERVER]/api/klassen`

### Body-Antwort

Parameter | Datentyp | Beschreibung
--------- | -------- | ------------
anzahl | number | Anzahl der Klassen.
 | array of [Klasse](#klasse-3) | Liste der Klassen.

```shell
curl "https://[SERVER]/api/klasse/1"
  --request GET
  --header "Authorization: Bearer [API-KEY]"
```

```json--response
{
  "id": 1,
  "bezeichnung": "Neue Klasse",
  "rang": 1,
  "folgeklasseId": 2,
  "klassensufeId": 1
}
```

```html
200 OK
```

### HTTP-Anfrage

`GET https://[SERVER]/api/klasse/[ID]`

### Query-Parameter

Parameter | Erforderlich | Beschreibung
--------- | :----------: | ---------
id | &#x2611; | ID der Klasse.

### Body-Antwort

[Klasse](#klasse-3)

## Klasse ändern

Dieser Endpunkt ändert eine Klasse.

```shell
curl "https://[SERVER]/api/klasse"
  --request PUT
  --header "Authorization: Bearer [API-KEY]"
  --header "Content-Type: application/json"
  --data '{"id":1,"bezeichnung":"Klasse geändert","rang":2,"folgeklasseId":2,"klassenstufeId":2}'
```

```json--request
{
  "id": 1,
  "bezeichnung": "Klasse geändert",
  "rang": 2,
  "folgeklasseId": 2,
  "klassenstufeId": 2
}
```

```html
204 NO CONTENT
```

### HTTP-Anfrage

`PUT https://[SERVER]/api/klasse`

### Body-Parameter

Parameter | Erforderlich | Datentyp | Beschreibung
--------- | :----------: | -------- | ---------
id | &#x2611; | number | ID der Klasse.
bezeichnung | &#x2611; | string | Bezeichnung der Klasse.
rang | &#x2611; | number | Rang der Klasse.
folgeklasseId | | number | ID der Folgeklasse.
klassenstufeId | &#x2611; | number | ID der Klassenstufe.

## Klasse löschen

Dieser Endpunkt löscht eine bestimmte Klasse.

```shell
curl "https://[SERVER]/api/klasse/[ID]"
  --request DELETE
  --header "Authorization: Bearer [API-KEY]"
```

```html
204 NO CONTENT
```

### HTTP-Anfrage

`DELETE https://[SERVER]/api/klasse/[ID]`

### Query-Parameter

Parameter | Erforderlich | Beschreibung
--------- | :----------: | ---------
id | &#x2611; | ID der Klasse.

# Identifikationsmedium

## Zuweisen

Dieser Endpunkt weist einer Person ein oder mehrere Identifikationsmedien zu. Das Datum kann weggelassen werden, dann wird das Medium zum heutigen Datum zugewiesen.

```shell
curl "https://[SERVER]/api/identifikation?mandantName=[MANDANTNAME]"
  --request POST
  --header "Authorization: Bearer [API-KEY]"
  --header "Content-Type: application/json"
  --data '[{"personId": 1, ...}]'
```

```json--request
[
  {
    "personId": 1,
    "identifikationsmedium": [
      {
        "typ": 2,
        "value": "ABAB1212",
        "datum": "2020-07-30T01:01:01.000"
      }
    ]
  },
  {
    "personId": 2,
    "identifikationsmedium": [
      {
        "typ": 2,
        "value": "EDEDED1212",
        "datum": "2020-07-30T01:01:01.000"
      }
    ]
  }
]
```

```html
200 OK
```

### HTTP-Anfrage

`POST https://[SERVER]/api/identifikation?mandantName=[MANDANTNAME]`

### Query-Parameter

Parameter | Erforderlich | Beschreibung
--------- | :----------: | ------------
mandantName | &#x2611; | Einschränkung der Personen, die diesem Mandanten zugeordnet sind.

### Body-Parameter

array of [Identifikation](#identifikation-2)

### Identifikation

Parameter | Erforderlich | Datentyp | Beschreibung
--------- | :----------: | -------- | ------------
personId | &#x2611; | number | ID der Person.
identifikationsmedium | &#x2611; | array of [Identifikationsmedium](#identifikationsmedium-2)

### Identifikationsmedium

Parameter | Erforderlich | Datentyp | Beschreibung
--------- | :----------: | -------- | ------------
typ | &#x2611; | number | [Identifikationstyp](#identifikation-typ)
value | &#x2611; | string | Wert der Identifikation.
datum | | string | Datum des ersten Tages in der Woche.

## Zurückgeben

Über diesen Endpoint können Identifikationsmedien zurückgegeben werden.
Dieses wird zu dem jetzigen Zeitpunkt zurückgegeben.

```shell
curl "https://[SERVER]/api/identifikation/deactivate?mandantName=[MANDANTNAME]"
  --request POST
  --header "Authorization: Bearer [API-KEY]"
  --header "Content-Type: application/json"
  --data '[{"personId": 1, ...}]'
```

```json--request
[
  {
    "personId": 1,
    "identifikationsmedium": [
      {
        "typ": 2,
        "value": "ABAB1212"
      }
    ]
  },
  {
    "personId": 2,
    "identifikationsmedium": [
      {
        "typ": 2,
        "value": "EDEDED1212"
      }
    ]
  }
]
```

```html
200 OK
```

### HTTP-Anfrage

`POST https://[SERVER]/api/identifikation?mandantName=[MANDANTNAME]`

### Query-Parameter

Parameter | Erforderlich | Beschreibung
--------- | :----------: | ------------
mandantName | &#x2611; | Einschränkung der Personen, die diesem Mandanten zugeordnet sind.

### Body-Parameter

array of [Identifikation](#identifikation-2)

# Server Fehler Antworten

HTTP-Fehlercode | Beschreibung
--------------- | ---------
400 | Bad Request - Die Anfrage ist nicht valide. Meistens sind die Eingaben fehlerhaft oder fehlen komplett.
401 | Unauthorized - Der API-Key ist ungültig, abgelaufen oder fehlt in der Anfrage.
403 | Forbidden - Der Zugriff auf diesen Endpoint ist nicht erlaubt.
404 | Not Found - Der angefragte Endpoint existiert nicht oder ein Datensatz wurde nicht gefunden.
405 | Method Not Allowed - Die angegebene HTTP-Methode ist auf diesem Endpunkt nicht erlaubt.
406 | Not Acceptable - Das angefragte Format ist nicht JSON.
409 | Conflict - Die Anfrage steht in Konflikt mit dem aktuellen Zustand auf dem Server. Die Antwort enthält Hinweise für die Lösung.
410 | Gone - Der angefragte Endpoint existiert nicht mehr.
429 | Too Many Requests - Zu viele Anfragen in kurzer Zeit.
500 | Internal Server Error - Wir haben auf unseren Servern ein Problem. Bitte später wiederholen.
503 | Service Unavailable - Wir sind aufgrund von Wartungsarbeiten vorübergehend offline.

Teilweise enthalten die Fehler-Antworten des Servers eine genauere Beschreibung im Body um die Fehlersuche so gut wie möglich einzugrenzen.
