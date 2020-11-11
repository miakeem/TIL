# 타입 변환과 단축 평가

# 1. 타입 변환이란?

자바스크립트의 모든 값은 타입이 있으며 값의 타입은 개발자의 의도에 따라 다른 타입으로 변환이 가능하다.

**명시적 타입 변환(explicit coercion)** 또는 **타입 캐스팅(type casting)** : 개발자가 의도적으로 값의 타입을 변환하는 것

<br>

**암묵적 타입 변환(implicit coercion)** 또는 **타입 강제 변환(type coercion)** : 개발자의 의도와는 상관없이 표현식을 평가하는 도중에 자바스크립트 엔진에 의해 암묵적으로 타입이 자동 변환되는 것

자바스크립트 엔진은 표현식을 에러 없이 평가하기 위해 피연산자의 값을 암묵적 타입 변환해 새로운 타입의 값을 만들어 단 한 번 사용하고 버린다. 이처럼 암묵적 타입 변환은 타입이 자동 변환되기 때문에 타입 변경에 대한 개발자의 의지가 코드에 명백히 나타나지 않는다.

따라서 자신이 작성한 코드의 타입 변환 결과를 예측하지 못하거나 예측이 결과와 일치하지 않는다면 오류를 생산할 가능성이 높아진다. 그러므로 코드를 예측할 수 있어야 한다. 동료가 작성한 코드를 정확히 이해할 수 있어야 하고 자신이 작성한 코드도 동료가 쉽게 이해할 수 있어야 한다. 이를 위해 타입 변환이 어떻게 동작하는지 정확히 이해하고 사용하자.

<br><br>

# 2. 암묵적 타입 변환

```javascript
// 피연산자가 모두 문자열 타입이어야 하는 문맥
"10" + 2; // -> '102'

// 피연산자가 모두 숫자 타입이어야 하는 문맥
5 * "10"; // -> 50

// 피연산자 또는 표현식이 불리언 타입이어야 하는 문맥
!0; // -> true
if (1) {
}
```

이처럼 표현식을 평가할 때 코드의 문맥에 부합하지 않는 다양한 상황이 발생할 수 있다. 이때 프로그래밍 언어에 따라 에러를 발생시키기도 하지만 자바스크립트는 가급적 에러를 발생시키지 않도록 암묵적 타입 변환을 통해 표현식을 평가한다.

<br>

## 2.1. 문자열 타입으로 변환

```javascript
1 + "2"; // -> "12"
```

위 예제의 + 연산자는 피연산자 중 하나 이상이 문자열이므로 *문자열 연결 연산자*로 동작한다. 문자열 연결 연산자는 문자열 값을 만든다. 따라서 문자열 연결 연산자의 모든 피연산자는 코드의 문맥상 모두 문자열 타입이어야 하기 때문에 자바스크립트 엔진은 문자열 연결 연산자 표현식을 평가하기 위해 문자열 연결 연산자의 피연산자 중에서 **문자열 타입이 아닌 피연산자를 문자열 타입으로 암묵적 타입 변환**한다.

자바스크립트 엔진은 문자열 타입 아닌 값을 문자열 타입으로 암묵적 타입 변환을 수행할 때 다음과 같이 동작한다.

```javascript
// 숫자 타입
0 + ''         // -> "0"
-0 + ''        // -> "0"
1 + ''         // -> "1"
-1 + ''        // -> "-1"
NaN + ''       // -> "NaN"
Infinity + ''  // -> "Infinity"
-Infinity + '' // -> "-Infinity"

// 불리언 타입
true + ''  // -> "true"
false + '' // -> "false"

// null 타입
null + '' // -> "null"

// undefined 타입
undefined + '' // -> "undefined"

// 심벌 타입
(Symbol()) + '' // -> TypeError: Cannot convert a Symbol value to a string

// 객체 타입
({}) + ''           // -> "[object Object]"
Math + ''           // -> "[object Math]"
[] + ''             // -> ""
[10, 20] + ''       // -> "10,20"
(function(){}) + '' // -> "function(){}"
Array + ''          // -> "function Array() { [native code] }"
```

<br>

## 2.2. 숫자 타입으로 변환

### 산술 연산자

```javascript
1 - "1"; // -> 0
1 * "10"; // -> 10
1 / "one"; // -> NaN
```

산술 연산자의 역할은 숫자 값을 만드는 것이다. 따라서 산술 연산자의 모든 피연산자는 코드 문맥 상 모두 숫자 타입이어야 한다.

