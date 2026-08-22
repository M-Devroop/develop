---
tags: [unity, 2d, movement, physik]
---

# Bewegung (Size, Force, Torque, max. Geschwindigkeit)

## Zwei Wege, etwas zu bewegen
1. **Über den Transform** — direkt die Position setzen. Einfach, ignoriert aber die Physik (Objekte können durch Wände rutschen).
```csharp
transform.position += Vector3.right * speed * Time.deltaTime;
transform.Translate(Vector3.up * speed * Time.deltaTime);
```
2. **Über den Rigidbody 2D** — mit Kräften. Fühlt sich träge und "echt" an, Kollisionen funktionieren richtig. Immer in `FixedUpdate()`.

## Force (Schub)
```csharp
rb.AddForce(richtung * force);                      // dauerhafter Schub
rb.AddForce(richtung * force, ForceMode2D.Impulse); // einmaliger Stoß
```
| ForceMode2D | Bedeutung |
|---|---|
| `Force` | wirkt über Zeit, langsam beschleunigend (Standard) |
| `Impulse` | sofortiger Schlag, z. B. Sprung oder Schuss |

Die Masse im Rigidbody beeinflusst, wie stark die Kraft wirkt.

## Torque (Drehung)
```csharp
rb.AddTorque(1f);    // dreht gegen den Uhrzeigersinn
rb.AddTorque(-1f);   // dreht mit dem Uhrzeigersinn
```
In 2D gibt es nur eine Drehachse (Z), deshalb reicht eine einzelne Zahl.
Gebremst wird die Drehung über **Angular Damping** im Rigidbody.

## Maximale Geschwindigkeit begrenzen
Kräfte beschleunigen sonst endlos. Deshalb nach dem Beschleunigen abschneiden:

```csharp
[SerializeField] float maxSpeed = 8f;

void FixedUpdate()
{
    rb.AddForce(richtung * force);

    if (rb.linearVelocity.magnitude > maxSpeed)
    {
        rb.linearVelocity = rb.linearVelocity.normalized * maxSpeed;
    }
}
```
Kürzer geht auch:
```csharp
rb.linearVelocity = Vector2.ClampMagnitude(rb.linearVelocity, maxSpeed);
```

> In älteren Unity-Versionen heißt `linearVelocity` noch `velocity`. Gleiches Prinzip.

Für die Drehung analog:
```csharp
rb.angularVelocity = Mathf.Clamp(rb.angularVelocity, -maxTurn, maxTurn);
```

## Size (Größe)
Die Größe steckt im Transform unter **Scale**:
```csharp
transform.localScale = new Vector3(2f, 2f, 1f);      // doppelt so groß
transform.localScale *= 1.1f;                        // 10 % größer
```
Wichtig:
- Der **Collider skaliert mit**, die Physik passt also.
- In 2D die Z-Skalierung bei 1 lassen.
- Weich wachsen lassen mit `Vector3.Lerp`:
```csharp
transform.localScale = Vector3.Lerp(transform.localScale, zielGroesse, Time.deltaTime * 2f);
```

## Typischer Bewegungsablauf
1. In `Update()` Eingabe lesen (Tasten oder [[Mausklick und Position tracken|Mausklick]])
2. In `FixedUpdate()` `AddForce` / `AddTorque` anwenden
3. Danach die Geschwindigkeit begrenzen
4. Damping sorgt dafür, dass das Objekt wieder langsamer wird

## Verwandt
- [[Sprite Physik]]
- [[Grundlagen Skripts]]
