---
tags: [unity, 2d, kamera, grundlagen]
---

# Kamera Grundlagen

Die **Main Camera** ist das, was der Spieler sieht. In der Hierarchy liegt sie standardmäßig schon in der Szene und hat den Tag `MainCamera` (wichtig für `Camera.main` im Skript).

## Projection: Orthographic vs. Perspective
| Modus | Bedeutung |
|---|---|
| **Orthographic** | keine Perspektive, entfernte Objekte bleiben gleich groß → **Standard für 2D** |
| **Perspective** | echte Tiefenwirkung, für 3D |

Im 2D-Template steht das bereits auf Orthographic. Falls die Szene komisch verzerrt aussieht, ist meist das hier falsch eingestellt.

## Size (bei Orthographic)
**Size** bestimmt den Zoom. Genauer gesagt:

> Size = die **halbe Höhe** des sichtbaren Bereichs in **Units**.

- Size = 5 → man sieht 10 Units in der Höhe
- Kleinere Size = näher dran (Zoom rein)
- Größere Size = mehr Übersicht (Zoom raus)

Die **Breite** wird nicht direkt eingestellt, sondern ergibt sich aus dem Seitenverhältnis:

```
Breite in Units = Size * 2 * Aspect Ratio
```

Beispiel bei 16:9 und Size 5:
Höhe = 10 Units, Breite = 10 * (16/9) ≈ 17,8 Units

Daraus folgt eine wichtige Regel für 2D:
> Ändert sich das Seitenverhältnis, bleibt die **Höhe gleich** und die **Breite** wird schmaler oder breiter. Wichtige Dinge also nicht ganz an den linken/rechten Rand legen.

Im Skript:
```csharp
Camera.main.orthographicSize = 8f;
```

## Format / Aspect Ratio (16:9 usw.)
Oben im **Game View** gibt es ein Dropdown mit dem Anzeigeformat:
- **Free Aspect** – passt sich der Fenstergröße an (kann täuschen)
- **16:9** – Standard für PC, Konsole, Full HD (1920x1080)
- **16:10**, **4:3** – ältere bzw. andere Formate
- Feste Auflösungen wie **1920x1080**

Zum Bauen des Spiels am besten ein festes Format wie 16:9 einstellen, damit man sieht, was der Spieler wirklich sieht. Zum Testen zwischendurch auf ein anderes Format umschalten, um zu prüfen, ob nichts abgeschnitten wird.

Die erlaubten Auflösungen fürs fertige Spiel stellt man ein unter
`Edit > Project Settings > Player > Resolution and Presentation`.

## Weitere wichtige Einstellungen
| Feld | Bedeutung |
|---|---|
| **Clear Flags** | was im leeren Bereich passiert. In 2D meist `Solid Color` |
| **Background** | Hintergrundfarbe (bei Solid Color) |
| **Culling Mask** | welche Layer die Kamera überhaupt anzeigt |
| **Clipping Planes** | Near/Far — Bereich, der gezeichnet wird |
| **Depth** | Reihenfolge bei mehreren Kameras, höher = weiter vorne |

## Position in 2D
Die Kamera steht auf **Z = -10**, schaut also aus der Ferne auf die X/Y-Ebene.
> Bleibt die Kamera bei Z = 0, sieht man nichts, weil sie mitten in den Objekten steckt.

Beim Verschieben der Kamera immer nur X und Y ändern, Z auf -10 lassen.

## Kamera dem Spieler folgen lassen
```csharp
public class CameraFollow : MonoBehaviour
{
    [SerializeField] Transform target;
    [SerializeField] float smooth = 5f;

    void LateUpdate()
    {
        Vector3 ziel = new Vector3(target.position.x, target.position.y, -10f);
        transform.position = Vector3.Lerp(transform.position, ziel, Time.deltaTime * smooth);
    }
}
```
- **LateUpdate** benutzen, damit sich der Spieler vorher schon bewegt hat → kein Ruckeln.
- Die Z-Position fest auf -10 setzen, sonst rutscht die Kamera in die Szene hinein.

## Pixel Perfect (bei Pixel-Art)
Für Pixel-Grafik gibt es die Component **Pixel Perfect Camera**. Dort gibt man Referenzauflösung und *Pixels Per Unit* an, damit die Pixel sauber und gleich groß bleiben. Bei normalen Sprites braucht man das nicht.

## Verwandt
- [[2D Grundlagen]]
- [[Sprites Grundlagen]]
- [[Mausklick und Position tracken]]
