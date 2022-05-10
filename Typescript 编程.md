
# 概述

#### 编译和运行TS

1. TS 源码 -> TS AST
1. 类型检查器检查 AST
1. TS AST -> JS 源码
1. JS 源码 -> JS AST
1. AST -> 字节码
1. 运行时计算字节码

注：1-3 步由 TSC (TS 编译器)操作，4-6 步由浏览器、NodeJS或其他JS引擎中的JS运行时操作。
       JS 编译器和运行时通常聚在一个称为引擎的程序中。引擎包括：

- V8 引擎：驱动 NodeJS、Chrome、Opera
- SpiderMonkey引擎：驱动 Firefox
- JSCore 引擎：驱动 Safari
- Chakra 引擎：驱动 Edge

#### 运行前
```bash
// 安装依赖
yarn add typescript tslint @types/node -D

// 生成 tslint.json
./node_modules/.bin/tslint --init

// 生成 tsconfig.json
./node_modules/.bin/tsc --init

// 运行 ts
./node_modules/.bin/tsc
node dist/index.js

// 编译加上 declarations 标志得到 *.d.ts
./node_modules/.bin/tsc -d *.ts

// 区别
let a = 1; // declare let a: number
const a = 1; // declare const a: 1

// 大型项目使用项目引用，减少类型检查和编译耗时。子项目可以访问各个子项目的声明文件和生成的 Javascript，但是不能访问 Typescript 源文件
// 可以使用 extends 字段减少 tsconfig.json 中的样板代码量
./node_modules/.bin/tsc --build 或 -d
```
```typescript
// 三斜线指令：特殊格式的 Typescript 注释，作用是为 tsc 下达命令。以三条斜线开头，后跟一个可用的 XML 标签，各 XML 标签有一些必须设置的属性
/// <directive attribute="value" />

// 外参变量声明
declare let process: {
  env: {
    NODE_ENV: 'development' | 'production'
  }
}

process = {
  env: {
    NODE_ENV: 'production'
  }
}

// 外参类型声明
// 外参模块声明
```

# 类型

#### typescript 的类型层次


                                    ** unknown**
**                                            |**
**undefined  <-  void  <-  any  ->  null**
**                                            |**
**number  -  bigint  -  boolean  -  string  -  symbol  -  object**
**     |                  |                |                  |                |                |  **
**number           |                |              string      unique       array  -  function  -  constructor**
** enum             |                 |              enum       symbol         |                |                       |**
**     |                 |                 |                  |                |             tuple           |                       |**
**     |                 |                 |                  |                |                |                |                       |**
**                                         never**

备注：

1. **any 是所有类型的超类型，所有类型都是 any 的子类型；**
1. **never 不是任何类型的超类型，never 是所有类型的子类型；**


#### unknown 类型

- Typescript 不会把任何值推导为 unknown 类型，必须显式注解。
- unknown 类型的值可以比较。
- 执行操作时不能假定 unknown 类型为某种特定类型，必须向 Typescript 证明一个值确实是某个类型（使用 typeof，instanceof 等）。
#### any 类型

- 在严格模式下或者在 tsconfig.json 中启用 noImplicitAny 标志情况下，如果想使用 any ，一定要显示注解才不会抛出运行时异常。
#### undefined 类型

- 尚未赋值。
#### null 类型

- 表示缺少值。
#### void 类型

- 表示函数没有显式返回任何值的返回类型（比如 console.log）。
#### never 类型

- 函数根本不返回类型（例如函数抛出异常或者函数永久运行下去）。
#### boolean 类型

- 类型字面量：把类型设置为某个值。
```typescript
let a: true = true; // 类型字面量
const b = true; // 类型字面量(const)
```
#### number 类型

- 处理较长的数字时，可以使用数字分隔符 _ 。
```typescript
let a: 26.828 = 26.828;
```
#### string 类型
#### symbol 类型
```typescript
const a: unique symbol = Symbol('a');
```
#### object 类型
```typescript
// 索引签名 [key: T]
let airPlaneSeating: {
  [seatNumber: string]: string
} = {
  '34D': 'John',
  '34E': 'Bob'
};

// readonly 修饰符
let user: {
  readonly firstName: string
} = {
  firstName: 'John'
};

// 类型别名
type Age = number;
type Person = {
  name: string,
  age: Age
};
let age: Age = 18;
let driver: Person = {
  name: 'John',
  age: age
};
```
#### array 类型
```typescript
let a: string[] = ['a']; // string[]
let b = [1, 'a']; // (string | number)[]

type A = readonly string[];             // readonly string[]
type B = ReadonlyArray<string>;         // readonly string[]
type C = Readonly<[string[]>;           // readonly string[]

type D = readonly [string, number];     // readonly [string, number]
type E = Readonly<[string, number]>;    // readonly [string, number]
```

# 泛型

