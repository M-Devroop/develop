---
tags:
  - unity
  - 2d
  - movement
  - physik
---
---

Ziel: Der Spieler kann **nur springen, wenn er tatsächlich auf dem Boden steht** — nicht an Wänden, nicht an der Decke. Zusätzlich soll er an seitlichen Plattformkanten abrutschen statt zu kleben.

---

## Das Problem

Ein naiver GroundCheck über `OnCollisionEnter2D` setzt `isOnGround = true` bei **jeder** Kollision:

```csharp
// FALSCH — reagiert auch auf Wände und Decken
private void OnCollisionEnter2D(Collision2D collision)
{
    isOnGround = true;
}
```

Folge: Man kann an Wänden endlos hochspringen ("Wall Climbing"), weil eine seitliche Berührung als Boden zählt.

Dazu kommt das **Kleben an Wänden**: Unity-2D-Collider haben standardmäßig Friction `0.4`. Drückt man sich per `linearVelocity.x` gegen eine Wand, entsteht eine Normalkraft und die Reibung hält den Spieler fest.

---

## Lösung 1 — Physics Material gegen das Kleben

1. Project-Fenster → Rechtsklick → _Create_ → _2D_ → _Physics Material 2D_
2. Name: `NoFriction`
3. Inspector: **Friction = 0**, **Bounciness = 0**
4. Player → **Rigidbody2D** → Feld **Material** → `NoFriction` reinziehen

> [!tip] Warum Friction 0 unproblematisch ist Die horizontale Geschwindigkeit wird in `FixedUpdate` jeden Physikschritt hart gesetzt. Der Spieler bleibt beim Loslassen der Taste trotzdem sofort stehen — die Bodenreibung wird gar nicht gebraucht.

---

## Lösung 2 — GroundCheck per OverlapCircle

Der zustandslose Ansatz: Jeden Physikschritt neu prüfen, ob unter den Füßen ein Collider auf dem Ground-Layer liegt.

### Setup

**Layer anlegen**

- Inspector oben rechts → **Layer** → _Add Layer..._
- Erste freie Zeile (ab User Layer 6): `Ground`
- Alle Böden und Plattformen auswählen → **Layer** → `Ground`
- Bei einer Tilemap: Der Layer gehört auf das **Tilemap-GameObject**, nicht auf das Grid

**Marker erstellen**

- Rechtsklick auf **Player** → _Create Empty_
- Name: `GroundCheck`
- Position ca. `(0, -0.5, 0)` — knapp unter die Füße

> [!info] Kein Collider, kein Sprite, kein Script `GroundCheck` ist reiner Positionsmarker. Der Kreis wird per Code an dieser Stelle abgefragt.

### Code

```csharp
using UnityEngine;
using UnityEngine.InputSystem;

public class Player_Movement : MonoBehaviour
{
    private Rigidbody2D rb;

    public float speed = 5f;
    public float jumpForce = 8f;

    [Header("Ground Check")]
    public Transform groundCheck;
    public float groundCheckRadius = 0.15f;
    public LayerMask groundLayer;

    private bool isOnGround;
    private float inputHor;
    private bool jumpRequested;

    void Start()
    {
        rb = GetComponent<Rigidbody2D>();
    }

    void Update()
    {
        inputHor = 0f;
        if (Keyboard.current.aKey.IsPressed()) inputHor = -1f;
        if (Keyboard.current.dKey.IsPressed()) inputHor = 1f;

        if (Keyboard.current.spaceKey.wasPressedThisFrame && isOnGround)
            jumpRequested = true;
    }

    void FixedUpdate()
    {
        isOnGround = Physics2D.OverlapCircle(
            groundCheck.position, groundCheckRadius, groundLayer);

        rb.linearVelocity = new Vector2(inputHor * speed, rb.linearVelocity.y);

        if (jumpRequested)
        {
            rb.linearVelocity = new Vector2(rb.linearVelocity.x, jumpForce);
            jumpRequested = false;
        }
    }

    private void OnDrawGizmosSelected()
    {
        if (groundCheck != null)
        {
            Gizmos.color = Color.green;
            Gizmos.DrawWireSphere(groundCheck.position, groundCheckRadius);
        }
    }
}
```

