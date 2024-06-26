---
title: 제작할 것을 결정하세요
description:
keywords:
  - 스마트 컨트랙트
  - 커스텀 팔레트
  - 커스텀 런타임
  - 파라체인
  - 파라스레드
---

애플리케이션을 설계할 때 가장 먼저 해야 할 결정 중 하나는 사용할 접근 방식입니다.
예를 들어, 프로젝트를 스마트 컨트랙트, 개별 팔렛, 사용자 정의 런타임 또는 파라체인 형태로 제공할지 결정해야 합니다.
무엇을 구축할지에 대한 결정은 다른 결정들에 거의 영향을 미칠 것입니다.
무엇을 구축할지에 대한 초기 결정을 돕기 위해, 이 섹션에서는 사용 가능한 옵션, 그들 간의 차이점 및 다른 접근 방식을 선택하는 이유에 대해 강조합니다.

## 스마트 컨트랙트

많은 개발자들은 스마트 컨트랙트에 익숙하며, 그들의 프로젝트가 스마트 컨트랙트 모델에 적합하다고 생각하기 쉽습니다.
그러나 스마트 컨트랙트 접근 방식을 결정할 때 고려해야 할 이점과 단점이 있습니다.

### 스마트 컨트랙트는 블록체인 규칙을 준수해야 합니다

스마트 컨트랙트는 특정 체인에 배포되고 특정 체인 주소에서 실행되는 명령입니다.
스마트 컨트랙트는 블록체인이 부과하는 규칙이나 제한 사항을 준수해야 합니다.
예를 들어, 스토리지 접근을 제한하거나 특정 유형의 트랜잭션을 방지할 수 있습니다.

또한, 스마트 컨트랙트를 수용하는 블록체인은 일반적으로 코드가 신뢰할 수 없는 출처(악의적인 행위자 또는 경험 부족한 개발자)에서 온 것으로 간주합니다.
신뢰할 수 없는 코드가 블록체인 작업을 방해하지 못하도록 하기 위해, 악의적이거나 오류가 있는 스마트 컨트랙트가 할 수 있는 작업을 제한할 수 있습니다.
예를 들어, 스마트 컨트랙트 안에서 사용하는 연산 및 스토리지에 대한 요금을 부과하거나 미터링을 강제할 수 있습니다.
스마트 컨트랙트 실행에 대한 요금과 규칙은 블록체인의 재량에 따라 결정됩니다.

### 스마트 컨트랙트와 상태

스마트 컨트랙트는 격리된 환경에서 실행되는 것으로 생각할 수 있습니다.
스마트 컨트랙트는 블록체인 스토리지나 다른 스마트 컨트랙트의 스토리지를 직접 수정하지 않습니다.
일반적으로, 스마트 컨트랙트는 자체 상태만 수정하고 다른 스마트 컨트랙트이나 런타임 함수를 호출하지 않습니다.
스마트 컨트랙트 실행에는 일반적으로 추가적인 오버헤드가 있으며, 스마트 컨트랙트의 오류로 인해 실행이 실패하면 상태가 업데이트되지 않도록 하기 위해 트랜잭션을 되돌릴 수 있도록 보장합니다.

### 스마트 컨트랙트 사용 시나리오

스마트 컨트랙트에는 제한 사항이 있지만, 프로젝트가 스마트 컨트랙트를 사용하는 것이 유리한 경우도 있습니다.
예를 들어, 스마트 컨트랙트는 진입 장벽이 낮고 짧은 시간 내에 구축하고 배포할 수 있는 경우가 많습니다.
개발 시간을 단축함으로써 제품-시장 적합성을 판단하고 빠르게 반복할 수 있는 이점을 얻을 수 있습니다.

마찬가지로, Solidity와 같은 언어를 사용하여 스마트 컨트랙트를 구축하는 데 익숙하다면, 프로젝트의 학습 곡선과 시장 진입 시간을 단축시킬 수 있습니다.
스마트 컨트랙트는 배포된 체인의 기능을 준수하기 때문에, 블록체인 인프라나 경제에 대해 걱정할 필요 없이 스마트 컨트랙트의 응용 프로그램 로직 구현에 좀 더 집중할 수 있습니다.

파라체인을 구축할 계획이라면, 스마트 컨트랙트를 사용하여 기능이나 프로토타입으로 구현할 수 있습니다.
런타임 개발자인 경우, 스마트 컨트랙트을 통합하여 커뮤니티가 런타임 로직에 대한 액세스 권한을 부여하지 않고 확장 기능을 개발할 수 있습니다.
또한, 스마트 컨트랙트를 사용하여 미래의 런타임 변경을 테스트할 수 있습니다.

일반적으로, 스마트 컨트랙트를 사용하여 프로젝트를 구축할 때 다음 특성을 고려해야 합니다:

