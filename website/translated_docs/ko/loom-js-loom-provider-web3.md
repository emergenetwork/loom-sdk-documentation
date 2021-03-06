---
id: loom-js-loom-provider-web3
title: Loom.js + Web3.js
sidebar_label: Loom.js + Web3.js
---
# 개요

`loom-js`는`LoomProvider`와 함께 제공되므로 Ethereum 개발자가 트랜잭션을 배포하고 보낼 수 있는 provider로써 `Web3.js`을 연결하고 Loom DAppChain 내에서 구동되는 스마트 컨트랙트 이벤트를 listen할 수 있도록 해줍니다, 더 자세한 사항은 [EVM 페이지](evm)를 확인하세요

NPM을 통해서 `loom-js` 설치를 시작하기:

```shell
yarn add loom-js
# 혹은 여러분이 선호하는 것으로...
npm install loom-js
```

# 컨트랙트를 인스턴스화하기

## SimpleContract

우리가 이미 Loom DAppChain에 컴파일되어 배포 된 Solidity 컨트랙트를 가지고 있다고 해봅시다

    pragma solidity ^0.4.22;
    
    contract SimpleStore {
      uint value;
    
      event NewValueSet(uint);
    
      function set(uint _value) public {
        value = _value;
        emit NewValueSet(value);
      }
    
      function get() public view returns (uint) {
        return value;
      }
    }
    

Solidity 컴파일러로 컴파일된 바이너리로 할 다음 단계는 Loom DappChain을 위한 `genesis.json`을 생성하는 것입니다. (컴파일된 바이너리의 `location`을 설정하는 것을 잊지마세요)

```Javascript
{
    "contracts": [
        {
            "vm": "EVM",
            "format": "hex",
            "name": "SimpleStore",
            "location": "/path_to_simple_store/SimpleStore.bin"
        }
    ]
}

```

컴파일된후 컨트랙트는 다음과 같은 ABI 인터페이스를 생성할 것입니다:

```js
const ABI = [{
  "constant": false,
  "inputs": [{
    "name": "_value",
    "type": "uint256"
  }],
  "name": "set",
  "outputs": [],
  "payable": false,
  "stateMutability": "nonpayable",
  "type": "function"
}, {
  "constant": true,
  "inputs": [],
  "name": "get",
  "outputs": [{
    "name": "",
    "type": "uint256"
  }],
  "payable": false,
  "stateMutability": "view",
  "type": "function"
}, {
  "anonymous": false,
  "inputs": [{
    "indexed": false,
    "name": "",
    "type": "uint256"
  }],
  "name": "NewValueSet",
  "type": "event"
}]
```

`LoomProvider`를 사용하여 `Web3`을 인스턴스화하고 사용하는 방법은 Ethereum 노드에서 사용하는 것과 유사하지만 먼저 `loom-js` client를 올바르게 초기화해야 합니다.

```js
import {
  Client, Address, LocalAddress, CryptoUtils, LoomProvider, EvmContract
} from '../loom.umd'

import Web3 from 'web3'

// 이 함수는 초기화를 하고 client를 반환합니다
function getClient(privateKey, publicKey) {
  const client = new Client(
    'default',
    'ws://127.0.0.1:46658/websocket',
    'ws://127.0.0.1:46658/queryws',
  )

  return client
}

// 키를 세팅합니다
const privateKey = CryptoUtils.generatePrivateKey()
const publicKey = CryptoUtils.publicKeyFromPrivateKey(privateKey)

// Client 준비
const client = getClient(privateKey, publicKey)
```

이제 준비된 client로 `Web3`를 인스턴스화 해보자, `Web3` 인스턴스를 정확히 초기화하기 위해서 `client`를 가지고 `LoomProvider`를 넘겨줄 것입니다

```js
const web3 = new Web3(new LoomProvider(client, privateKey))
```

컨트랙트를 인스턴스화할 준비가 되었습니다

