# 변수

## 💙 점진적 타입검사

- 타입 검사를 수행하면서 필요에 따라 타입 선언의 생략을 허용한다. 타입 선언을 생략하면 **암시적(implicit)**
 형변환이 일어난다.
- 타입스크립트에서 점진적 타이핑을 설명할 때 적절한 타입으로 **any**
가 있다.
    - **any 타입**은 모든 타입의 최상위 타입이며, **any 타입**으로 선언된 변수는 어떤 타입의 변수도 받아들이면서 심지어 타입이 없는 변수도 받아들인다.
    - **TypeScript, Python이 대표적**

## 💙 자바 스크립트의 동적타이핑

- **기본 타입 (primitive types)**: Number, Boolean, String
- **객체 타입 (object tpyes)**: 객체 리터럴, 배열, 내장 객체
- 자바스크립트에서는 타입이 추론되는데  **값을 변수에 할당할 때 타입이 정해지는 것**을 동적 타이핑(dynamic typing)이라 한다.

## 💙 기본 타입

- string, number, boolean, symbol, enum, 문자열 리터럴

### 문자열

```jsx
const str: string = "hello"
```

### 숫자

```jsx
const num: number = 10;
```

### 진위값 선언

```tsx
let show: boolean = true
```

### Symbol

객체의 key가 문자열이나 정수일 때의 문제는, 프로그래머의 실수등으로 존재하는 객체의 key 값이 변경될 수 있다. Symbol로 정의할 경우 해당 키는 unique 하기에 중복되지 않는다.

```tsx
let hello = Symbol("hello");
let hello2 = Symbol("hello");
console.log(hello === hello2); // false

const uniqueKey = Symbol();
let obj = {};

obj[uniqueKey] = 1234;
console.log(obj[uniqueKey]); //1234
console.log(obj); // { [Symbol()]: 1234 }
```

### 이넘

- 숫자형 이넘
    - 별도의 값을 지정하지 않으면 숫자형 이넘이다.
    - 디폴트로 0이 지정된다.

```tsx

enum Shoes{

    Nike,
    Adidas
}

var myshoes=Shoes.Nike //0
Shoes.Adidas//1
```

- 문자형 이넘
    
    ```tsx
    enum Shoes{
    
        Nike='나이키',
        Adidas='아디다스'
    }
    
    var myshoes=Shoes.Nike //이땐 '나이키'가 출력
    ```
    
- 예제
    
    ```tsx
    enum Answer{
        Yes='Y',
        No='N'
    }
    
    function askQuestion(answer:Answer){
    
        if(answer===Answer.Yes){
            console.log('정답입니다')
        }
        if(answer===Answer.No){
            console.log('오답입니다')
        }
    
    }
    
    askQuestion("예스") //안됨
    //이넘에서 지원하는 데이터만 들어갈 수 있음
    askQuestion(Answer.Yes)
    ```
    

## 💙 객체 타입

### 튜플 선언

```jsx
let address: [string, number] = ['gangnam', 100]
```

- 배열의 인덱스 타입까지 정의한다
- 배열의 길이가 고정되고, 각 요소의 타입이 지정되어 있는 배열 형식이라고 보면된다.

### 배열 선언

```jsx
const arr: Array<number> = [1, 2, 3]
const heroes: Array<string> = ['capt', 'Thor', 'Hulk']
// java같은 느낌을 첨가
const items: number[] = [1, 2, 3]
```

### 객체 선언

```tsx

let obj: object = {}
// 객체 안에 좀 더 정확한 타입지정
let person: { name: string, age: number } = {
    name: 'capt',
    age: 100
}

```

## 참고자료

[https://velog.io/@denmark-banana/TypeScript-변수-선언과-기본-타입](https://velog.io/@denmark-banana/TypeScript-%EB%B3%80%EC%88%98-%EC%84%A0%EC%96%B8%EA%B3%BC-%EA%B8%B0%EB%B3%B8-%ED%83%80%EC%9E%85)
