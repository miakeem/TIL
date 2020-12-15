# 프로토타입

## 1. 객체지향 프로그래밍

객체지향 프로그래밍(Object Oriented Programming, OOP)은 전통적인 명령형 프로그래밍(imperative programming)의 절차지향적 관점에서 벗어나 여러 개의 독립적 단위, 즉 객체(object)의 집합으로 프로그램을 표현하려는 프로그래밍 패러다임을 말한다.

객체지향 프로그래밍은 다양한 실체의 특징이나 성질을 나타내는 **속성(attribute/property)** 중에서 프로그램에 필요한 속성만 간추려 내어 표현한다. 이를 **추상화(abstraction)**라 한다.

```javascript
// 이름과 주소 속성을 갖는 객체
const person = {
  name: 'Kim',
  address: 'Seoul'
};

console.log(person); // {name: "Kim", address: "Seoul"}
```

프로그래머(subject, 주체)는 이름과 주소 속성으로 표현된 객체(object)인 person을 다른 객체와 구별하여 인식할 수 있다. 이처럼 **속성을 통해 여러 개의 값을 하나의 단위로 구성한 복합적인 자료구조**를 객체라 하며, 객체지향 프로그래밍은 독립적인 객체의 집합으로 프로그램을 표현하려는 프로그래밍 패러다임이다.

객체는 **상태(state) 데이터와 동작(behavior)을 하나의 논리적인 단위로 묶은 복합적인 자료구조**라고 할 수 있다. 이때 객체의 상태 데이터를 프로퍼티(property), 동작을 메서드(method)라 부른다.

<br><br>

##  2. 상속과 프로토타입

#### 상속(inheritance)이란?

어떤 객체의 프로퍼티 또는 메서드를 다른 객체가 상속받아 그대로 사용할 수 있는 것을 말한다.

자바스크립트는 프로토타입을 기반으로 상속을 구현하여 불필요한 중복을 제거한다. 중복을 제거하는 방법은 기존의 코드를 적극적으로 재사용하는 것이다.

```javascript
// 생성자 함수
function Circle(radius) {
  this.radius = radius;
  this.getArea = function () {
    // Math.PI는 원주율을 나타내는 상수다.
    return Math.PI * this.radius ** 2;
  };
}

// 반지름이 1인 인스턴스 생성
const circle1 = new Circle(1);
// 반지름이 2인 인스턴스 생성
const circle2 = new Circle(2);

// Circle 생성자 함수는 인스턴스를 생성할 때마다 동일한 동작을 하는
// getArea 메서드를 중복 생성하고 모든 인스턴스가 중복 소유한다.
// getArea 메서드는 하나만 생성하여 모든 인스턴스가 공유해서 사용하는 것이 바람직하다.
console.log(circle1.getArea === circle2.getArea); // false

console.log(circle1.getArea()); // 3.141592653589793
console.log(circle2.getArea()); // 12.566370614359172
```

<img src="https://poiemaweb.com/assets/fs-images/19-1.png" alt="img" style="zoom:50%;" />

 동일한 생성자 함수에 의해 생성된 모든 인스턴스가 동일한 메서드를 중복 소유하는 것은 메모리를 불필요하게 낭비하며 퍼포먼스에도 악영향을 준다. 

**자바스크립트는 프로토타입(prototype)을 기반으로 상속을 구현한다.**

```javascript
// 생성자 함수
function Circle(radius) {
  this.radius = radius;
}

// Circle 생성자 함수가 생성한 모든 인스턴스가 getArea 메서드를
// 공유해서 사용할 수 있도록 프로토타입에 추가한다.
// 프로토타입은 Circle 생성자 함수의 prototype 프로퍼티에 바인딩되어 있다.
Circle.prototype.getArea = function () {
  return Math.PI * this.radius ** 2;
};

// 인스턴스 생성
const circle1 = new Circle(1);
const circle2 = new Circle(2);

// Circle 생성자 함수가 생성한 모든 인스턴스는 부모 객체의 역할을 하는
// 프로토타입 Circle.prototype으로부터 getArea 메서드를 상속받는다.
// 즉, Circle 생성자 함수가 생성하는 모든 인스턴스는 하나의 getArea 메서드를 공유한다.
console.log(circle1.getArea === circle2.getArea); // true

console.log(circle1.getArea()); // 3.141592653589793
console.log(circle2.getArea()); // 12.566370614359172
```

