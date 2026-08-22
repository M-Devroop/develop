---
tags: [unity, ui, uitoolkit, csharp]
---

# Anpassen des UI durch ein Skript (UIDocument, rootVisualElement)

## Zugriff auf das UI
Das Skript kommt auf dasselbe GameObject wie die **UIDocument**-Component.

```csharp
using UnityEngine;
using UnityEngine.UIElements;   // wichtig!

public class HUD : MonoBehaviour
{
    Label scoreLabel;
    Button restartButton;
    VisualElement gameOverPanel;

    void OnEnable()
    {
        VisualElement root = GetComponent<UIDocument>().rootVisualElement;

        scoreLabel     = root.Q<Label>("ScoreLabel");
        restartButton  = root.Q<Button>("RestartButton");
        gameOverPanel  = root.Q<VisualElement>("GameOverPanel");

        restartButton.clicked += NeuStarten;
    }
}
```

**rootVisualElement** ist das oberste Element des UXML — von dort aus sucht man alles Weitere.

> Den Zugriff in `OnEnable()` machen, nicht in `Start()`. Das UI wird erst beim Aktivieren aufgebaut; in `Start()` kann es noch leer sein.

## Elemente finden mit Q
```csharp
root.Q<Label>("ScoreLabel");   // nach Typ und Name
root.Q<Button>("Start");       // Name aus dem UI Builder
root.Query<Button>().ToList(); // alle Buttons auf einmal
```
Findet Unity nichts, ist das Ergebnis `null` → häufigster Fehler: Name im UI Builder anders geschrieben.

## Werte ändern
```csharp
scoreLabel.text = "Punkte: " + score;
progressBar.value = leben;
```

## Sichtbarkeit umschalten
```csharp
gameOverPanel.style.display = DisplayStyle.None;  // ausblenden
gameOverPanel.style.display = DisplayStyle.Flex;  // einblenden
```

## Aussehen ändern
```csharp
scoreLabel.style.color = Color.red;
scoreLabel.style.fontSize = 32;
element.AddToClassList("highlight");     // USS-Klasse hinzufügen
element.RemoveFromClassList("highlight");
```
Sauberer ist es, das Aussehen in USS zu definieren und im Skript nur die Klasse zu wechseln.

## Buttons
```csharp
restartButton.clicked += NeuStarten;   // anmelden

void OnDisable()
{
    restartButton.clicked -= NeuStarten; // wieder abmelden
}

void NeuStarten()
{
    Debug.Log("Neu gestartet");
}
```

## Merken
- `using UnityEngine.UIElements;` nicht vergessen
- Zugriff in `OnEnable()`
- Namen exakt wie im UI Builder schreiben (Groß-/Kleinschreibung zählt)

## Verwandt
- [[UI anwenden und sichtbar machen]]
- [[Grundlagen Skripts]]
