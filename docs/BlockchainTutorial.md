# Blockchain Tutorial

2018-07-24

Heungjin Kim

Scoutchain

---

## Table of Contents

* Bitcoin
* Ethereum
* Scoutchain

---

## BitCoin

[Bitcoin White Paper(9 pages)](https://bitcoin.org/bitcoin.pdf)

### Introduction

* 2008년 금융위기와 기존 금융권에 대한 불신에서 출발.
* 2009년 비트코인 등장.

--

* 룰을 강제할 권력을 가진 중앙기관이 있어야만 거래가 가능한가?
  *  걔네들이 마음대로 내 돈을 가져가거나 못쓰게 막거나 하는 걸 막을 수 있는가?<!-- .element: class="fragment" -->
* 현재의 암호학적 도구들을 조합하면 가능할 수 있다!<!-- .element: class="fragment" -->
  * Cryptographic hash<!-- .element: class="fragment" -->
  * Digital signature<!-- .element: class="fragment" -->

---

### Transactions

* 모든 거래에 디지털 서명을 하면 서명 자체의 유효성은 누구든 체크할 수 있음
* 이중지불을 했는지는 어떻게 체크?<!-- .element: class="fragment" -->
  * 중앙 조폐국을 두고 모든 코인 발행을 여기서 하고 지불할땐 다시 조폐국으로 들어오고 매 거래마다 조폐국이 이중지불 체크<!-- .element: class="fragment" -->
  * 중앙 신뢰 기관이 없으면??<!-- .element: class="fragment" -->

---

### Proof-of-Work

* 이전 블록 해시값과 트랜잭션을 다 모으고 거기에 Nonce 값을 붙여서 다음 해시값을 결정<!-- .element: class="fragment" -->
* 해시값이 정해진 개수만큼의 0으로 시작하도록 조정: 해당 Nonce값을 찾으려면 반복 계산을 해야됨<!-- .element: class="fragment" -->
* 가장 긴 블록을 네트워크상의 모든 노드들이 따라가는 구조<!-- .element: class="fragment" -->
* 역사 조작을 위해서는 지금까지 찾아진 Proof-of-Work들 전체를 새로 찾아나가서 네트워크 나머지보다 더 빠르게 계산해내야 함<!-- .element: class="fragment" -->

--

### Proof-of-Work Chain

![Proof-of-Work Chain](https://i.stack.imgur.com/ukuq0.png)

--

### Merkle Trees

![Merkle Trees](https://raw.githubusercontent.com/ethereum/www/master-postsale/src/extras/gh_wiki/spv_bitcoin.png)

--

### Unspent Transaction Output (UTXO)

![UTXO](https://quarkblockchain.com/white-paper/images/image7.png)

---

### Network

1. 새 트랜잭션이 모든 노드들에게 방송<!-- .element: class="fragment" -->
2. 각 노드는 새 트랜잭션들을 블록 하나에 모음<!-- .element: class="fragment" -->
3. 각 노드는 그 블록에 적절한 nonce값(Proof-of-Work)을 열심히 계산<!-- .element: class="fragment" -->
4. Proof-of-Work를 찾으면 모든 노드에게 방송<!-- .element: class="fragment" -->
5. 노드들은 모든 트랜잭션들이 유효하고 이미 사용된 게 아닐 때에만 블록을 받아들임<!-- .element: class="fragment" -->
6. 노드들은 그 블록을 받아들였다는 것을 그 뒤에 다음 블록을 만드는 것으로 표현함<!-- .element: class="fragment" -->

---

### Incentive

* 관습적으로 블록의 첫 트랜잭션은 블록 생성자가 소유한 새 코인을 시작하는 특별한 트랜잭션<!-- .element: class="fragment" -->
  * 이것으로 네트워크를 뒷받침하는 노드들에 인센티브 제공
  * 코인 발행을 전담할 중앙 기관 없는 통화 공급의 수단이기도 함
* 거래에서 나가는 값이 들어오는 값보다 작을 때 그 차이를 트랜젹션 요금으로도 받음<!-- .element: class="fragment" -->
* 잠재적 공격자가 네트워크를 공격하기보다는 같은 파워로 자신의 이익을 추구하는 쪽이 더 이익이 되도록 하기도 함<!-- .element: class="fragment" -->

---

## Ethereum

[이더리움 백서](https://github.com/ethereum/wiki/wiki/White-Paper)

### Introduction

비트코인은 분산 합의의 툴로써 가능성을 보여주었지만, 장부 이외의 다른 응용을 개발하기 불편하다

--

#### 블록체인 응용 시스템 개발

* 독자적인 블록체인 개발 : 어렵고 충분한 유저 확보가 안되면 사실상 불가능<!-- .element: class="fragment" -->
* 비트코인 스크립트 활용: 여러가지 제한이 있음<!-- .element: class="fragment" -->
  * 루프가 없어서 Turing Complete하지 않음
  * 전달되는 값의 크기를 스크립트상에서 컨트롤하기 어려움
  * State 부족: UTXO는 쓰이거나 안쓰이거나 뿐
  * 블록체인 정보에 접근불가: Nonce, timestamp, previous block hash, etc.
* 블록체인 위에서 메타 프로토콜: light client 불가능<!-- .element: class="fragment" -->
* 이더리움: 손쉬운 블록체인 응용 시스템 개발을 위한 플래폼<!-- .element: class="fragment" -->

---

### Ethereum Account

* Nonce
* Ether balance
* Contract Code, if present
* Storage (empty by default)

--

#### Two type of accounts:

* Externally owned account
  * has no code
  * one can send messages from it by creating and signing a transaction
* Contract accounts
  * everytime it receives a message its code activates, allowing it to read and write to internal storage and send other messages or create contracts in turn

참고: Contracts는 Ethereum에서 사는 자동 에이전트 같은 존재들. 자체 이더 밸런스와 키밸류스토어를 가진다.

---

### Transaction

트랜잭션이 갖고있는 정보:

* 메세지 수신자
* 송신자 사인
* 보낼 Ether 총량
* (옵션)데이터 필드
* 시작 gas
* gas 가격

---

### Contract Message

* (암시적) 메세지 송신자
* 메세지 수신자
* 보낼 Ether 총량
* (옵션)데이터 필드
* 시작 gas

컨트렉트가 CALL opcode를 실행하면 메세지가 생성됨.

---

### 데이터구조에 있어서 비트코인과의 차별점

* 이더리움은 비트코인과는 다르게 현재 상태도 갖고 있음

--

![Ethereum State](https://i.stack.imgur.com/YZGxe.png)

--

![Next State](https://d2g355lhiymhv6.cloudfront.net/wp-content/uploads/2017/11/20104912/Screen-Shot-2017-11-20-at-10.48.47-AM.png)

---

## Scoutchain Mainnet

### Limits of current blockchains

* Low throughput: ~10 BTC transactions/sec, ~20 ETH transactions/sec<!-- .element: class="fragment" -->
* Meaningless PoW energy consumption<!-- .element: class="fragment" -->
* High Transaction fee / Gas fee<!-- .element: class="fragment" -->
* High Volatility of cryptocurrency<!-- .element: class="fragment" -->

---

### Scoutchain mainnet strategy

* Proof of Stake consensus with tendermint style BFT algorithm<!-- .element: class="fragment" -->
* Introduce ScoutChain Dollar: Stable-value coin<!-- .element: class="fragment" -->

