## 1. 지옥에서 온 비동기처리
비동기처리는 JS/TS 차이 없으므로 기본적인 원리 중심으로 작성하겠습니다!

## 개요
+ 비동기처리: 작업의 완료 유뮤와 상관없이 동시에 다음 작업을 수행하는 처리 방식
+ 동기처리: 작업이 반드시 순서대로만 수행되는 처리 방식

### 2. Callback Funcion
+ 콜백함수는 특정 시간에 도달하거나 이벤트가 실행되었을 때 시스템에서 호출하는 함수
```ts
console.log('server1');
setTimeout((): void) =>{
    console.log('server3');
}, 3000);
console.log('server2');
```
+ 하지만 콜백함수를 통한 비동기처리는 코드가 복잡해져 콜백 지옥에 빠지게 되므로 위험하다!

### 3. Promise
+ Promise를 활용한 비동기처리에는 Pending(대기), Fulfilled(이행), Rejected(실패) 3가지 상태가 있다.
```ts
const condition: boolean = true;

const promise = new Promise((resolve, reject) => {
    if (condition) {
        setTimeout(() => {
            resolve('Success');    
        }, 1000);
        // resolve('Success');
    } else {
        reject(new Error('Error! Condition is false'))
    }
});

promise
    .then((resolveData): void => console.log(resolveData))
    .catch(err => console.log(err));

console.log('test')
```
1. new Promise를 통해 promise에 Promise 객체를 할당
2. condition이 true이므로 콜백함수 실행
    + 그 사이 맨 아래 console.log('test')가 찍힐 것임(대기 상태)
    + 1초가 지나 resolve가 실행되면 'Success' 값이 then으로 전달되어 resolveData라는 매개변수로 받음(이행 상태)
    + 만약 condition이 false여서 reject가 실행되었다면, catch가 호출(실패)
    
### 3.1. 연쇄적으로 전달되는 progress
+ 따온 코드인데 코드가 너무 예뻐서.. 하나씩 차례로 따라가 보세요!
```ts
const restaurant = (callback: () => void, time: number) => {
    setTimeout(callback, time);
};

const order = (): Promise<string> => {		// order 객체가 string을 반환하는 Promise 객체라는 뜻이다.
    return new Promise((resolve, reject) => {	// Promise 객체를 반환한다
        restaurant(() => {						// Promise 내부 함수
            console.log('레스토랑 진행 상황 - 음식 주문');
            resolve('음식 주문 시작');			
        }, 1000);
    });
}

const cook = (progress: string): Promise<string> => {	// cook은 string인 progress라는 매개변수를 받아, string을 반환하는 Promise 객체라는 뜻.
    return new Promise((resolve, reject) => {
        console.log('레스토랑 진행 상황 - 음식 조리');
        restaurant(() => {
            resolve(`${progress} -> 음식 조리 중`);	// 전달받은 progress에 텍스트를 붙여 resolve로 반환한다.
        }, 2000);
    });
}

const serving = (progress: string): Promise<string>  => {
    return new Promise((resolve, reject) => {
        console.log('레스토랑 진행 상황 - 음식 서빙');
        restaurant(()=> {
            resolve(`${progress} -> 음식 서빙 중`);
        }, 3000);
    });
}

const eat = (progress: string): Promise<string> => {
    return new Promise((resolve, reject) => {
        console.log('레스토랑 진행 상황 - 음식 먹기');
        restaurant(() => {
            resolve(`${progress} -> 음식 먹는 중`);
        }, 4000);
    });
}

order()	// order() 실행
    .then(progress => cook(progress))	// order 내부에서 resolve가 실행되어 반환되는 값이 progress로 들어간다. progress => cook(progress)는 함수를 then의 매개변수로 넣은 것.
    .then(progress => serving(progress))
    .then(progress => eat(progress))
    .then(progress => console.log(progress));
```

### 4. Async - Await
+ await : 피연산자의 값을 반환. 만일 피연산자가 Promise 객체이면 then 메서드를 호출해 얻은 값을 반환해줍니다. 
```ts
let value = await Promise 객체 혹은 값
```

+ async : 함수는 값을 반환. 이때 반환값은 Promise 형태로 변환되므로 then 메소드를 통해서 반환되는 값을 얻어야 한다.
```ts
const hi = async ()=>{
    return [1, 2, 3];
}

const asyncReturn = async ()=>{
    const result = await hi()
    console.log('value0:', result)
    return result;
}

asyncReturn().then(value => 
    console.log('value1: ', value))
```
+ 몰랐는데, 진짜 몰랐는데........... 제일 외부에서는 then을 통해서 값에 접근할 수 있다고 하네요.....

```ts
value0: [ 1, 2, 3 ]
value1:  [ 1, 2, 3 ]
```





```ts
// 함수 표현식
const asyncFunction1 = async () => {

}

// // 함수 선언식
async function asyncFunction2() {

}
```

### 4.1. 참고!
```ts
let asyncFunc1 = (msg: string): Promise<string> => {
    return new Promise(resolve => {
        setTimeout(() => {
            resolve(`asyncFunc1 - ${msg}`);
        }, 1000);
    });
};

let asyncFunc2 = (msg: string): Promise<string> => {
    return new Promise(resolve => {
        setTimeout(() => {
            resolve(`asyncFunc2 - ${msg}`);
        }, 1500);
    });
};

const asyncMain = async (): Promise<void> => {
    let result = await asyncFunc1('hi');
    console.log(result);
    result = await asyncFunc2('hello');
    console.log(result);
};

const promiseMain = (): void => {
    asyncFunc1('hi').then((result: string) => {
        console.log(result);
        return asyncFunc2('hello');
    }).then((result: string) => {
        console.log(result);
    });
};

asyncMain();

promiseMain();
```