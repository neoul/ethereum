# javascript

https://ko.javascript.info

- 자바스크립트의 공식이름?: `ECMAScript`

## javascript framework

- `jQuery`
- `Lodash`

## use strict

- `ECMAScript5(ES5)` 기본 문법 일부 

```javascript
"use strict"
// ES5 모드로 동작
```

### 브라우저 콘솔

개발한 기능을 테스트하기 위해 브라우저 콘솔을 사용하는 경우, 기본적으로 use strict가 적용되어 있지 않음

## 변수와 상수

javascript의 변수는 값 수정이 아니라 교체되는 개념

```javascript
let message;

message = 'Hello'; // 문자열을 저장합니다.
alert(message); // 변수에 저장된 값을 보여줍니다.

// 선언 방법
let user = 'John', age = 25, message = 'Hello';

let user = 'John';
let age = 25;
let message = 'Hello';

let user = 'John',
  age = 25,
  message = 'Hello';

// var는 let과 거의 동일하나, local에서 선언하더라도 global
var message = 'Hello';

// 상수 선언; 한번 할당되면 변경 불가
const myBirthday = '18.04.1982';
myBirthday = '01.01.2001'; // error, can't reassign the constant!

// 대문자 상수 (일반적 관습)
const COLOR_RED = "#F00";
const COLOR_GREEN = "#0F0";

// 상수 사용
let color = COLOR_ORANGE;
alert(color); // #FF7F00

// 아래 상수는 값 할당 전이므로 이후 할당 가능
// 할당후에는 변경 불가
const pageLoadTime = ;

```

### 변수 명명 규칙

- `문자`와 `숫자`, 그리고 기호 `$`와 `_` 사용
- 카멜 표기법(camelCase) 주로 사용
- 예약어(reserved name) 사용 X
- 첫단어 `숫자` X
- 대소문자 구별


### 함수형 언어

- 함수형(functional) 프로그래밍 언어는 변숫값 변경을 금지
- e.g. 스칼라(Scala)와 얼랭(Erlang)
- 병렬 계산(parallel computation)에 유용

### 즉시 실행 함수 표현식

즉시 실행 함수 표현식(immediately-invoked function expressions): 선언과 함께 함수 실행

```javascript
// IIFE를 만드는 방법

(function() {
  alert("함수를 괄호로 둘러싸기");
})();

(function() {
  alert("전체를 괄호로 둘러싸기");
}());

!function() {
  alert("표현식 앞에 비트 NOT 연산자 붙이기");
}();

+function() {
  alert("표현식 앞에 단항 덧셈 연산자 붙이기");
}();
```

## 자료형

### 숫자형

- 숫자형 범위: `(2^53-1)` ~ `-(2^53-1)`; `9007199254740991`
- `BigInt`: 끝에 `n`을 붙이면 `BigInt`
- `Infinity` : 무한대, `1/0` 로도 구해짐
- `-Infinity`
- `NaN`: 계산 오류, e.g. `"STR"/2`, 수로 문자를 나눌 경우

```javascript
let n = 123;
n = 12.345;

// 끝에 'n'이 붙으면 BigInt형 자료입니다.
const bigInt = 1234567890123456789012345678901234567890n;
```

### 문자형

- character type 없음
- 큰따옴표: "Hello"
- 작은따옴표: 'Hello'
- 역 따옴표(백틱, backtick): `Hello`

```javascript
let str = "Hello";
let str2 = 'Single quotes are ok too';
let phrase = `can embed another ${str}`;

let name = "John";
// backtick 사용시 - 변수를 문자열 중간에 삽입하거나 계산 가능
alert( `Hello, ${name}!` ); // Hello, John!
alert( `the result is ${1 + 2}` ); // the result is 3
```

### Boolean형

```javascript
let nameFieldChecked = true;
let ageFieldChecked = false;
let isGreater = 4 > 1;
```

### `null` 값

일반 null point의 이미가 아니라, `존재하지 않는(nothing)` 값, 
`비어 있는(empty)` 값, `알 수 없는(unknown)` 값을 나타내는 데 사용