자바스크립트 엔진은 산술 연산자 표현식을 평가하기 위해 산술 연산자의 피연산자 중에서 **숫자 타입이 아닌 피연산자를 숫자 타입으로 암묵적 타입 변환**한다. 이때 피연산자를 숫자 타입으로 변환할 수 없는 경우는 산술 연산을 수행할 수 없으므로 표현식의 평가 결과는 NaN이 된다.

<br>

### 비교 연산자

```javascript
"1" > 0; // -> true
```

`>` 비교 연산자는 피연산자의 크기를 비교하여 불리언 값을 만들기 때문에 모든 피연산자는 코드의 문맥상 모두 숫자 타입이어야 한다. 자바스크립트 엔진은 비교 연산자 표현식을 평가하기 위해 비교 연산자의 피연산자 중에서 숫자 타입이 아닌 피연산자를 숫자 타입으로 암묵적 타입 변환한다.

<br>

### 숫자 타입으로 암묵적 타입 변환

`+` 단항 연산자는 피연산자가 숫자 타입의 값이 아니면 숫자 타입의 값으로 암묵적 타입 변환을 수행한다.

```javascript
// 문자열 타입
+"" + // -> 0
  "0" + // -> 0
  "1" + // -> 1
  "string" + // -> NaN
  // 불리언 타입
  true + // -> 1
  false + // -> 0
  // null 타입
  null + // -> 0
  // undefined 타입
  undefined + // -> NaN
  // 심벌 타입
  Symbol() + // -> ypeError: Cannot convert a Symbol value to a number
  // 객체 타입
  {} + // -> NaN
  [] + // -> 0
  [10, 20] + // -> NaN
  function () {}; // -> NaN
```

<br>

### 0

- 빈 문자열('')
- 빈 배열([])
- null
- false

<br>

### 1

- true

<br>

### NaN

- 객체
- 빈 배열이 아닌 배열
- undefined

<br>

## 2.3. 불리언 타입으로 변환

```javascript
if ("") console.log(x);
```

제어문(if 문, for 문 등) 또는 삼항 조건 연산자의 조건식은 참/거짓으로 평가되어야 하는 표현식이므로 자바스크립트 엔진은 조건식의 평가 결과를 불리언 타입으로 암묵적 타입 변환한다.

```javascript
if ("") console.log("1");
if (true) console.log("2");
if (0) console.log("3");
if ("str") console.log("4");
if (null) console.log("5");

// 2 4
```

**자바스크립트 엔진은 불리언 타입이 아닌 값을 Truthy 값(참으로 평가되는 값) 또는 Falsy 값(거짓으로 평가되는 값)으로 구분한다.** 즉, 불리언 값으로 평가되어야 할 문맥에서 Truthy 값은 true로, Falsy 값은 false로 암묵적 타입 변환된다.

<br>

### Falsy 값

Falsy 값 외의 모든 값은 모두 true로 평가되는 Truthy 값이다.

- false
- undefined
- null
- 0, -0
- NaN
- ’’ (빈 문자열)

<br>

```javascript
// 전달받은 인수가 Falsy 값이면 true, Truthy 값이면 false를 반환한다.
function isFalsy(v) {
  return !v;
}

// 전달받은 인수가 Truthy 값이면 true, Falsy 값이면 false를 반환한다.
function isTruthy(v) {
  return !!v;
}

// 모두 true를 반환한다.
isFalsy(false);
isFalsy(undefined);
isFalsy(null);
isFalsy(0);
isFalsy(NaN);
isFalsy("");

// 모두 true를 반환한다.
isTruthy(true);
isTruthy("0"); // 빈 문자열이 아닌 문자열은 Truthy 값이다.
isTruthy({});
isTruthy([]);
```

<br>

<br>

# 3. 명시적 타입 변환

### 명시적으로 타입을 변경하는 방법

- 표준 빌트인 생성자 함수(String, Number, Boolean)를 new 연산자 없이 호출하는 방법
- 빌트인 메서드를 사용하는 방법
- 암묵적 타입 변환을 이용하는 방법

<br>

## 3.1. 문자열 타입으로 변환

문자열 타입이 아닌 값을 문자열 타입으로 변환하는 방법

1. **String 생성자 함수**를 new 연산자 없이 호출하는 방법

```javascript
// 숫자 타입 => 문자열 타입
String(1); // -> "1"
String(NaN); // -> "NaN"
String(Infinity); // -> "Infinity"
// 불리언 타입 => 문자열 타입
String(true); // -> "true"
String(false); // -> "false"
```