```js
// 퍼블릭키 기반의 우리의 주소를 가져오기
const fromAddress = LocalAddress.fromPublicKey(publicKey).toString()

// 컨트랙트 주소를 가져옵니다 (우리는 genesis.json에 지정된 이름만 알 뿐 주소를 알지 못합니다)
const loomContractAddress = await client.getContractAddressAsync('SimpleStore')

// Web3와 호환되는 헥사값으로 loom 주소를 변환합니다
const contractAddress = CryptoUtils.bytesToHexAddr(loomContractAddress.local.bytes)

// 컨트랙트를 인스턴스화합니다
const contract = new web3.eth.Contract(ABI, contractAddress, {from: fromAddress})
```

컨트랙트가 인스턴스화되었고 준비가 되었습니다

# 트랜잭션과 Call

`Web3 컨트랙트`가 인스턴스화 된 후에 다음과 같은 트랜잭션 (`send`) 과 call (`call`) 을 위한 컨트랙트 메소드를 사용할 수 있습니다:

```js
(async function () {
  // 값에 47을 넣는다.
  await contract.methods.set(47).send()

  // 값을 가져온다
  const result = await contract.methods.get().call()
  // result should be 47
})()
```

# 이벤트

컨트랙트에 이벤트 listener를 추가하는 것도 가능합니다, 비록 필터 지원을 아직 하지 않지만

```js
(async function () {
  // 새로운 값의 설정을 listen한다
  contract.events.NewValueSet({}, (err, newValueSet) {
    if (err) {
      console.error('error', err)
      return
    }

    console.log('New value set', newValueSet.returnValues)
  })
})()
```

## 한꺼번에 해보기

자 이제 모든 것을 가지고 DAppChain이 구동되었는지 확인하십시오 그리고 다음 코드를 실행시키세요, 그럼 여러분은 콘솔에 찍히는 `Value: hello!` 를 보실수 있을 것입니다.

```js
import {
  Client, Address, LocalAddress, CryptoUtils, LoomProvider
} from '../loom.umd'

import Web3 from 'web3'

// 이 함수는 초기화를 하고 client를 반환합니다
function getClient(privateKey, publicKey) {
  const client = new Client(
    'default',
    'ws://127.0.0.1:46658/websocket',
    'ws://127.0.0.1:46658/queryws',
  )

  return client
}

// 키를 세팅합니다
const privateKey = CryptoUtils.generatePrivateKey()
const publicKey = CryptoUtils.publicKeyFromPrivateKey(privateKey)

// Client 준비
const client = getClient(privateKey, publicKey)

// web3 세팅하기
const web3 = new Web3(new LoomProvider(client, privateKey))

;(async () => {
  // Set the contract ABI
  const ABI = [{"constant":false,"inputs":[{"name":"_value","type":"uint256"}],"name":"set","outputs":[],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[],"name":"get","outputs":[{"name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"}]

  // 퍼블릭 키 기반의 우리의 주소를 가져오기
  const fromAddress = LocalAddress.fromPublicKey(publicKey).toString()

  // 컨트랙트 주소를 가져옵니다 (우리는 genesis.json에 지정된 이름만 알 뿐 주소를 알지 못합니다)
  const loomContractAddress = await client.getContractAddressAsync('SimpleStore')

  // Web3와 호환되는 헥사값으로 loom 주소를 변환합니다
  const contractAddress = CryptoUtils.bytesToHexAddr(loomContractAddress.local.bytes)

  // 컨트랙트를 인스턴스화합니다
  const contract = new web3.eth.Contract(ABI, contractAddress, {from: fromAddress})

  // 새로운 값의 설정을 listen한다
  contract.events.NewValueSet({}, (err, newValueSet) {
    if (err) {
      console.error('error', err)
      return
    }

    console.log('New value set', newValueSet.returnValues)
  })

  // 값에 47을 넣는다
  await contract.methods.set(47).send()

  // 값을 가져온다
  const result = await contract.methods.get().call()
  // result should be 47
})()

```