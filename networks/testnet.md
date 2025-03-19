---
outline: [2, 4]
---
# Codex 테스트넷

Codex 테스트넷이 출시되어 테스트에 사용할 준비가 되었습니다.

Your participation in the Codex Testnet is subject to the [Codex Testnet Terms and Conditions](https://github.com/codex-storage/codex-testnet-starter/blob/master/Codex%20Testnet%20Terms%20and%20Conditions.pdf) and [Codex Testnet Privacy Policy](https://github.com/codex-storage/codex-testnet-starter/blob/master/Codex%20Testnet%20Privacy%20Policy.pdf).

**Guides.** We have basic guides covering how to set up a Storage Client which can be used to upload and persist files by buying storage in the Codex network. We recommend that you start with those.

Running a Storage Provider is more involved and is covered as a separate guide which demonstrates the storage sales side, as well as how to run Codex with its own local Ethereum execution client.

Guides are available either on Discord, as step-by-step, interactive guides, or here as simple instructions that you can follow:

- **Basic: running a storage client.** [[Discord](#sc-guide-discord) | [web](#sc-guide-web)]
- **Advanced: Running a storage provider.** [[web](#sp-guide-web)]

The guides were tested on the following operating systems:

 - Linux: Ubuntu 24.04, Debian 12, Fedora 40
 - macOS: 15
 - Windows: 11, Server 2022

## 테스트넷 정보

| 항목 | 값 |
| --- | --- |
| 체인 ID | 2430 |
| 통화 기호 | ETH |
| 블록 시간 | 5초 |
| 합의 | Clique PoA |
| 네트워크 ID | 2430 |
| 네트워크 이름 | Codex Testnet |
| RPC URL | https://rpc.testnet.codex.storage |

## Running a Storage Client (Discord Version) {#sc-guide-discord}

You can join [Codex Discord server](https://discord.gg/codex-storage) and jump into the [#:tv:|join-testnet](https://discord.com/channels/895609329053474826/1289923125928001702) channel.

It is mostly the same as a [Web guide](#sc-guide-web), but uses Discord capabilities so you can have an interactive, step-by-step guide, and you also can get a support in the [#:sos:|node-help](https://discord.com/channels/895609329053474826/1286205545837105224) channel.

## Running a Storage Client (Web Version) {#sc-guide-web}

**Prerequisites**

 - Access to your Internet router so you can [configure port forwarding](#basic-common)

Steps for [Linux/macOS](#basic-linux-macos) and [Windows](#basic-windows) are slightly different, so please use ones for your OS.

<hr>

### Linux/macOS {#basic-linux-macos}

1. Download the master tarball from the Codex testnet starter repository, and untar its contents:
   ```shell
   curl -LO https://github.com/codex-storage/codex-testnet-starter/archive/master.tar.gz
   tar xzvf master.tar.gz
   rm master.tar.gz
   ```

2. Navigate to the scripts folder:
   ```shell
   cd codex-testnet-starter-master/scripts
   ```

3. Install dependencies when required:
   ```shell
   # Debian-based Linux
   sudo apt update && sudo apt install libgomp1
   ```

4. Download Codex binaries from GitHub releases:
   ```shell
   ./download_online.sh
   ```

5. Generate an ethereum keypair:
   ```shell
   ./generate.sh
   ```
   Your private key will be saved to `eth.key` and address to  `eth.address` file.

6. Fill-up your address shown on the screen with the tokens:
   - Use the web faucets to mint some [ETH](https://faucet-eth.testnet.codex.storage) and [TST](https://faucet-tst.testnet.codex.storage) tokens.
   - We can also do this using Discord [# bot](https://discord.com/channels/895609329053474826/1230785221553819669) channel
     - Use `/set ethaddress` command to enter your generated address
     - Use `/mint` command to receive ETH and TST tokens
     - Use `/balance` command to check if you have received test tokens successfully

7. Run Codex node:
   ```shell
   ./run_client.sh
   ```

8. Configure [port forwarding](#basic-common) and we are ready go to.

### Windows {#basic-windows}

1. Download the master tarball from the Codex testnet starter repository, and untar its contents:
   > [!WARNING]
   > Windows antivirus software and built-in firewalls may cause steps to fail. We will cover some possible errors here, but always consider checking your setup if requests fail - in particular, if temporarily disabling your antivirus fixes it, then it is likely to be the culprit.

   ```batch
   curl -LO https://github.com/codex-storage/codex-testnet-starter/archive/master.tar.gz
   ```

   If you see an error like:

   ```batch
   curl: (35) schannel: next InitializeSecurityContext failed: CRYPT_E_NO_REVOCATION_CHECK (0x80092012) - The revocation function was unable to check revocation for the certificate.
   ```

   You may need to add the `--ssl-no-revoke` option to your curl call, e.g.:

   ```batch
   curl -LO --ssl-no-revoke https://github.com/codex-storage/codex-testnet-starter/archive/master.tar.gz
   ```

1. Extract the contents of the tar file, and then delete it:
   ```batch
   tar xzvf master.tar.gz
   del master.tar.gz
   ```

2. Navigate to the scripts folder:
   ```batch
   cd codex-testnet-starter-master\scripts\windows
   ```

3. Download Codex binaries from GitHub releases:
   ```batch
   download-online.bat
   ```

4. Generate an ethereum keypair:
   ```batch
   generate.bat
   ```
   Your private key will be saved to `eth.key` and address to  `eth.address` file.

5. Fill-up your address shown on the screen with the tokens:
   - Use the web faucets to mint some [ETH](https://faucet-eth.testnet.codex.storage) and [TST](https://faucet-tst.testnet.codex.storage) tokens.
   - We can also do this using Discord [# bot](https://discord.com/channels/895609329053474826/1230785221553819669) channel
     - Use `/set ethaddress` command to enter your generated address
     - Use `/mint` command to receive ETH and TST tokens
     - Use `/balance` command to check if you have received test tokens successfully

6. Run Codex node:
   ```batch
   run-client.bat
   ```

 7. Configure [port forwarding](#basic-common) and we are ready go to.

### All OS {#basic-common}

Configure [port forwarding](https://en.wikipedia.org/wiki/Port_forwarding) on your Internet router
| # | Protocol | Port   | Description       |
| - | -------- | ------ | ----------------- |
| 1 | `UDP`    | `8090` | `Codex Discovery` |
| 2 | `TCP`    | `8070` | `Codex Transport` |

After your node is up and running, you can use the [Codex API](/developers/api) to be able to interact with your Codex node, please check our [API walk-through](/learn/using) for more details.

You also can use [Codex App UI](https://app.codex.storage) to interact with your local Codex node.

Need help? Reach out to us in [#:sos:|node-help](https://discord.com/channels/895609329053474826/1286205545837105224) channel or check [troubleshooting guide](/learn/troubleshoot.md).

## Running a Storage Provider (Web Version) {#sp-guide-web}

Work in progress :construction:

## Testnet Data

### Bootstrap Nodes
**Codex**
```shell
spr:CiUIAhIhAiJvIcA_ZwPZ9ugVKDbmqwhJZaig5zKyLiuaicRcCGqLEgIDARo8CicAJQgCEiECIm8hwD9nA9n26BUoNuarCEllqKDnMrIuK5qJxFwIaosQ3d6esAYaCwoJBJ_f8zKRAnU6KkYwRAIgM0MvWNJL296kJ9gWvfatfmVvT-A7O2s8Mxp8l9c8EW0CIC-h-H-jBVSgFjg3Eny2u33qF7BDnWFzo7fGfZ7_qc9P
spr:CiUIAhIhAyUvcPkKoGE7-gh84RmKIPHJPdsX5Ugm_IHVJgF-Mmu_EgIDARo8CicAJQgCEiEDJS9w-QqgYTv6CHzhGYog8ck92xflSCb8gdUmAX4ya78QoemesAYaCwoJBES39Q2RAnVOKkYwRAIgLi3rouyaZFS_Uilx8k99ySdQCP1tsmLR21tDb9p8LcgCIG30o5YnEooQ1n6tgm9fCT7s53k6XlxyeSkD_uIO9mb3
spr:CiUIAhIhA6_j28xa--PvvOUxH10wKEm9feXEKJIK3Z9JQ5xXgSD9EgIDARo8CicAJQgCEiEDr-PbzFr74--85TEfXTAoSb195cQokgrdn0lDnFeBIP0QzOGesAYaCwoJBK6Kf1-RAnVEKkcwRQIhAPUH5nQrqG4OW86JQWphdSdnPA98ErQ0hL9OZH9a4e5kAiBBZmUl9KnhSOiDgU3_hvjXrXZXoMxhGuZ92_rk30sNDA
spr:CiUIAhIhA7E4DEMer8nUOIUSaNPA4z6x0n9Xaknd28Cfw9S2-cCeEgIDARo8CicAJQgCEiEDsTgMQx6vydQ4hRJo08DjPrHSf1dqSd3bwJ_D1Lb5wJ4Qt_CesAYaCwoJBEDhWZORAnVYKkYwRAIgFNzhnftocLlVHJl1onuhbSUM7MysXPV6dawHAA0DZNsCIDRVu9gnPTH5UkcRXLtt7MLHCo4-DL-RCMyTcMxYBXL0
spr:CiUIAhIhAzZn3JmJab46BNjadVnLNQKbhnN3eYxwqpteKYY32SbOEgIDARo8CicAJQgCEiEDNmfcmYlpvjoE2Np1Wcs1ApuGc3d5jHCqm14phjfZJs4QrvWesAYaCwoJBKpA-TaRAnViKkcwRQIhANuMmZDD2c25xzTbKSirEpkZYoxbq-FU_lpI0K0e4mIVAiBfQX4yR47h1LCnHznXgDs6xx5DLO5q3lUcicqUeaqGeg
spr:CiUIAhIhAgybmRwboqDdUJjeZrzh43sn5mp8jt6ENIb08tLn4x01EgIDARo8CicAJQgCEiECDJuZHBuioN1QmN5mvOHjeyfmanyO3oQ0hvTy0ufjHTUQh4ifsAYaCwoJBI_0zSiRAnVsKkcwRQIhAJCb_z0E3RsnQrEePdJzMSQrmn_ooHv6mbw1DOh5IbVNAiBbBJrWR8eBV6ftzMd6ofa5khNA2h88OBhMqHCIzSjCeA
spr:CiUIAhIhAntGLadpfuBCD9XXfiN_43-V3L5VWgFCXxg4a8uhDdnYEgIDARo8CicAJQgCEiECe0Ytp2l-4EIP1dd-I3_jf5XcvlVaAUJfGDhry6EN2dgQsIufsAYaCwoJBNEmoCiRAnV2KkYwRAIgXO3bzd5VF8jLZG8r7dcLJ_FnQBYp1BcxrOvovEa40acCIDhQ14eJRoPwJ6GKgqOkXdaFAsoszl-HIRzYcXKeb7D9
```

**Geth**
```shell
enode://cff0c44c62ecd6e00d72131f336bb4e4968f2c1c1abeca7d4be2d35f818608b6d8688b6b65a18f1d57796eaca32fd9d08f15908a88afe18c1748997235ea6fe7@159.223.243.50:40010
enode://ea331eaa8c5150a45b793b3d7c17db138b09f7c9dd7d881a1e2e17a053e0d2600e0a8419899188a87e6b91928d14267949a7e6ec18bfe972f3a14c5c2fe9aecb@68.183.245.13:40030
enode://4a7303b8a72db91c7c80c8fb69df0ffb06370d7f5fe951bcdc19107a686ba61432dc5397d073571433e8fc1f8295127cabbcbfd9d8464b242b7ad0dcd35e67fc@174.138.127.95:40020
enode://36f25e91385206300d04b95a2f8df7d7a792db0a76bd68f897ec7749241b5fdb549a4eecfab4a03c36955d1242b0316b47548b87ad8291794ab6d3fecda3e85b@64.225.89.147:40040
enode://2e14e4a8092b67db76c90b0a02d97d88fc2bb9df0e85df1e0a96472cdfa06b83d970ea503a9bc569c4112c4c447dbd1e1f03cf68471668ba31920ac1d05f85e3@170.64.249.54:40050
enode://6eeb3b3af8bef5634b47b573a17477ea2c4129ab3964210afe3b93774ce57da832eb110f90fbfcfa5f7adf18e55faaf2393d2e94710882d09d0204a9d7bc6dd2@143.244.205.40:40060
enode://6ba0e8b5d968ca8eb2650dd984cdcf50acc01e4ea182350e990191aadd79897801b79455a1186060aa3818a6bc4496af07f0912f7af53995a5ddb1e53d6f31b5@209.38.160.40:40070
```

### Smart contracts

| Contract    | Address                                                                                                                                   |
| ----------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| Token       | [`0x34a22f3911De437307c6f4485931779670f78764`](https://explorer.testnet.codex.storage/address/0x34a22f3911De437307c6f4485931779670f78764) |
| Verifier    | [`0x02dd582726F7507D7d0F8bD8bf8053d3006F9092`](https://explorer.testnet.codex.storage/address/0x02dd582726F7507D7d0F8bD8bf8053d3006F9092) |
| Marketplace | [`0xAB03b6a58C5262f530D54146DA2a552B1C0F7648`](https://explorer.testnet.codex.storage/address/0xAB03b6a58C5262f530D54146DA2a552B1C0F7648) |

### Endpoints

| # | Service         | URL                                                                          |
| - | --------------- | ---------------------------------------------------------------------------- |
| 1 | Geth Public RPC | [rpc.testnet.codex.storage](https://rpc.testnet.codex.storage)               |
| 2 | Block explorer  | [explorer.testnet.codex.storage](https://explorer.testnet.codex.storage)     |
| 3 | Faucet ETH      | [faucet-eth.testnet.codex.storage](https://faucet-eth.testnet.codex.storage) |
| 4 | Faucet TST      | [faucet-tst.testnet.codex.storage](https://faucet-tst.testnet.codex.storage) |
| 5 | Status page     | [status.testnet.codex.storage](https://status.testnet.codex.storage)         |

## 유용한 링크

| # | 서비스          | URL                                                                        |
| - | --------------- | ---------------------------------------------------------------------------- |
| 1 | Geth 공개 RPC | [rpc.testnet.codex.storage](https://rpc.testnet.codex.storage)               |
| 2 | 블록 탐색기    | [explorer.testnet.codex.storage](https://explorer.testnet.codex.storage)     |
| 3 | ETH 수도꼭지    | [faucet-eth.testnet.codex.storage](https://faucet-eth.testnet.codex.storage) |
| 4 | TST 수도꼭지    | [faucet-tst.testnet.codex.storage](https://faucet-tst.testnet.codex.storage) |
| 5 | 상태 페이지     | [status.testnet.codex.storage](https://status.testnet.codex.storage)         |
