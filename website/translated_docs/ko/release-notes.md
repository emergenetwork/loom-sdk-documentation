---
id: release-notes
title: 릴리즈 노트
sidebar_label: 릴리즈 노트
---
## 개요

릴리즈 노트는 역순으로 구성되어 있습니다.

## 릴리즈 478 - 10월 8일

* Transfer Gateway 활용 예제 업데이트
* Transfer Gateway [플라즈마 테스트넷 튜토리얼](extdev-transfer-gateway.html)

## 릴리즈 458 - 9월 27일

**모든 사용자에게 이 릴리즈로 업데이트하는 것을 추천합니다.**

* 개발자들을 위한 Plasmachain 테스트넷이 사용 가능합니다 - [Plasmachain 테스트넷 문서](testnet-plasma.html)
* 테스트넷을 위한 카르마 Faucet - [Faucet](http://faucet.dappchains.com)
* Loom SDK 카르마 컨트랙트(Sybil 저항) - [카르마](karma.html)
* 작업량이 많은 환경을 위한 대규모 성능 향상
* 검증자 노드 운영에 대한 초기 정보 - [검증자](validator.html)

## 릴리즈 404 - 8월 24일

** 기능

* TransferGateway의 ERC20 지원 업그레이드
* Gateway ERC20 예제 - [예제](https://github.com/loomnetwork/token-gateway-example)
* Loom SDK Doc 사이트 [한글화](https://loomx.io/developers/ko/)
* EVM이 이제 Payable 함수에 대해 제한적으로 지원합니다. 다음 주에 더 업데이트됩니다. 
* TransferGateway를 통한 ERC20/ETH 전송용 Loom-JS 업데이트 
* DPoS 컨트랙트로 Loom-JS 통합 
* [Plasma Debits](https://github.com/loomnetwork/plasma-cash/pull/115)에 대한 실험적 지원

** 주목해야 할 수정 사항

* QueryServerHost를 Deprecate 합니다. RPCServer로 모든 기능을 통합합니다.
* 새로운 설정 옵션: RPCBindAddress: "tcp://0.0.0.0:46658"
* PBFT 엔진의 업그레이드. 이로 인해 업그레이드 시 호환성 문제가 발생할 수 있습니다. 스테이징 환경에서 시도하세요! 

## 릴리즈 375 - 8월 10일

*주의* 이것은 마이너 호환성 변경사항을 포함한 기능 테스트 릴리즈입니다, 여러분의 프로덕션 환경을 업그레이드 하기 앞서 임시 환경에서 확인을 부탁드립니다.

* EVM 컨트랙트의 이벤트와 관련된 수정
* [Transfer Gateway](transfer-gateway.html)의 베타 릴리즈
* [Transfer Gateway의 데모](https://github.com/loomnetwork/cards-gateway-example)
* 메모리 누수 수정
* 마이너 Api 충돌문제, go-loom/loom-js을 업그레이드 하세요. Unity는 곧 업데이트 예정입니다
* Go-loom은 API 충돌문제가 업그레이드 되었습니다
* loom-js는 이번 릴리즈로 업그레이드 되었습니다. 

## 릴리즈 330 - 7월 30일

* EVM 상의 컨센서스 문제 해결 
* Zombiechain 테스트넷을 위한 업데이트
* 성능 측정을 위한 더 많은 원격 측정 기능 추가
* Go 컨트랙트에서 범위 질의문
* loom.yaml에 ChainID가 추가

* [EVM 인덱스 필터](https://loomx.io/developers/docs/ko/web3js-event-filters.html)

* EVM 필터 pool 버그 수정 및 이벤트 시스템
* Loom-JS EVM의 인덱스 필터를 위한 업데이트
* Loom-JS EVM에서 해시로 블록을 가져올 때 버그 수정

## 릴리즈 288 - 7월 17일

* [EVM 인덱스 필터](https://loomx.io/developers/docs/ko/web3js-event-filters.html)
* EVM 필터 pool 버그 수정 및 이벤트 시스템
* Loom-JS EVM의 인덱스 필터를 위한 업데이트
* Loom-JS EVM에서 해시로 블록을 가져올 때 버그 수정

## 릴리즈 276 - 7월 13일

* [새로운 블록 탐색기](block-explorer-tutorial.html)
* 멀티노드 EVM 수정 
* 플라즈마 캐시를 위한 Loom-JS 업데이트
* Zombiechain 테스트넷 수정
* DPoS 업데이트

## 릴리즈 209 - 6월 20일

주요 릴리즈

* 플라즈마 캐시 초기 통합 - 데모는 다음주에 나올 예정입니다
* 멀티노드 수정 및 성능 향상
* 멀티노드를 위한 Ansible 업데이트
* EVM에서의 Querying / Filtering에 더 많은 타입을 지원 
* EVM을 위한 Unity SDK 업데이트 
* EVM을 위한 Loom-JS 업데이트

## 릴리즈186 - 6월 19일

* [EVM Unity 예제 앱](https://loomx.io/developers/docs/ko/unity-sample-tiles-chain-evm.html)
* Solidity 앱을 위한 Unity SDK 지원
* Ethereum 이벤트 Filtering/Querying에 관한 많은 수정사항

## 릴리즈 163 - 6월 11일

* Truffle의 최신 빌드를 위한 지원
* [Truffle 예제 업데이트](https://github.com/loomnetwork/loom-truffle-provider)
* Sybil resistance 프레임웍의 초기 구현
* Websocket 이벤트가 이제 topic을 지원합니다 
* 업데이트된 websocket topic 지원이 포함된 Loom-JS 1.8.0 릴리즈

## 릴리즈 161 - 6월 7일

* 수많은 Truffle 수정사항들
* 수많은 Loom-Js provider와 관련된 web3.js 수정사항들

## 릴리즈 155 - 6월 6일

* [Cocos SDK이 동작합니다](cocos-sdk-quickstart.html)
* [Truffle을 지원합니다](truffle-deploy.html)
* 이제 EVM에 static call이 허용됩니다
* 수많은 시나리오에 관한 EVM 수정 

## 릴리즈 143 - 6월 1일

* [BluePrint 도커 이미지 사용이 가능합니다](docker-blueprint.html)
* [일본 해커톤 결과](https://medium.com/loom-network-korean/첫-번째-loom-unity-sdk-해커톤-하이라이트-도쿄-에디션-70fe873c7fb1)
* [몇개의 Loom SDK 프로젝트를 위한 도커 이미지](https://hub.docker.com/r/loomnetwork/)
* Evm TX Reciepts 수정 

## 릴리즈 137 - 5월 30일

* Go Client가 EVM 컨트랙트에 접근 가능합니다
* EVM을 위한 수많은 버그 수정
* [소셜 네트워크 예제 앱 - Solidity](simple-social-network-example.html)

## 릴리즈 136 - 5월 28일

* 초기 Solidity 알파 테스트 빌드, 이제 solidity 컨트랙트를 배포할 수 있습니다
* Solidity를 위한 Websocket 이벤트 지원 
* [Solidity 이벤트에 관한 예제 프로젝트](phaser-sdk-demo-web3-websocket.html)
* [멀티노드 배포 가이드](multi-node-deployment.html)

## 릴리즈 133 - 5월 24일

* [Etherboy 데모 릴리즈](https://loomx.io/developers/docs/en/etherboy-game.html)
* [일본어 문서 릴리즈](https://loomx.io/developers/ja)
* [문서 사이트](https://loomx.io/developers/ko/)를 위한 홈페이지 업데이트 

## 릴리즈 132 - 5월 23일

* Websocket 성능 개선
* 새로운 Websocket 데모 앱 - TilesChain - [Github](https://github.com/loomnetwork/tiles-chain) 

## 릴리즈 129 - 5월 22일

* 이벤트를 위한 Websocket 지원
* Solidity 컨트랙트를 위한 indexing 레이어 업데이트
* Phaser 게임 데모 - [Github](https://github.com/loomnetwork/phaser-sdk-demo)

## 릴리즈 128 - 5월 21일

* Etherboy와 관련된 수많은 버그 수정

## 릴리즈 128 - 5월 19일

* Stable 베타 릴리즈
* 로깅이 기본적으로 여러 파일에 되도록 업데이트 
* 모든 RPC를 단일 인터페이스로 이동
* 외부 프로세스 인터페이스 업데이트