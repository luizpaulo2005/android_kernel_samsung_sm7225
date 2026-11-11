# Linux kernel for Samsung Galaxy M23 5G (m23xq)

Linux 4.19 kernel for the Samsung Galaxy M23 5G (SM-M236B, m23xq), used by [PixelOS 17](https://github.com/luizpaulo2005/pixelos-m23xq).

| | |
|---|---|
| **Base** | [samsung-sm7225/android_kernel_samsung_sm7225](https://github.com/samsung-sm7225/android_kernel_samsung_sm7225) @ `lineage-23.2-rebase` (`4dbb83072`) |
| **Target** | Android 17, PixelOS 17 (tested) |
| **Defconfig** | `arch/arm64/configs/vendor/samsung/m23xq.config` |
| **Branch** | `seventeen` (`lineage-23.2-bpf` is the same code, kept for old links) |

The upstream history is kept as is. Every commit keeps its original author.

## Changes on top of the base

| Commit | Change | Why |
|---|---|---|
| `6bc3694c` | Remove the `Android.mk` files | They define modules that clash with the ROM tree (PixelOS only) |
| `35d05dcc` | `CONFIG_UNICODE=y` | Android casefolding needs it; without it, first boot fails with `init_user0_failed` |
| `2f321b98` | kgsl: pass `&entry->memdesc` to `kgsl_destroy_ion()` | The backport passed `priv_data`, so ION buffers were never freed and the phone froze after a few minutes |
| `e873c456` | `CONFIG_DISABLE_LOCKSCREEN_USB_RESTRICTION=y` | Without the Samsung framework, USB data stays blocked |
| `3c7e04a2` | `__maybe_unused` on `disconnect_usb_driver` | Build fix with `-Werror` |
| `7da21f4d` | dwc3-msm: signal usb_notify when probe completes | Lost in the rebase; USB (MTP and adb) never enumerated |
| `d848fd27` | `CONFIG_LARGE_DIRTY_BUFFER=y` | The default 50 MB dirty limit stalled f2fs writeback during app installs |
| `f75f8d1c` | Remove the dirty threshold cap | Same stall, from the 100 MB cap |

## Building

Built inside the PixelOS 17 tree with the device's `BoardConfig` (`TARGET_KERNEL_CONFIG`, clang r416183b). See the [local manifest](https://github.com/luizpaulo2005/pixelos-m23xq/blob/main/m23xq.xml).

## License

GPL-2.0, see `COPYING`.