```javascript
let age = null; // 나이(age)를 알 수 없거나 그 값이 비어있음
```

### `undefined` 값

`undefined`는 '값이 할당되지 않은 상태’를 나타낼 때 사용

```javascript
let age;
alert(age); // 'undefined' 출력
```

### `object` and `symbol` (객체와 심볼)

### `typeof`

- `typeof X` 연산자는 인수의 자료형을 문자열로 반환
- 자료형에 따라 처리 방식을 다르게 할 경우 사용
- 변수의 자료형 확인에 사용
- 연산자형 사용: `typeof x`
- 함수형 사용: `typeof(x)`

```javascript
typeof undefined // "undefined"
typeof 0 // "number"
typeof 10n // "bigint"
typeof true // "boolean"
typeof "foo" // "string"
typeof Symbol("id") // "symbol"
typeof Math // "object"  (1)
typeof null // "object"  (2)
typeof alert // "function"  (3)
```

## Browser functions

- `alart`: 메시지가 있는 작은 창, 모달 창(modal window)을 띄움
- `prompt`: 메시지와 입력 필드(input field), 확인(OK) 및 취소(Cancel) 버튼이 있는 모달 창을 띄움
- `confirm`: 질문과 확인 및 취소 버튼이 있는 모달 창을 띄움

```javascript
let age = prompt('나이를 입력해주세요.', 100);
alert(`당신의 나이는 ${age}살 입니다.`);

let isBoss = confirm("당신이 주인인가요?");
alert( isBoss );
```

## 형변환 (type conversion)

```javascript
// 문자열 변환
let value = true; // boolean형
value = String(value); // 변수 value엔 문자열 "true"가 저장
alert(typeof value); // string

// 숫자로 변환
let str = "123";
let num = Number(str); // 문자열 "123"이 숫자 123으로 명시적 변환
alert(typeof num); // number
alert( "6" / "2" ); // 3, 문자열이 숫자형으로 자동변환된 후 연산이 수행

let age = Number("임의의 문자열 123");
alert(age); // NaN, 형 변환 실패
alert( Number("   123   ") ); // 123
alert( Number("123z") );      // NaN ("z"를 숫자로 변환하는 데 실패함)
alert( Number(true) );        // 1
alert( Number(false) );       // 0

// boolean으로 변환
alert( Boolean(1) ); // 숫자 1(true)
alert( Boolean(0) ); // 숫자 0(false)
alert( Boolean("hello") ); // 문자열(true)
alert( Boolean("") ); // 빈 문자열(false)
alert( Boolean(NaN) ); // false
alert( Boolean(undefined) ); // false
alert( Boolean(null) ); // false
```

## 연산자

- `+`: 덧셈 연산자
- `-`: 뺄셈 연산자
- `*`: 곱셈 연산자
- `/`: 나눗셈 연산자
- `%`: 나머지 연산자
- `**`: 거듭제곱 연산자
- `&`: AND 비트 연산자
- `|` OR 비트 연산자
- `^` XOR 비트 연산자
- `~` NOT 비트 연산자
- `<<`: LEFT SHIFT
- `>>`: RIGHT SHIFT
- `>>>`: ZERO-FILL RIGHT SHIFT
- `||`: OR 논리연산자
- `&&`: AND 논리연산자
- `!`: NOT 논리연산자

