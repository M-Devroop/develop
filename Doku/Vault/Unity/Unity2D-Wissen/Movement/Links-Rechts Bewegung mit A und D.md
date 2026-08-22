---
tags: [unity, 2d, movement, input]
---
---

# Links-Rechts Bewegung mit A und D (neues Input System)

Ziel: Objekt fährt nach links und rechts, keine Animation, kein Sprung.

## Voraussetzung

- Package **Input System** ist installiert (`Window > Package Manager`)
- `Edit > Project Settings > Player > Other Settings > Active Input Handling` steht auf **Input System Package (New)**
- Im Skript oben: `using UnityEngine.InputSystem;`

> Die alte Klasse `UnityEngine.Input` (z. B. `Input.GetAxisRaw`) funktioniert bei dieser Einstellung **nicht** und wirft eine `InvalidOperationException`.

---

## Variante A: Tasten direkt abfragen

Der schnellste Einstieg. Gut zum Lernen und für kleine Projekte.

### Keyboard.current

```csharp
if (Keyboard.current.aKey.isPressed) { }
```

**Was passiert hier?**

- `Keyboard` ist ein sogenanntes **Device** aus dem Input System.
- `Keyboard.current` ist die gerade aktive Tastatur. Ist keine angeschlossen, ist der Wert `null` — deshalb sicherheitshalber vorher prüfen.
- `.aKey` ist eine einzelne Taste. Genauso: `.dKey`, `.spaceKey`, `.leftArrowKey`, `.escapeKey`.
- `.isPressed` gibt `true` zurück, **solange** die Taste gehalten wird — also in jedem Frame neu.

Wichtige Abfragen an einer Taste:

|Eigenschaft|Wann `true`|
|---|---|
|`isPressed`|solange gedrückt gehalten|
|`wasPressedThisFrame`|nur im Frame des Drückens|
|`wasReleasedThisFrame`|nur im Frame des Loslassens|

Für dauerhafte Bewegung braucht man `isPressed`.

### Skript ohne Physik

```csharp
using UnityEngine;
using UnityEngine.InputSystem;

public class Player_Movement : MonoBehaviour
{
    [SerializeField] float speed = 5f;

    void Update()
    {
        float input = 0f;
        if (Keyboard.current == null) return;

        if (Keyboard.current.aKey.isPressed) input = -1f;
        if (Keyboard.current.dKey.isPressed) input =  1f;

        transform.position += new Vector3(input, 0f, 0f) * speed * Time.deltaTime;
    }
}
```

**Zeile für Zeile:**

- `[SerializeField] float speed = 5f;` — Variable bleibt privat, ist aber im Inspector einstellbar. So kann man das Tempo ausprobieren, ohne den Code zu ändern.
- `float input = 0f;` — Startwert 0 bedeutet „steht still". Wird nur überschrieben, wenn eine Taste gedrückt ist.
- `if (Keyboard.current == null) return;` — bricht die Methode ab, falls keine Tastatur da ist. Verhindert einen `NullReferenceException`-Fehler.
- Die beiden `if`-Zeilen setzen `input` auf `-1` (links) oder `1` (rechts). Die Zahl liefert damit gleichzeitig Richtung **und** Stärke und kann direkt mit `speed` multipliziert werden — man braucht kein `else`.
- `transform.position` — die aktuelle Position als `Vector3` (x, y, z).
- `new Vector3(input, 0f, 0f)` — Richtungsvektor. y und z bleiben 0, damit die Bewegung rein waagerecht ist.
- `+=` — rechnet den Vektor auf die alte Position drauf, das Objekt verschiebt sich also Frame für Frame ein Stück.
- `* Time.deltaTime` — **wichtig!** `Time.deltaTime` ist die Zeit in Sekunden seit dem letzten Frame (bei 60 FPS ca. 0,016). Dadurch wird aus „5 Einheiten pro Frame" ein „5 Einheiten pro Sekunde". Ohne diese Multiplikation läuft das Spiel auf einem schnellen Rechner schneller als auf einem langsamen.

### Skript mit Rigidbody 2D

Sobald Collider im Spiel sind, sollte man über die Physik bewegen:

```csharp
using UnityEngine;
using UnityEngine.InputSystem;

public class Player_Movement : MonoBehaviour
{
    [SerializeField] float speed = 5f;
    Rigidbody2D rb;
    float input;

    void Start()
    {
        rb = GetComponent<Rigidbody2D>();
    }

    void Update()   // Eingabe lesen
    {
        input = 0f;
        if (Keyboard.current == null) return;

        if (Keyboard.current.aKey.isPressed) input = -1f;
        if (Keyboard.current.dKey.isPressed) input =  1f;
    }

    void FixedUpdate()   // Physik anwenden
    {
        rb.linearVelocity = new Vector2(input * speed, rb.linearVelocity.y);
    }
}
```

**Neu darin:**

