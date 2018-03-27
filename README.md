这是一个学习typescript的笔记（摘抄o(^▽^)o）
===
# 使用TypeScript开发微信小程序（1）——基础：数据类型（Type） 

TypeScript 的基本数据类型有boolean、number、string 、array、enum、any、void

## Boolean（布尔值）
---

最基本的数据类型就是简单的 true / false 值，称之为“布尔值”。

``` javascript
let isDone: boolean = false;    
console.log(isDone);
```

## Number（数字）
---

TypeScript里的所有数字都是浮点数。这些浮点数的类型是 number。 除了支持十进制和十六进制字面量，Typescript还支持ECMAScript 2015中引入的二进制和八进制字面量。

``` javascript
let decimal: number = 6;    console.log(decimal); // 输出：6
let hex: number = 0xf00d;    console.log(hex); // 输出：61453
let binary: number = 0b1010;    console.log(binary); // 输出：10
let octal: number = 0o744;    console.log(octal); // 输出：484
```
## String（字符串）
---

可以使用双引号（ “）或单引号（’）表示字符串。

``` javascript
let color: string = "blue";
color = 'red';    console.log(color);    let fullName: string = `Bob Bobbington`;    console.log(fullName);
```
可以使用模版字符串，它可以定义多行文本和内嵌表达式。 这种字符串是被反引号包围（ ` ），并且以${ expr }这种形式嵌入表达式。

``` javascript
let name: string = "John"
let age: number = 37;
let sentence: string = `Hello, my name is ${name}.

    I'll be ${ age + 1} years old next month.`
console.log(sentence);
```

## Array（数组）
---

有两种方式可以定义数组。 第一种，可以在元素类型后面接上 []，表示由此类型元素组成的一个数组：

``` javascript
let list: number[] = [1, 2, 3];    console.log(list);
```

第二种方式是使用数组泛型，Array<元素类型>：

``` javascript
let genericList: Array<number> = [1, 2, 3];    console.log(genericList);
```

## Tuple（元组）
---

元组类型允许表示一个已知元素数量和类型的数组，各元素的类型不必相同。 比如，可以定义一对值分别为 string和number类型的元组。

``` javascript
let x: [string, number];
x = ["hello", 10];    // x = [10, "hello"]; // Error
```

当访问一个已知索引的元素，会得到正确的类型：

``` javascript
let y: [string, number] = ["hello", 10];    console.log(y[0].substr(1)); // 
// console.log(y[1].substr(1)); // Error
```

当访问一个越界的元素，会使用联合类型替代：

``` javascript
let z: [string, number] = ["hello", 10];
z[3] = "world";    console.log(z[3]);    
console.log(z[3].toString());    
// x[6] = true; // Error
```

## Enum（枚举）
---

使用枚举可以定义一些有名字的数字常量。 枚举通过 enum关键字来定义。enum类型是对JavaScript标准数据类型的一个补充。 像C#等其它语言一样，使用枚举类型可以为一组数值赋予友好的名字。

``` javascript
enum Color1 { Red, Green, Blue };    let c1: Color1 = Color1.Green;    console.log(c1);
```

默认情况下，从0开始为元素编号。 也可以手动的指定成员的数值。

``` javascript
enum Color2 { Red = 1, Green, Blue };    let c2: Color2 = Color2.Green;    console.log(c2);
```

或者，全部都采用手动赋值：

``` javascript
enum Color3 { Red = 1, Green = 2, Blue = 4 };    let c3: Color3 = Color3.Green;    console.log(c3);
```

枚举类型提供的一个便利是可以由枚举的值得到它的名字。

``` javascript
enum Color4 { Red = 1, Green, Blue };    let colorName: string = Color4[2];    console.log(colorName);
```

## Any（任意值）
---

有时候想要为那些在编程阶段还不清楚类型的变量指定一个类型。 这些值可能来自于动态的内容，比如来自用户输入或第三方代码库。 这种情况下，不希望类型检查器对这些值进行检查而是直接让它们通过编译阶段的检查。 那么可以使用 any 类型来标记这些变量：

``` javascript
let notSure: any = 4;
notSure = "maybe a string instead";    
console.log(notSure);
notSure = false;    console.log(notSure);
```

在对现有代码进行改写的时候，any 类型是十分有用的，它允许在编译时可选择地包含或移除类型检查。 Object类型的变量只是允许给它赋任意值（但是却不能够在它上面调用任意的方法，即便它真的有这些方法）

当只知道一部分数据的类型时，any 类型也是有用的。

``` javascript
let list: any[] = [1, true, "free"];
list[1] = 100;    console.log(list);
```

## Void（空值）
---

void 表示没有任何类型。 当一个函数没有返回值时，通常会见到其返回值类型是 void：

``` javascript
function warnUser(): void {        
    console.log("This is my warning message");
}
warnUser();
```

声明一个void类型的变量没有什么大用，因为你只能为它赋予undefined和null：

``` javascript
let unusable: void = undefined;
```

## Null 和 Undefined
---

TypeScript里，undefined 和 null 两者各自有自己的类型分别叫做 undefined 和 null。 和 void 相似，它们的本身的类型用处不是很大：

``` javascript
let u: undefined = undefined;    let n: null = null;    console.log(u === n); // 输出：false
```

默认情况下 null 和 undefined 是所有类型的子类型。 就是说可以把 null 和 undefined 赋值给 number 类型的变量。

然而，当指定了—strictNullChecks 标记，null 和 undefined 只能赋值给 void 和它们各自。

## Never
---

never 类型表示的是那些永不存在的值的类型。 例如， never 类型是那些总是会抛出异常或根本就不会有返回值的函数表达式或箭头函数表达式的返回值类型； 变量也可能是 never类型，当它们被永不为真的类型保护所约束时。

never 类型是任何类型的子类型，也可以赋值给任何类型；然而，没有类型是 never 的子类型或可以赋值给 never 类型（除了 never 本身之外）。 即使 any 也不可以赋值给 never。

``` javascript
// 返回never的函数必须存在无法达到的终点
function error(message: string): never {        throw new Error(message);
}    // 推断的返回值类型为never
function fail() {        return error("Something failed");
}    // 返回never的函数必须存在无法达到的终点
function infiniteLoop(): never {        while (true) {
    }
}
```

# 使用TypeScript开发微信小程序（2）——基础：变量（Variable） 

一直以来 JavaScript 都是通过 var 关键字定义变量。let 和 const 是 JavaScript 里相对较新的变量声明方式。 let 在很多方面与 var 是相似的，但是可以避免在JavaScript里常见一些问题。 const是对let的一个增强，它能阻止对一个变量再次赋值。

## var
---

var 多次声明同一个变量并不会报错。

``` javascript
var a = 123;    var a = 456;     
console.log(a); // 输出：456
```

var 声明的变量作用域是最近的函数作用域

``` javascript
var b = [];    for (var i = 0; i < 10; i++) {
    b[i] = function () {            
        console.log(i);
    };
}    
console.log(b[6]()); // 输出：10
```

以上代码中，变量 i 是 var 声明的，在全局范围内都有效。所以每一次循环，新的i值都会覆盖旧值，导致最后输出的是最后一轮的i的值。

## let
---

ES6新增let和const两个变量声明命令，都具有以下特性：

- 块局作用域；
- 不存在变量提升，一定声明后才能使用；
- 暂时性死区，在代码块内使用let命令声明变量之前，该变量都是不可用的，不受外部变量影响；
- 在相同作用域范围内不允许重复声明

``` javascript
let a = 123;    // let a = 123; // Error
console.log(a);  // 输出：123
```

let 声明的变量作用域是最接近的块作用域

``` javascript
let b = [];    
for (let i = 0; i < 10; i++) {
    b[i] = function () {            
        console.log(i);
    };
}
b[6](); // 输出：6
```

以上代码中，变量 i 是 let 声明的，当前的 i 只在本轮循环有效，所以每一次循环的i其实都是一个新的变量，所以最后输出的是 6。

## const
---

const 声明是声明变量的另一种方式。与 let 声明相似，但是就像它的名字所表达的，它们被赋值后不能再改变。 const 与 let 不同点在于：

- const 如果声明的变量是简单的值，则不能改变变量的值，修改会报错；
- const 如果声明的是复合类型的变量，则只保证变量地址不变，值可以变；
const 声明的常量，不可重复声明，如果 const 声明的变量是简单的值，则不能改变变量的值。

``` javascript
const YEAR = 2017;    console.log(YEAR);    // YEAR = 2016; // Error
// const YEAR = 3.1; // Error
```

对于 const 声明的复合类型的变量，变量名不指向数据，而是指向数据所在的地址。const 命令只是保证变量名指向的地址不变，并不保证该地址的数据不变。

``` javascript
const foo = { prop: 123 };
foo.prop = 456;    console.log(foo.prop) // 输出：456
// foo = { prop: 789 }; // Error
```

## 解构
---

### 解构数组

``` javascript
let input = [1, 2];    let [first, second] = input;    console.log(first); // 输出：1
console.log(second); // 输出：2
```

可以在数组里使用…语法创建剩余变量：

``` javascript
let [first, ...rest] = [1, 2, 3, 4];    console.log(first); // 输出：1
console.log(rest); // 输出：[ 2, 3, 4 ]
```

### 对象解构

``` javascript
let o = { a: "foo", b: 12, c: "bar" }    let { a, b } = o;    console.log(a); // 输出：foo
console.log(b); // 输出：12
```

在对象里使用…语法创建剩余变量：

``` javascript
let o = { a: "foo", b: 12, c: "bar" }    let { a, ...passthrough } = o;    let total = passthrough.b + passthrough.c.length;    console.log(total); // 输出：15
```

## 展开
---

展开操作符正与解构相反。 它允许你将一个数组展开为另一个数组，或将一个对象展开为另一个对象。

``` javascript
let first = [1, 2];    let second = [3, 4];    let bothPlus = [0, ...first, ...second, 5];    console.log(bothPlus); // 输出：[0, 1, 2, 3, 4, 5]
```

# 使用TypeScript开发微信小程序（3）——基础：函数（Function） 

函数是 JavaScript 应用程序的基础， 它可以实现抽象层，模拟类，信息隐藏和模块。 在TypeScript里，虽然已经支持类，命名空间和模块，但函数仍然是主要的定义行为的地方，TypeScript 为JavaScript 函数添加了额外的功能，更容易地使用。 

## 函数
---
和JavaScript一样，TypeScript函数可以创建有名字的函数和匿名函数。 可以随意选择适合应用程序的方式，不论是定义一系列API函数还是只使用一次的函数。

``` javascript
function add1(x: number, y: number): number {
    return x + y;
}    
let add2 = function (x: number, y: number): number { return x + y; };
```

### 完整函数类型

``` javascript
let sub1: (x:number, y:number) => number =        function(a: number, b: number): number { return x-y; };
```

函数类型包含两部分：参数类型和返回值类型。 当写出完整函数类型的时候，这两部分都是需要的。 只要参数类型是匹配的，那么就认为它是有效的函数类型，而不在乎参数名是否正确。对于返回值， 在函数和返回值类型之前使用( =>)符号，使之清晰明了。返回值类型是函数类型的必要部分，如果函数没有返回任何值，你也必须指定返回值类型为 void 而不能留空。

### 推断类型

``` javascript
let sub2 = function(a: number, b: number): number { return a - b; };
```

如果在赋值语句的一边指定了类型但是另一边没有类型的话，TypeScript编译器会自动识别出类型。

## 可选参数
---

TypeScript里的每个函数参数都是必须的。 这不是指不能传递 null或undefined作为参数，而是说编译器检查用户是否为每个参数都传入了值。 编译器还会假设只有这些参数会被传递进函数。 简短地说，传递给一个函数的参数个数必须与函数期望的参数个数一致。

在TypeScript里我们可以在参数名旁使用 ? 实现可选参数的功能。

``` javascript
function buildName1(firstName: string, lastName?: string) {
    if (lastName)
        return firstName + " " + lastName;        
    else
        return firstName;
}    
console.log(buildName1("Bob")); // 输出：Bob
// console.log(buildName1("Bob", "Adams", "Sr.")); // Error
console.log(buildName1("Bob", "Adams")); // 输出：Bob Adams
```

可选参数必须跟在必须参数后面。

## 默认参数
---

在TypeScript里，可以为参数提供一个默认值当用户没有传递这个参数或传递的值是 undefined 时，它们叫做有默认初始化值的参数。

``` javascript
 function buildName2(firstName: string, lastName = "Smith") {
        return firstName + " " + lastName;
    }
    console.log(buildName2("Bob")); // 输出：Bob Smith
    console.log(buildName2("Bob", undefined)); // 输出：Bob Smith
    // console.log(buildName2("Bob", "Adams", "Sr.")); // Error
    console.log(buildName2("Bob", "Adams")); // 输出：Bob Adams
