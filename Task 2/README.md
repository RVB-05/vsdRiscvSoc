This repository contains RISC-V programs demonstrating basic algorithms (factorial, max array, bubble sort, bit operations), each including a unique build header to print runtime metadata.

## Use of unique.h
The `unique.h` header embeds build- and run-time unique IDs into each program to verify build integrity and runtime uniqueness.
All compile commands use `unique.h` via `#include "unique.h"` in the source files to embed the metadata.

## Setup of Unique Identity Variables
Before compiling the programs, set environment variables to embed unique system and build metadata:

```bash
export U=$(id -un)
export H=$(hostname -s)
export M=$(head -c 16 /etc/machine-id)
export T=$(date -u +%Y-%m-%dT%H:%M:%SZ)
export E=$(date +%s)
```

## Spike Version / Info
Since `spike --version` is not supported, here is the output of `spike --help`:

```
Spike RISC-V ISA Simulator 1.1.1-dev

usage: spike [host options] <target program> [target options]
Host Options:
  -p<n>                 Simulate <n> processors [default 1]
  -m<n>                 Provide <n> MiB of target memory [default 2048]
  -m<a:m,b:n,...>       Provide memory regions of size m and n bytes
                          at base addresses a and b (with 4 KiB alignment)
  -d                    Interactive debug mode
  -g                    Track histogram of PCs
  -l                    Generate a log of execution
  -h, --help            Print this help message
  --halted              Start halted, allowing a debugger to connect
  --log=<name>          File name for option -l
  --debug-cmd=<name>    Read commands from file (use with -d)
  --isa=<name>          RISC-V ISA string [default rv64imafdc_zicntr_zihpm]
  --pmpregions=<n>      Number of PMP regions [default 16]
  --pmpgranularity=<n>  PMP Granularity in bytes [default 4]
  --priv=<m|mu|msu>     RISC-V privilege modes supported [default MSU]
  --pc=<address>        Override ELF entry point
  --hartids=<a,b,...>   Explicitly specify hartids, default is 0,1,...
  --ic=<S>:<W>:<B>      Instantiate a cache model with S sets,
  --dc=<S>:<W>:<B>        W ways, and B-byte blocks (with S and
  --l2=<S>:<W>:<B>        B both powers of 2).
  --big-endian          Use a big-endian memory system.
  --misaligned          Support misaligned memory accesses
  --device=<name>       Attach MMIO plugin device from an --extlib library,
                          specify --device=<name>,<args> to pass down extra args.
  --log-cache-miss      Generate a log of cache miss
  --log-commits         Generate a log of commits info
  --extension=<name>    Specify RoCC Extension
                          This flag can be used multiple times.
  --extlib=<name>       Shared library to load
                        This flag can be used multiple times.
  --rbb-port=<port>     Listen on <port> for remote bitbang connection
  --dump-dts            Print device tree string and exit
  --dtb=<path>          Use specified device tree blob [default: auto-generate]
  --disable-dtb         Don't write the device tree blob into memory
  --kernel=<path>       Load kernel flat image into memory
  --initrd=<path>       Load kernel initrd into memory
  --bootargs=<args>     Provide custom bootargs for kernel [default: console=ttyS0 earlycon]
  --real-time-clint     Increment clint time at real-time rate
  --triggers=<n>        Number of supported triggers [default 4]
  --dm-progsize=<words> Progsize for the debug module [default 2]
  --dm-sba=<bits>       Debug system bus access supports up to <bits> wide accesses [default 0]
  --dm-auth             Debug module requires debugger to authenticate
  --dmi-rti=<n>         Number of Run-Test/Idle cycles required for a DMI access [default 0]
  --dm-abstract-rti=<n> Number of Run-Test/Idle cycles required for an abstract command to execute [default 0]
  --dm-no-hasel         Debug module won't support hasel
  --dm-no-abstract-csr  Debug module won't support abstract CSR access
  --dm-no-abstract-fpr  Debug module won't support abstract FPR access
  --dm-no-halt-groups   Debug module won't support halt groups
  --dm-no-impebreak     Debug module won't support implicit ebreak in program buffer
  --blocksz=<size>      Cache block size (B) for CMO operations(powers of 2) [default 64]
  --instructions=<n>    Stop after n instructions
```

