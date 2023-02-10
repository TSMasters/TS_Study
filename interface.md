## 인터페이스란?

타입스크립트에서 인터페이스란 두개의 시스템 사이에 상호 간에 정의한 약속 혹은 규칙을 포괄하여 의미하는데

약속의 범주란

- 객체의 스펙(속성과 속성의 타입)
- 함수의 파라미터
- 함수의 스펙(파라미터, 반환 타입 등)
- 배열과 객체를 접근하는 방식
- 클래스

## 인터페이스와 타입별칭의 차이점

```tsx
type todoitems={
    id:number,
    title:string,
    done:boolean
}
interface todo{
    id:number,
    title:string,
    done:boolean
}
```

- 타입 별칭과 인터페이스의 가장 큰 차이점은 타입의 확장 가능 / 불가능 여부이다.
- **인터페이스는 확장이 가능한데 반해 타입 별칭은 확장이 불가능하다.**
- **데이터를 표현할 때는 타입 별칭 사용**
- **데이터를 포괄하는 객체를 묘사하는 경우에는 인터페이스를 사용**(메소드와 같이 구체적인 행위까지 포함된 객체를 디자인할 때)
- **클래스**를 묘사할 때는 클래스 자체가 데이터와 행위를 포괄하고 있기 때문에 **인터페이스를 사용**하는 것이 훨씬 자연스럽다.

## 함수의 인자를 정의하는 인터페이스

```tsx
//함수의 인자를 정의하는 인터페이스

interface User{
    age:number,
    name:string
}

function getUser(user:User){

    console.log(user)

}

const kathy={
    age:22,
    name:'kathy'
}

getUser(kathy)
```

User라는 타입을 정의할 때 인터페이스가 사용된다.

## 함수의 스펙에 인터페이스 사용

```tsx
// 함수의 스펙에 인터페이스 활용

interface SumFunction{
    (a:number,b:number):number;
    //매개변수 타입과 반환 타입
}

var sum2:SumFunction;

sum2=function(a:number,b:number):number{
    return a+b

}
```

- a:number는  매개변수 타입을 지정해준거고 괄호 밖에 있는 number는 반환타입을 지정해줌으로써 인터페이스를 통해 sum2라는 함수의 스펙을 정의한다.

## 읽기 전용 프로퍼티

```tsx
interface User {
   name: string;
   age: number;
   gender?: string;
   readonly birthYear: number; // 읽기 전용 속성
}
 
let user: User = {
   name: 'jeff',
   age: 30,
   birthYear: 2010, // 최초에 값을 초기화 할때만 할당이 가능
};
 
user.birthYear = 1999; // Error - 이후에는 수정이 불가능
```

- read-only 속성을 적용해준 변수는 수정이 불가능하다.
- 만약 하나의 인터페이스가 모두 read-only일 경우 필드 값 하나하나에 readonly를 붙여주기 보단 Readonly 유틸리티나 const를 활용한 타입 단언을 사용하자.

```tsx
// Readonly 유틸리티(Utility) 활용
interface IUser {
  name: string,
  age: number
}

let user: Readonly<IUser> = { // Array 처럼 따로 Readonly 라는 자료형이 있다고 생각하면 된다
  name: 'Neo',
  age: 36
};

user.age = 85; // Error
user.name = 'Evan'; // Error

// Type assertion
let user = {
  name: 'Neo',
  age: 36
} as const; // 따로 인터페이스를 이용하지 않고 객체 데이터 자체에 as const를 붙이게 되면 이 자체가 리터럴 타입이 되게 된다.

user.age = 85; // Error
user.name = 'Evan'; // Error
```

## 인터페이스 호환

- 같은 이름의 인터페이스를 여러개 만들거나
- 기존에 만들어진 인터페이스에 내용을 더 추가하는 경우 인터페이스 호환이 사용된다.

```tsx
interface IFullName {
  firstName: string,
  lastName: string
}
 
interface IFullName {
  middleName: string
}
 
const fullName: IFullName = {
  firstName: 'Tomas',
  middleName: 'Sean',
  lastName: 'Connery'
};
```

## 인터페이스 확장

```tsx

//인터페이스 확장

interface Person{

    name:string;
    age:number;
}

interface Developer extends Person{
    language:string;
}

var capt:Developer={

    name:'capt',
    age:21,
    language:'ts'
}

```

- 인터페이스가 확장되었기 때문에 name,age,language 모두 써줘야 한다!

## 인터페이스 클래스 타입

