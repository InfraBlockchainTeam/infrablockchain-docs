---
title: 테스트
description:
keywords:
---

어떤 종류의 체인을 구축하려고 하든, 당신은 아마도 단위 테스트와 디버깅을 위한 로컬 개발 환경부터 시작할 것입니다.
프로젝트가 진행됨에 따라, 체인 내 함수들이 사용하는 실행 시간과 저장 공간 요구 사항을 기준으로 다양한 조건에서 다른 작업의 성능을 평가하고자 할 것입니다.

만약 파라체인을 구축하고 있다면, ***인프라블록체인(InfraBlockchain)*** 과 같은 실제 운영 네트워크에 배포하기 전에 로컬 네트워크에서 하나 이상의 파라체인 작동을 시뮬레이션하고자 할 것입니다.

로컬 개발 네트워크에서는 노드를 완전히 제어할 수 있습니다.
노드를 시작하고 중지할 수 있습니다.
런타임을 임의로 수정, 재컴파일, 업그레이드할 수 있습니다.
원하는 만큼 저장된 상태에 쓰거나 지울 수 있습니다.
미리 정의된 계정과 팔렛을 사용하여 시도해 볼 수 있습니다.
하지만 테스트는 더 많은 사람들이 당신의 체인을 이용할 수 있도록 하고 경제적으로 실현 가능하게 만드는 열쇠입니다.

이 섹션에서는 로컬 개발에서 실제 테스트 네트워크로의 배포, 궁극적으로는 ***인프라블록체인(InfraBlockchain)*** 생태계의 일부로서의 프로덕션으로 전환하는 데 도움이 되는 블록체인 로직 테스트에 대한 도구와 기법을 소개합니다.

- **[단위 테스트](./unit-testing.md)**: 개별 함수나 코드 모듈을 검증하는 단위 테스트를 실행하기 위해 Rust 테스팅 프레임워크와 모의 런타임 환경을 사용하는 방법을 설명합니다.

- **[디버그](./debug.md)**: Rust 로깅 함수를 사용하여 런타임을 디버깅하는 방법을 설명합니다.

- **[벤치마크](./benchmark.md)**: 벤치마킹 프레임워크를 사용하여 코드의 함수 호출 성능을 평가하고 실행 시간을 정확하게 반영하기 위해 트랜잭션 가중치를 조정하는 역할을 설명합니다.

- **[파라체인 시뮬레이션](./simulate-parachains.md)**: 밸리데이터, 파라체인 콜레이터 노드 및 XCM 메시징을 포함한 릴레이 체인 네트워크를 시뮬레이션하기 위해 로컬 테스트 네트워크를 설정하는 방법을 안내합니다.

- **[런타임 확인](./check-runtime.md)**: `try-runtime` 커맨드 라인 도구를 사용하여 체인 상태의 프로덕션 스냅샷에 대해 런타임 상태를 테스트하는 방법을 설명합니다.