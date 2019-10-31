# Blockchain

## Introduction
Blockchain is a distributed ledger that consists of a list of records, called *blocks*, that are linked using a cryptographic hash. Each block contains the
cryptographic hash of the previous block, a timestamp, and the transaction
data. Everyone has access to the ledger and new records can only be added with
the consensus of the network majority.

## Block
Blocks store valuable information - bitcoin blocks store transactions - alongside
some technical information like its version, current timestamp, and the hash of
the previous block.

### Hashes
The way that hashes are calculated is fundamental to securing the blockchain.
Calculating a hash is a computationally difficult operation that makes adding
new blocks difficult, thus preventing retroactive modification of the block.


## Blockchain
In it's base form, a blockchain is just an ordered back-linked list. A key
component of a blockchain is that one has to perform some hard work to put data
in it; this hard work makes the blockchain secure and consistent.

### Hashing
A hash is a unique representation of the original data input. A hash function takes data of arbitrary size and produces a fixed size hash that cannot be reverse engineered to find the original data input. Since a change in even one byte of input data could result in a completely different hash, hashing functions are used to check the consistency of data.
In a blockchain, hashing is used to guarantee the consistency of a block. the input data for the hashing algorithm contains the hash of the previous block to prevent modification of the chain; one would have to recalculate its hash and the hashes of all the blocks after it.

### Proof-of-Work
The maintainers (miners) of the network that perform the work must have their
work proven through a mechanism called proof-of-work. The high computational power it takes to generate the hash makes it difficult to produce but once completed, the hash is easy for others to verify.

#### Hashcash
Hashcash is Bitcoin's Proof-of-Work algorithm. It is a brute force algorithm that hashes the combination of publicly known data and a counter until the hash meets a certain requirement; an example could be "the first 20 bits of the hash have to be zeros".

## Database Storage
To store data in the DB, this implementation copies Bitcoin Core.
Two buckets are used to store data:
1. **blocks** store metadata describing all the blocks in a chain.
2. **chainstate** stores the state of a chain.

Blocks are stored as separate files on the disk so that reading a single block won't require loading all of the other blocks.

### Blocks
In blocks, the key -> value pairs are:

'b' + 32-byte block hash -> block index record
'f' + 4-byte file number -> file information record
'l' -> 4-byte file number: the last block file number used
'R' -> 1-byte boolean: whether we're in the process of reindexing
'F' + 1-byte flag name length + flag name string -> 1 byte boolean: various flags that can be on or off
't' + 32-byte transaction hash -> transaction index record

### Chainstate
In chainstate, the key -> value pairs are:

'c' + 32-byte transaction hash -> unspent transaction output record for that transaction
'B' -> 32-byte block hash: the block hash up to which the database represents the unspent transaction outputs
