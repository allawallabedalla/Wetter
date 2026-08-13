# Saisonausblick

**Ehrlicher Langfristausblick für einen Ort — mit offengelegtem Skill.**

Langfristprognosen im Netz sind meist eines von beidem: Fachkarten mit
Terzil-Wahrscheinlichkeiten, die ohne Vorbildung niemand liest — oder
„42-Tage-Trends", die tagesgenaue Temperaturen für in sechs Wochen anzeigen
und damit eine Genauigkeit vortäuschen, die es physikalisch nicht gibt.

Diese Seite versucht das Dazwischen: ortsbezogen, mit Bandbreite, und mit der
ehrlichen Angabe, wie gut das Verfahren historisch wirklich funktioniert hat.

## Status

**An echten Daten.** Historie, Vorhersage und Saisonprognose werden live aus
den Open-Meteo-APIs geladen und im Browser ausgewertet.

## Aufbau der Seite

Über den Abschnitten liegt eine klebende **Sprungleiste**, darunter die
**Kurzantwort**: Temperaturtendenz der nächsten 30 Tage, nächster Regentag,
mittlerer Fehler des Modells im Rückblick — je mit Sprungmarke in den
zugehörigen Abschnitt, dazu eine Zeile zur Herkunft der Daten.

Die **Einordnung**, wogegen sich die Seite messen lässt, steht am Ende vor der
Methodik. Oben stand sie mit 450 Wörtern zwischen Suchfeld und Kalender.

Die Einordnung ist in Alltagssprache geschrieben und kommt ohne Fachbegriffe
aus — keine Reanalyse, kein Skill, kein Ensemble. Die Positionierung ist bewusst
eng gefasst und nachprüfbar: Für die ersten zwei Wochen rechnen alle
Wetterseiten mit denselben öffentlichen Daten, hier ist nichts genauer. Der
Unterschied fängt danach an, und der behauptete Vorsprung ist nicht „genauer",
sondern „sagt dazu, wie oft es gestimmt hat". Das dritte Feld behauptet das
nicht, sondern zeigt es: Es wird aus dem Rückblick gefüllt und nennt den
gemessenen Fehler der letzten 120 Tage.

| # | Abschnitt | Inhalt |
|---|-----------|--------|
| 01 | Tageskalender | Echte 16-Tage-Vorhersage, danach die beste Schätzung: Klimawert des Kalendertags, verschoben um die aktuellen Tendenzen. Umschaltbar auf eine Jahresansicht mit zwölf Monatszeilen auf gemeinsamer Temperaturachse |
| 02 | Rückblick | Dieselbe Schätzung, für die letzten 120 Tage nachgerechnet und gegen die Messung gestellt |
| 03 | Regenverlauf | Nächster Regentag aus der laufenden Vorhersage, danach die Regenwahrscheinlichkeit Woche für Woche |
| 04 | Aktuelle Lage | Gemessene Monatsabweichungen der letzten 14 Monate gegenüber dem Normalwert 1991–2020 |
| 05 | Saisonprognose | Ensemble-Monatsmittel eines Saisonvorhersagemodells, mit Streuung und Ensemble-Einigkeit |
| 06 | Vergleichbare Jahre | Die fünf ähnlichsten Jahre seit 1950 und was in ihnen danach folgte |
| 07 | Validierung | Leave-one-out über die gesamte Historie: Skill gegenüber der Baseline „es wird normal" |
| 08 | Methodik | Quellen, Rechenwege, Grenzen |

Die Abschnitte 02 und 07 sind der Punkt der Seite. Ein Analogverfahren sieht
immer überzeugend aus — ob es etwas taugt, zeigt erst der Vergleich gegen die
simpelste denkbare Vorhersage.

## Rückblick

Abschnitt 02 rechnet die beste Schätzung für jeden der letzten 120 Tage neu — mit
14, 30, 60 und 90 Tagen Vorlauf — und stellt ihn der gemessenen
Tageshöchsttemperatur gegenüber. Damit der Test nicht schummelt:

- Für jeden Prüftag wird ein **Stichtag** gesetzt. Verwendet wird
  ausschließlich, was an diesem Tag vorlag — inklusive der rund **sechs Tage
  Verzögerung**, mit der die ERA5-Reanalyse erscheint.
