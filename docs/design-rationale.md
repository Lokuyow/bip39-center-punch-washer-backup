# Design Rationale

[日本語](design-rationale_ja.md)

This document explains why the current reference format looks the way it does. The project is experimental, so these decisions may change after physical testing or review.

## Why washers?

Stainless-steel washers are inexpensive, widely available, heat resistant compared with paper, and easy to stack on a bolt. Both faces can be used, allowing compact storage of mnemonic words.

The current reference implementation uses large M8 washers and demonstrates a 12-word mnemonic with a seven-washer assembly, but neither M8, 12 words, nor seven washers is a fundamental requirement of the method.

## Why a 24 mm large M8 washer?

The current reference implementation uses a DIN 9021-style M8 flat washer with a 24.0 mm outside diameter and 8.4 mm inside diameter.

This size is a reference choice intended to provide a relatively wide annular face for the current eight-block layout and four-point candidate cells while still using inexpensive, commonly available hardware.

The dimensions are not an encoding requirement. A different washer size requires the candidate-point spacing, paper jig, ease of punching, and recovery readability to be verified again.

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

The tradeoff is that the project uses its own **1-based four-digit decimal convention, `0001` through `2048`**, so a numbered word list is required.

## Why numbers instead of the first four letters?

The BIP39 English word list is designed so that the first four letters uniquely identify each word. Recording those four letters directly is therefore a reasonable metal-backup approach.

Washer Punch39 uses decimal numbers instead. The main reasons are:

- punching only needs ten digit symbols, 0–9,
- the same four-point cell can encode order, BIP39 word number, and CHECK,
- alphabet letter punches or different geometric shapes for letters are unnecessary,
- a paper jig and center punch can record everything with one consistent geometry.

The tradeoff is that converting the number back to a word requires a numbered BIP39 English word list. This makes the format less self-describing than a first-four-letters backup.

## Why 1-based word numbering?

The current project convention uses `0001` through `2048`, corresponding to the first through 2048th entries of the BIP39 English word list.

This is intended to be easier to read in printed human-facing lists. It is deliberately distinguished from the 0-based 11-bit implementation index used by BIP39 software.

## Why braille-style digits?

Standard braille numerals encode decimal digits using the upper four dots of the six-dot cell. Reusing those shapes provides a compact decimal representation with an existing visual convention.

A numeric indicator is omitted because every digit field is explicitly numeric.

## Why combine START and SET?

The start marker and set identifier are both metadata rather than mnemonic word content. A combined distinctive marker can provide both functions without consuming two separate regions.

Two fixed marks identify the start structure. A left/right candidate mark encodes the set.

Current standard:

- A = right candidate
- B = left candidate
- both / neither = reserved or invalid by default

SET is **secondary metadata used to distinguish different mnemonics when more than one mnemonic is stored**. A/B does not represent shares of one mnemonic or a 2-of-2 scheme. All faces belonging to one mnemonic use the same SET value, and SET can be used to group mixed washers during recovery.

## Why are SET and order not protected by CHECK?

CHECK is intended to let a person verify the core per-face payload, the BIP39 word number, with a simple hand calculation.

SET and order have other consistency checks outside the per-face CHECK. For example:

- within one mnemonic, SET should normally be consistently A or consistently B,
- a complete 12-word set should contain each order value `01` through `12` exactly once,
- when the washers remain assembled on a bolt, physical order can provide another clue.

These checks are not complete error detection. For example, swapping the order values on two washers can still leave a complete `01`–`12` set. Excluding SET and order from CHECK is therefore a tradeoff between simplicity and detection strength.

Limiting CHECK to the BIP39 number keeps both its purpose and its calculation simple.

## Why simple mod 10?

Several alternatives were considered, including Damm, Luhn, weighted modular checks, and binary/CRC-style checks.

Simple mod 10 was selected because:

- it can be calculated and verified with basic addition,
- no lookup table or software is required,
- it detects a single decimal-digit substitution affecting one of the four BIP39 digits or the CHECK digit,
- other validation layers can also be used, including valid digit shapes, the `0001`–`2048` range, the BIP39 checksum, and an independent backup set.

However, simple mod 10 gives no positional weight to the digits, so **it cannot detect transpositions**. For example, `1391` and `1931` both have a digit sum of 14 and therefore use the same CHECK. Multiple digit changes can also cancel each other out.

The BIP39 checksum is another useful validation layer, but it does not prove that a recovered mnemonic is identical to the original. In the current 12-word reference case, the BIP39 checksum is only 4 bits, so an incorrect word sequence can still be checksum-valid.

CHECK, range validation, and the BIP39 checksum should therefore be treated as **layers of validation**, not as standalone guarantees of correctness. Where possible, recovery should also be checked against other known information such as a wallet fingerprint, a previously recorded receive address, or an independent backup.

Simple mod 10 is not mathematically strongest. The choice is a tradeoff favoring long-term human readability and minimal dependency during recovery.

## Why punch directly through the paper jig?

The current reference workflow aligns and secures the paper jig to the washer, then punches directly through the paper with a center punch.

Compared with first transferring the candidate positions to the washer with a pen and then punching them, direct punching:

- removes a work step,
- reduces position drift and transcription mistakes while copying marks,
- allows the same jig containing all candidate positions to be used directly.

The paper jig does not print seed-specific selections. Holes and punch marks created during use reflect work history, but because the jig contains every candidate position, repeated use adds more holes and weakens the correspondence between the used jig and any one secret backup.

## Adaptability

The current dimensions, block geometry, 12-word example, and seven-washer assembly are one reference implementation. The underlying idea can be adapted to different washer sizes, word counts, physical layouts, stacking arrangements, or punching tools, provided the resulting geometry and recovery rules are regenerated and independently verified where necessary.

## Design principles

The project prioritizes:

1. recoverability by a human with printed documentation,
2. simple and inspectable rules,
3. physical readability,
4. low-cost common materials,
5. making expected physical damage or reading errors easier to detect,
6. minimizing dependence on software during recovery.
