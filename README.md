# Dell XPS 9500 macOS Monterey with OpenCore

# Details

| OpenCore Version | 0.9.5 |
| --- | --- |
| macOS Version | 12.6.8 (Monterey) |
| SMSBios | MacBookPro16,4 |

# Hardware Specifications

| Hardware | Specification | Status |
| --- | --- | --- |
| CPU | Intel Core i7-10750K | ✅ Working |
| RAM | DDR4 16GB | ✅ Working |
| Audio | Realtek ALC3281 | ✅ Working |
| WiFi | Killer 1675 (AX201) | ✅ Working |
| Bluetooth | AX211 Wi-Fi 5 | ✅ Working |
| SSD | Crucial P3 2TB | ✅ Working |
| Keyboard | - | ✅ Working |
| Trackpad | I2C Connection | ✅ Working |
| Webcam | Microdia RGB IR HD camera | ✅ Working |
| MicroSD Card | RTS5260 Card Reader | 🔶 Partially working |
| Fingerprint Sensor | Shenzen Goodix | 🔶 Partially working |
| S4 | Hibernate/Wake | ✅ Working |
| GPU | Intel HD630 Graphics | ✅ Working |
| eGPU | AMD Sapphire Radeon RX580 | ✅ Working |
| Display | 1920 x 1200 FHD LCD | ✅ Working |

# Overview

This is the first working configuration for the Dell XPS 9500 with working S4 hibernate/resume. S3 seems to be elusive (for now) but S4 works seamlessly and honestly it's a much more practical option as the machine shuts down and then resumes seamlessly on power on.

# BIOS Settings

| Setting | Option |
| --- | --- |
| SATA Operation | AHCI |
| Fast Boot | Thorough |
| Secure Boot | Disabled |
| TMP 2.0 Security | Disabled |
| Intel SGX | Disabled |
| VT for Direct I/O | Disabled |
| Fingerprint Reader | Disabled |

# UEFI IFR edits
Ahead of installing, as with other hackintoshes you will need to disable 
CFG_LOCK using modGRUBshell as follows:

```bash
setup_var_cv CpuSetup 0x43 0x00
```

# S3 ACPI
Despite Dell's attempts to remove S3 from the sleep options, it's possible, in theory at least, to get S3 working using a combination of IFR edits 
and ACPI table changes. The first step is to re-enable S3 from the UEFI interface using modGRUBshell:

```bash
setup_var PchSetup 0x16 00 (RTC Memory Lock ->Disabled)
setup_var CpuSetup 0x3E 00 (CFG Lock ->Disabled)
```

# Known Issues

- Slight power consumption increase on resume from hibernate

This is likely due to the dGPU powering up again on resume from S4 sleep. I need to figure out how to ACPI debug and patch the offending method in DSDT, unless someone more experienced shows me first...

