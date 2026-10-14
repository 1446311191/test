## What changed in this version

The probe now maps the whole machine, down to how each driver talks to its hardware, so the installer can build the
right setup for your exact PC.

- **Full system map.** Which GPU drives each screen and the laptop panel, switchable-graphics hints, GPU features
  (architecture, CUDA level, RT and tensor cores), PCIe slots, memory modules (no serials), storage ports and modes
  (AHCI/RAID/VMD), fans and sensors, USB roles (Bluetooth, camera, RGB, fingerprint), touchpad and keyboard bus,
  CPU core layout, IOMMU and the firmware memory map.
- **Build report.** The stick also writes a `Report.json` in the format the EFI builder reads (collector from
  Hardware-Sniffer, BSD-3).
- **Driver trace.** At the end, each graphics and audio driver is restarted while the kernel records every register
  read and write it makes. NVIDIA cards get NVIDIA's own driver for their generation, carried on the stick:
  open 610 for Turing and newer, 580 for Maxwell/Pascal/Volta (Kepler and older: nouveau for now).
  The screen may go black for a few minutes during this step.
- **Wi-Fi, Bluetooth and Ethernet: your choice.** The stick still has no network drivers in it. At the start it asks
  (default: no). If you press Y, it loads only the drivers for your own chips, records them starting, switches each
  radio off the moment it appears, and removes the drivers again. Nothing is configured or sent.
- Traces are redacted like the rest of the report (including a network chip's address as it appears in its
  registers) and capped at 40 MB.

Every earlier version stays up, with its hashes, on the releases page and at https://nullmothsystems.com/probe

## About 1401 Probe

Free, offline, and open source.

1401 Probe turns a USB stick into a small scanner for your PC. You start the PC from the stick, it reads what
hardware the machine has, saves one report to the stick, and turns the PC off. Nothing leaves your computer unless
you upload the report yourself. Every report shows us what 1401 needs to support, so if you run it: thank you.

The same how-to, with the rest of the details, is on the site: https://nullmothsystems.com/probe

## Downloads

| File | What it is | SHA-256 |
|---|---|---|
| `1401-Probe-Setup.exe` | The Windows app. It makes the stick for you. Start here. | `655f616cfe439ebc24c4939193dc9374a19944cea2744d28dfabe9628e4e8ba0` |
| `1401-probe-img.zip` | The raw stick image, if you don't have Windows or want to flash it yourself | `5fa27706e95f03da47851be9c4f00fb368ca9c36eee2de73b8ae94ad291fa59d` |
| `1401-probe-linux.py` | For Linux: makes the stick and finds your report afterwards (`python3 1401-probe-linux.py`, then `--report`) | `6a77d850f4e7b5d20575d8ce0008be10ef324d6a479ede95edf5cac3ca46ea98` |
| `SHA256SUMS.txt` | Both hashes in one file | |

## Check the installer before you run it

The installer isn't code-signed yet, so Windows SmartScreen will warn you about it. Before you click past that,
make sure the file is really ours. Its fingerprint is published in DNS, signed with DNSSEC, on a different system
from this download:

    dig +short TXT _probe-sha256.nullmothsystems.com

Or on Windows, in PowerShell:

    (Resolve-DnsName -Type TXT _probe-sha256.nullmothsystems.com).Strings

Then hash the file you downloaded, from the folder you saved it in:

    Get-FileHash .\1401-Probe-Setup.exe

The two SHA-256 values should match exactly. If they don't, don't run it.

## From Linux

Download `1401-probe-linux.py` (and `1401-probe-img.zip` next to it, or let it download the image). Run
`python3 1401-probe-linux.py`: it checks the image against this release's SHA-256, lists USB sticks only (never
internal disks or the disk Linux runs from), asks you to type the stick's name and ERASE, writes it and reads it back
to verify. After the scan, plug the stick back in and run `python3 1401-probe-linux.py --report`.

## What you need

