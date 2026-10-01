# GT4 Flare Performance Patch for PCSX2

Toggleable patches that reduce the cost of light flares in **Gran Turismo 4 (US, SCUS-97328)** on PCSX2, mainly on evening and night tracks.

> **Status: untested.** Every patch address and original instruction was checked against a decompressed copy of the game's `CORE.GT4`, but the patches have not been run in PCSX2. Try one section at a time and keep a backup of your save. If something misbehaves, turn the patch off and the game returns to normal.

---

## What is in this package

| File | What it is |
|------|------------|
| `SCUS-97328_77E61C8A.pnach` | The patch file. Contains all the toggles. |
| `README.md` | This guide. |

## Before you start: check that it fits your game

This patch is for the **US release, serial SCUS-97328**, with the `CORE.GT4` file it was checked against. It will **not** work on other regions, on Spec II / Online Beta (SCUS-97436), or on a modified game.

1. Start GT4 in PCSX2 once.
2. Open the log (**Tools → Show Log**, or **Settings → Advanced → enable Log Window**) or look at the game list entry.
3. Find the game's **serial** and **CRC**. You need `SCUS-97328` and an 8-character CRC.
4. If your CRC is **`77E61C8A`**, the file name is already correct. If it is **different**, rename the `.pnach` file to `SCUS-97328_<YOUR CRC>.pnach` (uppercase, no spaces). The patch is tied to the addresses in the US `CORE.GT4`, so only do this if you are sure your disc is the standard US GT4.

---

## Step 1: Install the patch (pick ONE method)

### Method A: the `cheats` folder (recommended)

Easiest, and PCSX2 updates will not delete it.

1. In PCSX2, choose **Tools → Open Data Directory**.
2. Open the folder named **`cheats`**. Create it if it does not exist.
3. Copy `SCUS-97328_77E61C8A.pnach` into it.
4. Restart PCSX2.

In this method the toggles appear on the **Cheats** tab (not Patches), and **Enable Cheats** must be turned on (see Step 2).

### Method B: inside `patches.zip`

1. Open **Tools → Open Data Directory**, then the **`resources`** folder.
2. Open `patches.zip` with 7-Zip or similar.
3. Drag the `.pnach` file into the **root** of the zip (not inside a subfolder).
4. If the zip already has a `SCUS-97328_...pnach` file, do not overwrite it. Copy the sections from this file into the existing one instead.
5. Restart PCSX2.

In this method the toggles appear on the **Patches** tab. Note that a PCSX2 update can replace `patches.zip` and remove your file, so keep a copy.

### Do not mix

Do not use this file together with `FlarePerformance.pnach`, `HalfFlares.pnach`, `DisableSunFlare.pnach` or `ToggleCarLights.pnach`. Remove those first. Do not install the same file in both places.

---

## Step 2: Turn the toggles on (easy way, in the UI)

1. In the PCSX2 game list, right-click **Gran Turismo 4** and choose **Properties**.
2. Open the **Patches** tab (Method B) or the **Cheats** tab (Method A).
3. For Method A only: tick **Enable Cheats** on that tab, or under **Settings → Advanced**.
4. Tick the toggles you want (see the presets below).
5. Close the window and start the game.

