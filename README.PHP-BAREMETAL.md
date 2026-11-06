# php-baremetal-sim-qemu (Velxio QEMU source mirror)

Mirror of the **Velxio QEMU source** (GPL-2.0), used to build the
`libqemu-xtensa.so` / `libqemu-riscv32.so` libraries that the
[php-baremetal/php-baremetal-sim](https://github.com/php-baremetal/php-baremetal-sim) fork loads
to emulate the ESP32 / ESP32-S3 / ESP32-C3.

## Where it comes from

| | |
|---|---|
| Archive | `qemu-source-98580369` from `https://velxio.dev/api/pro/license/downloads` |
| sha256 | `2ed11c89094bed93059c48981fd23e6a158274fc514e261a980004387d4140e8` |
| Velxio commit | `985803692859cf8f08a48e4f13bbf95e8a6e6798` (2026-10-31) |
| Matching binaries | `libqemu-xtensa` 1.2.10 (`QEMU_ASSETS_REV=xtensa-1.2.10` in Velxio's `Dockerfile.standalone`) |
| Upstream chain | QEMU → `lcgamboa/qemu` (`picsimlab-esp32`) → Velxio fork |

The tree is imported **unmodified**. The only files added by us are this
README, `Dockerfile.php-baremetal` (the archive's `Dockerfile.build` is stale:
it lacks `tomli` and fails on Ubuntu 22.04) and any CI files under
`.github/workflows/` that are marked as php-baremetal.
`README.VELXIO-SOURCE.md` (from the archive) lists what Velxio adds over
`lcgamboa/qemu`, including the `*-picsimlab` machines, which is why an
upstream clone cannot replace this tree.

## Building

```bash
docker buildx build -f Dockerfile.php-baremetal --platform linux/amd64 \
  -o type=local,dest=./out .
ls -lh out/   # libqemu-xtensa.so + libqemu-riscv32.so, ~45-50 MB each
```

The 4 ROMs Velxio needs next to the `.so` are already in `pc-bios/`, byte-for-byte
identical to the ones velxio.dev distributes:
`esp32-v3-rom.bin`, `esp32-v3-rom-app.bin`, `esp32c3-rom.bin`, `esp32s3_rev0_rom.bin`.

## Release asset names

`Dockerfile.standalone` (with `QEMU_RELEASE_URL`) expects:

```
libqemu-xtensa-amd64.so   libqemu-riscv32-amd64.so
libqemu-xtensa-arm64.so   libqemu-riscv32-arm64.so   (optional)
esp32-v3-rom.bin  esp32-v3-rom-app.bin  esp32c3-rom.bin  esp32s3_rev0_rom.bin
```

## License

GPL-2.0: see `COPYING` and `LICENSE`. Whoever receives the binaries is
entitled to this source.
