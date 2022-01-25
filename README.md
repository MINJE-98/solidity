
# 시작하기
크립토 좀비를 해보면서 solidity에 대해 정리를 해보았습니다.
## 스마트 컨트랙트

스마트 컨트랙트는 솔리디티라는 언어로 작성되어 있다. 솔리디티를 유심히 보면 자바스크립트와 많이 비슷해 보일 것이다.

스마트 컨트랙트를 배포하게 된다면, 블록체인상에 영원히 남게 된다는 **불변성**을 지니고 있다.

만약 배포하게된 컨트랙트 코드에 결점이 발견된다면 고칠수가 없으므로 다른 스마트 컨트랙트 주소를 써야한다.

## 외부 의존성

스마트 컨트랙트는 한번 배포가 된다면 수정을 할 수 없기 때문에 중요한 일부를 수정할 수 있도록 하는 함수를 만들어놓는 것이 합리적이다.

## 솔리디티 버전

솔리디티는 `version pragma`로 시작해야 한다. (없으면 안됨)

`version pragma`솔리디티 버전을 선언하는 것이다. 버전을 선언하여 다른 컴파일 버전이 나왔을때 코드가 깨지지 않도록 예방할 수 있다.

```jsx
pragma solidity ^0.4.19; 
```

## 컨트랙트 생성

```jsx
contract HelloWorld{
	//someting code
}
```

## 상속

```jsx
contract Doge {
  function catchphrase() public returns (string) {
    return "So Wow CryptoDoge";
  }
}

contract BabyDoge is Doge {
  function anotherCatchphrase() public returns (string) {
    return "Such Moon BabyDoge";
  }
}
```

## 생성자

생성자는 다른언어에서 사용하는 것과 같다.

해당 컨트랙트가 실행되면 딱 한번 실행되는 함수이다.

```jsx
contract Ownable {
  function Ownable() public {
    owner = msg.sender;
  }
}
```

# 자료형

## 상태 변수 & 정수

상태변수는 컨트랙스 저장소에 영구적으로 저장되는 변수이다. 데이터 베이스에 데이터를 쓰는 것 과 동일하다고 생각하면 된다.

```jsx
contract Example {
	unit myUnsignedInteger = 100;
}
```

## 맵핑&Adress

매핑은 기본적으로 키-값저장소로, 데이터를 저장하고 검색하는 데 이용된다.

```jsx
// 금융 앱용으로, 유저의 계좌 잔액을 보유하는 uint를 저장한다: 
mapping (address => uint) public accountBalance;
// 아이디로 주소를 저장 가능.
mapping (uint => address) public accountBalance;
// 혹은 userID로 유저 이름을 저장/검색하는 데 매핑을 쓸 수도 있다 
mapping (uint => string) public userIdToName;
```

## 부호 없는 정수

`uint`자료형은 부호 없는 정수로 값이 음수가 아니어야 한다는 의미이다. MySQL에 `unsignedInt`와 같다고 보면된다.

정수 타입을 지정할때는 얼마나 사용할지 계산 후 해야한다. 256비트 잡아서 변수를 선언하게 된다면, 256비트의 저장 공간을 미리 잡아 두기 때문에 Gas가 낭비될 수 있다!

> 솔리디티에서 `uint`는 실제로 `uint256`, 즉 256비트 부호 없는 정수의 다른 표현이며, `uint8`, `uint16`, `uint32` 등과 같이 `uint`를 더 적은 비트로 선언할 수 있다.

## 현 변환

아래의 예시를 통해 알아보자.

```jsx
uint8 a = 5;
uint b = 6;
// a * b가 uint8이 아닌 uint를 반환하기 때문에 에러 메시지가 난다:
uint8 c = a * b; 
// b를 uint8으로 형 변환해서 코드가 제대로 작동하도록 해야 한다:
uint8 c = a * uint8(b);
```

# 연산자

아래의 예를 보면 솔리디티의 연산은 다른 프로그래밍과 같다. 

- 덧셈: `x + y`
- 뺄셈: `x - y`
- 곱셈: `x * y`
- 나눗셈: `x / y`
- 모듈로 / 나머지: `x % y`
- 지수 연산 : `5 ** 2`

# 배열

```jsx
// 2개의 원소를 담을 수 있는 고정 길이의 배열:
uint[2] fixedArray;
// 또다른 고정 배열으로 5개의 스트링을 담을 수 있다:
string[5] stringArray;
// 동적 배열은 고정된 크기가 없으며 계속 크기가 커질 수 있다:
uint[] dynamicArray;
```

