# HebClockPi — update packages

Over-the-air update packages for the HebClockPi synagogue prayer-times display
(Raspberry Pi Zero 2 W). This repository holds **only the released binaries**; the source
is not here.

Each release carries two files:

| file | what it is |
|---|---|
| `hebclockpi-<version>-aarch64.tar.gz` | the `hebclockd` binary, the web config UI and the device scripts |
| `manifest.json` | release number, version, package URL, SHA-256, and the vendor signature |

## How a clock updates itself

A unit is configured with the address of the newest manifest:

```
ota_url=https://github.com/giltal/hebclock-releases/releases/latest/download/manifest.json
```

It fetches that manifest, **verifies the Ed25519 signature** against the vendor public key
compiled into the running firmware, downloads the package, checks the SHA-256 the
signature covers, installs it, and restarts — rolling back to the previous build if the
new one does not come up healthy.

The signature covers the release number, the version and the package hash, so these files
can be hosted or mirrored anywhere: an altered package, or one from anyone but the vendor,
is refused by every unit. Hosting them publicly is therefore safe, and it means a clock
needs no account or credentials of any kind to update itself.

Units are licensed per board, so a package downloaded from here will not turn an
unlicensed device into a licensed one.
