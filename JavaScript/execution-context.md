# 실행 컨텍스트

실행 컨텍스트(execution context)는 자바스크립트의 동작 원리를 담고 있는 핵심 개념으로 아래의 개념을 설명할 수 있다.

-  자바스크립트가 스코프를 기반으로 식별자와 식별자에 바인딩된 값을 관리하는 방식
- 호이스팅 발생 이유
- 클로저 동작 방식
- 태스크 큐와 함께 동작하는 이벤트 핸들러와 비동기 처리의 동작 방식

<br><br>

## 1. 소스코드의 타입

ECMAScript 사양은 실행 컨텍스트를 생성하는 소스코드(ECMAScript code)를 4가지 타입으로 구분한다.

| 소스코드의 타입          | 설명                                                         |
| :----------------------- | :----------------------------------------------------------- |
| 전역 코드(global code)   | 전역에 존재하는 소스코드를 말한다. 전역에 정의된 함수, 클래스 등의 내부 코드는 포함되지 않는다. |
| 함수 코드(function code) | 함수 내부에 존재하는 소스코드를 말한다. 함수 내부에 중첩된 함수, 클래스 등의 내부 코드는 포함되지 않는다. |
| eval 코드(eval code)     | 빌트인 전역 함수인 eval 함수에 인수로 전달되어 실행되는 소스코드를 말한다. |
| 모듈 코드(module code)   | 모듈 내부에 존재하는 소스코드를 말한다. 모듈 내부의 함수, 클래스 등의 내부 코드는 포함되지 않는다. |

소스코드(실행 가능한 코드, executable code)를 4가지 타입으로 구분하는 이유는 **소스코드의 타입에 따라 실행 컨텍스트를 생성하는 과정과 관리 내용이 다르기 때문**이다.

<img src="https://poiemaweb.com/assets/fs-images/23-1.png" alt="img" style="zoom:50%;" />

#### 전역 코드

전역 코드는 전역 변수를 관리하기 위해 최상위 스코프인 전역 스코프를 생성해야 한다. 그리고 var 키워드로 선언된 전역 변수와 함수 선언문으로 정의된 전역 함수를 전역 객체의 프로퍼티와 메서드로 바인딩하고 참조하기 위해 전역 객체와 연결되어야 한다. 이를 위해 전역 코드가 평가되면 전역 실행 컨텍스트가 생성된다.



#### 함수 코드

함수 코드는 지역 스코프를 생성하고 지역 변수, 매개변수, arguments 객체를 관리해야 한다. 그리고 생성한 지역 스코프를 전역 스코프에서 시작하는 스코프 체인의 일원으로 연결해야 한다. 이를 위해 함수 코드가 평가되면 함수 실행 컨텍스트가 생성된다.



#### eval 코드

eval 코드는 strict mode(엄격 모드)에서 자신만의 독자적인 스코프를 생성한다. 이를 위해 eval 코드가 평가되면 eval 실행 컨텍스트가 생성된다.



#### 모듈 코드

모듈 코드는 모듈별로 독립적인 모듈 스코프를 생성한다. 이를 위해 모듈 코드가 평가되면 모듈 실행 컨텍스트가 생성된다.

<br><br>

## 2. 소스코드의 평가와 실행

자바스크립트 엔진은 소스코드를 2개의 과정으로 나누어 처리한다.

1. 소스코드 평가

   - 실행 컨텍스트 생성

   - 변수, 함수 등의 선언문 만을 먼저 실행
   - 생성된 변수나 함수 식별자를 실행 컨텍스트가 관리하는 스코프(렉시컬 환경의 환경 레코드)에 등록

2. 소스코드의 실행

   - 런타임 시작(선언문을 제외한 소스코드가 순차적으로 실행)

   - 변수나 함수의 참조를 실행 컨텍스트가 관리하는 스코프에서 검색 및 취득
   - 소스코드의 실행 결과는 스코드에 다시 등록

<img src="https://poiemaweb.com/assets/fs-images/23-2.png" alt="img" style="zoom:50%;" />

<br>

<br>

## 3. 실행 컨텍스트의 역할

**모든 코드는 실행 컨텍스트를 통해 실행되고 관리된다. 실행 컨텍스트(execution context)는 소스코드를 실행하는 데 필요한 환경을 제공하고 코드의 실행 결과를 실제로 관리하는 영역이다.**

1. 선언에 의해 생성된 모든 식별자(변수, 함수, 클래스 등)를 스코프를 구분하여 등록하고 상태 변화(식별자에 바인딩된 값의 변화)를 지속적으로 관리
2. 중첩된 스코프 내의 식별자 검색을 위한 스코프 체인 형성
3. 현재 실행 중인 코드의 실행 순서를 변경(예를 들어, 함수 호출에 의한 실행 순서 변경)할 수 있어야 하며 다시 되돌아갈 수도 있어야 함

