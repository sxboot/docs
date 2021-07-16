# Installation

Because there are currently no installers available, installation is relatively difficult. Some knowledge about hard drive layout is recommended.

**Disclaimer: If you install this boot loader on your real computer, this will replace the boot loader shipped with your operating system.
Contributors of *sxboot* will take no responsibility if your computer becomes unbootable or otherwise damaged. You do everything entirely at your own risk and responsibility.**


## Boot loader layout
*sxboot* will use its own partition formatted using FAT16 to store executable files, modules and other files.

The following files are present on this partition for correct startup:
- `/config/config.cfg` - The configuration file.
- `/boot/s2boot` - Second stage boot loader.
- `/boot/s3boot` - Third stage boot loader.
- `/boot/bdd.ko` (optional) - The disk driver for the boot drive. The exact file depends on the type of boot drive.
- `/lib/disk/` (optional) - A directory containing additional disk drivers.
- `/lib/fs/` (optional) - A directory containing additional file system drivers.
- `/lib/boot/` (optional) - A directory containing additional boot modules.


## Manual installation
### Create partition
Create a new partition formatted using the FAT16 file system and with at least 4MB space.

After that, copy the files `s2boot`, `s3boot` and the appropriate disk driver from the directory where you built *sxboot* or
from a pre-built package to a directory called `/boot` on the partition. If you have an AHCI (SATA) drive, for example, copy the disk driver
called `ahci.ko` to `/boot/bdd.ko`.

Create the configuration file at `/config/config.cfg` (See [Configuration File](Configuration_File) for more details).

Finally, you can add additional drivers under `/lib/`.

### x86 BIOS systems: Copy early stage boot files
The early stage boot files will be written directly to the hard drive. You will need a hex editor or other tools to write to specific sectors of the hard drive.

#### MBR-formatted hard drive
*s0boot* contains the Master Boot Record code and will be written to the first sector of the hard drive. Make sure you only write the
first 440 bytes to not overwrite the partition table.

The second sector contains information for *s0boot* about how to load *s1boot*. This sector starts with an 8-byte signature: "TSXBOOT2" in ASCII (`54 53 58 42 4F 4F 54 32` in Hex).
The next two bytes are a 0-based sector index of where *s1boot* starts (usually 2) and the length in sectors of *s1boot*, which must be at least 20 (0x14).
The next byte is the 0-based partition number of the *sxboot* partition.

Finally, copy *s1boot* to the hard drive starting at the sector written after the "TSXBOOT2" signature.

#### GPT-formatted hard drive
Installation on a GPT drive is probably even harder than on MBR drives.

*s0boot* is written the same way as on a MBR-formatted drive.
Because the previously unused area after the MBR is now used by the GPT partition table, *s1boot* is now written to the sxboot partition.
To make sure there is enough space you will need to reserve enough sectors before the start of the File Allocation Tables.
If your tool for creating partitions does not support this, you will need to move all FAT structures manually (which is very tedious).

Before you copy *s1boot*, you will also need to copy *sGboot* to the Volume Boot Record (the first sector of the partition).
Make sure you only write the first 3 bytes and bytes 0x2B to 0x200 (end) to not overwrite the FAT parameter block.

The next sector of the partition will contain a similar structure as on MBR drives on the second sector. It starts with an 8-byte signature: "TSXBOOT3" (`54 53 58 42 4F 4F 54 33` in Hex).
The next values are:
- a 4-byte 0-based number containing the start sector of *s1boot* **relative to the start of the partition** (usually 2)
- a 2-byte value containing the length in sectors of *s1boot* (same as the value on MBR drives)
- a 2-byte value containing the 0-based partition number of the *sxboot* partition

Finally, copy *s1boot* to the hard drive starting at the specified sector inside the partition.

### x86 UEFI systems: Copy EFI loader
Installation on UEFI systems is relatively easy compared to BIOS systems, because no files will be written directly to the hard drive, therefore not requiring tools like hex editors.

You will only need to copy *sUboot* to the EFI system partition as an EFI loader. The default location of the EFI loader on the EFI system partition is `/efi/boot/bootx64.efi` (amd64)
or `/efi/boot/bootia32.efi` (i386).
*sUboot* will then probe all drives to find *s2boot* and load it.

Note that you will need to disable secure boot on your computer to run *sUboot*.
