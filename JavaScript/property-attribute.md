# 프로퍼티 어트리뷰트

## 1. 내부 슬롯과 내부 메서드

#### 내부 슬롯(internal slot)과 내부 메서드(internal method)의 개념

내부 슬롯과 내부 메서드는 자바스크립트 엔진의 구현 알고리즘을 설명하기 위해 ECMAScript 사양에서 사용하는 **의사 프로퍼티(pseudo property)**와 **의사 메서드(pseudo method)**이다.

ECMAScript 사양에 등장하는 이중 대괄호([[…]])로 감싼 이름들이 내부 슬롯과 내부 메서드다.

<img src="https://poiemaweb.com/assets/fs-images/16-1.png" alt="img" style="zoom:50%;" />

내부 슬롯과 내부 메서드는 ECMAScript 사양에 정의된 대로 구현되어 자바스크립트 엔진에서 실제로 동작 하지만 개발자가 직접 접근할 수 있도록 **외부로 공개된 객체의 프로퍼티는 아니다**. 

- 자바스크립트는 내부 슬롯과 내부 메서드에 직접적으로 접근하거나 호출할 수 있는 방법을 원칙적으로 제공하지 않는다.
- 일부 내부 슬롯과 내부 메서드에 한하여 **간접적으로 접근할 수 있는 수단을 제공**한다.

<br>

```javascript
const o = {};

// 내부 슬롯은 자바스크립트 엔진의 내부 로직이므로 직접 접근할 수 없다.
o.[[Prototype]] // -> Uncaught SyntaxError: Unexpected token '['
// 단, 일부 내부 슬롯과 내부 메서드에 한하여 간접적으로 접근할 수 있는 수단을 제공하기는 한다.
// [[Prototype]] 내부 슬롯의 경우 __proto__를 통해 간접적으로 접근할 수 있다.
o.__proto__ // -> Object.prototype
```

<br><br>

## 2. 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체

**자바스크립트 엔진은 프로퍼티를 생성할 때 프로퍼티의 상태를 나타내는 프로퍼티 어트리뷰트를 기본값으로 자동 정의한다.** 

<br>

#### 프로퍼티의 상태

프로퍼티의 값(value), 값의 갱신 가능 여부(writable), 열거 가능 여부(enumerabel), 재정의 기능 여부(configurable)를 말한다.

<br>

#### 프로퍼티 어트리뷰트

자바스크립트 엔진이 관리하는 내부 상태 값(meta-property)인 내부 슬롯 [[Value]], [[Writable]], [[Enumerable]], [[Configurable]]이다.

프로퍼티 어트리뷰트에 직접 접근할 수 없지만 Object.getOwnPropertyDescriptor 메서드를 사용하여 간접적으로 확인할 수는 있다.

```javascript
const person = {
  name: 'Lee'
};

// 프로퍼티 디스크립터 객체를 반환
console.log(Object.getOwnPropertyDescriptor(person, 'name'));
// {value: "Lee", writable: true, enumerable: true, configurable: true}
```

- Object.getOwnPropertyDescriptor 메서드는 프로퍼티 어트리뷰트 정보를 제공하는 **프로퍼티 디스크립터(PropertyDescriptor) 객체**를 반환한다.

- 만약 존재하지 않는 프로퍼티나 상속받은 프로퍼티에 대한 프로퍼티 디스크립터를 요구하면 undefined가 반환된다.



```javascript
const person = {
  name: 'Lee'
};

// 프로퍼티 동적 생성
person.age = 20;

// 모든 프로퍼티의 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체들을 반환한다.
console.log(Object.getOwnPropertyDescriptors(person));
/*
{
  name: {value: "Lee", writable: true, enumerable: true, configurable: true},
  age: {value: 20, writable: true, enumerable: true, configurable: true}
}
*/
```

- ES8에서 도입된 Object.getOwnPropertyDescriptors 메서드는 모든 프로퍼티의 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체들을 반환한다.

<br>

<br>

## 3. 데이터 프로퍼티와 접근자 프로퍼티

- 데이터 프로퍼티(data property)

  키와 값으로 구성된 일반적인 프로퍼티다. 지금까지 살펴본 모든 프로퍼티는 데이터 프로퍼티다.

- 접근자 프로퍼티(accessor property)

  자체적으로는 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 호출되는 접근자 함수(accessor function)로 구성된 프로퍼티다.

<br>

### 3.1.  데이터 프로퍼티

데이터 프로퍼티 어트리뷰트는 자바스크립트 엔진이 프로퍼티를 생성할 때 기본값으로 자동 정의된다.

| 프로퍼티 어트리뷰트 | 프로퍼티 디스크립터 객체의 프로퍼티 | 설명                                                         |
| :------------------ | :---------------------------------- | :----------------------------------------------------------- |
| [[Value]]           | value                               | - 프로퍼티 키를 통해 프로퍼티 값에 접근하면 반환되는 값이다. - 프로퍼티 키를 통해 프로퍼티 값을 변경하면 [[Value]]에 값을 재할당한다. 이때 프로퍼티가 없으면 프로퍼티를 동적 생성하고 생성된 프로퍼티의 [[Value]]에 값을 저장한다. |
| [[Writable]]        | writable                            | - 프로퍼티 값의 변경 가능 여부를 나타내며 불리언 값을 갖는다. - [[Writable]]의 값이 false인 경우 해당 프로퍼티의 [[Value]]의 값을 변경할 수 없는 읽기 전용 프로퍼티가 된다. |
| [[Enumerable]]      | enumerable                          | - 프로퍼티의 열거 가능 여부를 나타내며 불리언 값을 갖는다. - [[Enumerable]]의 값이 false인 경우 해당 프로퍼티는 for…in 문이나 Object.keys 메서드 등으로 열거할 수 없다. |
| [[Configurable]]    | configurable                        | - 프로퍼티의 재정의 가능 여부를 나타내며 불리언 값을 갖는다. - [[Configurable]]의 값이 false인 경우 해당 프로퍼티의 삭제, 프로퍼티 어트리뷰트 값의 변경이 금지된다. 단, [[Writable]]이 true인 경우 [[Value]]의 변경과 [[Writable]]을 false로 변경하는 것은 허용된다. |

