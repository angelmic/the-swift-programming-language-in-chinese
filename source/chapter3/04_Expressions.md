# 表達式（Expressions）
-----------------

> 1.0
> 翻譯：[sg552](https://github.com/sg552)
> 校對：[numbbbbb](https://github.com/numbbbbb), [stanzhai](https://github.com/stanzhai)

> 2.0
> 翻譯+校對：[EudeMorgen](https://github.com/EudeMorgen)

> 2.1
> 翻譯：[mmoaay](https://github.com/mmoaay)

> 2.2 
> 校對：[175](https://github.com/Brian175)

> 3.0 
> 翻譯+校對：[chenmingjia](https://github.com/chenmingjia)

本頁包含內容：

- [前綴表達式](#prefix_expressions)
    - [try 運算符](#try_operator)
- [二元表達式](#binary_expressions)
    - [賦值表達式](#assignment_operator)
    - [三元條件運算符](#ternary_conditional_operator)
    - [類型轉換運算符](#type-casting_operators)
- [基本表達式](#primary_expressions)
    - [字面量表達式](#literal_expression)
    - [self 表達式](#self_expression)
    - [超類表達式](#superclass_expression)
    - [閉包表達式](#closure_expression)
    - [隱式成員表達式](#implicit_member_expression)
    - [圓括號表達式](#parenthesized_expression)
    - [通配符表達式](#wildcard_expression)
    - [選擇器表達式](#selector_expression)
- [後綴表達式](#postfix_expressions)
    - [函數調用表達式](#function_call_expression) 
    - [構造器表達式](#initializer_expression)
    - [顯式成員表達式](#explicit_member_expression)
    - [後綴 self 表達式](#postfix_self_expression)
    - [dynamicType 表達式](#dynamic_type_expression)
    - [下標表達式](#subscript_expression)
    - [強制取值表達式](#forced-Value_expression)
    - [可選鏈表達式](#optional-chaining_expression)

Swift 中存在四種表達式：前綴表達式，二元表達式，基本表達式和後綴表達式。表達式在返回一個值的同時還可以引發副作用。

通過前綴表達式和二元表達式可以對簡單表達式使用各種運算符。基本表達式從概念上講是最簡單的一種表達式，它是一種訪問值的方式。後綴表達式則允許你建立復雜的表達式，例如函數調用和成員訪問。每種表達式都在下面有詳細論述。

> 表達式語法  
<a name="expression"></a>
> *表達式* → [*try運算符*](#try-operator)<sub>可選</sub> [*前綴表達式*](#prefix-expression) [*二元表達式列表*](#binary-expressions)<sub>可選</sub>  
<a name="expression-list"></a>
> *表達式列表* → [*表達式*](#expression) | [*表達式*](#expression) **,** [*表達式列表*](#expression-list)  

<a name="prefix_expressions"></a>
## 前綴表達式
 
前綴表達式由可選的前綴運算符和表達式組成。前綴運算符只接收一個參數，表達式則緊隨其後。

關於這些運算符的更多信息，請參閱 [基本運算符](../chapter2/02_Basic_Operators.md) 和 [高級運算符](../chapter2/25_Advanced_Operators.md)。

關於 Swift 標准庫提供的運算符的更多信息，請參閱 [*Swift Standard Library Operators Reference*](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Reference/Swift_StandardLibrary_Operators/index.html#//apple_ref/doc/uid/TP40016054)。

除了標准庫運算符，你也可以對某個變量使用 `&` 運算符，從而將其傳遞給函數的輸入輸出參數。更多信息，請參閱 [輸入輸出參數](../chapter2/06_Functions.html#in_out_parameters)。

> 前綴表達式語法  
<a name="prefix-expression"></a>
> *前綴表達式* → [*前綴運算符*](02_Lexical_Structure.md#prefix-operator)<sub>可選</sub> [*後綴表達式*](#postfix-expression)  
> *前綴表達式* → [*輸入輸出表達式*](#in-out-expression)  
<a name="in-out-expression"></a>
> *輸入輸出表達式* → **&** [*標識符*](02_Lexical_Structure.md#identifier)  

<a name="try_operator"></a>
### try 運算符

try 表達式由 `try` 運算符加上緊隨其後的可拋出錯誤的表達式組成，形式如下：

> try `可拋出錯誤的表達式`

可選的 try 表達式由 `try?` 運算符加上緊隨其後的可拋出錯誤的表達式組成，形式如下：

> try? `可拋出錯誤的表達式`

如果可拋出錯誤的表達式沒有拋出錯誤，整個表達式返回的可選值將包含可拋出錯誤的表達式的返回值，否則，該可選值為 `nil`。

強制的 try 表達式由 `try!` 運算符加上緊隨其後的可拋出錯誤的表達式組成，形式如下：

> try! `可拋出錯誤的表達式`

如果可拋出錯誤的表達式拋出了錯誤，將會引發運行時錯誤。

在二進制運算符左側的表達式被標記上 `try`、`try?` 或者 `try!` 時，這個運算符對整個二進制表達式都產生作用。也就是說，你可以使用括號來明確運算符的作用范圍。

```swift
sum = try someThrowingFunction() + anotherThrowingFunction()   // try 對兩個函數調用都產生作用
sum = try (someThrowingFunction() + anotherThrowingFunction()) // try 對兩個函數調用都產生作用
sum = (try someThrowingFunction()) + anotherThrowingFunction() // 錯誤：try 只對第一個函數調用產生作用
```

`try` 表達式不能出現在二進制運算符的的右側，除非二進制運算符是賦值運算符或者 `try` 表達式是被圓括號括起來的。

關於 `try`、`try?` 和 `try!` 的更多信息，以及該如何使用的例子，請參閱 [錯誤處理](../chapter2/18_Error_Handling.html)。
> try 表達式語法  
<a name="try-operator"></a> 
> *try 運算符* → **try** | **try?** | **try!**

<a name="binary_expressions"></a>
## 二元表達式

二元表達式由中綴運算符和左右參數表達式組成。形式如下：

> `左側參數` `二元運算符` `右側參數`

關於這些運算符的更多信息，請參閱 [基本運算符](../chapter2/02_Basic_Operators.md) 和 [高級運算符](../chapter2/25_Advanced_Operators.md)。

關於 Swift 標准庫提供的運算符的更多信息，請參閱 [*Swift Standard Library Operators Reference*](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Reference/Swift_StandardLibrary_Operators/index.html#//apple_ref/doc/uid/TP40016054)。

> 注意  
> 在解析時，一個二元表達式將作為一個扁平列表表示，然後根據運算符的優先級，再進一步進行組合。例如，`2 + 3 * 5` 首先被看作具有五個元素的列表，即 `2`、`+`、`3`、`*`、`5`，隨後根據運算符優先級組合為 `(2 + (3 * 5))`。

<a name="binary-expression"></a>
> 二元表達式語法  
> *二元表達式* → [*二元運算符*](02_Lexical_Structure.md#binary-operator) [*前綴表達式*](#prefix-expression)  
> *二元表達式* → [*賦值運算符*](#assignment-operator) [*try運算符*](#try-operator)<sub>可選</sub> [*前綴表達式*](#prefix-expression)  
> *二元表達式* → [*條件運算符*](#conditional-operator) [*try運算符*](#try-operator)<sub>可選</sub> [*前綴表達式*](#prefix-expression)  
> *二元表達式* → [*類型轉換運算符*](#type-casting-operator)  
<a name="binary-expressions"></a>
> *二元表達式列表* → [*二元表達式*](#binary-expression) [*二元表達式列表*](#binary-expressions)<sub>可選</sub>

<a name="assignment_operator"></a>
### 賦值表達式

賦值表達式會為某個給定的表達式賦值，形式如下；

> `表達式` = `值`

右邊的值會被賦值給左邊的表達式。如果左邊表達式是一個元組，那麼右邊必須是一個具有同樣元素個數的元組。（嵌套元組也是允許的。）右邊的值中的每一部分都會被賦值給左邊的表達式中的相應部分。例如：

```swift
(a, _, (b, c)) = ("test", 9.45, (12, 3))
// a 為 "test"，b 為 12，c 為 3，9.45 會被忽略
```

賦值運算符不返回任何值。

> 賦值運算符語法  
<a name="assignment-operator"></a>
> *賦值運算符* → **=**  

<a name="ternary_conditional_operator"></a>
### 三元條件運算符

三元條件運算符會根據條件來對兩個給定表達式中的一個進行求值，形式如下：

> `條件` ? `表達式（條件為真則使用）` : `表達式（條件為假則使用）`

如果條件為真，那麼對第一個表達式進行求值並返回結果。否則，對第二個表達式進行求值並返回結果。未使用的表達式不會進行求值。

關於使用三元條件運算符的例子，請參閱 [三元條件運算符](../chapter2/02_Basic_Operators.md#ternary_conditional_operator)。

> 三元條件運算符語法  
<a name="conditional-operator"></a>
> *三元條件運算符* → **?** [try運算符](#try-operator)<sub>可選</sub> [*表達式*](#expression) **:**  

<a name="type-casting_operators"></a>
### 類型轉換運算符

有 4 種類型轉換運算符：`is`、`as`、`as? `和`as!`。它們有如下的形式：

> `表達式` is `類型`  
`表達式` as `類型`  
`表達式` as? `類型`  
`表達式` as! `類型`  

`is` 運算符在運行時檢查表達式能否向下轉化為指定的類型，如果可以則返回 `ture`，否則返回 `false`。

`as` 運算符在編譯時執行向上轉換和橋接。向上轉換可將表達式轉換成超類的實例而無需使用任何中間變量。以下表達式是等價的：

```swift
func f(any: Any) { print("Function for Any") }
func f(int: Int) { print("Function for Int") }
let x = 10
f(x)
// 打印 「Function for Int」
 
let y: Any = x
f(y)
// 打印 「Function for Any」
 
f(x as Any)
// 打印 「Function for Any」
```

橋接可將 Swift 標准庫中的類型（例如 `String`）作為一個與之相關的 Foundation 類型（例如 `NSString`）來使用，而不需要新建一個實例。關於橋接的更多信息，請參閱 [*Using Swift with Cocoa and Objective-C (Swift2.2)*](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/BuildingCocoaApps/index.html#//apple_ref/doc/uid/TP40014216) 中的 [Working with Cocoa Data Types](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/BuildingCocoaApps/WorkingWithCocoaDataTypes.html#//apple_ref/doc/uid/TP40014216-CH6)。

`as?` 運算符有條件地執行類型轉換，返回目標類型的可選值。在運行時，如果轉換成功，返回的可選值將包含轉換後的值，否則返回 `nil`。如果在編譯時就能確定轉換一定會成功或是失敗，則會導致編譯報錯。

`as!` 運算符執行強制類型轉換，返回目標類型的非可選值。如果轉換失敗，則會導致運行時錯誤。表達式 `x as! T` 效果等同於 `(x as? T)!`。

關於類型轉換的更多內容和例子，請參閱 [類型轉換](../chapter2/19_Type_Casting.md)。

<a name="type-casting-operator"></a>
> 類型轉換運算符語法  
> *類型轉換運算符* → **is** [*類型*](03_Types.md#type)  
> *類型轉換運算符* → **as** [*類型*](03_Types.md#type)  
> *類型轉換運算符* → **as** **?** [*類型*](03_Types.md#type)  
> *類型轉換運算符* → **as** **!** [*類型*](03_Types.md#type)   

<a name="primary_expressions"></a>
## 基本表達式

基本表達式是最基本的表達式。它們可以單獨使用，也可以跟前綴表達式、二元表達式、後綴表達式組合使用。

> 基本表達式語法  
<a name="primary-expression"></a>
> *基本表達式* → [*標識符*](02_Lexical_Structure.md#identifier) [*泛型實參子句*](08_Generic_Parameters_and_Arguments.md#generic-argument-clause)<sub>可選</sub>  
> *基本表達式* → [*字面量表達式*](#literal-expression)  
> *基本表達式* → [*self表達式*](#self-expression)  
> *基本表達式* → [*超類表達式*](#superclass-expression)  
> *基本表達式* → [*閉包表達式*](#closure-expression)  
> *基本表達式* → [*圓括號表達式*](#parenthesized-expression)  
> *基本表達式* → [*隱式成員表達式*](#implicit-member-expression)  
> *基本表達式* → [*通配符表達式*](#wildcard-expression)  
> *基本表達式* → [*選擇器表達式*](#selector-expression)

<a name="literal_expression"></a>
### 字面量表達式

字面量表達式可由普通字面量（例如字符串或者數字），字典或者數組字面量，或者下面列表中的特殊字面量組成：

字面量 | 類型 | 值
:------------- | :---------- | :----------
`#file` | `String` | 所在的文件名
`#line` | `Int` | 所在的行數
`#column` | `Int` | 所在的列數
`#function` | `String` | 所在的聲明的名字

`#line`除了上述含義外，還有另一種含義。當它出現在單獨一行時，會被理解成行控制語句，請參閱[線路控制語句](../chapter3/10_Statements.md#線路控制語句)。

對於 `＃function`，在函數中會返回當前函數的名字，在方法中會返回當前方法的名字，在屬性的存取器中會返回屬性的名字，在特殊的成員如 `init` 或 `subscript` 中會返回這個關鍵字的名字，在某個文件中會返回當前模塊的名字。

`#function` 作為函數或者方法的默認參數值時，該字面量的值取決於函數或方法的調用環境。

```swift
func logFunctionName(string: String = #function) {
    print(string)
}
func myFunction() {
    logFunctionName() 
}
myFunction() // 打印 「myFunction()」
```

數組字面量是值的有序集合，形式如下：

> [`值 1`, `值 2`, `...`]

數組中的最後一個表達式可以緊跟一個逗號。數組字面量的類型是 `[T]`，這個 `T` 就是數組中元素的類型。如果數組中包含多種類型，`T` 則是跟這些類型最近的的公共父類型。空數組字面量由一組方括號定義，可用來創建特定類型的空數組。

```swift
var emptyArray: [Double] = []
```

字典字面量是一個包含無序鍵值對的集合，形式如下：

> [`鍵 1` : `值 1`, `鍵 2` : `值 2`, `...`]

字典中的最後一個表達式可以緊跟一個逗號。字典字面量的類型是 `[Key : Value]`，`Key` 表示鍵的類型，`Value` 表示值的類型。如果字典中包含多種類型，那麼 `Key` 表示的類型則為所有鍵最接近的公共父類型，`Value` 與之相似。一個空的字典字面量由方括號中加一個冒號組成（`[:]`），從而與空數組字面量區分開，可以使用空字典字面量來創建特定類型的字典。

```swift
var emptyDictionary: [String : Double] = [:]
```

> 字面量表達式語法  

<a name="literal-expression"></a>
> *字面量表達式* → [*字面量*](02_Lexical_Structure.md#literal)  
> *字面量表達式* → [*數組字面量*](#array-literal) | [*字典字面量*](#dictionary-literal)  
> *字面量表達式* → **#file** | **#line** | **#column** | **#function**  

<a name="array-literal"></a>
> *數組字面量* → **[** [*數組字面量項列表*](#array-literal-items)<sub>可選</sub> **]**  
<a name="array-literal-items"></a>
> *數組字面量項列表* → [*數組字面量項*](#array-literal-item) **,**<sub>可選</sub> | [*數組字面量項*](#array-literal-item) **,** [*數組字面量項列表*](#array-literal-items)  
<a name="array-literal-item"></a>
> *數組字面量項* → [*表達式*](#expression)  

<a name="dictionary-literal"></a>
> *字典字面量* → **[** [*字典字面量項列表*](#dictionary-literal-items) **]** | **[** **:** **]**  
<a name="dictionary-literal-items"></a>
> *字典字面量項列表* → [*字典字面量項*](#dictionary-literal-item) **,**<sub>可選</sub> | [*字典字面量項*](#dictionary-literal-item) **,** [*字典字面量項列表*](#dictionary-literal-items)  
<a name="dictionary-literal-item"></a>
> *字典字面量項* → [*表達式*](#expression) **:** [*表達式*](#expression)  

<a name="self_expression"></a>
### self 表達式

`self` 表達式是對當前類型或者當前實例的顯式引用，它有如下形式：

> self  
> self.`成員名稱`  
> self[`下標索引`]  
> self(`構造器參數`)  
> self.init(`構造器參數`)

如果在構造器、下標、實例方法中，`self` 引用的是當前類型的實例。在一個類型方法中，`self` 引用的是當前的類型。

當訪問成員時，`self` 可用來區分重名變量，例如函數的參數：

```swift
class SomeClass {
    var greeting: String
    init(greeting: String) {
        self.greeting = greeting
    }
}
```

在 `mutating` 方法中，你可以對 `self` 重新賦值：

```swift
struct Point {
    var x = 0.0, y = 0.0
    mutating func moveByX(deltaX: Double, y deltaY: Double) {
        self = Point(x: x + deltaX, y: y + deltaY)
    }
}
```

> self 表達式語法  
<a name="self-expression"></a>
> *self 表達式* → **self**  | [*self 方法表達式*](#self-method-expression) ｜ [*self 下標表達式*](#self-subscript-expression) | [*self 構造器表達式*](#self-initializer-expression)
>
<a name="self-method-expression"></a>
> *self 方法表達式* → **self** **.** [*標識符*](02_Lexical_Structure.md#identifier)  
<a name="self-subscript-expression"></a>
> *self 下標表達式* → **self** **[** [*表達式*](#expression) **]** 
<a name="self-initializer-expression"></a> 
> *self 構造器表達式* → **self** **.** **init**  

<a name="superclass_expression"></a>
### 超類表達式

超類表達式可以使我們在某個類中訪問它的超類，它有如下形式：

> super.`成員名稱`  
> super[`下標索引`]  
> super.init(`構造器參數`)

第一種形式用來訪問超類的某個成員，第二種形式用來訪問超類的下標，第三種形式用來訪問超類的構造器。

子類可以通過超類表達式在它們的成員、下標和構造器中使用超類中的實現。

> 超類表達式語法  
<a name="superclass-expression"></a>
> *超類表達式* → [*超類方法表達式*](#superclass-method-expression) | [*超類下標表達式*](#superclass-subscript-expression) | [*超類構造器表達式*](#superclass-initializer-expression)  
<a name="superclass-method-expression"></a>
> *超類方法表達式* → **super** **.** [*標識符*](02_Lexical_Structure.md#identifier)  
<a name="superclass-subscript-expression"></a>
> *超類下標表達式* → **super** **[** [*表達式*](#expression) **]**  
<a name="superclass-initializer-expression"></a>
> *超類構造器表達式* → **super** **.** **init**  

<a name="closure_expression"></a>
### 閉包表達式

閉包表達式會創建一個閉包，在其他語言中也叫 *lambda* 或匿名函數。跟函數一樣，閉包包含了待執行的代碼，不同的是閉包還會捕獲所在環境中的常量和變量。它的形式如下：

```swift
{ (parameters) -> return type in  
    statements  
}
```

閉包的參數聲明形式跟函數一樣，請參閱 [函數聲明](05_Declarations.md#function_declaration)。

閉包還有幾種特殊的形式，能讓閉包使用起來更加簡潔：

- 閉包可以省略它的參數和返回值的類型。如果省略了參數名和所有的類型，也要省略 `in` 關鍵字。如果被省略的類型無法被編譯器推斷，那麼就會導致編譯錯誤。
- 閉包可以省略參數名，參數會被隱式命名為 `$` 加上其索引位置，例如 `$0`、`$1`、`$2` 分別表示第一個、第二個、第三個參數，以此類推。
- 如果閉包中只包含一個表達式，那麼該表達式的結果就會被視為閉包的返回值。表達式結果的類型也會被推斷為閉包的返回類型。

下面幾個閉包表達式是等價的：

```swift
myFunction {
    (x: Int, y: Int) -> Int in
    return x + y
}

myFunction {
    (x, y) in
    return x + y
}

myFunction { return $0 + $1 }

myFunction { $0 + $1 }
```

關於如何將閉包作為參數來傳遞的內容，請參閱 [函數調用表達式](#function_call_expression)。

#### 捕獲列表

默認情況下，閉包會捕獲附近作用域中的常量和變量，並使用強引用指向它們。你可以通過一個捕獲列表來顯式指定它的捕獲行為。

捕獲列表在參數列表之前，由中括號括起來，裡面是由逗號分隔的一系列表達式。一旦使用了捕獲列表，就必須使用 `in` 關鍵字，即使省略了參數名、參數類型和返回類型。

捕獲列表中的項會在閉包創建時被初始化。每一項都會用閉包附近作用域中的同名常量或者變量的值初始化。例如下面的代碼示例中，捕獲列表包含 `a` 而不包含 `b`，這將導致這兩個變量具有不同的行為。

```swift
var a = 0
var b = 0
let closure = { [a] in
    print(a, b)
}

a = 10
b = 10
closure()
// 打印 「0 10」
```

在示例中，變量 `b` 只有一個，然而，變量 `a` 有兩個，一個在閉包外，一個在閉包內。閉包內的變量 `a` 會在閉包創建時用閉包外的變量 `a` 的值來初始化，除此之外它們並無其他聯系。這意味著在閉包創建後，改變某個 `a` 的值都不會對另一個 `a` 的值造成任何影響。與此相反，閉包內外都是同一個變量 `b`，因此在閉包外改變其值，閉包內的值也會受影響。

如果閉包捕獲的值具有引用語義則有所不同。例如，下面示例中，有兩個變量 `x`，一個在閉包外，一個在閉包內，由於它們的值是引用語義，雖然這是兩個不同的變量，它們卻都引用著同一實例。

```swift
class SimpleClass {
    var value: Int = 0
}
var x = SimpleClass()
var y = SimpleClass()
let closure = { [x] in
    print(x.value, y.value)
}

x.value = 10
y.value = 10
closure()
// 打印 「10 10」
```

如果捕獲列表中的值是類類型，你可以使用 `weak` 或者 `unowned` 來修飾它，閉包會分別用弱引用和無主引用來捕獲該值。

```swift
myFunction { print(self.title) }                   // 以強引用捕獲
myFunction { [weak self] in print(self!.title) }   // 以弱引用捕獲
myFunction { [unowned self] in print(self.title) } // 以無主引用捕獲
```

在捕獲列表中，也可以將任意表達式的值綁定到一個常量上。該表達式會在閉包被創建時進行求值，閉包會按照指定的引用類型來捕獲表達式的值。例如：

```swift
// 以弱引用捕獲 self.parent 並賦值給 parent
myFunction { [weak parent = self.parent] in print(parent!.title) }
```

關於閉包表達式的更多信息和例子，請參閱 [閉包表達式](../chapter2/07_Closures.md#closure_expressions)。關於捕獲列表的更多信息和例子，請參閱 [解決閉包引起的循環強引用](../chapter2/16_Automatic_Reference_Counting.md#resolving_strong_reference_cycles_for_closures)。

> 閉包表達式語法  

<a name="closure-expression"></a>
> *閉包表達式* → **{** [*閉包簽名*](#closure-signature)<sub>可選</sub> [*語句*](10_Statements.md#statements) **}**  

<a name="closure-signature"></a>
> *閉包簽名* → [*參數子句*](05_Declarations.md#parameter-clause) [*函數結果*](05_Declarations.md#function-result)<sub>可選</sub> **in**  
> *閉包簽名* → [*標識符列表*](02_Lexical_Structure.md#identifier-list) [*函數結果*](05_Declarations.md#function-result)<sub>可選</sub> **in**  
> *閉包簽名* → [*捕獲列表*](#capture-list) [*參數子句*](05_Declarations.md#parameter-clause) [*函數結果*](05_Declarations.md#function-result)<sub>可選</sub> **in**  
> *閉包簽名* → [*捕獲列表*](#capture-list) [*標識符列表*](02_Lexical_Structure.md#identifier-list) [*函數結果*](05_Declarations.md#function-result)<sub>可選</sub> **in**  
> *閉包簽名* → [*捕獲列表*](#capture-list) **in**  

<a name="capture-list"></a>
> *捕獲列表* → **[** [*捕獲列表項列表*](#capture-list-items) **]**  
<a name="capture-list-items"></a>
> *捕獲列表項列表* → [*捕獲列表項*](#capture-list-item) | [*捕獲列表項*](#capture-list-item) **,** [*捕獲列表項列表*](#capture-list-items)
<a name="capture-list-item"></a>  
> *捕獲列表項* → [*捕獲說明符*](#capture-specifier)<sub>可選</sub> [*表達式*](#expression)
<a name="capture-specifier"></a>  
> *捕獲說明符* → **weak** | **unowned** | **unowned(safe)** | **unowned(unsafe)**  

<a name="implicit_member_expression"></a>
### 隱式成員表達式

若類型可被推斷出來，可以使用隱式成員表達式來訪問某個類型的成員（例如某個枚舉成員或某個類型方法），形式如下：

> .`成員名稱`

例如：

```swift
var x = MyEnumeration.SomeValue
x = .AnotherValue
```

> 隱式成員表達式語法  
<a name="implicit-member-expression"></a>
> *隱式成員表達式* → **.** [*標識符*](02_Lexical_Structure.md#identifier)  

<a name="parenthesized_expression"></a>
### 圓括號表達式

圓括號表達式由圓括號和其中多個逗號分隔的子表達式組成。每個子表達式前面可以有一個標識符，用冒號隔開。圓括號表達式形式如下：

> (`標識符 1` : `表達式 1`, `標識符 2` : `表達式 2`, `...`)

使用圓括號表達式來創建元組，然後將其作為參數傳遞給函數。如果某個圓括號表達式中只有一個子表達式，那麼它的類型就是子表達式的類型。例如，表達式 `(1)` 的類型是 `Int`，而不是 `(Int)`。

> 圓括號表達式語法  
<a name="parenthesized-expression"></a>
> *圓括號表達式* → **(** [*表達式元素列表*](#expression-element-list)<sub>可選</sub> **)**
<a name="expression-element-list"></a>  
> *表達式元素列表* → [*表達式元素*](#expression-element) | [*表達式元素*](#expression-element) **,** [*表達式元素列表*](#expression-element-list)
<a name="expression-element"></a>  
> *表達式元素* → [*表達式*](#expression) | [*標識符*](02_Lexical_Structure.md#identifier) **:** [*表達式*](#expression)  

<a name="wildcard_expression"></a>
### 通配符表達式

通配符表達式可以在賦值過程中顯式忽略某個值。例如下面的代碼中，`10` 被賦值給 `x`，而 `20` 則被忽略：

```swift
(x, _) = (10, 20)
// x 為 10，20 被忽略
```

> 通配符表達式語法  
<a name="wildcard-expression"></a>
> *通配符表達式* → **_**  

<a name="selector_expression"></a>
### 選擇器表達式

選擇器表達式可以讓你通過選擇器來引用在Objective-C中方法(method)和屬性(property)的setter和getter方法。

> \#selector(方法名)
\#selector(getter: 屬性名)
\#selector(setter: 屬性名)

方法名和屬性名必須是存在於 Objective-C 運行時中的方法和屬性的引用。選擇器表達式的返回值是一個 Selector 類型的實例。例如：

```swift
class SomeClass: NSObject {
    let property: String
    @objc(doSomethingWithInt:)
    func doSomething(_ x: Int) { }
    
    init(property: String) {
        self.property = property
    }
}
let selectorForMethod = #selector(SomeClass.doSomething(_:))
let selectorForPropertyGetter = #selector(getter: SomeClass.property)
```
當為屬性的getter創建選擇器時,屬性名可以是變量屬性或者常量屬性的引用。但是當為屬性的setter創建選擇器時,屬性名只可以是對變量屬性的引用。

方法名稱可以包含圓括號來進行分組,並使用as 操作符來區分具有相同方法名但類型不同的方法, 例如:

```swift
extension SomeClass {
    @objc(doSomethingWithString:)
    func doSomething(_ x: String) { }
}
let anotherSelector = #selector(SomeClass.doSomething(_:) as (SomeClass) -> (String) -> Void)
```

由於選擇器是在編譯時創建的，因此編譯器可以檢查方法或者屬性是否存在，以及是否在運行時暴露給了 Objective-C 。

> 注意  
> 雖然方法名或者屬性名是個表達式，但是它不會被求值。

更多關於如何在 Swift 代碼中使用選擇器來與 Objective-C API 進行交互的信息，請參閱 [Using Swift with Cocoa and Objective-C (Swift 3)](https://developer.apple.com/library/prerelease/content/documentation/Swift/Conceptual/BuildingCocoaApps/index.html#//apple_ref/doc/uid/TP40014216) 中[Objective-C Selectors](https://developer.apple.com/library/prerelease/content/documentation/Swift/Conceptual/BuildingCocoaApps/InteractingWithObjective-CAPIs.html#//apple_ref/doc/uid/TP40014216-CH4-ID59)部分。

> 選擇器表達式語法  
<a name="selector-expression"></a>
> *選擇器表達式* → __#selector__ **(** [*表達式*](#expression) **)**  
> *選擇器表達式* → __#selector__ **(** [*getter:表達式*](#expression) **)**  
> *選擇器表達式* → __#selector__ **(** [*setter:表達式*](#expression) **)**


<a name="postfix_expressions"></a>
## 後綴表達式

後綴表達式就是在某個表達式的後面運用後綴運算符或其他後綴語法。從語法構成上來看，基本表達式也是後綴表達式。

關於這些運算符的更多信息，請參閱 [基本運算符](../chapter2/02_Basic_Operators.md) 和 [高級運算符](../chapter2/25_Advanced_Operators.md)。

關於 Swift 標准庫提供的運算符的更多信息，請參閱 [*Swift Standard Library Operators Reference*](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Reference/Swift_StandardLibrary_Operators/index.html#//apple_ref/doc/uid/TP40016054)。

> 後綴表達式語法  
<a name="postfix-expression"></a>
> *後綴表達式* → [*基本表達式*](#primary-expression)  
> *後綴表達式* → [*後綴表達式*](#postfix-expression) [*後綴運算符*](02_Lexical_Structure.md#postfix-operator)  
> *後綴表達式* → [*函數調用表達式*](#function-call-expression)  
> *後綴表達式* → [*構造器表達式*](#initializer-expression)  
> *後綴表達式* → [*顯式成員表達式*](#explicit-member-expression)  
> *後綴表達式* → [*後綴 self 表達式*](#postfix-self-expression)  
> *後綴表達式* → [*dynamicType 表達式*](#dynamic-type-expression)  
> *後綴表達式* → [*下標表達式*](#subscript-expression)  
> *後綴表達式* → [*強制取值表達式*](#forced-value-expression)  
> *後綴表達式* → [*可選鏈表達式*](#optional-chaining-expression)

<a name="function_call_expression"></a>
### 函數調用表達式

函數調用表達式由函數名和參數列表組成，形式如下：

> `函數名`(`參數 1`, `參數 2`)

函數名可以是值為函數類型的任意表達式。

如果函數聲明中指定了參數的名字，那麼在調用的時候也必須得寫出來。這種函數調用表達式具有以下形式：

> `函數名`(`參數名 1`: `參數 1`, `參數名 2`: `參數 2`)

如果函數的最後一個參數是函數類型，可以在函數調用表達式的尾部（右圓括號之後）加上一個閉包，該閉包會作為函數的最後一個參數。如下兩種寫法是等價的：

```swift
// someFunction 接受整數和閉包參數
someFunction(x, f: {$0 == 13})
someFunction(x) {$0 == 13}
```

如果閉包是該函數的唯一參數，那麼圓括號可以省略。

```swift
// someFunction 只接受一個閉包參數
myData.someMethod() {$0 == 13}
myData.someMethod {$0 == 13}
```

> 函數調用表達式語法  
<a name="function-call-expression"></a>
> *函數調用表達式* → [*後綴表達式*](#postfix-expression) [*圓括號表達式*](#parenthesized-expression)  
> *函數調用表達式* → [*後綴表達式*](#postfix-expression) [*圓括號表達式*](#parenthesized-expression)<sub>可選</sub> [*尾隨閉包*](#trailing-closure)  
<a name="trailing-closure"></a>
> *尾隨閉包* → [*閉包表達式*](#closure-expression)  

<a name="initializer_expression"></a>
### 構造器表達式

構造器表達式用於訪問某個類型的構造器，形式如下：

> `表達式`.init(`構造器參數`)

你可以在函數調用表達式中使用構造器表達式來初始化某個類型的新實例。也可以使用構造器表達式來代理給超類構造器。

```swift
class SomeSubClass: SomeSuperClass {
    override init() {
        // 此處為子類構造過程
        super.init()
    }
}
```

和函數類似，構造器表達式可以作為一個值。 例如：

```swift
// 類型注解是必須的，因為 String 類型有多種構造器
let initializer: Int -> String = String.init
let oneTwoThree = [1, 2, 3].map(initializer).reduce("", combine: +)
print(oneTwoThree)
// 打印 「123」
```

如果通過名字來指定某個類型，可以不用構造器表達式而直接使用類型的構造器。在其他情況下，你必須使用構造器表達式。

```swift
let s1 = SomeType.init(data: 3) // 有效
let s2 = SomeType(data: 1)      // 有效

let s4 = someValue.dynamicType(data: 5)      // 錯誤
let s3 = someValue.dynamicType.init(data: 7) // 有效
```

> 構造器表達式語法  
<a name="initializer-expression"></a>
> *構造器表達式* → [*後綴表達式*](#postfix-expression) **.** **init**  
> *構造器表達式* → [*後綴表達式*](#postfix-expression) **.** **init** **(** [*參數名稱*](#argument-names) **)**

<a name="explicit_member_expression"></a>
### 顯式成員表達式

顯式成員表達式允許我們訪問命名類型、元組或者模塊的成員，其形式如下：

> `表達式`.`成員名`

命名類型的某個成員在原始實現或者擴展中定義，例如：

```swift
class SomeClass {
    var someProperty = 42
}
let c = SomeClass()
let y = c.someProperty // 訪問成員
```

元組的成員會隱式地根據表示它們出現順序的整數來命名，以 0 開始，例如：

```swift
var t = (10, 20, 30)
t.0 = t.1
// 現在元組 t 為 (20, 20, 30)
```

對於模塊的成員來說，只能直接訪問頂級聲明中的成員。

為了區分只有參數名有所不同的方法或構造器，在圓括號中寫出參數名，參數名後緊跟一個冒號，對於沒有參數名的參數，使用下劃線代替參數名。而對於重載方法，則需使用類型標注進行區分。例如：

```swift
class SomeClass {
    func someMethod(x: Int, y: Int) {}
    func someMethod(x: Int, z: Int) {}
    func overloadedMethod(x: Int, y: Int) {}
    func overloadedMethod(x: Int, y: Bool) {}
}
let instance = SomeClass()

let a = instance.someMethod              // 有歧義
let b = instance.someMethod(_:y:)        // 無歧義

let d = instance.overloadedMethod        // 有歧義
let d = instance.overloadedMethod(_:y:)  // 有歧義
let d: (Int, Bool) -> Void  = instance.overloadedMethod(_:y:)  // 無歧義
```

如果點號（`.`）出現在行首，它會被視為顯式成員表達式的一部分，而不是隱式成員表達式的一部分。例如如下代碼所展示的被分為多行的鏈式方法調用：

```swift
let x = [10, 3, 20, 15, 4]
    .sort()
    .filter { $0 > 5 }
    .map { $0 * 100 }
```

> 顯式成員表達式語法  
<a name="explicit-member-expression"></a>
> *顯式成員表達式* → [*後綴表達式*](#postfix-expression) **.** [*十進制數字*](02_Lexical_Structure.md#decimal-digit)  
> *顯式成員表達式* → [*後綴表達式*](#postfix-expression) **.** [*標識符*](02_Lexical_Structure.md#identifier) [*泛型實參子句*](08_Generic_Parameters_and_Arguments.md#generic-argument-clause)<sub>可選</sub><br/> 
> *顯式成員表達式* → [*後綴表達式*](#postfix-expression) **.** [*標識符*](02_Lexical_Structure.md#identifier) **(** [*參數名稱*](#argument-names) **)**
> 
<a name="argument-names"></a>
> *參數名稱* → [*參數名*](#argument-name) [*參數名稱*](#argument-names)<sub>可選</sub><br/>
<a name="argument-name"></a>
> *參數名* → [*標識符*](02_Lexical_Structure.md#identifier) **:**

<a name="postfix_self_expression"></a>
### 後綴 self 表達式

後綴 `self` 表達式由某個表達式或類型名緊跟 `.self` 組成，其形式如下：

> `表達式`.self  
> `類型`.self  

第一種形式返回表達式的值。例如：`x.self` 返回 `x`。

第二種形式返回相應的類型。我們可以用它來獲取某個實例的類型作為一個值來使用。例如，`SomeClass.self` 會返回 `SomeClass` 類型本身，你可以將其傳遞給相應函數或者方法作為參數。

> 後綴 self 表達式語法  
<a name="postfix-self-expression"></a>
> *後綴 self 表達式* → [*後綴表達式*](#postfix-expression) **.** **self**  

<a name="dynamic_type_expression"></a>
### dynamicType 表達式

dynamicType 表達式由類似[函數調用表達式(Function Call Expression)](#function-call-expression)的特殊語法表達式組成,形式如下:

> type(of:`表達式`)

上述形式中的表達式不能是類型名。type(of:) 表達式會返回某個實例在運行時的類型，具體請看下面的例子：

```swift
class SomeBaseClass {
    class func printClassName() {
        print("SomeBaseClass")
    }
}
class SomeSubClass: SomeBaseClass {
    override class func printClassName() {
        print("SomeSubClass")
    }
}
let someInstance: SomeBaseClass = SomeSubClass()
// someInstance 在編譯時的靜態類型為 SomeBaseClass，
// 在運行時的動態類型為 SomeSubClass
type(of: someInstance).printClassName()
// 打印 「SomeSubClass」
```

> 動態類型表達式語法  
<a name="dynamic-type-expression"></a>
> *動態類型表達式* → type(of:表達式) **.** **dynamicType**  

<a name="subscript_expression"></a>
### 下標表達式

可通過下標表達式訪問相應的下標，形式如下：

> `表達式`[`索引表達式`]

要獲取下標表達式的值，可將索引表達式作為下標表達式的參數來調用下標 getter。下標 setter 的調用方式與之一樣。

關於下標的聲明，請參閱 [協議下標聲明](05_Declarations.md#protocol_subscript_declaration)。

> 下標表達式語法  
<a name="subscript-expression"></a>
> *下標表達式* → [*後綴表達式*](#postfix-expression) **[** [*表達式列表*](#expression-list) **]**  

<a name="forced-Value_expression"></a>
### 強制取值表達式

當你確定可選值不是 `nil` 時，可以使用強制取值表達式來強制解包，形式如下：

> `表達式`!

如果該表達式的值不是 `nil`，則返回解包後的值。否則，拋出運行時錯誤。

返回的值可以被修改，無論是修改值本身，還是修改值的成員。例如：

```swift
var x: Int? = 0
x!++
// x 現在是 1
 
var someDictionary = ["a": [1, 2, 3], "b": [10, 20]]
someDictionary["a"]![0] = 100
// someDictionary 現在是 [b: [10, 20], a: [100, 2, 3]] 
```

> 強制取值語法  
<a name="forced-value-expression"></a>
> *強制取值表達式* → [*後綴表達式*](#postfix-expression) **!**  

<a name="optional-chaining_expression"></a>
### 可選鏈表達式

可選鏈表達式提供了一種使用可選值的便捷方法，形式如下：

> `表達式`?

後綴 `?` 運算符會根據表達式生成可選鏈表達式而不會改變表達式的值。

如果某個後綴表達式包含可選鏈表達式，那麼它的執行過程會比較特殊。如果該可選鏈表達式的值是 `nil`，整個後綴表達式會直接返回 `nil`。如果該可選鏈表達式的值不是 `nil`，則返回可選鏈表達式解包後的值，並將該值用於後綴表達式中剩余的表達式。在這兩種情況下，整個後綴表達式的值都會是可選類型。

如果某個後綴表達式中包含了可選鏈表達式，那麼只有最外層的表達式會返回一個可選類型。例如，在下面的例子中，如果 `c` 不是 `nil`，那麼它的值會被解包，然後通過 `.property` 訪問它的屬性，接著進一步通過 `.performAction()` 調用相應方法。整個 `c?.property.performAction()` 表達式返回一個可選類型的值，而不是多重可選類型。

```swift
var c: SomeClass?
var result: Bool? = c?.property.performAction()
```

上面的例子跟下面的不使用可選鏈表達式的例子等價：

```swift
var result: Bool? = nil
if let unwrappedC = c {
    result = unwrappedC.property.performAction()
}
```

可選鏈表達式解包後的值可以被修改，無論是修改值本身，還是修改值的成員。如果可選鏈表達式的值為 `nil`，則表達式右側的賦值操作不會被執行。例如：

```swift
func someFunctionWithSideEffects() -> Int {
    // 譯者注：為了能看出此函數是否被執行，加上了一句打印
    print("someFunctionWithSideEffects") 
    return 42 
}
var someDictionary = ["a": [1, 2, 3], "b": [10, 20]]

someDictionary["not here"]?[0] = someFunctionWithSideEffects()
// someFunctionWithSideEffects 不會被執行
// someDictionary 依然是 ["b": [10, 20], "a": [1, 2, 3]]

someDictionary["a"]?[0] = someFunctionWithSideEffects()
// someFunctionWithSideEffects 被執行並返回 42
// someDictionary 現在是 ["b": [10, 20], "a": [42, 2, 3]]
```

> 可選鏈表達式語法  
<a name="optional-chaining-expression"></a>
> *可選鏈表達式* → [*後綴表達式*](#postfix-expression) **?**  
