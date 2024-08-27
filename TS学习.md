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