```

在所有必须参数后面的带默认初始化的参数都是可选的，与可选参数一样，在调用函数的时候可以省略。 也就是说可选参数与末尾的默认参数共享参数类型。

与普通可选参数不同的是，带默认值的参数不需要放在必须参数的后面。 如果带默认值的参数出现在必须参数前面，用户必须明确的传入 undefined值来获得默认值。

## 剩余参数
---

必要参数，默认参数和可选参数有个共同点：它们表示某一个参数。 如果同时操作多个参数，或者并不知道会有多少参数传递进来。 在JavaScript里，可以使用 arguments 来访问所有传入的参数。

在 TypeScript 里，可以把所有参数收集到一个变量里：

``` javascript
function buildName3(firstName: string, ...restOfName: string[]) {        return firstName + " " + restOfName.join(" ");
}    console.log(buildName3("Joseph", "Samuel", "Lucas", "MacKinzie")); // 输出：Joseph Joseph Lucas Lucas
```

剩余参数会被当做个数不限的可选参数。 可以一个都没有，同样也可以有任意个。 编译器创建参数数组，名字是在省略号（ …）后面给定的名字，可以在函数体内使用这个数组。这个省略号（ …）也会在带有剩余参数的函数类型定义上使用到。

## 箭头函数
---
箭头函数（lambda表达式）是 ES6 在语法上提供的一个很好的特性。
``` javascript
var array = [1, 2, 3];
array.forEach(v => console.log(v)); // 输出: 1 2 3
```
普通 function 函数和箭头函数的行为有区别，箭头函数没有它自己的 this 值，箭头函数内的 this 值继承自外围作用域。
``` javascript
var array = [1, 2, 3];
array.forEach(v => console.log(v)); // 输出: 1 2 3