- 스마트 컨트랙트는 기반이 되는 체인에 내장된 보호 기능으로 인해 네트워크에 대해 더 안전하지만, 이러한 보호 기능이 부과하는 제한, 제한 사항 또는 계산 오버헤드에 대해 제어할 수 없습니다.
- 기반이 되는 체인이 악용에 대한 기본 경제적 인센티브를 제공하지만, 요금 및 미터링 시스템은 기반이 되는 체인에서 정의됩니다.
- 코드 복잡성과 배포 시간 측면에서 진입 장벽이 낮습니다.
- 프로토타이핑, 테스트 및 커뮤니티 참여를 위한 격리된 환경을 제공할 수 있습니다.
- 기존 네트워크를 활용하여 배포 및 유지 관리 오버헤드가 낮습니다.

다음은 스마트 컨트랙트를 사용하는 사용 사례의 예입니다:

- 기존 탈중앙화 거래소(Dex)에 파생 상품 추가
- 커스텀 거래 알고리즘 구현
- 특정 당사자 간 스마트 컨트랙트 로직 정의
- 파라체인으로 변환하기 전에 애플리케이션 프로토타입 및 테스트
- 기존 체인에 레이어-2 토큰 및 커스텀 자산 도입

### 스마트 컨트랙트 지원

***인프라블록체인(InfraBlockchain)*** 의 릴레이 체인은 스마트 컨트랙트를 지원하지 않습니다.
그러나 ***인프라블록체인(InfraBlockchain)*** 에 연결된 파라체인은 임의의 상태 전이(state transition)을 지원할 수 있으므로, 어떤 파라체인이든 스마트 컨트랙트 배포의 잠재적인 플랫폼이 될 수 있습니다.
예를 들어, 현재 ***인프라블록체인(InfraBlockchain)*** 생태계에는 다양한 유형의 스마트 컨트랙트 배포를 지원하는 여러 파라체인이 있습니다.
***인프라블록체인(InfraBlockchain)*** 생태계에서 스마트 컨트랙트를 개발하려면 먼저 구축할 스마트 컨트랙트 유형을 결정하고 해당 유형의 스마트 컨트랙트를 지원하는 파라체인을 식별해야 합니다.
Substrate는 두 가지 유형의 스마트 컨트랙트를 지원하기 위한 도구를 제공합니다:

- FRAME 라이브러리의 `contracts` 팔렛은 WebAssembly로 컴파일된 스마트 컨트랙트를 실행할 수 있도록 Substrate 기반 체인을 활성화합니다. 스마트 컨트랙트를 작성하는 데 사용된 언어에 관계없이 실행할 수 있습니다.
- Frontier 프로젝트의 `evm` 팔렛은 Solidity로 작성된 Ethereum 가상 머신(EVM) 스마트 컨트랙트를 실행할 수 있도록 Substrate 기반 체인을 활성화합니다.

### 스마트 컨트랙트 탐색

프로젝트가 스마트 계약으로 적합한 경우, 시작할 수 있는 몇 가지 간단한 예제를 다음 자습서에서 확인할 수 있습니다:

- [스마트 컨트랙트 개발](../tutorials/smart-contracts/README.md)
- [EVM 계정에 액세스](../tutorials/integrate-with-tools/README.md)

## 개별 팔렛

일부 경우에는 애플리케이션 로직을 독립적인 팔렛으로 구현하고 기능을 커뮤니티에 라이브러리로 제공하여 커스텀 런타임을 직접 구축하는 대신 개별 팔렛으로 구현할 수도 있습니다.
예를 들어, 애플리케이션별 블록체인을 배포하고 관리하기 원하지 않는 경우, 하나 이상의 개별 팔렛을 구축하여 Substrate 기반 체인 전체에 걸쳐 널리 사용되는 기능을 제공하거나 기존 기능을 개선하거나 ***인프라블록체인(InfraBlockchain)*** 생태계의 표준을 정의할 수 있습니다.
개별 팔렛은 일반적으로 FRAME을 사용하여 쉽게 개발할 수 있으며, Substrate 체인에서 쉽게 통합할 수 있습니다.

### 올바른 코드 작성

팔렛은 스마트 컨트랙트가 제공하는 어떤 유형의 보호 또는 안전장치도 기본적으로 제공하지 않는다는 점에 유의해야 합니다.
팔렛에서는 런타임 개발자가 구현할 수 있는 로직을 제어합니다.
필요한 메서드, 스토리지 항목, 이벤트 및 오류를 제공합니다.
팔렛은 기본적으로 요금이나 미터링 시스템을 도입하지 않습니다.
팔렛 로직이 나쁜 동작을 허용하거나 팔렛이 사용되는 네트워크를 공격에 취약하게 만들지 않도록 해야 합니다.
이러한 내장된 안전장치의 부재는 코드를 올바르게 작성해야 한다는 많은 책임을 의미합니다.

