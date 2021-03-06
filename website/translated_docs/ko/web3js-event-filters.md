---
id: web3js-event-filters
title: Web3 이벤트 필터
sidebar_label: Web3 이벤트 필터
---
## 개요

[Web3.js](https://github.com/ethereum/web3.js) 라이브러리는 개발자들이 Loom DAppChain에 포함된 [EVM](evm.html)으로 부터 이벤트를 손쉽게 listen하게 해줍니다. indexed 값들에 대한 필터를 만드는 것도 가능합니다.

## 필터링

Loom DAppChain에서 만들어진 가장 최근 블록을 얻어오는 필터를 만들고 콘솔에 블럭 해쉬가 계속 출력해 봅시다.

```js
const {
  Client, CryptoUtils, LoomProvider
} = require('loom-js')
const Web3 = require('web3')

// client를 만든다
const client = new Client(
  'default',
  'ws://127.0.0.1:46658/websocket',
  'ws://127.0.0.1:46658/queryws',
);

// 첫번째 계정에 대한 프라이빗 키를 만든다
const privateKey = CryptoUtils.generatePrivateKey()

// Provider로 LoomProvider를 사용해서 web3 client 인스턴스를 만든다
const web3 = new Web3(new LoomProvider(client, privateKey));

// 가장 최근 블록을 얻기 위한 필터를 만든다
const filter = web3.eth.filter('latest');

// 필터가 가장 최근 블록의 해쉬를 계속 출력하는지 감시하세요 
filter.watch(function (error, result) {
  if (error) {
    console.error(error)
  } else {
    console.log('Block hash', result)
```

## Indexed 값으로 필터링하기

또 하나의 좋은 기능은 `indexed` 값으로 필터링이 하는 것입니다, 이것은 특정`indexed` 값이 보내질때 이벤트 핸들러를 트리거하는데 사용될 수 있습니다.

다음 컨트랙트를 보세요:

```solidity
pragma solidity ^0.4.22;

contract SimpleStore {
  uint value;

  constructor() {
      value = 10;
  }

  event NewValueSet(uint indexed _value);

  function set(uint _value) public {
    value = _value;
    emit NewValueSet(value);
  }
}
```

`NewValueSet` 이벤트에 대해서 송출되는 `value`가 `10`일 때만 트리거 되는 이벤트 핸들러를 설정하는게 가능합니다, 만약 컨트랙트가 어떤 다른 값을 보낼때는 트리거되지 않을 것입니다.

```js
// 퍼블릭 키와 프라이빗 키를 생성한다
const privateKey = CryptoUtils.generatePrivateKey()
const publicKey = CryptoUtils.publicKeyFromPrivateKey(privateKey)

// client를 생성한다
const client = new Client(
  'default',
  'ws://127.0.0.1:46658/websocket',
  'ws://127.0.0.1:46658/queryws',
)

// 함수를 호출하는 주소
const from = LocalAddress.fromPublicKey(publicKey).toString()

// LoomProvider를 사용하는 web3 client를 인스턴스로 만든다
const web3 = new Web3(new LoomProvider(client, privateKey))

// 컨트랙트 ABI
const ABI = [
  {
    constant: false,
    inputs: [{ name: '_value', type: 'uint256' }],
    name: 'set',
    outputs: [],
    payable: false,
    stateMutability: 'nonpayable',
    type: 'function'
  },
  { inputs: [], payable: false, stateMutability: 'nonpayable', type: 'constructor' },
  {
    anonymous: false,
    inputs: [{ indexed: true, name: '_value', type: 'uint256' }],
    name: 'NewValueSet',
    type: 'event'
  }
]

// 배포된 컨트랙트 주소
const contractAddress = '0x...'

// contract를 인스턴스화하고 사용가능한 상태로 만든다
const contract = new web3.eth.Contract(ABI, contractAddress, {from})

// NewValueSet 이벤트를 받기위해 구독한다
contract.events.NewValueSet({ filter: { _value: 10 } }, (err: Error, event: any) => {
  if (err) t.error(err)
  else {
    // value가 10으로 설정될때만 터미널에 출력된다
    console.log('The value set is 10')
  }
})
```