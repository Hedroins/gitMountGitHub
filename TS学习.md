在构造函数的参数前面使用public,相当于创建同名的成员变量。

TypeScript 规定，变量只有赋值后才能使用，否则就会报错

类型声明并不是必需的，如果没有，TypeScript 会自己推断类型。
let foo = 123;
上面示例中，变量foo并没有类型声明，TypeScript 就会推断它的类型。由于它被赋值为一个数值，因此 TypeScript 推断它的类型为number。

后面，如果变量foo更改为其他类型的值，跟推断的类型不一致，TypeScript 就会报错


TypeScript 也可以推断函数的返回值。
function toString(num: number) {
  return String(num);
}
上面示例中，函数toString()没有声明返回值的类型，但是 TypeScript 推断返回的是字符串。正是因为 TypeScript 的类型推断，所以函数返回值的类型通常是省略不写的。

这一点务必牢记。TypeScript 项目里面，其实存在两种代码，一种是底层的“值代码”，另一种是上层的“类型代码”。前者使用 JavaScript 语法，后者使用 TypeScript 的类型语法。

添加noEmitOnError参数可以在tsc编译器报错时不生成js文件

tsconfig.json是可以配置tsc编译时的一些选项，等同于在命令行tsc后面加参数

any 类型表示没有任何限制，该类型的变量可以赋予任意类型的值。
let x: any;
x = 1; // 正确
x = "foo"; // 正确
x = true; // 正确
变量类型一旦设为any，TypeScript 实际上会关闭这个变量的类型检查。即使有明显的类型错误，只要句法正确，都不会报错。

应该尽量避免使用any类型，否则就失去了使用 TypeScript 的意义。

any类型主要适用以下两个场合：
（1）出于特殊原因，需要关闭某些变量的类型检查，就可以把该变量的类型设为any。

（2）为了适配以前老的 JavaScript 项目，让代码快速迁移到 TypeScript，可以把变量类型设为any。有些年代很久的大型 JavaScript 项目，尤其是别人的代码，很难为每一行适配正确的类型，这时你为那些类型复杂的变量加上any，TypeScript 编译时就不会报错

any类型可以看成是所有其他类型的全集，包含了一切可能的类型。TypeScript 将这种类型称为“顶层类型”（top type）；


类型推断：
对于开发者没有指定类型、TypeScript 必须自己推断类型的那些变量，如果无法推断出类型，TypeScript 就会认为该变量的类型是any

noImplicitAny 配置项 打开该选项，只要推断出any类型就会报错；
   例外：即使打开了noImplicitAny，使用let和var命令声明变量，但不赋值也不指定类型，是不会报错的。
   var x; // 不报错
   let y; // 不报错
   上面示例中，变量x和y声明时没有赋值，也没有指定类型，TypeScript 会推断它们的类型为any。这时即使打开了noImplicitAny，也不会报错。
   由于这个原因，建议使用let和var声明变量时，如果不赋值，就一定要显式声明类型，否则可能存在安全隐患。
   const命令没有这个问题，因为 JavaScript 语言规定const声明变量时，必须同时进行初始化（赋值）。

污染问题：
   any类型除了关闭类型检查，还有一个很大的问题，就是它会“污染”其他变量。它可以赋值给其他任何类型的变量（因为没有类型检查），导致其他变量出错。
   ```typescript 
       let x: any = "hello";
       let y: number;
       y = x; // 不报错
       y * 123; // 不报错
       y.toFixed(); // 不报错
   ```
   污染其他具有正确类型的变量，把错误留到运行时，这就是不宜使用any类型的另一个主要原因。


