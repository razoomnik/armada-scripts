# Armada Scripts

Recovery and maintenance scripts for handheld devices running
[SteamOS Armada](https://github.com/virtudude/armada).

## Scripts

### AYN Odin 3 gyro recovery

`scripts/odin3-gyro-recovery.sh`

Restores full gyroscope support on AYN Odin 3 after a clean Armada installation
or an update that removes or breaks the custom sensor stack.

The script automatically:

- verifies that it is running on an AYN Odin 3;
- detects the current Armada build and exact kernel image;
- finds the matching `armada-packages` source commit;
- builds `sns_iio.ko` for the currently running kernel;
- builds `adsprpcd` and `snsfeed`;
- installs the Qualcomm Sensor Core registry;
- installs and enables `odin3-sensors.service`;
- adds `bmi323-imu` to the Odin 3 InputPlumber profile;
- installs the correct accelerometer mount matrix for automatic screen rotation;
- verifies the IIO device, live sensor values, services, and InputPlumber detection.

### Requirements

- AYN Odin 3
- SteamOS Armada
- Internet access
- `podman`, `skopeo`, `git`, `curl`, and standard Armada system tools
- At least 8 GiB of free space is recommended for the build cache

Run the script as the regular Armada user, **not** as root:

```bash
chmod +x scripts/odin3-gyro-recovery.sh
./scripts/odin3-gyro-recovery.sh
```

Reboot after successful installation:

```bash
sudo reboot
```

Force a complete rebuild:

```bash
./scripts/odin3-gyro-recovery.sh --force-rebuild
```

Remove the installed integration:

```bash
./scripts/odin3-gyro-recovery.sh --uninstall
```

The build cache and logs are stored in:

```text
~/odin3-gyro-recovery/
```

## Verify the download

```bash
sha256sum -c SHA256SUMS
```

## Tested configuration

Initial working implementation tested on:

- Armada `20260725.4d0cacb`
- Linux kernel `7.0.11`
- InputPlumber `0.77.2`
- AYN Odin 3
- `snsfeed` rate: 100 Hz

The recovery script does not hard-code that tested kernel build. It resolves the
kernel image and matching Armada source revision for the currently installed
system.

## License

MIT