```javascript
// 숫자
alert( 5 % 2 ); // 5를 2로 나눈 후의 나머지인 1을 출력
alert( 8 % 3 ); // 8을 3으로 나눈 후의 나머지인 2를 출력
alert( 2 ** 4 ); // 16 (2 * 2 * 2 * 2)
alert( 4 ** (1/2) ); // 2 (1/2 거듭제곱은 제곱근)
alert( 8 ** (1/3) ); // 2 (1/3 거듭제곱은 세제곱근)

// 문자열
let s = "my" + "string";
alert(s); // mystring
alert( 2 + '1' ); // "21"
alert(2 + 2 + '1' ); // '221'이 아니라 '41'이 출력됩니다.
alert( 6 - '2' ); // 4, '2'를 숫자로 바꾼 후 연산이 진행됩니다.
alert( '6' / '2' ); // 3, 두 피연산자가 숫자로 바뀐 후 연산이 진행됩니다.
// 숫자형이 아닌 피연산자는 숫자형으로 변화합니다.
alert( +true ); // 1
alert( +"" );   // 0

let apples = "2";
let oranges = "3";
alert( apples + oranges ); // 23, 문자열 + 연산
alert( +apples + +oranges ); // 5 숫자변환 -> 숫자 + 연산
```

### 연산자 우선순위 Operator precedence table

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence

### 할당 연산자

```javascript
let a = 1;
let b = 2;
let c = 3 - (a = b + 1); // a = 3, c = 0

alert( a ); // 3
alert( c ); // 0

a = b = c = 2 + 2;

alert( a ); // 4
alert( b ); // 4
alert( c ); // 4

let n = 2;
n += 5; // n = n + 5
n *= 2; // n = n * 2
alert( n ); // 14
n *= 3 + 5; // n *= 8

let counter = 2;
counter++; // counter = counter + 1
counter--; // counter = counter - 1

counter = 1;
let a = ++counter;
alert(a); // 2

counter = 1;
a = counter++;
alert(a); // 1

```

### 쉼표 연산자

```javascript
// 한 줄에서 세 개의 연산이 수행됨
for (a = 1, b = 3, c = a * b; a < 10; a++) {
 ...
}
```

## 비교 연산자

- a `>` b
- a `<` b
- a `>=` b
- a `<=` b
- a `==` b
- a `!=` b

### 문자열 비교

자릿수 별로 사전순으로 비교; ascii등의 문자 순위

``` javascript
alert( 'Z' > 'A' ); // true
alert( 'Glow' > 'Glee' ); // true
alert( 'Bee' > 'Be' ); // true
```

### 다른형간의 비교

쓰지 않는게 좋을 듯 ..., 명시적으로 변형해 사용하시라.

### 일치 연산자(strict equality operator) ===

자료형의 동등 여부까지 검사; 피연산자 a와 b의 형이 다를 경우 a === b는 즉시 false를 반환함

### null이나 undefined와 비교하기

```javascript
alert( null === undefined ); // false
alert( null == undefined ); // true

alert( undefined > 0 ); // false (1)
alert( undefined < 0 ); // false (2)
alert( undefined == 0 ); // false (3)

alert( null > 0 );  // (1) false
alert( null == 0 ); // (2) false
alert( null >= 0 ); // (3) true

```

### 단락 평가 - short circuit evaluation

`bash` 동작과 동일

```javascript
true || alert("not printed");
false || alert("printed");
```

###  nullish 병합 연산자 (nullish coalescing operator) `??`

`a ?? b`의 평가 결과는 다음과 같습니다.

a가 `null`도 아니고 `undefined`도 아니면 `a`
그 외의 경우는 `b`

```javascript
x = a ?? b // nullish 아래와 동일하게 풀어쓸 수 있음.
x = (a !== null && a !== undefined) ? a : b;

// 사용예
let firstName = null;
let lastName = null;
let nickName = "바이올렛";
alert(firstName ?? lastName ?? nickName ?? "익명의 사용자"); // 바이올렛

// '??'와 '||'의 차이
let height = 0;
alert(height || 100); // 100
alert(height ?? 100); // 0
```


## 조건문

- `if`, `else if` and `else`
- `?`

```javascript
let year = prompt('ECMAScript-2015 명세는 몇 년도에 출판되었을까요?', '');
if (year == 2015) {
  alert( "정답입니다!" );
  alert( "아주 똑똑하시네요!" );
} else if (year > 2015) {
  alert( '숫자를 좀 더 내려보세요.' );
} else {
  alert( '오답입니다!' );
}

// let result = condition ? value1 : value2;
let accessAllowed = (age > 18) ? true : false;
let message = (age < 3) ? '아기야 안녕?' :
  (age < 18) ? '안녕!' :
  (age < 100) ? '환영합니다!' :
  '나이가 아주 많으시거나, 나이가 아닌 값을 입력 하셨군요!';

```

