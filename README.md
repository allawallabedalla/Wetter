# Saisonausblick

**Ehrlicher Langfristausblick für einen Ort — mit offengelegtem Skill.**

Langfristprognosen im Netz sind meist eines von beidem: Fachkarten mit
Terzil-Wahrscheinlichkeiten, die ohne Vorbildung niemand liest — oder
„42-Tage-Trends", die tagesgenaue Temperaturen für in sechs Wochen anzeigen
und damit eine Genauigkeit vortäuschen, die es physikalisch nicht gibt.

Diese Seite versucht das Dazwischen: ortsbezogen, mit Bandbreite, und mit der
ehrlichen Angabe, wie gut das Verfahren historisch wirklich funktioniert hat.

## Status

**Mockup mit Beispieldaten.** Layout, Design und Rechenlogik-Struktur stehen;
die Anbindung an die APIs folgt. Die Beispielwerte sind plausibel gewählt,
aber nicht gemessen.

## Aufbau der Seite

| # | Abschnitt | Inhalt |
|---|-----------|--------|
| 01 | Aktuelle Lage | Gemessene Monatsabweichungen der letzten 14 Monate gegenüber dem Normalwert 1991–2020 |
| 02 | Saisonprognose | Ensemble-Monatsmittel eines Saisonvorhersagemodells, mit Streuung und Ensemble-Einigkeit |
| 03 | Vergleichbare Jahre | Die fünf ähnlichsten Jahre seit 1950 und was in ihnen danach folgte |
| 04 | Validierung | Leave-one-out über die gesamte Historie: Skill gegenüber der Baseline „es wird normal" |
| 05 | Methodik | Quellen, Rechenwege, Grenzen |

Abschnitt 04 ist der Punkt der Seite. Ein Analogverfahren sieht immer
überzeugend aus — ob es etwas taugt, zeigt erst der Vergleich gegen die
simpelste denkbare Vorhersage.

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
- Icons ausschließlich als Inline-SVG, Strichstärke 1,5 px, keine Füllung
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
