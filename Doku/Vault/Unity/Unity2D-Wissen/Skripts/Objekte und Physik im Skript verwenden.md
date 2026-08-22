---
tags: [unity, csharp, physik]
---

# Objekte und Physik im Skript verwenden

## GetComponent
Damit holt sich ein Skript eine andere Component desselben GameObjects.

```csharp
using UnityEngine;

public class PlayerController : MonoBehaviour
{
    Rigidbody2D rb;

    void Start()
    {
        rb = GetComponent<Rigidbody2D>();
    }

    void FixedUpdate()
    {
        rb.AddForce(Vector2.up * 5f);
    }
}
```

**Wichtig:** `GetComponent` einmal in `Start()` oder `Awake()` aufrufen und in einer Variablen speichern. In `Update()` jeden Frame neu zu suchen ist langsam.

Weitere Varianten:
```csharp
GetComponentInChildren<ParticleSystem>();   // auch in Kind-Objekten suchen
anderesObjekt.GetComponent<SpriteRenderer>(); // an einem anderen Objekt
```

## Referenzen per Inspector
Oft einfacher als Suchen im Code: Feld anlegen und Objekt hineinziehen.
```csharp
[SerializeField] ParticleSystem explosion;
[SerializeField] Transform target;
```

## Kollisionen abfragen
Diese Methoden ruft Unity von selbst auf, wenn Collider sich berühren.

```csharp
void OnCollisionEnter2D(Collision2D collision)
{
    Debug.Log("Getroffen von: " + collision.gameObject.name);

    if (collision.gameObject.CompareTag("Asteroid"))
    {
        Destroy(collision.gameObject);
    }
}
```

| Methode | Wann |
|---|---|
| `OnCollisionEnter2D` | Berührung beginnt (echte Kollision) |
| `OnCollisionStay2D` | solange sie sich berühren |
| `OnCollisionExit2D` | Berührung endet |
| `OnTriggerEnter2D` | dasselbe, aber bei **Is Trigger** |

Trigger-Version bekommt `Collider2D` statt `Collision2D`:
```csharp
void OnTriggerEnter2D(Collider2D other)
{
    if (other.CompareTag("Pickup")) Destroy(other.gameObject);
}
```

## Tags
Tags setzt man oben im Inspector. Abfrage immer mit `CompareTag("Name")` — das ist schneller und sicherer als `tag == "Name"`.

## Nützliche Befehle
```csharp
Destroy(gameObject);            // Objekt löschen
Destroy(gameObject, 2f);        // nach 2 Sekunden löschen
gameObject.SetActive(false);    // ausblenden statt löschen
Instantiate(prefab, transform.position, Quaternion.identity); // erzeugen
```

## Verwandt
- [[Sprite Physik]]
- [[ParticleSystem durch Skript steuern]]