- **可以约束传参类型和返回类型关系。**
- **泛型参数**使用**尖括号**声明。
- 尖括号的位置限定泛型的作用域。
- 在一对尖括号中可以声明任意个以逗号分隔的泛型参数。
- 按照惯例，人们通常使用单个大写字母，如 T,U,V,W。
- 如果一行中声明的参数过多，或者使用方式较为复杂，可以不遵守这个惯例，使用更具描述性的名称，如 Value，WidgetType。
```typescript
type Filter = {
  <T>(array: T[], f: (item: T) => boolean): T[]
}
let filter: Filter = (array, f) => ...

type Filter<T> = {
  (array: T[], f: (item: T) => boolean): T[]
}
let filter: Filter<number> = (array, f) => ...
type StringFilter = Filter<string>
let stringFilter: StringFilter = (array, f) => ...

function map<T, U>(array: T[], f: (item: T) => U): U[] {
  let result = [];
  for(let i = 0; i < array.length; i++){
    result[i] = f(array[i]);
  }
  return result;
}
```

# 类型进阶

#### 子类型和超类型
子类型：给定两个类型 A 和 B，假设 B 是 A 的子类型，那么在需要 A 的地方都可以放心使用 B 。
超类型：给定两个类型 A 和 B，假设 B 是 A 的超类型，那么在需要 B 的地方都可以放心使用 A 。

#### 型变

- 不变：只能是 T
- 协变：可以是 <: T
- 逆变：可以是 >: T
- 双变：可以是 <: T 或 >: T

备注：

1. 约定 A <: B 指 A 类型为 B 类型的子类型，或者为同种类型。
1. 约定 A >: B 指 A 类型为 B 类型的超类型，或者为同种类型。

#### 类型拓宽

- 如果使用 let 或 var 重新为非拓宽类型赋值，typescript 将自动拓宽。倘若不想拓宽，一开始声明时要显式注解类型。
- 初始化为 null 或 undefined 的变量将自动拓宽为 any。
- typescript 中有个特殊的 const 类型，我们可以使用它在单个声明中禁止类型拓宽。const 不仅能阻止拓宽类型，还将递归把成员设置成 readonly ，不管数据结构的嵌套层级有多深。如果想让 typescript 的推导类型尽量窄一点，请使用 as const。
- 当尝试将一个新鲜对象字面量类型 T 赋值给另一个类型 U 时，如果 T 有不在 U 中的属性，typescript 将做多余属性检查，报错。

备注：

1. 新鲜对象字面量类型指 typescript 从对象字面量推导出来的类型。如果对象字面量有类型断言，或者把对象字面量赋值给变量，那么新鲜对象字面量类型将拓宽为常规的对象类型，也就不能称其为新鲜对象字面量类型。
```typescript
const a = 'x'; // 'x'
let b = a; // string
const c: 'x' = 'x'; // 'x'
let d = c; // 'x'

let a = {x: 3}; // {x: number}
let b = {x: 3} as const; // {readonly x: 3}
let c = [1, {x: 2}] as const; // readonly [1, {readonly x: 2}]
```
#### 
#### 对象类型进阶

- 对象类型
   - “键入”运算符：既然能在对象中查找值，那么也能在结构中查找类型。通过“键入”查找属性的类型时，只能通过方括号表示法。
   - keyof 运算符：获取对象所有键的类型，合并为一个字符串字面量类型。
- Record 类型：typescript 内置的 Record 类型用于描述有映射关系的对象。
- 映射类型
   - 减号运算符：一个特殊的类型运算符，只对映射类型可用。可以把 ? 和 readonly 撤销，分别还原为必须的和可写的。
   - 内置的映射类型
      - Record<Keys, Values>：键的类型为 Keys，值的类型为 Values 的对象。
      - Partial<Object>：把 Object 的每个字段都标记为可选的。
      - Required<Object>：把 Object 的每个字段都标记为必须的。
      - Readonly<Object>：把 Object 的每个字段都标记为只读的。
      - Pick<Object, Keys>：返回 Object 的子类型，只含指定的 Keys。
- 伴生对象模式：即把类型和对象配对在一起。优点是使用方可以一次性导入二者。

#### 函数类型进阶
```typescript
// 改善元组的类型推导
function tuple<
  T extends unknown[]
>(
  ...ts: T
): T {
  return ts;
}
let a = tuple(1, true); // 推导出的类型位置固定 [number, boolean]
let a = [1, true]; // (number | boolean)[]

// 用户定义的类型防护措施
function isString(a: unknown): boolean {
  return typeof a === 'string';
}
function isString(a: unknown): a is string {
  return typeof a === 'string';
}
```
#### 条件类型

- infer 关键字：条件类型的特性之一是可以在条件中声明泛型，不过使用的是 infer 关键字。之前声明泛型参数的方式是使用尖括号。
- 内置的条件类型
   - Exclude<T, U>：计算在 T 中而不在 U 中的类型。
   - Extract<T, U>：计算 T 中可赋值给 U 的类型。
   - NonNullable<T>：从 T 中排出 null 和 undefined。
   - ReturnType<F>：计算函数的返回类型（不适用于泛型和重载的函数）。
   - InstanceType<C>：计算类构造方法的实例类型。
