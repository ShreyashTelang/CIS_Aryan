# Playfair Cipher Interactive Simulator

An interactive, visual, and educational simulator for the **Playfair Cipher**, designed in accordance with the **Cryptography and Information Security (CIS)** syllabus.

Built entirely as a **single, self-contained HTML file** (`index.html`) using pure HTML5, CSS3, and modern vanilla JavaScript—with **zero external libraries, CDNs, or dependencies**. Works completely offline, in both light and dark mode, and on mobile screens.

---

## 🚀 Live Demo & Quick Start

Simply open `index.html` directly in any web browser:

```bash
# Double-click index.html or open via command line:
start index.html       # Windows
open index.html        # macOS
xdg-open index.html    # Linux
```

No build tools, package managers, or local web servers are required.

---

## ✨ Features

- **Mode Switching:** Easily toggle between **Encrypt** and **Decrypt** operations.
- **Input Sanitization:** Automatically converts text to uppercase, strips punctuation, whitespace, and numbers, and merges `J` into `I`.
- **Dynamic 5×5 Key Matrix:**
  - Constructs the 25-cell alphabet matrix using the keyword (with duplicates removed) followed by remaining alphabet letters (`I`/`J` unified).
  - Distinctive visual styling and badge highlights for keyword letters.
  - Coordinate labels (`R0`–`R4`, `C0`–`C4`) and subscripts on every cell.
- **Digraph Preparation (Encryption Mode):**
  - Splits plaintext into two-letter pairs.
  - Automatically separates identical letter pairs (e.g., `LL`) with a filler character.
  - Automatically pads odd-length messages with the filler.
  - **Customizable filler character** (default: `X`, supports alternates like `Q` or `Z`).
  - Interactive digraph chips allowing immediate inspection of any pair.
- **Geometric Transformation Rules:**
  - **Same Row:** Shifts each letter 1 step to the right (encrypt) or left (decrypt) with cyclic wrap-around.
  - **Same Column:** Shifts each letter 1 step downward (encrypt) or upward (decrypt) with cyclic wrap-around.
  - **Rectangle:** Swaps each letter with the one in its own row at the other letter's column (symmetric for both encryption and decryption).
- **Interactive Step-by-Step Visualizer:**
  - Controls: **Next Pair**, **Prev Pair**, **Auto Play / Pause** (with speed options: 0.5×, 1×, 2×), **Show Result**, and **Reset**.
  - **Live SVG Overlay:** Dynamically draws dashed bounding boxes for rectangles and rounded highlight bands with directional transition arrows for row/column shifts.
  - Visual color coding: Input letters (Blue & Purple) and Output letters (Green & Amber).
  - Clear real-time banner detailing the active rule, coordinate shift, and mathematical reasoning.
- **Side-by-Side Verification Table:**
  - Tabular view listing Pair #, Input Digraph, Rule, Coordinates, Output Digraph, and Cumulative Text.
- **Round-Trip Decryption ("Use Result as Input"):**
  - Seamlessly transfers the current ciphertext into the input box and switches to Decrypt mode.
  - Includes pedagogical notes explaining why trailing/intermediate filler letters (`X`) appear in the decrypted output and must be stripped manually.
- **Comprehensive CIS Syllabus Theory & Cryptanalysis Section:**
  - Explains the history, mathematical properties, and why Playfair is historically significant.
  - Details its cryptographic vulnerabilities: preservation of digraph frequency distributions, structural symmetry/reciprocity, impossibility of self-encryption ($E(x) \neq x$), and vulnerability to known-plaintext matrix reconstruction.
- **Graph-Paper Aesthetic & Accessibility:**
  - Clean blueprint graph-paper background pattern.
  - Crisp monospace font stack for matrix and cipher operations.
  - Full **Light** and **Dark** theme support with persistent settings.
  - High contrast ratios (WCAG compliant) and visible keyboard focus states.
  - Keyboard shortcuts:
    - <kbd>Space</kbd>: Auto Play / Pause
    - <kbd>→</kbd>: Next Pair
    - <kbd>←</kbd>: Previous Pair
    - <kbd>R</kbd>: Reset to Step 0

