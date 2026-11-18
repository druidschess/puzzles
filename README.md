# druidschess puzzles

Chess puzzle data for the druidschess app. The files are attached to the
[releases](https://github.com/druidschess/puzzles/releases) of this
repository. The repository itself holds only this description.

## Source

Every puzzle comes from the [Lichess puzzle database](https://database.lichess.org/#puzzles).
Lichess made the puzzles from Lichess games with Stockfish, and rates them
with Glicko-2 from players' attempts.

Release `v2026.11.01-1` is built from `lichess_db_puzzle.csv.zst` of
2026-11-01:

- 6,193,530 puzzles, 309,751,173 bytes
- SHA-256 `7b0d235fa40cfd7fdb7df1fec56a96b3b6ee1496b32ed2eb05be90d0ebf94253`

## Licence

Lichess releases its database exports under
[CC0 1.0 Universal](https://creativecommons.org/publicdomain/zero/1.0/).
The files here are released under CC0 1.0 too. You may use them for any
purpose, with no conditions.

## The set

The files hold the **90/1k set**: puzzles with a popularity of 90 or more
and 1,000 or more plays. The source has 1,737,318 of them. The daily
schedule also holds the Lichess daily puzzles that are outside the set.

| File | Contents |
|---|---|
| `puzzles-bundle.csv.zst` | 100,000 puzzles that the app ships |
| `puzzles-more.csv.zst` | The rest of the set (1,631,805 puzzles), for an optional download in the app |
| `puzzles-daily.csv.zst` | One puzzle per date from 2020-12-17 to 2036-12-31 (5,859 dates) |
| `SHA256SUMS` | The SHA-256 of each file |

No puzzle is in more than one file.

**The bundle** has five rating groups:

| Rating | Puzzles |
|---|---|
| Below 1000 | 10,000 |
| 1000–1499 | 10,000 |
| 1500–1999 | 35,000 |
| 2000–2499 | 35,000 |
| 2500 and above | 10,000 |

Half of each group comes from games with a titled player (the `master`
theme). The sample keeps the spread of popularity and plays, gives rare
motifs more weight, and has at least 500 mates in one below rating 700.

**The daily schedule** gives each Lichess daily puzzle the UTC date that
Lichess showed it. On the 33 dates that had two puzzles, the second puzzle
moves to the nearest empty date. Every other date gets a puzzle from the
set, with the same rating spread as the Lichess daily puzzles. No puzzle
appears twice.

## Format

Each file is CSV compressed with zstd, with a header row. No field is
quoted, and no field holds a comma.

The bundle and the download file have seven columns, sorted by `PuzzleId`:

| Column | Meaning |
|---|---|
| `PuzzleId` | The Lichess puzzle id (`https://lichess.org/training/<id>`) |
| `FEN` | The position before the opponent's move |
| `Moves` | Moves in UCI, separated by spaces. The first move is the opponent's. The solution starts at the second move. |
| `Rating` | The Lichess puzzle rating on the source date |
| `RatingDeviation` | Its Glicko-2 rating deviation |
| `Themes` | Lichess theme tags, separated by spaces |
| `OpeningTags` | Lichess opening tags, separated by spaces. Lichess sets them only for puzzles that start before move 20. |

The daily schedule has a `Date` column (`YYYY-MM-DD`) first, then the same
seven columns, sorted by date.

## How the files are built

A build tool in the druidschess app repository makes the files from the
source file. It checks the source against the SHA-256 above. With the same
source and seed, it gives the same files byte for byte.
