# fantasy-land

`Fantasy-Land-Specification` 又被称为**代数类型规范**。

fantasy-land规定了通用代数数据结构及其通用方法，可以看作是 js 函数式编程的数据结构规范。该项目规定了通用代数数据结构的互操作性。

## 依赖关系图

![fantasy-land 依赖关系图](../images/fantasy-land dependencies.png)



## 类型签名

### 类型约束 `=>`

`=>` 表示把类型约束为一个特定的接口(interface)

```javascript
sort :: Ord a => [a] -> [a]
// 表示a要实现Ord接口

assertEqual :: (Eq a, Show a) => a -> a -> Assertion

pluck :: Functor f => k → f {k: v} → f v
```



### 方法类型的构造函数 `~>`
什么是方法：当一个函数是一个对象的属性的时，它就被称为方法。方法是作为对象的属性的函数。

`a ~> a -> a` 表示是一个方法，它接受 `a` 类型的参数，并返回一个 `a` 类型的值。

**泛型-TypeVariable —— 类型变量**

举例说明：

```html
具有Setoid 必须要提供 equals方法
equals:: Setoid a => a ~> a -> Boolean

表示：equal方法，接受一个a类型的参数,返回一个Boolean类型的值。
```



## 代数

代数是遵循一定法则的、具有封闭性的，一系列值及一些列操作的集合。

每个 Fantasy Land代数是一个单独的规范。一个代数可能依赖于其他必须实现的代数。

### Setoid

在数学中，setoid是具有等价关系的集合。

必须要实现`equals`方法。



### Ord

实现 *Ord* 规范的值还必须要实现 *Setoid* 规范。

必须要实现`lte` 方法。



### Semigroupoid

必须要满足结合律 `a.compose(b).compose(c) === a.compose(b.compose(c))`

必须要提供`compose` 组合方法。



### Semigroup

必须要满足结合律 `a.concat(b).concat(c)` 等价于 `a.concat(b.concat(c))` （结合性）

必须要实现`concat`方法。



### Monoid

实现 *Monoid* 规范的值还必须实现 *Semigroup* 规范

必须要实现`empty`方法。



### Group

实现 *Group* 规范的值还必须实现 *Monoid* 规范

还必须实现`invert` 方法。



###  Functor

任何存储一个值，并且实现了 `map` 方法的类（或者构造器函数）或者数据类型都被称为**Functor**。

Functor的定律：

1. `u.map(a=>a)`等价于`u`(同一性)
2. `u.map(x=>f(g(x)))` 等价于 `u.map(g).map(f)` (组合性)

必须实现`map` 方法。



### Apply

实现 *Apply* 规范的值还必须实现 *Functor* 规范。

必须实现`ap` 方法。



### Applicative

实现 *Applicative* 规范的值还必须实现 *Apply* 规范。。

必须实现`of` 方法。



### Chain

实现 *Chain* 规范的值还必须实现 *Apply* 规范

`m.chain(f).chain(g)` 等价于 `m.chain(x=> f(x).chain(g))` (结合性)。



#### `chain` 方法

```javascript
chain :: Chain m => (a -> m b) -> m a -> m b
```



### Monad

实现 *Monad* 规范的值还必须实现 *Applicative* 和 *Chain* 规范。

1. `M.of(a).chain(f)` 等价于 `f(a)` (左同一性)
2. `m.chain(M.of)` 等价于 `m` (右同一性)

一般情况下我们使用更具体的，更有用的Monad,常用的Maybe Monad或者 Either Monad.



#### Maybe Monad

它的特殊之处是：**它用来处理 *null* 或者 *undefined* 。**

特别是，如果存储的数据是 *null* 或者 *undefined* ，那么它的 *map* 函数完全不会执行，因而避免了 *null* 或者 *undefined* 的问题。

空值类型的 *Maybe* 是`Nothing`

非空值类型的 *Maybe* 是`Just`

虽然 Just 和 Nothing 有相同的方法（map、orElse 等），不过 Just 的实例要做一些事情，而 Nothing 的实例是什么事都不做。



#### Either Monad

可以用它处理错误。

*Either Monad* 提供两种构造器：`Either.Left` 和`Either.Right` ，**Left 和 Right 都是 Monad！理念是在 Left 中存储错误和异常，而在 Right 中存储有用的值**。

虽然 Left 和 Right 都提供 map、chain 等方法，但是 Left 构造因为存储的是错误，所以不做任何事情。而 Right 构造器会实现所有的函数，因为它包含了实际的结果。















