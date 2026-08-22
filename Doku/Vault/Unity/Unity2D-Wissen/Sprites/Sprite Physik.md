---
tags: [unity, 2d, physik]
---

# Sprite Physik (Rigidbody 2D, Collider, Material)

Wichtig: In 2D immer die **2D-Varianten** benutzen (Rigidbody **2D**, Box Collider **2D**). 3D- und 2D-Physik reden nicht miteinander.

## Rigidbody 2D
Macht ein Objekt zum Teil der Physik-Simulation. Ohne Rigidbody bewegt sich nichts von selbst.

**Body Type**
- **Dynamic**: normal beweglich, reagiert auf Kräfte und Stöße (Spieler, Asteroiden)
- **Kinematic**: bewegt sich nur, wenn man es selbst bewegt, wird nicht weggeschubst
- **Static**: bewegt sich nie (Wände) — günstig für die Performance

**Wichtige Einstellungen**
| Feld | Bedeutung |
|---|---|
| Gravity Scale | Schwerkraft. Bei Weltraum-Spielen auf **0** setzen |
| Mass | Masse — schwerer = träger bei Stößen |
| Linear Damping (Drag) | Bremst die Bewegung, wie Luftwiderstand |
| Angular Damping | Bremst die Drehung |
| Constraints | Position oder Rotation einfrieren |

> In neueren Unity-Versionen heißt *Drag* jetzt *Linear Damping*.

## Collider 2D
Legt die Form fest, mit der ein Objekt kollidiert. Der Collider muss nicht exakt das Bild sein — Hauptsache es fühlt sich fair an.

- **Box Collider 2D** – Rechteck
- **Circle Collider 2D** – Kreis, am günstigsten zu berechnen
- **Capsule Collider 2D** – abgerundet
- **Polygon Collider 2D** – passt sich der Sprite-Form an, genauer aber teurer

**Is Trigger**
- aus: Objekte prallen voneinander ab (echte Kollision)
- an: Objekte fliegen durcheinander, es wird nur gemeldet, dass sie sich berühren (z. B. für Einsammeln)

Damit es kracht, brauchen beide Objekte einen Collider und mindestens eins davon einen Rigidbody 2D.

## Physics Material 2D
Ein Asset (`Create > 2D > Physics Material 2D`), das man in das Feld **Material** des Colliders oder Rigidbodys zieht.
- **Friction**: Reibung. 0 = rutscht wie auf Eis
- **Bounciness**: Abprallen. 0 = bleibt liegen, 1 = springt fast vollständig zurück

Damit lässt sich einstellen, wie sich ein Objekt bei einem Zusammenstoß anfühlt.

## Verwandt
- [[Objekte und Physik im Skript verwenden]]
- [[Bewegung]]