식별자와 스코프는 실행 컨텍스트의 **렉시컬 환경**으로 관리하고 코드 실행 순서는 **실행 컨텍스트 스택**으로 관리한다.

<br>

<br>

## 4. 실행 컨텍스트 스택

```javascript
const x = 1;

function foo() {
  const y = 2;

  function bar() {
    const z = 3;
    console.log(x + y + z);
  }
  bar();
}

foo(); // 6
```

위 예제는 전역 코드와 함수 코드로 이루어져 있다. 자바스크립트 엔진은 먼저 전역 코드를 평가하여 전역 실행 컨텍스트를 생성한다. 그리고 함수가 호출되면 함수 코드를 평가하여 함수 실행 컨텍스트를 생성한다. 이때 **생성된 실행 컨텍스트는 스택(stack) 자료구조로 관리된다.**  이를 **실행 컨텍스트 스택(execution context stack)이라고 부른다.**

> 실행 컨텍스트 스택을 콜 스택(call stack)이라고 부르기도 한다.

위 코드를 실행하면 코드가 실행되는 시간의 흐름에 따라 실행 컨텍스트 스택에는 다음과 같이 실행 컨텍스트가 추가(push)되고 제거(pop)된다.

<img src="https://poiemaweb.com/assets/fs-images/23-5.png" alt="img" style="zoom:50%;" />

이처럼 **실행 컨텍스트 스택은 코드의 실행 순서를 관리한다.** 소스코드가 평가되면 실행 컨텍스트가 생성되고 실행 컨텍스트 스택의 최상위에 쌓인다. **실행 컨텍스트 스택의 최상위에 존재하는 실행 컨텍스트는 언제나 현재 실행 중인 코드의 실행 컨텍스트다.** 따라서 실행 컨텍스트 스택의 최상위에 존재하는 실행 컨텍스트를 **실행 중인 실행 컨텍스트(running execution context)**라 부른다.

<br>

<br>

## 5. 렉시컬 환경

렉시컬 환경(Lexical Environment)은 실행 컨텍스트를 구성하는 컴포넌트로 식별자와 식별자에 바인딩된 값, 그리고 상위 스코프에 대한 참조를 기록하는 자료구조이다. 즉, 렉시컬 환경은 스코프를 구분하여 식별자를 등록하고 관리하는 저장소 역할을 하는 렉시컬 스코프의 실체다.

실행 컨텍스트는 LexicalEnvironment 컴포넌트와 VariableEnvironment 컴포넌트로 구성된다. 생성 초기의 실행 컨텍스트와 렉시컬 환경을 그림으로 표현하면 아래와 같다.

<img src="https://poiemaweb.com/assets/fs-images/23-7.png" alt="img" style="zoom:50%;" />

생성 초기에 LexicalEnvironment 컴포넌트와 VariableEnvironment 컴포넌트는 하나의 동일한 렉시컬 환경을 참조한다. 이후 몇 가지 상황을 만나면 VariableEnvironment 컴포넌트를 위한 새로운 렉시컬 환경이 생성되고 각 컴포넌트별 참조가 달라지는 경우도 있다. 이 글에서는 컴포넌트를 구분하지 않고 렉시컬 환경으로 통일해 설명한다.

**렉시컬 환경**은 다음과 같이 **두 개의 컴포넌트로 구성**된다.

<img src="https://poiemaweb.com/assets/fs-images/23-8.png" alt="img" style="zoom:50%;" />

1. **환경 레코드(Environment Record)**

   - 스코프에 포함된 식별자를 등록하고 등록된 식별자에 바인딩된 값을 관리하는 저장소다. 
   - 환경 레코드는 소스코드의 타입에 따라 관리하는 내용에 차이가 있다.

2. **외부 렉시컬 환경에 대한 참조(Outer Lexical Environment Reference)**

   - 상위 스코프를 가리킨다. 여기서 상위 스코프란 해당 실행 컨텍스트를 생성한 소스코드를 포함하는 **상위 코드의 렉시컬 환경**을 말한다. 

   - 외부 렉시컬 환경에 대한 참조를 통해 단방향 링크드 리스트인 스코프 체인을 구현한다.

<br><br>

## 6. 실행 컨텍스트의 생성과 식별자 검색 과정

1. 전역 객체 생성

2. 전역 코드 평가

   2.1. 전역 실행 컨텍스트 생성

   2.2. 전역 렉시컬 환경 생성

   ​	a. 전역 환경 레코드 생성

   ​		a. 객체 환경 레코드 생성

   ​		b. 선언적 환경 레코드 생성

   ​		c. this 바인딩

   ​	b. 외부 렉시컬 환경에 대한 참조 결정

