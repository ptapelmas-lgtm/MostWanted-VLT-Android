# VLT script support changes (Android Mods Loader)

## What changed

- Car identification now prefers a matching `CARNAME_*_<CAR>` entry and a car-named `.nfsms` script over a staging folder name such as `Binary/Main`.
- The loader follows `script <path>` references in a VltEd modscript wrapper. It selects the VltEd install branch and ignores OGVI and uninstall branches.
- `resize_field` now updates an array's logical count when the existing VLT array has enough reserved capacity. It reports a clear unsupported condition when the requested count exceeds that capacity; it does not relocate BIN data or rewrite arbitrary pointers.
- `MaxGs` is accepted as an alias for the existing `MaxG` vector member offset.
- The PC-side `tools/car_mod.py` converter has matching car-name, script-wrapper, and bounded-array-resize behavior.

## Validation performed

- `CarConverter.java` compiles with `javac`.
- `tools/car_mod.py` passes `python -m py_compile`.
- A focused test against the uploaded Nissan Skyline Z-Tune archive identifies `SKYLINEZT` rather than `MAIN` as the car name.
- The same test resolves the top-level `SKYLINEZT.nfsms` wrapper to `VLTData/install.nfsms` and `VLTData/installvlted.nfsms`; OGVI and uninstall scripts are excluded.
- The resolved scripts contain 18 `copy_node`, 298 `update_field`, and 1 `resize_field` operations.

## Important limitations

This is VLT modscript support, not a full Binary/Unlimiter installer. `add_field` and new VLT field definitions are still unsupported. The `.end` installer language is not executed. Consequently, imports such as custom `DBModelParts`, collision and material collections, frontend texture packs, Unlimiter tuning/part data, and custom vinyl packs are not installed. The Nissan archive's README requires NFSMW Unlimiter v4 and its `.end` installation step for its complete tuning/body-kit/vinyl feature set. This build can only attempt the base add-on car and supported VLT database edits; full Nissan customization is not yet verified or promised.

## APK build blocker in this environment

The uploaded source archive does not include the game `default.xbe`/disc image required by this project's build workflow. No Android SDK/NDK is configured in the build environment, and the SDL checkout required by Gradle is absent. Therefore an installable APK could not be produced or integration-tested here. The patched source is provided for a build environment containing the required game files and Android toolchain.