class Handler {
    message: string;
    change = (m: string) => { this.message = m };
    output = () => { console.log(this.message) };
}    let handler = new Handler();
handler.change("Hello");
handler.output();
```

## 重载
---
JavaScript 本身是个动态语言，JavaScript 里函数根据传入不同的参数而返回不同类型的数据是很常见的。可以为同一个函数提供多个函数类型定义来进行函数重载， 编译器会根据这个列表去处理函数的调用。
``` javascript
class FooClass {
    public bar(s: string): number;
    public bar(n: number): string;
    public bar(arg: string | number): any {
        if (typeof arg === 'number')
            return arg.toString();            
        if (typeof arg === 'string')
            return arg.length;
    }
}
```
为了让编译器能够选择正确的检查类型，它与JavaScript里的处理流程相似。 它查找重载列表，尝试使用第一个重载定义。 如果匹配的话就使用这个。 因此，在定义重载的时候，一定要把最精确的定义放在最前面。

# 使用TypeScript开发微信小程序（4）——类（Class） 

## 类
---
TypeScript 使用类：
``` typescript
class Greeter {
    greeting: string;
    constructor(message: string) {
        this.greeting = message;
    }
    greet() {            
        console.log("Hello, " + this.greeting);
    }
}    
let greeter = new Greeter("world");
greeter.greet();
```
以上代码，声明一个 Greeter类。这个类有3个成员：一个叫做greeting的属性，一个构造函数和一个greet方法。在引用任何一个类成员的时候都用了this。 它表示我们访问的是类的成员。 使用new构造了Greeter类的一个实例，它会调用之前定义的构造函数，创建一个 Greeter类型的新对象，并执行构造函数初始化它。

## 构造函数
---
在TypeScript里，构造函数在使用 new 创建类实例的时候被调用。
``` javascript
class Greeter {
    static standardGreeting = "Hello, there";
    greeting: string;
    greet() {
        if (this.greeting) {
            return "Hello, " + this.greeting;
        }            
        else {
            return Greeter.standardGreeting;
        }
    }
}    
let greeter1: Greeter;
greeter1 = new Greeter();    
console.log(greeter1.greet());    
let greeterMaker: typeof Greeter = Greeter;
greeterMaker.standardGreeting = "Hey there!";    let greeter2: Greeter = new greeterMaker();    console.log(greeter2.greet());
```
以上代码，创建了一个叫做 greeterMaker 的变量。 这个变量保存了这个类或者说保存了类构造函数。 然后我们使用 typeof Greeter，意思是取Greeter类的类型，而不是实例的类型。 或者更确切的说，”告诉我 Greeter标识符的类型”，也就是构造函数的类型。 这个类型包含了类的所有静态成员和构造函数，在 greeterMaker 上使用 new，创建 Greeter 的实例。

## 继承
---
在TypeScript里，基于类的程序设计中最基本的模式是允许使用继承来扩展现有的类。
``` typescript
class Animal {
    name: string;
    constructor(theName: string) { this.name = theName; }
    move(distanceInMeters: number = 0) {            console.log(`${this.name} moved ${distanceInMeters}m.`);
    }
}    
class Snake extends Animal {
    constructor(name: string) { super(name); }
    move(distanceInMeters = 5) {            console.log("Slithering...");
        super.move(distanceInMeters);
    }
}    
class Horse extends Animal {
    constructor(name: string) { super(name); }
    move(distanceInMeters = 45) {            console.log("Galloping...");
        super.move(distanceInMeters);
    }
}    
let sam = new Snake("Sammy the Python");    
let tom: Animal = new Horse("Tommy the Palomino");
sam.move();
tom.move(34);
```

## 公共，私有与受保护的修饰符
---
在TypeScript里，成员都默认为 public，使用 public 指定成员是可见的。

当成员被标记成private时，它就不能在声明它的类的外部访问。

protected修饰符与private修饰符的行为很相似，但有一点不同，protected成员在派生类中仍然可以访问。

``` typescript
class Person {
    private id: string;
    public age: number;
    protected name: string;

    constructor(name: string) { 
        this.id = "123";            
        this.name = name; 
    }
}    
class Employee extends Person {
    private department: string;

    constructor(name: string, department: string) {
        super(name);            
        // this.id = "456"; // Error
        this.department = department;
    }

