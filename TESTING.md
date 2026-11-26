# TM2309 Linux Platform Profile Support — Testing Report

## 1. Test Environment

| Item | Value |
|---|---|
| Laptop | Xiaomi Redmi Book Pro 16 2024 |
| DMI System Vendor | XIAOMI |
| DMI Board Name | TM2309 |
| Operating System | Arch Linux |
| Tested Kernel | 7.3.5-arch1-1 |
| Driver | bitland-mifs-wmi |

Testing was performed on the actual TM2309 hardware.

## 2. Implementation Under Test

The development implementation adds a TM2309-specific quirk to
the existing `bitland-mifs-wmi` driver.

The proposed firmware QFAN mapping is:

| Linux Platform Profile | QFAN Value |
|---|---:|
| Low Power | 2 |
| Balanced | 1 |
| Performance | 3 |
| Max Power | 4 |

The firmware may report QFAN value `0` as a stale Balanced state
after resume. The implementation maps this value to Balanced
for readback and marks it as non-settable.

A separate Performance entry is provided for battery operation.

The AC type check is skipped because the required AC type WMI
functionality is not implemented by this platform's firmware.

## 3. Build and Driver Loading

The development driver was compiled as an external module
against the running Arch Linux kernel.

The custom development module was successfully loaded.

The development source included local compatibility changes
required by the installed Arch Linux kernel headers. These
changes are separate from the submitted upstream patch.

Additionally, patched and unmodified driver objects were built
against `linux-next-20261120` without compiler warnings.

The exact standalone v2 patch was not independently loaded
as a module. Full modpost and runtime testing of that isolated
patch were not performed.

## 4. Functional Testing

### 4.1 AC Power

The following checks were performed using the development driver:

- Verification of the available platform profile choices.
- Selection and readback of Low Power.
- Selection and readback of Balanced.
- Selection and readback of Performance.
- Selection and readback of Max Power.

All four profile selection and readback paths were exercised
while the laptop was connected to AC power.

### 4.2 Battery Power

The following checks were performed:

- Selection of the Performance profile on battery power.
- Readback of the selected Performance profile.

### 4.3 Power Source Transitions

AC-to-battery and battery-to-AC transition checks were
performed while Performance was selected.

These checks were intended to examine profile behavior
across power-source changes.

### 4.4 CPU Load

Controlled CPU-load testing was performed using four
concurrent `yes` processes.

These observations were used as supplementary checks
during profile testing. They do not independently establish
the physical fan response or thermal behavior associated
with each firmware profile.

## 5. Hardware Monitoring Limitations

Available hwmon interfaces and driver diagnostics were inspected.

Fan RPM and temperature readings exposed through the
driver's hwmon interface returned:

`EOPNOTSUPP`

Consequently:

- Reliable fan RPM measurements were not available through
  the tested hwmon interface.
- Reliable temperature readings were not available through
  that interface.
- Physical fan response to individual QFAN values was not
  independently verified.

The CPU-load observations must not be interpreted as
direct measurements of fan speed or cooling performance.

## 6. Test Scope and Limitations

The successful module loading and functional checks described
above apply to the local development driver.

The development source contained local compatibility changes
for the installed Arch Linux kernel headers. Those changes
are not part of the submitted upstream patch.

Although patched and unmodified driver objects were compiled
against `linux-next-20261120` without compiler warnings,
the exact standalone v2 patch has not undergone independent
module loading, full modpost validation, or isolated runtime
testing.

Therefore, the results in this report should not be interpreted
as proof that the submitted patch has already passed complete
upstream integration testing.

## 7. Upstream Status

Patch v2 was submitted to the Linux kernel
`platform-driver-x86` mailing list on November 25, 2026.

The patch is awaiting review.

It has not been accepted into the mainline Linux kernel.

Submitted patch:

`patches/0001-platform-x86-bitland-mifs-wmi-add-profile-support-for-xiaomi-tm2309.patch`

## 8. Summary

Testing on the actual Xiaomi Redmi Book Pro 16 2024 (TM2309)
demonstrated platform profile selection and readback using
the local development driver.

Battery Performance selection and power-source transition
checks were also performed.

However, hwmon limitations prevented reliable fan RPM and
temperature measurements, and the exact standalone upstream
patch has not yet undergone independent runtime testing.

The results documented here distinguish observed development
testing from validation that remains outstanding.