```tsx
interface IUser {
   name: string;
   getName(): string;
}

// IUser 인터페이스를 implements 하면 User 클래스의 프로퍼티 구조는 반드시 IUser에 정의된 대로 따라야 한다.
// 즉, 반드시 name 변수와 getName() 메소드를 클래스에 기본값으로 구현해야 한다.
class User implements IUser {
   name: string;

   constructor(name: string) {
      this.name = name;
   }

   getName() {
      return this.name;
   }
}

const neo = new User('Neo');
neo.getName(); // Neo
```

- 인터페이스로 클래스를 정의하는 경우, implements 키워드를 사용해 클래스 정의 옆에다 붙여주면 된다.

## 인덱싱 방식을 정의하는 인터페이스

- 인터페이스에서 정의할 속성들이 엄청 많아 질경우에 사용한다.

```tsx
type Score = 'A' | 'B' | 'C' | 'D' | 'F';

interface User {
   name: string;
   [grade: number]: Score; // indexable 타입 (선택적 프로퍼티 처럼 반드시 선언 안해줘도 된다.)
}

const user1: User = {
   name: '홍길동',
   1: 'A',
};

const user2: User = {
   name: '임꺾정',
   3: 'F',
};

const user3: User = {
   name: '박혁거세',
   2: 'B',
};
```

- 위와 같이 key 값에 들어올 수 있는 경우의 수가 매우 많은데 이걸 하나하나 지정해줄 수 없을 때 인덱싱 방식을 이용한다.

## 인터페이스 딕셔너리 패턴

```java
interface StringArray{
    [index:number]:string;
}
var arr2:StringArray=['a','b','c']
```

위의 코드의 경우 arr2[0],arr2[1]이런식으로 접근하는데 만약 [index:String] 이라면?

```tsx
interface StringArr{
    [key:string]:string
}

var arr3:StringArr={
    'num1':'sss'
}

console.log(arr3['num1']);
```

접근이 된다! 이걸 인터페이스 딕셔너리 패턴이라고 한다.

### 궁금증!

arr3[num1] 이라 해도 되고 arr3[’num1’]이라 해도 되는데 어떻게 가능한거죠?

### 🚨 인덱싱 할때의 주의점

[인덱서]의 타입은 string과 number만 지정할 수 있다는 점에 유의하자.

이는 생각해보면 당연한 원리인데, key는 문자만 되고(object.key), 배열은 인덱스(array[0])는 숫자이기 때문이다.

## 인덱스 시그니처

인터페이스에 **정의되지 않은 속성** 들을 유기적으로 사용할 때 유용하다는 장점이 있다. (단, 해당 속성이 인덱스 시그니처에 정의된 반환 값을 가져야 한다)

```tsx
interface IUser {
   name: string;
   age: number;
   [userProp: string]: string | number;
}

let user: IUser = {
   name: 'Neo',
   age: 123,

   // 인터페이스에 직접 정의되지 않는 속성이라도, 인덱서블 타입에만 잘 맞추면 얼마든지 추가가 가능하다.
   email: 'thesecon@gmail.com',
   gender: 'F',
   height: 180,
};

console.log(user.email); // thesecon@gmail.com
console.log(user.height); // 180
```

궁금한점 객체에서 Key값은 “email”이 아니라 email이라 해도 string으로 인식이 되나요?

```tsx
interface PhoneNumberDictionary {
   [phone: string]: { // 문자열을 key로 받고, 객체 타입 value 로 구성된 인덱서블 시그니처
      num: string;
   };
}

interface Contact {
   name: string; // 이름
   address: string; // 주소
   phones: PhoneNumberDictionary; // 집번호, 회사번호 ..등 중첩 객체 구조로 구성
}

// 아래와 같은 꼴을 타이핑할 수 있게 됨
const inform = {
   name: 'Tony',
   address: 'Malibu',

   // [phone: string]: { num: number };
   phones: {
      home: {
         num: '033-1111-1111',
      },
      office: {
         num: '011-1111-1222',
      },
   },
};
```

## 참고자료

[https://inpa.tistory.com/entry/TS-📘-타입스크립트-인터페이스-💯-활용하기](https://inpa.tistory.com/entry/TS-%F0%9F%93%98-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4-%F0%9F%92%AF-%ED%99%9C%EC%9A%A9%ED%95%98%EA%B8%B0)

[https://lakelouise.tistory.com/179](https://lakelouise.tistory.com/179)

[https://kyounghwan01.github.io/blog/TS/fundamentals/interface/#duck-typing](https://kyounghwan01.github.io/blog/TS/fundamentals/interface/#duck-typing)
