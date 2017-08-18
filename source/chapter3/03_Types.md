# 類型（Types）
-----------------

> 1.0
> 翻譯：[lyuka](https://github.com/lyuka)
> 校對：[numbbbbb](https://github.com/numbbbbb), [stanzhai](https://github.com/stanzhai)

> 2.0
> 翻譯+校對：[EudeMorgen](https://github.com/EudeMorgen)

> 2.1
> 翻譯：[mmoaay](https://github.com/mmoaay)

本頁包含內容：

- [類型注解](#type_annotation)
- [類型標識符](#type_identifier)
- [元組類型](#tuple_type)
- [函數類型](#function_type)
- [數組類型](#array_type)
- [字典類型](#dictionary_type)
- [可選類型](#optional_type)
- [隱式解析可選類型](#implicitly_unwrapped_optional_type)
- [協議合成類型](#protocol_composition_type)
- [元類型](#metatype_type)
- [類型繼承子句](#type_inheritance_clause)
- [類型推斷](#type_inference)

Swift 語言存在兩種類型：命名型類型和復合型類型。命名型類型是指定義時可以給定名字的類型。命名型類型包括類、結構體、枚舉和協議。比如，一個用戶定義的類 `MyClass` 的實例擁有類型 `MyClass`。除了用戶定義的命名型類型，Swift 標准庫也定義了很多常用的命名型類型，包括那些表示數組、字典和可選值的類型。

那些通常被其它語言認為是基本或原始的數據型類型，比如表示數字、字符和字符串的類型，實際上就是命名型類型，這些類型在 Swift 標准庫中是使用結構體來定義和實現的。因為它們是命名型類型，因此你可以按照 [擴展](../chapter2/21_Extensions.html) 和 [擴展聲明](05_Declarations.html#extension_declaration) 中討論的那樣，聲明一個擴展來增加它們的行為以滿足你程序的需求。

復合型類型是沒有名字的類型，它由 Swift 本身定義。Swift 存在兩種復合型類型：函數類型和元組類型。一個復合型類型可以包含命名型類型和其它復合型類型。例如，元組類型 `(Int, (Int, Int))` 包含兩個元素：第一個是命名型類型 `Int`，第二個是另一個復合型類型 `(Int, Int)`。

本節討論 Swift 語言本身定義的類型，並描述 Swift 中的類型推斷行為。

> 類型語法  
<a name="type"></a>
> *類型* → [*數組類型*](#array-type) | [*字典類型*](#dictionary-type) | [*函數類型*](#function-type) | [*類型標識*](#type-identifier) | [*元組類型*](#tuple-type) | [*可選類型*](#optional-type) | [*隱式解析可選類型*](#implicitly-unwrapped-optional-type) | [*協議合成類型*](#protocol-composition-type) | [*元型類型*](#metatype-type) | **任意類型** | **自身類型**

<a name="type_annotation"></a>
## 類型注解

類型注解顯式地指定一個變量或表達式的值。類型注解始於冒號 `:` 終於類型，比如下面兩個例子：

```swift
let someTuple: (Double, Double) = (3.14159, 2.71828)
func someFunction(a: Int) { /* ... */ }
```
在第一個例子中，表達式 `someTuple` 的類型被指定為 `(Double, Double)`。在第二個例子中，函數 `someFunction` 的參數 `a` 的類型被指定為 `Int`。

類型注解可以在類型之前包含一個類型特性的可選列表。

> 類型注解語法  
<a name="type-annotation"></a>
> *類型注解* → **:** [*特性列表*](06_Attributes.html#attributes)<sub>可選</sub> **輸入輸出參數**<sub>可選</sub> [*類型*](#type)

<a name="type_identifier"></a>
## 類型標識符

類型標識符引用命名型類型，還可引用命名型或復合型類型的別名。

大多數情況下，類型標識符引用的是與之同名的命名型類型。例如類型標識符 `Int` 引用命名型類型 `Int`，同樣，類型標識符 `Dictionary<String, Int>` 引用命名型類型 `Dictionary<String, Int>`。

在兩種情況下類型標識符不引用同名的類型。情況一，類型標識符引用的是命名型或復合型類型的類型別名。比如，在下面的例子中，類型標識符使用 `Point` 來引用元組 `(Int, Int)`：

```swift
typealias Point = (Int, Int)
let origin: Point = (0, 0)
```

情況二，類型標識符使用點語法（`.`）來表示在其它模塊或其它類型嵌套內聲明的命名型類型。例如，下面例子中的類型標識符引用在 `ExampleModule` 模塊中聲明的命名型類型 `MyType`：

```swift
var someValue: ExampleModule.MyType
```

> 類型標識符語法  
<a name="type-identifier"></a>
> *類型標識符* → [*類型名稱*](#type-name) [*泛型參數子句*](08_Generic_Parameters_and_Arguments.html#generic_argument_clause)<sub>可選</sub> | [*類型名稱*](#type-name) [*泛型參數子句*](08_Generic_Parameters_and_Arguments.html#generic_argument_clause)<sub>可選</sub> **.** [*類型標識符*](#type-identifier)  
<a name="type-name"></a>
> *類型名稱* → [*標識符*](02_Lexical_Structure.html#identifier)  

<a name="tuple_type"></a>
## 元組類型

元組類型是使用括號括起來的零個或多個類型，類型間用逗號隔開。

你可以使用元組類型作為一個函數的返回類型，這樣就可以使函數返回多個值。你也可以命名元組類型中的元素，然後用這些名字來引用每個元素的值。元素的名字由一個標識符緊跟一個冒號 `(:)` 組成。[函數和多返回值](../chapter2/06_Functions.html#functions_with_multiple_return_values) 章節裡有一個展示上述特性的例子。

當一個元組類型的元素有名字的時候，這個名字就是類型的一部分。

```swift
var someTuple = (top: 10, bottom: 12)  // someTuple 的類型為 (top: Int, bottom: Int)
someTuple = (top: 4, bottom: 42) // 正確：命名類型匹配
someTuple = (9, 99)              // 正確：命名類型被自動推斷
someTuple = (left: 5, right: 5)  // 錯誤：命名類型不匹配
```

`Void` 是空元組類型 `()` 的別名。如果括號內只有一個元素，那麼該類型就是括號內元素的類型。比如，`(Int)` 的類型是 `Int` 而不是 `(Int)`。所以，只有當元組類型包含的元素個數在兩個及以上時才可以命名元組元素。

> 元組類型語法
<a name="tuple-type"></a>
> *元組類型* → **(** [*元組類型元素列表*](#tuple-type-element-list) <sub>可選</sub> **)**  
<a name="tuple-type-element-list"></a>
> *元組類型元素列表* → [*元組類型元素*](#tuple-type-element) | [*元組類型元素*](#tuple-type-element) **,** [*元組類型元素列表*](#tuple-type-element-list)  
<a name="tuple-type-element"></a>
> *元組類型元素* → [*元素名*](#element-name) [*類型注解*](#type-annotation) | [*類型*](#type)
<a name="element-name"></a>
> *元素名* → [*標識符*](02_Lexical_Structure.html#identifier)  

<a name="function_type"></a>
## 函數類型

函數類型表示一個函數、方法或閉包的類型，它由參數類型和返回值類型組成，中間用箭頭（`->`）隔開：

> `參數類型` -> `返回值類型`

參數類型是由逗號間隔的類型列表。由於參數類型和返回值類型可以是元組類型，所以函數類型支持多參數與多返回值的函數與方法。

你可以對函數參數使用 `autoclosure` 特性。這會自動將參數表達式轉化為閉包，表達式的結果即閉包返回值。這從語法結構上提供了一種便捷：延遲對表達式的求值，直到其值在函數體中被使用。以自動閉包做為參數的函數類型的例子詳見 [自動閉包](../chapter2/07_Closures.html#autoclosures) 。

函數類型可以擁有一個可變長參數作為參數類型中的最後一個參數。從語法角度上講，可變長參數由一個基礎類型名字緊隨三個點（`...`）組成，如 `Int...`。可變長參數被認為是一個包含了基礎類型元素的數組。即 `Int...` 就是 `[Int]`。關於使用可變長參數的例子，請參閱 [可變參數](../chapter2/06_Functions.html#variadic_parameters)。

為了指定一個 `in-out` 參數，可以在參數類型前加 `inout` 前綴。但是你不可以對可變長參數或返回值類型使用 `inout`。關於這種參數的詳細講解請參閱 [輸入輸出參數](../chapter2/06_Functions.html#in_out_parameters)。

函數和方法中的參數名並不是函數類型的一部分。例如：

```swift
func someFunction(left: Int, right: Int) {}
func anotherFunction(left: Int, right: Int) {}
func functionWithDifferentLabels(top: Int, bottom: Int) {}
 
var f = someFunction // 函數f的類型為 (Int, Int) -> Void, 而不是 (left: Int, right: Int) -> Void.
f = anotherFunction              // 正確
f = functionWithDifferentLabels  // 正確
 
func functionWithDifferentArgumentTypes(left: Int, right: String) {}
func functionWithDifferentNumberOfArguments(left: Int, right: Int, top: Int) {}
 
f = functionWithDifferentArgumentTypes     // 錯誤
f = functionWithDifferentNumberOfArguments // 錯誤
```

如果一個函數類型包涵多個箭頭(->)，那麼函數類型將從右向左進行組合。例如，函數類型 `Int -> Int -> Int` 可以理解為 `Int -> (Int -> Int)`，也就是說，該函數類型的參數為 `Int` 類型，其返回類型是一個參數類型為 `Int`，返回類型為 `Int` 的函數類型。

函數類型若要拋出錯誤就必須使用 `throws` 關鍵字來標記，若要重拋錯誤則必須使用 `rethrows` 關鍵字來標記。`throws` 關鍵字是函數類型的一部分，非拋出函數是拋出函數函數的一個子類型。因此，在使用拋出函數的地方也可以使用不拋出函數。拋出和重拋函數的相關描述見章節 [拋出函數與方法](05_Declarations.html#throwing_functions_and_methods) 和 [重拋函數與方法](05_Declarations.html#rethrowing_functions_and_methods)。

> 函數類型語法  
<a name="function-type"></a>
> *函數類型* → [*特性列表*](06_Attributes.html#attributes)<sub>可選</sub> [*函數類型子句*](#function-type-argument-clause) **throws**<sub>可選</sub> **->** [*類型*](#type)
> *函數類型* → [*特性列表*](06_Attributes.html#attributes)<sub>可選</sub> [*函數類型子句*](#function-type-argument-clause) **rethrows­** **->** [*類型*](#type) 
<a name="function-type-argument-clause"></a>
> *函數類型子句* → (­)­
> *函數類型子句* → ([*函數類型參數列表*](#function-type-argument-list)*...*­<sub>可選</sub>)­
<a name="function-type-argument-list"></a>
> *函數類型參數列表* → [*函數類型參數*](function-type-argument) | [*函數類型參數*](function-type-argument)， [*函數類型參數列表*](#function-type-argument-list)
<a name="function-type-argument"></a>
> *函數類型參數* → [*特性列表*](06_Attributes.html#attributes)<sub>可選</sub> **輸入輸出參數**<sub>可選</sub> [*類型*](#type) | [*參數標簽*](#argument-label) [*類型注解*](#type-annotation)
<a name="argument-label"></a>
> *參數標簽* → [*標識符*](02_Lexical_Structure.html#identifier) 

<a name="array_type"></a>
## 數組類型

Swift 語言為標准庫中定義的 `Array<Element>` 類型提供了如下語法糖：

> [`類型`]

換句話說，下面兩個聲明是等價的：

```swift
let someArray: Array<String> = ["Alex", "Brian", "Dave"]
let someArray: [String] = ["Alex", "Brian", "Dave"]
```

上面兩種情況下，常量 `someArray` 都被聲明為字符串數組。數組的元素也可以通過下標訪問：`someArray[0]` 是指第 0 個元素 `"Alex"`。

你也可以嵌套多對方括號來創建多維數組，最裡面的方括號中指明數組元素的基本類型。比如，下面例子中使用三對方括號創建三維整數數組：

```swift
var array3D: [[[Int]]] = [[[1, 2], [3, 4]], [[5, 6], [7, 8]]]
```

訪問一個多維數組的元素時，最左邊的下標指向最外層數組的相應位置元素。接下來往右的下標指向第一層嵌入的相應位置元素，依次類推。這就意味著，在上面的例子中，`array3D[0]` 是 `[[1, 2], [3, 4]]`，`array3D[0][1]` 是 `[3, 4]`，`array3D[0][1][1]` 則是 `4`。

關於 Swift 標准庫中 `Array` 類型的詳細討論，請參閱 [數組](../chapter2/04_Collection_Types.html#arrays)。

> 數組類型語法  
<a name="array-type"></a>
> *數組類型* → **[** [*類型*](#type) **]**

<a name="dictionary_type"></a>
## 字典類型

Swift 語言為標准庫中定義的 `Dictionary<Key, Value>` 類型提供了如下語法糖：

> [`鍵類型` : `值類型`]

換句話說，下面兩個聲明是等價的：

```swift
let someDictionary: [String: Int] = ["Alex": 31, "Paul": 39]
let someDictionary: Dictionary<String, Int> = ["Alex": 31, "Paul": 39]
```

上面兩種情況，常量 `someDictionary` 被聲明為一個字典，其中鍵為 `String` 類型，值為 `Int` 類型。

字典中的值可以通過下標來訪問，這個下標在方括號中指明了具體的鍵：`someDictionary["Alex"]` 返回鍵 `Alex` 對應的值。如果鍵在字典中不存在的話，則這個下標返回 `nil`。

字典中鍵的類型必須符合 Swift 標准庫中的 `Hashable` 協議。

關於 Swift 標准庫中 `Dictionary` 類型的詳細討論，請參閱 [字典](../chapter2/04_Collection_Types.html#dictionaries)。

> 字典類型語法  
<a name="dictionary-type"></a>
> *字典類型* → **[** [*類型*](#type) **:** [*類型*](#type) **]** 

<a name="optional_type"></a>
## 可選類型

Swift 定義後綴 `?` 來作為標准庫中的定義的命名型類型 `Optional<Wrapped>` 的語法糖。換句話說，下面兩個聲明是等價的：

```swift
var optionalInteger: Int?
var optionalInteger: Optional<Int>
```

在上述兩種情況下，變量 `optionalInteger` 都被聲明為可選整型類型。注意在類型和 `?` 之間沒有空格。

類型 `Optional<Wrapped>` 是一個枚舉，有兩個成員，`none` 和 `some(Wrapped)`，用來表示可能有也可能沒有的值。任意類型都可以被顯式地聲明（或隱式地轉換）為可選類型。如果你在聲明或定義可選變量或屬性的時候沒有提供初始值，它的值則會自動賦為默認值 `nil`。

如果一個可選類型的實例包含一個值，那麼你就可以使用後綴運算符 `!` 來獲取該值，正如下面描述的：

```swift
optionalInteger = 42
optionalInteger! // 42
```

使用 `!` 運算符解包值為 `nil` 的可選值會導致運行錯誤。

你也可以使用可選鏈式調用和可選綁定來選擇性地在可選表達式上執行操作。如果值為 `nil`，不會執行任何操作，因此也就沒有運行錯誤產生。

更多細節以及更多如何使用可選類型的例子，請參閱 [可選類型](../chapter2/01_The_Basics.html#optionals)。

> 可選類型語法  
<a name="optional-type"></a>
> *可選類型* → [*類型*](#type) **?**  

<a name="implicitly_unwrapped_optional_type"></a>
## 隱式解析可選類型

當可以被訪問時，Swift 語言定義後綴 `!` 作為標准庫中命名類型 `Optional<Wrapped>` 的語法糖，來實現自動解包的功能。換句話說，下面兩個聲明等價：

```swift
var implicitlyUnwrappedString: String!
var explicitlyUnwrappedString: Optional<String>
```

注意類型與 `!` 之間沒有空格。

由於隱式解包修改了包涵其類型的聲明語義，嵌套在元組類型或泛型的可選類型（比如字典元素類型或數組元素類型），不能被標記為隱式解包。例如：

```swift
let tupleOfImplicitlyUnwrappedElements: (Int!, Int!)  // 錯誤
let implicitlyUnwrappedTuple: (Int, Int)!             // 正確
 
let arrayOfImplicitlyUnwrappedElements: [Int!]        // 錯誤
let implicitlyUnwrappedArray: [Int]!                  // 正確
```

由於隱式解析可選類型和可選類型有同樣的表達式`Optional<Wrapped>`，你可以在使用可選類型的地方使用隱式解析可選類型。比如，你可以將隱式解析可選類型的值賦給變量、常量和可選屬性，反之亦然。

正如可選類型一樣，你在聲明隱式解析可選類型的變量或屬性的時候也不用指定初始值，因為它有默認值 `nil`。

可以使用可選鏈式調用來在隱式解析可選表達式上選擇性地執行操作。如果值為 `nil`，就不會執行任何操作，因此也不會產生運行錯誤。

關於隱式解析可選類型的更多細節，請參閱 [隱式解析可選類型](../chapter2/01_The_Basics.html#implicityly_unwrapped_optionals)。

> 隱式解析可選類型語法  
<a name="implicitly-unwrapped-optional-type"></a>
> *隱式解析可選類型* → [*類型*](#type) **!**  

<a name="protocol_composition_type"></a>
## 協議合成類型

協議合成類型是一種符合協議列表中每個指定協議的類型。協議合成類型可能會用在類型注解和泛型參數中。

協議合成類型的形式如下：

> `Protocol 1` & `Procotol 2`

協議合成類型允許你指定一個值，其類型符合多個協議的要求且不需要定義一個新的命名型協議來繼承它想要符合的各個協議。比如，協議合成類型 `Protocol A & Protocol B & Protocol C` 等效於一個從 `Protocol A`，`Protocol B`， `Protocol C` 繼承而來的新協議 `Protocol D`，很顯然這樣做有效率的多，甚至不需引入一個新名字。

協議合成列表中的每項必須是協議名或協議合成類型的類型別名。

> 協議合成類型語法  
<a name="protocol-composition-type"></a>
> *協議合成類型* → [*協議標識符*](#protocol-identifier) & [*協議合成延續*](#protocol-composition-continuation)
<a name="protocol-composition-continuation"></a>
> *協議合成延續* → [*協議標識符*](#protocol-identifier) | [*協議合成類型*](#protocol-composition-type)
<a name="protocol-identifier"></a>
> *協議標識符* → [*類型標識符*](#type-identifier) 

<a name="metatype_type"></a>
## 元類型

元類型是指類型的類型，包括類類型、結構體類型、枚舉類型和協議類型。

類、結構體或枚舉類型的元類型是相應的類型名緊跟 `.Type`。協議類型的元類型——並不是運行時符合該協議的具體類型——而是該協議名字緊跟 `.Protocol`。比如，類 `SomeClass` 的元類型就是 `SomeClass.Type`，協議 `SomeProtocol` 的元類型就是 `SomeProtocal.Protocol`。

你可以使用後綴 `self` 表達式來獲取類型。比如，`SomeClass.self` 返回 `SomeClass` 本身，而不是 `SomeClass` 的一個實例。同樣，`SomeProtocol.self` 返回 `SomeProtocol` 本身，而不是運行時符合 `SomeProtocol` 的某個類型的實例。還可以對類型的實例使用 `type(of:)` 表達式來獲取該實例在運行階段的類型，如下所示：

```swift
class SomeBaseClass {
    class func printClassName() {
        println("SomeBaseClass")
    }
}
class SomeSubClass: SomeBaseClass {
    override class func printClassName() {
        println("SomeSubClass")
    }
}
let someInstance: SomeBaseClass = SomeSubClass()
// someInstance 在編譯期是 SomeBaseClass 類型，
// 但是在運行期則是 SomeSubClass 類型
type(of: someInstance).printClassName()
// 打印 「SomeSubClass」
```

可以使用恆等運算符（`===` 和 `!==`）來測試一個實例的運行時類型和它的編譯時類型是否一致。

```swift
if type(of: someInstance) === someInstance.self {
    print("The dynamic and static type of someInstance are the same")
} else {
    print("The dynamic and static type of someInstance are different")
}
// 打印 "The dynamic and static type of someInstance are different"
```

可以使用初始化表達式從某個類型的元類型構造出一個該類型的實例。對於類實例，被調用的構造器必須使用 `required` 關鍵字標記，或者整個類使用 `final` 關鍵字標記。

```swift
class AnotherSubClass: SomeBaseClass {
    let string: String
    required init(string: String) {
        self.string = string
    }
    override class func printClassName() {
        print("AnotherSubClass")
    }
}
let metatype: AnotherSubClass.Type = AnotherSubClass.self
let anotherInstance = metatype.init(string: "some string")
```

> 元類型語法  
<a name="metatype-type"></a>
> *元類型* → [*類型*](#type) **.** **Type** | [*類型*](#type) **.** **Protocol** 

<a name="type_inheritance_clause"></a>
## 類型繼承子句

類型繼承子句被用來指定一個命名型類型繼承自哪個類、采納哪些協議。類型繼承子句也用來指定一個類類型專屬協議。類型繼承子句開始於冒號 `:`，其後是所需要的類、類型標識符列表或兩者都有。

類可以繼承單個超類，采納任意數量的協議。當定義一個類時，超類的名字必須出現在類型標識符列表首位，然後跟上該類需要采納的任意數量的協議。如果一個類不是從其它類繼承而來，那麼列表可以以協議開頭。關於類繼承更多的討論和例子，請參閱 [繼承](../chapter2/13_Inheritance.html)。

其它命名型類型可能只繼承或采納一系列協議。協議類型可以繼承自任意數量的其他協議。當一個協議類型繼承自其它協議時，其它協議中定義的要求會被整合在一起，然後從當前協議繼承的任意類型必須符合所有這些條件。正如在 [協議聲明](05_Declarations.html#protocol_declaration) 中所討論的那樣，可以把 `class` 關鍵字放到協議類型的類型繼承子句的首位，這樣就可以聲明一個類類型專屬協議。

枚舉定義中的類型繼承子句可以是一系列協議，或是枚舉的原始值類型的命名型類型。在枚舉定義中使用類型繼承子句來指定原始值類型的例子，請參閱 [原始值](../chapter2/08_Enumerations.html#raw_values)。

> 類型繼承子句語法  
<a name="type_inheritance_clause"></a>
> *類型繼承子句* → **:** [*類要求*](#class-requirement) **,** [*類型繼承列表*](#type-inheritance-list)  
> *類型繼承子句* → **:** [*類要求*](#class-requirement)  
> *類型繼承子句* → **:** [*類型繼承列表*](#type-inheritance-list)  
<a name="type-inheritance-list"></a>
> *類型繼承列表* → [*類型標識符*](#type-identifier) | [*類型標識符*](#type-identifier) **,** [*類型繼承列表*](#type-inheritance-list)  
<a name="class-requirement"></a>
> *類要求* → **class**

<a name="type_inference"></a>
## 類型推斷

Swift 廣泛使用類型推斷，從而允許你省略代碼中很多變量和表達式的類型或部分類型。比如，對於 `var x: Int = 0`，你可以完全省略類型而簡寫成 `var x = 0`，編譯器會正確推斷出 `x` 的類型 `Int`。類似的，當完整的類型可以從上下文推斷出來時，你也可以省略類型的一部分。比如，如果你寫了 `let dict: Dictionary = ["A" : 1]`，編譯器能推斷出 `dict` 的類型是 `Dictionary<String, Int>`。

在上面的兩個例子中，類型信息從表達式樹的葉子節點傳向根節點。也就是說，`var x: Int = 0` 中 `x` 的類型首先根據 `0` 的類型進行推斷，然後將該類型信息傳遞到根節點（變量 `x`）。

在 Swift 中，類型信息也可以反方向流動——從根節點傳向葉子節點。在下面的例子中，常量 `eFloat` 上的顯式類型注解（`: Float`）將導致數字字面量 `2.71828` 的類型是 `Float` 而非 `Double`。

```swift
let e = 2.71828 // e 的類型會被推斷為 Double
let eFloat: Float = 2.71828 // eFloat 的類型為 Float
```

Swift 中的類型推斷在單獨的表達式或語句上進行。這意味著所有用於類型推斷的信息必須可以從表達式或其某個子表達式的類型檢查中獲取到。


