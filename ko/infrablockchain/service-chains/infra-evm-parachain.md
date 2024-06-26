---
title: 인프라EVM
description: EVM 호환 블록체인에 대한 전반적인 내용을 다룹니다.
keywords:
  - 파라체인
  - EVM
---

## 시작하기 전에

시작하기 전에 다음을 확인하세요:

<!--
  해당 내용이 담긴 문서가 생성되면 그 문서로 링크를 연결 해 주세요
-->

- [파라체인 구축하기](../tutorials/build/build-a-parachain.md)

- [인프라릴레이체인 구축하기](../tutorials/build/build-infra-relay-chain.md)

- [좀비넷 사용하기](../tutorials/test/simulate-parachains.md)

## EVM이란

EVM은 Ethereum Virtual Machine의 약자입니다. EVM은 이더리움 네트워크에서 스마트 컨트랙트를 실행하기 위한 런타임 환경입니다. 주요 특성 및 기능은 다음과 같습니다:

- **스마트 컨트랙트 실행**: EVM의 주요 역할은 스마트 컨트랙트를 실행하는 것입니다. 스마트 컨트랙트는 계약의 조건이 코드로 작성된 자체 실행 계약입니다. 이더리움에서는 주로 Solidity라는 프로그래밍 언어로 작성됩니다.

- **튜링 완전성**: EVM은 튜링 완전성을 가지고 있어 충분한 시간과 자원이 주어지면 어떤 알고리즘도 실행할 수 있습니다. 이로 인해 개발자는 스마트 컨트랙트에 복잡한 로직과 작업을 작성할 수 있습니다.

- **가스**: EVM에서의 작업 실행은 컴퓨터 파워가 필요하고, 사용자는 이 계산에 "가스"라는 단위로 비용을 지불합니다. 가스는 이더리움 네트워크 작업에 비용을 발생시킴으로써 스팸 및 악의적 활동을 방지합니다. 필요한 가스의 양은 수행되는 작업의 복잡성에 따라 다릅니다.

- **합의**: EVM에서 스마트 컨트랙트가 실행되고 상태가 변경되면 이더리움 네트워크의 모든 노드는 결과에 동의해야 합니다. 이를 통해 이더리움 원장이 모든 노드에서 일관성 있게 유지됩니다.

- **바이트 코드**: 개발자는 EVM이 직접 이해하는 언어로 스마트 컨트랙트를 작성하지 않습니다. 대신 Solidity와 같은 언어는 EVM 바이트 코드로 컴파일되며, EVM이 실행합니다.

- **분산화**: 중앙 집중식 시스템과 달리 EVM은 전 세계 수천 대의 기기에서 실행되어 이더리움을 분산화합니다. 이러한 분산화는 이더리움이 검열 저항성을 가지며 높은 내결함성을 가진다는 것을 보장합니다.

EVM은 이더리움 생태계에서 중요한 역할을 하며, 분산 애플리케이션(dApps)을 실행하고 Ether(ETH)와 같은 암호화 자산을 관리하는 데 필요한 인프라를 제공합니다.

## 다음 단계로 넘어가기

- [인프라EVM 체인 구축하기](../tutorials/service-chains/infra-evm-parachain/README.md)