    public getElevatorPitch() {            
        return `Hello, my name is ${this.name} and I work in ${this.department}.`;
    }
}    
let howard = new Employee("Howard", "Sales");
howard.age = 42;  
console.log(howard.age);  
// console.log(howard.id); // Error
// console.log(howard.name); // Error
console.log(howard.getElevatorPitch());
```

## readonly修饰符
---
在TypeScript里，可以使用readonly关键字将属性设置为只读的。 只读属性必须在声明时或构造函数里被初始化。
``` typescript
class Octopus {
    readonly name: string;
    readonly numberOfLegs: number = 8;
    constructor(theName: string) {            this.name = theName;
    }
}    
let dad = new Octopus("Man with the 8 strong legs");
//dad.name = "Man with the 3-piece suit"; // Error
```

## 存取器
---
TypeScript 支持通过 getters/setters 来截取对对象成员的访问。 它能帮助你有效的控制对对象成员的访问。
``` typescript
let passcode = "secret passcode";   
 class User {
    private _name: string;

    get name(): string {
        return this._name;
    }

    set name(newName: string) {
        if (passcode && passcode == "secret passcode") {
            this._name = newName;
        }
        else {
            console.log("Error: Unauthorized update of employee!");
        }
    }
}    
let user = new User();
user.name = "admin";    
if (user.name) {
    console.log(user.name);
}
```
存取器要求你将编译器设置为输出 ECMAScript 5 或更高。 不支持降级到 ECMAScript 3。 只带有 get 不带有 set 的存取器自动被推断为readonly。 这在从代码生成 .d.ts文件时是有帮助的，因为利用这个属性的用户会看到不允许够改变它的值。

## 静态属性
---
在TypeScript 里，可以创建类的静态成员，这些属性存在于类本身上面而不是类的实例上。
``` typescript
 class Grid {
    static origin = { x: 0, y: 0 };
    calculateDistanceFromOrigin(point: { x: number; y: number; }) {
        let xDist = (point.x - Grid.origin.x);
        let yDist = (point.y - Grid.origin.y); 
        return Math.sqrt(xDist * xDist + yDist * yDist) / this.scale;
    }
    constructor(public scale: number) { }
}    
let grid1 = new Grid(1.0); 
let grid2 = new Grid(5.0);    
console.log(grid1.calculateDistanceFromOrigin({ x: 10, y: 10 }));    
console.log(grid2.calculateDistanceFromOrigin({ x: 10, y: 10 }));
```

## 抽象类
---
在TypeScript里，抽象类做为其它派生类的基类使用。 它们一般不会直接被实例化。 不同于接口，抽象类可以包含成员的实现细节。 abstract 关键字是用于定义抽象类和在抽象类内部定义抽象方法。
``` typescript
 abstract class Department {
    constructor(public name: string) {
    }
    printName(): void {
        console.log('Department name: ' + this.name);
    }
    abstract printMeeting(): void; 
}
class AccountingDepartment extends Department {
    constructor() {
        super('Accounting and Auditing'); 
    }
    printMeeting(): void {
        console.log('The Accounting Department meets each Monday at 10am.');
    }
    generateReports(): void {
        console.log('Generating accounting reports...');
    }
} 
let department: Department;    
// department = new Department(); // Error
department = new AccountingDepartment();
department.printName();
department.printMeeting();    
// department.generateReports(); // Error
```
抽象类中的抽象方法不包含具体实现并且必须在派生类中实现。 抽象方法的语法与接口方法相似。 两者都是定义方法签名但不包含方法体。 然而，抽象方法必须包含 abstract 关键字并且可以包含访问修饰符。

# 使用TypeScript开发微信小程序（5）——接口（Interface） 
TypeScript核心设计原则之一就是类型检查，通过使用接口(Interfaces)可以进行类型检查，满足传统面向对象思想，利于有效开发，有效避免类型转换问题。

在 TypeScript 中，接口的作用就是为类型命名和为代码或第三方代码定义契约。 在编译成 JavaScript 的时候，所有的接口都会被擦除掉，因为 JavaScript 中并没有接口这一概念。

## 接口
---
TypeScript 使用接口：
``` typescript
interface LabelledValue {
    label: string;
}    
function printLabel(labelledObj: LabelledValue) {        console.log(labelledObj.label);
}
let myObj = { size: 10, label: "Size 10 Object" };
printLabel(myObj);
```
以上代码，LabelledValue 接口就好比一个名字，用来描述要求。 它代表了有一个 label 属性且类型为 string 的对象。 并不能像在其它语言里一样，传给 printLabel 的对象实现了 LabelledValue 接口，我只会去关注值的外形。 只要传入的对象满足上面提到的必要条件，那么它就是被允许的。

类型检查器不会去检查属性的顺序，只要相应的属性存在并且类型也是对的就可以。

## 可选属性
---
接口里的属性不全都是必需的。 有些是只在某些条件下存在，或者根本不存在。 可选属性在应用“option bags”模式时很常用，即给函数传入的参数对象中只有部分属性赋值了。
```typescript
interface SquareConfig {
    color?: string;
    width?: number;
}
function createSquare(config: SquareConfig): { color: string; area: number } {
    let newSquare = { color: "white", area: 100 };
    if (config.color) {
        newSquare.color = config.color;
    }
    if (config.width) {
        newSquare.area = config.width * config.width;
    }
    return newSquare;
}
let mySquare = createSquare({ color: "black" });
```
带有可选属性的接口与普通的接口定义差不多，只是在可选属性名字定义的后面加一个?符号。

可选属性的好处之一是可以对可能存在的属性进行预定义，好处之二是可以捕获引用了不存在的属性时的错误。

## 只读属性
---
一些对象属性只能在对象刚刚创建的时候修改其值。 你可以在属性名前用 readonly 来指定只读属性:
``` typescript
interface Point {
    readonly x: number;
    readonly y: number;
}
let p1: Point = { x: 10, y: 20 };
// p1.x = 5; // Error
```
以上代码，通过赋值一个对象字面量来构造一个Point。 赋值后， x和y再也不能被改变了。

TypeScript具有ReadonlyArray类型，它与Array相似，只是把所有可变方法去掉了，因此可以确保数组创建后再也不能被修改：
``` typescript
let a: number[] = [1, 2, 3, 4];
let ro: ReadonlyArray<number> = a;
// ro[0] = 12; // Error
// ro.push(5); // Error
// ro.length = 100; // Error
// a = ro; // Error
```
最简单判断该用readonly还是const的方法是看要把它做为变量使用还是做为一个属性。 做为变量使用的话用 const，若做为属性则使用readonly。

## 额外的属性检查
---
当将对象字面量赋值给变量或作为参数传递的时候，对象字面量会被特殊对待而且会经过额外属性检查。 如果一个对象字面量存在任何“目标类型”不包含的属性时，会得到一个错误。

绕开这些检查非常简单。 最简便的方法是使用类型断言。然而，最佳的方式是能够添加一个字符串索引签名，前提是能够确定这个对象可能具有某些做为特殊用途使用的额外属性。 如果 SquareConfig 带有 color 和 width 属性，并且还会带有任意数量的其它属性。
```typescript
interface SquareConfig2 {
    color?: string;
    width?: number;
    [propName: string]: any;
}
let squareOptions2 = { colour: "red", width: 100 }; let mySquare2 = createSquare(squareOptions2);
```

## 函数类型
---
接口能够描述JavaScript中对象拥有的各种各样的外形。 除了描述带有属性的普通对象外，接口也可以描述函数类型。

为了使用接口表示函数类型，需要给接口定义一个调用签名。 它就像是一个只有参数列表和返回值类型的函数定义。参数列表里的每个参数都需要名字和类型。
```typescript
interface SearchFunc {
    (source: string, subString: string): boolean;
}
```
这样定义后，可以像使用其它接口一样使用这个函数类型的接口。
```typescript
let mySearch: SearchFunc;
mySearch = function (src, sub) {
    let result = src.search(sub);
    if (result == -1) {
        return false;
    }
    else {
        return true;
    }
}
```
对于函数类型的类型检查来说，函数的参数名不需要与接口里定义的名字相匹配。函数的参数会逐个进行检查，要求对应位置上的参数类型是兼容的。 如果不想指定类型，Typescript 的类型系统会推断出参数类型，因为函数直接赋值给了 SearchFunc 类型变量。 函数的返回值类型是通过其返回值推断出来的。 如果让这个函数返回数字或字符串，类型检查器会警告函数的返回值类型与 SearchFunc 接口中的定义不匹配。

## 可索引的类型
---
与使用接口描述函数类型差不多，也可以描述那些能够“通过索引得到”的类型，可索引类型具有一个 索引签名，它描述了对象索引的类型，还有相应的索引返回值类型。
```typescript
interface StringArray {
    [index: number]: string;
}
let myArray: StringArray;
myArray = ["Bob", "Fred"];
let myStr: string = myArray[0];
```
以上代码，定义了StringArray接口，它具有索引签名。 这个索引签名表示了当用 number 去索引 StringArray 时会得到 string 类型的返回值。

共有支持两种索引签名：字符串和数字。 可以同时使用两种类型的索引，但是数字索引的返回值必须是字符串索引返回值类型的子类型。 这是因为当使用 number来索引时，JavaScript 会将它转换成 string 然后再去索引对象。 也就是说用 100（一个 number）去索引等同于使用”100”（一个 string ）去索引，因此两者需要保持一致。

字符串索引签名能够很好的描述 dictionary 模式，并且它们也会确保所有属性与其返回值类型相匹配。 因为字符串索引声明了 obj.property 和 obj[“property”] 两种形式都可以。

可以将索引签名设置为只读，这样就防止了给索引赋值。

```typescript
interface NumberDictionary {
    [index: string]: number;
    length: number;        // name: string // Error
}    
interface ReadonlyStringArray {
    readonly[index: number]: string;
}
```

## 实现接口
---
与C#或Java里接口的基本作用一样，TypeScript 也能够用它来明确的强制一个类去符合某种契约。可以在接口中描述一个方法，在类里实现它。
```typescript
interface IClock {
    currentTime: Date;
    setTime(d: Date): void;
}    
class Clock implements IClock {
    currentTime: Date;
    setTime(d: Date) {            
        this.currentTime = d;
    }
    constructor(h: number, m: number) { }
}
```
当用构造器签名去定义一个接口并试图定义一个类去实现这个接口时，只对其实例部分进行类型检查。 constructor 存在于类的静态部分，所以不在检查的范围内。 应该直接操作类的静态部分。
```typescript
interface ClockConstructor {        
    new (hour: number, minute: number): ClockInterface;
}
interface ClockInterface {
    tick(): void;
}    
function createClock(ctor: ClockConstructor, hour: number, minute: number): ClockInterface {
    return new ctor(hour, minute);
}    
class DigitalClock implements ClockInterface {
    constructor(h: number, m: number) { }
    tick() {            
        console.log("beep beep");
    }
}    
class AnalogClock implements ClockInterface {
    constructor(h: number, m: number) { }
    tick() {            
        console.log("tick tock");
    }
}    
let digital = createClock(DigitalClock, 12, 17);
let analog = createClock(AnalogClock, 7, 32);
```

## 扩展接口
---
和类一样，接口也可以相互扩展。 可以从一个接口里复制成员到另一个接口里，更灵活地将接口分割到可重用的模块里。 一个接口可以继承多个接口，创建出多个接口的合成接口。
``` typescript
interface Shape {
    color: string;
}

