# Build From Source

## Prerequisites

Before you build *sxboot* from source, make sure you have the following programs installed and in your PATH:
- LLVM (`clang` and `ld.lld`), version 10 *recommended*
- `nasm` Assembler, version 2.x
- GNU `make`
- `git`, to download the source code

It is recommended you build on Unix(-like) operating systems.

### Preparation on FreeBSD

On FreeBSD, LLVM comes pre-installed. To install the additional programs, run:
```bash
pkg update
pkg install nasm gmake git
```
**Note:** Because GNU make will be installed as `gmake`, you will need to use the `gmake` command instead of `make` in the next steps.  
**Note:** You will not be able to build *sUboot* unless you install `lld-link`, which is not included in the base system.

### Preparation on Debian
Install all required packages:
```bash
apt-get update
apt-get install -y clang lld make nasm
```
**Note:** To install newer versions of LLVM, see [apt.llvm.org](https://apt.llvm.org/).


## Build Steps
Download the source code and compile everything using default settings:
```bash
git --recurse-submodules clone https://git.omegazero.org/sxboot/core.git
cd core
make world
```
More build options are listed below.

Temporary files and the executable images will be stored at `bin/` by default, relative to where the build is started,
meaning if you clone the repository to `/usr/local/core` and run `make` there, the files will be written to `/usr/local/core/bin/[...]`.


## Build Options
### make Targets
- `root` - Compile only the essential components.
- `modules` - Compile only modules. Modules include: disk drivers, additional file system drivers, boot modules.
- `world` - Compile everything (`root` and `modules`).
- `clean` - Delete intermediate object files.

### Arguments
- `ARCH` - Target architecture. Valid options are `amd64` and `i386`. **Default:** `amd64`
- `BINDIR` - Where to write the resulting executable files. **Default:** `bin/ARCH` (ARCH is replaced by the name of the architecture)
- `WINDOWS` - Set to `yes` if you are building on Windows. **Default:** (none)