- The Windows PC you want to scan
- A USB stick, 1 GB or bigger. It gets erased, so copy anything you want off it first.
- About 15 minutes
- Secure Boot turned off for the scan. You turn it back on afterwards, and the app walks you through it.

## How to use it

1. **Install.** Run `1401-Probe-Setup.exe`. When SmartScreen says "Windows protected your PC", click **More info**,
   then **Run anyway**.

2. **Make the stick.** Open 1401 Probe and click **Make the scanner USB stick**. If BitLocker is on, click **Pause
   BitLocker for 3 restarts** first. Pick your stick, tick the box, and click **Erase and make the stick**. Only USB
   drives show up in the list, so you can't wipe your main drive by accident.

3. **Turn off Secure Boot and start from the stick.** Click **Restart into BIOS setup**. Find Secure Boot (it's
   usually under Boot or Security), set it to Disabled, then save and exit. As the PC starts, press your boot menu
   key and pick the USB stick marked UEFI. Boot menu keys: ASUS F8, MSI F11, Gigabyte F12, ASRock F11, Dell F12,
   HP F9, Lenovo F12, Acer F12. You can also click **Restart now and pick the USB stick** in the app and choose
   "Use a device". On the stick's menu, pick **1401 Probe - scan this computer**. It starts by itself after 5
   seconds.

4. **Let it scan.** It takes 2 to 8 minutes. The last step is a graphics test, and the screen can flicker or go
   black for up to 3 minutes. That's normal. When it says DONE, the report is saved and the PC turns itself off.
   If nothing happens for 5 minutes, hold the power button. Everything up to that point is already saved. Start
   from the stick again and pick the normal scan: it skips the step that froze and finishes the rest. If it
   freezes a second time, pick **safe scan**, which skips the graphics test.

5. **Put your settings back.** Go into the BIOS again (usually Del or F2) and set Secure Boot back to Enabled. Back
   in Windows, open 1401 Probe and click **I already scanned - find my report**. It checks Secure Boot and BitLocker
   for you and offers to turn BitLocker back on. Your boot order was never changed.

6. **Send the report.** Optional, but it's the whole point. Click **Show both files**. The stick's REPORTS folder
   has a .zip and a .txt with the same name, something like `Z790-AORUS_i7-13700K_RTX4070_2026-09-25_a3f1.zip`.
   You can open both and read them first. Then go to https://nullmothsystems.com/#send, drag both files onto the
   1402 Card Reader, and click Send. Keep the report ID it gives you (NM-XXXXXXXX).

## What it does and doesn't do

- It reads your hardware and writes one report, to its own stick. That's it.
- It has no network, Wi-Fi or Bluetooth drivers, so it can't connect to anything.
- Before it scans, it marks every drive except its own stick read-only. It never mounts your internal drives.
- It never changes BIOS/UEFI settings. Secure Boot is the one setting you change, yourself.
- Serial numbers, UUIDs, MAC addresses and your Windows product key are removed from the report before it's saved.
- Nothing is uploaded automatically. Sending the report is up to you.

## No Windows?

Download `1401-probe-img.zip`, unzip it, and write `1401-probe.img` to a USB stick with balenaEtcher, or Rufus in
DD mode. Then do steps 3 and 4, turn Secure Boot back on in your BIOS yourself, and send the two files from the
stick's REPORTS folder like in step 6.

## Done with the stick?

Open 1401 Probe, click **I already scanned - find my report**, then **Make it an ordinary USB drive again**.

## Build it yourself

Everything builds in Docker containers pinned by digest, and nothing gets installed on your machine. From a clone
of this repo:

    NM_ALLOW_NO_PRIVATE=1 ./build.sh all

The README has the details and the QEMU test that boots the real stick image.

## License

GPL-3.0-or-later. The parts it's built from (Linux, GRUB, BusyBox, Python, Alpine packages, firmware files) keep
their own licenses, listed in THIRD-PARTY-NOTICES.txt on every stick. The NullMoth name and moth logo aren't
covered by the GPL (section 7(e)), so a modified version has to drop them.

Found a bug, or a machine it chokes on? Open an issue.

