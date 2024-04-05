## 7.2.4 Shielded transactions

Now that we have a very good understanding of shielded addresses and keys, it is time to see them in action and understand how they fit inside transactions that protect the privacy of Zcash users.

At the moment, the Zcash protocol supports two versions of transactions that are encoded inside blocks. These currently supported transaction versions are version 4 and version 5. The main difference between them is that version 4 allows for both Sprout and Sapling transactions, while version 5 supports Sapling and Orchard transactions.

The recommended transaction version for new software to use is, of course, version 5, while version 4 is there to support legacy Sprout transactions. Let's define the components of each transaction type.

Before actually going into how each protocol version (Sprout, Sapling, Orchard) handles shielded transactions, there are some general concepts that we should learn:

### Notes
A note represents the amount of Zcash (ZEC) that is being transferred in a shielded transaction. It contains information about the value being transferred and the recipient's address but does not reveal this information on the blockchain. Notes are part of the encrypted data that only the recipient can decrypt using their private viewing key.

### Note Commitment
A note commitment is a cryptographic commitment to the contents of a note, including the amount and the recipient's shielded address. When a note is created in a transaction, its commitment is publicly recorded on the blockchain. However, the commitment is designed such that it's computationally infeasible to derive the note's contents from it, ensuring the privacy of the transaction.

### Note Commitment Tree
The Note Commitment Tree is a Merkle tree that aggregates all the note commitments created from transactions into a single structure. Each time a note commitment is created, it is appended to the tree. The root of this tree (the Merkle root) is updated and included in the blockchain's block header. This structure allows for efficient verification of the existence of a note commitment within the tree without revealing its position or the details of other notes, contributing to the privacy and integrity of shielded transactions.

### Nullifiers
A nullifier is derived from a shielded note and is used to prevent double-spending. When a note is spent in a transaction, its nullifier is revealed and recorded on the blockchain. This nullifier is a unique identifier for the note but does not disclose any information about the note itself. Network participants can check the list of nullifiers to ensure that a note has not been spent twice without knowing anything about the note's value or recipient.

### 7.2.4.1 Sprout transactions

The main component of Sprout transactions is the notion of JoinSplits. JoinSplits are specific to the Sprout phase of Zcash and serve as the cornerstone of Zcash's privacy features in this phase. They enable the transaction to hide the sender, receiver, and amount transferred, providing privacy and interchangeability to Zcash transactions.

#### JoinSplit transactions

JoinSplit transactions conceal the transaction details from everyone except the sender and receiver. This includes hiding the identities of the sender and receiver and the amount being transferred, using zk-SNARKs to ensure that transactions are valid without revealing these details. 

Each JoinSplit transaction involves the combination of inputs (funds) from multiple sources (joining) and allocation to multiple outputs (splitting), within a single transaction. This process obscures the flow of funds, adding an additional layer of privacy. This combination of components is known as joinsplit descriptions.

A **JoinSplit description** includes encrypted transaction details, a zk-SNARK proof to validate the transaction's integrity and privacy, and, when applicable, any transparent inputs or outputs. These components ensure that while the transaction is verifiable by the network, the privacy of the transaction participants is maintained.

A **JoinSplit transfer** is an individual shielded value transfer.

I think we are in a good position to figure out how all this works by going to the encoding of a v4 transaction and exploring the fields related to the Sprout shielded transactions.

![transactionv4](assets/transactionv4.png)

Where we are interested in just some of the fields:

- `nJoinSplit`: Is just the number of joinsplit inside this transaction.
- `vJoinSplit`: Is the actual collection of joinsplits descriptions. Before Sapling network upgrade the zk-SNARKs used for proofs of JoinSplits were **BCTV14** while after Sapling network upgrade the joinsplit are proved using **Groth16** zk-SNARK.
- `joinSplitPubKey`:
- `joinSplitSig`:

> [!NOTE]
> BCTV14 and Groth16:
>
> They are both types of zk-SNARKs. 
> BCTV14 is named after its creators — Jens Groth, Alessandro Chiesa, Eran Tromer, and Madars Virza — and was introduced in 2014. It was the first zk-SNARK construction used by Zcash to enable privacy-preserving transactions.
> Groth16 was introduced by Jens Groth in 2016 and represented a significant improvement over BCTV14 and other previous zk-SNARK constructions. It was adopted in the Sapling upgrade of Zcash significantly reducing both the proof sizes and the verification times, making transactions faster and more scalable mantaining the same level of security of .
> Groth16 was introduced by Jens Groth in 2016 and represented a significant improvement over BCTV14 and other previous zk-SNARK constructions. It was adopted in the Sapling upgrade of Zcash significantly reducing both the proof sizes and the verification times, making transactions faster and more scalable mantaining the same level of security of BCTV14.

We then just need to then check the encoding of a JoinSplit:

![joinsplit](assets/joinsplit.png)

Where:

- 
- 

Here is a really good explication of how the pieces fit together:

> Value is carried by "notes" which specify an amount and a public key. To each note there is cryptographically associated a commitment, and a nullifier (so that there is a 1:1:1 relation between notes, commitments, and nullifiers).
>
>Each JoinSplit takes in a transparent value and up to two input notes, and produces a transparent value and up to two output notes. The nullifiers of the input notes are revealed (thus preventing them from being spent again) and the commitments of the output notes are revealed (this allowing them to be spent in future). Each JoinSplit also includes a computationally sound zero-knowledge proof-of-knowledge (SNARK) which proves all of the following:
>
>- The inputs and outputs balance (individually for each JoinSplit).
>- For each input note of non-zero value, some revealed commitment exists for that note.
> - The prover knew the private keys of the input notes.
> - The nullifiers and commitments are computed correctly.
> - The private keys of the input notes are cryptographically linked to a signature over the whole transaction, in such a way that the transaction cannot be modified by a party who did not know these private keys.
> - Each output note is generated in such a way that its nullifier will not collide with the nullifier of any other note.
>
>Outside the SNARK, it is also checked that the nullifiers for the input notes had not already been revealed (i.e. they had not already been spent).
>
>https://forum.zcashcommunity.com/t/is-my-understanding-of-how-zcash-works-correct/961/6

### 7.2.4.2 Sapling transactions

To understand Sapling transactions we can refer to the same image of the v4 transaction fields, we now pay attentioon to this fields:

- `valueBalanceSapling`:
- `nSpendsSapling`: Just the number of Sapling Spends in this transaction.
- `vSpendsSapling`:
- `nOutputsSapling`: Just the number of Sapling outputs in this transaction.
- `vOutputsSapling`:

To understand more we need to refer to the encoding of the Sapling spends:

![sapling_spend](assets/sapling_spend.png)




Where:

- 
- 

And the sapling outputs:

![sapling_spend](assets/sapling_spend.png)

Where:

- 
- 

TODO: EXPLAIN ALLTOGETHER HOW IT WORKS

### 7.2.4.3 Orchard transactions

We can understand orchard transactions by checking the encoding of the transaction version 5 where transparent and sapling fields are present, along with the new orchard fields:

![transactionv5](assets/transactionv5.png)

Where:

- `nActionsOrchard`
- ...

We now check the Actions encoding:

![orchard_actions](assets/orchard_actions.png)

Where:

- 

TODO: EXPLAIN ALLTOGETHER HOW IT WORKS