- `GetComponent<Rigidbody2D>()` — sucht am **selben GameObject** nach der Rigidbody-2D-Component. Nur einmal in `Start()` aufrufen und in `rb` merken, sonst wird jeden Frame neu gesucht (langsam).
- `rb.linearVelocity` — die aktuelle Geschwindigkeit als `Vector2` (x, y) in **Units pro Sekunde**. Setzt man sie direkt, bewegt sich das Objekt sofort mit genau diesem Tempo. Hier braucht man **kein** `Time.deltaTime`, weil „pro Sekunde" schon in der Einheit steckt.
- `new Vector2(input * speed, rb.linearVelocity.y)` — x wird neu gesetzt, y wird aus dem alten Wert **übernommen**. Sonst würde man beim Laufen die Schwerkraft ausschalten und das Objekt bliebe in der Luft hängen.
- **Warum zwei Methoden?** `Update()` läuft einmal pro Bild — dort werden Eingaben gelesen, damit kein Tastendruck verloren geht. `FixedUpdate()` läuft in festen Abständen (standardmäßig 50×/Sekunde) im Takt der Physik — dort gehört alles hin, was einen Rigidbody anfasst.

> In älteren Unity-Versionen heißt `linearVelocity` noch `velocity`.

---

## Variante B: Input Action im Skript

Der „richtige" Weg des neuen Input Systems. Vorteil: die Tastenbelegung steht im Inspector, nicht im Code. Damit kann man später Gamepad oder Pfeiltasten ergänzen, ohne das Skript anzufassen.

```csharp
using UnityEngine;
using UnityEngine.InputSystem;

public class Player_Movement : MonoBehaviour
{
    [SerializeField] float speed = 5f;
    [SerializeField] InputAction moveAction;   // im Inspector belegen

    void OnEnable()  => moveAction.Enable();
    void OnDisable() => moveAction.Disable();

    void Update()
    {
        float input = moveAction.ReadValue<float>();
        transform.position += new Vector3(input, 0f, 0f) * speed * Time.deltaTime;
    }
}
```

**Was passiert hier?**

- `InputAction` ist eine benannte Aktion wie „Bewegen" oder „Springen". Sie beschreibt **was** passieren soll, die zugehörigen Tasten (die **Bindings**) werden getrennt davon im Inspector festgelegt.
- `[SerializeField] InputAction moveAction;` — dadurch erscheint die Aktion im Inspector und kann dort mit Tasten belegt werden.
- `moveAction.Enable()` — eine Action ist am Anfang **abgeschaltet** und liefert sonst immer 0. Sie muss aktiviert werden. `OnEnable()` läuft, wenn das Objekt aktiviert wird, `OnDisable()` beim Deaktivieren — der passende Platz zum An- und Abmelden.
- `=>` ist nur eine Kurzschreibweise für eine Methode mit einer einzigen Zeile.
- `ReadValue<float>()` — liest den aktuellen Wert der Aktion aus. In den spitzen Klammern steht der erwartete Datentyp: `float` für eine Achse (-1 bis 1), `Vector2` für Stick-/WASD-Eingabe, `bool` für Knöpfe.

**Bindings im Inspector einrichten:**

1. Skript auf das GameObject ziehen, im Inspector das Feld `Move Action` aufklappen
2. Bei **Action Type** `Value`, bei **Control Type** `Axis` wählen
3. Unter Bindings `+` → **Add 1D Axis Composite**
4. Bei **Negative** die Taste `A` eintragen, bei **Positive** die Taste `D` (über den _Listen_-Knopf einfach die Taste drücken)

Ergebnis: A liefert `-1`, D liefert `1`, nichts gedrückt `0` — genau wie in Variante A, nur ohne feste Tasten im Code.

---

## Häufige Stolpersteine

- **`InvalidOperationException`** → im Code wird noch die alte `Input`-Klasse benutzt, oder `using UnityEngine.InputSystem;` fehlt
- **Nichts bewegt sich (Variante B)** → `Enable()` vergessen, oder es ist kein Binding eingetragen
- **`NullReferenceException` bei `Keyboard.current`** → Abfrage auf `null` fehlt
- **Nichts bewegt sich (Variante A)** → Skript nicht auf das GameObject gezogen, oder `speed` steht auf 0
- **Objekt kippt um** → im Rigidbody 2D unter **Constraints** `Freeze Rotation Z` anhaken
- **Tempo je nach Rechner unterschiedlich** → `Time.deltaTime` vergessen

---

## Optional: Sprite spiegeln

```csharp
if (input != 0)
{
    GetComponent<SpriteRenderer>().flipX = input < 0;
}
```

**Erklärung:** `flipX` ist ein `bool` im Sprite Renderer, der das Bild waagerecht spiegelt. Der Ausdruck `input < 0` ergibt selbst schon `true` oder `false` — ein `if/else` ist also unnötig. Die äußere Abfrage `input != 0` sorgt dafür, dass die Blickrichtung erhalten bleibt, wenn man stehen bleibt.

## Verwandt

- [[Bewegung]]
- [[Grundlagen Skripts]]