```typescript
// 获取 Array.slice 的第二个参数的类型
type SecondArg<F> = F extends (a: any, b: infer B) => any ? B : never;

type F = typeof Array['prototype']['slice'];
type A = SecondArg<F>; // number | undefined

// Extract<T, U>
type A = number | string;
type B = number;
type C = Extract<A, B>; // number
```
#### 解决办法

- 类型断言：使用 as 句法。
- 非空断言：使用非空断言运算符 !。
- 明确赋值断言：使用 !。

备注：

1. 没有条件向 typescript 证明安全性时使用，尽量少用。

# 函数

```typescript
// 可选参数
function add(a: number, b: number, c?: number): number {
  return a + b + (c ? c : 0);
}

// 默认参数
function add(a: number, b: number, c = 10): number {
  return a + b + c;
}

// 剩余参数
function add(...numbers: number[]): number {
  return numbers.reduce((total, n) => total + n, 0);
}
add(1, 2, 3); // 6

// 注解 this 类型
function fancyDate(this: Date): string {
  return `${this.getDate()}/${this.getMonth() + 1}/${this.getFullYear()}`;
}
fancyDate.call(new Date);

// 调用签名
// a. function log(message: string, userId?: string)
type Log = (message: string, userId?: string) => void;
// b. function sum(...numbers: number[]): number
type Sum = (...numbers: number[]) => number;

// 完整型调用签名
// 以 Log 为例子
type Log = {
  (message: string, userId?: string): void
};

// 函数类型重载
type Reserve = {
  (from: Date, to: Date, destination: string): Reservation
  (from: Date, destination: string): Reservation
};
let reserve: Reserve = (
  from: Date,
  toOrDestination: Date | string,
  destination?: string
) => {
  // code goes
};
```

# 类和接口

#### 访问修饰符
typescript 类中的属性和方法支持三个访问修饰符：

- public -- 任何地方都可以访问，默认级别。
- protected -- 可由当前类及其子类的实例访问。
- private -- 只可由当前类的实例访问。

#### 实现

- 声明类时，可以使用 implements 关键字指明该类满足某个接口。
- 需要实现，不可以修改，用implements，只定义接口。需要具体实现，或者可以被修改，扩展性好，用extends。
- 接口可以声明实例属性，但不能带有可见性修饰符（private, protected, public），也不能使用 static 关键字。另外，像对象类型一样，可以使用 readonly 把实例属性标记为只读。
- 接口不生成 javascript 代码，只存在于编译时。
- 选用？如果多个类共用同一个实现，使用抽象类；如果需要一种轻量的方式表示“这个类是 T 型”，使用接口。
- typescript 根据结构比较类，与类的名称无关。类与其他类型是否兼容，要看结构，如果常规对象定义了同样的属性或方法，也与类兼容。这意味着，如果一个函数接受 Zebra 实例，而我们传入了 Poodle 实例，typescript 可能并不介意，只要两个类的结构一致。

```typescript
// 类型别名
type Food = {
  calories: number,
  tasty: boolean
}
type Sushi = Food & {
  salty: boolean
}
type Cake = Food & {
  sweet: boolean
}
// 接口定义
interface Food {
  calories: number;
  tasty: boolean
}
interface Sushi extends Food {
  salty: boolean
}
interface Cake extends Food {
  sweet: boolean
}

// 一个类不限于只能实现一个接口
interface Animal {
  readonly name: string;
  eat(food: string): void;
  sleep(hours: number): void;
}
interface Feline {
  meow(): void;
}
class Cat implements Animal, Feline {
  name = 'whisper';
  eat(food: string){
    console.log('eat ', food);
  }
  sleep(hours: number){
    console.log('sleep ', hours);
  }
  meow(){
    console.log('meow');
  }
}

// 建造者模式
class RequestBuilder {
  private data: object | null = null;
  private method: 'get' | 'post' | null = null;
  private url: string | null = null;
  
  setMethod(method: 'get' | 'post'): this {
    this.method = method;
    return this;
  }
  setData(data: object): this {
    this.data = data;
    return this;
  }
  setURL(url: string): this {
    this.url = url;
    return this;
  }
  send(){
    // ...
  }
}
```

# 命名空间和模块

- import 可以作为一个语句使用，作用是静态获取代码；也可以作为一个函数调用，此时返回结果是一个 Promise。
- 命名空间与导入和导出不一样，不遵守 tsconfig.json 中的 module 设置，始终编译为全局变量。在中大型项目中，建议全部使用模块，不要使用命名空间。


# 与 Javascript 互操作

#### 使用第三方 Javascript
社区维护的三方包对应的类型声明查询：
[https://www.typescriptlang.org/dt/search](https://www.typescriptlang.org/dt/search)
为无类型的 Javascript 自动生成类型声明：
[https://www.npmjs.com/package/dts-gen](https://www.npmjs.com/package/dts-gen)（生成类型骨架）
