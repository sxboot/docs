# sxboot

## Overview

**sxboot** consists of multiple stages:
- *s0boot* - Master boot record code.
- *sGboot* - Intermediate loader code between *s0boot* and *s1boot* on GPT drives.
- *s1boot* - Loaded by *s0boot*, sets up the environment and loads and starts *s2boot*.
- *sUboot* - The UEFI loader replacing *s1boot* and earlier stages on UEFI systems.
- *s2boot* - The main executable. Loads and parses the configuration file, provides a selection menu, and loads the kernel.
- *s3boot* - Does late environment setup before calling the kernel. Not used when *s2boot* calls the kernel directly.

*s2boot* contains and can load additional disk drivers, file system drivers and boot modules.

A file system driver for FAT16 is the only file system driver included in the *s2boot* executable.

Boot modules provide additional ways to load an operating system kernel.
*s2boot* natively supports chain loading, loading a 32-bit ELF image, and loading a binary blob.


## Contact

For support or suggestions, contact [user94729@omegazero.org](mailto:user94729@omegazero.org) / [@user94729:matrix.omegazero.org](https://matrix.to/#/@user94729:matrix.omegazero.org).

