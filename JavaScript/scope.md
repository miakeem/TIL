# 스코프

## 1. 스코프란?

- 스코프는 식별자가 유효한 범위를 말한다.

- 스코프(scope, 유효범위)는 자바스크립트를 포함한 모든 프로그래밍 언어의 중요한 개념이다.

- 자바스크립트의 스코프는 다른 언어의 스코프와 구별되는 특징이 있다.

- var 키워드로 선언한 변수, let 또는 const 키워드로 선언한 변수의 스코프는 다르게 동작한다.

- 스코프는 변수, 함수와 깊은 관련이 있다.

- 함수의 매개변수를 참조할 수 있는 유효범위, 즉 매개변수의 스코프가 함수 몸체 내부로 한정된다.

```javascript
function add(x, y) {
  // 매개변수는 함수 몸체 내부에서만 참조할 수 있다.
  // 즉, 매개변수의 스코프(유효범위)는 함수 몸체 내부다.
  console.log(x, y); // 2 5
  return x + y;
}

add(2, 5);

// 매개변수는 함수 몸체 내부에서만 참조할 수 있다.
console.log(x, y); // ReferenceError: x is not defined
```

- 변수는 코드의 가장 바깥 영역뿐 아니라 코드 블록이나 함수 몸체 내에서도 선언할 수 있다.
- 이때 코드 블록이나 함수는 중첩될 수 있다.

```javascript
var var1 = 1; // 코드의 가장 바깥 영역에서 선언한 변수

if (true) {
  var var2 = 2; // 코드 블록 내에서 선언한 변수
  if (true) {
    var var3 = 3; // 중첩된 코드 블록 내에서 선언한 변수
  }
}

function foo() {
  var var4 = 4; // 함수 내에서 선언한 변수

  function bar() {
    var var5 = 5; // 중첩된 함수 내에서 선언한 변수
  }
}

console.log(var1); // 1
console.log(var2); // 2
console.log(var3); // 3
console.log(var4); // ReferenceError: var4 is not defined
console.log(var5); // ReferenceError: var5 is not defined
```

- 변수는 자신이 선언된 위치에 의해 자신이 유효한 범위,

  즉 다른 코드가 변수 자신을 참조할 수 있는 범위가 결정된다.

- 모든 식별자(변수 이름, 함수 이름, 클래스 이름 등)는 자신이 선언된 위치에 의해 다른 코드가 식별자 자신을 참조할 수 있는 유효 범위가 결정된다.
- 이를 스코프라 한다.

<br>

#### 식별자 결정(identifier resolution)

```javascript
var x = "global";

function foo() {
  var x = "local";
  console.log(x); // ①
}

foo();

console.log(x); // ②
```

- 자바스크립트 엔진은 이름이 같은 두 개의 변수 ①과 ② 중에서 어떤 변수를 참조해야 할 것인지를 결정해야 한다.
- 자바스크립트 엔진은 스코프를 통해 어떤 변수를 참조해야 할 것인지 결정한다.
- 따라서 스코프란 자바스크립트 엔진이 **식별자를 검색할 때 사용하는 규칙**이라고도 할 수 있다.

<br>

자바스크립트 엔진은 코드를 실행할 때 코드의 문맥(context)을 고려한다. 코드가 어디서 실행되며 주변에 어떤 코드가 있는지에 따라 위 예제의 ①과 ②처럼 동일한 코드도 다른 결과를 만들어 낸다.

위 예제에서 코드의 가장 바깥 영역에 선언된 x 변수는 어디서든 참조할 수 있다. 하지만 foo 함수 내부에서 선언된 x 변수는 foo 함수 내부에서만 참조할 수 있다. 이때 두개의 x 변수는 식별자 이름이 동일하지만 스코프는 다른 별개의 변수다.

<img src="https://poiemaweb.com/assets/fs-images/13-1.png" alt="img" style="zoom:50%;" />

- 프로그래밍 언어에서는 스코프(유효 범위)를 통해 식별자인 변수 이름의 충돌을 방지하여 같은 이름의 변수를 사용할 수 있게 한다.

- 스코프 내에서 식별자는 유일해야 하지만 다른 스코프에는 같은 이름의 식별자를 사용할 수 있다.
- 즉, 스코프는 네임스페이스다.

> 네임스페이스
>
> **네임스페이스**(Namespace)는 개체를 구분할 수 있는 범위를 나타내는 말로 일반적으로 하나의 이름 공간에서는 하나의 이름이 단 하나의 개체만을 가리키게 된다.

<br>