<img src="https://poiemaweb.com/assets/fs-images/19-2.png" alt="img" style="zoom:50%;" />

- Circle 생성자 함수가 생성한 모든 인스턴스는 자신의 프로토타입, 즉 상위(부모) 객체 역할을 하는 Circle.prototype의 모든 프로퍼티와 메서드를 상속받는다.
- getArea 메서드는 단 하나만 생성되어 프로토타입인 Circle.prototype의 메서드로 할당되어 있다.
- Circle 생성자 함수는 자신의 상태를 나타내는 radius 프로퍼티만 개별적으로 소유하고 내용이 동일한 메서드는 상속을 통해 공유하여 사용한다.

**상속**은 **코드의 재사용**이란 관점에서 매우 유용하다. 생성자 함수가 생성할 모든 인스턴스가 공통적으로 사용할 프로퍼티나 메서드를 프로토타입에 미리 구현해 두면 **생성자 함수가 생성할 모든 인스턴스**는 별도의 구현 없이 **상위(부모) 객체인 프로토타입의 자산을 공유하여 사용**할 수 있다.

<br><br>

### 3. 프로토타입 객체

#### 프로토타입 객체(또는 줄여서 프로토타입)이란?

- 객체지향 프로그래밍의 객체 간 상속(inheritance)을 구현한다.

- **프로토타입**은 어떤 객체의 **상위(부모) 객체의 역할을 하는 객체**로서 다른 객체에 공유 프로퍼티(메서드 포함)를 제공한다. 

- 프로토타입을 상속받은 하위(자식) 객체는 상위 객체의 프로퍼티를 자신의 프로퍼티처럼 자유롭게 사용할 수 있다.

- 모든 객체는 [[Prototype]]이라는 내부 슬롯을 가지며 객체가 생성될 때 객체 생성 방식에 따라 프로토타입이 결정되고 [[Prototype]]에 저장된다(null인 경우도 있다).

- 모든 객체는 하나의 프로토타입을 갖는다.
  - [[Prototype]] 내부 슬롯의 값이 null인 객체는 프로토타입이 없다.

- 모든 프로토타입은 생성자 함수와 연결되어 있다. 즉, 객체와 프로토타입과 생성자 함수는 다음 그림과 같이 서로 연결되어 있다.

<img src="https://poiemaweb.com/assets/fs-images/19-3.png" alt="img" style="zoom:50%;" />

- 위 그림처럼 `__proto__` 접근자 프로퍼티를 통해 자신의 프로토타입, 즉 자신의 [[Prototype]] 내부 슬롯이 가리키는 프로토타입에 간접적으로 접근할 수 있다.

- 프로토타입은 자신의 constructor 프로퍼티를 통해 생성자 함수에 접근할 수 있고, 생성자 함수는 자신의 prototype 프로퍼티를 통해 프로토타입에 접근할 수 있다.

<br>

### 3.1. `__proto__` 접근자 프로퍼티

**모든 객체는 `__proto__` 접근자 프로퍼티를 통해 자신의 프로토타입, 즉 [[Prototype]] 내부 슬롯에 간접적으로 접근할 수 있다.** 

```javascript
const person = { name: 'Lee' };
```

<img src="https://poiemaweb.com/assets/fs-images/19-4.png" alt="img" style="zoom:50%;" />

- 빨간 박스 : person 객체의 프로토타입, Object.prototype

모든 객체는 `__proto__` **접근자 프로퍼티**를 통해 프로토타입을 가리키는 [[Prototype]] 내부 슬롯에 접근할 수 있다.

접근자 프로퍼티는 자체적으로는 값([[Value]] 프로퍼티 어트리뷰트)을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수(accessor function), 즉 [[Get]], [[Set]] 프로퍼티 어트리뷰트로 구성된 프로퍼티다.

<br>

### 3.2. 함수 객체의 prototype 프로퍼티

**함수 객체만이 소유하는 prototype 프로퍼티는 생성자 함수가 생성할 인스턴스의 프로토타입을 가리킨다.**

```javascript
// 함수 객체는 prototype 프로퍼티를 소유한다.
(function () {}).hasOwnProperty('prototype'); // -> true

// 일반 객체는 prototype 프로퍼티를 소유하지 않는다.
({}).hasOwnProperty('prototype'); // -> false
```

