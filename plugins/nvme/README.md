---
title: Plugin: NVMe
---

## Introduction

This plugin adds support for NVMe storage hardware. Devices are enumerated from
the Identify Controller data structure and can be updated with appropriate
firmware file. Firmware is sent in 4kB chunks and activated on next reboot.

The device GUID is read from the vendor specific area and if not found then
generated from the trimmed model string.

## Firmware Format

The daemon will decompress the cabinet archive and extract a firmware blob in
an unspecified binary file format.

This plugin supports the following protocol ID:

* `org.nvmexpress`

## GUID Generation

These device use the NVMe DeviceInstanceId values, e.g.

* `NVME\VEN_1179&DEV_010F`
* `NVME\VEN_1179`

The FRU globally unique identifier (FGUID) is also added from the CNS if set.
Please refer to this document for more details on how to add support for
[FGUID](https://nvmexpress.org/wp-content/uploads/NVM_Express_Revision_1.3.pdf).

Additionally, an extra instance IDs containing the version may be added.

* `NVME\VEN_1179&DEV_010F&VER_3B2QGXA7`

Additionally, for NVMe drives with Dell vendor firmware two extra GUIDs are
added:

* `STORAGE-DELL-${component-id}`

and any optional GUID saved in the vendor extension block.

## Update Behavior

The firmware is deployed when the device is in normal runtime mode, but it is
only activated when the system is either restarted or in some cases shutdown.

## Quirk Use

This plugin uses the following plugin-specific quirks:

### NvmeBlockSize

The block size used for NVMe writes

Since: 1.1.3

### Flags

* `force-align` if image should be padded, since 1.2.4
* `commit-ca3` download and activate immediately, since 1.8.15

## Vendor ID Security

The vendor ID is set from the udev vendor, for example set to `NVME:0x1179`

## External Interface Access

This plugin requires ioctl `NVME_IOCTL_ADMIN_CMD` access.

## Version Considerations

This plugin has been available since fwupd version `1.1.2`.
