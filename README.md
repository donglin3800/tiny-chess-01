# tiny-chess-01

[![make.fun](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/competitions/tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/donglin3800/tiny-chess-01/rank.svg)](https://make.fun/competitions/tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/donglin3800/tiny-chess-01/reward.svg)](https://make.fun/competitions/tiny-chess-01)

A 4 KB Python chess engine for [make.fun's tiny-chess-01](https://make.fun/competitions/tiny-chess-01): the strongest engine in 4,096 bytes of Python, rated against a fixed gauntlet of house engines.

## The engine

`main.py` is the whole engine: Python 3.13, standard library only, no model calls, no clock, no unseeded randomness. It talks to the env at `MAKEFUN_ENV_URL` (`/game`, `/move`, `/resign`) and plays every game to the end.

- 0x88 board, incremental material + piece-square evaluation (tapered king table, bishop pair), Zobrist hashing
- Negamax alpha-beta with a transposition table, null-move pruning, killer moves, a history heuristic and MVV-LVA capture ordering
- Quiescence over captures, promotions and check evasions; repetition and fifty-move draws score 0
- Iterative deepening to a fixed node budget (60,000 nodes a move), so every trial replays move for move

## Size

`main.py` as committed is the `makefun compress` packing of the readable source (LZMA payload plus unpacker), at most 4,096 bytes; the whole repository is at most 8,192 bytes.

## Grade it locally

From the competition's dev kit, with the env server and scorer:

    node program/env/server.mjs   # MAKEFUN_ENV_DATA_DIR=<trial>/input/env
    python3 -P -s main.py         # MAKEFUN_ENV_URL=http://127.0.0.1:<port>
    node program/scorer/score.mjs # MAKEFUN_OUTPUT_DIR=<state>/export
