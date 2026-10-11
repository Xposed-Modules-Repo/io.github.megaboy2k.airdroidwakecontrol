# AirDroid Wake Control

AirDroid Wake Control is an Xposed module for unattended Android devices. It
allows AirDroid screen mirroring and the remote camera to start without leaving
a previously sleeping phone's physical OLED panel illuminated.

If the screen was already on, the module keeps it on and preserves its
brightness. AirDroid Remote Control continues to wake the display and accept
unlock gestures and touches normally.

The module also restores the Android MediaProjection AppOps state required by
AirDroid after a reboot, avoiding repeated **Start now** capture prompts on the
tested device.

> **Unofficial project:** this is an independent community module. It is not
> developed, supported, approved, or endorsed by Sand Studio (AirDroid),
> OnePlus, OPPO/OPlus, OxygenOS, Vector, or LSPosed.

> The module hooks private Android and OxygenOS implementation details. It has
> only been tested on a OnePlus 8T running Android 13 / OxygenOS 13.1. Other
> devices, ROMs, and Android versions may require different hook mappings.

## Features

- Keeps a previously sleeping OLED panel dark during AirDroid mirroring and
  remote camera sessions.
- Preserves the current display state when the screen was already on.
- Leaves normal AirDroid Remote Control wake, unlock, and input behavior intact.
- Restores UID-level and package-level `PROJECT_MEDIA` AppOps for
  `com.sand.airdroid` during `system_server` startup.
- Has no launcher activity, background service, network permission, or direct
  root request.

## Compatibility

- Tested device: OnePlus 8T.
- Tested system: Android 13 / OxygenOS 13.1.
- Tested framework: Vector 2.2 (API 102) with Zygisk.
- Module interface: legacy Xposed API, minimum framework API 93.
- Required scope: **System Framework only**.
- Target applications:
  - AirDroid Personal (`com.sand.airdroid`)
  - AirDroid Remote Support Add-on (`com.sand.remotesupportaddon`)

Vector is the tested loader because LSPosed 1.9.2 could not initialize LSPlant
on the tested OxygenOS build. The module does not use a Vector-specific API.

## Installation

1. Download and install the release APK as a normal Android application.
2. Open Vector, LSPosed, or another compatible Xposed manager.
3. Enable **AirDroid Wake Control**.
4. Confirm that its only scope is **System Framework** (`system/0` in Vector).
5. Reboot the device.

The APK declares the recommended scope through modern and legacy Xposed
metadata. Framework managers may preselect it, but activation of a module with
system scope still requires user confirmation.

## Verification

Check MediaProjection AppOps:

```shell
cmd appops get com.sand.airdroid PROJECT_MEDIA
```

Expected output includes both UID and package modes set to `allow`.

Check module logs:

```shell
su -c 'grep -h AirDroidWakeControl /data/adb/lspd/log/modules_*.log'
```

## Security notice

System Framework scope loads module code into `system_server`. Review the
source and keep tested recovery access before installation. A failed or
ROM-incompatible hook can destabilize Android or cause a boot loop.

## Source, documentation, and support

- Source code: https://github.com/megaboy2k/airdroid-wake-control
- Issues: https://github.com/megaboy2k/airdroid-wake-control/issues
- Discussions: https://github.com/megaboy2k/airdroid-wake-control/discussions
- License: Apache License 2.0

