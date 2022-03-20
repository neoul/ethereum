# ERC

- Ethereum Request for Comments
- Listed to https://eips.ethereum.org/erc


## EIR

Ethereum Improvement Proposals (EIPs) describe standards for the Ethereum platform, including core protocol specifications, client APIs, and contract standards.

- https://github.com/ethereum/EIPs
- **EIP Status**: [`Idea`, `Draft`, `Review`, `Last Call`, `Final`, `Stagnant`, `Withdrawn`, `Living`]
- **EIP Types**: [`Core`, `Networking`, `Interface`, `ERC`, `Meta`, `Informational`]


### ERC is one of EIR Types

Application-level standards and conventions, including contract standards such as token standards (ERC-20), name registries (ERC-137), URI schemes (ERC-681), library/package formats (EIP190), and wallet formats (EIP-85).


## ERC-20

ERC-20 is a standard or guideline for creating new tokens. The standard defines six mandatory functions that a smart contract should implement and three optional ones.

To start you can give your token a name, a symbol, and mention how dividable your token is, by specifying the decimals. ERC specifies a set of mandatory functions, which are a bit more complex and listed below:

- **totalSupply**: A method that defines the total supply of your tokens, When this limit is reached the smart contract will refuse to create new tokens.
- **balanceOf**: A method that returns the number of tokens a wallet address has.
- **transfer**: A method that takes a certain amount of tokens from the total supply and gives it to a user.
- **transferFrom**: Another type of transfer method which is used to transfer tokens between users.
- **approve**: This method verifies whether a smart contract is allowed to allocate a certain amount of tokens to a user, considering the total supply.
- **allowance**: This method is exactly the same as the approved method except that it checks if one user has enough balance to send a certain amount of tokens to another.

If you know something about Object Oriented programming you can compare ERC-20 to an Interface. If you want your token to be an ERC-20 token, you have to implement the ERC-20 interface and that forces you to implement these 6 methods. 

- See more details: https://www.quicknode.com/guides/solidity/how-to-create-and-deploy-an-erc20-token


## ERC-165

ERC-165는 사용하려는 contract가 어떠한 인터페이스(A set of functions) 구현했는지 확인할 수 있는 방법을 제시한다.

- **Function ID**: the string of function_name(argument type) > keccak256 hashing > cut first 4 bytes of the hashing value
- **Function Selector**: = Function ID

제공해야 할 인터페이스의 Function ID들을 XOR하여 아래 인터페이스를 통해 사용하려는 인터페이스의 지원 유무를 제공함.

```solidity
function supportsInterface(bytes4 interfaceID) external view returns (bool);
```

- See more details: https://medium.com/humanscape-tech/erc-165%EB%9E%80-%EB%AD%98%EA%B9%8C-910b29533188


## ERC-1820: Pseudo-introspection Registry Contract

- ERC-820을 대체함
- ERC-165의 역할도 함
- Smart contract나 regular account가 제공하고 있는 인터페이스를 등록하는 등기소 (Registry)
- 고정된 등기소 주소(Registry Contract Address) 제공: `0x1820a4B7618BdE71Dce8cdc73aAB6C95905faD24`
- raw transaction data를 제공하므로 어느 chain이건 위의 Registry Contract Address에 Registry Contract가 배포되어야 함

This standard defines a registry where smart contracts and regular accounts can publish which functionality they implement—either directly or through a proxy contract.
Anyone can query this registry to ask if a specific address implements a given interface and which smart contract handles its implementation.
Interfaces with zeroes (0) as the last 28 bytes are considered ERC-165 interfaces, and this registry SHALL forward the call to the contract to see if it implements the interface. This contract also acts as an ERC-165 cache to reduce gas consumption.

This standard also provides a unique address for all chains. Thus solving the problem of resolving the correct registry address for different chains.


### ERC-777과의 동작

ERC-777 token contract는 ERC-1820 Registry의 `setInterfaceImplementer`을 호출하여 자신이 ERC-777 token 인터페이스 임을 등록해야 한다.

This is done by calling the `setInterfaceImplementer` function on the ERC-1820 registry with the token contract address as both the address and the implementer and the keccak256 hash of ERC777Token (0xac7fbab5f54a3ca8194167523c6753bfeb96a445279294b6125b68cce2177054) as the interface hash.


