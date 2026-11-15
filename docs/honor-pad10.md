# Dextop Pad 10

This fork adds a HONOR Pad 10 compatibility profile to upstream [NarYuki/Dextop](https://github.com/NarYuki/Dextop), based on commit `3767eb2`, development version 2.0.0+39. It installs as **Dextop Pad 10**, package `moe.n4tsu.dextop.pad10`, alongside the Play Store app.

Validated on 15 November 2026 using a HEY3-W00 tablet, product HEY3-W09, running MagicOS 10.0.0.134(C636E4R104P1), Android 16. The profile matches HEY3-W00 and HEY3-W09 on Android API 36. Other firmware has not been validated.

## Behavior

HONOR's native freeform manager minimizes older windows when additional apps open. This profile selects Dextop Book and composites a separate fullscreen virtual display for each app. It does not change HONOR's protected window-count configuration.

The profile also disables virtual-display system decorations to avoid a HONOR navigation-bar crash, uses a direct desktop display, and temporarily disables forced external-display desktop mode. Dextop restores the previous global settings when the session stops.

Direct touch is the default on the Pad 10. An explicitly saved input-mode preference still takes precedence. Other device profiles retain their existing defaults.

## Use

1. Start Shizuku and authorize Dextop Pad 10 in its application management.
2. Open Dextop Pad 10 and select **Start**. Use Dextop Book as the window manager.
3. Open apps through the taskbar's app grid. Drag title bars to move windows and the outside borders to resize them.
4. To open Dextop controls in landscape, swipe right from the left edge with three fingers. In portrait, swipe down from the top edge with three fingers. The controls offer **Tap** for direct touch, **Cursor** for trackpad input, and **Stop** to leave the desktop.

Shizuku's ADB service stops after reboot. Restart it through wireless debugging or a connected computer before starting Dextop again.

To keep the tablet awake while charging, enable **Stay awake** in Android Developer options. This is a device preference, separate from the compatibility profile.

## Validation

- The debug APK compiled and installed successfully.
- All 12 `DeviceMatchTest` tests passed, including Pad 10 matching and isolation from other HONOR models and Android versions.
- Clock, Settings, and Files stayed visible together on separate app displays. A fourth window also remained visible in an earlier session. A maximum window count has not been established.
- Moving, resizing, app controls, and text entry worked during device testing.
- After changing the default input mode, input through the physical display opened the launcher, launched Clock, changed its tab, and moved its window. The tablet owner also confirmed that finger touch works.
- Stopping released the virtual displays and restored temporary global settings. Restarting the desktop worked.
- The existing display density override was preserved. No firmware files were modified, no root access was used, and no reboot was performed.

## Build

Use Flutter 3.47.7, Java 17, Android SDK 36, and NDK 28.2.13676358. Configure `android/local.properties` for the local SDK, then run:

```sh
flutter pub get
flutter build apk --flavor github --debug --target-platform android-arm64 --dart-define=DISTRIBUTION_CHANNEL=github
cd android
./gradlew :app:testGithubDebugUnitTest --tests moe.n4tsu.dextop.DeviceMatchTest
```

The Pad 10 package identity applies to the `github` flavor used above. The `play` flavor retains upstream's Play Store application ID and is not the build intended for this fork.

This is an experimental debug build from upstream development code. Android Auto companion pairing is unsupported in this separate-package fork. Compatibility with all apps, including protected video and apps with unusual display assumptions, has not been established. The upstream license remains applicable.

## Remove

Stop Dextop so session cleanup can finish, then uninstall Dextop Pad 10. The original Play Store installation remains installed. Shizuku can be stopped or removed if no other apps need it.
