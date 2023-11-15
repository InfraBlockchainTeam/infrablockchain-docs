---
title: 파라체인 테스트 네트워크 설정하기
description: 검증자와 파라체인 콜레이터 노드를 시뮬레이션하는 로컬 테스트 네트워크를 설정하는 방법을 설명합니다.
keywords:
  - 파라체인
  - Polkadot
  - 테스트넷
  - 콜레이터 노드
  - 검증자 노드
  - 릴레이 체인
  - 좀비넷
---

`zombienet` 명령행 도구를 사용하여 검증자와 파라체인 콜레이터 노드를 시뮬레이션하는 로컬 테스트 네트워크를 설정할 수 있습니다.
테스트 네트워크를 구성하여 여러 검증자와 다수의 파라체인을 포함시킬 수 있습니다.

Zombienet이 제공하는 기능의 전체 목록을 보려면 [README](https://github.com/paritytech/zombienet)를 참조하십시오.

이 튜토리얼에서는 다음과 같은 구성으로 테스트 네트워크를 설정하는 방법을 설명합니다:

- 검증자 4개
- 파라체인 2개
- 파라체인 당 콜레이터 1개
- 파라체인 간 메시지 교환을 가능하게 하는 HRMP 채널 1개

## 바이너리 파일이 있는 작업 폴더 준비하기

`zombienet` 명령줄 인터페이스는 테스트 네트워크의 특성을 지정하는 구성 파일을 사용합니다. 이 구성 파일에는 사용할 바이너리 파일, 도커 이미지 또는 쿠버네티스 배포의 이름과 위치 등의 정보가 포함됩니다.

이 튜토리얼에서는 릴레이 체인과 콜레이터 바이너리 파일을 사용하는 테스트 네트워크를 구성하는 방법을 설명하므로, 테스트 네트워크를 설정하기 위해 필요한 바이너리 파일이 있는 작업 폴더를 준비하는 것이 첫 번째 단계입니다.

테스트 네트워크에 필요한 바이너리 파일을 포함하는 작업 폴더를 준비하려면 다음 단계를 따르세요:

1. 필요한 경우 컴퓨터에서 새 터미널 셸을 엽니다.

2. 홈 디렉터리로 이동하고 테스트 네트워크를 생성하는 데 필요한 바이너리 파일을 보관할 새 폴더를 만듭니다.

   예를 들어:

   ```bash
   mkdir bin
   ```

   Linux에서 테스트 네트워크를 설정하는 경우, [릴리스](https://github.com/paritytech/polkadot/releases)에서 Polkadot 바이너리 파일을 작업 폴더로 다운로드할 수 있습니다.
   macOS에서 테스트 네트워크를 설정하거나 바이너리 파일을 직접 컴파일하려는 경우, 다음 단계로 진행하세요.

3. 다음 명령을 실행하여 Polkadot 저장소를 복제합니다:

   ```bash
   git clone --depth 1 --branch release-v1.0.0 https://github.com/paritytech/polkadot-sdk.git
   ```

   릴리스 브랜치는 `release-v<n.n.n>` 형식을 따릅니다.
   예를 들어, 이 튜토리얼에서 사용하는 릴리스 브랜치는 `release-v1.0.0`입니다.
   `release-v1.0.0` 대신 더 최신의 릴리스 브랜치를 사용할 수 있습니다.
   최근 릴리스에 대한 정보와 각 릴리스에 포함된 내용에 대한 정보는 [릴리스](https://github.com/paritytech/polkadot/releases) 탭에서 확인할 수 있습니다.

4. 다음 명령을 실행하여 `polkadot` 디렉터리의 루트로 이동합니다:

   ```bash
   cd polkadot
   ```

5. 다음 명령을 실행하여 릴레이 체인 노드를 컴파일합니다:

   ```bash
   cargo build --release
   ```

   노드를 컴파일하는 데는 15분에서 60분이 소요될 수 있습니다.

6. 다음 명령을 실행하여 Polkadot 바이너리 파일을 작업 `bin` 폴더로 복사합니다:

   ```bash
   cp ./target/release/polkadot ../bin/polkadot-v1.0.0
   ```

   이 예시에서는 일반적으로 `bin` 폴더의 파일을 정리하기 위해 바이너리 파일 이름에 `polkadot` 버전을 추가하는 것이 좋은 관행입니다.

7. 홈 디렉터리로 이동합니다.

### 첫 번째 파라체인 바이너리 파일 추가하기

작업 폴더에는 릴레이 체인의 바이너리 파일이 있지만, 파라체인 콜레이터 노드의 바이너리 파일도 필요합니다.
`substrate-parachain-template` 저장소를 복제하여 파라체인 콜레이터 바이너리 파일을 작업 폴더에 추가할 수 있습니다.
기본적으로 `substrate-parachain-template`을 컴파일하면 `paraId`가 1000으로 구성된 파라체인 콜레이터 바이너리 파일이 생성됩니다.
이 `paraId`를 테스트 네트워크의 첫 번째 파라체인에 사용할 수 있습니다.

작업 폴더에 첫 번째 파라체인 바이너리 파일을 추가하려면 다음 단계를 따르세요:

1. 다음 명령을 실행하여 `substrate-parachain-template` 저장소를 복제합니다:

   ```bash
   git clone https://github.com/substrate-developer-hub/substrate-parachain-template
   ```

2. 다음 명령을 실행하여 파라체인 템플릿 디렉터리의 루트로 이동합니다:

   ```bash
   cd substrate-parachain-template
   ```

3. 다음 명령을 실행하여 릴레이 체인을 구성하는 데 사용한 릴리스 브랜치와 일치하는 릴리스 브랜치로 전환합니다.

   예를 들어:

   ```bash
   git checkout polkadot-v1.0.0
   ```

4. 다음 명령을 실행하여 파라체인 템플릿 콜레이터를 컴파일합니다:

   ```bash
   cargo build --release
   ```

   이제 paraId 1000에 대한 파라체인 콜레이터 바이너리 파일이 준비되었습니다.

5. 다음 명령을 실행하여 파라체인 바이너리 파일을 작업 `bin` 폴더로 복사합니다:

   ```bash
   cp ./target/release/parachain-template-node ../bin/parachain-template-node-v1.0.0-1000
   ```

   이 예시에서는 일반적으로 `bin` 폴더의 파일을 정리하기 위해 바이너리 파일 이름에 버전과 `paraId`를 추가하는 것이 좋은 관행입니다.

### 두 번째 파라체인 바이너리 파일 추가하기

각 파라체인은 고유한 식별자를 가져야 하므로, 테스트 네트워크에 두 번째 파라체인을 생성하려면 기본 체인 스펙을 편집하여 다른 `paraId`를 지정해야 합니다.

작업 폴더에 두 번째 파라체인 바이너리 파일을 추가하려면 다음 단계를 따르세요:

1. 텍스트 편집기에서 `substrate-parachain-template/node/src/chain_spec.rs` 파일을 엽니다.

2. 로컬 테스트넷 구성을 기본 식별자 `1000` 대신 `1001`을 사용하도록 변경합니다.

   `local_testnet_config` 함수에서 변경해야 할 부분이 두 군데 있습니다:

   ```rust
   pub fn local_testnet_config() -> ChainSpec {
      // Give your base currency a unit name and decimal places
      let mut properties = sc_chain_spec::Properties::new();
      properties.insert("tokenSymbol".into(), "UNIT".into());
      properties.insert("tokenDecimals".into(), 12.into());
      properties.insert("ss58Format".into(), 42.into());

      ChainSpec::from_genesis(
        // Name
        "Local Testnet",
        // ID
        "local_testnet",
        ChainType::Local,
        move || {
            testnet_genesis(
              // initial collators.
              vec![
                (
                  get_account_id_from_seed::<sr25519::Public>("Alice"),get_collator_keys_from_seed("Alice"),
                ),
                (
                  get_account_id_from_seed::<sr25519::Public>("Bob"),
                  get_collator_keys_from_seed("Bob"),
                ),
              ],
              vec![
                  get_account_id_from_seed::<sr25519::Public>("Alice"),get_account_id_from_seed::<sr25519::Public>("Bob"),get_account_id_from_seed::<sr25519::Public>("Charlie"),get_account_id_from_seed::<sr25519::Public>("Dave"),get_account_id_from_seed::<sr25519::Public>("Eve"),get_account_id_from_seed::<sr25519::Public>("Ferdie"),get_account_id_from_seed::<sr25519::Public>("Alice//stash"),get_account_id_from_seed::<sr25519::Public>("Bob//stash"),get_account_id_from_seed::<sr25519::Public>("Charlie//stash"),get_account_id_from_seed::<sr25519::Public>("Dave//stash"),get_account_id_from_seed::<sr25519::Public>("Eve//stash"),get_account_id_from_seed::<sr25519::Public>("Ferdie//stash"),
              ],
            1001.into(),
          )
        },
        // Bootnodes
        Vec::new(),
        // Telemetry
        None,
        // Protocol ID
        Some("template-local"),
        // Fork ID
        None,
        // Properties
        Some(properties),
        // Extensions
        Extensions {
            relay_chain: "rococo-local".into(), // You MUST set this to the correct network!
            para_id: 1001,
      },
    )
   ```

3. 변경 사항을 저장하고 체인 스펙 파일을 닫습니다.

4. 다음 명령을 실행하여 수정된 체인 스펙으로 노드를 컴파일합니다:

   ```bash
   cargo build -release
   ```

5. 다음 명령을 실행하여 새로운 파라체인 콜레이터 바이너리 파일을 작업 `bin` 폴더로 복사합니다:

   ```bash:
   cp ./target/release/parachain-template-node ../bin/parachain-template-node-v1.0.0-1001
   ```

## 테스트 네트워크 설정하기

작업 폴더에 필요한 모든 바이너리 파일이 준비되었으므로, Zombienet이 사용할 테스트 네트워크의 설정을 구성할 준비가 되었습니다.

Zombienet을 다운로드하고 구성하려면 다음 단계를 따르세요:

1. Linux 또는 macOS 운영 체제용 적절한 [Zombienet 실행 파일](https://github.com/paritytech/zombienet/releases)을 다운로드합니다.

   보안 설정에 따라 실행 파일에 대한 액세스를 명시적으로 허용해야 할 수 있습니다.

   실행 파일을 시스템 전역에서 사용할 수 있도록 하려면, 다운로드한 실행 파일에 대해 다음과 유사한 명령을 실행하세요:

   ```bash
   chmod +x zombienet-macos
   cp zombienet-macos /usr/local/bin
   ```

2. 다음 명령을 실행하여 Zombienet이 올바르게 설치되었는지 확인합니다:

   ```bash
   ./zombienet-macos --help
   ```

   명령줄 도움말이 표시되면 Zombienet을 구성할 준비가 된 것입니다.

3. 다음 명령을 실행하여 Zombienet을 위한 구성 파일을 생성합니다:

   ```bash
   touch config.toml
   ```

   이 구성 파일을 사용하여 다음 정보를 지정합니다:

   - 테스트 네트워크의 바이너리 파일 위치
   - 사용할 릴레이 체인 스펙(`rococo-local`)
   - 네 개의 릴레이 체인 검증자에 대한 정보
   - 테스트 네트워크에 포함된 파라체인 식별자
   - 각 파라체인의 콜레이터에 대한 정보

   예를 들어:

   ```toml
   [relaychain]
   default_command = "./bin/polkadot-v1.0.0"
   default_args = [ "-lparachain=debug" ]

   chain = "rococo-local"

      [[relaychain.nodes]]
      name = "alice"
      validator = true

      [[relaychain.nodes]]
      name = "bob"
      validator = true

      [[relaychain.nodes]]
      name = "charlie"
      validator = true

      [[relaychain.nodes]]
      name = "dave"
      validator = true

   [[parachains]]
   id = 1000
   cumulus_based = true

      [parachains.collator]
      name = "parachain-A-1000-collator-01"
      command = "./bin/parachain-template-node-v1.0.0-1000"

   [[parachains]]
   id = 1001
   cumulus_based = true

      [parachains.collator]
      name = "parachain-B-1001-collator-01"
      command = "../bin/parachain-template-node-v1.0.0-1001"
   ```

4. 변경 사항을 저장하고 파일을 닫습니다.
5. 다음 명령을 실행하여 이 구성 파일을 사용하여 테스트 네트워크를 시작합니다:

   ```bash
   ./zombienet-macos spawn config.toml -p native
   ```

   명령은 시작된 테스트 네트워크 노드에 대한 정보를 표시합니다.
   모든 노드가 실행된 후에는 [Polkadot/Substrate Portal](https://polkadot.js.org/apps)을 열고 노드 엔드포인트에 연결하여 노드와 상호 작용할 수 있습니다.

## 메시지 전달 채널 열기

테스트 네트워크가 준비되었으므로, 파라체인 A(1000)와 파라체인 B(1001) 사이의 수평 릴레이 메시지 전달 채널을 열어 파라체인 간 통신을 가능하게 할 수 있습니다.
채널은 단방향이므로 다음 작업을 수행해야 합니다:

- 파라체인 A(1000)에서 파라체인 B(1001)로 채널 열기 요청을 보냅니다.
- 파라체인 B(1001)에서 요청을 수락합니다.
- 파라체인 B(1001)에서 파라체인 A(1000)로 채널 열기 요청을 보냅니다.
- 파라체인 A(1000)에서 요청을 수락합니다.

Zombienet은 테스트 목적으로 구성 파일에 기본 채널 설정을 포함하여 이러한 채널을 열기를 간소화합니다.

테스트 네트워크의 파라체인 간 통신을 설정하려면 다음 단계를 수행하세요:

1. 텍스트 편집기에서 `config.toml` 파일을 엽니다.

2. 다음과 유사한 채널 정보를 구성 파일에 추가합니다:

   ```toml
   [[hrmpChannels]]
   sender = 1000
   recipient = 1001
   maxCapacity = 8
   maxMessageSize = 8000

   [[hrmpChannels]]
   sender = 1001
   recipient = 1000
   maxCapacity = 8
   maxMessageSize = 8000
   ```

   **maxCapacity**와 **maxMessageSize**에 설정하는 값은 릴레이 체인의 `hrmpChannelMaxCapacity` 및 `hrmpChannelMaxMessageSize` 매개변수에 정의된 값보다 크지 않아야 합니다.

   [Polkadot/Substrate Portal](https://polkadot.js.org/apps/)에서 현재 릴레이 체인의 구성 설정을 확인하려면 다음 단계를 수행하세요:

   - **개발자**를 클릭하고 **Chain State**를 선택합니다.
   - **configuration**을 선택한 다음 **activeConfig()**를 선택합니다.
   - 다음 매개변수 값을 확인합니다:

     ```text
     hrmpChannelMaxCapacity: 8
     hrmpChannelMaxTotalSize: 8,192
     hrmlChannelMaxMessageSize: 1,048,576
     ```

3. 변경 사항을 저장하고 파일을 닫습니다.
4. 다음 명령을 실행하여 Zombienet을 다시 시작합니다:

   ```bash
   ./zombienet-macos spawn config.toml -p native
   ```

   이제 파라체인 A(1000)와 파라체인 B(1001) 사이에 양방향 HRMP 채널이 열린 테스트 네트워크가 준비되었습니다.

   [Polkadot/Substrate Portal](https://polkadot.js.org/apps)을 사용하여 파라체인에 연결하고 메시지를 보낼 수 있습니다.

5. **개발자**를 클릭하고 **Extrinsics**를 선택합니다.

6. **polkadotXcm**을 선택한 다음 **sent(dest, message)**를 선택하여 전송할 XCM 메시지를 작성합니다.

   XCM 메시지는 다른 트랜잭션과 마찬가지로 메시지를 보내는 사람이 작업을 실행하기 위해 지불해야 합니다.
   필요한 모든 정보는 메시지 자체에 포함되어야 합니다.
   HRMP 채널을 열고 나서 XCM을 사용하여 메시지를 작성하는 방법에 대한 자세한 내용은 [XCM 메시지 작성](/learn/xcm-communication)을 참조하세요.