<br>

### 3.2. 접근자 프로퍼티

- 접근자 프로퍼티(accessor property)는 자체적으로는 값을 갖지 않고 다른 **데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수(accessor function)로 구성된 프로퍼티**다.
- 접근자 프로퍼티는 getter와 setter 함수를 모두 정의할 수도 있고 하나만 정의할 수도 있다. 

| 프로퍼티 어트리뷰트 | 프로퍼티 디스크립터 객체의 프로퍼티 | 설명                                                         |
| :------------------ | :---------------------------------- | :----------------------------------------------------------- |
| [[Get]]             | get                                 | 접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 읽을 때 호출되는 접근자 함수다. 즉, 접근자 프로퍼티 키로 프로퍼티 값에 접근하면 프로퍼티 어트리뷰트 [[Get]]의 값, 즉 getter 함수가 호출되고 그 결과가 프로퍼티 값으로 반환된다. |
| [[Set]]             | set                                 | 접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 저장할 때 호출되는 접근자 함수다. 즉, 접근자 프로퍼티 키로 프로퍼티 값을 저장하면 프로퍼티 어트리뷰트 [[Set]]의 값, 즉 setter 함수가 호출되고 그 결과가 프로퍼티 값으로 저장된다. |
| [[Enumerable]]      | enumerable                          | 데이터 프로퍼티의 [[Enumerable]]과 같다.                     |
| [[Configurable]]    | configurable                        | 데이터 프로퍼티의 [[Configurable]]과 같다.                   |



<br>

<br>

## 4. 프로퍼티 정의

- 새로운 프로퍼티를 추가하면서 프로퍼티 어트리뷰트를 명시적으로 정의하거나,

  기존 프로퍼티의 프로퍼티 어트리뷰트를 재정의하는 것을 말한다.

  이를 통해 객체의 프로퍼티가 어떻게 동작해야 하는지를 명확히 정의할 수 있다.

  - 프로퍼티 값의 갱신 여부, 프로퍼티를 열거 가능 여부, 프로퍼티 재정의 가능 여부를 정의할 수 있다.

- Object.defineProperty 메서드를 사용하면 프로퍼티의 어트리뷰트를 정의할 수 있다. 

  인수로는 객체의 참조와 데이터 프로퍼티의 키인 문자열, 프로퍼티 디스크립터 객체를 전달한다.

<br>

Object.defineProperty 메서드로 프로퍼티를 정의할 때 프로퍼티 디스크립터 객체의 프로퍼티를 일부 생략할 수 있다. 프로퍼티 디스크립터 객체에서 생략된 어트리뷰트는 다음과 같이 기본값이 적용된다.

| 프로퍼티 디스크립터 객체의 프로퍼티 | 대응하는 프로퍼티 어트리뷰트 | 생략했을 때의 기본값 |
| :---------------------------------- | :--------------------------- | :------------------- |
| value                               | [[Value]]                    | undefined            |
| get                                 | [[Get]]                      | undefined            |
| set                                 | [[Set]]                      | undefined            |
| writable                            | [[Writable]]                 | false                |
| enumerable                          | [[Enumerable]]               | false                |
| configurable                        | [[Configurable]]             | false                |

Object.defineProperty 메서드는 한번에 하나의 프로퍼티만 정의할 수 있다. Object.defineProperties 메서드를 사용하면 여러 개의 프로퍼티를 한 번에 정의할 수 있다.



## 5. 객체 변경 방지

- 객체는 변경 가능한 값이므로 재할당 없이 직접 변경할 수 있다.
- 즉, 프로퍼티를 추가하거나 삭제할 수 있고, 프로퍼티 값을 갱신할 수 있으며, Object.defineProperty 또는 Object.defineProperties 메서드를 사용하여 프로퍼티 어트리뷰트를 재정의할 수도 있다.

자바스크립트는 객체의 변경을 방지하는 다양한 메서드를 제공한다.

| 구분           | 메서드                   | 프로퍼티 추가 | 프로퍼티 삭제 | 프로퍼티 값 읽기 | 프로퍼티 값 쓰기 | 프로퍼티 어트리뷰트 재정의 |
| :------------- | :----------------------- | :-----------: | :-----------: | :--------------: | :--------------: | :------------------------: |
| 객체 확장 금지 | Object.preventExtensions |       ✕       |       ○       |        ○         |        ○         |             ○              |
| 객체 밀봉      | Object.seal              |       ✕       |       ✕       |        ○         |        ○         |             ✕              |
| 객체 동결      | Object.freeze            |       ✕       |       ✕       |        ○         |        ✕         |             ✕              |

<br>

