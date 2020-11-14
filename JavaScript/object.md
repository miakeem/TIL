# 객체 리터럴

# 1. 객체란?

- 객체 기반의 프로그래밍 언어인 자바스크립트를 구성하는 거의 "모든 것"
- 원시값을 제외한 나머지 값들(함수, 배열, 정규표현식 등)

- 다양한 타입의 값(원시값 또는 다른 객체)을 하나의 단위로 구성한 복합적인 자료구조(data structure)
- 변경 가능한 값(mutable value)

- 0개 이상의 프로퍼티로 구성된 집합
- 키(key)와 값(value)로 구성

<img src="https://poiemaweb.com/assets/fs-images/10-1.png" alt="img" style="zoom:50%;" />



자바스크립트에서 사용할 수 있는 모든 값은 프로퍼티 값이 될 수 있으며 일급객체인 함수 또한 마찬가지로 프로퍼티 값으로 사용 가능하다. 프로퍼티 값이 함수일 경우 일반 함수와 구분하기 위해 메서드(method)라 부른다.

<br>

#### 프로퍼티와 메서드의 역할

프로퍼티 : 객체의 상태를 나타내는 값(data)

메서드 : 프로퍼티(상태 데이터)를 참조하고 조작할 수 있는 동작(behavior)

이처럼 객체는 프로퍼티와 메서드로 구성된 집합체이며 상태와 동작을 하나의 단위로 구조화할 수 있어 유용하다.

<br>

#### 객체지향 프로그래밍

객체들의 집합으로 프로그램을 표현하려는 프로그래밍 패러다임

<br><br>

# 2. 객체 리터럴에 의한 객체 생성

자바스크립트는 **프로토타입 기반 객체지향 언어**로서 객체 생성 방법을 지원한다.

- **객체 리터럴**(가장 일반적이고 간단한 객체 생성 방법)
- Object 생성자 함수
- 생성자 함수
- Object.create 메서드
- 클래스 (ES6)

<br>

#### 객체 리터럴

객체 리터럴은 객체를 생성하기 위한 표기법이다.

> 리터럴(literal)
>
> 리터럴(literal)은 사람이 이해할 수 있는 문자 또는 약속된 기호를 사용하여 값을 생성하는 표기법을 말한다.

객체 리터럴은 중괄호({…}) 내에 0개 이상의 프로퍼티를 정의한다. 변수에 할당이 이루어지는 시점에 자바스크립트 엔진은 객체 리터럴을 해석해 객체를 생성한다.

```javascript
var person = {
    name: 'Kim',
    sayHello: function() {
        console.log(`Hello! My name is ${this.name}.`);
    }
};

console.log(typeof person); // object
console.log(person); // {name: "Lee", sayHello: f}
```

중괄 호 내, 프로퍼티를 정의하지 않으면 빈 객체가 생성된다.

```javascript
var empty = {}; // 빈 객체
console.log(typeof empty); // object
```

<br>

객체 리터럴은 값으로 평가되는 표현식이며 이의 중괄호는 코드 블록을 의미하지 않으므로 객체 리터럴의 닫는 중괄호 뒤에는 세미콜론을 붙인다. 

객체 리터럴은 클래스나 new 연산자와 함께 생성자를 호출할 필요 없이 리터럴로 객체를 생성한다. 객체 리터럴에 프로퍼티를 포함시켜 객체를 생성함과 동시에 프로퍼티를 만들 수도 있고, 객체를 생성한 이후에 프로퍼티를 동적으로 추가할 수도 있다.

<br><br>

# 3. 프로퍼티

객체는 프로퍼티(property)들의 집합이며 프로퍼티 키(key)는 값(value)으로 구성된다. 프로퍼티를 나열할 때는 쉼표(,)로 구분한다. (마지막 프로퍼티 뒤에는 사용하지 않으나 사용해도 좋음)

```javascript
var person = {
    name: 'Kim',
    age: 35
};
```



**프로퍼티 키와 프로퍼티 값으로 사용할 수 있는 값**

- 프로퍼티 키 : 빈 문자열을 포함하는 모든 문자열 또는 심벌 값

  ​						(일반적으로 문자열 사용)

- 프로퍼티 값 : 자바스크립트에서 사용할 수 있는 모든 값



프로퍼티 키는 프로퍼티 값에 접근할 수 있는 이름으로 식별자 역할을 한다. 하지만 반드시 식별자 네이밍 규칙을 따라야 하는 것은 아니나 네이밍 규칙을 준수하는 프로퍼티 키와 그렇지 않은 프로퍼티 키는 차이가 있다. 

일반적으로 문자열을 프로퍼티 키로 사용하며 따옴표('...' 또는 "...")로 묶어야 한다. 하지만 식별자 네이킹 규칙을 준수하는 이름, 즉 자바스크립트에서 사용 가능한 유효한 이름인 경우 따옴표를 생략할 수 있다.

