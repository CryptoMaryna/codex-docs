---
outline: [2, 3]
---
# 문제 해결

테스트넷에 Codex 노드를 연결하는 데 문제가 있으신가요? 여기 일반적인 Codex 연결 문제와 이를 진단하고 해결하는 단계들을 모아놨습니다. 여기서 해결되지 않는 문제가 있다면, Github의 미해결 이슈를 확인하거나 Discord 서버를 통해 연락해 주세요.

## 기본 사항

You've probably already considered these. But just in case:

1. Are you using a VPN? Make sure it's configured correctly to forward the right ports, and make sure you announce your node by the public IP address where you can be reached.
1. Are you using a firewall or other security software? Make sure it's configured to allow incoming connections to Codex's discovery and peer-to-peer ports.

## Check your announce address

Your node announces your public address to the network, so other nodes can connect to you. A common issue is connection failure due to incorrect announce addresses. Follow these steps to check your announce address.

1. Go to a whats-my-ip site, or `ip.codex.storage` and note the IP address.
1. Go into your router/modem WAN settings and find the public IP address.
1. These two addresses should match.
1. If they do not, it's possible that A) you're behind a VPN. In this case, it's up to you to disable the VPN or make sure all forwarding is configured correctly. or B) Your internet-service-provider has placed your uplink behind a secondary NAT. ISPs do this to save public IP addresses. The address assigned to your router/moderm is not a 'true' public internet address. Usually this issue can be solved by your ISP. Contact customer support and ask them to give you a public address (sometimes also called Dynamic IP address).
1. Call Codex's debug/info endpoint. See the [Using Codex](/learn/using) for the details.
1. In the JSON response, you'll find "announceAddresses".
1. The IP address listed there should match your public IP.
1. If the announce address in the JSON is incorrect, you can adjust it manually by changing Codex's CLI argument `--nat` or setting the environment variable `CODEX_NAT`. After you've changed your announce address and restarted your node, please allow some time (20-30mins) for the network to disseminate the updated address.

If you've performed these steps and haven't found any issues, your announce address is probably not the problem.


