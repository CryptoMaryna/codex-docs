# 설명 및 아키텍처

Codex는 web3 애플리케이션에 부패 및 검열 저항성을 제공하는 완전히 분산된 내구성 있는 데이터 저장 엔진을 구축하고 있습니다. 노드 운영자에게는 저장하는 데이터에 대한 타당한 부인 가능성을, 클라이언트에게는 최대 99.99%의 입증 가능한 내구성 보장을 제공하면서도 저장소와 대역폭 효율성을 유지합니다.

These four key features combine to differentiate Codex from existing projects in the decentralised storage niche:

- **Erasure coding:** Provides efficient data redundancy, which increases data durability guarantees.

- **ZK-based proof-of-retrievability:** For lightweight data durability assurances.

- **Lazy repair mechanism:** For efficient data reconstruction and loss prevention.

- **Incentivization:**  To encourage rational behaviour, widespread network participation, and the efficient provision of finite network resources.


### Incentivized decentralisation

Incentivization mechanisms are one of the key pieces missing from traditional file-sharing networks. Codex believes that a robust marketplace-based incentive structure will ensure wide participation across the node types detailed below.

The development of an adequate incentive structure is driven by the following goals: 

- Supply and demand to encourage optimum network resource usage.

- Increase participation by enabling nodes to utilise their competitive advantages to maximise profits.

- Prevent spam and discourage malicious participation. 

Although still to be finalised, the Codex incentive structure will involve a marketplace of participants who want to store data, and those provisioning storage posting collateral, with the latter bidding on open storage contracts. This structure aims to ensure that participants' incentives align, resulting in Codex functioning as intended.


### Network architecture

Codex is composed of multiple node types, each taking a different role in the network's operation. Similarly, the hardware demands for each node type vary, enabling those operating resource-restricted devices to participate.

**Storage nodes**

As Codex's long-term reliable storage providers, storage nodes stake collateral based on the collateral posted on the request side of contracts, and the number of slots that a contract has. This is tied to the durability demanded by the user. Failure to provide periodic proof of data possession results in slashing penalties.

**Aggregator Node**

A method for off-loading erasure coding, proof generation and proof aggregation by a client node with low-resources, currently a WIP and will be part of subsequent Codex release Q2/Q4 next year.

**Client nodes**

Client nodes make requests for other nodes to store, find, and retrieve data. Most of the Codex network will be Client nodes, and these participants can double as caching nodes to offset the cost of the network resources they consume. 

When a node commits to a storage contract and a user uploads data, the network will proactively verify that the storage node is online and that the data is retrievable. Storage nodes are then randomly queried to broadcast proofs of data possession over an interval corresponding to the contract duration and 9's of retrievability guarantee the protocol provides.

If the storage node sends invalid proofs or fails to provide them in time, the network evicts the storage node from the slot, and the slot will become available for the first node that generates a valid proof for that slot. 

When the contract is reposted, some of the faulty node's collateral pays for the new storage node's bandwidth fees. Erasure coding complements the repair scheme by allowing the reconstruction of the missing chunks from data in other slots within the same storage contract hosted by faultless storage nodes.


![architect](/learn/architecture.png)

### Marketplace architecture ###

The marketplace consists of a smart contract that is deployed on-chain, and the
purchasing and sales modules that are part of the node software. The purchasing
module is responsible for posting storage requests to the smart contract. The
sales module is its counterpart that storage providers use to determine which
storage requests they are interested in.

#### Smart contract ####

The smart contract facilitates matching between storage providers and storage
clients. A storage client can request a certain amount of storage for a certain
duration. This request is then posted on-chain, so that storage providers can
see it, and decide whether they want to fill a slot in the request.

The main parameters of a storage request are:
- the amount of bytes of storage that is requested
- a content identifier (CID) of the data that should be stored
- the duration for which the data should be stored
- the number of slots (based on the erasure coding parameters)
- an amount of tokens to pay for the storage

At the protocol level a storage client is free to determine these parameters as
it sees fit, so that it can choose a level of durability that is suitable for
the data, and adjust for changing storage prices. Applications built on Codex
can provide guidance to their users for picking the correct parameters,
analogous to how Ethereum wallets help with determining gas fees.

The smart contract also checks that storage providers keep their promises.
Storage providers post collateral when they promise to fill a slot of a storage
request. They are expected to post periodic storage proofs to the contract,
either directly or through an aggregator. If they fail to do so repeatedly, then
their collateral can be forfeited. Their slot is then awarded to another storage
provider.

The smart contract indicates when a certain storage provider has to provide a
storage proof. This is not done on a fixed time interval, but determined
stochastically to ensure that it is not possible for a storage provider to
predict when it should provide the next storage proof.

#### Purchasing ####

The purchasing module in the node software interacts with the smart contract on
behalf of the node operator. It posts storage requests, and handles any other
interactions that are required during the lifetime of the request. For instance,
when a request is canceled because there are not enough interested storage
providers, then the purchasing module can withdraw the tokens that were
associated with the request.

#### Sales ####

The sales module is the counterpart to the sales module. It monitors the smart
contract to be notified of incoming storage requests. It keeps a list of the
most promising requests that it can fulfill. It will favor those requests that
have a high reward and low collateral. As soon as it finds a suitable request,
it will then try to first reserve and then fill a slot by downloading the
associated data, creating a storage proof, and posting it to the smart contract.
It will then continue to monitor the smart contract to provide it with storage
proofs when they are required.

The sales module contains a best effort strategy for determining which storage
requests it is interested in. Over time, we expect more specialized strategies
to emerge to cater to the needs of e.g. large providers versus providers that
run a node from their home.

### 백서 ###

[Codex 백서](/learn/whitepaper)를 읽어보세요.