<br>

#### 프로퍼티 키의 동적 생성

문자열 또는 문자열로 평가할 수 있는 표현식을 사용해 프로퍼티 키를 동적으로 생성할 수 도 있다. 이 경우에는 프로퍼티 키로 사용할 표현식을 대괄호([...])로 묶어야 한다.

```javascript
var obj = {};
var key = 'hello';

// ES5 : 프로퍼티 키 동적 생성
obj[key] = 'world';
// ES6: 계산된 프로퍼티 이름
// var obj = { [key]: 'world' };

console.log(obj); // {hello: "world"}
```

<br>

#### 프로퍼티 키로 빈 문자열 사용

빈 문자열을 프로퍼티 키로 사용해도 에러가 발생하지 않는다. 하지만 키로서의 의미를 갖지 못하므로 권장하지 않는다.

```javascript
var foo = {
    '': '' // 빈 문자열이나 프로퍼티 키로 사용 가능
};

console.log(foo); // {"": ""}
```

<br>

#### 문자열, 심벌 값 이외의 값 프로퍼티 키에 사용

프로퍼티 키에 문자열이나 심벌 값 이외의 값을 사용하면 암묵적 타입 변환을 통해 문자열이 된다. 예를 들어, 프로퍼티 키로 숫자 리터럴을 사용하면 따옴표는 붙지 않지만 내부적으로는 문자열로 변환된다.

```javascript
var foo = {
  0: 1,
  1: 2,
  2: 3
};

console.log(foo); // {0: 1, 1: 2, 2: 3}
```

<br>

#### 프로퍼티 키로 예약어 사용

예약어를 프로퍼티 키로 사용해도 에러가 발생하지 않는다. 하지만 예상치 못한 에러가 발생할 여지가 있으므로 권장하지 않는다.

```javascript
var foo = {
  var: '',
  function: ''
};

console.log(foo); // {var: "", function: ""}
```

<br>

#### 프로퍼티 키의 중복 선언

이미 존재하는 프로퍼티 키를 중복 선언하면 나중에 선언한 프로퍼티가 먼저 선언한 프로퍼티를 덮어쓴다. 이때 에러가 발생하지 않는다.

```javascript
var foo = {
  name: 'Lee',
  name: 'Kim'
};

console.log(foo); // {name: "Kim"}
```

<br><br>

# 4. 메서드

자바스크립트의 함수는 객체(일급 객체(first-class object))다. 따라서 함수는 값으로 취급할 수 있기 때문에 프로퍼티 값으로 사용할 수 있다.

### 메서드란?

객체에 묶여있는 함수를 의미한다. 프로퍼티 값이 함수일 경우 일반 함수와 구분하기 위해 메서드(method)라 부른다. 

```javascript
var circle = {
    radius: 5, // <- 프로퍼티
    
    getDiameter: function () { // <- 메서드
        return 2 * this.radius; // this는 circle을 가리킨다.
    }
};

console.log(circle.getDiameter()); // 10
```

메서드 내부에서 사용한 this 키워드는 객체 자신을 가리키는 **참조변수**이다.

<br><br>

## 5. 프로퍼티 접근

### 프로퍼티에 접근하는 방법

- **마침표 표기법(dot notation)**
  - 마침표 프로퍼티 접근 연산자(.)를 사용
- **대괄호 표기법(bracket notation)**
  - 대괄호 프로퍼티 접근 연산자([...])를 사용



프로퍼티 키가 식별자 네이밍 규칙을 준수하는 이름, 즉 **자바스크립트에서 사용 가능한 유효한 이름**이면 **마침표 표기법과 대괄호 표기법을 모두 사용**할 수 있다.

1. 마침표 프로퍼티 접근 연산자 또는 대괄호 프로퍼티 접근 연산자의 좌측에는 객체로 평가되는 표현식을 기술하고 마침표 프로퍼티 접근 연산자의 우측 또는 대괄호 프로퍼티 접근 연산자의 내부에는 프로퍼티 키를 지정한다.

2. 대괄호 표기법을 사용하는 경우 **대괄호 프로퍼티 접근 연산자 내부에 지정하는 프로퍼티 키는 반드시 따옴표로 감싼 문자열이어야 한다.** 

3. 대괄호 프로퍼티 접근 연산자 내에 따옴표로 감싸지 않은 이름을 프로퍼티 키로 사용하면 자바스크립트 엔진은 식별자로 해석한다.
4. **객체에 존재하지 않는 프로퍼티에 접근하면 undefined를 반환한다.** 이때 ReferenceError가 발생하지 않는 데 주의하자.

```javascript
var person = {
    name: 'Kim'
};

// 1.
console.log(person.name); // Kim
// 2.
console.log(person['name']) // Kim
// 3.
console.log(person[name]); // ReferenceError: name is not defined
// 4.
console.log(person.age); // undefined
```

