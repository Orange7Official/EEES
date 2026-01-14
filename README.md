# 🧠 EEES – European Entity Encoding Standard (v1.1)

## 🔹 EEES Smart Inference Rules

### Tier (Encoding Scope) — *(Lowest encoding)*

| Tier | Description                                |
|------|--------------------------------------------|
| `7`  | Basic ASCII (7-bit Latin, no diacritics)   |
| `8`  | Extended ASCII (8-bit; symbols, legacy sets) |
| `U`  | Full Unicode Range (U+0000–U+10FFFF)       |

---

### Case / Type

- `U`, `L`, `T` → Uppercase, Lowercase, Titlecase (Cased scripts only)
- `u` → Unicase (e.g., Hebrew, Arabic, Kana, Han, etc.)
- `M` → Modifier Letter (e.g., ʼ)
- `S` → Superscript Letter (e.g., ²)
- `s` → Subscript Letter (e.g., ₂)
- `C` → Small Capital Letter (e.g., ᴀ)

> ⚠️ Type and case are in the same slot. Special typographic letters are treated as case variants.

---

### Position

- Reflects the **character’s index** in its applicable block.
- Format:
  - Decimal → `#65`
  - Hexadecimal → `0x41`
- For Tier `U`, position is derived from **Unicode blocks**.

---

## 🧩 EEES Descriptor Structure
`[Tier][Type][Position]`

- **No script tag** is needed — the script is inferred based on tier, case/type, and position.

---

## 🧪 Optimized Inference Examples

| Character | EEES     | Script Inferred From               |
|-----------|----------|------------------------------------|
| `A`       | `7U#65`  | Latin (ASCII Tier + Uppercase)     |
| `a`       | `7L#97`  | Latin (ASCII Tier + Lowercase)     |
| `ñ`       | `8L#241` | Latin Extended (Tier 8 + Lowercase)|
| `Д`       | `UL#1044`| Cyrillic (Unicode + Uppercase)     |
| `א`       | `Uu#1488`| Hebrew (Unicode + Unicase)         |
| `@`       | `8S#64`  | ASCII Symbol (no case needed)      |
| `π`       | `UL#960` | Greek (Unicode + Lowercase)        |
| `ʔ`       | `Uu#660` | IPA (Unicode + Unicase)            |
| `ǈ`       | `UT#456` | Latin Digraph (Unicode + Titlecase)|
| `ʼ`       | `UM#700` | Modifier Letter                    |
| `²`       | `US#178` | Superscript                        |
| `₂`       | `Us#8322`| Subscript                          |
| `ᴀ`       | `UC#11392`| Small Capital Letter               |
| ` ` (Em space) | `U0x2003` | Whitespace, inferred by Unicode block |

---

## 🧾 Notes

- `#` always indicates a **decimal** position.
- `0x` always indicates a **hexadecimal** position.
- The **position** is always **within its Unicode block or encoding range**.
- **Whitespace characters** (e.g., em space) are supported.
- Descriptor omits **type/case** if character isn’t a letter.

---

## 🧮 Tier Position Mapping Summary

| Tier | Range Description           | Example         |
|------|-----------------------------|-----------------|
| `7`  | ASCII (0–127)               | `7U#65` = `A`    |
| `8`  | Extended ASCII (0–255)      | `8L#241` = `ñ`   |
| `U`  | Full Unicode (0–10FFFF)     | `U0x03A9` = `Ω`  |

---

## 📘 Design Intention

The EEES format encodes minimal information per character:
- Encoding level
- Case or typographic variant (when applicable)
- Precise character location

This enables reliable **character-to-script inference** across all common European and multiscript use cases.

---
