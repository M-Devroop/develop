---
tags: [unity, ui, uitoolkit]
---

# UI anwenden und sichtbar machen

## Schritt für Schritt
1. **UXML anlegen**: `Create > UI Toolkit > UI Document`
2. Im **UI Builder** öffnen und Elemente hineinziehen (z. B. ein `Label` für die Punkte)
3. Jedem wichtigen Element einen **Namen** geben
4. In der Szene ein leeres GameObject anlegen, z. B. `UI`
5. Darauf die Component **UI Document** hinzufügen
6. Im Inspector eintragen:
   - **Source Asset** → das UXML
   - **Panel Settings** → ein Panel-Settings-Asset (kann man ebenfalls über `Create > UI Toolkit > Panel Settings` anlegen)
7. Play drücken — das UI liegt jetzt über dem Spiel

## Wenn nichts zu sehen ist
- Panel Settings fehlt oder Source Asset ist leer
- Das Element hat keine Größe oder keine Hintergrundfarbe
- **Sort Order** im UIDocument: höherer Wert liegt weiter vorne (wenn mehrere UIs existieren)
- Text ist weiß auf weißem Hintergrund

## Sichtbarkeit steuern
Zwei Wege, die sich unterscheiden:
| Einstellung | Wirkung |
|---|---|
| `display: none` | Element ist weg, nimmt **keinen Platz** mehr ein |
| `visibility: hidden` | Element ist unsichtbar, **behält** aber seinen Platz |
| `opacity: 0` | durchsichtig, reagiert aber weiter auf Klicks |

Für Menüs, die ein- und ausgeblendet werden, nimmt man meist `display`.

## Typischer Aufbau bei einem kleinen Spiel
- Ein UXML mit dem HUD (Punkte, Leben)
- Ein Container `GameOverPanel`, der am Anfang auf `display: none` steht
- Das Skript blendet ihn ein, wenn das Spiel vorbei ist → siehe [[UI per Skript anpassen]]

## Verwandt
- [[UI Grundlagen]]