unknown类型：
   为了解决any类型“污染”其他变量的问题，TypeScript 3.0 引入了unknown类型。它与any含义相同，表示类型不确定，可能是任意类型，但是它的使用有一些限制，不像any那样自由，可以视为严格版的any。

   unknown跟any的相似之处，在于所有类型的值都可以分配给unknown类型。
   unknown类型跟any类型的不同之处在于，它不能直接使用。主要有以下几个限制:
      1、unknown类型的变量，不能直接赋值给其他类型的变量（除了any类型和unknown类型）。
      2、不能直接调用unknown类型变量的方法和属性
      3、unknown类型变量能够进行的运算是有限的，只能进行比较运算（==、===、!=、!==）逻辑运算（||、&&、!）、typeof运算符和instanceof运算符这几种，其他运算都会报错。

   怎么才能使用unknown类型变量呢？
      只有经过“类型缩小”，unknown类型变量才可以使用。所谓“类型缩小”，就是缩小unknown变量的类型范围，确保不会出错。
     ```typescript
        let a: unknown = 1;

        if (typeof a === "number") {
        let r = a + 10; // 正确
     }```
     即将一个不确定的类型缩小为更明确的类型
     unknown也可以视为所有其他类型（除了any）的全集，所以它和any一样，也属于 TypeScript 的顶层类型。


never类型：
    空类型，不包括任何值。ts为了保持与集合论的对应关系，以及类型运算的完整性
    never类型的使用场景，主要是在一些类型运算之中，保证类型运算的完整性。
    不可能返回值的函数，返回值的类型就可以写成never.

    如果一个变量可能有多种类型（即联合类型），通常需要使用分支处理每一种类型。这时，处理所有可能的类型之后，剩余的情况就属于never类型。
    ```typescript
    function fn(x: string | number) {
         if (typeof x === "string") {
          // ...
           } else if (typeof x === "number") {
            // ...
         } else {
           x; // never 类型
      }
    }
    ```
    never类型可以赋值给任意其他类型
    为什么never类型可以赋值给任意其他类型呢？
       这也跟集合论有关，空集是任何集合的子集。
    任何类型都包含了never类型。因此，never类型是任何其他类型所共有的，
    TypeScript 把这种情况称为“底层类型”（bottom type）
    never类型就是底层类型


类型系统：
    TypeScript 继承了 JavaScript 的类型设计，JavaScript中的boolean、string、number、bigint、symbol、object、undefined、null都是TypeScript 的基本类型

    undefined 和 null 既可以作为值，也可以作为类型，取决于在哪里使用它们。
    复杂类型由基础类型组合而成。

    bigint类型和number类型虽然都表示数值，但是是不兼容的，
    ```typescript
    const x: bigint = 123; // 报错
    const y: bigint = 3.14; // 报错
    
    ```
    object 类型包含了所有对象、数组和函数.
    undefined 和 null 是两种独立类型，它们各自都只有一个值

    注意，如果没有声明类型的变量，被赋值为undefined或null，它们的类型会被推断为any
    ```typescript
    let a = undefined; // any
    const b = undefined; // any

    let c = null; // any
    const d = null; // any
    ```
    如果希望避免这种情况，则需要打开编译选项strictNullChecks。打开编译设置strictNullChecks以后，赋值为undefined的变量会被推断为undefined类型，赋值为null的变量会被推断为null类型


包装对象：
    undefined和null其实是两个特殊值，object属于复合类型，剩下的五种属于原始类型（primitive value），代表最基本的、不可再分的值
    boolean、string、number、bigint、symbol 这五种原始类型的值，都有对应的包装对象（wrapper object）。所谓“包装对象”，指的是这些值在需要时，会自动产生的对象,例如：'sdfsdf'.charAt(2) 会自动转成包装对象，调用包装对象的charAt方法。
    五种包装对象之中，symbol 类型和 bigint 类型无法直接获取它们的包装对象（即Symbol()和BigInt()不能作为构造函数使用），但是剩下三种可以
    Boolean()
    String()
    Number()

    由于包装对象的存在，导致每一个原始类型的值都有包装对象和字面量两种情况。
    为了区分这两种情况，TypeScript 对五种原始类型分别提供了大写和小写两种类型。
        Boolean 和 boolean
        String 和 string
        Number 和 number
        BigInt 和 bigint
        Symbol 和 symbol
    大写类型同时包含包装对象和字面量两种情况，小写类型只包含字面量，不包含包装对象
    ```typescript
      const s1: String = "hello"; // 正确
      const s2: String = new String("hello"); // 正确
      const s3: string = "hello"; // 正确
      const s4: string = new String("hello"); // 报错
    ```
    建议只使用小写类型，不使用大写类型。因为绝大部分使用原始类型的场合，都是使用字面量，不使用包装对象。而且，TypeScript 把很多内置方法的参数，定义成小写类型，使用大写类型会报错。



