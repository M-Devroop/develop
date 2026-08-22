---
tags: [unity, effekte, partikel]
---

# ParticleSystem Grundlagen

## Was ist ein Particle System?
Eine Component, die viele kleine Bilder (Partikel) erzeugt und automatisch bewegt — für Triebwerksflammen, Explosionen, Staub oder Funken.

Anlegen: `GameObject > Effects > Particle System`, oder per **Add Component** an ein bestehendes Objekt.

## Aufbau
Ein Particle System besteht aus **Modulen**, die man im Inspector einzeln aufklappt und aktiviert.

### Main-Modul (immer aktiv)
| Einstellung | Bedeutung |
|---|---|
| Duration | wie lange Partikel ausgestoßen werden |
| Looping | wiederholt sich endlos (Flamme: an, Explosion: aus) |
| Start Lifetime | wie lange ein einzelner Partikel lebt |
| Start Speed | Anfangsgeschwindigkeit |
| Start Size | Größe |
| Start Color | Farbe |
| Gravity Modifier | Schwerkraft, in 2D oft 0 |
| Simulation Space | **Local** = Partikel folgen dem Objekt, **World** = bleiben zurück (gut für Schweife) |
| Play On Awake | startet automatisch — für Effekte per Skript **ausschalten** |

### Emission
Wie viele Partikel entstehen.
- **Rate over Time**: dauerhafter Strom (Triebwerk)
- **Bursts**: einmalig viele auf einen Schlag (Explosion)

### Shape
Aus welcher Form heraus die Partikel starten: Kegel, Kreis, Rechteck. Der **Angle** beim Cone bestimmt, wie weit der Strahl aufgefächert ist.

### Over Lifetime
- **Size over Lifetime**: Partikel wird kleiner (typisch für Rauch/Feuer)
- **Color over Lifetime**: Farbverlauf, meist mit Alpha auf 0 am Ende, damit er sanft verschwindet

### Renderer
- **Material**: das Bild der Partikel
- **Sorting Layer / Order in Layer**: damit der Effekt in 2D vor oder hinter den Sprites liegt

## Tipp für 2D
Effekte oft als **Kind-Objekt** an das Raumschiff hängen (z. B. Flamme hinten). Dann bewegt sich der Effekt automatisch mit.

## Verwandt
- [[ParticleSystem durch Skript steuern]]
