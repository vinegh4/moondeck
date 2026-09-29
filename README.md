# MoonDeck ![Status](https://github.com/FrogTheFrog/moondeck/actions/workflows/build.yaml/badge.svg) [![Chat](https://img.shields.io/badge/Chat-on%20discord-7289da.svg)](https://discord.com/invite/U88fbeHyzt)

A plugin that lets you play any of your Steam games via Moonlight without needing to add them to Sunshine first, providing a similar experience to GeForce GameStream or Steam Remote Play.

![quicksettings](.github/assets/quickmenu.png)

## What is it really?

MoonDeck is an automation tool that will simplify launching your Steam games via the Moonlight client for streaming.

It requires an additional lightweight app to be installed on the host PC - [MoonDeck Buddy](https://github.com/FrogTheFrog/moondeck-buddy). Additional one-time setup instructions can be found within the settings page of the plugin itself.

## Armada

MoonDeck also runs on ARM64 handhelds with [Armada OS](https://github.com/armada-os/armada), tested on the AYN Odin 2 Portal. No extra configuration is needed:

* The runner loads an aarch64 build of `psutil` from `defaults/python/externals-aarch64`. Every other bundled dependency falls back to pure Python.
* The Moonlight flatpak is started for the host architecture, so use the aarch64 Moonlight from the Armada store.
* The automatic resolution is rounded down to even dimensions. The Portal reports 1919x1078, and host encoders reject odd sizes.

To install, download `moondeck.zip` from this repository's releases, enable Developer mode in Decky and use **Install Plugin from ZIP**. A MoonDeck update from the Decky store replaces it with the upstream x86_64-only build.

Known limitations:

* The Moonlight flatpak has no working hardware video decoding on the Adreno GPU, so it decodes on the CPU. If the stream stutters, lower the resolution or bitrate, or set **Video codec** to H.264 in MoonDeck -> Moonlight settings -> General.
* Linked-display features don't work, because the ARM Steam client lacks `SteamClient.System.DisplayManager`. The related frontend errors are harmless.

When bumping versions in `defaults/python/requirements.txt`, update `defaults/python/requirements-aarch64.txt` to match and run `scripts/update-arch-externals.sh`.

## Building

To build and deploy the plugin package first copy `.env.example` to `.env` and update any relevant settings. Then either:

* Run `pnpm` commands `pnpm run setup` and `pnpm run build:plugin` and `pnpm run deploy`.
* Run VSCode tasks Ctrl+Shift+P or Cmd+Shift+P and run: `Tasks: Run Task` and choose the task to run.

## Internal data

This plugin stores data in the following directories:

* Settings - `/home/$USER/.config/moondeck/settings.json`
* Backend logs - `/tmp/moondeck.log`
* Runner logs - `/tmp/moondeck-runner.log`
* Moonlight logs - `/tmp/moondeck-runner-moonlight.log`

Additional frontend logs are written to the `web console`.

## License

This is licensed under GNU GPLv3.
