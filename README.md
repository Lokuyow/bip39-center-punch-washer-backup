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

## Features

- Uses inexpensive, widely available stainless-steel washers
- Can be recovered from printed reference material without special software
- Preserves mnemonic order even if washers become separated
- Includes a simple error-detection check that can be verified by hand
- Keeps the printable jig free of seed-specific secret information

## Advantages compared with other approaches

Washer Punch39 prioritizes **commodity materials, easy rework after mistakes, human-readable recovery, and recoverability after the washers are separated** rather than minimizing physical size or punch count.

- **No dedicated metal plate required** — It can be built from commonly available stainless-steel washers, a bolt, nuts, a center punch, and a printed paper jig. The method does not depend on continued availability of a proprietary plate.
- **Punching mistakes stay local** — If one word is punched incorrectly, only that washer needs to be replaced and remade. This avoids designs where a mistake on a single plate can require remaking a much larger portion of the backup.
- **Recoverable even when separated** — Every marked face carries SET, order, BIP39 word number, and CHECK, so recovery does not depend solely on the physical stacking order.
- **Human-friendly decimal encoding** — BIP39 words are stored as the decimal range `0001`–`2048`. No binary conversion is required, and the backup can be decoded and checked using printed reference material and pencil-and-paper arithmetic.
- **Per-word local checking** — Each face includes a simple mod-10 CHECK for its BIP39 word number, allowing a reader to detect a suspicious reading before the entire mnemonic has been reconstructed.
- **Easy to obscure for disposal** — Because the data is encoded by the presence or absence of candidate punch marks, random additional punches near the candidate positions can destroy the original pattern. Disposal does not require the jig or precise alignment.
- **Blank outer surfaces without an extra cover** — In the 12-word, seven-washer reference assembly, the first and last washers are punched on only one side and their blank faces are placed outward.

The tradeoff is that Washer Punch39 may use more parts and more punch marks than a single-plate design or a dense binary encoding. The design deliberately favors **availability, repairability, and human verification** over maximum compactness.

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

## Improvements over the earlier electro-etching design

Washer Punch39 evolves an earlier washer-based DIY metal wallet published by the author in 2022. The following reference articles are in Japanese:

- [激安！DIYメタルウォレット【電解エッチング】](https://spotlight.soy/detail?article_id=9hbcxftqd) (Japanese)
- [検証！DIYメタルウォレット【火あぶりの刑】](https://spotlight.soy/detail?article_id=ojr7i5w1k) (Japanese)

Main improvements:

- **Faster construction** — Electro-etching requires masking, handwriting, drying, etching, and cleanup; the author's 24-word build took about four hours. Washer Punch39 instead uses center-punch marks through a paper jig.
- **Less fine manual work** — Scratching handwritten letters through the masking layer requires dexterity. The current method only requires punching defined candidate positions.
- **Simpler tools and cleanup** — The workflow no longer needs batteries, alligator clips, etching solution, nail polish, or nail-polish remover.
- **More explicit encoding** — Instead of writing English words directly, Washer Punch39 records BIP39 word numbers with braille-style digits and defines SET, order, and CHECK as part of a fixed format.
- **Easier disposal** — Electro-etched text is cumbersome to make reliably unreadable. With Washer Punch39, the information is the presence or absence of candidate marks, so random extra punch marks near those positions can destroy the original pattern. No jig or precise alignment is required for disposal.
- **Double nut instead of a spring washer** — The earlier design used a spring washer, but its spring action was largely lost after the heat test. The current reference assembly uses two nuts tightened against each other.
- **Seven washers instead of eight for 12 words** — The earlier arrangement used six data washers plus blank washers on the front and back. The current arrangement makes the first and last washers single-sided and places their blank faces outward, reducing the total to seven washers.

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
