---
tags: [unity, ui, uitoolkit]
---

# UI Grundlagen

## Zwei UI-Systeme in Unity
- **UI Toolkit** (neuer): arbeitet mit **UXML** (Aufbau) und **USS** (Aussehen), ähnlich wie HTML und CSS. Dazu gehören `UIDocument` und `rootVisualElement`.
- **uGUI / Canvas** (älter): Canvas, Text, Button und Image als GameObjects in der Hierarchy.

Diese Notizen beziehen sich auf **UI Toolkit**.

## Die drei Bausteine
| Datei / Objekt | Aufgabe |
|---|---|
| **UXML** (UI Document) | Aufbau: welche Elemente gibt es? |
| **USS** (Style Sheet) | Aussehen: Farben, Größen, Abstände |
| **Panel Settings** | verbindet das UI mit dem Bildschirm (Auflösung, Skalierung) |
| **UIDocument** (Component) | zeigt ein UXML im Spiel an |

## Wichtige Elemente
- `VisualElement` — leerer Container, wie ein `div`
- `Label` — Text
- `Button` — Knopf
- `ProgressBar` — Balken, z. B. für Leben oder Treibstoff

Elemente werden verschachtelt: ein Container enthält weitere Elemente.

## UI Builder
Fenster unter `Window > UI Toolkit > UI Builder`. Damit baut man das UXML per Drag & Drop zusammen, statt es von Hand zu schreiben.

## Layout
UI Toolkit nutzt **Flexbox**, wie im Web:
- `flex-direction`: Elemente nebeneinander (`row`) oder untereinander (`column`)
- `justify-content` / `align-items`: Ausrichtung
- `position: absolute` mit `top / left`, wenn etwas frei platziert werden soll

## Namen vergeben
Jedes Element bekommt im UI Builder ein **Name**-Feld. Über diesen Namen findet man es später im Skript wieder — also sprechende Namen vergeben, z. B. `ScoreLabel`, `RestartButton`.

## Verwandt
- [[UI anwenden und sichtbar machen]]
- [[UI per Skript anpassen]]
