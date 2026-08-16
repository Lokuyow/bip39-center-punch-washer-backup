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

## Read one washer face

1. Turn the face you want to read toward you.
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
- compare with the opposite face and other washers for contextual clues,
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

1. Read all washer faces.
2. Sort them by order `01` through `12`.
3. Convert every BIP39 number to its English word.
4. Review the complete 12-word sequence.
5. Independently validate the recovered mnemonic before moving funds or relying on it.

Useful independent checks can include the BIP39 checksum and known non-secret wallet information such as an expected wallet fingerprint or previously recorded receive address.

## Reverse face rule

Each side is read independently.

When reading the reverse face, turn that reverse face toward you and again read clockwise from its own START/SET marker. Do not mirror the layout mentally from the front face.

## Security note

The washers contain the mnemonic in encoded but not encrypted form. Anyone who can correctly read all required faces can recover the mnemonic.
