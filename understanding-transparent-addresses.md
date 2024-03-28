## 7.1.3 Understanding transparent addresses

In Zcash transparent mode, a **public key hash** is created by applying two hash functions to the public key, just as in Bitcoin:

$public\\_key\\_hash = RIPEMD160(SHA256(public\\_key))$

Where:

- $SHA256$: This hash function produces a secure, fixed-length output of $X$ bits.
- $RIPEMD160$: This hash function generates a slightly less secure, fixed-length output of $Y$ bits. Where $Y < X$

RIPEMD160 is considered less secure but produces a smaller output, which is desirable in large-scale blockchains. On the other hand, SHA256 is considered more secure but with a larger output. Thus, the solution at the time of Bitcoin's creation was to obtain a secure hash with SHA256 and then compress the output size by applying RIPEMD160.
