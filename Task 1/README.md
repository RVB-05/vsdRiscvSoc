1. Source File:
[unique_test.c](https://github.com/RVB-05/vsdRiscvSoc/blob/main/unique_test.c)

2. Compile command used:
```bash
riscv64-unknown-elf-gcc -O2 -Wall -march=rv64imac -mabi=lp64 -DUSERNAME=\"$(id -un)\" -DHOSTNAME=\"$(hostname -s)\" unique_test.c -o unique_test
```
3. Program Output:
```
bbl loader
RISC-V Uniqueness Check
User: bhagavathy
Host: bhagavathy-VirtualBox
UniqueID: 0x34e88d61ce36574e
GCC_VLEN: 5

```
