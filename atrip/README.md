# atrip — Retrocomputing disk image library & CLI

Python library and command-line tool for parsing, reading, writing, and
manipulating retrocomputing disk images, primarily for **Atari 8-bit** and
**Apple ][** platforms. This is the disk-image backend for
[Omnivore](https://github.com/robmcmullen/omnivore), a reverse-engineering
toolbox for 8-bit computers.

## Install

```bash
pip install atrip
```

**Note:** Requires [numpy](https://numpy.org/). Some features (assembler,
disassembler) use C extensions built from Cython and C sources in this repo.

## Usage

```bash
atrip list mydisk.atr          # list files on disk image
atrip extract mydisk.atr -a    # extract all files
atrip add mydisk.atr program.xex
atrip create newdisk.atr dos2sd
```

Commands: `list` (`ls`, `dir`), `extract` (`x`), `add` (`a`), `create` (`c`),
`delete` (`rm`), `assemble` (`asm`), `boot`, `crc`, `vtoc`, `segments`, `menu`.

## Supported formats

| Media | Filesystems |
|-------|-------------|
| ATR, XFD, DCM, CAS (Atari) | Atari DOS 2 (SD/DD/ED), DOS 2.5, SpartaDOS (partial), MyDOS (partial) |
| DSK (Apple) | Apple DOS 3.3 |
| CAR, ROM (cartridge) | Atari 2600/5200/800, Vectrex signatures |

Compression: gzip, bzip2, lzma/xz, lz4, unix compress (.Z), DCM, zlib.
Archives: zip, tar.

## License

Mozilla Public License 2.0

## Author

Rob McMullen — <feedback@playermissile.com>
