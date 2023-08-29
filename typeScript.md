### any 类型

变量如果无法推断出类型，TypeScript 就会认为该变量的类型是any。
编译选项noImplicitAny，打开该选项，只要推断出any类型就会报错.
any类型除了关闭类型检查，还会“污染”其他变量.

### unknown 类型

严格版的any ,所有类型的值都可以分配给unknown类型
unknown类型的变量，不能直接赋值给其他类型的变量（除了any类型和unknown类型）
只有经过“类型缩小”，unknown类型变量才可以使用。

### never 类型

由于不存在任何属于“空类型”的值，所以该类型被称为never，即不可能有这样的值
任何类型都包含了never类型。因此，never类型是任何其他类型所共有的

### 包装对象类型与字面量类型
为了区分这两种情况，TypeScript 对五种原始类型分别提供了大写和小写两种类型。

Boolean 和 boolean
String 和 string
Number 和 number
BigInt 和 bigint
Symbol 和 symbol
其中，大写类型同时包含包装对象和字面量两种情况，小写类型只包含字面量，不包含包装对象。

建议只使用小写类型，不使用大写类型。因为绝大部分使用原始类型的场合，都是使用字面量，不使用包装对象。

除了undefined和null这两个值不能转为对象，其他任何值都可以赋值给Object类型
小写的object只包含对象、数组和函数，不包括原始类型的值。

## 各种类型
任何其他类型的变量都可以赋值为undefined或null

单个值也是一种类型，称为“值类型”

子类型x不能赋值为父类型y，但是反过来是可以的。如果一定要让子类型可以赋值为父类型的值，就要用到类型断言 as

联合类型（union types）指的是多个类型组成的一个新类型，使用符号|表示。
交叉类型（intersection types）指的多个类型组成的一个新类型，交叉类型常常用来为对象类型添加新属性

## 类中修饰符
属性readonly 修饰符，就表示该属性是只读的。实例对象不能修改这个属性。
类可以使用 extends 关键字继承另一个类（这里又称“基类”）的所有属性和方法。
public修饰符表示这是公开成员，外部可以自由访问。
private修饰符表示私有成员，只能用在当前类的内部，类的实例和子类都不能使用该成员。
protected修饰符表示该成员是保护成员，只能在类的内部使用该成员，实例无法使用该成员，但是子类内部可以使用。
类的内部可以使用static关键字，定义静态成员。静态成员是只能通过类本身使用的成员，不能通过实例对象使用。
关键字abstract，表示该类不能被实例化，只能当作其他类的模板

## 泛型
类型参数的约束条件采用下面的形式。<TypeParameter extends ConstraintType>
```typeScript
function comp<T extends { length: number }>(
  a: T,
  b: T
) {
  if (a.length >= b.length) {
    return a;
  }
  return b;
}
```

泛型使用注意点：
1）尽量少用泛型。（2）类型参数越少越好。（3）类型参数需要出现两次。（4）泛型可以嵌套。

## Enum 枚举

Enum 成员默认不必赋值，系统会从零开始逐一递增，
多个同名的 Enum 结构会自动合并。

keyof 运算符可以取出 Enum 结构的所有成员名，作为联合类型返回。
in 运算符可以取出所有的  Enum 结构的所有成员的值

数值 Enum 存在反向映射，即可以通过成员值获得成员名。比如 Color[0]
```typeScript
enum Color {
        Red,
        Green,
        Blue
      }

type foo = keyof typeof Color

type oo = {[key in Color]:any}
```