# Storage 와 Memory

**Storage**는 블록체인 상에 영구적으로 저장되는 변수이다.

**Memory**는 임시적으로 저장되는 변수로 컨트랙트 함수에 대한 외부 호출들이 일어나는 사이에 지워진다.

> **Storage**는 블록체인상에 영구적으로 저장되기 때문에 Gas가 가장 많이든다.

## Memmory에 배열 선언

```jsx
function getArray() external pure returns(uint[]) {
  // 메모리에 길이 3의 새로운 배열을 생성한다.
  uint[] memory values = new uint[](3);
  // 여기에 특정한 값들을 넣는다.
  values.push(1);
  values.push(2);
  values.push(3);
  // 해당 배열을 반환한다.
  return values;
}
```

# 구조체

솔리디티는 구조체를 제공합니다.

```jsx
struct Person {
  uint age;
  string name;
}
```

## 구조체 배열

구조체의 배열을 생성할 수 있다. 

```jsx
Person[] people;
```

## public 배열

public으로 배열을 선언할 수 있습니다.

솔리디티는 이런 배열을 위해 `getter`메소드를 자동으로 생성하지만 `readonly`로 생성됩니다.

이는 컨트랙트에 공개 데이터를 저장할 때 유용하다.

```jsx
Person[] public people;
```

## 구조체에 배열 추가하기

```jsx
// 새로운 사람을 생성한다:
Person satoshi = Person(172, "Satoshi");

// 이 사람을 배열에 추가한다:
people.push(satoshi);

// 위의 코드를 한줄로 표현
people.push(Person(172, "Satoshi"));
```

# 함수

솔리디티에는 함수도 있으며 아래와 같이 선언할 수 있다.

```jsx
function eatHamburgers(string _name, uint _amount) {

}
```

위 함수는 _name, _amount 2개의 인자를 string, uint 자료형으로 전달 받고 있다.

## 함수 호출

다른 언어와 똑같이 호출

```jsx
eatHamburgers("vitalik", 100);
```

## 함수 접근자

### Private, Public

솔리디티의 함수는 기본적으로 Public으로 선언된다. 즉 함수를 그냥 선언할 경우 누구나 함수를 호출할 수 있다는 것이다. 

> Private 키워드는 언더바(_)로 시작하는것이 관례이다.

### Internal과 External

**Internal**은  private와 같지만, 함수가 정의된 컨트랙트를 상속하는 컨트랙트에서도 접근이 가능하다.

**External**은 public와 같지만, 함수가 컨트랙트 바깥에서만 호출될 수 있고 컨트랙트 내의 다른 함수에 의해 호출될 수 없다.

### 보안

Public으로 함수를 선언하고 Internal 또는 External을 사용하여 보안적인 요소로 사용할 수 있다.

아래의 예제를 통해 살펴 보자.

```jsx
function setHello(string _say) public internal returns(string){
	return _say;
} // 외부에서 해당 함수를 호출할 수 없다.

function getHello() public {
	setHellow("hi solidity!"); // getHello함수를 통해 호출됨.
}
```

## 반환 값

```jsx
string greeting = "What's up dog";

function sayHello() public returns (string) {
  return greeting;
}
```

솔리디티에서는 함수에서 어떤 값을 반환하기 위해서는 위와 같이 함수를 작성해야한다.

> 위 코드에서는 `greeting` 를 `returns (string)` 타입으로 반환하고 있다.

### 여러값 반환

솔리디티에서는 여러개의 값을 반환 할 수 있다.

```jsx
function multipleReturns() internal returns(uint a, uint b, uint c) {
  return (1, 2, 3);
}

function processMultipleReturns() external {
  uint a;
  uint b;
  uint c;
  // 다음과 같이 다수 값을 할당한다:
  (a, b, c) = multipleReturns();
}

// 혹은 단 하나의 값에만 관심이 있을 경우: 
function getLastReturnValue() external {
  uint c;
  // 다른 필드는 빈칸으로 놓기만 하면 된다: 
  (,,c) = multipleReturns();
}
```

## 함수 제어자

함수 제어자는 다른 함수들에 대한 접근을 제어하기 위해 사용되는 일종의 유사 함수이다.

### **view 함수**

상태를 변화시키지 않는 함수가 있을 경우 함수에다 `view`를 추가 하여 함수를 선언한다.

보통의 경우 데이터를 읽기 할때 많이 사용하며, `view`는  Gas가 필요 없으므로 Gas를 절약하기 위해 자주 사용된다.

