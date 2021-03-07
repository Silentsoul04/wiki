## Common tools

- EasyRecovery
- MedAnalyze
- FTK
- Elcomsoft Forensic Disk Decryptor
- Volatility

## Computer disk

Common disk partition formats are as follows:

- Windows: FAT12 -> FAT16 -> FAT32 -> NTFS
- Linux: EXT2 -> EXT3 -> EXT4
- FAT master disk structure
- Delete file: The first byte of the file name in the directory table is `e5`.

## VMDK

The VMDK file is essentially a virtual version of the physical hard disk. There are also similar fill areas in the
partitions and sectors of the physical hard disk. We can use these filled areas to hide the data we need to hide, to
avoid hiding. The files increase the size of the VMDK file (such as attaching directly to the file backend), and can
also avoid virtual machine errors caused by changes in the size of the VMDK file. VMDK files are generally large and
suitable for hiding large files.

## RAM

- Resolve Windows / Linux / Mac OS X memory structure
- Analysis process, in-memory data
- Find clues and ideas based on the prompts, and extract specific memory data for the specified process

## Reference

- [Data Hiding Technology](<https://www.sciencedirect.com/topics/computer-science/data-hiding-technique>)