---
tags: [unity, input, maus]
---

# Mausklick und Position tracken und anwenden

## Mausklick abfragen
```csharp
void Update()
{
    if (Input.GetMouseButtonDown(0)) { /* einmal beim Drücken */ }
    if (Input.GetMouseButton(0))     { /* solange gedrückt */ }
    if (Input.GetMouseButtonUp(0))   { /* beim Loslassen */ }
}
```
`0` = linke Taste, `1` = rechte, `2` = mittlere.

> Mit dem neuen Input System heißt es stattdessen `Mouse.current.leftButton.wasPressedThisFrame` und `Mouse.current.position.ReadValue()`.

## Das Problem: Bildschirm vs. Welt
`Input.mousePosition` liefert **Pixel** (0,0 unten links). Die Objekte in der Szene liegen aber in **Units**. Beides muss umgerechnet werden:

```csharp
Vector3 mausWelt = Camera.main.ScreenToWorldPoint(Input.mousePosition);
mausWelt.z = 0f;   // in 2D wichtig, sonst liegt der Punkt bei der Kamera
```

## Richtung zum Mauszeiger berechnen
```csharp
Vector2 richtung = (Vector2)mausWelt - (Vector2)transform.position;
Vector2 richtungNormiert = richtung.normalized; // Länge = 1, nur die Richtung
float abstand = richtung.magnitude;             // Entfernung
```

`normalized` ist wichtig, damit die Kraft immer gleich stark ist, egal wie weit die Maus weg ist.

## Anwenden: zum Klick fliegen
```csharp
public class ClickMove : MonoBehaviour
{
    [SerializeField] float force = 10f;
    Rigidbody2D rb;
    Vector2 ziel;
    bool hatZiel;

    void Start() => rb = GetComponent<Rigidbody2D>();

    void Update()
    {
        if (Input.GetMouseButtonDown(0))
        {
            Vector3 welt = Camera.main.ScreenToWorldPoint(Input.mousePosition);
            ziel = welt;
            hatZiel = true;
        }
    }

    void FixedUpdate()
    {
        if (!hatZiel) return;
        Vector2 richtung = (ziel - rb.position).normalized;
        rb.AddForce(richtung * force);
    }
}
```

## Objekt zur Maus ausrichten
```csharp
float winkel = Mathf.Atan2(richtung.y, richtung.x) * Mathf.Rad2Deg;
transform.rotation = Quaternion.Euler(0, 0, winkel - 90f); // -90, wenn das Sprite nach oben zeigt
```

## Merken
- `Camera.main` sucht die Kamera mit dem Tag `MainCamera` — am besten einmal in `Start()` speichern.
- Klicks in `Update()` abfragen, nicht in `FixedUpdate()` (sonst gehen Klicks verloren).

## Verwandt
- [[Bewegung]]
