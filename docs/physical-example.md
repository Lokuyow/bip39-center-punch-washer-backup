# Physical example

This page shows a physical Washer Punch39 workflow from aligning and securing the paper jig to punching and final assembly. The photos are arranged in workflow order.

## 1. Jig aligned and secured to the washer

![Washer Punch39 jig aligned and secured to a washer with masking tape](../images/photos/washer-punch39-jig-alignment.jpg)

Print the full-scale jig at **100% / actual size / no scaling** and verify the printed scale. Align the washer with the paper jig, then secure it with masking tape so it does not shift during punching.

## 2. After punching through the paper jig

![Washer Punch39 paper jig taped to a washer after punching](../images/photos/washer-punch39-jig-after-punching.jpg)

This is the state **immediately after the required candidate positions were punched through the paper with an automatic center punch**. The visible holes and punch marks in the paper were made by the punching step.

## 3. Washer after removing the paper jig

![Punched Washer Punch39 washer after removing the paper jig](../images/photos/washer-punch39-punched-washer.jpg)

After removing the paper jig, check the punched positions on the washer face.

```text
A | ⠚ ⠉ | ⠁ ⠉ ⠊ ⠁ | ⠋
```

Decoded, this gives:

```text
A | 03 | 1391 | 6
```

- SET: **A**
- order: **03**
- BIP39 word number: **1391 = punch**
- CHECK: **6**

The CHECK is valid because `1 + 3 + 9 + 1 + 6 = 20`, which is a multiple of 10.

## 4. Assembled with bolt and double nut

![Washer Punch39 washer stack assembled with a bolt and double nut](../images/photos/washer-punch39-assembled-stack.jpg)

In the current reference implementation, the washers are stacked on a bolt and secured with **two nuts tightened against each other as a double-nut arrangement** to reduce the chance of loosening during storage.

For dimensions, encoding rules, and recovery instructions, see the main [README](../README.md) and the other documents in this directory.