interface PenStroke {
    penWidth: number;
}

interface Square extends Shape, PenStroke {
    sideLength: number;
}    
let square = <Square>{};
square.color = "blue";
square.sideLength = 10;
square.penWidth = 5.0;
```
## 混合类型
---
接口能够描述JavaScript里丰富的类型, 因为 JavaScript 其动态灵活的特点，一个对象可以同时具有多种类型。 一个对象可以同时做为函数和对象使用，并带有额外的属性。

## 接口继承类
---
当接口继承了一个类类型时，它会继承类的成员但不包括其实现。 就好像接口声明了所有类中存在的成员，但并没有提供具体实现一样。 接口同样会继承到类的 private 和protected 成员。 当创建了一个接口继承了一个拥有私有或受保护的成员的类时，这个接口类型只能被这个类或其子类所实现（implement）。

当有一个庞大的继承结构时这很有用，但代码只在子类拥有特定属性时起作用，这个子类除了继承至基类外与基类没有任何关系。

``` typescript
class Control {
    private state: any;
}

interface SelectableControl extends Control {
    select(): void;
}    
class Button extends Control {
    select() { }
}    
class TextBox extends Control {
    select() { }
}    
class Image {
    select() { }
}    
class Location {
    select() { }
}
```
以上代码，SelectableControl 包含了 Control 的所有成员，包括私有成员 state。 因为 state 是私有成员，所以只能够是 Control 的子类们才能实现 SelectableControl 接口。 因为只有 Control 的子类才能够拥有一个声明于 Control 的私有成员 state，这对私有成员的兼容性是必需的。

在 Control 类内部，是允许通过 SelectableControl 的实例来访问私有成员 state 的。 实际上， SelectableControl 就像 Control 一样，并拥有一个 select 方法。 Button 和TextBox 类是 SelectableControl 的子类，但 Image 和 Location 类并不是这样的。

## 把类当做接口使用
---
类定义会创建两个东西：类的实例类型和一个构造函数。 因为类可以创建出类型，所以能够在允许使用接口的地方使用类。

``` typescript
class Chart {
    x: number;
    y: number;
}
interface LabelChart extends Chart {
    text: string;
}    
let textShape: LabelChart = { x: 1, y: 2, text: 'hello' };
```
# 使用TypeScript开发微信小程序（6）——泛型（Generic） 
软件工程中，不仅要创建一致的定义良好的API，同时也要考虑可重用性，组件不仅能够支持当前的数据类型，同时也能支持未来的数据类型，为创建大型系统提供了十分灵活的功能。在 TypeScript里， 可以像C#和Java语言一样，使用泛型来创建可重用的组件，一个组件可以支持多种类型的数据，这样用户就可以以自己的数据类型来使用组件。

## 泛型
---
可以使用类型变量，它是一种特殊的变量，只用于表示类型而不是值，使返回值的类型与传入参数的类型是相同的。
``` typescript
function identity<T>(arg: T): T {
    return arg;
}
```
以上代码，给 identity 添加了类型变量 T。 T  可以捕获用户传入的类型（比如：number），并使用 T 当做返回值类型。参数类型与返回值类型是相同的了，可以跟踪函数里使用的类型的信息。 把 identity 函数叫做泛型，因为它可以适用于多个类型。 不同于使用 any，它不会丢失信息，参数类型与返回值类型保持准确性。

定义了泛型函数后，可以用两种方法使用。

第一种是，传入所有的参数，包含类型参数：
``` typescript
let output1 = identity<string>("myString");
```
第二种方法更普遍。利用了类型推论 — 即编译器会根据传入的参数自动地帮助我们确定T的类型：
``` typescript
let output2 = identity("myString");
```
使用泛型创建泛型函数时，编译器要求在函数体必须正确的使用这个通用的类型。 换句话说，必须把这些参数当做是任意或所有类型。

``` typescript
function loggingIdentity<T>(arg: T): T { 
    console.log(arg.length);  // Error
    return arg;
}
```
以上代码，编译器会报错说使用了arg 的 length 属性，但是没有地方指明 ar g具有这个属性。 些类型变量代表的是任意类型，所以使用这个函数的人可能传入的是个数字，而数字是没有  length 属性的。

除了可以创建泛型函数，还可以创建泛型接口和泛型类，但无法创建泛型枚举和泛型命名空间。

## 泛型函数的类型
---
泛型函数的类型与非泛型函数的类型没什么不同，只是有一个类型参数在最前面。可以使用不同的泛型参数名，只要在数量上和使用方式上能对应上就可以。
``` typescript
function identity2<T>(arg: T): T {        
    return arg;
}    
let myIdentity2: <U>(arg: U) => U = identity2;
```
还可以使用带有调用签名的对象字面量来定义泛型函数。
``` typescript
function identity3<T>(arg: T): T {        
    return arg;
}    
let myIdentity3: { <T>(arg: T): T } = identity3;
```
## 泛型接口
---
可以把泛型参数当作整个接口的一个参数，就能清楚的知道使用的具体是哪个泛型类型（比如： Dictionary而不只是Dictionary）。 这样接口里的其它成员也能知道这个参数的类型了。
``` typescript
interface GenericIdentityFn<T> {
    (arg: T): T;
}    
function identity3<T>(arg: T): T {        
    return arg;
}    
let myIdentity: GenericIdentityFn<number> = identity;
```
以上代码，不再描述泛型函数，而是把非泛型函数签名作为泛型类型一部分。 当使用 GenericIdentityFn 的时候，还得传入一个类型参数来指定泛型类型（这里是：number），锁定了之后代码里使用的类型。

## 泛型类
---
泛型类看上去与泛型接口差不多。 泛型类使用（ <>）括起泛型类型，跟在类名后面。
``` typescript
class GenericNumeric<T> {
    zeroValue: T;
    add: (x: T, y: T) => T;
}    
let numberNumeric = new GenericNumeric<number>();
numberNumeric.zeroValue = 0;
numberNumeric.add = function (x, y) { 
    return x + y; 
};    
console.log(numberNumeric.add(numberNumeric.zeroValue, 100));    
let stringNumeric = new GenericNumeric<string>();
stringNumeric.zeroValue = "";
stringNumeric.add = function (x, y) { 
    return x + y; 
};    
console.log(stringNumeric.add(stringNumeric.zeroValue, "test"));
```
与接口一样，直接把泛型类型放在类后面，可以帮助确认类的所有属性都在使用相同的类型。泛型类指的是实例部分的类型，所以类的静态属性不能使用这个泛型类型。
## 泛型约束
---
可以定义一个接口来描述约束条件。创建一个包含 属性的接口，使用这个接口和 extends 关键字还实现约束：
``` typescript
interface Lengthwise {
    length: number;
}    
function logIdentity<T extends Lengthwise>(arg: T): T {        
    console.log(arg.length); 
    return arg;
}    
// logIdentity(3);  // Error
logIdentity({length: 10, value: 3});
```
以上代码，泛型函数 logIdentity 被定义了约束，因此它不再是适用于任意类型，需要传入符合约束类型的值，必须包含必须的属性。

### 在泛型约束中使用类型参数
可以声明一个类型参数，且它被另一个类型参数所约束。 比如，现在有两个对象，并把一个对象的属性拷贝到另一个对象， 需要确保没有不小心地把额外的属性从源对象拷贝到目标对象，因此需要在这两个类型之间使用约束。
``` typescript
function copyFields<T extends U, U>(target: T, source: U): T {        
    for (let id in source) {
        (<U>target)[id] = source[id];
    }
    return target;
}