**prototype 프로퍼티**는 **생성자 함수가 생성할 객체(인스턴스)의 프로토타입**을 가리킨다. 

따라서 생성자 함수로서 호출할 수 없는 함수(non-constructor)인 화살표 함수와 ES6 메서드 축약 표현으로 정의한 메서드는 prototype 프로퍼티를 소유하지 않으며 프로토타입도 생성하지 않는다.

```javascript
// 화살표 함수는 non-constructor다.
const Person = name => {
  this.name = name;
};

// non-constructor는 prototype 프로퍼티를 소유하지 않는다.
console.log(Person.hasOwnProperty('prototype')); // false

// non-constructor는 프로토타입을 생성하지 않는다.
console.log(Person.prototype); // undefined

// ES6의 메서드 축약 표현으로 정의한 메서드는 non-constructor다.
const obj = {
  foo() {}
};

// non-constructor는 prototype 프로퍼티를 소유하지 않는다.
console.log(obj.foo.hasOwnProperty('prototype')); // false

// non-constructor는 프로토타입을 생성하지 않는다.
console.log(obj.foo.prototype); // undefined
```

**모든 객체가 가지고 있는(엄밀히 말하면 Object.prototype으로부터 상속받은) __proto__ 접근자 프로퍼티와 함수 객체만이 가지고 있는 prototype 프로퍼티는 결국 동일한 프로토타입을 가리킨다.** 하지만 이들 프로퍼티를 사용하는 주체가 다르다.

| 구분                      | 소유        | 값                | 사용 주체   | 사용 목적                                                    |
| :------------------------ | :---------- | :---------------- | :---------- | :----------------------------------------------------------- |
| __proto__ 접근자 프로퍼티 | 모든 객체   | 프로토타입의 참조 | 모든 객체   | 객체가 자신의 프로토타입에 접근 또는 교체하기 위해 사용      |
| prototype 프로퍼티        | constructor | 프로토타입의 참조 | 생성자 함수 | 생성자 함수가 자신이 생성할 객체(인스턴스)의 프로토타입을 할당하기 위해 사용 |



생성자 함수로 객체를 생성한 후 __proto__ 접근자 프로퍼티와 prototype 프로퍼티로 프로토타입 객체에 접근하면 결국 Person.prototype과 me.__proto__는 결국 동일한 프로토타입을 가리킨다.

```javascript
// 생성자 함수
function Person(name) {
  this.name = name;
}

const me = new Person('Lee');

console.log(Person.prototype === me.__proto__);  // true
```

<img src="https://poiemaweb.com/assets/fs-images/19-7.png" alt="img" style="zoom:50%;" />

<br>

### 3.3. 프로토타입의 constructor 프로퍼티와 생성자 함수

- 모든 프로토타입은 constructor 프로퍼티를 갖는다. 

- constructor 프로퍼티는 prototype 프로퍼티로 자신을 참조하고 있는 생성자 함수를 가리킨다.

- 이 연결은 생성자 함수가 생성될 때, 즉 함수 객체가 생성될 때 이뤄진다.

```javascript
// 생성자 함수
function Person(name) {
  this.name = name;
}

const me = new Person('Lee');

// me 객체의 생성자 함수는 Person이다.
console.log(me.constructor === Person);  // true
```

<img src="https://poiemaweb.com/assets/fs-images/19-8.png" alt="img" style="zoom:50%;" />

<br><br>

## 4. 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입

생성자 함수에 의해 생성된 인스턴스의 프로토타입은 constructor 프로퍼티에 의해 생성자 함수와 연결되고 이때 constructor 프로퍼티는 인스턴스를 생성한 생성자 함수를 가리킨다. 

```javascript
// obj 객체를 생성한 생성자 함수는 Object다.
const obj = new Object();
console.log(obj.constructor === Object); // true
```



리터럴 표기법에 의한 객체 생성 방식과 같이 명시적으로 new 연산자와 함께 생성자 함수를 호출하여 인스턴스를 생성하지 않는 객체 생성 방식도 있다.

```javascript
// 객체 리터럴
const obj = {};

// 함수 리터럴
const add = function (a, b) { return a + b; };

// 배열 리터럴
const arr = [1, 2, 3];

// 정규표현식 리터럴
const regexp = /is/ig;
```

리터럴 표기법에 의해 생성된 객체도 프로토타입이 존재하지만 프로토타입의 constructor 프로퍼티가 가리키는 생성자 함수가 반드시 객체를 생성한 생성자 함수는 아니다.