```javascript
function foo() {
  var x = 1;
  // var 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용한다.
  // 아래 변수 선언문은 자바스크립트 엔진에 의해 var 키워드가 없는 것처럼 동작한다.
  var x = 2;
  console.log(x); // 2
}
foo();
```

하지만 **let**이나 **const** 키워드로 선언된 변수는 **같은 스코프 내에서 중복 선언을 허용하지 않는다**.

```javascript
function bar() {
    let x = 1;
    // let이나 const 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용하지 않는다.
    let x = 2; // SyntaxError: Identifier 'x' has already been declared
}
```

<br><br>

## 2. 스코프의 종류

| 구분 | 설명                  | 스코프      | 변수      |
| :--: | :-------------------- | :---------- | :-------- |
| 전역 | 코드의 가장 바깥 영역 | 전역 스코프 | 전역 변수 |
| 지역 | 함수 몸체 내부        | 지역 스코프 | 지역 변수 |

- 코드는 전역(global)과 지역(local)으로 구분할 수 있다.
- 변수는 자신이 선언된 위치(전역 또는 지역)에 의해 자신이 유효한 범위인 스코프가 결정된다.

<br>

### 2.1. 전역과 전역 스코프

#### 전역이란?

- 코드의 가장 바깥 영역을 말한다.
- 전역은 전역 스코프(global scope)를 만든다.
- 전역에 변수를 선언하면 전역 스코프를 갖는 전역 변수(global variable)가 된다. 
- **전역 변수는 어디서든지 참조할 수 있다.**

<br>

### 2.2. 지역과 지역 스코프

- 지역이란 **함수 몸체 내부**를 말한다. 
- 지역은 지역 스코프(local scope)를 만든다.
- 지역에 변수를 선언하면 지역 스코프를 갖는 지역 변수(local variable)가 된다.
- **지역 변수는 자신의 지역 스코프와 하위 지역(중첩 함수) 스코프에서 유효하다.**



<img src="https://poiemaweb.com/assets/fs-images/13-2.png" alt="img" style="zoom:50%;" />

위 예제에서 outer 함수 내부에서 선언된 z 변수는 지역 변수다. 지역 변수 z는 자신의 지역 스코프인 outer 함수 내부와 하위 지역 스코프인 inner 함수 내부에서 참조할 수 있다. 하지만 지역 변수 z를 전역에서 참조하면 참조 에러가 발생한다.

inner 함수 내부에서 선언된 x 변수도 지역 변수이며 이는 inner 내부에서만 참조할 수 있다. 따라서 지역 변수 x를 전역 또는 inner 함수 내부 이외의 지역에서 참조하면 참조 에러가 발생한다.

그런데 inner 함수 내부에서 선언된 x 변수 이외에 이름이 같은 전역 변수 x가 존재한다. 이때 inner 함수 내부에서 x 변수를 참조하면 전역 변수 x를 참조하는 것이 아니라 inner 함수 내부에서 선언된 x 변수를 참조한다. 이는 자바스크립트 엔진이 스코프 체인을 통해 참조할 변수를 검색(identifier resolution)했기 때문이다.

<br><br>

## 3. 스코프 체인

- 함수는 전역에서 정의할 수도 있고 함수 몸체 내부에서 정의할 수도 있다. 

- 함수는 중첩될 수 있으므로 함수의 지역 스코프도 중첩될 수 있다. 
- **스코프가 함수의 중첩에 의해 계층적 구조를 갖는다**는 것을 의미한다.



#### 중첩 함수(nested function)

함수 몸체 내부에서 함수가 정의된 것을 ‘함수의 중첩’이라 하며 함수 몸체 내부에서 정의한 함수를 중첩 함수라고 한다.

#### 외부 함수(outer function)

중첩 함수를 포함하는 함수를 말한다.

중첩 함수의 지역 스코프는 중첩 함수를 포함하는 외부 함수의 지역 스코프와 계층적 구조를 갖는다. 이때 외부 함수의 지역 스코프를 중첩 함수의 상위 스코프라 한다.

<br>

outer 함수의 지역과 inner 함수의 지역을 갖고 있는 2.2. 지역과 지역 스코프의 예제를 다시 보자.

- inner 함수는 outer 함수의 중첩 함수다.

- 이때 outer 함수가 만든 지역 스코프는 inner 함수가 만든 지역 스코프의 상위 스코프다.
- outer 함수의 지역 스코프의 상위 스코프는 전역 스코프다. 이러한 계층 구조를 그림으로 나타내면 다음과 같다.

<img src="https://poiemaweb.com/assets/fs-images/13-3.png" alt="img" style="zoom: 67%;" />