---

## 📖 Textbook Example Walkthrough

### Configuration
- **Keyword:** `MONARCHY`
- **Plaintext:** `INSTRUMENTS`
- **Filler:** `X`

### 1. Matrix Construction (5×5)

```
       C0   C1   C2   C3   C4
  R0 [ M    O    N    A    R ]
  R1 [ C    H    Y    B    D ]
  R2 [ E    F    G    I    K ]
  R3 [ L    P    Q    S    T ]
  R4 [ U    V    W    X    Z ]
```

### 2. Digraph Preparation
- `INSTRUMENTS` has 11 letters.
- Pairs: `IN`, `ST`, `RU`, `ME`, `NT`, `S` (trailing single letter padded with `X` $\rightarrow$ `SX`).
- Resulting digraphs: `IN`, `ST`, `RU`, `ME`, `NT`, `SX`.

### 3. Step-by-Step Cipher Execution

| Pair # | Input Digraph | Geometric Rule | Coordinate Transformation | Output Digraph | Cumulative Output |
|:------:|:-------------:|:--------------:|:--------------------------|:--------------:|:-----------------:|
| 1 | **IN** | Rectangle | `(R2, C3)` & `(R0, C2)` $\rightarrow$ `(R2, C2)` & `(R0, C3)` | **GA** | `GA` |
| 2 | **ST** | Same Row | `R3: C3` $\rightarrow$ `C4`, `C4` $\rightarrow$ `C0` (wrap) | **TL** | `GATL` |
| 3 | **RU** | Rectangle | `(R0, C4)` & `(R4, C0)` $\rightarrow$ `(R0, C0)` & `(R4, C4)` | **MZ** | `GATLMZ` |
| 4 | **ME** | Same Column | `C0: R0` $\rightarrow$ `R1`, `R2` $\rightarrow$ `R3` | **CL** | `GATLMZCL` |
| 5 | **NT** | Rectangle | `(R0, C2)` & `(R3, C4)` $\rightarrow$ `(R0, C4)` & `(R3, C2)` | **RQ** | `GATLMZCLRQ` |
| 6 | **SX** | Same Column | `C3: R3` $\rightarrow$ `R4`, `R4` $\rightarrow$ `R0` (wrap) | **XA** | `GATLMZCLRQXA` |

**Ciphertext Output:** `GATLMZCLRQXA`

### 4. Decryption Round-Trip
Feeding `GATLMZCLRQXA` into Decrypt mode shifts letters in the reverse direction, faithfully reconstructing `IN ST RU ME NT SX` (`INSTRUMENTSX`). The reader removes the final padding character `X` to restore `INSTRUMENTS`.

---

## 🔒 Cryptanalysis & Security Summary

1. **Digraph Frequency Analysis:** While Playfair flattens single-letter frequencies (such as `E` or `T`), it preserves bigram frequency characteristics. Common English digraphs (`TH`, `HE`, `IN`, `ER`, `AN`, `RE`) still appear with recognizable statistical frequencies in ciphertext.
2. **Symmetry & Reciprocal Mapping:**
   If digraph $AB$ encrypts to $CD$ using the rectangle rule, then $BA$ will always encrypt to $DC$, and $CD$ decrypts to $AB$.
3. **No Self-Encryption:**
   A letter can never encrypt to itself ($E(x) \neq x$). This structural property significantly aids cryptanalysts during crib-dragging attacks.
4. **Known-Plaintext Vulnerability:**
   With approximately 25 to 30 known digraph pairs, analysts can formulate systems of equations over the matrix coordinates and recover the entire 5×5 key matrix.

---

## 📁 Repository Structure

```
.
├── index.html   # Complete, self-contained application (HTML + CSS + JS)
└── README.md    # Documentation, syllabus notes, and user guide
```

---

## 📄 License

This educational simulator is developed for academic study in Cryptography and Information Security (CIS). Feel free to use, modify, and distribute for learning and teaching purposes.
