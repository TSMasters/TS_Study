# 제네릭,타입가드,타입호환,타입단언

## 제네릭

- 타입을 함수의 파라미터로 받게 된다 정도로 생각할 것!
- 정적 type 언어는 클래스나 함수를 정의할 때 type을 선언해야 한다.
- Generic은 코드를 작성할 때가 아니라 **코드를 수행될 때(런타임) 타입을 명시한다.**
- 코드를 작성할 때 식별자를 써서 아직 정해지지 않은 타입을 표시한다.
    - 일반적으로 식별자는 T, U, V ...를 사용한다.
    - 필드 이름의 첫 글자를 사용하기도 한다.

```tsx
function logText<T>(text:T):T{

    console.log(typeof(text))
    return text;
}
//함수 호출할때 타입을 지정함
logText<string>('힝구')
logText<number>(10)
```

### 제네릭의 장점은?

- 한 가지 타입보다 여러 가지 타입에서 동작하는 컴포넌트를 생성하는 데 사용된다.
- 재사용성이 높은 함수와 클래스를 생성할 수 있다.
    - 여러 타입에서 동작이 가능하다. (한 번의 선언으로 다양한 타입에 재사용할 수 있다.)
    - 코드의 가독성이 향상된다.
- 오류를 쉽게 포착할 수 있다.
    - any타입을 사용하면 컴파일 시 타입을 체크하지 않는다.
    - 타입을 체크하지 않아 관련 메서드의 힌트를 사용할 수 없다.
    - 컴파일 시에 컴파일러가 오류를 찾지 못한다.
- generic도 any처럼 타입을 지정하지 않지만, 타입을 체크해 컴파일러가 오류를 찾을 수 있다.

### 인터페이스에 제네릭 선언하는 방법

인터페이스에서 제네릭을 쓸 때는 타입을 적어도 한개는 지정해줘야 한다!

```tsx
interface DropDown2<T>{
    value:T;
    selected:boolean;

}

const obj2:DropDown2<string>={value:"dd",selected:false}
```

```tsx
//인터페이스에 제네릭을 선언하는 방법

interface DropDown2<T>{
    value:T;
    selected:boolean;

}

const obj2:DropDown2<string>={value:"dd",selected:false}
const obj3:DropDown2<number>={value:1,selected:false}
```

이런식으로 코드 재사용도 가능!

### 클래스에서 제네릭 사용하기

```jsx
class Queue<T> {
    protected data: Array<T> = [];
 
    push(item: T) {
        this.data.push(item);
    }
 
    pop(): T | undefined {
        return this.data.shift();
    }
}
 
const numberQueue = new Queue<number>();
 
numberQueue.push(0);
numberQueue.push("1"); // 의도하지 않은 실수를 사전 검출 가능
numberQueue.push(+"1"); // 실수를 사전 인지하고 수정할 수 있다.
```

### 문제점

 매개변수로 받아온 값의 length를 알아내고 싶다.

예를들어 logTextLength(’a’) 를 하고

```java
function logTextLength<T>(text:T):T{

	// return text.length

}

console.log(logTextLength('a')
```

를 하게 되면 text의 length에 접근할 수 없다! 이때 어떻게 문제를 해결할 수 있을까?

### 제네릭의 타입제한 → 제네릭을 배열로 받아본다.

```tsx
// 제네릭의 타입제한

function logTextLength<T>(text:T[]):T[]{
    // 만약 length를 찍고 싶으면 배열로 제네릭을 선언하면됨
    console.log(text.length)
    return text
}

logTextLength<string>(['hi','abc'])
```

### 정의된 타입으로 타입을 제한하기 (⭐)

→ extends를 사용해서 제네릭을 확장해서 타입을 제한한다.  (LengthType 이라는 인터페이스를 만들어서 제네릭에 length가 있다는 것을 알려준다!)

```tsx
// 제네릭 타입 제한 2- 정의된 타입 제한하기

interface LengthType{
    length:number;
}

// length를 찍는 또 다른 방법
function logTextLength<T extends LengthType>(text:T):T{
    text.length
    return text
}
```

```tsx
interface ShoppingItem{
    name:string;
    price:number;
    stock:number;
}

function getShoppingItemOption<T extends ShoppingItem>(itemOption:T):T{
    return itemOption
}

//

getShoppingItemOption<ShoppingItem>({name:"pants",price:10000,stock:1})
```

이런식으로 extends를 사용해볼 수 있을 것 같다! ShoppingItem 타입을 엄격히 제한!

### keyof로 제네릭의 타입 제한하기(여기 아직 잘 이해가 안됨..ㅜ)

```tsx
// keyof

interface ShoppingItem{
    name:string;
    price:number;
    stock:number;
}

function getShoppingItemOption<T>(itemOption:T):T{
    return itemOption
}

getShoppingItemOption(10);
getShoppingItemOption<string>('a');
```