Object类型和object类型：
   大写的Object类型代表 JavaScript 语言里面的广义对象。所有可以转成对象的值，都是Object类型，这囊括了几乎所有的值。
    ```typescript
    let obj: Object;

    obj = true;
    obj = "hi";
    obj = 1;
    obj = { foo: 123 };
    obj = [1, 2];
    obj = (a: number) => a + 1;
    ```
    原始类型值、对象、数组、函数都是合法的Object类型,除了undefined和null这两个值不能转为对象，其他任何值都可以赋值给Object类型。
    Object类型的简写形式{}   let a:{} 等价于let a :Object

    小写的object类型代表 JavaScript 里面的狭义对象，即可以用字面量表示的对象，只包含对象、数组和函数，不包括原始类型的值。
    ```typescript
    let obj: object;

    obj = { foo: 123 };
    obj = [1, 2];
    obj = (a: number) => a + 1;
    obj = true; // 报错
    obj = "hi"; // 报错
    obj = 1; // 报错
    
    ```
    大多数时候，我们使用对象类型，只希望包含真正的对象，不希望包含原始类型。所以，建议总是使用小写类型object，不使用大写类型Object。
    无论是大写的Object类型，还是小写的object类型，都只包含 JavaScript 内置对象原生的属性和方法，用户自定义的属性和方法都不存在于这两个类型之中
    ```typescript
    const o1: Object = { foo: 0 };
    const o2: object = { foo: 0 };

    o1.toString(); // 正确
    o1.foo; // 报错

    o2.toString(); // 正确
    o2.foo; // 报错
    
    ```
    上面示例中，toString()是对象的原生方法，可以正确访问。foo是自定义属性，访问就会报错


    undefined和null的特殊性：undefined和null即是值又是类型。
     作为值他们有一个特殊的地方:任何类型的变量都可以赋值为undefined或null。
     ```javascript
       let a:number = 24;
       a = undefined; // 正确
       a = null; // 正确
     ```
     上面代码中，变量age的类型是number，但赋值为null或undefined不报错。
     这并不是因为undefined和null包含在number类型里面，而是故意这样设计，任何类型的变量都可以赋值为undefined或null，以便跟javascript的行为保持一致。

     但是这并不是开发者想要的行为，也不利于发挥类型系统的优势。为了避免这种情况，TS提供了一个编译选项strictNullChecks，当这个选项打开时，undefined和null就不能赋值给其他类型的变量。（除了any类型和unknown类型），只能赋值给自身。



     值类型：TS规定单个值也是一种类型。称为"值类型"
     ```typescript
       let a: 1 = 1;
       let b: "hi" = "hi";
       let c: true = true;
     ```
     值类型只能给变量赋值为这个值，否则会报错。
     TS推断类型时，遇到const命令声明的变量，如果代码里面没有注明类型，就会推断该变量是值类型。
     ```typescript
     const x = "https" // x的类型是 "https"
     const y = 1 // y的类型是 1
     ```
     上面代码中，x和y的类型都是值类型，而不是string和number类型。
     这样推断是合理的，因为const命令声明的变量，一旦声明就不能改变，相当于常量。值类型就意味着不能赋值为其他值。

     
     const命令声明的变量，如果赋值为对象，并不会推断为值类型。
     ```typescript
     const obj = { foo: 123 };
     // obj的类型是 { foo: number }
     ```
     上面代码中，obj没有被推断为值类型，而是推断属性foo的类型是number，这是因为 JavaScript 里面，const变量赋值为对象时，属性值是可以改变的。


     值类型可能会出现一些很奇怪的报错：
      ```TypeScript
      const x:5 = 4+1; // 报错
      ```
   上面示例中，等号左侧的类型是数值5，等号右侧4 + 1的类型，TypeScript 推测为number。由于5是number的子类型，number是5的父类型，父类型不能赋值给子类型，所以报错了

   反过来是可以的，子类型可以赋值给父类型
   ```typescript
   let x: 5 = 5;
   let y: number = 4 + 1;

     x = y; // 报错
     y = x; // 正确
   ```

   如果一定要让子类型可以赋值为父类型，就要用到类型断言
   ```typescript
   const x: 5 = (4 + 1) as 5; // 正确
   ```
   上面示例中，在4 + 1后面加上as 5，就是告诉编译器，可以把4 + 1的类型视为值类型5，这样就不会报错了



   联合类型：联合类型（union types）指的是多个类型组成的一个新类型，使用符号|表示
   ```typescript
   let a: number | string;
   a = 1;
   a = "hi";
   ```
   上面示例中，变量a的类型是number | string，表示a既可以是number类型，也可以是string类型。

   联合类型可以与值类型相结合，表示一个变量的值有若干种可能
   ```typescript
   let rainbowColor: "赤" | "橙" | "黄" | "绿" | "青" | "蓝" | "紫";
   ```
    
    前面提到，打开编译选项strictNullChecks后，其他类型的变量不能赋值为undefined或null。这时，如果某个变量确实可能包含空值，就可以采用联合类型的写法。
    ```typescript
    let a: string | undefined;
    a = "hi";
    a = undefined;
    ```
   如果一个变量有多种类型，读取该变量时，往往需要进行“类型缩小”（type narrowing），区分该值到底属于哪一种类型，然后再进一步处理。



   交叉类型：交叉类型（intersection types）指的多个类型组成的一个新类型，使用符号&表示。
   交叉类型A&B表示，任何一个类型必须同时属于A和B，才属于交叉类型A&B，即交叉类型同时满足A和B的特征。
     
   ```typescript
   let x: number & string;
   ```
   上面示例中，变量x同时是数值和字符串，这当然是不可能的，所以 TypeScript 会认为x的类型实际是never。

   交叉类型的主要用途是表示对象的合成

   ```typescript
   let obj: { foo: string } & { bar: string };
      obj = {
         foo: "hello",
         bar: "world",
      };
    ```
   上面示例中，变量obj同时具有属性foo和属性bar。


   交叉类型常常用来为对象类型添加新属性：

   ```typescript
   type A = { foo: number };
   type B = A & { bar: number };
   ```
   上面示例中，类型B是一个交叉类型，用来在A的基础上增加了属性bar。


   type命令： type命令用来定义一个类型的别名。

   ```typescript
   type Age = number;
   let age: Age = 55;
   ```

   上面示例中，type命令为number类型定义了一个别名Age。这样就能像使用number一样，使用Age作为类型。
   别名可以让类型的名字变得更有意义，也能增加代码的可读性，还可以使复杂类型用起来更方便，便于以后修改变量的类型。

   别名不允许重名。

   ```typescript
   type Color = "red";
   type Color = "blue"; // 报错
   
   ```
   上面示例中，同一个别名Color声明了两次，就报错了

   别名的作用域是块级作用域，这意味着，代码块内部定义的别名，影响不到外部。
   ```typescript
   type Color = "red";

   if (Math.random() < 0.5) {
       type Color = "blue";
    }

   ```
   上面示例中，if代码块内部的类型别名Color，跟外部的Color是不一样。


   别名支持使用表达式，也可以在定义一个别名时，使用另一个别名，即别名允许嵌套。
   ```typescript
   type World = "world";
   type Greeting = `hello ${World}`;
   ```
   上面示例中，别名Greeting使用了模板字符串，读取另一个别名World。
   type命令属于类型相关的代码，编译成 JavaScript 的时候，会被全部删除


   typeof运算符：TypeScript 将typeof运算符移植到了类型运算，它的操作数依然是一个值，但是返回的不是字符串，而是该值的 TypeScript 类型。
   ```typescript
   const a = { x: 0 };
   type T0 = typeof a; // { x: number }
   type T1 = typeof a.x; // number
   ```
   这种用法的typeof返回的是 TypeScript 类型，所以只能用在类型运算之中（即跟类型相关的代码之中），不能用在值运算。

   也就是说，同一段代码可能存在两种typeof运算符，一种用在值相关的 JavaScript 代码部分，另一种用在类型相关的 TypeScript 代码部分.

   ```typescript
   let a = 1;
   let b: typeof a;

   if (typeof a === "number") {
      b = a;
   }
   
   ```
   上面示例中，用到了两个typeof，第一个是类型运算，第二个是值运算。它们是不一样的，不要混淆。
   它们的一个重要区别在于，编译后，前者会保留，后者会被全部删除。

   由于编译时不会进行 JavaScript 的值运算，所以 TypeScript 规定，typeof 的参数只能是标识符，不能是需要运算的表达式。
   ```typescript
   type T = typeof Date(); // 报错
   ```

   另外，typeof命令的参数不能是类型。
   ```typescript
   type Age = number;
   type MyAge = typeof Age; // 报错
   ```

   typeof 是一个很重要的 TypeScript 运算符，有些场合不知道某个变量foo的类型，这时使用typeof foo就可以获得它的类型。


   块级类型声明:TypeScript 支持块级类型声明，即类型可以声明在代码块（用大括号表示）里面，并且只在当前代码块有效。

   ```typescript
   if (true) {
      type T = number;
      let v: T = 5;
    } else {
       type T = string;
       let v: T = "hello";
   }
   ```
   上面示例中，存在两个代码块，其中分别有一个类型T的声明。这两个声明都只在自己的代码块内部有效，在代码块外部无效
   

   类型兼容：
     ```typescript
     type T = number | string;

     let a: number = 1;
     let b: T = a;
     ```
     上面示例中，变量a和b的类型是不一样的，但是变量a赋值给变量b并不会报错。这时，我们就认为，b的类型兼容a的类型。

     TypeScript 为这种情况定义了一个专门术语。如果类型A的值可以赋值给类型B，那么类型A就称为类型B的子类型（subtype）。在上例中，类型number就是类型number|string的子类型。

     TypeScript 的一个规则是，凡是可以使用父类型的地方，都可以使用子类型，但是反过来不行。

     之所以有这样的规则，是因为子类型继承了父类型的所有特征，所以可以用在父类型的场合。但是，子类型还可能有一些父类型没有的特征，所以父类型不能用在子类型的场合。



     数组：JavaScript 数组在 TypeScript 里面分成两种类型，分别是数组（array）和元组（tuple）

     TypeScript 数组有一个根本特征：所有成员的类型必须相同，但是成员数量是不确定的，可以是无限数量的成员，也可以是零成员。

     数组的类型有两种写法。第一种写法是在数组成员的类型后面，加上一对方括号。
     ```typescript
     let arr: number[] = [1, 2, 3];
     ```
     上面示例中，数组arr的类型是number[]，其中number表示数组成员类型是number。


     如果数组成员的类型比较复杂，可以写在圆括号里面。
     ```typescript
     let arr: (number | string)[] = [1, "hello", 2, "world"];
     ```
     上面示例中，数组arr的类型是(number|string)[]，表示数组成员类型是number或string。
     这个例子里面的圆括号是必须的，否则因为竖杠|的优先级低于[]，TypeScript 会把number|string[]理解成number和string[]的联合类型。


     如果数组成员可以是任意类型，写成any[]。当然，这种写法是应该避免的
     ```typescript
     let arr: any[];
     ```

     数组类型的第二种写法是使用 TypeScript 内置的 Array 接口
     ```typescript
     let arr: Array<number> = [1, 2, 3];
     ```

     复杂成员类型则可以写成：
     ```typescript
     let arr: Array<number | string>;
     ```
    数组类型声明了以后，成员数量是不限制的，任意数量的成员都可以，也可以是空数组

    正是由于成员数量可以动态变化，所以 TypeScript 不会对数组边界进行检查，越界访问数组并不会报错。
    ```typescript
    let arr: number[] = [1, 2, 3];
    let foo = arr[3]; // 正确

    ```
   TypeScript 允许使用方括号读取数组成员的类型。
   ```typescript
   type Names = string[];
   type Name = Names[0]; // string

   ```
   上面示例中，类型Names是字符串数组，那么Names[0]返回的类型就是string。


   由于数组成员的索引类型都是number，所以读取成员类型也可以写成下面这样。
   ```typescript
   type Names = string[];
   type Name = Names[number]; // string
   ```
   上面示例中，Names[number]表示数组Names所有数值索引的成员类型，所以返回string。


   数组类型的推断:如果数组变量没有声明类型，TypeScript 就会推断数组成员的类型。这时，推断行为会因为值的不同，而有所不同。


   如果变量的初始值是空数组，那么 TypeScript 会推断数组类型是any[]。
   ```typescript
       // 推断为 any[]
     const arr = [];
   
   ```
   后面，为这个数组赋值时，TypeScript 会自动更新类型推断
   ```typescript
   const arr = [];
   arr; // 推断为 any[]

   arr.push(123);
   arr; // 推断类型为 number[]

   arr.push("abc");
   arr; // 推断类型为 (string|number)[]
   
   ```
   上面示例中，数组变量arr的初始值是空数组，然后随着新成员的加入，TypeScript 会自动修改推断的数组类型。

   但是，类型推断的自动更新只发生初始值为空数组的情况。如果初始值不是空数组，类型推断就不会更新。

   ```typescript
   // 推断类型为 number[]
    const arr = [123];
    arr.push("abc"); // 报错
   ```
   上面示例中，数组变量arr的初始值是[123]，TypeScript 就推断成员类型为number。新成员如果不是这个类型，TypeScript 就会报错，而不会更新类型推断



   只读数组，const断言：很多时候确实有声明为只读数组的需求，即不允许变动数组成员。
   TypeScript 允许声明只读数组，方法是在数组类型前面加上readonly关键字。

   ```typescript
   const arr: readonly number[] = [0, 1];
   arr[1] = 2; // 报错
   arr.push(3); // 报错
   delete arr[0]; // 报错
   
   ```
   上面示例中，arr是一个只读数组，删除、修改、新增数组成员都会报错。

   TypeScript 将readonly number[]与number[]视为两种不一样的类型，后者是前者的子类型。
   这是因为只读数组没有pop()、push()之类会改变原数组的方法，所以number[]的方法数量要多于readonly number[]，这意味着number[]其实是readonly number[]的子类型

    ```typescript
    let a1: number[] = [0, 1];
    let a2: readonly number[] = a1; // 正确
    a1 = a2; // 报错
    
    ```
    子类型number[]可以赋值给父类型readonly number[]，但是反过来就会报错

    只读数组是数组的父类型，所以它不能代替数组。这一点很容易产生令人困惑的报错。

    ```typescript
    function getSum(s: number[]) {
      // ...
    }

    const arr: readonly number[] = [1, 2, 3];

     getSum(arr); // 报错
    
    ```
    面示例中，函数getSum()的参数s是一个数组，传入只读数组就会报错。原因就是只读数组是数组的父类型，父类型不能替代子类型。这个问题的解决方法是使用类型断言getSum(arr as number[])


    readonly关键字不能与数组的泛型写法一起使用。
   ```typescript
     // 报错
    const arr: readonly Array<number> = [0, 1];
   ```
   上面示例中，readonly与数组的泛型写法一起使用，就会报错

   实际上，TypeScript 提供了两个专门的泛型，用来生成只读数组的类型。
   ```typescrit
   const a1: ReadonlyArray<number> = [0, 1];
   const a2: Readonly<number[]> = [0, 1];
   ```
   上面示例中，泛型ReadonlyArray<T>和Readonly<T[]>都可以用来生成只读数组类型。两者尖括号里面的写法不一样，Readonly<T[]>的尖括号里面是整个数组（number[]），而ReadonlyArray<T>的尖括号里面是数组成员（number）。


   只读数组还有一种声明方法，就是使用“const 断言”。
   ```typescript
   const arr = [0, 1] as const;
   arr[0] = [2]; // 报错
   ```
   上面示例中，as const告诉 TypeScript，推断类型时要把变量arr推断为只读数组，从而使得数组成员无法改变。

   
   多维数组：TypeScript 使用T[][]的形式，表示二维数组，T是最底层数组成员的类型。
   ```typescript
   var multi: number[][] = [
       [1, 2, 3],
       [23, 24, 25],
    ];
   ```
   上面示例中，变量multi的类型是number[][]，表示它是一个二维数组，最底层的数组成员类型是number



   元组:它表示成员类型可以自由设置的数组，即数组的各个成员的类型可以不同。
   元组必须明确声明每个成员的类型
   ```typescript
   const s: [string, string, boolean] = ["a", "b", true];
   ```
   上面示例中，元组s的前两个成员的类型是string，最后一个成员的类型是boolean。
   元组类型的写法，与上一章的数组有一个重大差异。数组的成员类型写在方括号外面（number[]），元组的成员类型是写在方括号里面（[number]）。


   TypeScript 的区分方法是，成员类型写在方括号里面的就是元组，写在外面的就是数组。
   ```typescript

   let a: [number] = [1];
   ```
   上面示例中，变量a是一个元组，只有一个成员，类型是number

   使用元组时，必须明确给出类型声明（上例的[number]），不能省略，否则 TypeScript 会把一个值自动推断为数组
   ```typescript
   // a 的类型为 (number | boolean)[]
    let a = [1, true];
   ```
   元组成员的类型可以添加问号后缀（?），表示该成员是可选的。
   ```typescript
   let a: [number, number?] = [1];
   ```
   上面示例中，元组a的第二个成员是可选的，可以省略
   注意，问号只能用于元组的尾部成员，也就是说，所有可选成员必须在必选成员之后。
   上面示例中，元组myTuple的最后两个成员是可选的。也就是说，它的成员数量可能有两个、三个和四个。


   由于需要声明每个成员的类型，所以大多数情况下，元组的成员数量是有限的，从类型声明就可以明确知道，元组包含多少个成员，越界的成员会报错。
   ```typescript
   let x: [string, string] = ["a", "b"];
    x[2] = "c"; // 报错
   
   ```

   但是，使用扩展运算符（...），可以表示不限成员数量的元组。
   ```typescript
     type NamedNums = [string, ...number[]];
     const a: NamedNums = ["A", 1, 2];
     const b: NamedNums = ["B", 1, 2, 3];
   ```
   上面示例中，元组类型NamedNums的第一个成员是字符串，后面的成员使用扩展运算符来展开一个数组，从而实现了不定数量的成员。

   扩展运算符用在元组的任意位置都可以，但是它后面只能是数组或元组。

   ```typescript
   type t1 = [string, number, ...boolean[]];
   type t2 = [string, ...boolean[], number];
   type t3 = [...boolean[], string, number];
   ```

   元组可以通过方括号，读取成员类型
   ```typescript
   type t = [string, number];
   type t1 = t[0]; // string
   type t2 = t[1]; // number
   type t3 = t[2]; // 报错
   ```
