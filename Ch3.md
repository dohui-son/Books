# 3장 타입의 모든 것

### 3장 타입의 모든 것

- Topic - 타입스크립트에서 이용 가능한 타입 소개, 각 타입으로 가능한 일

---

- 타입 정의 - 값과 이 값으로 할 수 있는 일의 집합
    - 일에는 연산, 메서드 등을 포함
    - string 타입은 문자열과 문자열에 수행할 수 있는 모든 연산(||, &&, + )과 문자열에 호출할 수 있는 모든 메서드의 집합이다.
    - value의 타입을 알고 있으면, 그 value로 무엇을 하고 무엇을 할 수 없는지 알 수 있음
    - (따라서) 유효하지 않은 동작 실행을 예방 가능
    - “타입 확인자”가 특정 동작 유효성 판단
    
- 타입스크립트의 타입 계층
<img width="713" alt="스크린샷 2023-04-11 오후 8 20 45" src="https://user-images.githubusercontent.com/59991281/231174526-33d72a04-a2eb-4b4f-ae27-a6bba302821e.png">



## 3.1 타입을 이야기하다

```tsx
function squareOf(n : number) {    
	return n * n
}
squareOf(2)    // 4로 평가
squareOf('z')  // [에러] NaN으로 평가했던 것을, 매개변수 n의 타입을 명시하여 에러가 나게 됨
```

- 이처럼 타입 어노테이션을 통해 매개변수 타입 제한 가능
- 경계 개념으로 해석 → 타입스크립트의 타입 계층과 같이 위 함수의 매개변수는 number 이하의 값을 할당 가능
- 타입스크립트는 특정 타입만 와야할 때 이를 명시 / 제한 가능

## 3.2 타입의 가나다

**타입스크립트가 지원하는 타입 / 포함관계 / 수행 가능한 동작 / 언어기능(type alias, union type, intersection type)**

### 3.2.1 any

- 필수불가결한 상황 외에 절대 지양
- 타입스크립트에서는 “컴파일 타임”에 모두 타입이 있어야함

<aside>
💡 꿀팁 - TSC 플래그 : nolmplicitAny

any로 추론되는 값이 나타났을때 예외를 일으키고 싶으면 tsconfig.json파일에서 noImplicitAny 플래그를 활성화해야함. 혹은 strict를 활성화 하면 됨.

</aside>

### 3.2.2 unknown

- 타입을 미리 알 수 없는 어떤 값이 있을 때, any 말고 unkown을 사용해라
- unknown도 any 같이 모든 값을 대표하지만 unknow타입을 검사해서 refine(정제)하기 전까지 타입스크립트가 unknown 타입의 값을 사용 못하도록 force함
- 해당 타입이 지원하는 연산
    - 비교연산 ==, ===, ||, &&, ?
    - 반전연산 !
- 정제
    - typeof
        - `if(typeof unknownTypeValue === ‘number’ ) return 0;`
    - instanceof
- unknown타입과 unknown타입이 아닌 값 비교 가능
- unknown 값이 특정 타입이라는 가정하에 그 특정 타입에서 지원하는 동작 수행 불가능 → 그러려면 이전에 그 값이 특정 타입임을 증명해야함
- union타입에 unknown이 포함 되어있으면 그 타입은 unknown타입이 됨

### 3.2.3 boolean

- true false 두 개의 값을 가지므로 비교 연산과 반전 연산만 지원

```tsx
let a = true // boolean - 어떤 값이 boolean 타입인지 타입스크립트가 추론
var b = false

var c = true

let d: boolean = true // boolean
let e: true = true // true
let f: true = false // 에러 TS2322: 'false' 타입을 // 'true' 타입에 할당할 수 없음
```

- 위와 같이 정의할 경우 내부 동작 방식
    1. 어떤 값이 boolean인지 타입스크립트가 추론하게 한다(a, b).
    2. 어떤 값이 특정 boolean인지 타입스크립트가 추론하게 한다(c).
    3. 값이 boolean임을 명시적으로 타입스크립트에 알린다(d).
    4. 값이 특정 boolean임을 명시적으로 타입스크립트에 알린다(e, f) → 이를 타입 리터럴이라 부름
- 타입 리터럴 type literal - 오직 하나의 값을 나타내는 타입
    - 실수를 방지해 안전성을 추가로 확보해줌
- let이냐 const냐에 따라 타입스크립트가 추론하는 타입이 달라지는 이유? → 6장에 나옴

### 3.2.4 number

