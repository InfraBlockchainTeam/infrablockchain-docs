---
title: 트랜잭션에 투표 포함 시키기
description: 이 튜토리얼은 PoT를 이용하여 인프라 릴레이체인의 밸리데이터를 선출하는 방법을 배웁니다.
keywords:
  - Proof-of-Transaction(PoT)
  - 시스템 토큰
---

이 튜토리얼은 실제 멀티체인 환경에서 [Aggregated TaaV(Transaction-as-a-Vote)](../learn/protocol/proof-of-transaction.md#aggregated-proof-of-transactionpot)를 이용하여 [트랜잭션 증명(Proof-of-Transaction, PoT)](../learn/protocol/proof-of-transaction.md) 에 기반한 **_인프라릴레이체인_** 의 밸리데이터를 선출하는 방법을 알아 보겠습니다.

## 튜토리얼 목표

이 튜토리얼을 완료함으로써 다음 목표를 달성할 수 있습니다:

- [좀비넷](../tutorials/test/simulate-parachains.md)을 활용하여 `릴레이체인 <> 파라체인` 테스트 네트워크를 구축할 수 있습니다.
- 시스템 토큰을 이용하여 트랜잭션 수수료를 지불하고 **_인프라릴레이체인_** 의 밸리데이터 후보에 투표할 수 있습니다.
- **_인프라릴레이체인_** 의 밸리데이터가 Seed Trust 노드와 PoT 노드로 구성된 것을 확인할 수 있습니다.

## 시작하기 전에

시작하기 전에 다음을 확인하세요:

- [릴레이 체인 구축하기](../tutorials/build/build-infra-relay-chain.md)와 [파라체인 구축하기](../tutorials/build/build-a-parachain.md)를 통해 테스트에 사용할 바이너리 생성하는 방법 알아보기

- [트랜잭션 증명](../../learn/protocol/proof-of-transaction.md) 방법 알아보기

- [시스템 토큰](../../learn/protocol/system-token.md)으로 트랜잭션 가스비를 내는 방법 알아보기

## 튜토리얼 단계

해당 튜토리얼은 다음과 같은 순서로 진행됩니다.

- 좀비넷으로 테스트 환경을 구축합니다. 본 튜토리얼에서는 **InfraAssetHub** 와 **Parachain Template Node** 두 개의 파라체인을 이용하여 진행할 예정입니다.

- 각 파라체인에서 [시스템 토큰](../learn/protocol/system-token.md)을 이용하여 투표할 대상을 포함하여 트랜잭션을 요청합니다.

- **_인프라릴레이체인_** 에서 투표가 취합되는 것을 확인합니다.

- 특정 시점이 지났을 때, [트랜잭션 증명](../learn/protocol/proof-of-transaction.md)에 기반한 밸리데이터가 선출되어 블록을 생성하는지 확인합니다.

## 테스트 환경 구축하기

튜토리얼을 위해 사용할 **_인프라블록체인_** 테스트 환경을 구축합니다. 먼저, 좀비넷 바이너리를 설치합니다.

- 좀비넷 [깃허브](https://github.com/paritytech/zombienet)에서 최신 릴리즈 버전을 설치합니다.

- `infrablockchain-substrate/infra-cumulus/zombienet/examples/` 에서 `toml` 을 파일을 만들어줍니다. 여기서는 `example.toml` 을 생성하고 다음과 같이 작성해줍니다. 해당 코드는 다음과 같은 테스트 환경을 구축해줍니다:
  - 5 개의 밸리데이터 릴레이체인 노드
  - 2 개의 **InfraAssetHub** 콜레이터 노드
  - 2 개의 **Parachain Template Node** 콜레이터 노드

```toml
[relaychain]
default_command = "{path-to-binary}/infrablockspace"
default_args = ["-lparachain=debug", "-l=xcm=trace"]
chain = "infra-relay-local"

[[relaychain.nodes]]
name = "alice"
validator = true
rpc_port = 7100
ws_port = 7101

[[relaychain.nodes]]
name = "bob"
validator = true
rpc_port = 7200
ws_port = 7201

[[relaychain.nodes]]
name = "charlie"
validator = true
rpc_port = 7300
ws_port = 7301

[[relaychain.nodes]]
name = "dave"
validator = true
rpc_port = 7400
ws_port = 7401

[[relaychain.nodes]]
name = "eve"
validator = true
rpc_port = 7500
ws_port = 7501

[[relaychain.nodes]]
name = "ferdie"
validator = true
rpc_port = 7600
ws_port = 7601

# Asset-Hub
[[parachains]]
id = 1000
chain = "infra-asset-hub-local"
cumulus_based = true

# run alice & bob as parachain collator
[[parachains.collators]]
name = "alice"
validator = true
command = "{path-to-binary}/infra-parachain"
args = ["-lparachain=debug", "-l=xcm=trace"]
rpc_port = 9500
ws_port = 9501

[[parachains.collators]]
name = "bob"
validator = true
command = "{path-to-binary}/infra-parachain"
args = ["-lparachain=debug", "-l=xcm=trace"]
rpc_port = 9510
ws_port = 9511

# Template-node
[[parachains]]
id = 2000
chain = "local"
cumulus_based = true

# run alice & bob as parachain collator
[[parachains.collators]]
name = "alice"
validator = true
command = "{path-to-binary/parachain-template-node}"
args = ["-lparachain=debug", "-l=xcm=trace"]
rpc_port = 9600
ws_port = 9601

# run alice as parachain collator
[[parachains.collators]]
name = "bob"
validator = true
command = "{path-to-binary/parachain-template-node}"
args = ["-lparachain=debug", "-l=xcm=trace"]
rpc_port = 9610
ws_port = 9611

[[hrmp_channels]]
sender = 1000
recipient = 2000
max_capacity = 8
max_message_size = 1048576
[[hrmp_channels]]
sender = 2000
recipient = 1000
max_capacity = 8
max_message_size = 1048576
```

- 터미널에서 좀비넷 명령어를 입력해줍니다:

```bash
# 좀비넷 명령어
./zombienet-macos spawn --provider native ./zombienet/examples/example.toml

# zombinet-macos 명령어가 되지 않을시, 댜음 명령어 입력 후 진행하시기 바랍니다.
chmod +x zombienet-macos
```

## 트랜잭션에 투표 포함시키기

- 시스템 토큰을 이용해 트랜잭션 수수료를 지불하고 투표를 하기 위해서 먼저 [시스템 토큰 생성하기](./how-to-interact-with-system-token.md) 문서를 참고하시기 바랍니다. 해당 튜토리얼에서는 **Parachain Template Node** 에서 시스템 토큰을 생성했다고 가정하겠습니다.

- [인프라블록체인 익스플로러](https://portal.infrablockspace.net) 로 이동합니다.

- 해당 익스플로러에서는 어떠한 트랜잭션이든 투표를 포함시켜 전송할 수 있습니다. 이번 튜토리얼에서는 **Parachain Template Node** 에서 `balance-transfer` 트랜잭션을 전송하는 상황으로 가정하겠습니다.

- `Developer` 섹션에서 `Extrinsics` 탭을 들어갑니다. `balances` 의 `transfer` 를 선택합니다. 트랜잭션에 필요한 값들을 채워 넣은 후 `Submit Transaction` 을 클릭 합니다.

  ![transfer](/media/images/docs/infrablockchain/tutorials/tx-detail.png)

- `include an optional asset id`(필수 옵션) 과 `include an optional vote` (선택)옵션을 켭니다.

  - `asset_id` 옵션은 트랜잭션 수수료로 사용할 시스템 토큰을 명시하는 옵션입니다. 현재 런타임 시점에서 토큰의 `MultiLocation` 을 명시해줍니다. 예를 들면, 파라체인의 경우 릴레이 체인의 [wrapped 토큰](../../learn/protocol/system-token.md#랩드wrapped-시스템-토큰) 을 사용한다면 MultiLocation 은 다음과 같습니다:

  ```text
  - RelayChainAsset = MultiLocation { parents: 1, interior: X2(Pallet, GeneralIndex)}
  ```

  - `vote_candidate` 에는 투표할 대상의 계정 정보를 입력해줍니다.

- 블록이 처리될 때까지 기다린 후 **_인프라릴레이체인_** 에서 다음과 같은 이벤트를 확인합니다.

  ![Event](/media/images/docs/infrablockchain/tutorials/infra-relay-event.png)

- 해당 weight 만큼 투표가 집계가 되었고 **_인프라릴레이체인_** 스토리지에서도 확인할 수 있습니다.

  ![Storage](/media/images/docs/infrablockchain/tutorials/infra-relay-storage.png)

## 다음 단계로 넘어가기

- [밸리데이터로써 리워드를 받는 방법](./how-to-get-validator-reward.md)

- [시스템 토큰 가중치](../learn/protocol/transaction-fee.md#시스템-토큰-가중치)
