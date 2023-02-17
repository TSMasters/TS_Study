### 1. 배열 타입 할당 조건을 명시적으로 선언하는 방법
```ts
// 오직 숫자 아이템만 허용
let nums:number[] = [100, 101, 102];
​
// 오직 문자 아이템만 허용
let strs:string[] = ['ㄱ', 'ㄴ', 'ㄷ'];
​
// 오직 불리언 아이템만 허용
let boos:boolean[] = [true, false, true];
​
// 모든 데이터 타입을 아이템으로 허용
let anys:any[] = [100, 'ㄴ', true];
​
// 특정 데이터 타입만 아이템으로 허용
let selects:(number|string)[] = [102, 'ㅇ'];
```
### 만약 이런 상황이라면 어떻게 명시적으로 선언해줘야 할까?
```ts
only_num_str_boo.push(10);
only_num_str_boo.push(true);
only_num_str_boo.push('프로그래밍');

console.log(only_num_str_boo);

//이렇게 선언해주면 된다!
let only_num_str_boo:(number|string|boolean)[] = [];
```

### 2. type를 이용해 선언하기
```ts
type IPerson = {name : string, age?: number}
let personArray : IPerson[] [{name : 'Jack'}, {name:'jane', age:32}]
```

### 3. 배열의 비구조화 할당
```ts
let array: number[] = [1,2,3,4,5]
let [first,second,third, ... rest] = array;
console.log(first, second, third, rest) /// 1 2 3 [4, 5]
```

### 4. for...of문 사용하기
```ts
for(let name of ['Jack', 'Jane', 'Steve'])
	console.log(name);
```

### 5. 제네릭 방식 타입
배열을 다루는 함수를 작성할 때는 number[]과 같이 고정된 타입의 함수를 만들기보다는 T[]형태로 배열의 아이템 타입을 한꺼번에 표현하는 것이 편리하다. 타입을 T와 같은 일종의 변수(타입 변수)로 취급하는 것을 제네릭 타입이라 한다.

제네릭 함수로 구현했으므로 다양한 배열 타입에 모두 정상적으로 대응
```ts
//T가 티입변수라고 알려줘야 한다!
const arrayLength = <T>(array: T[]): number => array.length
const isEmpty = <T>(array: T[]): boolean => arrayLength<T>(array) == 0

let numArray : number[] = [1, 2, 3]
let strArray : string[] = ['Hello', 'World']

type IPesrson = {name: string, age?: number}
let personArray : IPerson[] = [{name: 'Jack'}, {name: 'Jane', age: 32}]

console.log(
	arrayLength(numArray), // 3
	arrayLength(strArray), // 2
	arrayLength(personArray), // 2
	isEmpty([]), // true
	isEmpry([1])  // false
)
```

### 6. 메서드 체인
+ filter: 조건에 맞는 아이템만 추려내기
+ map: 배열 데이터 가공하기

```ts
const multiply = (result, val) => result * val  //07행에서 사용

let numbers1: number[] = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
let result1 = numbers1
    .filter(val => val % 2 != 0)
console.log(result1) 

let numbers2: number[] = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
let result2 = numbers2
    .map(val => val * val)
console.log(result2) 

let numbers: number[] = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
let result3 = numbers
    .reduce(multiply, 1);
console.log(result3) 
```