- 모든 숫자 집합이 number임
- 정수/소수/양수/음수/Infinity/NaN/-Infinity 등
- 덧셈, 뺄셈, 모둘로%, 비교 연산 등 가능
- 2^53까지 표현 가능

```tsx
let a = 1234             // number
var b = Infinity * 0.10  // number
const c = 5678           // 5678
let d = a < b            // boolean
let e: number = 100      // number
let f: 26.218 = 26.218   // 26.218
let g: 26.218 = 10       // 에러 TS2322: '10' 타입을 '26.218' 타입에 할당할 수 없음
```

- 타입 지정 방식 차이점
    1. 타입스크립트가 값이 number임을 추론하게 한다(a, b)
    2. const를 이용해 타입스크립트가 값이 특정 number임을 추론하게 한다(c)
    3. 값이 number임을 명시적으로 타입스크립트에 알린다(e).
    4. 타입스크립트에 값이 특정 number임을 명시적으로 알린다(f, g).

<aside>
💡 꿀팁 - 숫자 분리자로 긴 숫자 처리시 가독성 높이기

값과 타입에 모두 사용 가능
`let oneMillion = 1_000_000`   // 1000000과 같음
`let twoMillion: 2_000_000 = 2_000_000`

</aside>

### 3.2.5 bigint

- 자바스크립트와 타입스크립트에 새로 추가된 타입
- bigint를 이용하면 라운딩 관련 에러 걱정 없이 큰 정수 처리 가능
- 2^53이상 표현 가능
- 가능하면 타입스크립트가 bigint타입(string 타입도)을 추론하도록 하는 방식이 좋음

```tsx
let a = 1234n         // bigint
const b = 5678n       // 5678n
var c = a + b         // bigint
let d = a < 1235      // boolean
let e = 88.5n         // 에러 TS1353: bigint 리터럴은 반드시 정수여야 함
let f: bigint = 100n  // bigint
let g: 100n = 100n    // 100n
let h: bigint = 100   // 에러 TS2322: '100' 타입은 'bigint' 타입에 할당할 수 없음
```

- 일부 자바스크립트 엔진이 bigint를 자체적으로 지원하지 않으니 플랫폼에서 지원하는지 일부 확인 필요

### 3.2.6 string

- 연결(+)나 문자열 메서드 등의 연산 수행 가능

```tsx
let a = 'hello'           
string var b = 'billy'          
string const c = '!'            // '!'
let d = a + ' ' + b + c  // string
let e: string = 'zoom'   // string
let f: 'john' = 'john'   // 'john'
let g: 'john' = 'zoe'    // 에러 TS2322: "zoe" 타입을 "john" 타입에 할당할 수 없음
```

### 3.2.7 symbol

- symbol은 ES2015에 새로 추가된 기능
- 흔히 쓰이지는 않지만 object와 map에서 문자열 키를 대신하는 용도로 사용
    - symbol키를 사용하는 경우 잘 알려진 키만 사용하도록 force가능 → 키를 잘못 설정하는 문제 미연 방지
- object의 기본 반복자(Symbol.iterator)를 설정하거나 object가 어떤 인스턴스인지(Symbol.hasInstance)를 런타임에 오버라이딩하는 것과 유사한 기능 제공!

```tsx
let a = Symbol('a')          // symbol 
let b: symbol = Symbol('b')  // symbol
var c = a === b      // boolean
let d = a + 'x'      // 에러 TS2469: '+' 연산을 'symbol' 타입에 적용할 수 없음
```

- Symbol(’a’)는 주어진 이름으로 새로운 symbol 만든다는 의미
- symbol은 unique하고 비교 연산 가능 symbol끼리 비고해도 같지 않다고 판단.
- `const falseValue = Symbol(’a’) === Symbol(’a’)`

```tsx
const e = Symbol('e')                 // typeof e
const f: unique symbol = Symbol('f')  // typeof f

let g: unique symbol = Symbol('f')    
// 에러 TS1332: 'unique symbol' 타입은 반드시 'const'여야 함

let h = e === e  // boolean
let i = e === f  
// 에러 TS2367:  'unique symbol' 타입은 서로 겹치는 일이 없으므로 
// 이 비교문의 결과는 항상 'false'
```

- code editor에서 ‘typeof 변수명’형태로 보여주고 타입스크립트가 unique symbol타입으로 추론함
- 타입스크립트 컴파일 타임에 unique symbol이 다른 unique symbol과 같지 않음
- unique symbol도 true, 1, “literal” 등 다른 리터럴 타입과 마찬가지로 특정 symbol을 나타내는 타입

### 3.2.8 객체
