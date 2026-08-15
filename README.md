# BIP39 Center Punch Washer Backup

A DIY metal-backup method for a 12-word BIP39 English mnemonic using M8 stainless-steel washers and a center punch.

The design stores one mnemonic word per washer face as a human-readable decimal number encoded with four-point braille-style digits. A printable full-scale punching jig is used to position the punch marks.

> **Status:** Experimental / work in progress. Do not use this as the only backup of real funds until you have independently verified the specification, printed dimensions, punching process, and recovery procedure.

## Design goals

- Use inexpensive, widely available M8 stainless-steel washers.
- Avoid manual 11-bit binary conversion during backup and recovery.
- Keep recovery possible with printed reference material and no special software.
- Preserve word order even if washers become separated.
- Make the start position visually identifiable.
- Add a simple per-word error-detection check that can be verified by hand.
- Keep the printable jig free of seed-specific secret information.

## Current physical format

The current reference design targets:

- DIN 9021-style M8 flat washer
- SUS304 / A2 stainless steel
- Outside diameter: **24.0 mm**
- Inside diameter: **8.4 mm**
- Thickness: **2.0 mm**
- Both faces are used
- 1 face = 1 mnemonic word
- 1 washer = 2 mnemonic words
- 12-word mnemonic = 6 washers

## Data stored on each face

Read the face from the **START/SET marker clockwise**:

```text
START/SET | order (2 digits) | BIP39 word number (4 digits) | CHECK
```

This is an 8-block layout:

1. START/SET
2. order tens digit
3. order ones digit
4. BIP39 thousands digit
5. BIP39 hundreds digit
6. BIP39 tens digit
7. BIP39 ones digit
8. CHECK digit

### Order

The mnemonic position is stored as two decimal digits:

```text
01 ... 12
```

### BIP39 word number

This project currently uses a **1-based human-facing numbering scheme** for the BIP39 English word list:

```text
0001 = abandon
0002 = ability
...
2048 = zoo
```

This is intentionally different from the 0-based 11-bit index used internally by BIP39 implementations.

## Four-point digit encoding

Digits use the shape of standard braille numerals, restricted to the upper four dots. No numeric indicator is stored because the field is defined to contain digits only.

For this project the four candidate positions are numbered:

```text
1   3   outside
2   4   inside
```

Only the numbering of the candidate points is project-specific; the shapes of digits 0–9 follow standard braille numerals.

## START/SET marker

The start marker and set identifier share one compact symbol.

Two fixed marks are always present on the center radial line:

- outer fixed point: **r = 10.0 mm**
- inner fixed point: **r = 6.4 mm**

Two SET candidate points sit between them:

- left SET candidate: **r = 8.4 mm, -10°**
- right SET candidate: **r = 8.4 mm, +10°**

Current standard use:

- **A = right SET point only**
- **B = left SET point only**
- both SET points = reserved / invalid by default
- neither SET point = reserved / invalid by default

The reserved states may be assigned by a derivative implementation, but are not part of the current A/B standard.

## CHECK digit

CHECK is a single decimal digit calculated with simple mod 10.

Only the four digits of the BIP39 word number are included. The SET identifier and order are not included.

Example:

```text
BIP39 number: 2048
2 + 0 + 4 + 8 = 14
CHECK = 6
14 + 6 = 20
```

During recovery, the four BIP39 digits plus CHECK should sum to a multiple of 10.

CHECK is for **error detection, not error correction**. A single digit substitution is detected, but some combinations of multiple errors can cancel each other out.

## Punching workflow

The current workflow uses a full-scale paper jig and punches directly through the paper with a center punch.

1. Print the jig at **100% / actual size / no scaling**.
2. Verify the printed scale using the 50 mm reference line.
3. Align the jig with the M8 washer.
4. Punch only the candidate positions required by the data.
5. Remove the jig and visually verify the result.
6. Repeat on the opposite face.

The jig itself contains all candidate positions and therefore does not need to contain seed-specific secret data.

## Recovery overview

1. Face the washer side being read toward you.
2. Find the START/SET marker.
3. Determine A or B from the left/right SET mark.
4. Read clockwise.
5. Decode the two-digit order.
6. Decode the four-digit BIP39 word number.
7. Decode CHECK and verify mod 10.
8. Convert the 1-based number to the BIP39 English word.
9. Arrange all faces by order `01` through `12`.
10. Independently verify the recovered mnemonic before using it.

## Documentation

- [Specification](docs/specification.md)
- [Recovery guide](docs/recovery.md)
- [Japanese README](README_ja.md)

## Important limitations

- This is a physical backup format, not encryption.
- Anyone who can read all required washer faces can recover the mnemonic.
- The mod-10 CHECK does not detect every possible multi-digit error.
- SET and order are intentionally not covered by the per-face CHECK.
- Printable dimensions must be independently verified before punching real backup material.
- The current reference design is specifically for a 12-word BIP39 English mnemonic and the stated washer dimensions.

## License

License is not yet selected.