<br>

프로퍼티 키가 식별자 네이밍 규칙을 준수하지 않는 이름, 즉 **자바스크립트에서 사용 가능한 유효한 이름이 아니면 대괄호 표기법을 사용**해야 한다.

단, 프로퍼티 키가 숫자로 이뤄진 문자열인 경우는 따옴표를 생략할 수 있다.

<br><br>

# 6. 프로퍼티 값 갱신

이미 존재하는 프로퍼티의 값에 할당하면 프로퍼티 값이 갱신된다.

```javascript
var person = {
    name: 'Kim'
};

person.name = 'Min';

console.log(person) // {name: "Min"}
```

<br><br>

# 7. 프로퍼티 동적 생성

존재하지 않는 프로퍼티에 값을 할당하면 프로퍼티가 동적으로 생성되어 추가되고 프로퍼티 값이 할당된다.

```javascript
var person = {
    name: 'Kim'
};

person.age = 20;

console.log(person); // {name: "Kim", age: 20}
```

<br><br>

# 8. 프로퍼티 삭제

### delete

프로퍼티 값에 접근할 수 있는 표현식인 피연산자에 delete 연산자를 사용하면 객체의 프로퍼티를 삭제한다. 그리고 존재하지 않는 프로퍼티를 삭제하면 아무런 에러 없이 무시된다.

```javascript
var person = {
    name: 'Kim'
};

person.age = 20;

delete person.age;

delete person.address;

console.log(person); // {name: "Kim"}
```

<br><br>

# 9. ES6에서 추가된 객체 리터럴의 확장 기능

## 9.1. 프로퍼티 축약 표현

프로퍼티는 프로퍼티 키와 프로퍼티 값으로 구성된다. 해당 프로퍼티 값은 변수에 할당된 값이므로 이는 식별자 표현식일 수도 있다.

```javascript
// ES5
var x = 1, y = 2;

var obj = {
    x: x,
    y: y
};

console.log(obj) // {x: 1, y: 2}
```

<br>

### ES6, 프로퍼티 키의 생략

프로퍼티 값으로 변수를 사용하는 경우 변수 이름과 프로퍼티 키가 동일한 이름일 때, 프로퍼티 키를 생략(property shorthand)할 수 있다. 이때 프로퍼티 키는 변수 이름으로 자동 생성된다.

```javascript
// ES6
let x = 1, y = 2;

// 프로퍼티 축약 표현
const obj = { x, y };

console.log(obj); // {x: 1, y: 2}
```

<br>

## 9.2. 계산된 프로퍼티 이름

문자열 또는 문자열로 타입 변환할 수 있는 값으로 평가되는 표현식을 대괄호([...])로 묶어서 사용해 동적으로 생성한 프로퍼티 키를 말한다.

### ES5

객체 릴터럴 외부에서 대괄호 표기법을 사용해 프로퍼티 키를 동적으로 생성할 수 있다.

```javascript
// ES5
var prefix = 'prop';
var i = 0;

var obj = {};

// 계산된 프로퍼티 이름으로 프로퍼티 키 동적 생성
obj[prefix + '-' + ++i] = i;
obj[prefix + '-' + ++i] = i;
obj[prefix + '-' + ++i] = i;

console.log(obj); // {prop-1: 1, prop-2: 2, prop-3: 3}
```

<br>

### ES6

ES6에서는 객체 리터럴 내부에서도 계산된 프로퍼티 이름으로 프로퍼티 키를 동적 생성할 수 있다.

```javascript
// ES6
const prefix = 'prop';
let i = 0;

// 객체 리터럴 내부에서 계산된 프로퍼티 이름으로 프로퍼티 키 동적 생성
const obj = {
  [`${prefix}-${++i}`]: i,
  [`${prefix}-${++i}`]: i,
  [`${prefix}-${++i}`]: i
};

console.log(obj); // {prop-1: 1, prop-2: 2, prop-3: 3}
```

<br>

## 9.3. 메서드 축약 표현

### ES5

메서드를 정의하려면 프로퍼티 값으로 함수를 할당한다.

```javascript
// ES5
var obj = {
    name: 'Kim',
    sayHi: function () {
        console.log('Hi! ' + this.name);
    }
};

obj.sayHi(); // Hi! Kim
```

<br>

### ES6

메서드를 정의할 때, function 키워드를 생략한 축약 표현을 사용할 수 있다.

```javascript
// ES6
const obj = {
    name: 'Kim',
    // 메서드 축약 표현
    sayHi() {
        console.log('Hi! ' + this.name);
    }
};

obj.sayHi(); // Hi! Kim
```

ES6의 메서드 축약 표현으로 정의한 메서드는 프로퍼티에 할당한 함수와 다르게 동작한다.