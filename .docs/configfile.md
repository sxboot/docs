# Configuration File

The configuration file is loaded and parsed by *s2boot*. It must be located on the *sxboot* partition at `/config/config.cfg`


## Syntax
The configuration file is made up of key-value pairs separated by an *equals*-sign (`=`).
A key-value pair is terminated by a line break or a semicolon (`;`).

The configuration file may also contain 'subcategories', denoted by a key identifier followed by a name string in quotes and an opening brace `{`.
This subcategory may contain more key-value pairs followed by a closing brace `}`.

Comments start with a `#` at the beginning of a line, optionally with preceding whitespaces, and will cause the whole line to be ignored.


## Global configuration options
| Name | Type | Description | Default value |
| --- | --- | --- | --- |
| timeout | number | The number of seconds to show the menu without user interaction. If the user presses any key, this countdown will be stopped. | 10 |
| hdDrivers | string | Directory to load additional hard disk drivers from on the *sxboot* partition. | /lib/disk/ |
| fsDrivers | string | Directory to load additional file system drivers from on the *sxboot* partition. | /lib/fs/ |
| serialBaud | number | The serial port speed (baud). Must be one of the standard serial port speeds supported by the computer. | 9600 |
| fontScale | number | Font scale in graphics mode. Must be a value between *0.5* and *3*. There must be at most a single digit after the dot. | 1.0 |


## Menu entry options
A menu entry starts with the keyword `entry`, followed by the name in quotes and an opening brace.
It then contains the arguments listed below and must end with a closing brace.

All arguments are passed to the boot handler. See the respective documentation for additional arguments.

| Name | Type | Description | Default value |
| --- | --- | --- | --- |
| type | string | A string identifier of the boot procedure to use. May be anything available in the boot modules directory (`/lib/boot/` by default). For example, if `type` is "ubi", *s2boot* will attempt to load `/lib/boot/ubi.ko` and start the module or show an error if the file does not exist. Available boot types are listed in [Boot Types](Boot_Types). | `-` (required) |


## File path format
File paths have this general format: `/(driveType)(driveNum).(partitionNum)/(path to file)`.

- `driveType` - The drive type, for example `ahci`. Up to four letters long.
- `driveNum` - A 0-based drive number. Must be below 255.
- `partitionNum` - A 0-based partition number of the partition where the is located. Must be 255 or lower.
- `path to file` - The path to the file on the partition, for example `config/config.cfg`.

**Example file path:** `/ahci0.0/config/config.cfg`


## Variables
File paths may contain variables. Variables can be accessed using `[(variable name)]`. The following variables are currently available:
- `DRIVETYPE` - The boot drive type.
- `DRIVENUM` - The boot drive number.
- `DRIVE` - The full boot drive identifier. Same as `[DRIVETYPE][DRIVENUM]`.
- `PART` - The boot partition number.
- `BDRIVE` - The full boot drive and partition identifier. Same as `[DRIVE].[PART]`.


## Example configuration file

```
# Set menu timeout to 5 seconds
timeout = 5
entry "ELF file" {
	type = image
	# Load file /boot/kernel from the second partition of the boot drive
	file = /[DRIVE].1/boot/kernel
}
```