If you do not see the toggles, go to [Troubleshooting](#troubleshooting).

---

## Recommended presets

Start with the first one. Move down only if you still need more speed.

**Balanced (all effects still drawn)**
- Flare Cap 16
- Flare Distance Cull 60
- Flare Glow Medium
- Sun Flare Half Size

**Light (stronger reduction)**
- Flare Cap 8
- Flare Distance Cull 40
- Flare Glow Small
- Flare Halo Pass Off
- Sun Flare Quarter Size

**Maximum speed (removes the effects)**
- Lamp Flares Off (Max Speed)
- Streak Lights Off
- Sun Flare Off

---

## Toggle reference

### Groups: enable at most ONE from each group

Toggles in a group write to the same memory addresses. If you enable two, only the last one loaded takes effect.

**Flare Cap:** the maximum number of lamp and track-light flares queued per frame. The game default is 100.

| Toggle | Effect |
|--------|--------|
| Flare Cap 4 | Fewest flares |
| Flare Cap 6 | |
| Flare Cap 8 | |
| Flare Cap 12 | |
| Flare Cap 16 | |
| Flare Cap 20 | |
| Flare Cap 24 | |
| Flare Cap 32 | Most flares of the set |

**Flare Distance Cull:** skips flares farther than this many units from the camera. A higher number means more flares are drawn, which is more taxing. The units are the game's internal camera depth, not meters.

| Toggle | Effect |
|--------|--------|
| Flare Distance Cull 25 | Only very close lights |
| Flare Distance Cull 40 | |
| Flare Distance Cull 60 | |
| Flare Distance Cull 80 | |
| Flare Distance Cull 100 | Far lights still drawn |

**Flare Glow:** draws every lamp and track-light glow smaller, which reduces GPU overdraw.

| Toggle | Width and height | Approx. screen area |
|--------|------------------|---------------------|
| Flare Glow Large | 75% | 56% |
| Flare Glow Medium | 50% | 25% |
| Flare Glow Small | 25% | 6% |
| Flare Glow Tiny | 12.5% | 1.6% |

**Sun Flare:**

| Toggle | Effect |
|--------|--------|
| Sun Flare Half Size | Sun flare at 50% width and height |
| Sun Flare Quarter Size | 25% |
| Sun Flare Eighth Size | 12.5% |
| Sun Flare Off | Removes the sun flare completely |

### Independent toggles (can be combined with anything)

| Toggle | Effect |
|--------|--------|
| Flare Halo Pass Off | Removes the second, larger halo sprite that close lights draw |
| Streak Lights Off | Turns off the second flare draw function (streak-type lights). This removes that effect completely |

### Use on its own

| Toggle | Effect |
|--------|--------|
| Lamp Flares Off (Max Speed) | Stops all lamp and track-light flare drawing. Largest speedup, but lights lose their glow. Makes the cap, cull, glow and halo toggles pointless |

---

## Alternative: edit the game settings file by hand

Use this only if you prefer editing text. **The exact syntax comes from memory of how PCSX2 writes its files, so confirm it as described below.**

1. Close PCSX2 completely.
2. Open **Tools → Open Data Directory**, then the **`gamesettings`** folder.
3. Open `SCUS-97328_77E61C8A.ini` in a text editor. If it does not exist, open the game's Properties once, change any setting, and close the window so PCSX2 creates it.
4. Add the toggles under a `[Patches]` heading, one `Enable =` line each. The text must match the toggle name exactly:

```
[Patches]
Enable = Flare Cap 16
Enable = Flare Distance Cull 60
Enable = Flare Glow Medium
Enable = Sun Flare Half Size
```

If you used Method A (the `cheats` folder), the heading is `[Cheats]` instead, and you also need this:

```
[EmuCore]
EnableCheats = true
```

To turn a toggle off, delete its line.

**To confirm the syntax on your version:** tick one or two toggles in the UI, close the window, and open the ini file. What PCSX2 wrote there is the format to copy.

---

## Testing it properly

1. Load a night or evening race, ideally a city course with many lights.
2. Turn on the performance overlay (**Settings → Advanced → On-Screen Display**, enable FPS, CPU and GPU usage).
3. Drive the same stretch with no toggles on, and note the FPS.
4. Turn on **one** toggle, drive the same stretch again, and compare.
5. Add more toggles one at a time until it is fast enough.

If the overlay shows no change however many toggles you enable, the slowdown is not caused by the flares. Look at your PCSX2 settings instead (below).

## PCSX2 settings that matter more than this patch

Flares are many small blended sprites, and PCSX2's blending emulation is expensive. If you are still short of full speed:

- Set **Blending Accuracy** to **Basic** or **Medium** (Settings → Graphics → Advanced).
- Use the **Vulkan** or **Direct3D 12** renderer.
- Lower the **Internal Resolution** by one step.
- Turn on **MTVU**.

---

## Troubleshooting

**I do not see any toggles.**
- Check the file name: `SCUS-97328_` then your CRC in uppercase, ending in `.pnach`. The CRC must match what the PCSX2 log shows.
- Method A: make sure the file is in the `cheats` folder and look on the **Cheats** tab, with Enable Cheats on.
- Method B: make sure the file is in the **root** of `patches.zip` and look on the **Patches** tab.
- Restart PCSX2 after adding the file.

**A toggle is on but nothing changes.**
- Check that you did not enable two toggles from the same group.
- Check that the name in the ini matches the pnach exactly, if you edited it by hand.
- Confirm you are running the US GT4 (SCUS-97328).

**The game crashes or looks wrong.**
- Turn off all toggles, then enable them one at a time to find the culprit.
- The Flare Distance Cull, Flare Glow and Sun Flare sections use small pieces of code stored in unused space of the game. If one of these crashes, that is the section to switch off first.
- The Off toggles remove effects entirely, so missing lights are expected with them.

**Lights pop in too close, or look too small.**
- Raise the Flare Distance Cull value, raise the Flare Cap, or move Flare Glow up one size.

**Restoring the original behavior.**
- Untick every toggle, or delete the `.pnach` file, and restart PCSX2. The patch writes only to memory while the game runs, so your game files and saves are not modified.

---

## Technical notes

- Addresses were checked against the decompressed `CORE.GT4` (raw deflate stream starting at file offset 6). In the decompressed image, offset = address - 0x100000 + 0x134.
- The flare draw functions are at `0x00498198` (lamp and track-light glow) and `0x00498398` (streak lights). The two flare queues are near `0x003B7B70` and `0x003B84xx`, and the shared visibility test is at `0x003B7C98`.
- Stubs live in zero-filled padding at `0x00495580`, `0x004955F0` and `0x00495640`.
- Each section's comments in the `.pnach` list the original instruction words and how to change the constants.

## Credits and disclaimer

Generated from disassembly of `CORE.GT4`. You need your own legally obtained copy of the game. This is unofficial and not affiliated with PCSX2, Sony or Polyphony Digital. Use at your own risk.
