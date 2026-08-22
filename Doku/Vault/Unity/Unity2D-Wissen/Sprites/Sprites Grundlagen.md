---
tags: [unity, 2d, sprites]
---

# Sprites Grundlagen

## Was ist ein Sprite?
Ein **Sprite** ist ein 2D-Bild, das in der Szene angezeigt wird — z. B. das Raumschiff, ein Asteroid oder der Hintergrund.

## Bild importieren
Bild in den `Assets`-Ordner ziehen. Dann im Inspector prüfen:
- **Texture Type**: `Sprite (2D and UI)` (im 2D-Projekt ist das schon Standard)
- **Pixels Per Unit**: wie groß das Bild in der Welt wird. Kleinerer Wert = größeres Objekt.
- **Sprite Mode**: `Single` für ein Bild, `Multiple` für ein Sprite-Sheet mit mehreren Bildern.
- **Filter Mode**: `Point (no filter)` für Pixel-Art, sonst `Bilinear`.

## Sprite Renderer
Die Component, die das Bild tatsächlich anzeigt. Wichtige Felder:
- **Sprite**: welches Bild angezeigt wird
- **Color**: Farbton und Transparenz (Alpha)
- **Flip X / Y**: Bild spiegeln
- **Sorting Layer** und **Order in Layer**: Reihenfolge, wer vor wem liegt

## Zeichenreihenfolge
Zwei Möglichkeiten, um zu bestimmen was vorne liegt:
1. **Order in Layer**: höhere Zahl = weiter vorne (üblicher Weg)
2. **Z-Position** im Transform

Sorting Layers legt man an unter `Edit > Project Settings > Tags and Layers`, z. B.:
`Background` → `Objects` → `Player` → `Effects`

## Sprite Editor
Damit zerschneidet man ein Sprite-Sheet in einzelne Sprites (*Slice*) und setzt den **Pivot** — den Punkt, um den sich das Objekt dreht und der als Position gilt. Bei einem Raumschiff, das sich drehen soll, ist der Pivot meist die Mitte.

## Verwandt
- [[Sprite Physik]]
- [[2D Grundlagen]]