let x = { a: 1, b: 2, c: 3, d: 4 };
copyFields(x, { b: 10, d: 20 }); 
// copyFields(x, { Q: 90 }); // Error
```

### 在泛型里使用类类型
在TypeScript使用泛型创建工厂函数时，需要引用构造函数的类类型。

``` typescript
class BeeKeeper {
    hasMask: boolean;
}

class ZooKeeper {
    nametag: string;
}

class Animal {
    numLegs: number;
}

class Bee extends Animal {
    keeper: BeeKeeper;
}

class Lion extends Animal {
    keeper: ZooKeeper;
}

function findKeeper<A extends Animal, K>(a: {
    new (): A;
    prototype: { keeper: K }
}): K {
    return a.prototype.keeper;
}

findKeeper(Lion).nametag;
```
以上代码，使用原型属性推断并约束构造函数与类实例的关系。

# 使用TypeScript开发微信小程序（7）——迭代器（Iterator）

Symbol.iterator 为每一个对象定义了默认的迭代器。该迭代器可以被 for…of 循环结构使用。

一些内置的类型如 Array，Map，Set，String，Int32Array，Uint32Array 等都已经实现了各自的 Symbol.iterator。 对象上的 Symbol.iterator 函数负责返回供迭代的值。

## for..of
---
for..of会遍历可迭代的对象，调用对象上的Symbol.iterator方法。
``` typescript
let someArray = [1, "string", false];    
for (let entry of someArray) {        
    console.log(entry); // 1, "string", false
}
```
## for..of 和 for..in
---
or..of 和 for..in 均可迭代一个列表；但是用于迭代的值却不同，for..in 迭代的是对象的键的列表，而 for..of 则迭代对象的键对应的值。
``` typescript
let list = [4, 5, 6];    
for (let i in list) {        
    console.log(i); // 输出：0, 1, 2
}    
for (let i of list) {        
    console.log(i); // 输出：4, 5,6
}
```
另一个区别是 for..in 可以操作任何对象；它提供了查看对象属性的一种方法。 但是 for..of 关注于迭代对象的值。内置对象 Map 和 Set 已经实现了 Symbol.iterator方法，可以访问它们保存的值。

# 使用TypeScript开发微信小程序（8）——模块（Module） 
从ECMAScript 2015 开始，JavaScript 引入了模块的概念。TypeScript 也沿用这个概念。

模块功能主要由两个命令构成：export和import。export命令用于用户自定义模块，规定对外接口；import命令用于输入其他模块提供的功能。

## 模块
---
模块在其自身的作用域里执行，而不是在全局作用域里；这意味着定义在一个模块里的变量，函数，类等等在模块外部是不可见的，除非明确地使用 export形式之一导出它们。 相反，如果想使用其它模块导出的变量，函数，类，接口等的时候，必须要导入它们，可以使用 import 形式之一。

模块是自声明的；两个模块之间的关系是通过在文件级别上使用 imports 和 exports 建立的。

模块使用模块加载器去导入其它的模块。 在运行时，模块加载器的作用是在执行此模块代码前去查找并执行这个模块的所有依赖。

TypeScript 与 ECMAScript 2015 一样，任何包含顶级 import 或者 export 的文件都被当成一个模块。

## 导出
---
任何声明（比如变量，函数，类，类型别名或接口）都能够通过添加export关键字来导出。
``` typescript
export interface StringValidator {
    isAcceptable(s: string): boolean;
}

export class LettersOnlyValidator implements StringValidator {
    isAcceptable(s: string) {            
        return lettersRegexp.test(s);
    }
}

class ZipCodeValidator implements StringValidator {
    isAcceptable(s: string) {            
        return s.length === 5 && numberRegexp.test(s);
    }
}
export { ZipCodeValidator };
export { ZipCodeValidator as mainValidator };
```
可以扩展其它模块，并且只导出那个模块的部分内容。重新导出功能并不会在当前模块导入那个模块或定义一个新的局部变量。

``` typescript
export class ParseIntBasedZipCodeValidator {
    isAcceptable(s: string) {            
        return s.length === 5 && parseInt(s).toString() === s;
    }
}

export {ZipCodeValidator as RegExpBasedZipCodeValidator} from "./ZipCodeValidator";
```
一个模块可以包裹多个模块，并把他们导出的内容联合在一起通过语法：export * from “module”。
``` typescript
    export * from "./Validation"; 
    export * from "./LettersOnlyValidator"; 
    export * from "./ZipCodeValidator";
```
## 导入
---
模块的导入操作与导出一样简单。 可以使用以下 import形式之一来导入其它模块中的导出内容。
``` typescript
import { ZipCodeValidator } from "./ZipCodeValidator";    
let myValidator = new ZipCodeValidator();
```
将整个模块导入到一个变量，并通过它来访问模块的导出部分
``` typescript
import * as validator from "./ZipCodeValidator";   
let myValidator = new validator.ZipCodeValidator();
```
一些模块会设置一些全局状态供其它模块使用，这些模块可能没有任何的导出或用户根本就不关注它的导出。 使用下面的方法来导入这类模块。

## 默认导出
---
每个模块都可以有一个default导出。 默认导出使用 default关键字标记；并且一个模块只能够有一个default导出。 需要使用一种特殊的导入形式来导入 default导出。

类和函数声明可以直接被标记为默认导出。 标记为默认导出的类和函数的名字是可以省略的。

default导出也可以是一个值
``` typescript
export default "123";
import num from "./OneTwoThree";    
console.log(num); // 输出：123
```
## export = 、import id = require(“…”) 语法
---
CommonJS 和 AMD 都有一个 exports 对象的概念，它包含了一个模块的所有导出内容。

它们也支持把 exports 替换为一个自定义对象。 默认导出就好比这样一个功能；然而，它们却并不相互兼容。 TypeScript模块支持 export = 语法以支持传统的 CommonJS 和 AMD 的工作流模型。

export = 语法定义一个模块的导出对象。 它可以是类，接口，命名空间，函数或枚举。

若要导入一个使用了export = 的模块时，必须使用TypeScript提供的特定语法 import let = require(“…”)。

在 TypeScript 里，编译器会检测是否每个模块都会在生成的 JavaScript 中用到。 如果一个模块标识符只在类型注解部分使用，并且完全没有在表达式中使用时，就不会生成 require 这个模块的代码。 省略掉没有用到的引用对性能提升是很有益的，并同时提供了选择性加载模块的能力。

这种模式的核心是import id = require(“…”)语句可以让我们访问模块导出的类型。 模块加载器会被动态调用（通过 require），就像下面if代码块里那样。 它利用了省略引用的优化，所以模块只在被需要时加载。 为了让这个模块工作，一定要注意 import定义的标识符只能在表示类型处使用（不能在会转换成JavaScript的地方）。

为了确保类型安全性，我们可以使用 typeof 关键字。 typeof 关键字，当在表示类型的地方使用时，会得出一个类型值，这里就表示模块的类型。

## 使用其它的JavaScript库
---

要想描述非 TypeScript 编写的类库的类型，我们需要声明类库所暴露出的 API。 它们通常是在 .d.ts文件里定义的。

可以使用顶级的 export 声明来为每个模块都定义一个 .d.ts 文件，但最好还是写在一个大的 .d.ts 文件里。 使用 module 关键字并且把名字用引号括起来，方便之后import。

node.d.ts
``` typescript
declare module "url" {
    export interface Url {
        protocol?: string;
        hostname?: string;
        pathname?: string;
    }