3. 전역 코드 실행

4. 함수 코드 평가

   4.1. 함수 실행 컨텍스트 생성

   4.2. 함수 렉시컬 환경 생성

   ​	a. 함수 환경 레코드 생성

   ​	b. 외부 렉시컬 환경에 대한 참조 결정

   ​	c. this 바인딩

5. 함수 코드 실행

6. 함수 코드 실행 종료

7. 전역 코드 실행 종료

<br>

아래의 예제를 통해 어떻게 실행 컨텍스트가 생성되고 코드 실행 결과가 관리되는지, 그리고 어떻게 실행 컨텍스트를 통해 식별자를 검색하는지 살펴보자.

```javascript
var x = 1;
const y = 2;

function foo(a) {
  var x = 3;
  const y = 4;

  function bar(b) {
    const z = 5;
    console.log(a + b + x + y + z);
  }
  bar(10);
}

foo(20); // 42
```

<br>

### 6.1. 전역 객체 생성

- 전역 객체는 전역 코드가 평가되기 이전에 생성된다.

- 이때 전역 객체에는 빌트인 전역 프로퍼티, 빌트인 전역 함수, 표준 빌트인 객체, 호스트 객체를 포함한다.

<br>

### 6.2. 전역 코드 평가

소스코드가 로드되면 자바스크립트 엔진은 전역 코드를 평가한다. 전역 코드 평가는 다음과 같은 순서로 진행된다.

```
1. 전역 실행 컨텍스트 생성
2. 전역 렉시컬 환경 생성
  	2.1. 전역 환경 레코드 생성
    		2.1.1. 객체 환경 레코드 생성
    		2.1.2. 선언적 환경 레코드 생성
  	2.2. this 바인딩
 	2.3. 외부 렉시컬 환경에 대한 참조 결정
```

전역 코드 평가 과정을 거쳐 생성된 전역 실행 컨텍스트와 렉시컬 환경은 다음과 같다.

<img src="https://poiemaweb.com/assets/fs-images/23-9.png" alt="img" style="zoom:50%;" />

<br>

#### 1. 전역 실행 컨텍스트 생성

- 빈 전역 실행 컨텍스트를 생성하여 실행 컨텍스트 스택에 push한다. 

- 이때 전역 실행 컨텍스트는 실행 중인 실행 컨텍스트(running execution context, 실행 컨텍스트 스택의 최상위)가 된다.

<img src="https://poiemaweb.com/assets/fs-images/23-10.png" alt="img" style="zoom:50%;" />

<br>

#### 2. 전역 렉시컬 환경 생성

전역 렉시컬 환경(Global Lexical Environment)을 생성하여 전역 실행 컨텍스트의 LexicalEnvironment 컴포넌트와 VariableEnvironment 컴포넌트에 바인딩한다.

<img src="https://poiemaweb.com/assets/fs-images/23-11.png" alt="img" style="zoom:50%;" />

전역 렉시컬 환경은 2개의 컴포넌트로 구성된다.

- 전역 환경 레코드(Global Environment Record)

- 외부 렉시컬 환경에 대한 참조(OuterLexicalEnvironmentReference)

<br>

##### 2.1. 전역 환경 레코드 생성

- 전역 렉시컬 환경을 구성하는 컴포넌트다.

- 전역 환경 레코드는 전역 변수를 관리하는 전역 스코프, 전역 객체의 빌트인 전역 프로퍼티와 빌트인 전역 함수, 표준 빌트인 객체를 제공한다.
- var 키워드로 선언한 전역 변수와 ES6의 let, const 키워드로 선언한 전역 변수를 구분하여 관리하기 위해 전역 스코프 역할을 한다.
  - 전역 환경 레코드의 구성(컴포넌트 2, 내부 슬롯 1)
    - 객체 환경 레코드(Object Environment Record)
    - 선언적 환경 레코드(Declarative Environment Record)

    - [[GlobalThisValue]] (This 바인딩)
- 객체 환경 레코드와 선언적 환경 레코드는 서로 협력하여 전역 스코프와 전역 객체를 관리한다.

##### 2.1.1. 객체 환경 레코드 생성

- 객체 환경 레코드는 이전 6.1.에서 생성된 BindingObject 객체와 연결된다.

- 전역 코드 평가 과정에서 var 키워드로 선언한 전역 변수와 함수 선언문으로 정의한 전역 함수는 BindingObject를 통해 전역 객체의 프로퍼티와 메서드가 된다. 그리고 이때 등록된 식별자를 객체 환경 레코드에서 검색하면 전역 객체의 프로퍼티를 검색하여 반환한다. 이것이 전역 객체를 가리키는 식별자 없이 전역 객체의 프로퍼티를 참조할 수 있는 메커니즘이다.

- 
