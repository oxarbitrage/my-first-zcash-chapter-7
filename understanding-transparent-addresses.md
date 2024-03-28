## 7.1.3 Understanding transparent addresses

In Zcash transparent mode, a **public key hash** is created by applying two hash functions to the public key, just as in Bitcoin:

$public\\_key\\_hash = RIPEMD160(SHA256(public\\_key))$

Where:

- $SHA256$: This hash function produces a secure, fixed-length output of $X$ bits.
- $RIPEMD160$: This hash function generates a slightly less secure, fixed-length output of $Y$ bits. Where $Y < X$

RIPEMD160 is considered less secure but produces a smaller output, which is desirable in large-scale blockchains. On the other hand, SHA256 is considered more secure but with a larger output. Thus, the solution at the time of Bitcoin's creation was to obtain a secure hash with SHA256 and then compress the output size by applying RIPEMD160.

> [!NOTE]
> A hash function is a mathematical algorithm that takes an input (or "message") and produces a fixed-size string of bytes, which is typically a hexadecimal number. The output is often referred to as the hash value, hash code, or digest.
> 
> **SHA256** (Secure Hash Algorithm 256):
> 
> - SHA256 is a widely used cryptographic hash function that produces a 256-bit (32-byte) hash value.
> - It takes an input message of any size and produces a fixed-size output hash value.
> - The output hash value is typically represented as a 64-character hexadecimal string.
> 
> Example:
> - Input: "Hello, world!"
> - SHA256 Hash: "315f5bdb76d078c43b8ac0064e4a0164612b1fce77c869345bfc94c75894edd3"
>
> **RIPEMD160** (RACE Integrity Primitives Evaluation Message Digest 160):
>
>- RIPEMD160 is a cryptographic hash function that produces a 160-bit (20-byte) hash value.
>- It is designed to be faster than SHA-1 and MD5 but is considered less secure than SHA256.
>- The output hash value is typically represented as a 40-character hexadecimal string.
>
> Example:
> - Input: "Hello, world!"
> - RIPEMD160 Hash: "58262d1fbdbe4530d8865d3518c6d6e41002610f"