    export function parse(urlStr: string, parseQueryString?: string, slashesDenoteHost?: string): Url;
}
declare module "path" {
    export function normalize(p: string): string; 
    export function join(...paths: any[]): string;
    export let sep: string;
}
//import url = require(“url”)
import * as URL from "url";    
let myUrl = URL.parse("http://www.com");
```

# 使用TypeScript开发微信小程序（9）——命名空间（Namespace） 
TypeScript里，可以使用命名空间（之前叫做“内部模块”，现在叫做“命名空间”）来组织你的代码。

## 命名空间
---
Validation.ts
``` typescript
export interface StringValidator {
    isAcceptable(s: string): boolean;
}
```
LettersOnlyValidator.ts
``` typescript
import { StringValidator } from "./Validation";    const lettersRegexp = /^[A-Za-z]+$/;

export class LettersOnlyValidator implements StringValidator {
    isAcceptable(s: string) {            
        return lettersRegexp.test(s);
    }
}
```
ZipCodeValidator.ts
``` typescript
import { StringValidator } from "./Validation";    
const numberRegexp = /^[0-9]+$/;    
class ZipCodeValidator implements StringValidator {
    isAcceptable(s: string) {        
        return s.length === 5 && numberRegexp.test(s);
    }
}
export { ZipCodeValidator };
export { ZipCodeValidator as mainValidator };

let strings = ["Hello", "98052", "101"];      
let validators: { [s: string]: Validation.StringValidator; } = {};
validators["ZIP code"] = new Validation.ZipCodeValidator();
validators["Letters only"] = new Validation.LettersOnlyValidator();       
strings.forEach(s => {        
    for (let name in validators) {            
        console.log("\"" + s + "\"" + (validators[name].isAcceptable(s) ? " matches " : " does not match ") + name);
    }
});
```

## 别名
---
简化命名空间操作的方法是别名，使用 import q = x.y.z 给常用的对象起一个短的名字，可以用这种方法为任意标识符创建别名，也包括导入的模块中的对象。
``` typescript
namespace Shapes {
    export namespace Polygons {
        export class Triangle { }
        export class Square { }
    }
}

import polygons = Shapes.Polygons;    
let sq = new polygons.Square();
```

## 使用其它的JavaScript库
为了描述不是用TypeScript编写的类库的类型，需要声明类库导出的 API。 由于大部分程序库只提供少数的顶级对象，命名空间是用来表示它们的一个好办法。

D3.d.ts
``` typescript
declare namespace D3 {
    export interface Selectors {
        select: {
            (selector: string): Selection;
            (element: EventTarget): Selection;
        };
    }

    export interface Event {
        x: number;
        y: number;
    }

    export interface Base extends Selectors {
        event: Event;
    }
}

declare let d3: D3.Base;
```

# 使用TypeScript开发微信小程序（10）——装饰器（Decorator） 
随着 TypeScript 和 ES6 里引入了类，在一些场景下需要额外的特性来支持标注或修改类及其成员。 装饰器（Decorators）可以在类的声明及成员上通过元编程语法添加标注提供了一种方式。 Javascript 里的装饰器目前处在建议征集的第一阶段，但在TypeScript里已做为一项实验性特性予以支持。

## 装饰器
---
装饰器是一种特殊类型的声明，它能够被附加到类声明，方法， 访问符，属性或参数上。 装饰器使用 @expression 这种形式，expression 求值后必须为一个函数，它会在运行时被调用，被装饰的声明信息做为参数传入。

例如，有一个 @sealed 装饰器，定义 sealed 函数：

``` typescript
function sealed(target) {        
    // do something with "target" ...
}
```
若要启用实验性的装饰器特性，你须在命令行或 tsconfig.json 里启用 experimentalDecorators 编译器选项。

命令行:
``` typescript
    tsc --target ES5 --experimentalDecorators
```

``` json
{
    "compilerOptions": {
        "target": "ES5",
        "experimentalDecorators": true
    }    
}
```

## 装饰器工厂
---
装饰器工厂就是一个简单的函数，它返回一个表达式，以供装饰器在运行时调用。
``` typescript
function color(value: string) { 
    // 这是一个装饰器工厂
    return function (target) { 
        //  这是装饰器
        // do something with "target" and "value"...
    }
}
```

## 装饰器组合
多个装饰器可以同时应用到一个声明上。

书写在同一行上：
``` typescript
@f @g x
```

书写在多行上：
``` typescript
@f
@g
x
```
当多个装饰器应用于一个声明上，它们求值方式与复合函数相似。在这个模型下，当复合f和g时，复合的结果(f ∘ g)(x)等同于f(g(x))。

同样的，在TypeScript里，当多个装饰器应用在一个声明上时会进行如下步骤的操作：
- 由上至下依次对装饰器表达式求值。
- 求值的结果会被当作函数，由下至上依次调用。
``` typescript
function f() {     
console.log("f(): evaluated");     
return function (target: any, propertyKey: string, descriptor: PropertyDescriptor) {
    console.log("f(): called");
}
} 
function g() {     
    console.log("g(): evaluated");     
    return function (target: any, propertyKey: string, descriptor: PropertyDescriptor) {
        console.log("g(): called");
    }
} 
class C {
    @f()
    @g()
    method() { }
}
// f(): evaluated
// g(): evaluated
// g(): called
// f(): called
```

## 装饰器求值
---
类中不同声明上的装饰器将按以下规定的顺序应用：
- 参数装饰器，然后依次是方法装饰器，访问符装饰器，或属性装饰器应用到每个实例成员。
- 参数装饰器，然后依次是方法装饰器，访问符装饰器，或属性装饰器应用到每个实例成员。
- 参数装饰器，然后依次是方法装饰器，访问符装饰器，或属性装饰器应用到每个实例成员。
- 参数装饰器，然后依次是方法装饰器，访问符装饰器，或属性装饰器应用到每个实例成员。

## 类装饰器
---
类装饰器在类声明之前被声明（紧靠着类声明）。 类装饰器应用于类构造函数，可以用来监视，修改或替换类定义。 类装饰器不能用在声明文件中( .d.ts)，也不能用在任何外部上下文中（比如 declare 的类）。

类装饰器表达式会在运行时当作函数被调用，类的构造函数作为其唯一的参数。

如果类装饰器返回一个值，它会使用提供的构造函数来替换类的声明。如果要返回一个新的构造函数，必须注意处理好原来的原型链。 在运行时的装饰器调用逻辑中不会做这些。
``` typescript
function sealed(constructor: Function) {        
    Object.seal(constructor);        
    Object.seal(constructor.prototype);
}
@sealed    
class Greeter {
    greeting: string;
    constructor(message: string) {            
        this.greeting = message;
    }
    greet() {            
        return "Hello, " + this.greeting;
    }
}
```
以上代码，当 @sealed 被执行的时候，它将密封此类的构造函数和原型。

## 方法装饰器
---
方法装饰器声明在一个方法的声明之前（紧靠着方法声明）。 它会被应用到方法的属性描述符上，可以用来监视，修改或者替换方法定义。 方法装饰器不能用在声明文件( .d.ts)，重载或者任何外部上下文（比如declare的类）中。

方法装饰器表达式会在运行时当作函数被调用，传入下列3个参数：
- 对于静态成员来说是类的构造函数，对于实例成员是类的原型对象。
- 成员的名字。
- 成员的属性描述符。
如果代码输出目标版本小于 ES5，属性描述符将会是 undefined。

如果方法装饰器返回一个值，它会被用作方法的属性描述符。如果代码输出目标版本小于 ES5 返回值会被忽略。

``` typescript
function enumerable(value: boolean) {        
    return function (target: any, propertyKey: string, descriptor: PropertyDescriptor) {
        descriptor.enumerable = value;
    };
}    
class Greeter {
    greeting: string;
    constructor(message: string) {            
        this.greeting = message;
    }