#### 스코프 체인(scope chain)

이처럼 **모든 스코프는 하나의 계층적 구조로 연결**되며, 모든 지역 스코프의 **최상위 스코프는 전역 스코프이다.** 이것을 스코프 체인이라 한다.



**변수를 참조할 때 자바스크립트 엔진은 스코프 체인을 통해 변수를 참조하는 코드의 스코프에서 시작하여 상위 스코프 방향으로 이동하며 선언된 변수를 검색(identifier resolution)한다.** 이를 통해 상위 스코프에서 선언한 변수를 하위 스코프에서도 참조할 수 있다.

- 스코프 체인은 물리적인 실체로 존재한다. 

- 자바스크립트 엔진은 코드(전역 코드와 함수 코드)를 실행하기에 앞서 위 그림과 유사한 자료구조인 렉시컬 환경(Lexical Environment)을 실제로 생성한다.

- 변수 선언이 실행되면 변수 식별자가 이 자료구조(렉시컬 환경)에 키로 등록되고,

  변수 할당이 일어나면 이 자료구조의 변수 식별자에 해당하는 값을 변경한다.

- 변수의 검색도 이 자료구조 상에서 이뤄진다.

<br>

> 렉시컬 환경(Lexical Environment)
>
> 스코프 체인은 실행 컨텍스트의 렉시컬 환경을 단방향으로 연결(chaining)한 것이다. 전역 렉시컬 환경은 코드가 로드되면 곧바로 생성되고 함수의 렉시컬 환경은 함수가 호출되면 곧바로 생성된다

<br>

### 3.1. 스코프 체인에 의한 변수 검색

위 예제의 ④, ⑤, ⑥을 보면 자바스크립트 엔진이 스코프 체인을 통해 어떻게 변수를 찾아내는지 이해할 수 있다.

- 자바스크립트 엔진은 스코프 체인을 따라 변수를 참조하는 코드의 스코프에서 시작해서 상위 스코프 방향으로 이동하며 선언된 변수를 검색한다.
- **상위 스코프에서 유효한 변수는 하위 스코프에서 자유롭게 참조할 수 있지만 하위 스코프에서 유효한 변수를 상위 스코프에서 참조할 수 없다.**

- 스코프 체인으로 연결된 스코프의 계층적 구조는 부자 관계로 이뤄진 상속(inheritance)과 유사하다.

- 상속을 통해 부모의 자산을 자식이 자유롭게 사용할 수 있지만 자식의 자산을 부모가 사용할 수는 없다. 

<br>

### 3.2. 스코프 체인에 의한 함수 검색

```javascript
// 전역 함수
function foo() {
  console.log('global function foo');
}

function bar() {
  // 중첩 함수
  function foo() {
    console.log('local function foo');
  }

  foo(); // ①
}

bar();
```

- 함수 선언문으로 정의된 함수는 런타임 이전에 함수 객체가 먼저 생성되고 자바스크립트 엔진이 함수 이름과 동일한 이름의 식별자를 암묵적으로 선언하고 생성된 함수 객체를 할당하므로 위 예제의 모든 함수는 함수 이름과 동일한 이름의 식별자에 할당된다. 
- ①에서 함수를 호출하면 자바스크립트 엔진은 함수를 호출하기 위해 먼저 함수를 가리키는 식별자 foo를 검색한다.
- 이처럼 함수도 식별자에 할당되기 때문에 스코프를 갖는다.
- 따라서 스코프를 “변수를 검색할 때 사용하는 규칙”이라고 표현하기 보다는 **“식별자를 검색하는 규칙”**이라고 표현하는 편이 좀 더 적합하다.

<br><br>

### 4. 함수 레벨 스코프

- **지역 스코프는 코드 블록이 아닌 함수에 의해서만 생성된다.**
- 대부분의 프로그래밍 언어는 함수 몸체만이 아니라 모든 코드 블록(if, for, while, try/catch 등)이 지역 스코프를 만든다. 이러한 특성을 **블록 레벨 스코프(block level scope)**라 한다.
- **var 키워드로 선언된 변수는 오로지 함수의 코드 블록(함수 몸체)만을 지역 스코프로 인정한다.** 이러한 특성을 **함수 레벨 스코프(function level scope)**라 한다. 