### 런타임 개발 외의 팔렛

팔렛 작성은 종종 런타임 개발의 시작점이며, 완전한 블록체인 애플리케이션을 구축하지 않고도 기존 팔렛과 코딩 패턴을 실험할 수 있는 기회를 제공합니다.
개별 팔렛은 자체 응용 프로그램을 작성하지 않고도 프로젝트에 기여할 수 있는 대안적인 방법을 제공합니다.

팔렛을 작성하고 테스트하는 것은 일반적으로 대규모 애플리케이션을 구축하는 중간 단계입니다.
그러나 개별 팔렛이 전체 생태계에 가치를 제공할 수 있는 많은 예제가 있습니다.

하나의 팔렛을 구축하더라도, 해당 팔렛을 런타임 맥락에서 테스트해야 합니다.
개별 팔렛을 개발하는 가장 큰 단점은 사용되는 런타임의 다른 부분을 제어할 수 없다는 것입니다.
팔렛을 격리된 코드로 처리하면 개선할 기회를 놓칠 수 있습니다.
또한, FRAME이나 Substrate의 변경 사항이 개별 팔렛의 유지 관리 문제를 일으킬 수 있습니다.

### 개별 팔렛 구축에 대해 알아보기

프로젝트가 개별 팔렛으로 적합한 경우, 다음 섹션에서 시작할 수 있는 몇 가지 간단한 예제를 확인할 수 있습니다:

- [커스텀 팔렛](../learn/frame/custom-pallets.md)
- [팔렛 사용하기](../tutorials/build-application-logic/README.md)
- [컬렉터블 워크샵](../tutorials/collectibles-workshop/README.md)

## 커스텀 런타임

대부분의 경우, 커스텀 런타임을 구축하는 결정은 ***인프라블록체인(InfraBlockchain)*** 생태계의 일부로 파라체인이라는 애플리케이션별 블록체인을 구축하고 배포하기 위한 중요한 단계입니다.
Substrate과 FRAME을 사용하여 완전한 커스텀 런타임을 개발할 수 있습니다.
커스텀 런타임을 사용하면 경제적 인센티브, 거버넌스, 합의 및 리소스 관리를 포함한 애플리케이션의 모든 측면을 완전히 제어할 수 있습니다.

이러한 기능을 위해 플러그 가능한 모듈을 제공하는 팔렛이 있습니다.
그러나 어떤 모듈을 사용할지, 어떻게 수정하여 필요에 맞게 조정할지, 어떤 부분에서 커스텀 모듈이 필요한지 결정하는 것은 개발자에게 달려 있습니다.
각 노드에서 실행되는 기본 로직을 완전히 제어하기 때문에, 스마트 컨트랙트가나 개별 팔렛을 작성하는 것보다 코딩 기술과 경험이 더 필요합니다.

개별 팔렛과 마찬가지로, 커스텀 런타임은 악의적인 행위자나 잘못된 코드로 인한 피해를 방지하기 위한 내장된 안전장치를 제공하지 않습니다.
네트워크와 사용자 커뮤니티를 충분히 보호하기 위해 런타임 로직에서 리소스 사용량을 올바르게 평가하고 트랜잭션 수수료를 적용해야 합니다.

스마트 컨트랙트나 개별 팔렛과 달리 커스텀 런타임은 완전한 블록체인입니다.
커스텀 런타임을 다른 개발자가 스마트 컨트랙트를 실행할 수 있도록 확장할 수 있도록 선택할 수 있습니다.
커스텀 런타임을 사용하면 스마트 컨트랙트가나 개별 팔렛에서 제공할 수 있는 것보다 더 복잡한 기능과 사용자 상호작용을 제공할 수 있습니다.

### 커스텀 런타임 구축에 대해 알아보기

개별 팔렛보다는 커스텀 런타임을 구축하려면, [팔렛 사용하기](../tutorials/build-application-logic/README.md) 및 [컬렉터블 워크샵](../tutorials/collectibles-workshop/README.md)과 같은 몇 가지 간단한 예제로 시작할 수 있습니다.
그러나 솔로 체인이나 파라체인에 대한 개념 증명으로 커스텀 런타임을 구축하려면 런타임 구성 요소와 FRAME 팔렛에 대한 보다 포괄적이고 심층적인 이해가 필요합니다.
가장 관련 있는 주제는 [빌드](../build/README.md) 및 [테스트](../../../tutorials/test/README.md)에서 찾을 수 있으며, 다음 섹션에서 확인할 수 있습니다:

- [런타임 스토리지 구조](../learn/frame/runtime-storage.md)
- [트랜잭션, Weight 및 수수료](../learn/frame/tx-weights-fees.md)
- [FRAME 팔렛](../learn/frame/frame-pallets.md)
- [FRAME 매크로](../learn/frame/frame-macros.md)

