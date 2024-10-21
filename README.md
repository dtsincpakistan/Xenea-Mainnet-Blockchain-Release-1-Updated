Xenea is a Layer 1 blockchain. 
## Xenea

Xenea or Xenea is a clone of Geth which is the implementation of the Ethereum protocol in Golang. The reason for going with Geth was that we required a programmable blockchain. Geth has a good implementation for the virtual machine required for making the blockchain programmable. Utilizing Geth helped us focus more on transaction validity part without re-inventing all things again. Xenea is a Layer 1 blockchain. Geth comes with a builtin Proof of Authority (PoA) consensus mechanism which is named as 'Clique'. Inside Xenea we have added a new consensus mechanism by the name of 'dts'. Clique does not include reward mechanism as PoA consensus mechanisms do not have a reward mechanism. In contrast in dts we have a reward mechanism where on every block generation we reward Escrow nodes, DACS node and voting nodes.

There are 3 important types of nodes in Xenea which are Escrow nodes, DACS node and voting nodes. All nodes will utilize the same source code. The only difference will be in parameters when running Xenea on each node. Let's discuss each node one by one. The purpose of the Escrow nodes is to generate new blocks. Only Escrow nodes can do this task. Escrow nodes' wallet addresses are added in the JSON file which is utilized for Xenea initialization. DACS node is for storing NFT contents and generating the hash against NFT contents. As NFT contents are stored therefore DACS node requires a good capacity storage inside. Voting nodes are for validating transactions that are being generated. Only those transactions are added in block that have a validity percentage of 75% or above. Voting nodes are selected after every 10 minutes. Voting nodes sent their replies to Escrow nodes which does the final calculation for the validity percentage. Nodes are identified using the identity flags e.g. for the Escrow nodes we utilize the identity of signer-node. On the other hand voting nodes are identified by their wallet addresses.

Currently 300 XCR are generated per block where as block generation time is 1 minute per block. Roughly we will have block generation like this:
1) 60 blocks per hour
2) 1440 blocks per day

The reward mechanism per block is like this:
1) DACS node 		   240 XCR
2) Escrow nodes	   36 XCR
3) Each Voting node	4.8 XCR
Maximum voting nodes can be 5 therefore total reward for voting nodes becomes 24 XCR

Roughly DACS node will earn a reward of 345,600 XENE per day. On number 2 the Escrow nodes will earn approximately 51,840 XENE per day.

XENE to XENE transfers under xenea are free. No gas fees is charged for XENE to XENE transfers. On the other hand NFT related transactions will be charged according to the complexity of the smart contract utilized. The more complex is the smart contract then more fee will be charged and vice versa.

We have utilized 2 types of wallet with Xenea
1) Wallet inside Xenea (console based)
2) Metamask

The Xenea explorer can be accessed using the following URL:
https://www.crossvaluescan.com/

## Building the source

Building `xenea` requires both a Go (version 1.16 or later) and a C compiler. You can install
them using your favourite package manager. Once the dependencies are installed, run

```shell
make all
```

in order to build the full suite of utilities.

## Executables

The Xenea project comes with several wrappers/executables found in the `cmd`
directory.

|    Command    | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| :-----------: | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|  **`xenea`**   | Our main CLI client. It is the entry point into the xenea network, capable of running as a full node (default). It can be used by other processes as a gateway into the Xenea network via JSON RPC endpoints exposed on top of HTTP, WebSocket and/or IPC transports. exnea --help and the CLI page for command line options.          |
|   `clef`    | Stand-alone signing tool, which can be used as a backend signer for `xenea`.  |
|   `devp2p`    | Utilities to interact with nodes on the networking layer, without running a full blockchain. |
|   `abigen`    | Source code generator to convert contract definitions into easy to use, compile-time type-safe Go packages. |
|     `evm`     | Developer utility version of the EVM (Ethereum Virtual Machine) that is capable of running bytecode snippets within a configurable environment and execution mode. Its purpose is to allow isolated, fine-grained debugging of EVM opcodes (e.g. `evm --code 60ff60ff --debug run`).                                                                                                                                                                                                                                                                     |
|   `rlpdump`   | Developer utility tool to convert binary RLP dumps to user-friendlier hierarchical representation (e.g. `rlpdump --hex CE0183FFFFFFC4C304050583616263`).                                                                                                                                                                                                                                 |
## Running `crossvalue`

