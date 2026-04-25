# KoLocal

&#x26AB; **Try it now &rarr; https://naklitechie.github.io/KoLocal/**

Play Go (Baduk/Weiqi) against an AI opponent &mdash; entirely in your browser. No install, no account, no ads, nothing uploaded anywhere.

---

## What it does

- Full Go rules on 9&times;9, 13&times;13, or 19&times;19 boards
- Three difficulty levels (Easy / Medium / Hard) backed by Monte Carlo Tree Search
- Liberties, capture, ko, suicide, and Japanese territory scoring &mdash; all implemented correctly
- Stone hover preview shows where you're about to place
- Last-move marker so you always know what the AI just did
- **Correspondence mode** &mdash; every move updates the URL. Copy the link, send it to a friend, play async. No server, no login &mdash; the URL is the save file.
- SGF export for game records
- Tap-to-move on mobile (no drag required)

## How to play

1. You are **Black** and move first. Tap an intersection to place a stone.
2. Surround opponent stones to capture them &mdash; a group is captured when all its adjacent empty points (liberties) are filled.
3. You can't play a move that leaves your own stone with zero liberties, unless it captures.
4. **Ko rule:** you can't immediately recapture a single stone that just captured yours.
5. Both players pass consecutively to end the game. Territory is then counted.
6. Score = your territory + your captures. White gets 7.5 points **komi** (compensation for going second).

## Tech

| Concern | Solution |
|---|---|
| Game logic | Pure vanilla JS &mdash; liberties, capture, ko, suicide, scoring |
| AI engine | Monte Carlo Tree Search with fast numeric playout board |
| Board rendering | Canvas with radial-gradient stones, DPI awareness |
| Correspondence | URL hash &mdash; compressed board state in fragment |
| Dependencies | **Zero** |
| Build step | **None** &mdash; open `index.html`, it works |

### Why the AI is fast

MCTS needs thousands of random game simulations (playouts) per move. Running those through the full rules engine &mdash; with DFS group searches, string-keyed Sets, and board hashing &mdash; was far too slow.

The fix: a separate `FastBoard` playout engine that uses `Int8Array` for the board, pre-allocated scratch buffers (no GC pressure), inline capture/suicide checks, and simple ko tracking via a single forbidden-point index. It shuffles empty intersections and tries random moves directly instead of pre-computing all legal moves.

Result: **~500ms for 2000 playouts on 9&times;9** &mdash; the full rules engine is only used for the real game and tree expansion, never during simulation.

## Palette

Coloured with **`japan-02 · 生成り KINARI`** — Edo merchant ledger, washi paper. Warm cream body, kaya-wood goban, classic black-and-white stones, Edo-navy action accent. The colour of a 19th-century Japanese paper-and-wood game room.

Palette pulled from [**Rangrez**](https://github.com/NakliTechie/rangrez), the global colour-palette library that backs all NakliTechie projects.

## Part of the NakliTechie series

Browser-native tools &mdash; no server, no API keys, no data leaving the device.

| Tool | What it does |
|------|--------------|
| [**BabelLocal**](https://github.com/NakliTechie/BabelLocal) | Offline translation &mdash; 200 languages, NLLB model |
| [**StripLocal**](https://github.com/NakliTechie/StripLocal) | EXIF metadata stripper &mdash; nothing leaves the browser |
| [**GambitLocal**](https://github.com/NakliTechie/GambitLocal) | Chess vs Stockfish &mdash; correspondence mode via URL |
| [**VoiceVault**](https://github.com/NakliTechie/VoiceVault) | Audio transcription &mdash; Whisper, offline-first |
| [**KingMe**](https://github.com/NakliTechie/KingMe) | English draughts &mdash; custom minimax AI, zero deps |
| **KoLocal** | Go (Baduk) &mdash; MCTS AI, zero deps |
| [**SnipLocal**](https://github.com/NakliTechie/SnipLocal) | Background remover &mdash; RMBG-1.4, passport mode |
| [**PredictionMarket**](https://github.com/NakliTechie/PredictionMarket) | Prediction market simulator &mdash; Parimutuel & LMSR |
| [**LocalMind**](https://github.com/NakliTechie/LocalMind) | Private AI chatbot &mdash; Gemma multimodal via WebGPU |

---

**Built by [Chirag Patnaik](https://github.com/NakliTechie)**
