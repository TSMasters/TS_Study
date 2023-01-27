### 1. 함수 선언문

```ts
//표현식
function 함수이름(매개변수1: 타입1, 매개변수2: 타입2[, ...]): 반환값 타입{
    함수 몸통
}
```
```ts
function add1(num1: number, num2: number) : void{
    //return num1 + num2
    console.log(num1+ num2);
}
add1(1,2);
```

### 2. void타입
+ 값을 반환하지 않는 함수의 반환 타입: void => 함수 반환 타입으로만 사용 가능

```ts
fucntion printMe(name: string, age: number): void{
    console.log(`name: ${name}, age: ${age}`)
}
```

### 3. 함수 시그니처
+ 변수에 타입이 있듯이 함수에도 타입이 있다. => 함수의 타입
```ts
//표현식
(매개변수1 타입, 매개변수2 타입[, ...]) => 반환값 타입
```
```ts
//ex1
let printMe: (string, number) => void 
    = function(name: string, age: number): void{}
```
```ts
//ex2
const sayHello: (name: string, age: number) => void 
    = function(name: string, age: number): void {}


type sayHello = (name: string, age: number) => void


const hello: sayHello = (a, b) => {
    console.log(`이름: ${a}, 나이: ${b}`)
}
```
+ () => void : 매개변수도 반환값도 없는 함수 시그니처

### 4. 선택적 매개변수

```ts
//표현식
function fn(arg1: string, arg?: number): void{}
```
```ts
//#1. basic
function hello1(name?: string): string{
    return `hello, ${ name || 'world'}`;
}

hello1('sikyung'); //hello, sikyung
hello1(); //hello, world

//#2. js에서 매개변수 default 값 주기
function hello2(name='world'){
    return `hello, ${ name }`;
}
```

+ optional parameter의 위치
```ts
//optional의 위치 - 올바른 예시
function loc1(name: string, age?: number): string{
    if (age != undefined){
        return `hello, My name is ${name}.`;
    } else{
        return `hello, ${name}. I am ${age}.`;
    }
}

//optional의 위치 - 잘못된 예시
function loc2( age?: number, name: string): string{
    if (age == undefined){
        return `hello, My name is ${name}.`;
    } else{
        return `hello, ${name}. I am ${age}.`;
    }
}

// optional을 앞에 쓰고 싶다면 undefined로 타입 작성
function loc2( age: number | undefined, name: string): string{
    if (age == undefined){
        return `hello, My name is ${name}.`;
    } else{
        return `hello, ${name}. I am ${age}.`;
    }
}
```

### 5. rest paramiters
+ 전달받은 매개변수를 배열로 반환
```ts
function add2(...nums: number[]){
    return nums.reduce((result, num)=> result + num, 0);
}
console.log(add2(1,2,3));
console.log(add2(1,2,3,4,5,6,7,8,9,10));
```

### 6. this 
+ this 타입도 (매개변수 맨 앞에) 정의해줘야 함 
```ts
interface User{
    name: string;
}
const Bob: User = { name: "Bob"};

function Info(this: User, age: number, gender: 'm'|'f') {
    console.log(this.name, age, gender)
}

//this로 Bob 객체를 강제함
const a = Info.bind(Bob);
a(30, 'm');
```
+ 콜백에서의 this
```ts
interface UIElement {
    // 아래 함수의 `this: void` 코드는 함수에 `this` 타입을 선언할 필요가 없다는 의미
    addClickListener(onclick: (this: void, e: Event) => void): void;
}
  
class Handler {
    info: string;
    onClick(this: void, e: Event) {
    // `this`의 타입이 void이기 때문에 여기서 `this`를 사용할 수 없다.
    console.log('clicked!');
    }
}

let handler = new Handler();
UIElement.addClickListener(handler.onClick);
```

### 7. 함수 오버로드
+ 동일한 함수지만 매개변수의 타입/갯수에 따라 다르게 동작하게 하려면 함수를 오버로드 함
```ts
interface Info{
    name: string;
    age: number;
}

function join(name: string, age: string): string; //오버로드
function join(name: string, age: number): Info; //오버로드

function join(name:string, age: number | string): Info | string {
    if (typeof age === "number"){
        return { name, age };
    } else{
        return "나이는 숫자로 입력해주세요";
    }
}

const Mike: Info = join("Mike", 20);
const Anna: String = join("Anna", "25");
```

### 8. 콜백함수
+ 함수 표현식을 매개변수로 받을 수 있다. => 매개변수 형태로 동작하는 함수

```ts
//표현식
const f = (callback: () => void): void => callback()
```
```ts
//init.ts
export const init = (callback: ()=> void): void =>{
    console.log('default initialzation finished')
    callback()
    console.log('all initialization finished')
}

//callback.ts
import {init} from './init'
init(()=>console.log('custom initialization finished.'))
```

### 9. 클래스 메서드
+ function 함수와 this 메서드</br>
: 타입스크립트의 function 키워드로 만든 함수는 Function이란 클래스의 인스턴스, 즉 함수는 객체이다. 
+ 객체지향언어에서 인스턴스는 this를 사용할 수 있다. 
+ function 키워드로 만든 함수에 this 키워드를 사용할 수 있다. 
+ 반면에 화살표 함수에는 this키워드를 사용할 수 없다.
```ts
//A.ts
export class A{
    value: number = 1
    method: () => void = function(): void{
        console.log(`value: ${this.value}`)
    }
}
//testA.ts
import {A} from './A'
let a: A = new A
a.method //value: 1
```
```ts
//B.ts
export class B{
    constructor(public value: number = 1){}
    method(): void{
        console.log(`value: ${this.value}`)
    }
}
//testB.ts
import {B} from './B'
let b: B = new B(2)
b.method() //value: 2
```