上面示例中，t[1]返回 1 号位置的成员类型
  
  由于元组的成员都是数值索引，即索引类型都是number，所以可以像下面这样读取。
  ```typescript
  type Tuple = [string, number, Date];
  type TupleEl = Tuple[number]; // string|number|Date
  
  ```
  上面示例中，Tuple[number]表示元组Tuple的所有数值索引的成员类型，所以返回string|number|Date，即这个类型是三种值的联合类型
 


 只读元组：元组也可以是只读的，不允许修改，有两种写法
 ```typescript
      // 写法一
     type t = readonly [number, string];

     // 写法二
     type t = Readonly<[number, string]>;

 ```
    跟数组一样，只读元组是元组的父类型。所以，元组可以替代只读元组，而只读元组不能替代元组。
    ```typescript
    type t1 = readonly [number, number];
    type t2 = [number, number];

    let x: t2 = [1, 2];
    let y: t1 = x; // 正确

    x = y; // 报错
    ```
    类型t1是只读元组，类型t2是普通元组。t2类型可以赋值给t1类型，反过来就会报错。

    ```typescript
    function distanceFromOrigin([x, y]: [number, number]) {
        return Math.sqrt(x ** 2 + y ** 2);
     }

    let point = [3, 4] as const;

    distanceFromOrigin(point); // 报错
    ```
    上面示例中，函数distanceFromOrigin()的参数是一个元组，传入只读元组就会报错，因为只读元组不能替代元组。

    读者可能注意到了，上例中[3, 4] as const的写法，在上一章讲到，生成的是只读数组，其实生成的同时也是只读元组。因为它生成的实际上是一个只读的“值类型”readonly [3, 4]，把它解读成只读数组或只读元组都可以。

    上面示例报错的解决方法，就是使用类型断言，在最后一行将传入的参数断言为普通元组.

    ```typescript
   
    distanceFromOrigin(point as [number, number]);
    ```


  成员数量的推断:如果没有可选成员和扩展运算符，TypeScript 会推断出元组的成员数量（即元组长度）
  ```typescript
  function f(point: [number, number]) {
  if (point.length === 3) {
    // 报错
    // ...
  }
}
  
  ```
  上面示例会报错，原因是 TypeScript 发现元组point的长度是2，不可能等于3，这个判断无意义

  如果包含了可选成员，TypeScript 会推断出可能的成员数量 
  ```typescript
  function f(point: [number, number?, number?]) {
  if (point.length === 4) {
    // 报错
    // ...
  }
}
```
上面示例会报错，原因是 TypeScript 发现point.length的类型是1|2|3，不可能等于4。
  