## ERC-777 Token Contract

ERC-20에 대한 호환성을 유지하면서 contract, account간의 token 교환 및 token의 send/receive hooks를 통한 세부제어 방법을 정의한다.

This standard defines a new way to interact with a token contract while remaining backward compatible with ERC-20. It defines advanced features to interact with tokens.
It defines advanced features to interact with tokens. Namely, operators to send tokens on behalf of another address—contract or regular account—and send/receive hooks to offer token holders more control over their tokens.

This standard tries to improve upon the widely used ERC-20 token standard. The main advantages of this standard are:

1. Ether를 보낼 때 와 동일한 개념을 사용
2. `tokensToSend`와 `tokensReceived` hook를 사용하여 token 송수신을 거부할 수 있다. (Rejection is done by reverting in the hook function.)
3. `tokensReceived` hook으로 token 송신을 승인할 수 있으며, 그 결과를 공지할 수 있다. The tokensReceived hook allows to send tokens to a contract and notify it in a single transaction, unlike ERC-20 which requires a double call (approve/transferFrom) to achieve this.
4. token 소유자가 token 송수신에 대한 제 3자의 제어 (operator, exchange agent, automatic charging system) 를 허용한다. The holder can “authorize” and “revoke” operators which can send tokens on their behalf. These operators are intended to be verified contracts such as an exchange, a cheque processor or an automatic charging system.
5. 모든 token 교환에 사용된 정보(operator 정보 포함)를 기록한다. Every token transaction contains data and operatorData bytes fields to be used freely to pass data from the holder and the operator, respectively.
6. proxy contract를 배포해 `tokenReceived` hook이 없는 wallet과의 호환성을 지원한다. It is backward compatible with wallets that do not contain the tokensReceived hook function by deploying a proxy contract implementing the tokensReceived hook for the wallet.

### operator

An operator is an address which is allowed to send and burn tokens on behalf of some holder.



## ERC-20, ERC-1155, ERC-223, ERC-721 – 차이점은 무엇인가요?

- See more details: https://academy.binance.com/ko/articles/an-introduction-to-erc-20-tokens

ERC-20은 첫 이더리움 토큰 표준이었습니다(그리고 오늘날까지도 가장 인기 있는 것입니다). 그러나 ERC-20만이 유일한 것은 아닙니다. 수 년에 걸쳐 다른 많은 것들이 등장했으며, 이는 ERC-20 개선을 제안하는 것이거나, 전적으로 다른 목적을 달성하기 위한 것입니다.

상대적으로 일반적이지 않은 표준 중 일부는 NFT(대체 불가능한 토큰)에 사용됩니다. 때로는 다른 속성을 가진 고유한 토큰을 사용하는 데서 이점을 얻을 수 있습니다. 여러분이 예술 작품의 한 부분이나, 게임 내 자산 등을 토큰화하고자 할 경우, 이러한 콘트랙트 유형 중 하나가 더 적합할 수 있습니다.
예를 들어, ERC-721 표준은 무척이나 인기 있는 크립토키티 디앱에서 사용됐습니다. 이러한 콘트랙트는 사용자에게 API를 제공하여, 자신만의 대체 불가능한 토큰을 주조하고 메타 데이터(이미지, 설명 등)를 표현할 수 있게 했습니다. 
ERC-1155 표준은 ERC-721과 ERC-20 모두를 개선한 것으로 볼 수 있습니다. 간략히 말해 이는 대체 가능한 토큰과 대체 불가능한 토큰을 동일한 콘트랙트 내에서 모두 지원하는 표준입니다.

ERC-223 또는 ERC-621처럼 사용성을 개선하고자 하는 다른 선택지도 있습니다. ERC-223은 토큰 전송 사고를 방지하기 위한 안전 장치를 사용합니다. ERC-621은 토큰 공급량 증가와 감소를 위한 부가적인 기능을 제공합니다.

NFT와 관련한 보다 자세한 설명은, 암호화폐 수집물과 대체 불가능한 토큰(NFT) 설명을 확인해 보시기 바랍니다.
