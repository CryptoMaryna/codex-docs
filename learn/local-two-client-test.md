# Codex 두 클라이언트 테스트

두 클라이언트 테스트는 설정을 확인하고 Codex API에 익숙해지기 위해 수행할 수 있는 수동 테스트입니다. 이 단계들은 두 개의 노드를 실행하고 연결하여 하나에 파일을 업로드한 다음 다른 노드에서 해당 파일을 다운로드하는 과정을 안내합니다. 이 테스트에는 마켓플레이스 기능을 사용할 수 있도록 로컬 블록체인 노드를 실행하는 것도 포함됩니다.

## Prerequisite

Make sure you have [built the client](/learn/build) or obtained [compiled binary](/learn/quick-start#get-codex-binary).

## Steps

### 0. Setup blockchain node (optional)

You need to have installed NodeJS and npm in order to spinup a local blockchain node.

Go to directory `vendor/codex-contracts-eth` and run these two commands:
```
npm ci
npm start
```

This will launch a local Ganache blockchain.

### 1. Launch Node #1

Open a terminal and run:
- Mac/Linux:
  ```shell
   codex \
     --data-dir="$(pwd)/Data1" \
     --api-port=8080 \
     --disc-port=8090 \
     --listen-addrs="/ip4/127.0.0.1/tcp/8070"
   ```
- Windows:
  ```batch
  codex.exe ^
    --data-dir="Data1" ^
    --api-port=8080 ^
    --disc-port=8090 ^
    --listen-addrs="/ip4/127.0.0.1/tcp/8070"
  ```

Optionally, if you want to use the Marketplace blockchain functionality, you need to also include these flags: `--persistence --eth-account=<account>`, where `account` can be one following:

  - `0x70997970C51812dc3A010C7d01b50e0d17dc79C8`
  - `0x3C44CdDdB6a900fa2b585dd299e03d12FA4293BC`
  - `0x90F79bf6EB2c4f870365E785982E1f101E93b906`
  - `0x15d34AAf54267DB7D7c367839AAf71A00a2C6A65`

**For each node use a different account!**

| Argument       | Description                                                           |
|----------------|-----------------------------------------------------------------------|
| `data-dir`     | We specify a relative path where the node will store its data.        |
| `listen-addrs` | Multiaddress where the node will accept connections from other nodes. |
| `api-port`     | Port on localhost where the node will expose its API.                 |
| `disc-port`    | Port the node will use for its discovery service.                     |
| `persistence`  | Enables Marketplace functionality. Requires a blockchain connection.  |
| `eth-account`  | Defines which blockchain account the node should use.                 |

Codex uses sane defaults for most of its arguments. Here we specify some explicitly for the purpose of this walk-through.

### 2. Sign of life

Run the command :

```bash
curl -X GET http://127.0.0.1:8080/api/codex/v1/debug/info
```

This GET request will return the node's debug information. The response will be in JSON and should look like:

```json
{
  "id": "16Uiu2HAmJ3TSfPnrJNedHy2DMsjTqwBiVAQQqPo579DuMgGxmG99",
  "addrs": [
    "/ip4/127.0.0.1/tcp/8070"
  ],
  "repo": "/Users/user/projects/nim-codex/Data1",
  "spr": "spr:CiUIAhIhA1AL2J7EWfg7x77iOrR9YYBisY6CDtU2nEhuwDaQyjpkEgIDARo8CicAJQgCEiEDUAvYnsRZ-DvHvuI6tH1hgGKxjoIO1TacSG7ANpDKOmQQ2MWasAYaCwoJBH8AAAGRAh-aKkYwRAIgB2ooPfAyzWEJDe8hD2OXKOBnyTOPakc4GzqKqjM2OGoCICraQLPWf0oSEuvmSroFebVQx-3SDtMqDoIyWhjq1XFF",
  "announceAddresses": [
    "/ip4/127.0.0.1/tcp/8070"
  ],
  "table": {
    "localNode": {
      "nodeId": "f6e6d48fa7cd171688249a57de0c1aba15e88308c07538c91e1310c9f48c860a",
      "peerId": "16Uiu2HAmJ3TSfPnrJNedHy2DMsjTqwBiVAQQqPo579DuMgGxmG99",
      "record": "...",
      "address": "0.0.0.0:8090",
      "seen": false
    },
    "nodes": []
  },
  "codex": {
    "version": "untagged build",
    "revision": "b3e626a5"
  }
}
```

| Field               | Description                                                                              |
| ------------------- | ---------------------------------------------------------------------------------------- |
| `id`                | Id of the node. Also referred to as 'peerId'.                                            |
| `addrs`             | Multiaddresses currently open to accept connections from other nodes.                    |
| `repo`              | Path of this node's data folder.                                                         |
| `spr`               | Signed Peer Record, encoded information about this node and its location in the network. |
| `announceAddresses` | Multiaddresses used for annoucning this node                                             |
| `table`             | Table of nodes present in the node's DHT                                                 |
| `codex`             | Codex version information                                                                |

### 3. Launch Node #2

We will need the signed peer record (SPR) from the first node that you got in the previous step.

Replace `<SPR HERE>` in the following command with the SPR returned from the previous command, note that it should include the `spr:` at the beginning.

Open a new terminal and run:
- Mac/Linux:
  ```shell
  codex \
    --data-dir="$(pwd)/Data2" \
    --api-port=8081 \
    --disc-port=8091 \
    --listen-addrs=/ip4/127.0.0.1/tcp/8071 \
    --bootstrap-node=<SPR HERE>
  ```
- Windows:
  ```
  codex.exe ^
    --data-dir="Data2" ^
    --api-port=8081 ^
    --disc-port=8091 ^
    --listen-addrs=/ip4/127.0.0.1/tcp/8071 ^
    --bootstrap-node=<SPR HERE>
  ```

Alternatively on Mac, Linux, or MSYS2 and a recent Codex binary you can run it in one command like:

```shell
codex \
  --data-dir="$(pwd)/Data2" \
  --api-port=8081 \
  --disc-port=8091 \
  --listen-addrs=/ip4/127.0.0.1/tcp/8071 \
  --bootstrap-node=$(curl -H "Accept: text/plain" http://127.0.0.1:8080/api/codex/v1/spr)
```

Notice we're using a new data-dir, and we've increased each port number by one. This is needed so that the new node won't try to open ports already in use by the first node.

We're now also including the `bootstrap-node` argument. This allows us to link the new node to another one, bootstrapping our own little peer-to-peer network. SPR strings always start with `spr:`.

### 4. Connect The Two

Normally the two nodes will automatically connect. If they do not automatically connect or you want to manually connect nodes you can use the peerId to connect nodes.

You can get the first node's peer id by running the following command and finding the `"peerId"` in the results:

```shell
curl -X GET \
  -H "Accept: text/plain" \
  http://127.0.0.1:8081/api/codex/v1/peerid
```

Next replace `<PEER ID HERE>` in the following command with the peerId returned from the previous command:

```shell
curl -X GET \
  http://127.0.0.1:8080/api/codex/v1/connect/<PEER ID HERE>?addrs=/ip4/127.0.0.1/tcp/8071
```

Alternatively on Mac, Linux, or MSYS2 and a recent Codex binary you can run it in one command like:

```shell
curl -X GET \
  http://127.0.0.1:8080/api/codex/v1/connect/$(curl -X GET -H "Accept: text/plain" http://127.0.0.1:8081/api/codex/v1/peerid)\?addrs=/ip4/127.0.0.1/tcp/8071
```

Notice that we are sending the "`peerId`" and the multiaddress of node 2 to the `/connect` endpoint of node 1. This provides node 1 all the information it needs to communicate with node 2. The response to this request should be `Successfully connected to peer`.

### 5. Upload The File

We're now ready to upload a file to the network. In this example we'll use node 1 for uploading and node 2 for downloading. But the reverse also works.

Next replace `<FILE PATH>` with the path to the file you want to upload in the following command:

```shell
curl -X POST \
  127.0.0.1:8080/api/codex/v1/data \
  -H "Content-Type: application/octet-stream" \
  -H "Expect: 100-continue" \
  -T "<FILE PATH>"
```
> [!TIP]
> If curl is reluctant to show you the response, add `-o <FILENAME>` to write the result to a file.

Depending on the file size this may take a moment. Codex is processing the file by cutting it into blocks and generating erasure-recovery data. When the process is finished, the request will return the content-identifier (CID) of the uploaded file. It should look something like `zdj7WVxH8HHHenKtid8Vkgv5Z5eSUbCxxr8xguTUBMCBD8F2S`.

### 6. Download The File

Replace `<CID>` with the identifier returned in the previous step. Replace `<OUTPUT FILE>` with the filename where you want to store the downloaded file:

```bash
curl -X GET \
  127.0.0.1:8081/api/codex/v1/data/<CID>/network \
  -o <OUTPUT FILE>
```

Notice we are connecting to the second node in order to download the file. The CID we provide contains the information needed to locate the file within the network.

### 7. Verify The Results

If your file is downloaded and identical to the file you uploaded, then this manual test has passed. Rejoice! If on the other hand that didn't happen or you were unable to complete any of these steps, please leave us a message detailing your troubles.

## Notes

When using the Ganache blockchain, there are some deviations from the expected behavior, mainly linked to how blocks are mined, which affects certain functionalities in the Sales module.
Therefore, if you are manually testing processes such as payout collection after a request is finished or proof submissions, you need to mine some blocks manually for it to work correctly. You can do this by using the following curl command:

```shell
curl -X POST \
  127.0.0.1:8545 \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","method":"evm_mine","params":[],"id":67}'
```

## 알려진 문제점

요청이 완료된 후 지불금 수집이나 증명 제출과 같은 프로세스를 수동으로 테스트하는 경우, 제대로 작동하려면 수동으로 블록을 채굴해야 합니다. 다음 curl 명령을 사용하여 이 작업을 수행할 수 있습니다:

```shell
curl -X POST \
  127.0.0.1:8545 \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","method":"evm_mine","params":[],"id":67}'
```