## 반복문

```javascript
// while
let i = 0;
while (i < 3) { // 0, 1, 2가 출력됩니다.
  alert( i );
  i++;
}

// do while
let i = 0;
do {
  alert( i );
  i++;
} while (i < 3);

// for
for (let i = 0; i < 3; i++) { // 0, 1, 2가 출력됩니다.
  alert(i);
}

// break
let sum = 0;
while (true) {
  let value = +prompt("숫자를 입력하세요.", '');
  if (!value) break; // (*)
  sum += value;
}
alert( '합계: ' + sum );
```

## switch/case문

switch/case문의 인수엔 어떤 표현식이든 올 수 있음

```javascript
let a = 2 + 2;
let b = 0;
switch (a) {
  case b + 1:
    break;
  case 3:
    alert( '비교하려는 값보다 작습니다.' );
    break;
  case 4:
    alert( '비교하려는 값과 일치합니다.' );
    break;
  case 5:
  case 6:
    alert( '비교하려는 값보다 큽니다.' );
    break;
  default:
    alert( "어떤 값인지 파악이 되지 않습니다." );
}
```

## Function

- local variable: 함수 내 선언
- 매개변수에 값을 전달하지 않으면 그 값은 `undefined`
- 자바스크립트는 함수를 특별한 종류의 값으로 취급

```javascript
// Function declaration
let userName = 'John'; // global variable
function showMessage() {
  let message = 'Hello, ' + userName; // local variable
  alert(message);
}

showMessage(); // Hello, John

// Function arguments
function showMessage(from, text) { // 인수: from, text
  alert(from + ': ' + text);
}
showMessage(from, "Hello"); // *Ann*: Hello
showMessage("Ann"); // Ann: undefined

// Function arguments with default value
function showMessage(from, text= "no text given") { // 인수: from, text
  alert(from + ': ' + text);
}
showMessage("Ann"); // Ann: undefined

// Function return; return이 없을 경우 undefined
function sum(a, b) {
  return a + b;
}

// Function return 주의 사항
function sum(a, b) {
  return 
    a + b; // return후 ; 자동 삽입되므로 return시 개행 하지 않기
}
```

### Function Expression (함수 표현식)

```javascript
let sayHi = function() {
  alert( "Hello" );
}; // 함수 표현식에서 세미콜론 필수

alert( sayHi ); // 함수 코드 출력
alert( sayHi() ); // 함수 실행!!

```

### Arrow function (화살표 함수)

- 함수 축약 (python lambda)
- this를 가지지 않음 (선언된 상위 object의 this 가져다 씀)
- arguments 없음
- new와 함께 호출 불가능
- super도 없음
- 자체 '컨텍스트’가 없는 짧은 코드용

```javascript
let func = (arg1, arg2, ...argN) => expression

// e.g.
let sum = (a, b) => a + b;
let double = n => n * 2; // let double = function(n) { return n * 2 }
let sayHi = () => alert("안녕하세요!");

let age = prompt("나이를 알려주세요.", 18);
let welcome = (age < 18) ?
  () => alert('안녕') :
  () => alert("안녕하세요!");
welcome();

// example 1
let group = {
  title: "1모둠",
  students: ["보라", "호진", "지민"],

  showList() {
    this.students.forEach(
      student => alert(this.title + ': ' + student)
    );
  }
};

group.showList();

// example 2
function defer(f, ms) {
  return function() {
    setTimeout(() => f.apply(this, arguments), ms)
  };
}

function sayHi(who) {
  alert('안녕, ' + who);
}

let sayHiDeferred = defer(sayHi, 2000);
sayHiDeferred("철수"); // 2초 후 "안녕, 철수"가 출력됩니다.
```