    @enumerable(false)
    greet() {            
        return "Hello, " + this.greeting;
    }
}
```
以上代码，@enumerable(false) 是一个装饰器工厂。 当装饰器 @enumerable(false) 被调用时，它会修改属性描述符的 enumerable 属性。

## 访问器装饰器
---
访问器装饰器声明在一个访问器的声明之前（紧靠着访问器声明）。 访问器装饰器应用于访问器的 属性描述符并且可以用来监视，修改或替换一个访问器的定义。 访问器装饰器不能用在声明文件中（.d.ts），或者任何外部上下文（比如 declare 的类）里。

TypeScript 不允许同时装饰一个成员的 get 和 set 访问器。取而代之的是，一个成员的所有装饰的必须应用在文档顺序的第一个访问器上。这是因为，在装饰器应用于一个属性描述符时，它联合了 get 和 set 访问器，而不是分开声明的。

访问器装饰器表达式会在运行时当作函数被调用，传入下列3个参数：
- 对于静态成员来说是类的构造函数，对于实例成员是类的原型对象。
- 成员的名字。
- 成员的属性描述符。
如果代码输出目标版本小于 ES5，Property Descriptor 将会是 undefined。

如果访问器装饰器返回一个值，它会被用作方法的属性描述符。如果代码输出目标版本小于 ES5 返回值会被忽略。
``` typescript
function configurable(value: boolean) {        
    return function (target: any, propertyKey: string, descriptor: PropertyDescriptor) {
        descriptor.configurable = value;
    };
}    
class Point {
    private _x: number;
    private _y: number;
    constructor(x: number, y: number) {            
        this._x = x;            
        this._y = y;
    }

    @configurable(false)
    get x() { return this._x; }

    @configurable(false)
    get y() { return this._y; }
}
```
## 属性装饰器
---
属性装饰器声明在一个属性声明之前（紧靠着属性声明）。 属性装饰器不能用在声明文件中（.d.ts），或者任何外部上下文（比如 declare的类）里。

属性装饰器表达式会在运行时当作函数被调用，传入下列2个参数：
- 对于静态成员来说是类的构造函数，对于实例成员是类的原型对象。
- 成员的名字。
属性描述符不会做为参数传入属性装饰器，这与TypeScript是如何初始化属性装饰器的有关。 因为目前没有办法在定义一个原型对象的成员时描述一个实例属性，并且没办法监视或修改一个属性的初始化方法。 因此，属性描述符只能用来监视类中是否声明了某个名字的属性。

如果属性装饰器返回一个值，它会被用作方法的属性描述符。如果代码输出目标版本小于 ES5，返回值会被忽略。

如果访问符装饰器返回一个值，它会被用作方法的属性描述符。

``` typescript
const formatMetadataKey = Symbol("format");    
function format(formatString: string) {        
    return Reflect.metadata(formatMetadataKey, formatString);
}    
function getFormat(target: any, propertyKey: string) {        
    return Reflect.getMetadata(formatMetadataKey, target, propertyKey);
}    
class PropGreeter {
    @format("Hello, %s")
    greeting: string;

    constructor(message: string) {            
        this.greeting = message;
    }
    greet() {            
        let formatString = getFormat(this, "greeting");            
        return formatString.replace("%s", this.greeting);
    }
}
```
以上代码，这个 @format(“Hello, %s”) 装饰器是个装饰器工厂。 当 @format(“Hello, %s”) 被调用时，它添加一条这个属性的元数据，通过reflect-metadata 库里的 Reflect.metadata 函数。 当 getFormat 被调用时，它读取格式的元数据。

## 参数装饰器
---
参数装饰器声明在一个参数声明之前（紧靠着参数声明）。 参数装饰器应用于类构造函数或方法声明。 参数装饰器不能用在声明文件（.d.ts），重载或其它外部上下文（比如 declare的类）里。

参数装饰器表达式会在运行时当作函数被调用，传入下列3个参数：
- 对于静态成员来说是类的构造函数，对于实例成员是类的原型对象。
- 成员的名字。
- 参数在函数参数列表中的索引。
参数装饰器只能用来监视一个方法的参数是否被传入。参数装饰器的返回值会被忽略。

``` typescript
const requiredMetadataKey = Symbol("required");    function required(target: Object, propertyKey: string | symbol, parameterIndex: number) {        
    let existingRequiredParameters: number[] = Reflect.getOwnMetadata(requiredMetadataKey, target, propertyKey) || [];
    existingRequiredParameters.push(parameterIndex);
    Reflect.defineMetadata(requiredMetadataKey, existingRequiredParameters, target, propertyKey);
}    
function validate(target: any, propertyName: string, descriptor: TypedPropertyDescriptor<Function>) {        
    let method = descriptor.value;
    descriptor.value = function () {            
        let requiredParameters: number[] = Reflect.getOwnMetadata(requiredMetadataKey, target, propertyName);            
    
        if (requiredParameters) {                
            for (let parameterIndex of requiredParameters) {                    
                if (parameterIndex >= arguments.length || arguments[parameterIndex] === undefined) {                        
                    throw new Error("Missing required argument.");
                }
            }
        }            
        return method.apply(this, arguments);
    }
}    
class ParaGreeter {
    greeting: string;

    constructor(message: string) {            
        this.greeting = message;
    }

    @validate
    greet( @required name: string) {            
        return "Hello " + name + ", " + this.greeting;
    }
}
```
以上代码，@required装饰器添加了元数据实体把参数标记为必需的。 @validate装饰器把greet方法包裹在一个函数里在调用原先的函数前验证函数参数。

## reflect-metadata
---
使用 reflect-metadata 库来支持实验性的 metadata API。 这个库还不是 ECMAScript 标准的一部分。 然而，当装饰器被 ECMAScript 官方标准采纳后，这些扩展也将被推荐给 ECMAScript 以采纳。

通过 npm 安装 reflect-metadata 库：

`npm install reflect-metadata --save`

TypeScript 支持为带有装饰器的声明生成元数据，需要在命令行或 tsconfig.json 里启用 emitDecoratorMetadata 编译器选项。

命令行:

`tsc --target ES5 --experimentalDecorators --emitDecoratorMetadata`

tsconfig.json

``` json
{
    "compilerOptions": {
        "target": "ES5",
        "experimentalDecorators": true,
        "emitDecoratorMetadata": true
    }    
}
```
当启用后，只要reflect-metadata库被引入了，设计阶段添加的类型信息可以在运行时使用。

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
结语：做好一件事
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