## 파라체인

커스텀 런타임은 개인 네트워크나 솔로 체인의 비즈니스 로직으로 독립적으로 존재할 수 있지만, 프로젝트를 생산 체인으로 만들고자 하는 경우, 비즈니스 로직과 상태 전이 기능을 파라체인이나 파라스레드로 배포하는 것이 여러 가지 이점이 있습니다.

파라체인과 파라스레드는 독립적인 1차 블록체인으로 작동합니다.
각 파라체인은 고유한 로직을 가지며, 생태계의 다른 체인과 병렬로 실행됩니다.
생태계의 모든 체인은 네트워크의 공유 보안, 거버넌스, 확장성 및 상호운용성의 이점을 누릅니다.

### 파라체인은 최대한의 유연성을 제공합니다

파라체인으로 프로젝트를 개발하면 체인의 설계 및 기능에 대해 많은 자유와 유연성을 가질 수 있습니다.
무엇을 구축할지는 완전히 당신에게 달려 있습니다.
예를 들어, 체인에 어떤 데이터를 체인 상에 저장할지 또는 저장하지 않을지 정할 수 있습니다.
자체적인 경제적 기본 요소, 트랜잭션 요구 사항, 수수료 정책, 거버넌스 모델, 트레저리 계정 및 액세스 제어 규칙을 정의할 수 있습니다.
파라체인은 트랜잭션 당 오버헤드를 최소화하거나 최대화할 수 있으며, 업그레이드와 최적화를 통해 시간이 지남에 따라 파라체인을 발전시킬 수 있습니다.
유일한 요구 사항은 파라체인이나 파라스레드가 ***인프라블록체인(InfraBlockchain)*** API와 호환되어야 한다는 것입니다.

### 파라체인 리소스 요구 사항 계획

파라체인으로 프로젝트를 개발하면 개인 체인이나 솔로 체인보다 보다 안전한 방식으로 더 넓은 커뮤니티에 기능을 제공할 수 있습니다.
그러나 제품 수준의 파라체인을 구축하려면 다음과 같은 추가 요구 사항을 염두에 두어야 합니다:

- 프로그래밍 언어로 Rust를 사용하거나 UX 디자인에 대한 배경을 가진 충분한 기술과 경험을 갖춘 개발 팀이 필요합니다.

  파라체인 개발은 다른 옵션보다 더 많은 리소스가 필요할 수 있습니다.

- 마케팅, 홍보 또는 생태계 개발 프로그램을 통해 커뮤니티를 구축해야 합니다.

- 인프라 및 네트워크 유지 관리를 위한 리소스가 필요합니다.

  파라체인은 완전한 블록체인입니다.
  릴레이 체인은 프로젝트에 대한 보안과 합의를 제공하지만, 체인과 네트워크 인프라를 유지 관리해야 합니다.
  개발자 운영(DevOps) 외에도, 파라체인 슬롯을 보호하고, 크라우드론 또는 경매 전략을 설계하고, 슬롯을 확장하기 위한 충분한 리소스를 축적하기 위한 작업을 해야 합니다.

- 샌드박스나 시뮬레이션된 네트워크에서 체인 작업을 테스트하고 검증하는 데 충분한 시간이 필요합니다.

### 파라체인 사용 사례

일반적으로, 파라체인은 복잡한 작업이 필요한 경우에 프로젝트를 구축해야 합니다. 파라체인은 트랜잭션의 실행을 더 빠르고 효율적으로 처리할 수 있습니다.
예를 들어, 다음과 같은 파라체인을 만들 수 있습니다: 

- 탈중앙화 금융(DeFi) 애플리케이션
- 디지털 지갑
- 사물 인터넷(IoT) 애플리케이션
- 게임 애플리케이션
- 웹 3.0 인프라

### 파라체인 구축 알아보기

***인프라블록체인(InfraBlockchain)*** 릴레이 체인 생태계의 보안, 거버넌스 및 상호운용성을 활용하기 위해 커스텀 런타임을 파라체인으로 배포하려면, 초기 테스트를 위해 로컬에서 구축하고 자체 테스트 네트워크를 설정할 수 있습니다.

시작할 수 있는 몇 가지 예제를 확인하려면 다음 섹션을 참조하세요:

- [네트워크에 파라체인 연결](../../../tutorials/build/build-a-parachain.md)
- [테스트 네트워크에서 파라체인 시뮬레이션](../../../tutorials/test/simulate-parachains.md)
- [파라체인](../../../learn/architecture/parachain/README.md)

무엇을 구축할 수 있는지 자세히 알아보려면 다음 리소스를 참조하세요:

- [인프라블록체인 빌드하기](../../../tutorials/build/README.md)
- [스마트 컨트랙트](../tutorials/smart-contracts/README.md)