# Recovery Guide

[日本語](recovery_ja.md)

This guide describes the current recovery procedure for the **Washer Punch39 format draft v1** reference 12-word BIP39 English format.

## Before you begin

Confirm that the backup uses **Washer Punch39 draft v1**, and use the matching quick reference, specification, and word list.

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

The seven-washer arrangement is optional; the person making the backup may have chosen a different physical arrangement.

## SET A / B

SET A / B is an **identifier used to distinguish different mnemonics when more than one mnemonic is stored**. It is not a scheme for splitting one mnemonic into A and B.

Published reference examples use A, but the maker may choose either A or B. All faces belonging to one mnemonic use the same SET value.

If multiple mnemonics are mixed together, group faces by SET before reconstructing the order within each SET.

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
2. If multiple SET values are present, separate the faces by SET.
3. Within each SET, sort the faces by order `01` through `12`.
4. Convert every BIP39 number to its English word.
5. Review the complete 12-word sequence.
6. Independently validate the recovered mnemonic before moving funds or relying on it.

Useful independent checks can include the BIP39 checksum and, **when all wallet conditions including any required BIP39 passphrase are known**, expected wallet information such as a wallet fingerprint or previously recorded receive address.

## BIP39 passphrase

Washer Punch39 format draft v1 stores the **BIP39 mnemonic**. A BIP39 passphrase is not stored on the washers.

Recovering a wallet that used a passphrase therefore also requires that passphrase from a separate backup. A correct mnemonic combined with a different passphrase derives a different wallet.

## Reverse face rule

Each marked side is read independently.

When reading a marked reverse face, turn that face toward you and again read clockwise from its own START/SET marker. Do not mirror the layout mentally from the front face.

In the seven-washer reference assembly, the two outer blank faces contain no encoded data.

## Security note

The washers contain the mnemonic in encoded but not encrypted form. Anyone who can correctly read all required faces can recover the mnemonic.

The blank outer faces only conceal the punch marks from direct outside view while the washers remain assembled. They are not a security boundary.