우선 이렇게만 해줘도 잘 돌아가긴 한다.

하지만 내가 타입을 ShoppingItem인 타입만 올 수 있게 엄격하게 제한하려 한다.  → 이때 keyof씀

ShoppingItem의 key들 중에 하나가 제네릭이 된다! 타입이 된다! 라는 것

```tsx
interface ShoppingItem{
    name:string;
    price:number;
    stock:number;
}

function getShoppingItemOption<T extends keyof ShoppingItem>(itemOption:T):T{
    return itemOption
}

getShoppingItemOption('name')
```

T에는 ShoppingItem의 key값 중 하나만 들어간다!

• keyof : 객체 형태의 타입을, 따로 속성들만 뽑아 모아 유니온 타입으로 만들어주는 연산자(OR 연산자( ||))

```jsx
interface Todo {
	id: number
	text: string
}

type Keys = keyof Todo
// === type Keys = 'id' | 'text'

let a: Keys = 'id'
a = 'text'
a = 'ids' // 🚨ERROR!
a = 'id' | 'text' // 🚨ERROR!
```

## 타입가드

**컴파일러가 타입을 예측할 수 있도록 타입을 좁혀 주어서(narrowing)** 좀 더 `type safety`함을 보장

```jsx
// 구별된 유니온
type Human = {
    think: () => void;
};
 
type Dog = {
    bark: () => void;
}
 
declare function getEliceType(): Human | Dog;
 
const elice =  getEliceType();
```

여기까지는 elice가 human인지 dog인지 모름

구별된 유니온으로 타입가드를 해보자!

```jsx
type Human = {
    think: () => void;
};
 
type Dog = {
    tail: string; // 구별한 단서(태그)
    bark: () => void;
}
 
declare function getEliceType(): Human | Dog;
 
const elice =  getEliceType();
 
if ('tail' in elice) {
    elice.bark(); // Dog 타입으로 추론할 수 있다.
} else {
    elice.think(); // Human 타입으로 추론할 수 있다.
}
```

### 실무에서 자주쓰는 타입가드 유형

```jsx
type SuccessResponse = {
    status: true;
    data: any;
};
 
type FailureResponse = {
    status: false;
    error: Error;
};
 
type CustomResponse = SuccessResponse | FailureResponse;
 
declare function getData(): CustomResponse;
 
const response: CustomResponse = getData();
 
if(response.status) {
    console.log(response.data);
} else if (response.status === false) {
    console.log(response.error);
}
```

[[TypeScript] 타입스크립트 타입 가드 (Type Guard)](https://lakelouise.tistory.com/191)

## 타입호환

타입 호환이란 단어 그대로 타입스크립트 코드에서 특정 타입 간에 구조가 비슷하면 타입 호환이 될 수 있다는 것을 의미한다.

```jsx

interface Ironman {
  name: string;
}

class Avengers {
  name: string;
}

let i: Ironman;
i = new Avengers(); // OK, because of structural typing
```

- 인터페이스로 선언한 Ironman 타입을 가진 변수 i가 Avengers 클래스 생성자를 어떤 에러 없이 할당 받은 것을 볼 수 있다.
- Ironman과 Avengers는 서로 다른 자료형임에도  타입 구성을 보면 name: string 으로 서로 비슷하게 보인다.
- 사람 눈에도 그렇게 보이듯이 컴파일러도 그렇게 타입 추론 해줌으로서 타입 호환

[https://inpa.tistory.com/entry/TS-📘-타입-추론-타입-호환-타입-단언-타입-가드-💯-총정리](https://inpa.tistory.com/entry/TS-%F0%9F%93%98-%ED%83%80%EC%9E%85-%EC%B6%94%EB%A1%A0-%ED%83%80%EC%9E%85-%ED%98%B8%ED%99%98-%ED%83%80%EC%9E%85-%EB%8B%A8%EC%96%B8-%ED%83%80%EC%9E%85-%EA%B0%80%EB%93%9C-%F0%9F%92%AF-%EC%B4%9D%EC%A0%95%EB%A6%AC)

## 타입단언

개발자가 ****이 타입은 내가 정의할게 맞다고 컴파일러에게 알려주는 것을 말한다.

런타임에는 영향을 미치지는 않고 오직 컴파일 과정에서만 사용된다.

1.  앵글 브라켓(angle-bracket, <>) 문법을 사용

```tsx
let assertion: unknown = "타입 어설션은 '타입을 단언'합니다.";
// 방법 1: assertion 변수의 타입을 string으로 단언 처리
let assertion_count:number = (<string>assertion).length;Copy
```

1.  as 문법을 사용

```tsx
let assertion: unknown = "타입 어설션은 '타입을 단언'합니다.";
// 방법 2: assertion 변수의 타입을 string으로 단언 처리
let assertion_count:number = (assertion as string).length;
```
