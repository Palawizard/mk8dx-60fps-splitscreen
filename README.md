# Mario Kart 8 Deluxe 60 FPS split-screen cheat for version 4.0.1

Mario Kart 8 Deluxe runs at 60 FPS with one or two players, then drops to 30 FPS as soon as a third player joins. This cheat removes that limit, so 3 and 4 player split-screen races run at 60 FPS too.

The code is written for **version 4.0.1** only:

| Game | Title ID | Version | Build ID |
|---|---|---|---|
| Mario Kart 8 Deluxe | `0100152000022000` | 4.0.1 | `158C0C7399AABD7B` |

It uses the Atmosphère cheat format, which Eden, Citron, Sudachi and the other yuzu forks read, as do Ryujinx and Atmosphère on a modded Switch. It has been tested on Eden. It has not been tested on a real console.

## The code

```
[3-4 player 60 FPS]
04000000 00AFEE98 1400007F
```

The file is [`60fps-splitscreen/cheats/158C0C7399AABD7B.txt`](60fps-splitscreen/cheats/158C0C7399AABD7B.txt).

## Install

### Eden, Citron, Sudachi and other yuzu forks

1. Right-click Mario Kart 8 Deluxe in the game list and pick **Open Mod Data Location**.
2. Copy the `60fps-splitscreen` folder into it. The file should end up at `load/0100152000022000/60fps-splitscreen/cheats/158C0C7399AABD7B.txt`.
3. Right-click the game, open **Properties**, and check that `60fps-splitscreen` is ticked in the **Add-Ons** tab.
4. Start the game. Cheats are read at boot, so restart it if it was already running.

### Ryujinx

1. Right-click the game and pick **Open Mods Directory**.
2. Copy the `60fps-splitscreen` folder into it.
3. Right-click the game, pick **Manage Cheats** and enable `3-4 player 60 FPS`.
4. Restart the game.

### Atmosphère (Switch)

Copy the `cheats` folder from `60fps-splitscreen` to `/atmosphere/contents/0100152000022000/` on the SD card. The file should end up at `/atmosphere/contents/0100152000022000/cheats/158C0C7399AABD7B.txt`.

Atmosphère turns on every cheat in the file when the game starts. Hold L while launching the game to start it without cheats.

## Check your game version

The cheat does nothing on another version, because the file name has to match the build ID of the game's main executable.

- Eden and other yuzu forks write it to the log when the game starts: look for `name=main` in `eden_log.txt` (or `yuzu_log.txt`). If the log says `No cheats file found` next to `158C0C7399AABD7B`, the file is in the wrong folder.
- Ryujinx shows the build ID at the top of the **Manage Cheats** window.

For older versions of the game, [switch-cheats-db](https://github.com/HamletDuFromage/switch-cheats-db/blob/master/cheats/0100152000022000.json) has the `3-4 player 60 FPS` code for several earlier builds.

## Turn it off

Untick the add-on (yuzu forks), disable the cheat in Manage Cheats (Ryujinx), or delete `158C0C7399AABD7B.txt`. Nothing in the game files or the save is changed.

## How it works

The cheat writes one instruction in the game's main executable. At `main + 0xAFEE98`, the function that handles the 3-4 player frame rate compares a value with 2 and jumps to its end when they differ (`b.ne`, `54000FE1`). The cheat replaces that conditional jump with an unconditional one (`b`, `1400007F`), so the function always exits before the 30 FPS limit is applied.

This is the same patch as the earlier `3-4 player 60 FPS` codes. To port it to a later update, dump the decompressed main executable (in Eden: **Dump Decompressed NSOs** in the debug settings), search the `.text` section for `54000FE1` right after a compare with 2 (`cmp w8, #2` in 4.0.1), and use that offset in the code. The file name is the first 16 hex characters of the new build ID.

## Credits

The 60 FPS patch for Mario Kart 8 Deluxe was first made by [theboy181](https://linktr.ee/theboy181). It was ported to later updates by B3711, b0rd2dEAth and Craftyawesome, whose codes are collected in switch-cheats-db. This repository ports it to version 4.0.1.

## License

[MIT](LICENSE)