```javascript
// obj 객체는 Object 생성자 함수로 생성한 객체가 아니라 객체 리터럴로 생성했다.
const obj = {};

// 하지만 obj 객체의 생성자 함수는 Object 생성자 함수다.
console.log(obj.constructor === Object); // true
```

obj 객체는 Object 생성자 함수로 생성한 객체가 아니 객체 리터럴로 생성된 객체이나 obj 객체는 Object 생성자 함수와 constructor 프로퍼티로 연결되어 있다. 

ECMAScript 사양에 따르면 Object 생성자 함수는 다음과 같이 구현하도록 정의되어 있다.

![img](https://poiemaweb.com/assets/fs-images/19-9.png)

2번에서 Object 생성자 함수에 인수를 전달하지 않거나 undefined 또는 null을 인수로 전달하면서 new 연산자와 함께 호출하면 내부적으로는 추상 연산 OrdinaryObjectCreate를 호출하여 Object.prototype을 프로토타입으로 갖는 빈 객체를 생성한다.

> 추상 연산(abstract operation)
>
> 추상 연산은 ECMAScript 사양에서 내부 동작의 구현 알고리즘을 표현한 것이다. ECMAScript 사양에서 설명을 위해 사용되는 함수와 유사한 의사 코드라고 이해하자.

```javascript
// 2. Object 생성자 함수에 의한 객체 생성
// Object 생성자 함수는 new 연산자와 함께 호출하지 않아도 new 연산자와 함께 호출한 것과 동일하게 동작한다.
// 인수가 전달되지 않았을 때 추상 연산 OrdinaryObjectCreate를 호출하여 빈 객체를 생성한다.
let obj = new Object();
console.log(obj); // {}
```



참고로 1은 class Foo extends Object {}와 같이 Object 생성자 함수를 확장한 클래스를 호출하는 경우이고, 3은 new 없이 Object 생성자 함수를 호출하는 경우다.

```javascript
// 1. new.target이 undefined나 Object가 아닌 경우
// 인스턴스 -> Foo.prototype -> Object.prototype 순으로 프로토타입 체인이 생성된다.
class Foo extends Object {}
new Foo(); // Foo {}

// 3. 인수가 전달된 경우에는 인수를 객체로 변환한다.
// Number 객체 생성
obj = new Object(123);
console.log(obj); // Number {123}
```



객체 리터럴이 평가될 때는 다음과 같이 추상 연산 OrdinaryObjectCreate를 호출하여 빈 객체를 생성하고 프로퍼티를 추가하도록 정의되어 있다.

![img](https://poiemaweb.com/assets/fs-images/19-10.png)

이와 같이 **Object 생성자 함수 호출과 객체 리터럴의 평가**는 **추상 연산 OrdinaryObjectCreate를 호출하여 빈 객체를 생성하는 점에서 동일**하나 new.target의 확인이나 프로퍼티를 추가하는 처리 등 **세부 내용은 다르다**. 따라서 객체 리터럴에 의해 생성된 객체는 Object 생성자 함수가 생성한 객체가 아니다.

함수 객체의 경우 Function 생성자 함수를 호출하여 생성한 함수는 렉시컬 스코프를 만들지 않고 전역 함수인 것처럼 스코프를 생성하며 클로저도 만들지 않는다. 함수 선언문과 함수 표현식을 평가하여 함수 객체를 생성한 것은 Function 생성자 함수가 아니다. 

```javascript
// foo 함수는 Function 생성자 함수로 생성한 함수 객체가 아니라 함수 선언문으로 생성했다.
function foo() {}

// 하지만 constructor 프로퍼티를 통해 확인해보면 함수 foo의 생성자 함수는 Function 생성자 함수다.
console.log(foo.constructor === Function); // true
```



- 리터럴 표기법에 의해 생성된 객체도 상속을 위해 프로토타입이 필요하다. 

- 리터럴 표기법에 의해 생성된 객체도 가상적인 생성자 함수를 갖는다.

- 프로토타입은 constructor 프로퍼티에 의해 연결되어 있기 때문에 생성자 함수와 더불어 생성된다.

- **프로토타입과 생성자 함수는 단독으로 존재할 수 없고 언제나 쌍(pair)으로 존재한다.**

| 리터럴 표기법      | 생성자 함수 | 프로토타입         |
| :----------------- | :---------- | :----------------- |
| 객체 리터럴        | Object      | Object.protptype   |
| 함수 리터럴        | Function    | Function.prototype |
| 배열 리터럴        | Array       | Array.prototype    |
| 정규 표현식 리터럴 | RegExp      | RegExp.protptype   |

<br>

<br>

## 5. 프로토타입의 생성 시점

**프로토타입은 생성자 함수가 생성되는 시점에 더불어 생성된다.** 프로토타입과 생성자 함수는 단독으로 존재할 수 없고 언제나 쌍으로 존재하기 때문이다.

생성자 함수는 사용자가 직접 정의한 사용자 정의 생성자 함수와 자바스크립트가 기본 제공하는 빌트인 생성자 함수로 구분할 수 있다.

<br>

### 5.1. 사용자 정의 생성자 함수와 프로토타입 생성 시점

내부 메서드 [[Construct]]를 갖는 함수 객체(일반 함수(함수 선언문, 함수 표현식)로 정의한 함수 객체)는 new 연산자와 함께 생성자 함수로서 호출할 수 있다.

**constructor는 함수(생성자 함수로서 호출 가능) 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다.**

```javascript
// 함수 정의(constructor)가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다.
console.log(Person.prototype); // {constructor: ƒ}

// 생성자 함수
function Person(name) {
  this.name = name;
}
```

non-constructor는 프로토타입이 생성되지 않는다.

```javascript
// 화살표 함수는 non-constructor다.
const Person = name => {
  this.name = name;
};

// non-constructor는 프로토타입이 생성되지 않는다.
console.log(Person.prototype); // undefined
```

함수 선언문은 다른 코드가 실행되기 이전에 자바스크립트 엔진에 의해 먼저 실행되므로 함수 선언문으로 정의된 Person 생성자 함수는 어떤 코드보다 먼저 평가되어 함수 객체가 된다. 이때 프로토타입도 더불어 생성된다.

생성된 프로토타입은 오직 constructor 프로퍼티만을 갖는 객체다. 프로토타입도 객체이고 모든 객체는 프로토타입을 가지므로 프로토타입도 자신의 프로토타입을 갖는다. 생성된 프로토타입의 프로토타입은 Object.prototype이다.

<img src="https://poiemaweb.com/assets/fs-images/19-12.png" alt="img" style="zoom:50%;" />

<br>

### 5.2. 빌트인 생성자 함수와 프로토타입 생성 시점

- 빌트인 생성자 함수(Object, String, Number 등)도 일반 함수와 마찬가지로 빌트인 생성자 함수가 생성되는 시점에 프로토타입이 생성된다. 

- 모든 빌트인 생성자 함수는 전역 객체가 생성되는 시점에 생성된다. 

- 생성된 프로토타입은 빌트인 생성자 함수의 prototype 프로퍼티에 바인딩된다.

<img src="https://poiemaweb.com/assets/fs-images/19-13.png" alt="img" style="zoom:50%;" />

> 전역 객체(global object)
>
> 전역 객체는 코드가 실행되기 이전 단계에 자바스크립트 엔진에 의해 생성되는 특수한 객체이다. 전역 객체는 클라이언트 사이드 환경(브라우저)에서는 window, 서버 사이드 환경(Node.js)에서는 global 객체를 의미한다.
> 전역 객체는 표준 빌트인 객체(Object, String, Number, Function, Array 등)들과 환경에 따른 호스트 객체(클라이언트 web API 또는 Node.js의 호스트 API), 그리고 var 키워드로 선언한 전역 변수와 전역 함수를 프로퍼티로 갖는다. Math, Reflect, JSON을 제외한 표준 빌트인 객체는 모두 생성자 함수이다.

```javascript
// 전역 객체 window는 브라우저에 종속적이므로 아래 코드는 브라우저 환경에서 실행해야 한다.
// 빌트인 객체인 Object는 전역 객체 window의 프로퍼티다.
window.Object === Object // true
```

<br>

<br>

## 6. 객체 생성 방식과 프로토타입의 결정

객체는 다음과 같이 다양한 생성 방법이 있다.

- 객체 리터럴
- Object 생성자 함수
- 생성자 함수
- Object.create 메서드
- 클래스 (ES6)

공통점 : 추상 연산 OrdinaryObjectCreate에 의해 생성된다.

<br>

### 6.1. 객체 리터럴에 의해 생성된 객체의 프로토타입

자바스크립트 엔진은 추상 연산 OrdinaryObjectCreate를 호출하여 객체 리터럴을 평가하여 객체를 생성한다. 추상 연산 OrdinaryObjectCreate에 전달되는 프로토타입은 Object.prototype으로 다시 말해,  객체 리터럴에 의해 생성되는 객체의 프로토타입은 Object.prototype이다.



```javascript
const obj = { x: 1 };
```

<img src="https://poiemaweb.com/assets/fs-images/19-14.png" alt="img" style="zoom:50%;" />

- 객체 리터럴에 의해 생성된 obj 객체는 Object.prototype을 프로토타입으로 갖게 되며, 이로써 Object.prototype을 상속받는다.
- **obj 객체**는 constructor 프로퍼티와 hasOwnProperty 메서드 등을 소유하지 않지만 자신의 프로토타입인 **Object.prototype의 constructor 프로퍼티와 hasOwnProperty 메서드**를 자신의 자산인 것처럼 **자유롭게 사용**할 수 있다.
- obj 객체가 자신의 프로토타입인 Object.prototype 객체를 상속받았기 때문이다.

```javascript
const obj = { x: 1 };

// 객체 리터럴에 의해 생성된 obj 객체는 Object.prototype을 상속받는다.
console.log(obj.constructor === Object); // true
console.log(obj.hasOwnProperty('x'));    // true
```

<br>

### 6.2. Object 생성자 함수에 의해 생성된 객체의 프로토타입

- Object 생성자 함수를 인수 없이 호출하면 빈 객체가 생성된다.
- Object 생성자 함수를 호출하면 추상 연산 OrdinaryObjectCreate가 호출 된다.
- 이때 추상 연산 OrdinaryObjectCreate에 전달되는 프로토타입은 Object.prototype이다.
- Object 생성자 함수에 의해 생성된 obj 객체는 Object.prototype을 프로토타입으로 갖게 되며, 이로써 Object.prototype을 상속받는다.

```javascript
const obj = new Object();
obj.x = 1;
```

<img src="https://poiemaweb.com/assets/fs-images/19-15.png" alt="img" style="zoom:50%;" />



- 객체 생성 방식의 차이
  - 객체 리터럴 : 객체 리터럴 내부에 프로퍼티를 추가한다.
  - Object 생성자 함수 : 빈 객체를 생성한 이후 프로퍼티를 푸가해야 한다.

<br>

### 6.3. 생성자 함수에 의해 생성된 객체의 프로토타입

- new 연산자와 함께 생성자 함수를 호출하여 인스턴스를 생성하면 추상 연산 OrdinaryObjectCreate가 호출 된다.
- 추상 연산 OrdinaryObjectCreate에 전달되는 프로토타입은 생성자 함수의 prototype 프로퍼티에 바인딩되어 있는 객체다.

- 즉, 생성자 함수에 의해 생성되는 객체의 프로토타입은 생성자 함수의 prototype 프로퍼티에 바인딩되어 있는 객체이다.

```javascript
function Person(name) {
  this.name = name;
}

const me = new Person('Lee');
```

<img src="https://poiemaweb.com/assets/fs-images/19-16.png" alt="img" style="zoom:50%;" />

Object.prototype은 다양한 빌트인 메서드(hasOwnProperty, propertyIsEnumerable 등)를 갖고있지만 사용자 정의 생성자 함수 Person과 더불어 생성된 프로토타입 Person.prototype의 프로퍼티는 constructor 뿐이다.

프로토타입은 객체이므로 일반 객체와 같이 프로토타입에도 프로퍼티를 추가/삭제할 수 있다. 또 추가/삭제된 프로퍼티는 프로토타입 체인에 즉각 반영된다.

```javascript
function Person(name) {
  this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function () {
  console.log(`Hi! My name is ${this.name}`);
};

const me = new Person('Lee');
const you = new Person('Kim');

me.sayHello();  // Hi! My name is Lee
you.sayHello(); // Hi! My name is Kim
```

Person 생성자 함수를 통해 생성된 모든 객체는 프로토타입에 추가된 sayHello 메서드를 상속받아 자신의 메서드처럼 사용할 수 있다.

<img src="https://poiemaweb.com/assets/fs-images/19-17.png" alt="img" style="zoom:50%;" />

<br>

<br>

## 7. 프로토타입 체인

```javascript
function Person(name) {
  this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function () {
  console.log(`Hi! My name is ${this.name}`);
};

const me = new Person('Lee');

// hasOwnProperty는 Object.prototype의 메서드다.
console.log(me.hasOwnProperty('name')); // true
```

- me 객체(Person 생성자 함수에 의해 생성)는 Object.prototype의 메서드인 hasOwnProperty를 호출할 수 있다.

- me 객체가 Person.prototype 뿐만 아니라 Object.prototype도 상속받았다는 것을 의미이다.

- me 객체의 프로토타입은 Person.prototype이다.

- Person.prototype의 프로토타입은 Object.prototype이다. 프로토타입의 프로토타입은 언제나 Object.prototype이다.

<img src="https://poiemaweb.com/assets/fs-images/19-18.png" alt="img" style="zoom:50%;" />

#### 프로토타입 체인

**자바스크립트는 객체의 프로퍼티(메서드 포함)에 접근하려고 할 때 해당 객체에 접근하려는 프로퍼티가 없다면 [[Prototype]] 내부 슬롯의 참조를 따라 자신의 부모 역할을 하는 프로토타입의 프로퍼티를 순차적으로 검색한다. 이를 프로토타입 체인이라 한다. 프로토타입 체인은 자바스크립트가 객체지향 프로그래밍의 상속을 구현하는 메커니즘이다.**

```javascript
// hasOwnProperty는 Object.prototype의 메서드다.
// me 객체는 프로토타입 체인을 따라 hasOwnProperty 메서드를 검색하여 사용한다.
me.hasOwnProperty('name'); // -> true
```

`me.hasOwnProperty('name')`과 같이 메서드를 호출하면 자바스크립트 엔진은 다음과 같은 과정을 거쳐 메서드를 검색한다. 물론 프로퍼티를 참조하는 경우도 마찬가지다.

1. 먼저 hasOwnProperty 메서드를 호출한 me 객체에서 hasOwnProperty 메서드를 검색한다. me 객체에는 hasOwnProperty 메서드가 없으므로 프로토타입 체인을 따라, 다시 말해 [[Prototype]] 내부 슬롯에 바인딩되어 있는 프로토타입(위 예제의 경우 Person.prototype)으로 이동하여 hasOwnProperty 메서드를 검색한다.
2. Person.prototype에도 hasOwnProperty 메서드가 없으므로 프로토타입 체인을 따라, 다시 말해 [[Prototype]] 내부 슬롯에 바인딩되어 있는 프로토타입(위 예제의 경우 Object.prototype)으로 이동하여 hasOwnProperty 메서드를 검색한다.
3. Object.prototype에는 hasOwnProperty 메서드가 존재한다. 자바스크립트 엔진은 Object.prototype.hasOwnProperty 메서드를 호출한다. 이때 Object.prototype.hasOwnProperty 메서드의 this에는 me 객체가 바인딩된다.

```javascript
Object.prototype.hasOwnProperty.call(me, 'name');
```

- 프로토타입 체인의 최상위에 위치하는 객체는 언제나 Object.prototype이므로 모든 객체는 Object.prototype을 상속받는다.
- **Object.prototype을 프로토타입 체인의 종점(end of prototype chain)**이라 한다.
- Object.prototype의 프로토타입, 즉 [[Prototype]] 내부 슬롯의 값은 null이다.
- 프로토타입 체인의 종점인 Object.prototype에서도 프로퍼티를 검색할 수 없는 경우, undefined를 반환한다.(에러 발생X)

```javascript
console.log(me.foo); // undefined
```

- 자바스크립트 엔진은 객체 간의 상속 관계로 이루어진 프로토타입의 계층적인 구조에서 객체의 프로퍼티를 검색한다. 
- **프로토타입 체인은 상속과 프로퍼티 검색을 위한 메커니즘**이라고 할 수 있다

- 반면, 자바스크립트 엔진은 함수의 중첩 관계로 이루어진 스코프의 계층적 구조에서 식별자를 검색한다. 
- **스코프 체인은 식별자 검색을 위한 메커니즘**이라고 할 수 있다.

<br>

```javascript
me.hasOwnProperty('name');
```

1. 먼저 스코프 체인에서 me 식별자를 검색한다. 
2. me 식별자는 전역에서 선언되었으므로 전역 스코프에서 검색된다. 
3. me 식별자를 검색한 다음, me 객체의 프로토타입 체인에서 hasOwnProperty 메서드를 검색한다.

이처럼 **스코프 체인과 프로토타입 체인은 서로 연관없이 별도로 동작하는 것이 아니라 서로 협력하여 식별자와 프로퍼티를 검색하는 데 사용된다.**

<br>

<br>

## 8. 오버라이딩과 프로퍼티 섀도잉

```javascript
const Person = (function () {
  // 생성자 함수
  function Person(name) {
    this.name = name;
  }

  // 프로토타입 메서드
  Person.prototype.sayHello = function () {
    console.log(`Hi! My name is ${this.name}`);
  };

  // 생성자 함수를 반환
  return Person;
}());

const me = new Person('Lee');

// 인스턴스 메서드
me.sayHello = function () {
  console.log(`Hey! My name is ${this.name}`);
};

// 인스턴스 메서드가 호출된다. 프로토타입 메서드는 인스턴스 메서드에 의해 가려진다.
me.sayHello(); // Hey! My name is Lee
```

생성자 함수로 객체(인스턴스)를 생성한 다음, 인스턴스에 메서드를 추가했다. 이를 그림으로 나타내면 다음과 같다.

<img src="https://poiemaweb.com/assets/fs-images/19-19.png" alt="img" style="zoom:50%;" />

- 프로토타입이 소유한 프로퍼티(메서드 포함) : 프로토타입 프로퍼티

- 인스턴스가 소유한 프로퍼티 : 인스턴스 프로퍼티

<br>

#### 프로퍼티 섀도잉(property shadowing)

상속 관계에 의해 프로퍼티가 가려지는 현상을 말한다.

위의 예제를 보면 프로토타입 프로퍼티와 같은 이름의 프로퍼티를 인스턴스에 추가하면 프로토타입 체인을 따라 프로토타입 프로퍼티를 검색하여 프로토타입 프로퍼티를 덮어쓰는 것이 아니라 인스턴스 프로퍼티로 추가한다. 이때 인스턴스 메서드 sayHello는 프로토타입 메서드 sayHello를 오버라이딩했고 프로토타입 메서드 sayHello는 가려진다. 

> 오버라이딩(overriding)
>
> 상위 클래스가 가지고 있는 메서드를 하위 클래스가 재정의하여 사용하는 방식이다.
>
> 오버로딩(overloading)
>
> 함수의 이름은 동일하지만 매개변수의 타입 또는 개수가 다른 메서드를 구현하고 매개변수에 의해 메서드를 구별하여 호출하는 방식이다. 자바스크립트는 오버로딩을 지원하지 않지만 arguments 객체를 사용하여 구현할 수는 있다.

<br>

프로퍼티를 삭제하는 경우.  위 예제에서 추가한 인스턴스 메서드 sayHello를 삭제해보자.

```javascript
// 인스턴스 메서드를 삭제한다.
delete me.sayHello;
// 인스턴스에는 sayHello 메서드가 없으므로 프로토타입 메서드가 호출된다.
me.sayHello(); // Hi! My name is Lee
```

당연히 프로토타입 메서드가 아닌 인스턴스 메서드 sayHello가 삭제된다. 다시 한번 sayHello 메서드를 삭제하여 프로토타입 메서드의 삭제를 시도해보자.

```javascript
// 프로토타입 체인을 통해 프로토타입 메서드가 삭제되지 않는다.
delete me.sayHello;
// 프로토타입 메서드가 호출된다.
me.sayHello(); // Hi! My name is Lee
```

- 하위 객체를 통해 프로토타입의 프로퍼티를 변경 또는 삭제하는 것은 불가능하다. 

  다시 말해, 하위 객체를 통해 프로토타입에 get 액세스는 허용되나 set 액세스는 허용되지 않는다.

<br>

프로토타입 프로퍼티를 변경 또는 삭제하려면 하위 객체를 통해 프로토타입 체인으로 접근하는 것이 아니라 프로토타입에 직접 접근해야 한다.

```javascript
// 프로토타입 메서드 변경
Person.prototype.sayHello = function () {
  console.log(`Hey! My name is ${this.name}`);
};
me.sayHello(); // Hey! My name is Lee

// 프로토타입 메서드 삭제
delete Person.prototype.sayHello;
me.sayHello(); // TypeError: me.sayHello is not a function
```