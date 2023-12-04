---
title: 인프라 블록체인 네트워크 참여자
description: 콜래이터와 밸리데이터의 역할에 대해 설명합니다. 
keywords:
  - 콜래이터
  - 밸리데이터
---

멀티체인 기반 인프라 블록체인 생태계에서 중요한 역할을 수행하는 참여자들을 소개합니다:
- 밸리데이터: 블록을 생성하며 공유된 보안(shared security)을 형성하여 릴레이 체인 및 파라체인을 보호하는 역할을 수행합니다.
- 콜래이터: 파라체인 블록을 생성하여 밸리데이터에게 전달하는 역할을 수행합니다.

![네트워크 참여자](/media/images/docs/infrablockchain/learn/architecture/network-participants.png)

## 밸리데이터

밸리데이터는 콜래이터로부터의 증명을 검증하며([파라-밸리데이터](#파라-밸리데이터)), 다른 밸리데이터와의 합의에 참여함으로써 릴레이 체인 및 파라체인을 보호합니다. 밸리데이터는 새로운 블록을 릴레이 체인에 추가하는 중요한 역할을 담당하며([블록 생성자](#블록-생성자)), 
이를 통해 [모든 파라체인 블록이 추가](./architecture.md#파라체인-프로토콜)됩니다.

 ***인프라 블록체인(InfraBlockchain)*** 밸리데이터들은 *[공유된 보안(shared security)](./architecture.md#공유된-보안shared-security)* 를 통해 전체 네트워크의 안전을 보장하기 떄문에 [XCM 을 활용하여 체인간 메세지](../xcm/xcm.md)(e.g 토큰 전송, 특정 팔렛의 트랜잭션 호출 등)를 주고 받을 수 있습니다. 밸리데이터는 각 파라체인이 고유한 규칙을 따르고, 신뢰할 수 있는 환경에서 파라체인 간에 메시지를 전달할 수 있도록 보장합니다.

## 파라-밸리데이터

### 개요

**_파라-밸리데이터_** 는 ***가용 및 검증(Availability and Validity)*** 프로토콜의 파라체인 단계에 참여하며, [candidate receipt](https://github.com/InfraBlockchain/infrablockspace-sdk/blob/822bc6c9706774a98122eb432f412b871a98a4bd/infrablockspace/primitives/src/v6/mod.rs#L521) 을 릴레이 체인에 제출하여 블록 작성자가 파라체인 블록에 대한 정보를 가용 및 검증 과정을 거처 릴레이 체인 블록에 포함시킬 수 있도록 합니다.

### Candidate Receipt 예시
```rust
pub struct CandidateReceipt<H = Hash> {
	pub descriptor: CandidateDescriptor<H>,
	pub commitments_hash: Hash,
}

pub struct CandidateDescriptor<H = Hash> {
	pub para_id: Id,
	pub relay_parent: H,
	pub collator: CollatorId,
	pub persisted_validation_data_hash: Hash,
	pub pov_hash: Hash,
	pub erasure_root: Hash,
	pub signature: CollatorSignature,
	pub para_head: Hash,
	pub validation_code_hash: ValidationCodeHash,
}
```
### 파라-밸리데이터 선정

파라-밸리데이터는 그룹으로 작동하며, 런타임에 의해 매 에포크마다 릴레이 체인에 연결된 모든 파라체인에 대한 파라체인 블록을 검증하기 위해 선택됩니다. 선택된 파라-밸리데이터는 (에포크당) 무작위로 선택된  [***인프라 블록체인(InfraBlockchain)*** 밸리데이터](./network-participants.md#밸리데이터) 중 하나로 검증에 참여하여 파라-밸리데이터 풀을 구성합니다.

### 파라-밸리데이터 역할

파라-밸리데이터는 할당된 일련의 파라체인 블록에 포함된 정보가 유효한지를 검증합니다. 그들은 콜래이터로부터 파라체인 블록 후보(candidate)와 그 유효성을 증명하는 [Proof-of-Validity(PoV)](https://github.com/InfraBlockchain/infrablockspace-sdk/blob/822bc6c9706774a98122eb432f412b871a98a4bd/cumulus/primitives/core/src/lib.rs#L155)를 함께 받습니다.

파라-밸리데이터는 블록 후보에 대한 첫 번째 유효성 검증을 수행합니다. 충분한 서명이 포함된 유효성 검증을 받은 후보는 백킹 가능(backable)한 것으로 간주됩니다.

## 블록 생성자

다른 밸리데이터의 유효성을 기반으로 릴레이 체인 블록을 생성하기 위해 합의 메커니즘에 참여하는 밸리데이터가 있습니다. 이러한 밸리데이터를 블록 생성자라고 하며, 합의 엔진(e.g BABE, Aura)에 의해 선택되며, 릴레이 체인에 포함할 각 파라체인에 대해 최대 하나의 백킹 가능한(backable) 후보를 기록할 수 있습니다. 릴레이 체인에 포함된 백킹 가능한 후보는 해당 릴레이 체인 포크의 백킹된 것으로 간주됩니다.

릴레이 체인 블록에서 블록 생성자는 이전 릴레이 체인 블록에서 부모 후보 데이터([Candidate Receipt](./network-participants.md#candidate-receipt-예시))를 가진 후보 파라체인 블록에 대해서만 포함합니다. **이는 파라체인이 유효한 체인을 따르도록 보장합니다.** 또한, 블록 작성자는 [erasure-coding](https://wiki.polkadot.network/docs/learn-parachains-protocol#erasure-codes) 청크를 가지고 있는 후보 파라체인 블록에 대해서만 포함합니다(가용성). 이는 시스템이 다음 라운드의 가용성 및 유효성 검사를 수행할 수 있도록 보장합니다.

## 기타

### 파라체인 블록 가용성

밸리데이터는 또한 이른바 **가용성 분산**에 기여합니다. 실제로, 후보가 릴레이 체인의 포크에서 백킹된 후(_backed_)에도 가용성이 보류(_pending availability_)되어 있으며, 파라체인의 일부로 완전히 포함되지 않습니다(임시적으로만 포함됨). 후보의 가용성에 관한 정보는 다음 릴레이 체인 블록에 기록됩니다. 충분한 정보가 있는 경우에만 후보는 완전한 파라체인 블록 또는 파라블록(parablock)으로 간주됩니다.

### 파라체인 블록 승인

밸리데이터는 또한 이른바 **승인 과정(approval)** 에 참여합니다. 파라블록이 가용하고 파라체인의 일부로 간주되더라도, 여전히 승인 대기(_pending approval_) 상태입니다. 파라-밸리데이터는 모든 밸리데이터의 작은 하위 집합이기 때문에, 특정 파라체인에 할당된 대다수의 파라-밸리데이터가 부정직할 가능성이 있기 때문입니다. 

### 보상

따라서 시스템의 처리량을 저하시킬 가능성이 있는 더 많은 파라-밸리데이터를 할당하는 것을 피하기 위해 파라블록의 보조 검증을 실행하는 것이 필요합니다.
검증이 실패한다면, 파라-밸리데이터는 벌점을 받으며 밸리데이터 풀에서 제외됨으로써 부정행위를 억제합니다. 그러나 성과가 좋은 경우, 해당 밸리데이터는 활동에 대한 보상으로 법정 화폐 기반의 [시스템 토큰](../protocol/system-token.md)을 블록 보상(트랜잭션 수수료 포함)을 받게 됩니다.

### 포크 선택(Fork Choice)

마지막으로, 밸리데이터는 GRANDPA 내에서 체인 선택 과정에 참여하여, 이용 가능하고 유효한 블록만이 최종화된 릴레이 체인에 포함되도록 보장합니다.

## Collator

### 콜래이터의 역할

콜래이터는 사용자로부터 파라체인에서 일어나는 모든 트랜잭션을 최대한  수집하고, 릴레이 체인 밸리데이터를 위해 상태 전이(state transition) 증명(PoV)를 생성합니다. 정상적인 상황에서, 콜래이터는 트랜잭션을 수집하고 실행하여 블록 후보(candidate)를 생성하고, 이를 PoV와 함께 파라체인 블록을 제안하는 하나 이상의 밸리데이터에게 제공합니다.

콜래이터는 다른 블록체인의 밸리데이터와 유사하지만, ***인프라 블록체인(InfraBlockchain)*** 밸리데이터들이 보안을 보장하고 있기 때문에 따로 보안에 대해 신경쓸 필요없이 [서비스에 특화된 블록체인](../../service-chains/README.md)을 구축할 수 있습니다. **파라체인 블록이 유효하지 않으면 밸리데이터에 의해 거부됩니다.** 각 파라체인에 할당된 파라-밸리데이터는 제출된 후보들의 유효성을 확인한 다음, 다른 파라-밸리데이터에 대한 유효성을 수집하고 집계합니다. 이 과정을 백킹(*candidate backing*)이라고 합니다. 파라-밸리데이터는 신뢰할 수 없는 콜래이터로부터 연관된 PoV를 가진 임의의 수의 파라체인 블록 후보를 받습니다. 후보는 적어도 할당된 밸리데이터의 2/3가 해당 후보에 대한 유효성을 검증 받았을 때 백킹 가능(_backable_)한 것으로 간주됩니다.

밸리데이터는 다음 순서대로 다음 조건을 성공적으로 확인해야 합니다:

1. 파라체인 후보 블록이 검증 데이터의 어떤 매개변수도 초과하지 않아야 합니다.
2. 콜래이터의 서명이 유효해야 합니다.
3. 파라체인 런타임을 실행함으로써 파라체인 후보 블록을 검증합니다.

해당 후보 블록이 릴레이 체인 블록에 포함될 수 있도록 지정된 기준을 충족하는 경우, 선택된 릴레이 체인 [블록 생성자](./network-participants.md#블록-생성자)는 각 파라체인에 대해 백킹 가능한 후보(_backable_) 중 하나를 선택하여 릴레이 체인 블록에 포함시킵니다(_inclusion_). 후보 블록이 백킹된 것(_backed_)으로 간주됩니다.

또한 콜래이터가 많을수록 더 나은 또는 더 안전하다는 가정은 옳지 않습니다. 오히려 너무 많은 콜래이터는 네트워크를 느리게 할 수 있습니다. 콜래이터가 가진 유일한 악의적인 행동은 트랜잭션 검열입니다. 검열을 방지하기 위해 파라체인은 중립적인 콜래이터가 있는 것만 보장하면 되며, **반드시 다수가 필요한 것은 아닙니다**. 이론적으로는 검열 문제는 정직한 콜래이터가 한 명만 있어도 해결됩니다.