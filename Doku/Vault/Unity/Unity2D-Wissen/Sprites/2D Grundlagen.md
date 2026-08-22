---
tags: [unity, 2d, grundlagen]
---

# 2D Grundlagen

## Was ist ein 2D-Projekt in Unity?
Unity ist eigentlich immer 3D. Im 2D-Modus wird nur so getan, als gäbe es keine Tiefe:
- Die Kamera steht auf **Orthographic** (keine Perspektive, alles gleich groß).
- Gespielt wird auf der **X/Y-Ebene**. Die **Z-Achse** wird fast nur benutzt, um zu entscheiden, was vorne und was hinten liegt.

## Aufbau eines Projekts
- **Scene**: Die Spielwelt. Alles, was du siehst, liegt in der Szene.
- **Hierarchy**: Liste aller Objekte in der Szene.
- **Project-Fenster**: Alle Dateien (Sprites, Skripte, Prefabs).
- **Inspector**: Einstellungen des ausgewählten Objekts.
- **Scene View** = Bearbeiten, **Game View** = so sieht es der Spieler.

## GameObject + Component
Das wichtigste Prinzip in Unity:
> Ein **GameObject** kann alleine gar nichts. Es bekommt seine Fähigkeiten durch **Components**.

Beispiel Spieler-Raumschiff:
| Component | Aufgabe |
|---|---|
| Transform | Position, Drehung, Größe |
| Sprite Renderer | zeigt das Bild an |
| Rigidbody 2D | macht es physikalisch |
| Collider 2D | macht es "anfassbar" |
| Eigenes Skript | Steuerung |

**Transform** hat jedes GameObject immer. Die anderen fügt man über *Add Component* hinzu.

## Units
Unity rechnet in **Units**, nicht in Pixeln. 1 Unit = 1 Meter in der Physik.
Wie viele Pixel eines Bildes eine Unit ergeben, legt man beim Sprite mit **Pixels Per Unit** fest (Standard: 100).

## Play Mode
Mit dem Play-Knopf startest du das Spiel. Wichtig:
> Änderungen im Play Mode gehen beim Stoppen wieder verloren.

## Prefabs
Ein **Prefab** ist eine gespeicherte Vorlage eines GameObjects (im Project-Fenster).
Damit kann man ein Objekt beliebig oft in die Szene ziehen oder im Skript erzeugen. Ändert man das Prefab, ändern sich alle Kopien.

## Verwandt
- [[Sprites Grundlagen]]
- [[Grundlagen Skripts]]
