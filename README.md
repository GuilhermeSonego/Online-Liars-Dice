# Online Liar's Dice

A multiplayer [Liar's Dice](https://en.wikipedia.org/wiki/Liar%27s_dice) game you play in the terminal. It uses a client–server design written in C++ over raw TCP sockets.

Built as a team project for a Computer Networks course (2025).

## Highlights

- **Custom binary protocol:** length-prefixed messages (`| size | type | payload |`) with hand-written, endianness-safe serialization ([`protocolo.cpp`](source/protocolo.cpp)).
- **Concurrent server:** one POSIX thread per player, with turns coordinated by semaphores.
- **Disconnect detection:** heartbeat messages and `poll()` let the server drop dead clients and keep the game going with the remaining players.
- **Server-side authority:** the server validates every bid and resolves challenges, so clients only render state.

## Build & Run

Requires Linux, `g++` with C++23, and `make`.

```bash
make all             # builds exec/servidor and exec/cliente
./exec/servidor      # start the server (port 4737, waits for 3 players)
./exec/cliente       # run once per player, in separate terminals
```

Server IP, port, and player count are compile-time constants in [`headers/cliente.h`](headers/cliente.h) and [`headers/server.h`](headers/server.h).

## How to Play

Each player starts with 3 hidden dice. On your turn you can:

1. **Raise:** bid that at least *N* dice show face *F* (typed as `NxF`, e.g. `3x5`).
2. **Challenge:** call the previous bid a lie. All dice are revealed, and whoever was wrong loses a die.
3. **Spot on:** claim the bid is *exactly* right. If you're correct, every other player loses a die. If not, you lose one.

The last player with dice left wins.

## Project Layout

```
headers/   protocol message types, game state, constants
source/    server.cpp · cliente.cpp · protocolo.cpp (serialization) · jogo.cpp (game rules)
docs/      technical report (Portuguese)
```

> Source code identifiers and in-game text are in Portuguese.

## Authors

Guilherme Sonego

Licensed under [Apache 2.0](LICENSE).
