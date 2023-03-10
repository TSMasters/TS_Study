# λ³μ

## πΒ μ μ§μ  νμκ²μ¬

- νμ κ²μ¬λ₯Ό μννλ©΄μ νμμ λ°λΌ νμ μ μΈμ μλ΅μ νμ©νλ€. νμ μ μΈμ μλ΅νλ©΄Β **μμμ (implicit)**
Β νλ³νμ΄ μΌμ΄λλ€.
- νμμ€ν¬λ¦½νΈμμ μ μ§μ  νμ΄νμ μ€λͺν  λ μ μ ν νμμΌλ‘Β **any**
κ° μλ€.
    - **any νμ**μ λͺ¨λ  νμμ μ΅μμ νμμ΄λ©°,Β **any νμ**μΌλ‘ μ μΈλ λ³μλ μ΄λ€ νμμ λ³μλ λ°μλ€μ΄λ©΄μ μ¬μ§μ΄ νμμ΄ μλ λ³μλ λ°μλ€μΈλ€.
    - **TypeScript, Pythonμ΄ λνμ **

## πΒ μλ° μ€ν¬λ¦½νΈμ λμ νμ΄ν

- **κΈ°λ³Έ νμ (primitive types)**: Number, Boolean, String
- **κ°μ²΄ νμ (object tpyes)**: κ°μ²΄ λ¦¬ν°λ΄, λ°°μ΄, λ΄μ₯ κ°μ²΄
- μλ°μ€ν¬λ¦½νΈμμλ νμμ΄ μΆλ‘ λλλ°  **κ°μ λ³μμ ν λΉν  λ νμμ΄ μ ν΄μ§λ κ²**μ λμ  νμ΄ν(dynamic typing)μ΄λΌ νλ€.

## πΒ κΈ°λ³Έ νμ

- string, number, boolean, symbol, enum, λ¬Έμμ΄ λ¦¬ν°λ΄

### λ¬Έμμ΄

```jsx
const str: string = "hello"
```

### μ«μ

```jsx
const num: number = 10;
```

### μ§μκ° μ μΈ

```tsx
let show: boolean = true
```

### Symbol

κ°μ²΄μ keyκ° λ¬Έμμ΄μ΄λ μ μμΌ λμ λ¬Έμ λ, νλ‘κ·Έλλ¨Έμ μ€μλ±μΌλ‘ μ‘΄μ¬νλ κ°μ²΄μ key κ°μ΄ λ³κ²½λ  μ μλ€. Symbolλ‘ μ μν  κ²½μ° ν΄λΉ ν€λ unique νκΈ°μ μ€λ³΅λμ§ μλλ€.

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

### μ΄λ

- μ«μν μ΄λ
    - λ³λμ κ°μ μ§μ νμ§ μμΌλ©΄ μ«μν μ΄λμ΄λ€.
    - λν΄νΈλ‘ 0μ΄ μ§μ λλ€.

```tsx

enum Shoes{

    Nike,
    Adidas
}

var myshoes=Shoes.Nike //0
Shoes.Adidas//1
```

- λ¬Έμν μ΄λ
    
    ```tsx
    enum Shoes{
    
        Nike='λμ΄ν€',
        Adidas='μλλ€μ€'
    }
    
    var myshoes=Shoes.Nike //μ΄λ 'λμ΄ν€'κ° μΆλ ₯
    ```
    
- μμ 
    
    ```tsx
    enum Answer{
        Yes='Y',
        No='N'
    }
    
    function askQuestion(answer:Answer){
    
        if(answer===Answer.Yes){
            console.log('μ λ΅μλλ€')
        }
        if(answer===Answer.No){
            console.log('μ€λ΅μλλ€')
        }
    
    }
    
    askQuestion("μμ€") //μλ¨
    //μ΄λμμ μ§μνλ λ°μ΄ν°λ§ λ€μ΄κ° μ μμ
    askQuestion(Answer.Yes)
    ```
    

## πΒ κ°μ²΄ νμ

### νν μ μΈ

```jsx
let address: [string, number] = ['gangnam', 100]
```

- λ°°μ΄μ μΈλ±μ€ νμκΉμ§ μ μνλ€
- λ°°μ΄μ κΈΈμ΄κ° κ³ μ λκ³ , κ° μμμ νμμ΄ μ§μ λμ΄ μλ λ°°μ΄ νμμ΄λΌκ³  λ³΄λ©΄λλ€.

### λ°°μ΄ μ μΈ

```jsx
const arr: Array<number> = [1, 2, 3]
const heroes: Array<string> = ['capt', 'Thor', 'Hulk']
// javaκ°μ λλμ μ²¨κ°
const items: number[] = [1, 2, 3]
```

### κ°μ²΄ μ μΈ

```tsx

let obj: object = {}
// κ°μ²΄ μμ μ’ λ μ νν νμμ§μ 
let person: { name: string, age: number } = {
    name: 'capt',
    age: 100
}

```

## μ°Έκ³ μλ£

[https://velog.io/@denmark-banana/TypeScript-λ³μ-μ μΈκ³Ό-κΈ°λ³Έ-νμ](https://velog.io/@denmark-banana/TypeScript-%EB%B3%80%EC%88%98-%EC%84%A0%EC%96%B8%EA%B3%BC-%EA%B8%B0%EB%B3%B8-%ED%83%80%EC%9E%85)
