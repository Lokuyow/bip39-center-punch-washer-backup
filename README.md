# Washer Punch39

**BIP39 Center-Punch Washer Backup**

[日本語版 README](README_ja.md)

A DIY metal-backup method for a [BIP39](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki) English mnemonic using stainless-steel washers and a center punch.

**Format version: v1**

<p align="center">
  <img src="images/washer-punch39-jig-preview.png" alt="Washer Punch39 punching jig preview" height="220">
  <img src="images/photos/washer-punch39-punched-washer.jpg" alt="Punched Washer Punch39 washer" height="220">
  <img src="images/photos/washer-punch39-assembled-stack.jpg" alt="Washer Punch39 washer stack secured with a double nut" height="220">
</p>

Each marked washer face stores one mnemonic word as a four-digit decimal BIP39 word number encoded with four-point braille-style digits. A printable full-scale paper jig positions the punch marks.

> **Status:** Experimental / work in progress. Do not use this as the only backup of real funds until you have independently verified the specification, printed dimensions, punching process, and recovery procedure.

> **About the photos:** Some physical-example photos were made with a jig revision from before the current v1 geometry update. For exact current dimensions, use the [specification](docs/specification.md) and [punching-jig PDF](pdf/bip39-washer-punching-jig-m8-a4.pdf).

## Features

- Uses inexpensive, widely available stainless-steel washers
- Can be recovered from printed reference material without special software
- Preserves mnemonic order even if washers become separated
- Includes a simple error-detection check that can be verified by hand
- Keeps the printable jig free of seed-specific secret information

## How it works

Read each marked face **clockwise from the START/SET marker**:

```text
START/SET | order (2 digits) | BIP39 word number (4 digits) | CHECK
```

- **SET** — identifies different mnemonics when more than one is stored; standard use is A=right, B=left
- **Order** — `01` through `12` in the current 12-word reference implementation
- **BIP39 word number** — the BIP39 English list numbered from `0001` through `2048`
- **CHECK** — simple mod 10 calculated from the four BIP39 digits only

Digits use the upper four dots of standard braille numerals. The numeric indicator is omitted because the fields are numeric-only.

| 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
| :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: |
| ⠚ | ⠁ | ⠃ | ⠉ | ⠙ | ⠑ | ⠋ | ⠛ | ⠓ | ⠊ |

See the [specification](docs/specification.md) for exact block angles, candidate-point coordinates, digit-cell dimensions, and other geometry.

## Current reference implementation

The printable reference material in this repository targets:

- DIN 9021-style M8 flat washer
- SUS304 / A2 stainless steel
- **24.0 mm OD / 8.4 mm ID / 2.0 mm thickness**
- 1 marked face = 1 mnemonic word
- **12-word mnemonic = 7 washers**

In the seven-washer reference assembly, the blank faces of the first and seventh washers are placed outward so punch marks are not directly visible while assembled. The stack is secured on a bolt with a **double-nut arrangement**.

The seven-washer arrangement and washer dimensions are part of the current reference implementation, not requirements of the underlying encoding method.

## Punching workflow

1. Print the [punching-jig PDF](pdf/bip39-washer-punching-jig-m8-a4.pdf) at **100% / actual size / no scaling**.
2. Verify the print scale using the 50 mm reference line.
3. Align and secure the paper jig to the washer.
4. Punch only the required candidate positions directly through the paper, then remove the jig and inspect the result.
5. Repeat on the opposite face when needed.

See the [recovery guide](docs/recovery.md) for the recovery procedure.

## Printable PDFs

- [Quick reference (Japanese)](pdf/bip39-washer-quick-reference-a4.pdf)
- [Punching jig (Japanese)](pdf/bip39-washer-punching-jig-m8-a4.pdf)
- [BIP39 English word list (1-based)](pdf/bip39-english-wordlist-1based-a4.pdf)

## Documentation

- [Specification](docs/specification.md)
- [Recovery guide](docs/recovery.md)
- [Design rationale](docs/design-rationale.md)
- [Physical example](docs/physical-example.md)
- [Heat and quench test (supplemental)](docs/fire-test.md)

## Important limitations

- This is a physical backup format, not encryption. Anyone who can read all required washer faces can recover the mnemonic.
- **Washer Punch39 v1 stores the BIP39 mnemonic only.** If you use a BIP39 passphrase, back it up separately.
- The mod-10 CHECK is for error detection, not error correction, and does not detect every possible multi-digit error.
- SET and order are intentionally excluded from the per-face CHECK.
- Verify the printed scale and punching/recovery procedure before creating a real backup.
- If adapting the method to another washer size, word count, layout, or tool, regenerate and verify the corresponding geometry and recovery rules.

## License

This project is licensed under the [MIT License](LICENSE).
