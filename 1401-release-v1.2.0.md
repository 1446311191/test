# 1401 1.2.0 for Windows

Includes the Mac companion 1401 1.2.0 and the NVIDIA driver 1.2.0 ([release notes](https://github.com/nullmoth/nvidia-macos-driver/releases/tag/v1.2.0)).

Changes
- A stick that failed to start is read before it is erased: when you write the stick again, 1401 first rebuilds with that stick's startup log, so the AMD memory-map steps and the other log-based fixes now reach the next build.
- Sticks that already have EFI or MSR partitions are accepted.
- Stalled Apple recovery downloads resume; a refused download retries with a new session.
- macOS 10.13 and 10.14 targets.
- Mirror downloads retry dropped connections.
- Firmware tables that define one device twice (seen on AM5 boards) no longer stop the build.

Keep the whole 1401 folder together and run 1401.exe.
