# 빠른 시작

이 가이드를 통해 Codex를 실행하려면 다음 단계를 수행해야 합니다:
- [면책 조항 검토](/codex/disclaimer)
- [Codex 바이너리 얻기](#get-codex-binary)
- [Codex 실행하기](#run-codex)
- [Codex와 상호작용하기](#interact-with-codex)

## Get Codex binary

For quick a start we will use precompiled binaries from [GitHub release page](https://github.com/codex-storage/nim-codex/releases). If you prefer to compile from the sources, please check [Build Codex](/learn/build).

Please follow the steps for your OS from the list:
- [Linux/macOS](#linux-macos)
- [Windows](#windows)

### Linux/macOS

1. Install latest Codex release
   ```shell
   curl -s https://get.codex.storage/install.sh | bash
   ```

2. Install dependencies
   ```shell
   # Debian-based Linux
   sudo apt update && sudo apt install libgomp1
   ```

3. Check the result
   ```shell
   codex --version
   ```

### Windows

1. Install latest Codex release
   ```batch
    curl -sO https://get.codex.storage/install.cmd && install.cmd 
   ```

   > [!WARNING]
   > Windows antivirus software and built-in firewalls may cause steps to fail. We will cover some possible errors here, but always consider checking your setup if requests fail - in particular, if temporarily disabling your antivirus fixes it, then it is likely to be the culprit.

   If you see an error like:

   ```batch
   curl: (35) schannel: next InitializeSecurityContext failed: CRYPT_E_NO_REVOCATION_CHECK (0x80092012) - The revocation function was unable to check revocation for the certificate.
   ```

   You may need to add the `--ssl-no-revoke` option to your curl calls, i.e., modify the calls above so they look like this:

   ```batch
    curl -LO --ssl-no-revoke https://...
    ```

2. Update path using console output
    - Current session only
      ```batch
      :: Default installation directory
      set "PATH=%PATH%%LOCALAPPDATA%\Codex;"
      ```

    - Update PATH permanently
      - Control Panel --> System --> Advanced System settings --> Environment Variables
      - Alternatively, type `environment variables` into the Windows Search box

3. Check the result
   ```shell
   codex --version
   ```

## Run Codex

We may [run Codex in different modes](/learn/run#run), and for a quick start we will run [Codex node](/learn/run#codex-node), to be able to share files in the network.

1. Run Codex

   **Linux/macOS**
   ```shell
   codex \
     --data-dir=datadir \
     --disc-port=8090 \
     --listen-addrs=/ip4/0.0.0.0/tcp/8070 \
     --nat=`curl -s https://ip.codex.storage` \
     --api-cors-origin="*" \
     --bootstrap-node=spr:CiUIAhIhAiJvIcA_ZwPZ9ugVKDbmqwhJZaig5zKyLiuaicRcCGqLEgIDARo8CicAJQgCEiECIm8hwD9nA9n26BUoNuarCEllqKDnMrIuK5qJxFwIaosQ3d6esAYaCwoJBJ_f8zKRAnU6KkYwRAIgM0MvWNJL296kJ9gWvfatfmVvT-A7O2s8Mxp8l9c8EW0CIC-h-H-jBVSgFjg3Eny2u33qF7BDnWFzo7fGfZ7_qc9P
   ```

   **Windows**

   > [!WARNING]
   > Windows might at this stage prompt you to grant internet access to Codex. You must allow it for things to work.
   > It also might be required to add incoming firewall rules for Codex and we can use `netsh` utility.

   <details>
   <summary>add firewall rules using netsh</summary>

   ```batch
   :: Add rules
   netsh advfirewall firewall add rule name="Allow Codex (TCP-In)" protocol=TCP dir=in localport=8070 action=allow
   netsh advfirewall firewall add rule name="Allow Codex (UDP-In)" protocol=UDP dir=in localport=8090 action=allow

   :: List rules
   netsh advfirewall firewall show rule name=all | find /I "Codex"

   :: Delete rules
   netsh advfirewall firewall delete rule name="Allow Codex (TCP-In)"
   netsh advfirewall firewall delete rule name="Allow Codex (UDP-In)"
   ```
   </details>

   ```batch
   :: Get Public IP
   for /f "delims=" %a in ('curl -s --ssl-reqd ip.codex.storage') do set nat=%a

   :: Run Codex
   codex ^
     --data-dir=datadir ^
     --disc-port=8090 ^
     --listen-addrs=/ip4/0.0.0.0/tcp/8070 ^
     --nat=%nat% ^
     --api-cors-origin="*" ^
     --bootstrap-node=spr:CiUIAhIhAiJvIcA_ZwPZ9ugVKDbmqwhJZaig5zKyLiuaicRcCGqLEgIDARo8CicAJQgCEiECIm8hwD9nA9n26BUoNuarCEllqKDnMrIuK5qJxFwIaosQ3d6esAYaCwoJBJ_f8zKRAnU6KkYwRAIgM0MvWNJL296kJ9gWvfatfmVvT-A7O2s8Mxp8l9c8EW0CIC-h-H-jBVSgFjg3Eny2u33qF7BDnWFzo7fGfZ7_qc9P
   ```

   > [!TIP]
   > In the example above we use [Codex Testnet](/networks/testnet#bootstrap-nodes) bootstrap nodes and thus we join Testnet. If you would like to join a different network, please use [appropriate value](/networks/networks).

2. Configure port-forwarding for the TCP/UDP ports on your Internet router
   | Protocol | Service   | Port   |
   | -------- | --------- | ------ |
   | UDP      | Discovery | `8090` |
   | TCP      | Transport | `8070` |

If you would like to purchase or sell storage, please consider to run [Codex node with marketplace support](/learn/run#codex-node-with-marketplace-support) or [Codex storage node](/learn/run#codex-storage-node).

## Interact with Codex

When your Codex node is up and running you can interact with it using [Codex App UI](https://app.codex.storage) for files sharing.

Also, you can interact with Codex using [Codex API](/developers/api) and for a walk-through of the API, consider following the [Using Codex](/learn/using) guide.

## Stay in touch

Want to stay up-date, or looking for further assistance? Try our [discord-server](https://discord.gg/codex-storage).

Ready to explore Codex functionality? Please [Join Codex Testnet](/networks/testnet).

If you want to run Codex locally without joining the Testnet, consider trying the [Codex Two-Client Test](/learn/local-two-client-test) or the [Running a Local Codex Network with Marketplace Support](/learn/local-marketplace).