### Inspector-Zuweisung

|Feld|Wert|
|---|---|
|Ground Check|`GroundCheck`-Objekt aus der Hierarchy reinziehen|
|Ground Check Radius|`0.15` (danach justieren)|
|Ground Layer|Dropdown → nur `Ground` anhaken|

> [!warning] Alle drei Felder müssen gesetzt sein Leeres `groundCheck` → NullReference. Ground Layer auf `Nothing` → Sprung funktioniert nie, ohne Fehlermeldung.

---

## Warum Input in Update, Sprung in FixedUpdate

`wasPressedThisFrame` ist nur genau **einen Frame lang** true und muss deshalb in `Update` abgefragt werden — in `FixedUpdate` würde der Tastendruck je nach Timing verschluckt.

Die eigentliche Geschwindigkeitsänderung gehört aber in `FixedUpdate`. Sonst überschreibt die Zeile

```csharp
rb.linearVelocity = new Vector2(inputHor * speed, rb.linearVelocity.y);
```

gelegentlich den gerade gesetzten `jumpForce`, bevor die Physik ihn verarbeiten konnte. Der Sprung fällt dann scheinbar zufällig aus.

**Muster:** Input sammeln in `Update` → als Flag merken → in `FixedUpdate` ausführen und Flag zurücksetzen.

---

## Radius justieren

Player in der Scene View auswählen → grüner Gizmo-Kreis wird sichtbar.

- Er muss ein Stück **unter** dem Spieler-Collider herausragen
- Er darf **nicht breiter** sein als der Spieler

> [!danger] Zu breiter Radius Ragt der Kreis seitlich über den Spieler hinaus, kann man in der Luft neben einer Plattform springen.

---

## Troubleshooting

> [!question]- Springen funktioniert gar nicht Der Reihe nach prüfen:
> 
> 1. **Ground Layer** im Inspector auf `Nothing`? → `Ground` anhaken
> 2. Plattformen noch auf Layer `Default`? → auf `Ground` setzen
> 3. Kreis zu klein oder falsch positioniert? → Radius testweise auf `0.3`
> 4. Kein Gizmo-Kreis sichtbar? → `groundCheck`-Feld ist leer
> 
> Diagnose: `Debug.Log("isOnGround: " + isOnGround);` in `FixedUpdate` nach der OverlapCircle-Zeile.

> [!question]- Man klebt weiterhin an Wänden Physics Material wurde nicht zugewiesen oder liegt auf dem Collider statt auf dem Rigidbody2D. Auf dem Rigidbody gilt es für alle Collider des Objekts.

> [!question]- Hängenbleiben an Tile-Kanten BoxCollider2D gegen einen vertikalen **CapsuleCollider2D** tauschen. Die runden Enden gleiten über kleine Unebenheiten zwischen benachbarten Tiles.

> [!question]- Spieler erkennt sich selbst als Boden Der Player darf nicht auf dem Layer `Ground` liegen.

---

## Alternativen

**Trigger-Collider als Child** Kleiner Collider mit `Is Trigger` unter den Füßen, `OnTriggerEnter2D`/`OnTriggerExit2D` mit Zähler (nicht bool — sonst bricht es an Kanten zwischen zwei Tiles). Vorteil: Man weiß, _welches_ Objekt berührt wird — nützlich für bewegliche Plattformen. Nachteil: Zustand muss korrekt gepflegt werden, `OnTriggerExit2D` feuert nicht zuverlässig beim Deaktivieren von Objekten.

**Kollisionsnormale auswerten** In `OnCollisionStay2D` über `collision.contacts` iterieren und auf `contact.normal.y > 0.5f` prüfen. Kommt ohne Marker-Objekt und Layer aus, ist aber umständlicher zu debuggen.

**`Physics2D.BoxCast` / `Raycast`** Präziser als der Kreis, besonders bei rechteckigen Spielern. Für den Einstieg ist `OverlapCircle` aber ausreichend.

---

## Verwandte Notizen

- [[Unity 2D - Rigidbody2D Grundlagen]]
- [[Unity - New Input System]]
- [[Unity 2D - Coyote Time und Jump Buffer]]