# Solidity with vscode

기본 설정

- vscode
- vscode extension: etheriem remix (ext install RemixProject.ethereum-remix)
- vscode extension: solidity (ext install JuanBlanco.solidity)
- vscode extension: solidity virtual developer

- `vscode`> `ethereum remix`> `Run & Deploy`: use the remix IDE in vscode.
- `vscode`> `Change workspace compiler version (Remote)`: select solidity version to clear compiler mismatch error.
- install `npm install @openzeppelin/contracts` for zeppelin solidity reference.
- `vscode`>`Open User Setting`> add `"solidity.packageDefaultDependenciesDirectory": "node_modules"` as follows.

```json
{
  "solidity.compileUsingRemoteVersion": "v0.8.0+commit.c7dfd78e",
  "solidity.defaultCompiler": "remote",
  "solidity.packageDefaultDependenciesContractsDirectory": "",
  "solidity.packageDefaultDependenciesDirectory": "node_modules"
}
```

## Solidity Output (Bytecode, ABI)

### Bytecode

- EVM executable bytecode
- The bytecode is stored in the blockchain when a contract is deployed.

### ABI (Application Binary Interface)


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

## Starting remix with local filesystem

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


## Terms

- Transaction(Tx):
- IPFS (InterPlanetary File System): file server based on blockchain?
- `EVM (Ethereum Virtual Machine)`: Smart Contract를 blockchain내에서 실행하기 위한 런타임 환경
- `Account`: blockchain의 Service Access Point
- `EOA (Externally Owned Accounts)`: 외부에서 PKI (공개키 기반 암호 알고리즘)로 만들어진 계정
  - `Public key`: Account Address 생성 및 Transaction 검증에 사용
  - `Private key`: Transaction 서명에 사용하며, miner에 의해 Ethereum network에서 실행전 검증
  - `Account Component`: 지갑 (Wallet) 에는 키 값만 소유하고, Ether나 Token에 대한 정보는 모두 blockchain에 기록.
    - `nonce`: 해당 account의 transaction 수
    - `balance`: Ether 잔고
    - `root`: 해당 account가 저장될 머클 매트리시아 트리의 루트노드?
    - `codehash`: Smart Contract byte code hash value?
  - `Account Wallet`: 키만 저장되며, 나머지 정보는 blockchain 내 존재
- `CA (Contract Accounts)`: 컨트랙트 접근을 위해 컨트랙트 생성시 만들어진 계정, 주소 (생성한 사용자의 주소와 주소로부터 보내진 Transaction의 수, "Nonce"에 기반)
- `Account Address`: EOA의 공개키를 keccak-256로 해싱후 뒤 20 bytes (160 bits)를 16진수로 표현한 byte string
- `Byte Code`: EVM code
- `Parity`: Rust로 구현한 Ethereum client
- `Geth`: Ethereum Foundation의 공식 Ethereum client
- `Web3`와 `Truffle`: Web API for Ethereum
- `Ganache`, `ganache-cli`: 테스트용 local Ethereum network
- `Metamask`: Ethereum Wallet application

### User Transaction (Transaction)

- EOA's private key로 서명된 message; 작업단위
- ECDSA (Ellictic Curve Digital Signature Algorithm)로 서명
- 구성요소
  - nonce: 발신자의 transaction 갯수
  - price: transaction 수행 수수료 (fee); unit: wei
  - gaslimit: 사용할 수 있는 gas 최대값
  - recipient: 수신자 주소
  - amount: 전송할 ether 량; unit: wei
  - payload: optional field; smart contract 호출일 경우 필요한 매개변수
  - v,r,s: ?


```bash
#!/bin/bash

docker run -v $PWD/solidity:/solidity ethereum/solc:0.4.24 -o /solidity/output --abi --bin /solidity/$1
# docker run --rm -v $(pwd):/root ethereum/solc:0.4.24 --abi /root/Store.sol -o /root/build
abigen --bin=$PWD/solidity/output/Store.bin --abi=$PWD/solidity/output/Store.abi --pkg=store --out=store.go
```