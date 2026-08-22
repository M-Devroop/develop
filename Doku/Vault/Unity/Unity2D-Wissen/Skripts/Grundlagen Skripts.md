---
tags: [unity, csharp, skripte]
---

# Grundlagen Skripts

## Skript anlegen
`Create > C# Script` im Project-Fenster, dann auf ein GameObject ziehen.
> Der Dateiname muss genau so heißen wie die Klasse im Skript, sonst funktioniert es nicht.

## Grundgerüst
```csharp
using UnityEngine;

public class PlayerController : MonoBehaviour
{
    void Start()
    {
        // wird einmal am Anfang aufgerufen
    }

    void Update()
    {
        // wird in jedem Frame aufgerufen
    }
}
```

**MonoBehaviour** ist die Basisklasse. Nur damit kann ein Skript an ein GameObject gehängt werden und Unity-Methoden benutzen.

## Wichtige Methoden
| Methode | Wann |
|---|---|
| `Awake()` | ganz am Anfang, noch vor Start |
| `Start()` | einmal, bevor der erste Frame läuft |
| `Update()` | jeden Frame — für Eingaben, Anzeige |
| `FixedUpdate()` | in festen Zeitabständen — für **Physik** |
| `OnEnable()` | jedes Mal, wenn das Objekt aktiviert wird |

Merksatz: **Eingaben in Update, Kräfte in FixedUpdate.**

## Variablen im Inspector
```csharp
public float speed = 5f;          // sichtbar im Inspector
[SerializeField] float health = 3; // sichtbar, aber trotzdem privat
private int score;                // nicht sichtbar
```
Der Vorteil: Werte lassen sich im Inspector ändern, ohne das Skript neu zu schreiben — auch während das Spiel läuft (Achtung: geht beim Stoppen verloren).

## Zeit
`Time.deltaTime` ist die Zeit seit dem letzten Frame. Damit rechnet man, wenn etwas pro Sekunde passieren soll:
```csharp
transform.position += Vector3.right * speed * Time.deltaTime;
```
Ohne `Time.deltaTime` würde das Spiel auf schnellen Rechnern schneller laufen.

## Testen
```csharp
Debug.Log("Punkte: " + score);
```
Ausgabe landet in der Console. Das einfachste Werkzeug zur Fehlersuche.

## Verwandt
- [[Objekte und Physik im Skript verwenden]]
- [[Bewegung]]
