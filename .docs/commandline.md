# Command Line

The command line of *s2boot* can be accessed by pressing *C* in the boot menu.

Below is a list of the standard commands available in the command line. The command line additionally accepts several debugging commands not listed here.


## Builtin commands

### help
Prints a list of basic commands.

### menu
Returns to the boot menu.

### boot
Boots the menu entry given by the first argument, `0` being the first menu entry.

**First argument:** A 0-based index specifying which menu entry to boot.

### mem
Shows current memory usage.

### memmap
The memory allocation map printed out in a human readable format.

### vmemmap
A map of all virtual memory ranges mapped.

### bootdrive
Prints out the name of the boot drive.

### cat
Prints out the contents of a file.

**First argument:** The absolute path to the file that should be printed out. See **File path format** in [Configuration File](Configuration_File).

### ls
Prints out the files in a directory.

**First argument:** The absolute path to the directory of which the files should be listed.

### e820map (x86-only)
The memory map provided by BIOS interrupt 0x15, function 0xe820 ***or*** UEFI boot services if running on a UEFI system.
