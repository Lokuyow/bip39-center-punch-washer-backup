# Specification

[日本語](specification_ja.md)

This document records the current reference format for Washer Punch39 (BIP39 Center-Punch Washer Backup).

## Format version

The current specification is **Washer Punch39 format draft v1**.

draft v1 is not final and may receive incompatible changes before v1 is finalized. After v1 is finalized, changes that alter how existing washers are interpreted should use a new format version.

The format name itself is not encoded on each washer face. If using draft v1, keep the non-secret identifier `Washer Punch39 draft v1` with the backup documentation.

## Scope

The published reference implementation in this repository targets a **12-word BIP39 English mnemonic**.

- BIP39 English
- DIN 9021-style M8 washer
- SUS304 / A2 stainless steel
- 24.0 mm OD
- 8.4 mm ID
- 2.0 mm thickness
- 7 washers for 12 words

Because order uses two decimal digits, the same encoding can also represent a 24-word mnemonic. A 24-word assembly is not part of the current published reference implementation.

These choices define the printable reference material and reference assembly in this repository; they are not fundamental requirements of the underlying method. Different washer dimensions, word counts, layouts, tooling, or stacking arrangements require corresponding geometry and recovery rules to be regenerated and independently verified where applicable.

## Reference 12-word assembly

The current reference assembly uses seven washers for twelve mnemonic words:

- washer 1: one marked face, one blank face
- washers 2–6: both faces marked
- washer 7: one marked face, one blank face

This provides twelve marked faces in total.

When the washers are stacked on a bolt, the blank faces of washers 1 and 7 are placed outward. In the assembled state, the braille-style punch marks are therefore not directly visible from the outside.

After assembly, the stack is secured with **two nuts tightened against each other as a double-nut arrangement** to reduce loosening during storage.

This seven-washer arrangement is optional and is not part of the encoding requirement. The person making the backup may use another physical arrangement. The blank outer faces provide visual concealment only and should not be treated as encryption or a security boundary.

## Face structure

Read each marked face from the START/SET marker clockwise.

```text
START/SET | order tens | order ones | BIP39 thousands | hundreds | tens | ones | CHECK
```

There are eight logical blocks in the current reference layout.

## Angular reference and block layout

With the marked face viewed straight on, define the **center radial line of START/SET as 0°**. Angles increase **clockwise**.

The logical block center angles are:

| Center angle | Field |
| :-: | --- |
| 0° | START/SET |
| 45° | order tens |
| 90° | order ones |
| 135° | BIP39 thousands |
| 180° | BIP39 hundreds |
| 225° | BIP39 tens |
| 270° | BIP39 ones |
| 315° | CHECK |

## Local coordinate system

The geometry of each block is defined in a local Cartesian coordinate system rotated to that block's center angle `θ`.

- `y` axis: radial direction from the washer center toward angle `θ`, positive outward
- `x` axis: tangential direction perpendicular to `y`, positive toward the clockwise side when viewed from the front
- origin: washer center

At `θ = 0°` for START/SET, the left side therefore has `x < 0` and the right side has `x > 0`. For digit blocks, the local coordinates below are rotated by the block center angle `θ` to place them on the washer.

## START/SET geometry

The START/SET marker occupies the 0° block in the current reference layout.

Fixed points:

- outer fixed point: `x = 0 mm`, `y = 10.0 mm`
- inner fixed point: `x = 0 mm`, `y = 6.4 mm`

SET candidates:

- left: `x = -1.5 mm`, `y = 8.2 mm`
- right: `x = +1.5 mm`, `y = 8.2 mm`

**SET A / B is an identifier used to distinguish different mnemonics when more than one mnemonic is stored.** It does not split one mnemonic into A and B; all faces belonging to one mnemonic use the same SET value.

Standard assignments:

- A = right SET candidate marked
- B = left SET candidate marked
- both = reserved / invalid by default
- neither = reserved / invalid by default

Published reference examples use SET A, but the maker may choose either A or B.

## Digit cells

Each decimal digit is represented by four candidate points forming a **2.0 mm × 2.0 mm square whose center lies on the block center radial line at radius 8.2 mm**. One pair of square sides is radial and the other pair is tangential.

For a digit block whose center angle is `θ`, place the four candidate points at these local coordinates, then rotate the whole cell by `θ`:

- outer, counterclockwise side: `x = -1.0 mm`, `y = 9.2 mm`
- outer, clockwise side: `x = +1.0 mm`, `y = 9.2 mm`
- inner, counterclockwise side: `x = -1.0 mm`, `y = 7.2 mm`
- inner, clockwise side: `x = +1.0 mm`, `y = 7.2 mm`

The cell center is therefore `x = 0`, `y = 8.2 mm`, and the candidate-point spacing is **2.0 mm** in both the radial and tangential directions.

The dot shapes for decimal digits 0–9 are those of standard braille numerals using the upper four dots. The numeric indicator is omitted because the field is defined as numeric.

When mapping a Unicode braille digit onto the washer cell, orient the **top row toward the outside of the washer, the bottom row toward the inside, the left column toward the counterclockwise side, and the right column toward the clockwise side**.

The same digit shapes can be represented with Unicode braille characters:

| 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
| :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: |
| ⠚ | ⠁ | ⠃ | ⠉ | ⠙ | ⠑ | ⠋ | ⠛ | ⠓ | ⠊ |

The Unicode braille characters above are a text representation of the punch patterns; the numeric indicator is omitted.

## Order field

In the current 12-word reference implementation, the mnemonic position is encoded as two decimal digits:

```text
01 through 12
```

The Japanese documentation uses the term `語順` consistently for this field.

## BIP39 word-number field

The current standard uses a 1-based numbering of the BIP39 English list:

```text
0001 = abandon
...
2048 = zoo
```

This is a project-level human-facing numbering convention. It is not the 0-based 11-bit index used internally in BIP39 implementations.

## CHECK field

CHECK is one decimal digit calculated from the four BIP39 word-number digits only.

The SET and order fields are excluded.

Choose CHECK so that:

```text
BIP39 digit 1 + digit 2 + digit 3 + digit 4 + CHECK
```

is divisible by 10.

Example:

```text
2048
2 + 0 + 4 + 8 = 14
CHECK = 6
14 + 6 = 20
```

CHECK detects any single decimal-digit substitution but is not an error-correcting code and does not detect every possible multi-error combination.

## Reading direction

For either marked side of a washer:

1. turn that side toward the reader,
2. locate START/SET,
3. read clockwise.

Do not interpret the reverse side as a mirrored continuation of the front side.

A blank outer face in the seven-washer reference assembly contains no data and is intentionally left unpunched.

## Paper jig

The current reference jig is designed for A4 printing with:

- 12 identical faces
- 3 columns × 4 rows
- digit cells centered at radius 8.2 mm as 2.0 mm squares
- SET candidates at `x = ±1.5 mm`, `y = 8.2 mm`
- 50 mm scale-verification line
- 100% / actual-size / no-scaling printing

The jig contains all candidate points, not secret-specific selections.

## Punching method

The current reference workflow punches directly through the paper jig with a center punch. An automatic center punch is convenient, but the data format does not require an automatic mechanism.

## BIP39 passphrase

Washer Punch39 format draft v1 stores the **BIP39 mnemonic**. A BIP39 passphrase is not part of this format. If a passphrase is used, it must be backed up separately.

## Status

draft v1 is the candidate specification for the future v1 release. Physical testing, dimensional verification, and independent review are still ongoing.
