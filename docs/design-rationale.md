# Design Rationale

[日本語](design-rationale_ja.md)

This document explains why the current reference format looks the way it does. The project is experimental, so these decisions may change after physical testing or review.

## Why washers?

Stainless-steel washers are inexpensive, widely available, heat resistant compared with paper, and easy to stack on a bolt. Both faces can be used, allowing compact storage of mnemonic words.

The current reference implementation uses large M8 washers and demonstrates a 12-word mnemonic with a seven-washer assembly, but neither M8, 12 words, nor seven washers is a fundamental requirement of the method.

## Why seven washers for 12 words?

Twelve words require twelve marked faces, so they could be placed on six washers if every washer used both faces.

The current reference assembly intentionally uses seven washers instead:

- washer 1 carries one word on one face,
- washers 2–6 carry two words each,
- washer 7 carries one word on one face.

The unused faces of washers 1 and 7 are placed outward when the washers are stacked on a bolt. This means that, in the assembled state, no braille-style punch marks are directly visible from the outside of the stack.

This is a physical presentation choice, not part of the encoding. The person making the backup may use six washers, seven washers, or another arrangement that suits the storage method. The blank outer faces are not encryption and should not be treated as meaningful protection against someone who can handle or disassemble the backup.

## Why decimal numbers instead of 11-bit binary?

A BIP39 English word can be represented internally by an 11-bit index. An earlier design stored those bits directly as inner/outer punch positions.

The current design instead records a four-digit decimal word number. This removes the need for a human to convert between an English word, an 11-bit value, and punch positions during normal backup and recovery.

The tradeoff is that the decimal representation is project-specific and requires a numbered word list.

## Why 1-based word numbering?

The current project convention uses `0001` through `2048`, corresponding to the first through 2048th entries of the BIP39 English word list.

This is intended to be easier to read in printed human-facing lists. It is deliberately distinguished from the 0-based 11-bit implementation index used by BIP39 software.

## Why braille-style digits?

Standard braille numerals encode decimal digits using the upper four dots of the six-dot cell. Reusing those shapes provides a compact decimal representation with an existing visual convention.

The project renumbers the four physical candidate positions as `1 3 / 2 4` for diagram simplicity, but does not change the shapes of the digits themselves.

A numeric indicator is omitted because every digit field is explicitly numeric.

## Why combine START and SET?

The start marker and set identifier are both metadata rather than mnemonic word content. A combined distinctive marker can provide both functions without consuming two separate regions.

Two fixed marks identify the start structure. A left/right candidate mark encodes the set.

Current standard:

- A = right candidate
- B = left candidate
- both / neither = reserved or invalid by default

SET is intentionally treated as secondary metadata. Physical grouping on a bolt, two-sided recording, and potentially separate storage locations already provide additional context for identifying sets.

## Why is SET not protected by CHECK?

The CHECK is intended primarily to protect the least redundant per-face information: the BIP39 word number.

SET has other physical/contextual redundancy, including grouping washers on a bolt and potentially storing different sets separately. The order field can also have physical redundancy from washer order and the relationship between the two faces of one washer.

Keeping CHECK limited to the BIP39 number also makes its purpose and hand calculation simple.

## Why simple mod 10?

Several alternatives were considered, including Damm, Luhn, weighted modular checks, and binary/CRC-style checks.

Simple mod 10 was selected because:

- it can be calculated and verified with basic addition,
- no lookup table or software is required,
- any single decimal-digit substitution changes the check,
- the project already has other layers of validation, including valid digit shapes, the `0001`–`2048` range, the BIP39 checksum, and potentially an independent backup set.

It is not mathematically strongest. Some multiple errors can cancel each other out. The choice is a tradeoff favoring long-term human readability and minimal dependency.

## Adaptability

The current dimensions, block geometry, 12-word example, and seven-washer assembly are one reference implementation. The underlying idea can be adapted to different washer sizes, word counts, physical layouts, stacking arrangements, or punching tools, provided the resulting geometry and recovery rules are regenerated and independently verified where necessary.

## Design principle

The project prioritizes:

1. recoverability by a human with printed documentation,
2. simple and inspectable rules,
3. physical readability,
4. low-cost common materials,
5. error detection appropriate to the expected physical failure modes,
6. minimizing dependence on software during recovery.
