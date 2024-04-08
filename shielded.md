# 7.2 Shielded World

Building on our understanding of the Zcash's transparent world, let's explore into the more private side of Zcash with shielded addresses and transactions, the features that truly set Zcash apart in the blockchain space.

## 7.2.1 Value pools

The Zcash blockchain is structured around various value pools, each corresponding to a different type of address or stage of the network's evolution. These pools include:

- Transparent Pool: Contains all funds in transparent addresses, similar to Bitcoin's transaction model.
- Sprout Pool: The initial shielded pool, to be deprecated, representing the first implementation of private transactions.
- Sapling Pool: Represents the next evolution, introducing improved efficiency and privacy.
- Orchard Pool: The latest advancement in Zcash's privacy technology.

The total of these pools equates to the sum of ZEC in circulation. The `getblockchaininfo` RPC method allows users to check the total value in each pool, providing transparency into the distribution of ZEC across different address types.

## 7.2.2 Evolution of Shielded Technology

The evolution of shielded technology in Zcash shows its pioneering journey towards privacy within the blockchain space.

### Sprout: The Foundation of Privacy

Sprout, introduced at Zcash's launch on October 28, 2016, marked the first implementation of shielded transactions using zk-SNARKs. This was a groundbreaking technology designed to enable transaction verification without revealing sensitive information. 

### Sapling: Boosting Efficiency and Adoption

The Sapling network upgrade, activated on October 29, 2018, significantly improved the efficiency and usability of shielded transactions. It introduced new shielded addresses with improved transaction speed and memory usage. This upgrade facilitated mobile apps, exchanges, and vendor adoption of Zcash by making private transactions more practical for everyday use. One of the key features of Sapling was the decoupling of the spending authority from the ability to see transactions with the introduction of the viewing keys.

### Orchard: Advancing Privacy with Halo 2

Orchard, part of the Network Upgrade 5 (NU5) activated in May 2022, represented the latest advancement in Zcash's privacy technology. It incorporates the Halo 2 proving system, eliminating the need for a trusted setup previously required for generating zk-SNARKs. This upgrade also introduced the concept of Unified Addresses, simplifying the user experience by supporting both shielded and transparent transactions within a single address format.

### Zcash Main Network Upgrades

```mermaid
graph TD
    A[Sprout: Launch of Zcash] -->|October 28, 2016| B[Overwinter]
    B -->|June 26, 2018| C[Sapling]
    C -->|December 11, 2019| D[Blossom]
    D -->|July 16, 2020| E[Heartwood]
    E -->|November 18, 2020| F[Canopy]
    F -->|May 31, 2022| G[NU5]

    click A href "https://z.cash/upgrade/sprout/"
    click B href "https://z.cash/upgrade/overwinter/"
    click C href "https://z.cash/upgrade/sapling/"
    click D href "https://z.cash/upgrade/blossom/"
    click E href "https://z.cash/upgrade/heartwood/"
    click F href "https://z.cash/upgrade/canopy/"
    click G href "https://z.cash/upgrade/nu5/"

    style A fill: #fff,stroke:#FFA500,stroke-width:2px;
    style B fill: #fff,stroke:#FFA500,stroke-width:2px;
    style C fill: #fff,stroke:#FFA500,stroke-width:2px;
    style D fill: #fff,stroke:#FFA500,stroke-width:2px;
    style E fill: #fff,stroke:#FFA500,stroke-width:2px;
    style F fill: #fff,stroke:#FFA500,stroke-width:2px;
    style G fill: #fff,stroke:#FFA500,stroke-width:2px;
```

> [!TIP] 
>In Zcash, the "trusted setup" is a foundational step required for the secure implementation of zk-SNARKs, the cryptographic method enabling privacy in transactions. This process involves generating a set of public parameters (often referred to as "toxic waste") essential for constructing zero-knowledge proofs. The integrity of this setup is crucial; any compromise could potentially allow the creation of counterfeit coins. To mitigate this risk, Zcash's initial setups for the Sprout and Sapling upgrades involved elaborate ceremonies with multiple participants, designed to ensure that as long as one party securely destroyed their portion of the toxic waste, the network would remain secure.
>
>However, acknowledging the inherent risk and trust involved in such setups, Zcash moved towards an "untrusted setup" with the introduction of the Halo 2 proving system in the Orchard network upgrade. Halo 2 eliminates the need for a trusted setup by using recursive zk-SNARKs that don't require a setup phase vulnerable to the toxic waste dilemma. This advancement significantly reduces the trust required in the setup process, enhancing the security and decentralization of the network by removing the potential for a single point of failure in the creation of zk-SNARK parameters. This shift marks a significant milestone in Zcash's evolution, offering robust privacy without the need for a trusted setup ceremony.

> [!NOTE]
> A **viewing key** has the necessary information to view information about payments to an Address, or (in the case of a Full Viewing Key) from an Address. An Incoming Viewing Key can be derived from a Full Viewing Key, and an Address can be derived from an Incoming Viewing Key.
