---
tags: [unity, effekte, partikel, csharp]
---

# ParticleSystem durch Skript anzeigen (Play(), Stop())

## Vorbereitung
Im Inspector **Play On Awake ausschalten**, sonst startet der Effekt sofort beim Spielstart.

## Referenz holen
```csharp
using UnityEngine;

public class Thruster : MonoBehaviour
{
    [SerializeField] ParticleSystem flamme;   // im Inspector hineinziehen

    void Start()
    {
        // Alternativen:
        // flamme = GetComponent<ParticleSystem>();
        // flamme = GetComponentInChildren<ParticleSystem>();
    }
}
```
Wenn das Particle System ein Kind-Objekt ist, braucht man `GetComponentInChildren`.

## Starten und Stoppen
```csharp
flamme.Play();   // startet den Effekt
flamme.Stop();   // stoppt das Ausstoßen, vorhandene Partikel fliegen aus
flamme.Clear();  // löscht alle Partikel sofort
flamme.Pause();
```

## Beispiel: Flamme nur beim Schub
```csharp
void Update()
{
    if (Input.GetMouseButtonDown(0) && !flamme.isPlaying)
    {
        flamme.Play();
    }

    if (Input.GetMouseButtonUp(0))
    {
        flamme.Stop();
    }
}
```
`isPlaying` verhindert, dass der Effekt jeden Frame neu gestartet wird.

## Beispiel: Explosion bei Kollision
```csharp
[SerializeField] ParticleSystem explosion;

void OnCollisionEnter2D(Collision2D collision)
{
    if (collision.gameObject.CompareTag("Asteroid"))
    {
        explosion.Play();
    }
}
```
Damit die Explosion nicht mit dem Objekt verschwindet, wenn dieses zerstört wird:
- **Simulation Space** auf `World` stellen, **oder**
- den Effekt als eigenes Prefab mit `Instantiate` erzeugen und danach automatisch löschen lassen (`Stop Action = Destroy` im Main-Modul).

## Nützlich
```csharp
flamme.isPlaying;    // läuft gerade?
flamme.Emit(20);     // 20 Partikel sofort ausstoßen
```

Werte im Skript ändern geht über die Module:
```csharp
var main = flamme.main;
main.startSpeed = 8f;
```

## Verwandt
- [[ParticleSystem Grundlagen]]
- [[Objekte und Physik im Skript verwenden]]