```javascript
var x = 1;

if (true) {
  // var 키워드로 선언된 변수는 함수의 코드 블록(함수 몸체)만을 지역 스코프로 인정한다.
  // 함수 밖에서 var 키워드로 선언된 변수는 코드 블록 내에서 선언되었다 할지라도 모두 전역 변수다.
  // 따라서 x는 전역 변수다. 이미 선언된 전역 변수 x가 있으므로 x 변수는 중복 선언된다.
  // 이는 의도치 않게 변수 값이 변경되는 부작용을 발생시킨다.
  var x = 10;
}

console.log(x); // 10
```

전역 변수 x가 선언되었고 if 문의 코드 블록 내에도 x 변수가 선언되었다. 이때 if 문의 코드 블록 내에서 선언된 x 변수는 전역 변수다.

var 키워드로 선언된 변수는 함수 레벨 스코프만 인정하기 때문에 함수 밖에서 var 키워드로 선언된 변수는 코드 블록 내에서 선언되었다 할지라도 모두 전역 변수다.

따라서 전역 변수 x는 중복 선언되고 그 결과 의도치 않은 전역 변수의 값이 재할당된다.

```javascript
var i = 10;

// for 문에서 선언한 i는 전역 변수다. 이미 선언된 전역 변수 i가 있으므로 중복 선언된다.
for (var i = 0; i < 5; i++) {
  console.log(i); // 0 1 2 3 4
}

// 의도치 않게 변수의 값이 변경되었다.
console.log(i); // 5
```

블록 레벨 스코프를 지원하는 프로그래밍 언어에서는 for 문에서 반복을 위해 선언된 i 변수가 for 문의 코드 블록 내에서만 유효한 지역 변수다. 이 변수를 for 문 외부에서 사용할 일은 없기 때문이다. 하지만 var 키워드로 선언된 변수는 블록 레벨 스코프를 인정하지 않기 때문에 i 변수는 전역 변수가 된다. 따라서 전역 변수 i는 중복 선언되고 그 결과 의도치 않은 전역 변수의 값이 재할당된다.

- var 키워드로 선언된 변수는 오로지 함수의 코드 블록 만을 지역 스코프로 인정하지만, ES6에서 도입된 let, const 키워드는 블록 레벨 스코프를 지원한다. 

<br>

<br>

## 5. 렉시컬 스코프

```javascript
var x = 1;

function foo() {
  var x = 10;
  bar();
}

function bar() {
  console.log(x);
}

foo(); // 1
bar(); // 1
```

#### 1. 동적 스코프(dynamic scope) 방식

**함수를 어디서 호출**했는지에 따라 함수의 상위 스코프를 결정한다면 bar 함수의 상위 스코프는 foo 함수의 지역 스코프와 전역 스코프일 것이다.

-> 함수를 정의하는 시점에는 함수가 어디서 호출될지 알 수 없다. 따라서 함수가 호출되는 시점에 동적으로 상위 스코프를 결정해야 하기 때문에 동적 스코프라고 부른다.

<br>

#### 2. 렉시컬 스코프(lexical scope) 또는 정적 스코프(static scope)

**함수를 어디서 정의**했는지에 따라 함수의 상위 스코프를 결정한다면 bar 함수의 상위 스코프는 전역 스코프일 것이다.

-> 동적 스코프 방식처럼 상위 스코프가 동적으로 변하지 않고 함수 정의가 평가되는 시점에 상위 스코프가 정적으로 결정되기 때문에 정적 스코프라고 부른다. 자바스크립트를 비롯한 대부분의 프로그래밍 언어는 렉시컬 스코프를 따른다.

프로그래밍 언어는 일반적으로 이 두 가지 방식 중 한 가지 방식으로 함수의 상위 스코프를 결정한다.

<br>

**자바스크립트는 렉시컬 스코프를 따르므로 함수를 어디서 호출했는지가 아니라 함수를 어디서 정의했는지에 따라 상위 스코프를 결정한다. 함수가 호출된 위치는 상위 스코프 결정에 어떠한 영향도 주지 않는다. 즉, 함수의 상위 스코프는 언제나 자신이 정의된 스코프다.**

**이처럼 함수의 상위 스코프는 함수 정의가 실행될 때 정적으로 결정된다. 함수 정의(함수 선언문 또는 함수 표현식)가 실행되어 생성된 함수 객체는 이렇게 결정된 상위 스코프를 기억한다. 함수가 호출될 때마다 함수의 상위 스코프를 참조할 필요가 있기 때문이다.**

<br>

------

<br>

### 참고자료

- [PoiemaWeb - 스코프](https://poiemaweb.com/fastcampus/scope)

- [위키백과 - 이름공간](https://ko.wikipedia.org/wiki/%EC%9D%B4%EB%A6%84%EA%B3%B5%EA%B0%84)