- Trend, Streuung und Analogsuche laufen auf einer Monatsreihe, die am Ende
  des jeweiligen Ankermonats endet.
- Auch die Tagesklimatologie des Rückblicks endet **vor** dem Prüfzeitraum,
  sonst enthielte der Erwartungswert die zu prüfenden Tage bereits.
- Der **Saisonbaustein fehlt** im Rückblick: alte CFSv2-Läufe sind nicht frei
  abrufbar. Nachgerechnet werden nur Persistenz und Analogjahre; der Kalender
  nutzt zusätzlich das Saisonmodell.

120 Tage an einem Ort sind eine Stichprobe, kein Beweis — der belastbarere
Test über die gesamte Historie steht in Abschnitt 07. Im Kalender zeigen
vergangene Tage entsprechend den gemessenen Wert; der Balken darunter ist
dort der Fehler, den die beste Schätzung an diesem Tag gemacht hätte.

## Geplante Datenquellen

Alle kostenlos, ohne Schlüssel, CORS-fähig und damit direkt aus dem Browser
nutzbar — es braucht kein Backend:

- **Historie:** [Open-Meteo Archive API](https://open-meteo.com/en/docs/historical-weather-api) (ERA5-Reanalyse des ECMWF, Tageswerte ab 1950)
- **Prognose:** [Open-Meteo Seasonal API](https://open-meteo.com/en/docs/seasonal-forecast-api) (NOAA CFSv2, Ensemble)
- **Ortssuche:** [Open-Meteo Geocoding API](https://open-meteo.com/en/docs/geocoding-api)

## Design-Guideline

Plain Black & White, flat. Keine Farben, keine Schatten, keine Verläufe,
keine Eckenradien. Hierarchie entsteht ausschließlich über Linienstärke,
Flächenkontrast, Typo-Gewicht und Weißraum.

- **Warm** = gefüllte Fläche · **Kalt** = Schraffur · **Neutral** = Kontur
- Große Type gibt es nur einmal — in der Kurzantwort. Die Kennzahlenzeilen der
  Abschnitte ordnen sich darunter ein
- Temperaturabweichungen stehen in **Grad Celsius**, nicht in Kelvin; bei
  Differenzen ist beides dasselbe, nur versteht das eine jeder
- Fachbegriffe bleiben stehen — die Seite ist analytisch. Beim ersten Auftreten
  sind sie gepunktet unterstrichen und erklären sich im Tooltip
- Icons ausschließlich als Inline-SVG, Strichstärke 1,5 px, keine Füllung
- Tooltips sind eigene Elemente statt `title` — Kopfzeile, Wertzeilen und
  optional ein Abweichungsbalken, im selben Schwarzweiß wie der Rest
- Dunkelmodus invertiert die Flächen und folgt der Systemeinstellung

## Bekannte Einschränkungen

- **Die Modellwerte sind nicht bias-korrigiert.** Sauber wäre eine Korrektur
  gegen die Hindcast-Klimatologie des Modells — dafür bräuchte es Datenmengen,
  die eine statische Seite nicht laden kann. Ein Teil der gezeigten Abweichung
  ist deshalb systematischer Modellfehler, nicht Signal.
- **CFSv2 ist nicht das beste verfügbare System.** Für Europa liefert
  ECMWF SEAS5 im Copernicus-C3S-Verbund bessere Ergebnisse, ist aber nur mit
  Registrierung und als GRIB/NetCDF abrufbar — für eine reine Browser-Seite
  also nicht nutzbar.
- **Niederschlag** hat auf Saisonskala in Mitteleuropa praktisch keinen Skill.
  Die Werte stehen nur zur Einordnung da.
- **Kein Wetter für einen einzelnen Tag.** Jenseits von etwa zwei Wochen ist
  Tagesgenauigkeit nicht erreichbar.

## Technik

Eine einzige `index.html` — kein Build, kein Backend, keine Cookies, kein
Tracking. Läuft über GitHub Pages oder jeden statischen Webserver.

```bash
python3 -m http.server 8000
# → http://localhost:8000
```

## Lizenz

Siehe [LICENSE](LICENSE). Daten: Open-Meteo (CC BY 4.0), ERA5/ECMWF, NOAA CFSv2.
