# PPSSPP QA fork

Upstream PPSSPP plus one headless flag, for scripted test runs driven over the WebSocket debugger (by a harness such
as the translate-retro-game skill's `psp.py`). Upstream behaviour is unchanged unless the flag is used.

## Headless flag

| Flag | Effect |
|---|---|
| `--ppge` | Draw PPGe, the PSP system dialogs (save/load prompts, messages), so screenshots show their text. Stock headless skips PPGe's atlas and draws the boxes only. Needs `assets/` (with `ppge_atlas.zim`, `ppge_atlas.meta`, `flash0/`) beside the exe or up to 5 folders above it. |

Upstream already gives a scripted run what it needs: `PPSSPPHeadless --debugger=PORT --memstick=DIR GAME.iso` opens
no window, stops at the entry point, and the client drives it with `cpu.runUntilTime` (run to an emulated time and
stop), `input.buttons.send`, `gpu.buffer.screenshot`, `game.state.load/save {path, wait: true}`.

## Build (Windows, Visual Studio 2022 or later)

```
git submodule update --init --recursive
msbuild Windows/PPSSPP.sln /t:PPSSPPHeadless /p:Configuration=Release /p:Platform=x64
```

Ship `Windows/x64/Release/PPSSPPHeadless.exe` with `assets/flash0`, `assets/ppge_atlas.zim` and
`assets/ppge_atlas.meta` in an `assets/` folder beside it.

License: GPL-2.0-or-later, as upstream.
