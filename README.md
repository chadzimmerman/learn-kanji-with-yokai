# Learn Kanji with Yokai! — iOS App

> **Note:** Source code only. App Store link coming after beta.

---

## Overview

An iOS kanji writing game built as a companion to the published book *Learn Kanji with Yokai!* by Chad and Brittany Zimmerman. Players learn to write Japanese kanji by tracing characters in a scaffold phase, then recalling them from memory against a countdown timer. Mastered kanji unlock illustrated Yokai collector cards drawn by the co-author.

---

## Features

- **Calligraphy canvas** — Custom `UIView` drawing engine with velocity-based stroke width using Catmull-Rom spline interpolation, simulating an ink brush aesthetic
- **Two-phase learning loop** — Scaffold (trace the ghost kanji to build muscle memory) → Recall (write from memory with the Heisig English meaning as the only prompt)
- **Anti-cheat stroke validation** — Submissions are rejected if the drawn stroke count deviates from the expected count by more than ±1, preventing blob-fill exploits
- **Template-matching recognition** — Pixel-level image comparison via `KanjiRecognizer` evaluates drawing accuracy against pre-rendered reference glyphs
- **Hint system** — Players can summon the ghost kanji during recall. A correct submission with the hint active clears the canvas but does not advance — they must pass clean
- **SRS level system** — 10 kanji per level. Pass all 10 → kanji banked, level advances. Timer hits zero → retry same level. Inspired by Anki-style spaced repetition
- **Review mode** — Shuffled drill of every kanji in the player's bank; no pass/fail, plays until timer expires or all cards cleared
- **Yokai card collection** — Completing the kanji set for a given Yokai triggers an animated card reveal with fireworks. Cards persist via `AppStorage`
- **Onboarding tutorial** — First-launch walkthrough with a live mini-game and animated card reveal sequence
- **Heisig component hints** — Each card optionally shows component radicals (e.g., "person + cow") to aid memory formation

---

## Tech Stack

| Layer | Technology |
|---|---|
| Language | Swift 5.9 |
| UI Framework | SwiftUI + UIKit interop |
| Drawing Engine | Custom `UIViewRepresentable` canvas |
| Character Recognition | Pixel-based template matching |
| Persistence | `AppStorage` / `UserDefaults` |
| Architecture | `ObservableObject` MVVM |
| Platform | iOS 17+ |

---

## Architecture

```
AppState (ObservableObject)
├── screen: AppScreen          — drives top-level navigation (RootView)
├── currentLevel: Int          — persisted, drives deck slice
├── kanjiBank: Set<String>     — persisted, mastered kanji for Review mode
└── earnedCardIds: [String]    — persisted Yokai collection

GameViewModel (ObservableObject)
├── mode: GameMode             — .newKanji | .review
├── deck: [KanjiCard]          — current level slice or shuffled bank
├── gameState: GameState       — .playing | .recognizing | .correct | .wrong | .gameOver | .levelComplete
├── submitDrawing(...)         — dispatches recognition to background queue
└── callbacks: onLevelPassed, onYokaiCardEarned

KanjiDeck
├── level(_ index:) → [KanjiCard]   — 10-card slice
├── reviewDeck(bank:) → [KanjiCard] — shuffled bank subset
└── 188 cards with strokeCount and optional Heisig components
```

---

## Content

- 188 kanji from the *Learn Kanji with Yokai!* book, ordered from simplest to most complex
- Each kanji includes: character, Heisig English meaning, exact stroke count, optional component radicals
- Yokai cards tied to kanji completion sets (e.g., mastering 一、二、三 unlocks a Yokai card)

---

## Monetization (Planned)

- Free tier: base kanji deck
- One-time purchase: $9.99
- Monthly subscription: $2.99/month
- Future: additional kanji decks, art style packs, custom card creation

---

## Status

Currently in development / internal beta. App Store link forthcoming.

The book: [Learn Kanji with Yokai!](https://a.co/d/0d28CJNn) *(published)*

---

*Built with Swift + SwiftUI. Art by Svetlana Zimmerman.*