如果使用了扩展运算符，TypeScript 就无法推断出成员数量
```typescript
    const myTuple: [...string[]] = ["a", "b", "c"];

    if (myTuple.length === 4) {
     // 正确
     // ...
    }

```
一旦扩展运算符使得元组的成员数量无法推断，TypeScript 内部就会把该元组当成数组处理。


扩展运算符与成员数量：
   扩展运算符（...）将数组（注意，不是元组）转换成一个逗号分隔的序列，这时 TypeScript 会认为这个序列的成员数量是不确定的，因为数组的成员数量是不确定的。

   这导致如果函数调用时，使用扩展运算符传入函数参数，可能发生参数数量与数组长度不匹配的报错。

   ```typescript
   
   const arr = [1, 2];

   function add(x: number, y: number) {
  // ...
   }

   add(...arr); // 报错
   
   ```
   上面示例会报错，原因是函数add()只能接受两个参数，但是传入的是...arr，TypeScript 认为转换后的参数个数是不确定的。

   有些函数可以接受任意数量的参数，这时使用扩展运算符就不会报错
   ```typescript
   const arr = [1, 2, 3];
   console.log(...arr); // 正确
   
   ```

   解决这个问题的一个方法，就是把成员数量不确定的数组，写成成员数量确定的元组，再使用扩展运算符。

```typescript
const arr: [number, number] = [1, 2];

function add(x: number, y: number) {
  // ...
}

add(...arr); // 正确
```

另一种写法是使用as const断言。
```typescript

const arr = [1, 2] as const;
```
上面这种写法也可以，因为 TypeScript 会认为arr的类型是readonly [1, 2]，这是一个只读的值类型，可以当作数组，也可以当作元组。只读数组只能读取不能被删除、修改、添加