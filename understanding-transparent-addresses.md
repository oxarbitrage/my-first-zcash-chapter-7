## 7.1.3 Understanding transparent addresses

In Zcash transparent mode, a **public key hash** is created by applying two hash functions to the public key, just as in Bitcoin:

$public\\_key\\_hash = RIPEMD160(SHA256(public\\_key))$

Where:

- $SHA256$: This hash function produces a secure, fixed-length output of $X$ bits.
- $RIPEMD160$: This hash function generates a slightly less secure, fixed-length output of $Y$ bits. Where $Y < X$

RIPEMD160 is considered less secure but produces a smaller output, which is desirable in large-scale blockchains. On the other hand, SHA256 is considered more secure but with a larger output. Thus, the solution at the time of Bitcoin's creation was to obtain a secure hash with SHA256 and then compress the output size by applying RIPEMD160.

> [!NOTE]
> A hash function is a mathematical algorithm that takes an input (or "message") and produces a fixed-size string of bytes, which is typically a hexadecimal number. The output is often referred to as the hash value, hash code, or digest. There are many hash functions, let's see a bit more detail in the ones we already used: 
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

In Zcash, transparent addresses serve as identifiers for receiving funds in transparent transactions. There are two types of transparent addresses, which correspond to address types in the Bitcoin protocol: **P2SH** (Pay to Script Hash) and **P2PKH** (Pay to Public Key Hash).

### P2PKH addresses

P2PKH addresses in Zcash are derived from the public key hash. Here's how they are encoded:

![p2pkh](https://github.com/oxarbitrage/my-first-zcash-chapter-7/assets/21685097/4741f524-c1f1-4bf8-8153-a9168941c466)

The diagram illustrates that the last 20 bytes represent the public key hash we studied earlier. To create a P2PKH address, the public key hash is encoded using **Base58Check**, with the prefix `t3` for the Zcash Mainnet:

$transparent\\_address = 't3' \mathbin\Vert Base58Check(public\\_key\\_hash)$

For example, if the Base58Check for the public key hash `58262d1fbdbe4530d8865d3518c6d6e41002610f` is `936CR3RBty2VxQWX2MgKk6vz63LTAm1UD`, then the corresponding Zcash Mainnet transparent address would be `t3936CR3RBty2VxQWX2MgKk6vz63LTAm1UD`.

> [!NOTE]
> Transparent addresses in Zcash are commonly referred to as **transparent payment addresses**, as they are used to receive funds in transparent transactions.

> [!NOTE]
> The Base58Check encoding is a way to encode byte arrays into into human-typable strings.

### P2SH addresses

TODO

![p2sh](https://github.com/oxarbitrage/my-first-zcash-chapter-7/assets/21685097/c06d614f-690c-4e3c-8cb6-b55fa4ed63d0)



