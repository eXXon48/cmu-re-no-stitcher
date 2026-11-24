# No Stitcher

`cmu-re-no-stitcher` · Revised Store package · Mazda CMU150, firmware 74.00.324A

Turns off Stitcher: its service (blm_ira_stitcher) and backend (irsstitcher_server) do not start at boot, MMUI no longer loads its screen module, and the Stitcher entry leaves the Entertainment list.

## Why

Stitcher (the podcast service, discontinued in 2023) still starts at boot with its backend `irsstitcher_server`, and its entry still sits in the Entertainment list. Nothing on the other side answers it any more, so it is pure boot cost.

## What it changes

| File | How | Why |
|---|---|---|
| `/jci/sm/sm.conf` | xml-node | Stitcher service and its backend off at boot; they never reset the board |
| `/jci/sm/sm_WCP.conf` | xml-node | Stitcher service and its backend off at boot; they never reset the board |
| `/jci/mmui/mmui_config.xml` | xml-node | MMUI no longer loads the screen module(s) of this service, which it loads one by one before it answers the GUI |
| `/jci/gui/apps/system/js/systemApp.js` | 3way | removes the Stitcher entry from the Entertainment list |

`xml-node` edits address single nodes by XPath; `3way` edits are small diffs against the stock file. The Store merges them with every other installed package, refuses a real clash, and takes them back out on uninstall. No stock file is replaced as a whole.

## Install and uninstall

Install from the Revised Store app on the unit, then restart. Uninstall the same way: after the restart the unit behaves as stock again.

Safety: every change is recorded in the Store's vanilla index, so the boot-loop tripwire reverts it on its own if the unit fails to boot several times in a row. No package edits the boot script above the tripwire line.

## Requires

- `cmu-re-store (>= 0.13.4)`

## Part of

- `cmu-re-no-streaming`

## Develop

```sh
sh test/run.sh                                  # manifest and payload checks
sh tools/mkpkg.sh <version> <rvp-pack> out.rvp   # build the .rvp
```

Releases: a `dev/vN` tag on `develop` gives a dev prerelease, a `vN` tag on `main` a release; CI packs the `.rvp` and attaches it to the GitHub release.
