# Boot Types

It is recommended you have read [Configuration File](Configuration_File) before reading this.


The boot type for a menu entry specifies how the operating system should be loaded.

*s2boot* supports a few built-in boot types; others will be available through modules loaded at runtime.


## Native Boot Types
These boot types are directly built into the *s2boot* executable due to their low complexity.

### chain
Chainloads a VBR of a partition and drive given by the `partition` and `drive` arguments.

#### Parameters
- `partition` - Specifies which partition to use. Must be below 256.
- `drive` (optional) - Specifies which drive to use. Drive identifiers are made up of two parts: an up to four letter drive type name (eg `ahci`), followed by a 0-based drive number. Example: `ahci0`. Defaults to the boot drive.

### binary
Loads a file given by `file` at the memory address of `destination`. The boot loader will jump to *destination + offset* in either 16, 32, or 64 bit mode (set by argument `bits`).

#### Parameters
- `file` - Absolute path to the file to be loaded. See **File path format** in [Configuration File](Configuration_File).
- `destination` - Specifies the memory address where to load the file. Must be in hexadecimal format (*0xhhh...*).
- `offset` - Specifies an entry point offset. Must be in hexadecimal format.
- `bits` - Specifies in which mode to start the file. Must be 16, 32 or 64.

### mbr
Similar to *binary*, except that `destination` and `bits` are preset to `0x7c00` (on x86) and `16`, respectively.

#### Parameters
- `file` - Absolute path to the MBR file. Must be 512 bytes in size.

### image
Loads an ELF file given by `file`, sets it up according to the ELF program header and jumps to the entry point. Must be a 32-bit executable.

#### Parameters
- `file` - Absolute path to the ELF file.


## External Boot Types
These boot types are available through modules loaded during runtime. They are available in the *sxboot/modules* repository, along with other modules.

### ubi
The *Universal Boot Interface*.

#### Parameters
- `file` - Absolute path to the kernel file.

### linux86
The Linux x86 boot protocol (x86-BIOS-only).

#### Parameters
- `kernel` - Absolute path to the kernel file.
- `initrd` - Absolute path to the initrd file.
- `args` - The command line passed to the kernel.
