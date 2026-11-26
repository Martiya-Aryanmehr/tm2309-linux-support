# Xiaomi Redmi Book Pro 16 2024 (TM2309) — Linux Support

Linux platform profile support for the Xiaomi Redmi Book Pro 16 2024, identified by DMI as `XIAOMI / TM2309`.

This project documents the development, testing, and upstream submission of platform profile support for the TM2309 laptop through the existing `bitland-mifs-wmi` Linux kernel driver.

## Project Status

**Upstream status:** Patch v2 submitted to the `platform-driver-x86` mailing list on November 25, 2026. Awaiting review.

**Hardware:** Xiaomi Redmi Book Pro 16 2024  
**Board:** TM2309  
**Operating system used for testing:** Arch Linux  
**Test kernel:** `7.3.5-arch1-1`

## Supported Platform Profiles

The proposed driver quirk maps the platform profiles to the following firmware QFAN values:

| Linux platform profile | QFAN value |
|---|---:|
| Low Power | 2 |
| Balanced | 1 |
| Performance | 3 |
| Max Power | 4 |

The firmware also reports QFAN value `0` as Balanced. The proposed implementation treats this value as read-only and uses QFAN `1` when setting Balanced.

Performance mode is configured for both AC and battery operation.

## Implementation

The support is implemented as a TM2309-specific quirk in the existing upstream `bitland-mifs-wmi` driver.

The DMI match uses:

- System vendor: `XIAOMI`
- Board name: `TM2309`

The quirk also skips the AC type check because the required AC type WMI functionality is not available on this platform.

No separate driver or proprietary userspace service is required by the proposed implementation.

## Testing

The development build was tested on the actual TM2309 hardware.

Tests performed:

- External module compilation against the running Arch Linux kernel.
- Successful loading of the custom module.
- Verification of the TM2309 platform profile choices.
- Profile selection and readback for all four profiles on AC power.
- Performance profile selection and readback on battery power.
- AC-to-battery and battery-to-AC transition checks while Performance was selected.
- Controlled CPU-load tests using four concurrent `yes` processes.
- Inspection of available hwmon interfaces and driver diagnostics.

### Important limitations

- Firmware fan RPM and temperature readings exposed through the driver's hwmon interface returned `EOPNOTSUPP` during testing.
- The CPU-load measurements provide useful observations but do not independently prove the physical fan response to each QFAN value.
- The tested development source included local compatibility changes for the installed Arch kernel headers. Those changes are separate from the submitted upstream patch.

## Patch

The upstream submission is maintained separately from local development modifications.

See the [exact v2 patch submitted to the mailing list](patches/0001-platform-x86-bitland-mifs-wmi-add-profile-support-for-xiaomi-tm2309.patch).

## Upstream Discussion

The patch was submitted to the Linux kernel `platform-driver-x86` mailing list.

This project is not yet part of the mainline Linux kernel. Its upstream status will be updated as the review progresses.

## Author

**Martiya Aryan Mehr**

- GitHub: [@martiya-aryanmehr](https://github.com/martiya-aryanmehr)
- Telegram: [@martiya_a](https://t.me/martiya_a)

---

*This repository documents hardware-specific Linux support development and upstream contribution work. Testing results are from the author's own TM2309 device.*
