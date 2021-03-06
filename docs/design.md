# 「天地」Ametsuchi's Design

アメツチホシソラヤマカハミネタニクモキリムロコケヒトイヌウエスヱユワサルオフセヨエノ枝ヲナレヰテ

## Purpose of Iroha

Iroha is a distributed ledger made to create a verifiable data store, where every action is digitally signed and keeping track of digital assets and contracts is trivial. 

### End vision

Iroha will be the platform to easily manage any kind of digital asset, currency, or contract. All the major banks and insurance companies of the world will use Iroha and it will become the backbone of IT infrastructure to make the world more efficient.

## Iroha use cases

### Digital identity

Digital certificates containing hashes of users' personal identity that are signed by a known source can function as a form of digital identity, by using public-private key cryptography to prove identity.

### Supply chain management

### Electronic payments

Electronic payments, especially mobile payments, can benefit from Iroha for managing digital currencies in real time. Iroha aims to achieve transaction finality within 2 seconds internally for many network configurations, so this allows for the creation of mobile apps for enabling face-to-face payments.

### Stock/bond management

### Health record management

### Voting

### Land registration

## Iroha's use of Ametsuchi

Iroha stores all its data as flatbuffers. From the data storage to the user-facing apps, they all use flatbuffers, provided via a grpc interface.

## Specific requirements

Below are some specific functional and non-functional requirements for *Ametsuchi*.

### Functional requirements

| Requirement ID | FR-01                                    |
| -------------- | ---------------------------------------- |
| Title          | Save flatbuffers to the database.        |
| Description    | We should be able to *atomically* commit flatbuffers to add them to the database. These will not be updated later, so this should be only for additions. |
| Priority       | 10                                       |
| Risk           | low                                      |

| Requirement ID | FR-02                                    |
| -------------- | ---------------------------------------- |
| Title          | Create Merkle tree from flatbuffers      |
| Description    | A binary hash tree (Merkle tree) should be constructed from flatbuffers stored in *Ametsuchi*. Digital signatures from the 2f+1 validating nodes should be stored with each Merkle root for the global and block-level roots. |
| Priority       | 10                                       |
| Risk           | medium                                   |

| Requirement ID | FR-03                                    |
| -------------- | ---------------------------------------- |
| Title          | Create cache of world state.             |
| Description    | There should be a cache of the **world state**, calculated by applying the transactions in the ledger in order. For example, if a digital Yen asset is created and 100 Yen are sent to account *A*, then the cache of the global state will have some record like: **A: 100 Yen**. This cache needs to be updated *atomically* when new transactions are committed to the ledger. There should also be some way to audit this cache periodically to guarantee that it has not been hacked. An attacker could potentially hack this cache to change an account balance, without touching the confirmed ledger. This is a risk for financial applications, as APIs used in production apps will simply access this cache. Also, this cache needs to be updated at potentially every transaction. There are potentially scalability issues related to handling large numbers of accounts. One potential attack vector could be to create trillions of accounts that have to be cached in the world state. |
| Priority       | 10                                       |
| Risk           | high                                     |

| Requirement ID | FR-04                                    |
| -------------- | ---------------------------------------- |
| Title          | Query for flatbuffers.                   |
| Description    | It should be possible to query for flatbuffers using a programmatic API. |
| Priority       | 10                                       |
| Risk           | medium                                   |

| Requirement ID | FR-05                                    |
| -------------- | ---------------------------------------- |
| Title          | Query for world state.                   |
| Description    | It should be possible to query for various parameters in the world state. For example, to query for the balance of assets owned by an account. |
| Priority       | 10                                       |
| Risk           | high                                     |

| Requirement ID | FR-06                                    |
| -------------- | ---------------------------------------- |
| Title          | Machine learning and data analytics.     |
| Description    | It should be possible to train statistical models using data stored in *Ametsuchi*. This is required to make the data come "alive," so *Ametsuchi* is is more than just a dumb database, but also can be used to support OLAP and decision making. Methods such as map-reduce and tensorflow should be considered. Functionality that is implemented should be to support use-cases. |
| Priority       | 5                                        |
| Risk           | high                                     |

### Non-functional requirements

### Security

The system should be secure against internal and external threats.

### Immutability

Data should not be changeable after committing to the database.

### Data privacy

Data should not be available to those who do not have permissions to view it.

### Ease-of-use

The API should be easy for competent programmers to understand.

### High performance

It should be very fast to save and query data. Targets are 100,000 saves/s and 100,000 queries/s.

### Big data

System should easily scale to 100 petabytes.
