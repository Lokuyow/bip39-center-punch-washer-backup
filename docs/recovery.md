# Recovery Guide

[日本語](recovery_ja.md)

This guide describes the current recovery procedure for the reference 12-word BIP39 English format.

## Before you begin

Use the quick-reference sheet for:

- START/SET interpretation
- digit dot patterns
- 1-based BIP39 word numbering
- mod-10 CHECK verification

Do not rely on memory alone when recovering a real backup.

## Reference seven-washer assembly

The current 12-word reference assembly uses seven washers. The first and seventh washers are marked on only one face, while washers 2–6 are marked on both faces.

When assembled on a bolt, the two intentionally blank faces are placed outward so that punch marks are not directly visible from the outside.

During recovery, do not treat those two blank outer faces as missing data. The reference assembly contains twelve marked faces in total.

The seven-washer arrangement is optional; another builder may have chosen a different physical arrangement.

## Read one washer face

1. Turn the marked face you want to read toward you.
2. Find the START/SET marker: two fixed marks on the center radial line with the SET position between them.
3. Determine the set:
   - right SET mark only = A
   - left SET mark only = B
   - both / neither = reserved or invalid in the current standard; investigate before continuing
4. From START/SET, read clockwise.
5. Decode the two decimal digits of the order field (`01`–`12`).
6. Decode the four decimal digits of the BIP39 word number (`0001`–`2048`).
7. Decode the final CHECK digit.

## Verify CHECK

Add the four BIP39 word-number digits and CHECK.

The result must be divisible by 10.

Example:

```text
2048 / CHECK 6
2 + 0 + 4 + 8 + 6 = 20
```

If CHECK fails:

- inspect the dot patterns again,
- check for corrosion, scratches, shallow punch marks, or false marks,
- compare with the opposite marked face and other washers for contextual clues,
- do not silently guess a replacement digit.

CHECK is an error detector, not an error corrector.

## Convert number to word

Use the project's 1-based BIP39 English table:

```text
0001 = abandon
0002 = ability
...
2048 = zoo
```

The project numbering is 1-based and should not be confused with the 0-based implementation index used by BIP39 software.

## Reconstruct all 12 words

1. Read all marked washer faces.
2. Sort them by order `01` through `12`.
3. Convert every BIP39 number to its English word.
4. Review the complete 12-word sequence.
5. Independently validate the recovered mnemonic before moving funds or relying on it.

Useful independent checks can include the BIP39 checksum and known non-secret wallet information such as an expected wallet fingerprint or previously recorded receive address.

## Reverse face rule

Each marked side is read independently.

When reading a marked reverse face, turn that face toward you and again read clockwise from its own START/SET marker. Do not mirror the layout mentally from the front face.

In the seven-washer reference assembly, the two outer blank faces contain no encoded data.

## Security note

The washers contain the mnemonic in encoded but not encrypted form. Anyone who can correctly read all required faces can recover the mnemonic.

The blank outer faces only conceal the punch marks from direct outside view while the washers remain assembled. They are not a security boundary.
