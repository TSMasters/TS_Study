### 1. 클래스
+ class, private, public, protected, implements, extend 키워드 제공

### 2. 멤버 변수 미리 선언하기
```ts
class Car{
    color: string; //멤버 변수 미리 선언
    constructor(color:string){
        this.color = color;
    }

    // constructor(public color: string){}

    start() {
        console.log('start');
    }
}

const bmw = new Car('red');
```

### 3. 접근 제한자 Access Modifier - public, private, protected
1. public
+ 자식 클래스 내부에서 사용 가능
+ 클래스 인스턴스에서는 참조 가능
2. private
+ 자식 클래스 내부에서 사용 불가능
+ 해당 클래스 내부에서만 접근 가능 
3. protected
+ 자식 클래스 내부에서 사용 가능
+ 클래스 인스턴스에서는 참조 불가능

```ts
class Car{
    name: string = "car";
    // private name: string = "car"; 
    color: string;
    constructor(color:string){
        this.color = color;
    }
    start() {
        console.log('start');
        console.log(this.#name);
    }
}

class Bmw extends Car{
    constructor(color: string){
        super(color);
    }
    showName(){
        console.log(this.name);
    }
}

const z4 = new Bmw('black');
console.log(z4.name); //public OK, Private NO
```

### 4. 인터페이스 구현 implements
+ 인터페이스는 이러이러한 속성이 있어야 한다는 규약에 불과할 뿐 물리적으로 해당 속성을 만들지 않음
+ 따라서, 클래스 몸통에는 반드시 인터페이스가 정의하고 있는 속성을 멤버 속성으로 포함해야 함
```ts
interface IPerson{
    name: string
    age: number
}
class person implements Iperson{
    name: string
    age: number
}
```

### 5.읽기 전용 readonly
```ts
class Car{
    readonly name: string = "car";
    color: string;
    constructor(color:string, name: string){
        this.color = color;
        this.name = name;
    }
    start() {
        console.log('start');
        console.log(this.name);
    }
}

class Bmw extends Car{
    constructor(color: string, name: string){
        super(color, name);
    }
    showName(){
        console.log(this.name);
    }
}

const z4 = new Bmw('black', '여기서 바꿔야지');
console.log(z4.name); //public OK, Private NO
// z4.name = "rename"; //OK, readonly NO
```

### 6.정적 멤버 변수 static
```ts
class Car{
    name: string = "car";
    color: string;
    static wheels = 4; //
    constructor(color:string, name: string){
        this.color = color;
        this.name = name;
    }
    start() {
        console.log('start');
        console.log(this.name);
        console.log(Car.wheels);
        // console.log(this.wheels); //Error

    }
}

class Bmw extends Car{
    constructor(color: string, name: string){
        super(color, name);
    }
    showName(){
        console.log(this.name);
    }
}

console.log(Car.wheels);
// console.log(this.wheels); //Error
```

### 7. 추상클래스 class
```ts
abstract class Car{
    color: string;
    constructor(color:string){
        this.color = color;
    }
    start() {
        console.log('start');
    }
    abstract doSomething(): void; //부모에선 정의만
}

//const car = new Car('red') //Error

class Bmw extends Car{
    constructor(color: ){
        super(color);
    }
    doSomething(){
        alert(3);
    }
}

const z4 = new Bmw('black');
```