# Solidity with vscode

vscode에서 remix로 solidity 코드를 컴파일하기

준비물: vscode, etheriem-remix vscode extension

1. vscode 설치
2. etheriem-remix 확장팩 설치; https://marketplace.visualstudio.com/items?itemName=RemixProject.ethereum-remix

remix 확장팩은 solidity를 remix의 remote compiler를 사용해 컴파일한다. 컴파일시에 최신 버전의 compiler를 사용한다면 `F5`나 `실행> solidity: Compile Contract`로 컴파일 할 수 있으나, 낮은 버전의 solidity를 컴파일한다면, `solidity.compileUsingRemoteVersion`을 버전에 아래와 같이 변경하거나, `실행> solidity: Change workspace compiler version`로 변경해야 한다.

```json
{
    "solidity.compileUsingRemoteVersion": "v0.4.26+commit.4563c3fc"
}
```


# Running a Ethereum private network with geth with remix web IDE

geth를 통해 private network을 하나 생성하고, 최신 버전의 geth와 remix IDE 연동 방법을 확인/정리해 보았다.

- OS: Ubuntu 22.04
- Geth: 1.10.16-stable
- Remix: remixd 0.5.6

## Starting geth

```bash
# Geth Client account 생성
# data directory 지정
geth --datadir data account new
geth --datadir data account new
geth --datadir data account new

# genesis.json genesis block 설정파일 작성
# genesis.json에 alloc 추가하여 account의 eth 설정
# e.g. genesis.json
# {
#     "config": {
#           "chainId": 999,
#           "homesteadBlock": 0,
#           "eip150Block": 0,
#           "eip155Block": 0,
#           "eip158Block": 0
#     },
#     "nonce": "0x0000000000000042",
#     "timestamp": "0x00",
#     "extraData": "0x00",
#     "gasLimit": "0x80000000",
#     "difficulty": "0x400",
#     "mixHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
#     "coinbase": "0x3333333333333333333333333333333333333333",
#     "parentHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
#     "alloc": {
#         "0x6c2cc8050F50368Ce497E81415DB1B06662AefAC": {
#             "balance": "1000000000000000000000"
#         },
#         "0x8ab2F2bBC57CB7B70EfF905f028FFF99485E2367": {
#             "balance": "1000000000000000000000"
#         },
#         "0x6b73C78f7FDAf4d93DdA672C2364F49Bf6cdeE9D": {
#             "balance": "1000000000000000000000"
#         }
#     }
# }

# genesis block 생성
geth --datadir data init genesis.json

# ethereum bootnode 실행 with private ethereum network
# remix 연동 옵션 추가: --http --http.corsdomain="https://remix.ethereum.org"
# available api ="[admin debug web3 eth txpool personal ethash miner net]"
geth --networkid 999 --http --http.corsdomain="https://remix.ethereum.org" --http.api web3,eth,debug,personal,net --vmdebug --nodiscover --datadir data console

# Simple remix 연동
geth --http --http.corsdomain https://remix.ethereum.org
geth --http --http.corsdomain="https://remix.ethereum.org" --http.api web3,eth,debug,personal,net --vmdebug --datadir <path/to/local/folder/for/test/chain> --dev console
```

## Starting remix

### Local filesystem 접속

remix에서 나의 파일시스템에 있는 solidity 파일에 접근하려면 생각보다 번거로운 과정을 요구한다.
아래 사이트의 설명대로 remixd를 설치하고, 아래와 같이 IDE 주소를 지정하여 실행한다.

```bash
remixd -s ./remix_data/ -u https://remix.ethereum.org/
```

https://remix-ide.readthedocs.io/en/latest/remixd.html#ports-usage

### Local private network에 접속

geth를 통해 만들어진 private network에 연결하여 contract를 실행하려면, remix가 node가 열어놓은 web interface에 연동되어야 하는 듯 하다.
solidty의 compile은 online으로 동작가능하고, deploy시 private network으로 연결하여 contract 생성하는 것으로 보인다.

- **solidity file compilation**: `solidity compiler` tab > `compile` button
- **contract deploy and run transaction**: `deploy & run transactions` tab > `ENVIRONMENT` > select `Web3 Provider`

앞서 geth 실행에서 `--http --http.corsdomain="https://remix.ethereum.org"` 옵션과 함께 실행했다면, 접속이 된다.

## solidity

```
```

### Pragma solidity ^0.0.0;

solidity 버전을 기술하여 compile에 참조한다.

### Contract

C++, Java의 class와 유사하게 내부에 변수와 함수를 갖는다.

### Constructor

Contract가 생성될 때 한번 호출되는 초기화 함수로 contract와 같은 이름으로 함수를 선언한다.

### Variable Types


### Visibility

- external: contract 외부에 공개, 외부 접근 사용 인터페이스
- public: contract 외부에 공개, getter() 자동생성, internal, external에서 접근 ok
- internal: contract 내부에서만 사용, 내부 함수 (protected와 비슷)
- private: contract 내부에서만 사용, 상속 X

### Addtional options

- constant: 상태 변경 없을 경우, gas 소모 x
- view: readonly, 소모 gas 없음
- payable: 다른 EOA, CA에서 ETH 송금받길 원할 때

#### view function
view function은 contract의 state를 변경하지 않을 경우 선언할 수 있다.

```solidity
pragma solidity ^0.5.0;

contract Test {
   function getResult() public view returns(uint product, uint sum){
      uint a = 1; // local variable
      uint b = 2;
      product = a * b;
      sum = a + b; 
   }
}
```

contract의 state는 일반적으로 다음과 같은 상황에서 변경된다.

- Modifying state variables.
- Emitting events.
- Creating other contracts.
- Using selfdestruct.
- Sending Ether via calls.
- Calling any function which is not marked view or pure.
- Using low-level calls.
- Using inline assembly containing certain opcodes.

