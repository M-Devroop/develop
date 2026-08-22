---
tags: [unity, moc]
---

# Unity 2D — Wissensübersicht

Notizen aus dem Unity-Tutorial-Projekt **Sprite Flight**.

## Sprites
- [[2D Grundlagen]]
- [[Sprites Grundlagen]]
- [[Sprite Physik]]

## Skripts
- [[Grundlagen Skripts]]
- [[Objekte und Physik im Skript verwenden]]

## Maustracking
- [[Mausklick und Position tracken]]

## Movement
- [[Bewegung]]

## UI
- [[UI Grundlagen]]
- [[UI anwenden und sichtbar machen]]
- [[UI per Skript anpassen]]

## Effekte
- [[ParticleSystem Grundlagen]]
- [[ParticleSystem durch Skript steuern]]

---

## Roter Faden des Projekts
1. Sprite in die Szene setzen → **Sprite Renderer**
2. Physik hinzufügen → **Rigidbody 2D** + **Collider 2D**
3. Skript schreiben → **MonoBehaviour**, `Start` / `Update` / `FixedUpdate`
4. Maus auslesen → `ScreenToWorldPoint`
5. Bewegen → `AddForce`, `AddTorque`, Geschwindigkeit begrenzen
6. Zusammenstöße → `OnCollisionEnter2D`
7. Anzeige → **UIDocument** und `rootVisualElement`
8. Effekte → **Particle System** mit `Play()` und `Stop()`

## Offene Themen für später
- Animationen (Animator, Animation Clips)
- Sound (AudioSource)
- Szenenwechsel (SceneManager)
- Speichern von Punktzahlen
- Tilemaps