## Running programs on Spike

Programs were executed using the RISC-V simulator Spike with the proxy kernel (`pk`):

```bash
spike pk ./factorial | tee factorial_output.txt
spike pk ./max_array | tee max_array_output.txt
spike pk ./bitops | tee bitops_output.txt
spike pk ./bubble_sort | tee bubble_sort_output.txt
```

## riscv64-unknown-elf-gcc Version

```
Using built-in specs.
COLLECT_GCC=riscv64-unknown-elf-gcc
COLLECT_LTO_WRAPPER=/home/bhagavathy/riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14/bin/../libexec/gcc/riscv64-unknown-elf/8.3.0/lto-wrapper
Target: riscv64-unknown-elf
Configured with: /scratch/carsteng/freedom-tools-master/obj/x86_64-linux-ubuntu14/build/riscv-gnu-toolchain/riscv-gcc/configure --target=riscv64-unknown-elf --host=x86_64-linux-gnu --prefix=/scratch/carsteng/freedom-tools-master/obj/x86_64-linux-ubuntu14/install/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14 --with-pkgversion='SiFive GCC 8.3.0-2019.08.0' --with-bugurl=https://github.com/sifive/freedom-tools/issues --disable-shared --disable-threads --enable-languages=c,c++ --enable-tls --with-newlib --with-sysroot=/scratch/carsteng/freedom-tools-master/obj/x86_64-linux-ubuntu14/install/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14/riscv64-unknown-elf --with-native-system-header-dir=/include --disable-libmudflap --disable-libssp --disable-libquadmath --disable-libgomp --disable-nls --disable-tm-clone-registry --src=../riscv-gcc --without-system-zlib --enable-checking=yes --enable-multilib --with-abi=lp64d --with-arch=rv64imafdc CFLAGS=-O2 CXXFLAGS=-O2 'CFLAGS_FOR_TARGET=-Os  -mcmodel=medany' 'CXXFLAGS_FOR_TARGET=-Os  -mcmodel=medany'
Thread model: single
gcc version 8.3.0 (SiFive GCC 8.3.0-2019.08.0)
```

## Compile Commands

```bash
riscv64-unknown-elf-gcc -O0 -g -march=rv64imac -mabi=lp64 \
-DUSERNAME="\"$U\"" -DHOSTNAME="\"$H\"" -DMACHINE_ID="\"$M\"" \
-DBUILD_UTC="\"$T\"" -DBUILD_EPOCH=$E \
factorial.c -o factorial

riscv64-unknown-elf-gcc -O0 -g -march=rv64imac -mabi=lp64 \
-DUSERNAME="\"$U\"" -DHOSTNAME="\"$H\"" -DMACHINE_ID="\"$M\"" \
-DBUILD_UTC="\"$T\"" -DBUILD_EPOCH=$E \
max_array.c -o max_array

riscv64-unknown-elf-gcc -O0 -g -march=rv64imac -mabi=lp64 \
-DUSERNAME="\"$U\"" -DHOSTNAME="\"$H\"" -DMACHINE_ID="\"$M\"" \
-DBUILD_UTC="\"$T\"" -DBUILD_EPOCH=$E \
bubble_sort.c -o bubble_sort

riscv64-unknown-elf-gcc -O0 -g -march=rv64imac -mabi=lp64 \
-DUSERNAME="\"$U\"" -DHOSTNAME="\"$H\"" -DMACHINE_ID="\"$M\"" \
-DBUILD_UTC="\"$T\"" -DBUILD_EPOCH=$E \
bitops.c -o bitops
```

## Disassembly and Instruction Decoding
Each program’s main section was disassembled using:

```bash
riscv64-unknown-elf-objdump -d ./<program> | sed -n '/<main>:/,/^$/p' > <program>_main_objdump.txt
```

## Output Verification
Program outputs, including the ProofID and RunID, were captured using `tee` for each run, e.g.:

```bash
spike pk ./factorial | tee factorial_output.txt
```

## ProofID and RunID Confirmation
Each program prints ProofID and RunID at runtime. These IDs confirm:

  - ProofID is unique per user, host, machine, and build timestamp.

  - RunID adds per-run randomness using current time.

These IDs are visible in all submitted output screenshots.
