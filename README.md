# Void Wars

A small voxel bed-wars game: two or four teams on floating islands, bots to fill
the teams, and LAN multiplayer. Three.js in the page, one shared match server,
and a standalone macOS app.

Protect your bed, break theirs. While your bed stands you come back three seconds
after dying; without it you are out, and the last team standing wins.

## Download and play (macOS, no terminal)

1. Open the [latest release](https://github.com/Isaiah4775/void-wars/releases) and
download **VoidWars-mac.zip**.
2. Unzip it and drag **Void Wars** into Applications.
3. Open it from Spotlight (or Launchpad). It starts the game server, opens the
game in your browser and shows the address friends join.

The app carries the page and a standalone server compiled for Apple Silicon and
Intel, so it needs no Bun, Node or terminal. It is not notarised: the first
launch of a downloaded copy needs right-click, Open. If macOS says the app is
damaged, run
`xattr -dr com.apple.quarantine "/Applications/Void Wars.app"` in Terminal once.

## Play with friends on your Wi-Fi

Launching the app prints an address such as `http://192.168.1.24:3000` and shows
it in the start menu. Friends on the same Wi-Fi open that address in a browser
and click **Play with friends on your Wi-Fi**: everyone joins one match, with the
server's bots filling the empty teams.

The server is the referee. It moves everyone, counts the ticks you hold on a
block, checks reach, items and cooldowns, and sends the page 20 snapshots a
second, so a page cannot fly, place blocks across the map or break the island.
A new round starts eight seconds after the last one ends.

## Controls

WASD move, Space jump, Shift sneak (no falling off edges), Ctrl or R sprint,
left click hit or hold to break, right click place or eat, 1-9 or the wheel for
the hotbar, B shop, H help, F full screen, Esc pause. Stand on your base's white
pad to collect iron.

## Build from source

Needs [Bun](https://bun.sh) 1.3 or later.

```sh
bun install
bun run dev                    # solo, http://localhost:5173
bun run build && bun run lan   # LAN server on http://localhost:3000
bun run app                    # VoidWars-mac.zip with "Void Wars.app"
bun test                       # physics, combat and match rules, headless
bun run typecheck
```

`src/game/` is the DOM-free game (world, physics, rules, bots, one map) and
`src/client/` draws it: three.js scene, HUD, menus, and `net.ts` for LAN play.
`server/index.ts` serves the build and owns the room. `bun run app` compiles that
server for both Mac architectures, so the zip runs on a Mac without Bun.