> 자바스크립트에서의 `readonly`와 비슷하게 작동하도록 하기 위해서는 `external view` 라고 작성한다.

```jsx
function sayHello() public view returns (string) {}
```

### **pure 함수**

앱에서 어떤 데이터도 접근하지 않는 함수는 `pure`를 추가하여 함수를 선언한다.

```jsx
function _multiply(uint a, uint b) private pure returns (uint) {
  return a * b;
}
```

## 함수 상태 제어자

### **modifier**

함수 상태 제어자는 기존의 함수를 작성하듯 작성도 가능하다.

아래의 컨트랙트는 오직 소유자만 해당 함수를 실행할 수 있게 작성되어있다.

```jsx
modifier onlyOwner() {
    require(msg.sender == owner);
    _; // 호출한 컨트랙트로 돌아간다.
  }
```

> `msg.sender` 는 컨트랙트를 호출한 사람의 주소이다.

### 인수를 가지는 함수 상태 제어자

함수제어자는 다른 함수들과 같이 인수받을 수 있다.

```jsx
// 사용자의 나이를 저장하기 위한 매핑
mapping (uint => uint) public age;

// 사용자가 특정 나이 이상인지 확인하는 제어자
modifier olderThan(uint _age, uint _userId) {
  require (age[_userId] >= _age);
  _;
}

// 차를 운전하기 위햐서는 16살 이상이어야 하네(적어도 미국에서는).
// `olderThan` 제어자를 인수와 함께 호출하려면 이렇게 하면 되네:
function driveCar(uint _userId) public olderThan(16, _userId) {
  // 필요한 함수 내용들
}
```

## Require

특정 조건이 참이 아닐 때 함수가 에러 메시지를 발생한다.

```jsx
require(A == B);
```

## Assert

특정 조건을 만족하지 않으면 에러가 발생한다.

```jsx
assert(A == B);
```

> **require**로 에러가 날경우 Gas를 사용하지 않지만, **assert**의 에러는 Gas를 사용한다.
**assert**는 ****코드가 심각하게 잘못 실행될 경우 사용된다.

## payable 제어자

함수가 실행하는 동시에 컨트랙트에 돈을 지불하는 것이 가능하다.

아래의 코드를 보자.

```jsx
contract OnlineStore {
  function buySomething() external payable {
    // 함수 실행에 0.001이더가 보내졌는지 확실히 하기 위해 확인:
    require(msg.value == 0.001 ether);
    // 보내졌다면, 함수를 호출한 자에게 디지털 아이템을 전달하기 위한 내용 구성:
    transferThing(msg.sender);
  }
}
```

> 참만약 함수가 payable로 표시되지 않았는데 자네가 위에서 본 것처럼 이더를 보내려 한다면, 함수에서 트랜잭션을 거부할 것이다.

## transfer

이더리움을 인출할 때 사용한다.

```jsx
function withdraw() external onlyOwner {
    owner.transfer(this.balance);
  }
```

# 이벤트

이벤트는 컨트랙트가 블록체인 상에서 앱의 사용자 단에서 무언가 액션이 발생했을 때 의사소통하는 방법이다.

컨트랙트는 특정 이벤트가 일어나는지 감시 하고 있다가 이벤트가 발생하면 행동을 취한다.

아래는 이벤트를 선언한 코드이다.

```jsx
// 이벤트를 선언한다
event IntegersAdded(uint x, uint y, uint result);

function add(uint _x, uint _y) public {
  uint result = _x + _y;
  // 이벤트를 실행하여 앱에게 add 함수가 실행되었음을 알린다:
  IntegersAdded(_x, _y, result);
  return result;
}
```

## Web3js에서 발생한 이벤트 받기

node에서 컨트랙트가 발생한 이벤트에 대해 피드백을 아래의 코드로 읽을 수 있다.

```jsx
YourContract.IntegersAdded(function(error, result) {
  // 결과와 관련된 행동을 취한다
})
```

# OpenZppeline

## Owner

스마트 컨트랙트의 소유자 주소를 지정할 수 있게 OpenZeppelin에서 제공하는 라이브러리이다.

## SafeMath

변수들의 오버플로우 및 언더플로우를 막기위해 OpenZeppelin에서 제공하는 라이브러리이다.

add, sub, mul, div 4개의 함수를 가지고있다. 

해당 함수는 아래와 같이 사용할 수 있다.

```jsx
using SafeMath for uint256;

uint256 a = 5;
uint256 b = a.add(3); // 5 + 3 = 8
uint256 c = a.mul(2); // 5 * 2 = 10
```
