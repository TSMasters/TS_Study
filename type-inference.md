## 1. 타입 추론
사실 본론부터 말하자면, 타입스크립트를 선택해서 사용하는만큼 제대로 명시해서 사용해주어 웬만하면 사용하지 말도록 하자! 

### 1.1. 타입 추론이란?
+ 타입스크립트가 코드를 해석해나가는 동작
+ 타입스크립트에서 명시적인 타입 표기가 없을 때 타입 정보를 제공하기 위해 사용되는 것

### 2.1. 타입 추론 1: 변수/함수 리턴값 지정
```ts
let x = 3; //let x: number
let y = "4"; // let y: string
let z = [0, 1, null]; //let z:( number | null []]
```

### 2.2. 타입 추론 2: 인터페이스 + 제네릭 방식
+ 하 이게 뭔소리지? 라고 했는데 제네릭 방식이 결국엔 타입추론을 통해 가능해지는 것이므로 특별히 어렵게 생각할 필요가 없다.!
+ 인터페이스 제네릭을 정의했을 때, 제네릭의 값을 타입스크립트에서 추론해서 변수의 필요한 속성/타입까지 보장해주는 타입 추론의 기본적인 방식이기 때문!
```ts
interface Product<T>{
    title: string;
    price: T;
}
let item:Product<number> = {
    title: 'pencil'
    price: 4000,
}
```
### 2.3. 타입 추론 3: 제네릭 방식에서 extends로 까지 확장 해 복잡한 경우
+ 확장한 경우에도 제네릭까지 확장됨
```ts
interface Product<T>{
    title: string;
    price: T;
}
interface DetailedProduct<K> extends Product<K>{
    description: string;
    popular: K;
}
let item:etailedProduct<number> = {
    title: 'pencil',
    price: 4000,
    description: 'smooooooth!',
    popular: 1,
}
```
### 2.4. 타입 추론 4: Best Common Type
+ 배열에 있는 요소들 중에서 근접한 타입을 추론해서 유니온으로 표현해주는 방식
```ts
let arr = [1,2,true,false,'a'] //let arr: (string | number | boolean)[]
```

### 2.5. 타입 추론 5: Contextual Typing
+ 타입스크립트는 코드의 위치에 따라 다른 추론을 할 수 있다. 
+ 이 문맥상의 타이핑(타입 결정)은 코드의 위치(문맥)를 기준으로 일어난다. 

```ts
const click = (e) => {
  e; // any
}

document.addEventListener('click', (e) => {
  e; //MouseEvent
})
```

### 3. Typescript Language Server : intelliSense


+ 추가로 이 링크 넘 조아서 한 번 읽어보시길 추천.....!!!
+ https://inpa.tistory.com/entry/TS-📘-타입-추론-타입-호환-타입-단언-타입-가드-💯-총정리s