Going through all the possible command line flags is out of scope here but we've enumerated a few common parameter combos to get you up to speed quickly on how you can run your own `xenea` instance.

### Hardware Requirements

Minimum:

* CPU with 2+ cores
* 4GB RAM
* 1TB free storage space to sync the Mainnet
* 8 MBit/sec download Internet service

### Programmatically interfacing `crossvalue` nodes

As a developer, sooner rather than later you'll want to start interacting with crossvalue and the Xenea network via your own programs and not manually through the console. To aid this, crossvalue has built-in support for a JSON-RPC based APIs (standard APIs and xenea specific APIs). These can be exposed via HTTP, WebSockets and IPC (UNIX sockets on UNIX based platforms, and named pipes on Windows).

The IPC interface is enabled by default and exposes all the APIs supported by `crossvalue`,
whereas the HTTP and WS interfaces need to manually be enabled and only expose a
subset of APIs due to security reasons. These can be turned on/off and configured as
you'd expect.

HTTP based JSON-RPC API options:

  * `--http` Enable the HTTP-RPC server
  * `--http.addr` HTTP-RPC server listening interface (default: `localhost`)
  * `--http.port` HTTP-RPC server listening port (default: `8545`)
  * `--http.api` API's offered over the HTTP-RPC interface (default: `eth,net,web3`)
  * `--http.corsdomain` Comma separated list of domains from which to accept cross origin requests (browser enforced)
  * `--ws` Enable the WS-RPC server
  * `--ws.addr` WS-RPC server listening interface (default: `localhost`)
  * `--ws.port` WS-RPC server listening port (default: `8546`)
  * `--ws.api` API's offered over the WS-RPC interface (default: `eth,net,web3`)
  * `--ws.origins` Origins from which to accept websockets requests
  * `--ipcdisable` Disable the IPC-RPC server
  * `--ipcapi` API's offered over the IPC-RPC interface (default: `admin,debug,eth,miner,net,personal,txpool,web3`)
  * `--ipcpath` Filename for IPC socket/pipe within the datadir (explicit paths escape it)

You'll need to use your own programming environments' capabilities (libraries, tools, etc) to
connect via HTTP, WS or IPC to a `crossvalue` node configured with the above flags and you'll
need to speak [JSON-RPC](https://www.jsonrpc.org/specification) on all transports. You
can reuse the same connection for multiple requests!

### Voting node installation

Run this command to initialize your voting node:
```shell
./xenea --datadir ./data init ./dts.json
```
Further run this command to start your voting node:
```shell
./xenea --networkid 1133 --datadir "./data/" --bootnodes enode://78c74774875815aa84a7393c79231a88588639956a3fc2df5b3a0ed9591311f582db9b83099a50e176821814be6720cd4f80aa976d6450132e4e425faf8a36f7@211.132.7.236:0?discport=30301 --syncmode full --http --allow-insecure-unlock --http.corsdomain "*" -unlock 0x<voting node wallet address> --password password console --identity 0x<voting node wallet address>
```
## License

The Xenea library (i.e. all code outside of the `cmd` directory) is licensed under the
[GNU Lesser General Public License v3.0](https://www.gnu.org/licenses/lgpl-3.0.en.html),
also included in our repository in the `COPYING.LESSER` file.

The Xenea binaries (i.e. all code inside of the `cmd` directory) is licensed under the
[GNU General Public License v3.0](https://www.gnu.org/licenses/gpl-3.0.en.html), also
included in our repository in the `COPYING` file.

