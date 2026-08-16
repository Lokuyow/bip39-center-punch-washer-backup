# Specification

[日本語](specification_ja.md)

This document records the current reference format for BIP39 Center Punch Washer Backup.

## Scope

The method itself is intended to be adaptable. The current reference implementation uses:

- BIP39 English
- a 12-word mnemonic as the reference example
- DIN 9021-style M8 washer
- SUS304 / A2 stainless steel
- 24.0 mm OD
- 8.4 mm ID
- 2.0 mm thickness
- 7 washers in the current 12-word reference assembly

These choices define the printable reference material and reference assembly in this repository; they are not fundamental requirements of the underlying method. Different washer dimensions, word counts, layouts, tooling, or stacking arrangements require corresponding geometry and recovery rules to be regenerated and independently verified where applicable.

## Reference 12-word assembly

The current reference assembly uses seven washers for twelve mnemonic words:

- washer 1: one marked face, one blank face
- washers 2–6: both faces marked
- washer 7: one marked face, one blank face

This provides twelve marked faces in total.

When the washers are stacked on a bolt, the blank faces of washers 1 and 7 are placed outward. In the assembled state, the braille-style punch marks are therefore not directly visible from the outside.

This seven-washer arrangement is optional and is not part of the encoding requirement. A builder may use another physical arrangement. The blank outer faces provide visual concealment only and should not be treated as encryption or a security boundary.

## Face structure

Read each marked face from the START/SET marker clockwise.

```text
START/SET | order tens | order ones | BIP39 thousands | hundreds | tens | ones | CHECK
```

There are eight logical blocks in the current reference layout.

## START/SET geometry

The START/SET marker occupies the 0° block in the current reference layout.

Fixed points:

- outer fixed point: radius 10.0 mm, angle 0°
- inner fixed point: radius 6.4 mm, angle 0°

SET candidates:

- left: radius 8.4 mm, angle -10° / 350°
- right: radius 8.4 mm, angle +10°

Standard assignments:

- A = right SET candidate marked
- B = left SET candidate marked
- both = reserved / invalid by default
- neither = reserved / invalid by default

## Digit cells

Each decimal digit is represented by four candidate points arranged as a compact square-like cell on the washer.

Reference candidate radii:

- inner candidate-center radius: 7.2 mm
- outer candidate-center radius: 9.2 mm

Project-local point numbering:

```text
1   3   outside
2   4   inside
```

The dot shapes for decimal digits 0–9 are those of standard braille numerals using the upper four dots. The numeric indicator is omitted because the field is defined as numeric.

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
- 50 mm scale-verification line
- 100% / actual-size / no-scaling printing

The jig contains all candidate points, not secret-specific selections.

## Punching method

The current reference workflow punches directly through the paper jig with a center punch. An automatic center punch is convenient, but the data format does not require an automatic mechanism.

## Status

This specification is still experimental. Physical testing, dimensional verification, and independent review are expected before treating it as stable.