2. **Object.prototype.toString** 메서드를 사용하는 방법

```javascript
// 숫자 타입 => 문자열 타입
(1).toString(); // -> "1"
NaN.toString(); // -> "NaN"
Infinity.toString(); // -> "Infinity"
// 불리언 타입 => 문자열 타입
true.toString(); // -> "true"
false.toString(); // -> "false"
```

3. **문자열 연결 연산자**를 이용하는 방법

```javascript
1 + ""; // -> "1"
NaN + ""; // -> "NaN"
Infinity + ""; // -> "Infinity"
// 불리언 타입 => 문자열 타입
true + ""; // -> "true"
false + ""; // -> "false"
```

<br>

## 3.2. 숫자 타입으로 변환

### 숫자 타입이 아닌 값을 숫자 타입으로 변환하는 방법

1. Number 생성자 함수를 new 연산자 없이 호출하는 방법

```javascript
// 문자열 타입 => 숫자 타입
Number("0"); // -> 0
Number("-1"); // -> -1
Number("10.53"); // -> 10.53
// 불리언 타입 => 숫자 타입
Number(true); // -> 1
Number(false); // -> 0
```

2. parseInt, parseFloat 함수를 사용하는 방법(문자열만 숫자 타입으로 변환 가능)

```javascript
// 문자열 타입 => 숫자 타입
parseInt("0"); // -> 0
parseInt("-1"); // -> -1
parseFloat("10.53"); // -> 10.53
```

3. \+ 단항 산술 연산자를 이용하는 방법

```javascript
// 문자열 타입 => 숫자 타입
+"0"; // -> 0
+"-1"; // -> -1
+"10.53"; // -> 10.53
// 불리언 타입 => 숫자 타입
+true; // -> 1
+false; // -> 0
```

4.  \* 산술 연산자를 이용하는 방법

```javascript
// 문자열 타입 => 숫자 타입
"0" * 1; // -> 0
"-1" * 1; // -> -1
"10.53" * 1; // -> 10.53
// 불리언 타입 => 숫자 타입
true * 1; // -> 1
false * 1; // -> 0
```

<br>

## 3.3. 불리언 타입으로 변환

### 불리언 타입이 아닌 값을 불리언 타입으로 변환하는 방법

1. Boolean 생성자 함수를 new 연산자 없이 호출하는 방법

```javascript
// 문자열 타입 => 불리언 타입
Boolean("x"); // -> true
Boolean(""); // -> false
Boolean("false"); // -> true
// 숫자 타입 => 불리언 타입
Boolean(0); // -> false
Boolean(1); // -> true
Boolean(NaN); // -> false
Boolean(Infinity); // -> true
// null 타입 => 불리언 타입
Boolean(null); // -> false
// undefined 타입 => 불리언 타입
Boolean(undefined); // -> false
// 객체 타입 => 불리언 타입
Boolean({}); // -> true
Boolean([]); // -> true
```

2. ! 부정 논리 연산자를 두 번 사용하는 방법

```javascript
// 문자열 타입 => 불리언 타입
!!"x"; // -> true
!!""; // -> false
!!"false"; // -> true
// 숫자 타입 => 불리언 타입
!!0; // -> false
!!1; // -> true
!!NaN; // -> false
!!Infinity; // -> true
// null 타입 => 불리언 타입
!!null; // -> false
// undefined 타입 => 불리언 타입
!!undefined; // -> false
// 객체 타입 => 불리언 타입
!!{}; // -> true
!![]; // -> true
```

<br>

<br>

# 4. 단축 평가

## 4.1. 논리 연산자를 사용한 단축 평가

### 논리곱(&&)

```javascript
"Cat" && "Dog"; // -> "Dog"
```

논리곱(`&&`) 연산자는 좌항에서 우항으로 평가가 진행되며 두 개의 피연산자가 모두 true로 평가될 때 true를 반환한다.

위의 예제에서 첫 번째 피연산자 ‘Cat’은 Truthy 값이므로 true로 평가된다. 하지만 이 시점까지는 위 표현식을 평가할 수 없으므로 두 번째 피연산자가 위 논리곱 연산자 표현식의 평가 결과를 결정한다. 따라서 **논리곱 연산자는 논리 연산의 결과를 결정하는 두 번째 피연산자, 즉 문자열 ‘Dog’를 그대로 반환한다.**

<br>

### 논리합(||)

```javascript
"Cat" || "Dog"; // -> "Cat"
```

