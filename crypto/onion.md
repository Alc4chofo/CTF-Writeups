# 🧅

**CTF:** Cátedra Ada Byron UAH (INCIBE)  
**Category:** Cryptography  
**Difficulty:** 50

## Challenge

A web portal with a locked gate, an encoded string, and an input field. The challenge name "onion" tells you to expect multiple layers of encoding.

## Solution

### The gate

The portal displays:

```
MJUWK3TWMVXGSZDPL5QV6Y3BONQV6===
```

Uppercase-only charset (A–Z, 2–7) and `===` padding — that's base32. Decoding gives `bienvenido_a_casa_`, which unlocks stage 2.

### Peeling the onion

Submitting the answer reveals a much longer encoded string. Four layers to peel:

**Layer 1 — Base64.** The string ends with `==` and uses the standard base64 charset. Decoding produces a 394-character hex string.

**Layer 2 — Hex.** Decoding the hex reveals space-separated 8-bit binary groups: `01010011 01011001 01001110 01010100 01111011 ...`

**Layer 3 — Binary.** Converting each 8-bit group to ASCII: `SYNT{MF91MAVG2s3o8Ywa}`

**Layer 4 — ROT13.** `SYNT{` is the classic ROT13 encoding of `FLAG{`. Applying ROT13 to the full string:

| Layer | Encoding | Output |
|-------|----------|--------|
| Gate  | Base32   | `bienvenido_a_casa_` |
| 1     | Base64   | Hex string (394 chars) |
| 2     | Hex      | Binary string (197 chars) |
| 3     | Binary   | `SYNT{MF91MAVG2s3o8Ywa}` |
| 4     | ROT13    | `FLAG{ZS91ZNIT2f3b8Ljn}` |

Flag: `FLAG{ZS91ZNIT2f3b8Ljn}`

## Takeaway

Each encoding has a visual signature: `===` padding and restricted charset for base32, `==` for base64, hex-only digits, uniform 8-bit groups for binary, and `SYNT` → `FLAG` for ROT13. None of these are encryption — they're all reversible with no key required. Recognizing those signatures is the entire challenge.
