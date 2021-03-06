# 표현식과 문



## 1. 값

값은 식이 평가되어 생성된 결과를 말한다.

평가란, 식을 해석해서 값을 생성하거나 참조하는 것을 의미한다.





## 2. 리터럴

리터럴(literal)은 사람이 이해할 수 있는 문자 또는 약속된 기호를 사용해 값을 생성하는 표기 방식(notation)을 말한다.

자바스크립트 엔진은 코드가 실행되는 시점인 런타임(runtime)에 리터럴을 평가해 값을 생성한다. 즉, 리터럴은 값을 생성하기 위해 미리 약속한 표기법이라고 할 수 있다.

```javascript
// 정수 리터럴
100
// 부동소수점 리터럴
10.5
// 2진수 리터럴(0b로 시작)
0b01000001
// 8진수 리터럴(ES6에서 도입. 0o로 시작)
0o101
// 16진수 리터럴(ES6에서 도입. 0x로 시작)
0x41

// 문자열 리터럴
'Hello'
"World"

// 불리언 리터럴
true
false

// null 리터럴
null

// undefined 리터럴
undefined

// 객체 리터럴
{ name: 'Lee', address: 'Seoul' }

// 배열 리터럴
[ 1, 2, 3 ]

// 함수 리터럴
function() {}

// 정규표현식 리터럴
/[A-Z]+/g
```





## 3. 표현식

표현식(expression)은 값으로 평가될 수 있는 문(statement)이다. 즉, 표현식이 평가되면 새로운 값을 생성하거나 기존 값을 참조한다.

변수 식별자를 참조하면 변수 값으로 평가된다. 식별자 참조는 값을 생성하지는 않으나 값으로 평가되므로 표현식이다.

```javascript
// 리터럴 표현식
10
'Hello'

// 식별자 표현식(선언이 이미 존재한다고 가정)
sum
person.name
arr[1]

// 연산자 표현식
10 + 20
sum = 10
sum !== 10

// 함수/메서드 호출 표현식(선언이 이미 존재한다고 가정)
square()
person.getName()
```





## 4. 문

문(statement)은 프로그램을 구성하는 기본 단위이자 최소 실행 단위이다. 문의 집합이 프로그램이며, 문을 작성하고 순서에 맞게 나열하는 것이 프로그래밍이다.

문은 여러 토큰으로 구성된다. 토큰(token)이란 문법적인 의미를 가지며, 문법적으로 더 이상 나눌 수 없는 코드의 기본 요소를 의미한다.

<img src="https://poiemaweb.com/assets/fs-images/5-2.png" alt="img" style="zoom:50%;" />

문은 선언문, 할당문, 조건문, 반복문 등으로 구분할 수 있다. 변수 선언문을 실행하면 변수가 선언되고, 할당문을 실행하면 값이 할당이 된다. 조건문을 실행하면 지정한 조건에 따라 실행할 코드 블록({…})이 결정되어 실행되고, 반복문을 실행하면 특정 코드 블록이 반복 실행된다.

```javascript
// 변수 선언문
var x;

// 표현식 문(할당문)
x = 5;

// 함수 선언문
function foo () {}

// 조건문
if (x > 1) { console.log(x); }

// 반복문
for (var i = 0; i < 2; i++) { console.log(i); }
```





## 5. 세미콜론과 세미콜론 자동 삽입 기능

세미콜론(;)은 문의 종료를 나타낸다. 따라서 문을 끝낼 때는 세미콜론을 붙여야 한다. 단, 0개 이상의 문을 중괄호로 묶은 코드 블록 ({ … }) 뒤에는 세미콜론을 붙이지 않는다. 예를 들어, if 문, for 문, 함수 등의 코드 블록 뒤에는 세미콜론을 붙이지 않는다. 이러한 코드 블록은 언제나 문의 종료를 의미하는 자체 종결성(self closing)을 갖기 때문이다.

세미콜론은 옵션이므로 생략 가능하다. 자바스크립트 엔진에 의해 세미콜론 자동 삽입 기능(ASI, automatic semicolon insertion)이 암묵적으로 수행되기 때문이다.





## 6. 표현식인 문과 표현식이 아닌 문

표현식은 문의 일부일 수도 있고 그 자체로 문이 될 수도 있다. 

```javascript
// 변수 선언문은 값으로 평가될 수 없으므로 표현식이 아니다.
var x;
// 1, 2, 1 + 2, x = 1 + 2는 모두 표현식이다.
// x = 1 + 2는 표현식이면서 완전한 문이기도 하다.
x = 1 + 2;
```



### 표현식과 문을 구별하는 방법

표현식인 문은 값으로 평가될 수 있는 문이며 표현식이 아닌 문은 값으로 평가될 수 없는 문을 말한다. 예를 들어, 변수 선언문은 값으로 평가될 수 없다. 따라서 표현식이 아닌 문이다. 하지만 할당문은 값으로 평가될 수 있다. 따라서 표현식인 문이다.

```javascript
// 변수 선언문은 표현식이 아닌 문이다.
var x;

// 할당문은 그 자체가 표현식이지만 완전한 문이기도 하다. 즉, 할당문은 표현식인 문이다.
x = 100;

// 표현식이 아닌 문은 값처럼 사용할 수 없다.
var foo = var x; // SyntaxError: Unexpected token var
```

**표현식인 문과 표현식이 아닌 문을 구별하는 가장 간단하고 명료한 방법은 변수에 할당해 보는 것이다.** 표현식인 문은 값으로 평가되므로 변수에 할당할 수 있다. 하지만 표현식이 아닌 문은 값으로 평가할 수 없으므로 변수에 할당하면 에러가 발생한다.

이에 반해 위 예제의 할당문 x = 100은 그 자체가 표현식이다. 즉, 할당문은 표현식인 문이기 때문에 값처럼 사용할 수 있다. 

```javascript
// 표현식인 문은 값처럼 사용할 수 있다
var foo = x = 100;
console.log(foo); // 100
```

표현식인 문인 할당문은 할당한 값으로 평가된다. 즉, x = 100은 x 변수에 할당한 값 100으로 평가된다. 따라서 foo 변수에는 100이 할당된다.



> 완료 값(completion value)
>
> 크롬 개발자 도구에서 표현식이 아닌 문을 실행하면 언제나 undefined를 출력한다. 이를 완료 값이라 한다. 완료 값은 표현식의 평가 결과가 아니다. 따라서 다른 값과 같이 변수에 할당할 수 없고 참조할 수도 없다.



<img src="https://poiemaweb.com/assets/fs-images/5-4.png" alt="img" style="zoom: 80%;" />



크롬 개발자 도구에서 표현식인 문을 실행하면 언제나 평가된 값을 반환한다.

<img src="https://poiemaweb.com/assets/fs-images/5-5.png" alt="img" style="zoom: 50%;" />