논리합(`||`) 연산자는 두 개의 피연산자 중 하나만 true로 평가되어도 true를 반환한다. (평가 진행 방향 상동)

첫 번째 피연산자 ‘Cat’은 Truthy 값이므로 true로 평가된다. 이 시점에 두 번째 피연산자까지 평가해 보지 않아도 위 표현식을 평가할 수 있다. 따라서 **논리합 연산자는 논리 연산의 결과를 결정한 첫 번째 피연산자, 즉 문자열 ‘Cat’을 그대로 반환한다.**

<br>

### 단축 평가(short-circuit evaluation)란?

논리곱(`&&`) 연산자와 논리합(`||`) 연산자와 같이 논리 연산의 결과를 결정하는 피연산자를 타입 변환하지 않고 그대로 반환하는데 이처럼 **표현식을 평가하는 도중에 평가 결과가 확정된 경우 나머지 평가 과정을 생략하는 것**을 말한다.

<br>

### 단축 평가 규칙

| 단축 평가 표현식    | 평가 결과 |
| :------------------ | :-------- |
| true \|\| anything  | true      |
| false \|\| anything | anything  |
| true && anything    | anything  |
| false && anything   | false     |

```javascript
// 논리합(||) 연산자
"Cat" || "Dog"; // -> "Cat"
false || "Dog"; // -> "Dog"
"Cat" || false; // -> "Cat"

// 논리곱(&&) 연산자
"Cat" && "Dog"; // -> "Dog"
false && "Dog"; // -> false
"Cat" && false; // -> false
```

<br>

### 단축 평가와 if 문

어떤 조건이 Truthy 값(참으로 평가되는 값)일 때 무언가를 해야 한다면 논리곱(`&&`) 연산자 표현식으로 if 문을 대체할 수 있다.

```javascript
var done = true;
var message = "";

// 주어진 조건이 true일 때
if (done) message = "완료";

// if 문은 단축 평가로 대체 가능하다.
// done이 true라면 message에 '완료'를 할당
message = done && "완료";
console.log(message); // 완료
```

<br>

주어진 조건이 Falsy 값(거짓으로 평가되는 값)일 때 무언가를 해야 한다면 논리합(`||`) 연산자 표현식으로 if 문을 대체할 수 있다.

```javascript
var done = false;
var message = "";

// 주어진 조건이 false일 때
if (!done) message = "미완료";

// if 문은 단축 평가로 대체 가능하다.
// done이 false라면 message에 '미완료'를 할당
message = done || "미완료";
console.log(message); // 미완료
```

<br>

### 삼항 조건 연산자와 if...else문

```javascript
var done = true;
var message = "";

// if...else 문
if (done) message = "완료";
else message = "미완료";
console.log(message); // 완료

// if...else 문은 삼항 조건 연산자로 대체 가능하다.
message = done ? "완료" : "미완료";
console.log(message); // 완료
```

<br>

## 4.2. 옵셔널 체이닝 연산자

옵셔널 체이닝(optional chaining) 연산자 `?.`는 좌항의 피연산자가 null 또는 undefined인 경우 undefined를 반환하고, 그렇지 않으면 우항의 프로퍼티 참조를 이어간다.

객체를 가리키길 기대하는 변수가 null 또는 undefined 의 여부 확인 및 프로퍼티 참조시 유용하다.

```javascript
var elem = null;

var value = elem?.value;
console.log(value); // undefined
```

<br>

옵셔널 체이닝 연산자 `?.`는 좌항 피연산자가 false로 평가되는 Falsy 값(false, undefined, null, 0, -0, NaN, ‘‘)이라도 null 또는 undefined가 아니면 우항의 프로퍼티 참조를 이어간다.

```javascript
var str = "";

var length = str?.length;
console.log(length); // 0
```

<br>

## 4.3. null 병합 연산자

null 병합(nullish coalescing) 연산자 `??`는 좌항의 피연산자가 null 또는 undefined인 경우 우항의 피연산자를 반환하고, 그렇지 않으면 좌항의 피연산자를 반환한다.

null 병합 연산자 `??`는 변수에 기본값을 설정할 때 유용하다.

```javascript
var foo = null ?? "default string";
console.log(foo); // "default string"
```

<br>

null 병합 연산자 `??`는 좌항의 피연산자가 false로 평가되는 Falsy 값(false, undefined, null, 0, -0, NaN, ‘‘)이라도 null 또는 undefined가 아니면 좌항의 피연산자를 그대로 반환한다.

```javascript
var foo = "" ?? "default string";
console.log(foo); // ""
```
