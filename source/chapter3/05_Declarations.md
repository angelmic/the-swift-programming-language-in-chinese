<a name="declarations"></a>
# 聲明（Declarations）
-----------------

> 1.0
> 翻譯：[marsprince](https://github.com/marsprince) [Lenhoon](https://github.com/marsprince)[(微博)](http://www.weibo.com/lenhoon)
> 校對：[numbbbbb](https://github.com/numbbbbb), [stanzhai](https://github.com/stanzhai)

> 2.0
> 翻譯+校對：[Lenhoon](https://github.com/Lenhoon),
> [BridgeQ](https://github.com/WXGBridgeQ)

> 2.1 
> 翻譯：[mmoaay](https://github.com/mmoaay), [shanks](http://codebuild.me)
> 校對：[shanks](http://codebuild.me)

> 2.2
> 翻譯：[星夜暮晨](https://github.com/SemperIdem)

> 3.0
> 翻譯：[chenmingjia](https://github.com/chenmingjia)

本頁包含內容：

- [頂級代碼](#top-level_code)
- [代碼塊](#code_blocks)
- [導入聲明](#import_declaration)
- [常量聲明](#constant_declaration)
- [變量聲明](#variable_declaration)
  - [存儲型變量和存儲型變量屬性](#stored_variables_and_stored_variable_properties)
  - [計算型變量和計算型屬性](#computed_variables_and_computed_properties)
  - [存儲型變量和屬性的觀察器](#stored_variable_observers_and_property_observers)
  - [類型變量屬性](#type_variable_properties)
- [類型別名聲明](#type_alias_declaration)
- [函數聲明](#function_declaration)
  - [參數名](#parameter_names)
  - [輸入輸出參數](#in-out_parameters)
  - [特殊參數](#special_kinds_of_parameters)
  - [特殊方法](#special_kinds_of_methods)
  - [拋出錯誤的函數和方法](#throwing_functions_and_methods)
  - [重拋錯誤的函數和方法](#rethrowing_functions_and_methods)
- [枚舉聲明](#enumeration_declaration)
  - [任意類型的枚舉用例](#enumerations_with_cases_of_any_type)
  - [遞歸枚舉](#enumerations_with_indirection)
  - [擁有原始值的枚舉用例](#enumerations_with_cases_of_a_raw-value_type)
  - [訪問枚舉用例](#accessing_enumeration_cases)
- [結構體聲明](#structure_declaration)
- [類聲明](#class_declaration)
- [協議聲明](#protocol_declaration)
  - [協議屬性聲明](#protocol_property_declaration)
  - [協議方法聲明](#protocol_method_declaration)
  - [協議構造器聲明](#protocol_initializer_declaration)
  - [協議下標聲明](#protocol_subscript_declaration)
  - [協議關聯類型聲明](#protocol_associated_type_declaration)
- [構造器聲明](#initializer_declaration)
  - [可失敗構造器](#failable_initializers)
- [析構器聲明](#deinitializer_declaration)
- [擴展聲明](#extension_declaration)
- [下標聲明](#subscript_declaration)
- [運算符聲明](#operator_declaration)
- [聲明修飾符](#declaration_modifiers)
  - [訪問控制級別](#access_control_levels)

*聲明 (declaration)* 用以向程序裡引入新的名字或者結構。舉例來說，可以使用聲明來引入函數和方法，變量和常量，或者定義新的具有命名的枚舉、結構、類和協議類型。還可以使用聲明來擴展一個既有的具有命名的類型的行為，或者在程序裡引入在其它地方聲明的符號。

在 Swift 中，大多數聲明在某種意義上講也是定義，因為聲明往往伴隨著實現或初始化。由於協議並不提供實現，大多數協議成員僅僅只是聲明而已。為了方便起見，也是因為這些區別在 Swift 中並不是很重要，「聲明」這個術語同時包含了聲明和定義兩種含義。

> 聲明語法  
> <a name="declaration"></a>
> *聲明* → [*導入聲明*](#import-declaration)  
> *聲明* → [*常量聲明*](#constant-declaration)  
> *聲明* → [*變量聲明*](#variable-declaration)  
> *聲明* → [*類型別名聲明*](#typealias-declaration)  
> *聲明* → [*函數聲明*](#function-declaration)  
> *聲明* → [*枚舉聲明*](#enum-declaration)  
> *聲明* → [*結構體聲明*](#struct-declaration)  
> *聲明* → [*類聲明*](#class-declaration)  
> *聲明* → [*協議聲明*](#protocol-declaration)  
> *聲明* → [*構造器聲明*](#initializer-declaration)  
> *聲明* → [*析構器聲明*](#deinitializer-declaration)  
> *聲明* → [*擴展聲明*](#extension-declaration)  
> *聲明* → [*下標聲明*](#subscript-declaration)  
> *聲明* → [*運算符聲明*](#operator-declaration)  
> <a name="declarations"></a>
> *多條聲明* → [*聲明*](#declaration) [*多條聲明*](#declarations)<sub>可選</sub>

<a name="top-level_code"></a>
## 頂級代碼

Swift 的源文件中的頂級代碼 (top-level code) 由零個或多個語句、聲明和表達式組成。默認情況下，在一個源文件的頂層聲明的變量，常量和其他具有命名的聲明可以被同模塊中的每一個源文件中的代碼訪問。可以使用一個訪問級別修飾符來標記聲明來覆蓋這種默認行為，請參閱 [訪問控制級別](#access_control_levels)。

> 頂級聲明語法  
> *頂級聲明* → [*多條語句*](10_Statements.md#statements)<sub>可選</sub>

<a name="code_blocks"></a>
## 代碼塊

*代碼塊 (code block)* 可以將一些聲明和控制結構組織在一起。它有如下的形式：

```swift
{
	語句
}
```
代碼塊中的「語句」包括聲明、表達式和各種其他類型的語句，它們按照在源碼中的出現順序被依次執行。

> 代碼塊語法  
> <a name="code-block"></a>
> *代碼塊* → **{** [*多條語句*](10_Statements.md#statements)<sub>可選</sub> **}**  

<a name="import_declaration"></a>
## 導入聲明

*導入聲明 (import declaration)* 讓你可以使用在其他文件中聲明的內容。導入語句的基本形式是導入整個模塊，它由 `import` 關鍵字和緊隨其後的模塊名組成：

```swift
import 模塊
```

可以對導入操作提供更細致的控制，如指定一個特殊的子模塊或者指定一個模塊或子模塊中的某個聲明。提供了這些限制後，在當前作用域中，只有被導入的符號是可用的，而不是整個模塊中的所有聲明。

```swift
import 導入類型 模塊.符號名
import 模塊.子模塊
```

<a name="grammer_of_an_import_declaration"></a>
> 導入聲明語法  
> <a name="import-declaration"></a>
> *導入聲明* → [*特性列表*](06_Attributes.md#attributes)<sub>可選</sub> **import** [*導入類型*](#import-kind)<sub>可選</sub> [*導入路徑*](#import-path)  
> <a name="import-kind"></a>
> *導入類型* → **typealias** | **struct** | **class** | **enum** | **protocol** | **var** | **func**  
> <a name="import-path"></a>
> *導入路徑* → [*導入路徑標識符*](#import-path-identifier) | [*導入路徑標識符*](#import-path-identifier) **.** [*導入路徑*](#import-path)  
> <a name="import-path-identifier"></a>
> *導入路徑標識符* → [*標識符*](02_Lexical_Structure.md#identifier) | [*運算符*](02_Lexical_Structure.md#operator)  

<a name="constant_declaration"></a>
## 常量聲明

*常量聲明 (constant declaration)* 可以在程序中引入一個具有命名的常量。常量以關鍵字 `let` 來聲明，遵循如下格式：

```swift
let 常量名稱: 類型 = 表達式
```

常量聲明在「常量名稱」和用於初始化的「表達式」的值之間定義了一種不可變的綁定關系；當常量的值被設定之後，它就無法被更改。這意味著，如果常量以類對象來初始化，對象本身的內容是可以改變的，但是常量和該對象之間的綁定關系是不能改變的。

當一個常量被聲明為全局常量時，它必須擁有一個初始值。在類或者結構中聲明一個常量時，它將作為*常量屬性 (constant property)*。常量聲明不能是計算型屬性，因此也沒有存取方法。

如果常量名稱是元組形式，元組中每一項的名稱都會和初始化表達式中對應的值進行綁定。

```swift
let (firstNumber, secondNumber) = (10, 42)
```

在上例中，`firstNumber` 是一個值為 `10` 的常量，`secnodeName` 是一個值為 `42` 的常量。所有常量都可以獨立地使用：

```swift
print("The first number is \(firstNumber).")
// 打印 「The first number is 10.」
print("The second number is \(secondNumber).")
// 打印 「The second number is 42.」
```

當常量名稱的類型 (`:` 類型) 可以被推斷出時，類型標注在常量聲明中是可選的，正如 [類型推斷](03_Types.md#type_inference) 中所描述的。

聲明一個常量類型屬性要使用 `static` 聲明修飾符。類型屬性在 [類型屬性](../chapter2/10_Properties.md#type_properties)中有介紹。

如果還想獲得更多關於常量的信息或者想在使用中獲得幫助，請參閱 [常量和變量](../chapter2/01_The_Basics.md#constants_and_variables) 和 [存儲屬性](../chapter2/10_Properties.md#stored_properties)。

<a name="grammer_of_a_constant_declaration"></a>
> 常量聲明語法  
> <a name="constant-declaration"></a>
> *常量聲明* → [*特性列表*](06_Attributes.md#attributes)<sub>可選</sub> [*聲明修飾符列表*](#declaration-modifiers)<sub>可選</sub> **let** [*模式構造器列表*](pattern-initializer-list)  
> <a name="pattern-initializer-list"></a>
> *模式構造器列表* → [*模式構造器*](#pattern-initializer) | [*模式構造器*](#pattern-initializer) **,** [*模式構造器列表*](#pattern-initializer-list)  
> <a name="pattern-initializer"></a>
> *模式構造器* → [*模式*](07_Patterns.md#pattern) [*構造器*](#initializer)<sub>可選</sub>  
> <a name="initializer"></a>
> *構造器* → **=** [*表達式*](04_Expressions.md#expression)  

<a name="variable_declaration"></a>
## 變量聲明

*變量聲明 (variable declaration)* 可以在程序中引入一個具有命名的變量，它以關鍵字 `var` 來聲明。

變量聲明有幾種不同的形式，可以聲明不同種類的命名值和可變值，如存儲型和計算型變量和屬性，屬性觀察器，以及靜態變量屬性。所使用的聲明形式取決於變量聲明的適用范圍和打算聲明的變量類型。

> 注意  
> 也可以在協議聲明中聲明屬性，詳情請參閱 [協議屬性聲明](#protocol_property_declaration)。

可以在子類中重寫繼承來的變量屬性，使用 `override` 聲明修飾符標記屬性的聲明即可，詳情請參閱 [重寫](../chapter2/13_Inheritance.md#overriding)。

<a name="stored_variables_and_stored_variable_properties"></a>
### 存儲型變量和存儲型變量屬性

使用如下形式聲明一個存儲型變量或存儲型變量屬性：

```swift
var 變量名稱: 類型 = 表達式
```

可以在全局范圍，函數內部，或者在類和結構的聲明中使用這種形式來聲明一個變量。當變量以這種形式在全局范圍或者函數內部被聲明時，它代表一個存儲型變量。當它在類或者結構中被聲明時，它代表一個*存儲型變量屬性 (stored variable property)*。

用於初始化的表達式不可以在協議的聲明中出現，在其他情況下，該表達式是可選的。如果沒有初始化表達式，那麼變量聲明必須包含類型標注 (`:` *type*)。

如同常量聲明，如果變量名稱是元組形式，元組中每一項的名稱都會和初始化表達式中對應的值進行綁定。

正如名字所示，存儲型變量和存儲型變量屬性的值會存儲在內存中。

<a name="computed_variables_and_computed_properties"></a>
### 計算型變量和計算型屬性

使用如下形式聲明一個計算型變量或計算型屬性：

```swift
var 變量名稱: 類型 {  
	get {  
		語句
	}  
	set(setter 名稱) {  
		語句
	}  
}  
```

可以在全局范圍、函數內部，以及類、結構、枚舉、擴展的聲明中使用這種形式的聲明。當變量以這種形式在全局范圍或者函數內部被聲明時，它表示一個計算型變量。當它在類、結構、枚舉、擴展聲明的上下文中被聲明時，它表示一個*計算型屬性 (computed property)*。

getter 用來讀取變量值，setter 用來寫入變量值。setter 子句是可選的，getter 子句是必須的。不過也可以將這些子句都省略，直接返回請求的值，正如在 [只讀計算型屬性](../chapter2/10_Properties.md#computed_properties) 中描述的那樣。但是如果提供了一個 setter 子句，就必須也提供一個 getter 子句。

setter 的圓括號以及 setter 名稱是可選的。如果提供了 setter 名稱，它就會作為 setter 的參數名稱使用。如果不提供 setter 名稱，setter 的參數的默認名稱為 `newValue`，正如在 [便捷 setter 聲明](../chapter2/10_Properties.md#shorthand_setter_declaration) 中描述的那樣。

與存儲型變量和存儲型屬性不同，計算型變量和計算型屬性的值不存儲在內存中。

要獲得更多關於計算型屬性的信息和例子，請參閱 [計算型屬性](../chapter2/10_Properties.md#computed_properties)。

<a name="stored_variable_observers_and_property_observers"></a>
### 存儲型變量和屬性的觀察器

可以在聲明存儲型變量或屬性時提供 `willSet` 和 `didSet` 觀察器。一個包含觀察器的存儲型變量或屬性以如下形式聲明：

```swift
var 變量名稱: 類型 = 表達式 {  
	willSet(setter 名稱) {  
		語句
	}  
	didSet(setter 名稱) {  
		語句
	}  
}  
```

可以在全局范圍、函數內部，或者類、結構的聲明中使用這種形式的聲明。當變量以這種形式在全局范圍或者函數內部被聲明時，觀察器表示一個存儲型變量觀察器。當它在類和結構的聲明中被聲明時，觀察器表示一個屬性觀察器。

可以為任何存儲型屬性添加觀察器。也可以通過重寫父類屬性的方式為任何繼承的屬性（無論是存儲型還是計算型的）添加觀察器
，正如 [重寫屬性觀察器](../chapter2/13_Inheritance.md#overriding_property_observers) 中所描述的。

用於初始化的表達式在類或者結構的聲明中是可選的，但是在其他聲明中則是必須的。如果可以從初始化表達式中推斷出類型信息，那麼可以不提供類型標注。

當變量或屬性的值被改變時，`willSet` 和 `didSet` 觀察器提供了一種觀察方法。觀察器會在變量的值被改變時調用，但不會在初始化時被調用。

`willSet` 觀察器只在變量或屬性的值被改變之前調用。新的值作為一個常量傳入 `willSet` 觀察器，因此不可以在 `willSet` 中改變它。`didSet` 觀察器在變量或屬性的值被改變後立即調用。和 `willSet` 觀察器相反，為了方便獲取舊值，舊值會傳入 `didSet` 觀察器。這意味著，如果在變量或屬性的 `didiset` 觀察器中設置值，設置的新值會取代剛剛在 `willSet` 觀察器中傳入的那個值。

在 `willSet` 和 `didSet` 中，圓括號以及其中的 setter 名稱是可選的。如果提供了一個 setter 名稱，它就會作為 `willSet` 和 `didSet` 的參數被使用。如果不提供 setter 名稱，`willSet` 觀察器的默認參數名為 `newValue`，`didSet` 觀察器的默認參數名為 `oldValue`。

提供了 `willSet` 時，`didSet` 是可選的。同樣的，提供了 `didSet` 時，`willSet` 則是可選的。

要獲得更多信息以及查看如何使用屬性觀察器的例子，請參閱 [屬性觀察器](../chapter2/10_Properties.md#property_observers)。

<a name="type_variable_properties"></a>
### 類型變量屬性

要聲明一個類型變量屬性，用 `static` 聲明修飾符標記該聲明。類可以改用 `class` 聲明修飾符標記類的類型計算型屬性從而允許子類重寫超類的實現。類型屬性在 [類型屬性](../chapter2/10_Properties.md#type_properties) 章節有詳細討論。

> 注意  
> 在一個類聲明中，使用關鍵字 `static` 與同時使用 `class` 和 `final` 去標記一個聲明的效果相同。

<a name="grammer_of_a_variable_declaration"></a>
> 變量聲明語法  

<a name="variable-declaration"></a>
> *變量聲明* → [*變量聲明頭*](#variable-declaration-head) [*模式構造器列表*](#pattern-initializer-list)  
> *變量聲明* → [*變量聲明頭*](#variable-declaration-head) [*變量名稱*](#variable-name) [*類型標注*](03_Types.md#type-annotation) [*代碼塊*](#code-block)  
> *變量聲明* → [*變量聲明頭*](#variable-declaration-head) [*變量名稱*](#variable-name) [*類型標注*](03_Types.md#type-annotation) [*getter-setter代碼塊*](#getter-setter-block)  
> *變量聲明* → [*變量聲明頭*](#variable-declaration-head) [*變量名稱*](#variable-name) [*類型標注*](03_Types.md#type-annotation) [*getter-setter關鍵字代碼塊*](#getter-setter-keyword-block)   
> *變量聲明* → [*變量聲明頭*](#variable-declaration-head) [*變量名稱*](#variable-name) [*構造器*](#initializer) [*willSet-didSet代碼塊*](#willSet-didSet-block)   
> *變量聲明* → [*變量聲明頭*](#variable-declaration-head) [*變量名稱*](#variable-name) [*類型標注*](03_Types.md#type-annotation) [*構造器*](#initializer)<sub>可選</sub> [*willSet-didSet代碼塊*](#willSet-didSet-block)  

<a name="variable-declaration-head"></a>
> *變量聲明頭* → [*特性列表*](06_Attributes.md#attributes)<sub>可選</sub> [*聲明修飾符列表*](#declaration-modifiers)<sub>可選</sub> **var**  
> <a name="variable-name"></a>
> *變量名稱* → [*標識符*](02_Lexical_Structure.md#identifier)  

<a name="getter-setter-block"></a>
> *getter-setter 代碼塊* → [*代碼塊*](#code-block)  
> *getter-setter 代碼塊* → **{** [*getter子句*](#getter-clause) [*setter子句*](#setter-clause)<sub>可選</sub> **}**  
> *getter-setter 代碼塊* → **{** [*setter子句*](#setter-clause) [*getter子句*](#getter-clause) **}**  
> <a name="getter-clause"></a>
> *getter 子句* → [*特性列表*](06_Attributes.md#attributes)<sub>可選</sub> **get** [*代碼塊*](#code-block)  
> <a name="setter-clause"></a>
> *setter 子句* → [*特性列表*](06_Attributes.md#attributes)<sub>可選</sub> **set** [*setter名稱*](#setter-name)<sub>可選</sub> [*代碼塊*](#code-block)  
> <a name="setter-name"></a>
> *setter 名稱* → **(** [*標識符*](02_Lexical_Structure.md#identifier) **)**  

<a name="getter-setter-keyword-block"></a>
> *getter-setter 關鍵字代碼塊* → **{** [*getter關鍵字子句*](#getter-keyword-clause) [*setter關鍵字子句*](#setter-keyword-clause)<sub>可選</sub> **}**  
> *getter-setter 關鍵字代碼塊* → **{** [*setter關鍵字子句*](#setter-keyword-clause) [*getter關鍵字子句*](#getter-keyword-clause) **}**  
> <a name="getter-keyword-clause"></a>
> *getter 關鍵字子句* → [*特性列表*](06_Attributes.md#attributes)<sub>可選</sub> **get**  
> <a name="setter-keyword-clause"></a>
> *setter 關鍵字子句* → [*特性列表*](06_Attributes.md#attributes)<sub>可選</sub> **set**  

<a name="willSet-didSet-block"></a>
> *willSet-didSet 代碼塊* → **{** [*willSet子句*](#willSet-clause) [*didSet子句*](#didSet-clause)<sub>可選</sub> **}**  
> *willSet-didSet 代碼塊* → **{** [*didSet子句*](#didSet-clause) [*willSet子句*](#willSet-clause)<sub>可選</sub> **}**  
> <a name="willSet-clause"></a>
> *willSet 子句* → [*特性列表*](06_Attributes.md#attributes)<sub>可選</sub> **willSet** [*setter名稱*](#setter-name)<sub>可選</sub> [*代碼塊*](#code-block)  
> <a name="didSet-clause"></a>
> *didSet 子句* → [*特性列表*](06_Attributes.md#attributes)<sub>可選</sub> **didSet** [*setter名稱*](#setter-name)<sub>可選</sub> [*代碼塊*](#code-block)  

<a name="type_alias_declaration"></a>
## 類型別名聲明

*類型別名 (type alias)* 聲明可以在程序中為一個既有類型聲明一個別名。類型別名聲明語句使用關鍵字 `typealias` 聲明，遵循如下的形式：

```swift
typealias 類型別名 = 現存類型
```

當聲明一個類型的別名後，可以在程序的任何地方使用「別名」來代替現有類型。現有類型可以是具有命名的類型或者混合類型。類型別名不產生新的類型，它只是使用別名來引用現有類型。
類型別名聲明可以通過泛型參數來給一個現有泛型類型提供名稱。類型別名為現有類型的一部分或者全部泛型參數提供具體類型。例如:
```swift
typealias StringDictionary<Value> = Dictionary<String, Value>
 
// 下列兩個字典擁有同樣的類型
var dictionary1: StringDictionary<Int> = [:]
var dictionary2: Dictionary<String, Int> = [:]
```
當一個類型別名帶著泛型參數一起被聲明時,這些參數的約束必須與現有參數的約束完全匹配。例如:
```swift
typealias DictionaryOfInts<Key: Hashable> = Dictionary<Key, Int>
```
因為類型別名可以和現有類型相互交換使用,類型別名不可以引入額外的類型約束。
在協議聲明中,類型別名可以為那些經常使用的類型提供一個更短更方便的名稱,例如:
```swift
protocol Sequence {
    associatedtype Iterator: IteratorProtocol
    typealias Element = Iterator.Element
}
 
func sum<T: Sequence>(_ sequence: T) -> Int where T.Element == Int {
    // ...
}
```
假如沒有類型別名,sum函數將必須引用關聯類型通過T.Iterator.Element的形式來替代 T.Element。

另請參閱 [協議關聯類型聲明](#protocol_associated_type_declaration)。

<a name="grammer_of_a_type_alias_declaration"></a>
> 類型別名聲明語法  
> <a name="typealias-declaration"></a>
> *類型別名聲明* → [*類型別名頭*](#typealias-head) [*類型別名賦值*](#typealias-assignment)  
> <a name="typealias-head"></a>
> *類型別名頭* → [*特性列表*](06_Attributes.md#attributes)<sub>可選</sub> [*訪問級別修飾符*](#access-level-modifier)<sub>可選</sub> **typealias** [*類型別名名稱*](#typealias-name)  
> <a name="typealias-name"></a>
> *類型別名名稱* → [*標識符*](02_Lexical_Structure.md#identifier)  
> <a name="typealias-assignment"></a>
> *類型別名賦值* → **=** [*類型*](03_Types.md#type)  

<a name="function_declaration"></a>
## 函數聲明

使用*函數聲明 (function declaration)* 在程序中引入新的函數或者方法。在類、結構體、枚舉，或者協議中聲明的函數會作為方法。函數聲明使用關鍵字 `func`，遵循如下的形式：

```swift
func 函數名稱(參數列表) -> 返回類型 {  
	語句 
}
```

如果函數返回 `Void` 類型，返回類型可以省略，如下所示：

```swift
func 函數名稱(參數列表) {  
	語句
}
```

每個參數的類型都要標明，因為它們不能被推斷出來。如果您在某個參數類型前面加上了 `inout`，那麼這個參數就可以在這個函數作用域當中被修改。更多關於 `inout` 參數的討論，請參閱 [輸入輸出參數](#in-out_parameters)。

函數可以使用元組類型作為返回類型來返回多個值。

函數定義可以出現在另一個函數聲明內。這種函數被稱作*嵌套函數 (nested function)*。更多關於嵌套函數的討論，請參閱 [嵌套函數](../chapter2/06_Functions.md#Nested_Functions)。

<a name="parameter_names"></a>
### 參數名

函數的參數列表由一個或多個函數參數組成，參數間以逗號分隔。函數調用時的參數順序必須和函數聲明時的參數順序一致。最簡單的參數列表有著如下的形式：

`參數名稱`: `參數類型`

一個參數有一個內部名稱，這個內部名稱可以在函數體內被使用。還有一個外部名稱，當調用函數時這個外部名稱被作為實參的標簽來使用。默認情況下，第一個參數的外部名稱會被省略，第二個和之後的參數使用它們的內部名稱作為它們的外部名稱。例如：

```swift
func f(x: Int, y: Int) -> Int { return x + y }
f(1, y: 2) // 參數 y 有標簽，參數 x 則沒有
```

可以按照如下兩種形式之一，重寫參數名稱的默認行為：

`外部參數名稱` `內部參數名稱`: `參數類型`    
_ `內部參數名稱`: `參數類型`

在內部參數名稱前的名稱會作為這個參數的外部名稱，這個名稱可以和內部參數的名稱不同。外部參數名稱在函數被調用時必須被使用，即對應的參數在方法或函數被調用時必須有外部名。

內部參數名稱前的下劃線（`_`）可使該參數在函數被調用時沒有名稱。在函數或方法調用時，對應的參數必須沒有名字。

```swift
func f(x x: Int, withY y: Int, _ z: Int) -> Int { return x + y + z }
f(x: 1, withY: 2, 3) // 參數 x 和 y 是有標簽的，參數 z 則沒有
```

<a name="in-out_parameters"></a>
### 輸入輸出參數

輸入輸出參數被傳遞時遵循如下規則：

1. 函數調用時，參數的值被拷貝。
2. 函數體內部，拷貝後的值被修改。
3. 函數返回後，拷貝後的值被賦值給原參數。

這種行為被稱為*拷入拷出 (copy-in copy-out)* 或*值結果調用 (call by value result)*。例如，當一個計算型屬性或者一個具有屬性觀察器的屬性被用作函數的輸入輸出參數時，其 getter 會在函數調用時被調用，而其 setter 會在函數返回時被調用。

作為一種優化手段，當參數值存儲在內存中的物理地址時，在函數體內部和外部均會使用同一內存位置。這種優化行為被稱為*引用調用 (call by reference)*，它滿足了拷入拷出模式的所有要求，且消除了復制帶來的開銷。在代碼中，要規范使用拷入拷出模式，不要依賴於引用調用。

不要使用傳遞給輸入輸出參數的值，即使原始值在當前作用域中依然可用。當函數返回時，你對原始值所做的更改會被拷貝的值所覆蓋。不要依賴於引用調用的優化機制來試圖避免這種覆蓋。

不能將同一個值傳遞給多個輸入輸出參數，因為這種情況下的拷貝與覆蓋行為的順序是不確定的，因此原始值的最終值也將無法確定。例如：

```swift
var x = 10
func f(inout a: Int, inout _ b: Int) {
    a += 1
    b += 10
}
f(&x, &x) // 編譯報錯 error: inout arguments are not allowed to alias each other
```

如果嵌套函數在外層函數返回後才調用，嵌套函數對輸入輸出參數造成的任何改變將不會影響到原始值。例如：

```swift
func outer(inout a: Int) -> () -> Void {
    func inner() {
        a += 1
    }
    return inner
}

var x = 10
let f = outer(&x)
f()
print(x)
// 打印 「10」
```

調用嵌套函數 `inner()` 對 `a` 遞增後，`x` 的值並未發生改變，因為 `inner()` 在外層函數 `outer()` 返回後才被調用。若要改變 `x` 的值，必須在 `outer()` 返回前調用 `inner()`。

關於輸入輸出參數的詳細討論，請參閱 [輸入輸出參數](../chapter2/06_Functions.md#in_out_parameters)。

<a name="special_kinds_of_parameters"></a>
### 特殊參數

參數可以被忽略，數量可以不固定，還可以為其提供默認值，使用形式如下：

```swift
_ : 參數類型
參數名稱: 參數類型...  
參數名稱: 參數類型 = 默認參數值  
```

以下劃線（`_`）命名的參數會被顯式忽略，無法在函數體內使用。

一個參數的基本類型名稱如果緊跟著三個點（`...`），會被視為可變參數。一個函數至多可以擁有一個可變參數，且必須是最後一個參數。可變參數會作為包含該參數類型元素的數組處理。舉例來講，可變參數 `Int...` 會作為 `[Int]` 來處理。關於使用可變參數的例子，請參閱 [可變參數](../chapter2/06_Functions.md#variadic_parameters)。

如果在參數類型後面有一個以等號（`=`）連接的表達式，該參數會擁有默認值，即給定表達式的值。當函數被調用時，給定的表達式會被求值。如果參數在函數調用時被省略了，就會使用其默認值。

```swift
func f(x: Int = 42) -> Int { return x }
f()     // 有效，使用默認值
f(7)    // 有效，提供了值
f(x: 7) // 無效，該參數沒有外部名稱
```

<a name="special_kinds_of_methods"></a>
### 特殊方法

枚舉或結構體的方法如果會修改 `self`，則必須以 `mutating` 聲明修飾符標記。

子類重寫超類中的方法必須以 `override` 聲明修飾符標記。重寫方法時不使用 `override` 修飾符，或者被 `override` 修飾符修飾的方法並未對超類方法構成重寫，都會導致編譯錯誤。

枚舉或者結構體中的類型方法，要以 `static` 聲明修飾符標記，而對於類中的類型方法，除了使用 `static`，還可使用 `class` 聲明修飾符標記。

<a name="throwing_functions_and_methods"></a>
### 拋出錯誤的函數和方法

可以拋出錯誤的函數或方法必須使用 `throws` 關鍵字標記。這類函數和方法被稱為拋出函數和拋出方法。它們有著下面的形式:

```swift
func 函數名稱(參數列表) throws -> 返回類型 {
	語句
}
```

拋出函數或拋出方法的調用必須包裹在 `try` 或者 `try!` 表達式中（也就是說，在作用域內使用 `try` 或者 `try!` 運算符）。

`throws` 關鍵字是函數的類型的一部分，非拋出函數是拋出函數的子類型。所以，可以在使用拋出函數的地方使用非拋出函數。

不能僅基於函數能否拋出錯誤來進行函數重載。也就是說，可以基於函數的函數類型的參數能否拋出錯誤來進行函數重載。

拋出方法不能重寫非拋出方法，而且拋出方法不能滿足協議對於非拋出方法的要求。也就是說，非拋出方法可以重寫拋出方法，而且非拋出方法可以滿足協議對於拋出方法的要求。

<a name="rethrowing_functions_and_methods"></a>
### 重拋錯誤的函數和方法

函數或方法可以使用 `rethrows` 關鍵字來聲明，從而表明僅當該函數或方法的一個函數類型的參數拋出錯誤時，該函數或方法才拋出錯誤。這類函數和方法被稱為重拋函數和重拋方法。重新拋出錯誤的函數或方法必須至少有一個參數的類型為拋出函數。

```swift
func someFunction(callback: () throws -> Void) rethrows {
    try callback()
}
```

重拋函數或者方法不能夠從自身直接拋出任何錯誤，這意味著它不能夠包含 `throw` 語句。它只能夠傳遞作為參數的拋出函數所拋出的錯誤。例如，在 `do-catch` 代碼塊中調用拋出函數，並在 `catch` 子句中拋出其它錯誤都是不允許的。

拋出方法不能重寫重拋方法，而且拋出方法不能滿足協議對於重拋方法的要求。也就是說，重拋方法可以重寫拋出方法，而且重拋方法可以滿足協議對於拋出方法的要求。

<a name="functions_that_never_return"></a>
### 永不返回的函數

Swift定義了`Never`類型，它表示函數或者方法不會返回給它的調用者。`Never`返回類型的函數或方法可以稱為不歸,不歸函數、方法要麼引發不可恢復的錯誤,要麼永遠不停地運作,這會使調用後本應執行得代碼就不再執行了。但即使是不歸函數、方法,拋錯函數和重拋出函數也可以將程序控制轉移到合適的`catch`代碼塊。

不歸函數、方法可以在guard語句的else字句中調用,具體討論在[*Guard語句*](10_Statements.md#guard_statements)。  
你可以重載一個不歸方法,但是新的方法必須保持原有的返回類型和沒有返回的行為。

<a name="grammer_of_a_function_declaration"></a>
> 函數聲明語法  

<a name="function-declaration"></a>
> *函數聲明* → [*函數頭*](#function-head) [*函數名*](#function-name) [*泛型形參子句*](08_Generic_Parameters_and_Arguments.md#generic-parameter-clause)<sub>可選</sub> [*函數簽名*](#function-signature) [*函數體*](#function-body)<sub>可選</sub>  

<a name="function-head"></a>
> *函數頭* → [*特性列表*](06_Attributes.md#attributes)<sub>可選</sub> [*聲明修飾符列表*](#declaration-modifiers)<sub>可選</sub> **func**  
> <a name="function-name"></a>
> *函數名* → [*標識符*](02_Lexical_Structure.md#identifier) | [*運算符*](02_Lexical_Structure.md#operator)  

<a name="function-signature"></a>
> *函數簽名* → [*參數子句列表*](#parameter-clauses) **throws**<sub>可選</sub> [*函數結果*](#function-result)<sub>可選</sub>  
> *函數簽名* → [*參數子句列表*](#parameter-clauses) **rethrows** [*函數結果*](#function-result)<sub>可選</sub>  
> <a name="function-result"></a>
> *函數結果* → **->** [*特性列表*](06_Attributes.md#attributes)<sub>可選</sub> [*類型*](03_Types.md#type)  
> <a name="function-body"></a>
> *函數體* → [*代碼塊*](#code-block)  

<a name="parameter-clauses"></a>
> *參數子句列表* → [*參數子句*](#parameter-clause) [*參數子句列表*](#parameter-clauses)<sub>可選</sub>  
> <a name="parameter-clause"></a>
> *參數子句* → **(** **)** | **(** [*參數列表*](#parameter-list) **)**  
> <a name="parameter-list"></a>
> *參數列表* → [*參數*](#parameter) | [*參數*](#parameter) **,** [*參數列表*](#parameter-list)  
> <a name="parameter"></a>
> *參數* → **let**<sub>可選</sub> [*外部參數名*](#external-parameter-name)<sub>可選</sub> [*內部參數名*](#local-parameter-name) [*類型標注*](03_Types.md#type-annotation) [*默認參數子句*](#default-argument-clause)<sub>可選</sub>  
> *參數* → **inout** [*外部參數名*](#external-parameter-name)<sub>可選</sub> [*內部參數名*](#local-parameter-name) [*類型標注*](03_Types.md#type-annotation)  
> *參數* → [*外部參數名*](#external-parameter-name)<sub>可選</sub> [*內部參數名*](#local-parameter-name) [*類型標注*](03_Types.md#type-annotation) **...**  
> <a name="external-parameter-name"></a>
> *外部參數名* → [*標識符*](02_Lexical_Structure.md#identifier) | **_**  
> <a name="local-parameter-name"></a>
> *內部參數名* → [*標識符*](02_Lexical_Structure.md#identifier) | **_**  
> <a name="default-argument-clause"></a>
> *默認參數子句* → **=** [*表達式*](04_Expressions.md#expression)  




<a name="enumeration_declaration"></a>
## 枚舉聲明

在程序中使用*枚舉聲明 (enumeration declaration)* 來引入一個枚舉類型。

枚舉聲明有兩種基本形式，使用關鍵字 `enum` 來聲明。枚舉聲明體包含零個或多個值，稱為枚舉用例，還可包含任意數量的聲明，包括計算型屬性、實例方法、類型方法、構造器、類型別名，甚至其他枚舉、結構體和類。枚舉聲明不能包含析構器或者協議聲明。

枚舉類型可以采納任意數量的協議，但是枚舉不能從類、結構體和其他枚舉繼承。

不同於類或者結構體，枚舉類型並不隱式提供默認構造器，所有構造器必須顯式聲明。一個構造器可以委托給枚舉中的其他構造器，但是構造過程僅當構造器將一個枚舉用例賦值給 `self` 後才算完成。

和結構體類似但是和類不同，枚舉是值類型。枚舉實例在被賦值到變量或常量時，或者傳遞給函數作為參數時會被復制。更多關於值類型的信息，請參閱 [結構體和枚舉是值類型](../chapter2/09_Classes_and_Structures.md#structures_and_enumerations_are_value_types)。

可以擴展枚舉類型，正如在 [擴展聲明](#extension_declaration) 中討論的一樣。

<a name="enumerations_with_cases_of_any_type"></a>
### 任意類型的枚舉用例

如下的形式聲明了一個包含任意類型枚舉用例的枚舉變量：

```swift
enum 枚舉名稱: 采納的協議 {
	case 枚舉用例1
	case 枚舉用例2(關聯值類型)
}
```

這種形式的枚舉聲明在其他語言中有時被叫做可識別聯合。

在這種形式中，每個用例塊由關鍵字 `case` 開始，後面緊接一個或多個以逗號分隔的枚舉用例。每個用例名必須是獨一無二的。每個用例也可以指定它所存儲的指定類型的值，這些類型在關聯值類型的元組中被指定，緊跟用例名之後。

具有關聯值的枚舉用例可以像函數一樣使用，通過指定的關聯值創建枚舉實例。和真正的函數一樣，你可以獲取枚舉用例的引用，然後在後續代碼中調用它。

```swift
enum Number {
    case Integer(Int)
    case Real(Double)
}

// f 的類型為 (Int) -> Number
let f = Number.Integer

// 利用 f 把一個整數數組轉成 Number 數組
let evenInts: [Number] = [0, 2, 4, 6].map(f)
```

要獲得更多關於具有關聯值的枚舉用例的信息和例子，請參閱 [關聯值](../chapter2/08_Enumerations.md#associated_values)。

<a name="enumerations_with_indirection"></a>
#### 遞歸枚舉

枚舉類型可以具有遞歸結構，就是說，枚舉用例的關聯值類型可以是枚舉類型自身。然而，枚舉類型的實例具有值語義，這意味著它們在內存中有固定布局。為了支持遞歸，編譯器必須插入一個間接層。

要讓某個枚舉用例支持遞歸，使用 `indirect` 聲明修飾符標記該用例。

```swift
enum Tree<T> {
	case Empty
	indirect case Node(value: T, left: Tree, right:Tree)
}
```

要讓一個枚舉類型的所有用例都支持遞歸，使用 `indirect` 修飾符標記整個枚舉類型，當枚舉有多個用例且每個用例都需要使用 `indirect` 修飾符標記的時候這將非常便利。

被 `indirect` 修飾符標記的枚舉用例必須有一個關聯值。使用 `indirect` 修飾符標記的枚舉類型可以既包含有關聯值的用例，同時還可包含沒有關聯值的用例。但是，它不能再單獨使用 `indirect` 修飾符來標記某個用例。

<a name="enumerations_with_cases_of_a_raw-value_type"></a>
### 擁有原始值的枚舉用例

以下形式聲明了一種枚舉類型，其中各個枚舉用例的類型均為同一種基本類型：

```swift
enum 枚舉名稱: 原始值類型, 采納的協議 {  
	case 枚舉用例1 = 原始值1
	case 枚舉用例2 = 原始值2
}  
```

在這種形式中，每一個用例塊由 `case` 關鍵字開始，後面緊跟一個或多個以逗號分隔的枚舉用例。和第一種形式的枚舉用例不同，這種形式的枚舉用例包含一個基礎值，叫做原始值，各個枚舉用例的原始值的類型必須相同。這些原始值的類型通過原始值類型指定，必須表示一個整數、浮點數、字符串或者字符。原始值類型必須符合 `Equatable` 協議和下列字面量轉換協議中的一種：整型字面量需符合 `IntergerLiteralConvertible` 協議，浮點型字面量需符合 `FloatingPointLiteralConvertible` 協議，包含任意數量字符的字符串型字面量需符合 `StringLiteralConvertible` 協議，僅包含一個單一字符的字符串型字面量需符合 `ExtendedGraphemeClusterLiteralConvertible` 協議。每一個用例的名字和原始值必須唯一。

如果原始值類型被指定為 `Int`，則不必為用例顯式地指定原始值，它們會隱式地被賦值 `0`、`1`、`2` 等。每個未被賦值的 `Int` 類型的用例會被隱式地賦值，其值為上一個用例的原始值加 `1`。

```Swift
enum ExampleEnum: Int {
    case A, B, C = 5, D
}
```

在上面的例子中，`ExampleEnum.A` 的原始值是 `0`，`ExampleEnum.B` 的原始值是 `1`。因為 `ExampleEnum.C` 的原始值被顯式地設定為 `5`，因此 `ExampleEnum.D` 的原始值會自動增長為 `6`。

如果原始值類型被指定為 `String` 類型，你不用明確地為用例指定原始值，每個沒有指定原始值的用例會隱式地將用例名字作為原始值。

```swift
enum WeekendDay: String {
    case Saturday, Sunday
}
```

在上面這個例子中，`WeekendDay.Saturday` 的原始值是 `"Saturday"`，`WeekendDay.Sunday` 的原始值是 `"Sunday"`。

枚舉用例具有原始值的枚舉類型隱式地符合定義在 Swift 標准庫中的 `RawRepresentable` 協議。所以，它們擁有一個 `rawValue` 屬性和一個可失敗構造器 `init?(rawValue: RawValue)`。可以使用 `rawValue` 屬性去獲取枚舉用例的原始值，例如 `ExampleEnum.B.rawValue`。還可以根據原始值來創建一個相對應的枚舉用例，只需調用枚舉的可失敗構造器，例如 `ExampleEnum(rawValue: 5)`，這個可失敗構造器返回一個可選類型的用例。要獲得更多關於具有原始值的枚舉用例的信息和例子，請參閱 [原始值](../chapter2/08_Enumerations.md#raw_values)。

<a name="accessing_enumeration_cases"></a>
### 訪問枚舉用例

使用點語法（`.`）來引用枚舉類型的枚舉用例，例如 `EnumerationType.EnumerationCase`。當枚舉類型可以由上下文推斷而出時，可以省略它（但是 `.` 仍然需要），正如 [枚舉語法](../chapter2/08_Enumerations.md#enumeration_syntax) 和 [顯式成員表達式](04_Expressions.md#explicit_member_expression) 所述。

可以使用 `switch` 語句來檢驗枚舉用例的值，正如 [使用 switch 語句匹配枚舉值](../chapter2/08_Enumerations.md#matching_enumeration_values_with_a_switch_statement) 所述。枚舉類型是模式匹配的，依靠 `switch` 語句 `case` 塊中的枚舉用例模式，正如 [枚舉用例模式](07_Patterns.md#enumeration_case_pattern) 所述。

<a name="grammer_of_an_enumeration_declaration"></a>
> 枚舉聲明語法 

<a name="enum-declaration"></a>
> *枚舉聲明* → [*特性列表*](06_Attributes.md#attributes)<sub>可選</sub> [*訪問級別修飾符*](#access-level-modifier)<sub>可選</sub> [*聯合風格枚舉*](#union-style-enum)  
> *枚舉聲明* → [*特性列表*](06_Attributes.md#attributes)<sub>可選</sub> [*訪問級別修飾符*](#access-level-modifier) <sub>可選</sub> [*原始值風格枚舉*](#raw-value-style-enum) 

<a name="union-style-enum"></a>
> *聯合風格枚舉* → **indirect**<sub>可選</sub> **enum** [*枚舉名稱*](#enum-name) [*泛型形參子句*](08_Generic_Parameters_and_Arguments.md#generic-parameter-clause)<sub>可選</sub> [類型繼承子句](03_Types.md#type-inheritance-clause)<sub>可選</sub> **{** [*多個聯合風格枚舉成員*](#union-style-enum-members)<sub>可選</sub> **}**  
> <a name="union-style-enum-members"></a>
> *多個聯合風格枚舉成員* → [*聯合風格枚舉成員*](#union-style-enum-member) [*多個聯合風格枚舉成員*](#union-style-enum-members)<sub>可選</sub>  
> <a name="union-style-enum-member"></a>
> *聯合風格枚舉成員* → [*聲明*](#declaration) | [*聯合風格枚舉用例子句*](#union-style-enum-case-clause)  
> <a name="union-style-enum-case-clause"></a>
> *聯合風格枚舉用例子句* → [*特性列表*](06_Attributes.md#attributes)<sub>可選</sub> **indirect**<sub>可選</sub> **case** [*聯合風格枚舉用例列表*](#union-style-enum-case-list)  
> <a name="union-style-enum-case-list"></a>
> *聯合風格枚舉用例列表* → [*聯合風格枚舉用例*](#union-style-enum-case) | [*聯合風格枚舉用例*](#union-style-enum-case) **,** [*聯合風格枚舉用例列表*](#union-style-enum-case-list)  
> <a name="union-style-enum-case"></a>
> *聯合風格枚舉用例* → [*枚舉用例名稱*](#enum-case-name) [*元組類型*](03_Types.md#tuple-type)<sub>可選</sub>  
> <a name="enum-name"></a>
> *枚舉名稱* → [*標識符*](02_Lexical_Structure.md#identifier)  
> <a name="enum-case-name"></a>
> *枚舉用例名稱* → [*標識符*](02_Lexical_Structure.md#identifier)  

<a name="raw-value-style-enum"></a>
> *原始值風格枚舉* → **enum** [*枚舉名稱*](#enum-name) [*泛型形參子句*](08_Generic_Parameters_and_Arguments.md#generic-parameter-clause)<sub>可選</sub> [類型繼承子句](03_Types.md#type-inheritance-clause) **{** [*多個原始值風格枚舉成員*](#raw-value-style-enum-members) **}**  
> <a name="raw-value-style-enum-members"></a>
> *多個原始值風格枚舉成員* → [*原始值風格枚舉成員*](#raw-value-style-enum-member) [*多個原始值風格枚舉成員*](#raw-value-style-enum-members)<sub>可選</sub>  
> <a name="raw-value-style-enum-member"></a>
> *原始值風格枚舉成員* → [*聲明*](#declaration) | [*原始值風格枚舉用例子句*](#raw-value-style-enum-case-clause)  
> <a name="raw-value-style-enum-case-clause"></a>
> *原始值風格枚舉用例子句* → [*特性列表*](06_Attributes.md#attributes)<sub>可選</sub> **case** [*原始值風格枚舉用例列表*](#raw-value-style-enum-case-list)  
> <a name="raw-value-style-enum-case-list"></a>
> *原始值風格枚舉用例列表* → [*原始值風格枚舉用例*](#raw-value-style-enum-case) | [*原始值風格枚舉用例*](#raw-value-style-enum-case) **,** [*原始值風格枚舉用例列表*](#raw-value-style-enum-case-list)  
> <a name="raw-value-style-enum-case"></a>
> *原始值風格枚舉用例* → [*枚舉用例名稱*](#enum-case-name) [*原始值賦值*](#raw-value-assignment)<sub>可選</sub>  
> <a name="raw-value-assignment"></a>
> *原始值賦值* → **=** [*原始值字面量*](#raw-value-literal)  
> <a name="raw-value-literal"></a>
> *原始值字面量* → [數字型字面量](02_Lexical_Structure.md#numeric-literal) | [字符串型字面量](02_Lexical_Structure.md#static-string-literal) | [布爾型字面量](02_Lexical_Structure.md#boolean-literal)

<a name="structure_declaration"></a>
## 結構體聲明

使用*結構體聲明 (structure declaration)* 可以在程序中引入一個結構體類型。結構體聲明使用 `struct` 關鍵字，遵循如下的形式：

```swift
struct 結構體名稱: 采納的協議 {  
	多條聲明
}
```

結構體內可包含零個或多個聲明。這些聲明可以包括存儲型和計算型屬性、類型屬性、實例方法、類型方法、構造器、下標、類型別名，甚至其他結構體、類、和枚舉聲明。結構體聲明不能包含析構器或者協議聲明。關於結構體的詳細討論和示例，請參閱 [類和結構體](../chapter2/09_Classes_and_Structures.md)。

結構體可以采納任意數量的協議，但是不能繼承自類、枚舉或者其他結構體。

有三種方法可以創建一個已聲明的結構體實例：

* 調用結構體內聲明的構造器，正如 [構造器](../chapter2/14_Initialization.md#initializers) 所述。

* 如果沒有聲明構造器，調用結構體的成員逐一構造器，正如 [結構體類型的成員逐一構造器](../chapter2/14_Initialization.md#memberwise_initializers_for_structure_types) 所述。

* 如果沒有聲明構造器，而且結構體的所有屬性都有初始值，調用結構體的默認構造器，正如 [默認構造器](../chapter2/14_Initialization.md#default_initializers) 所述。

結構體的構造過程請參閱 [構造過程](../chapter2/14_Initialization.md)。

結構體實例的屬性可以用點語法（`.`）來訪問，正如 [訪問屬性](../chapter2/09_Classes_and_Structures.md#accessing_properties) 所述。

結構體是值類型。結構體的實例在被賦予變量或常量，或傳遞給函數作為參數時會被復制。關於值類型的更多信息，請參閱
[結構體和枚舉是值類型](../chapter2/09_Classes_and_Structures.md#structures_and_enumerations_are_value_types)。

可以使用擴展聲明來擴展結構體類型的行為，請參閱 [擴展聲明](#extension_declaration)。

<a name="grammer_of_a_structure_declaration"></a>
> 結構體聲明語法  
> <a name="struct-declaration"></a>
> *結構體聲明* → [*特性列表*](06_Attributes.md#attributes)<sub>可選</sub> [*訪問級別修飾符*](#access-level-modifier) <sub>可選</sub> **struct** [*結構體名稱*](#struct-name) [*泛型形參子句*](08_Generic_Parameters_and_Arguments.md#generic-parameter-clause)<sub>可選</sub> [類型繼承子句](03_Types.md#type-inheritance-clause)<sub>可選</sub> [*結構體主體*](#struct-body)  
> <a name="struct-name"></a>
> *結構體名稱* → [*標識符*](02_Lexical_Structure.md#identifier)  
> <a name="struct-body"></a>
> *結構體主體* → **{** [*多條聲明*](#declarations)<sub>可選</sub> **}**  

<a name="class_declaration"></a>
## 類聲明

可以在程序中使用*類聲明 (class declaration)* 來引入一個類。類聲明使用關鍵字 `class`，遵循如下的形式：

```swift
class 類名: 超類, 采納的協議 {  
	多條聲明
}
```

類內可以包含零個或多個聲明。這些聲明可以包括存儲型和計算型屬性、實例方法、類型方法、構造器、唯一的析構器、下標、類型別名，甚至其他結構體、類和枚舉聲明。類聲明不能包含協議聲明。關於類的詳細討論和示例，請參閱 [類和結構體](../chapter2/09_Classes_and_Structures.md)。

一個類只能繼承自一個超類，但是可以采納任意數量的協議。超類緊跟在類名和冒號後面，其後跟著采納的協議。泛型類可以繼承自其它泛型類和非泛型類，但是非泛型類只能繼承自其它非泛型類。當在冒號後面寫泛型超類的名稱時，必須寫上泛型類的全名，包括它的泛型形參子句。

正如 [構造器聲明](#initializer_declaration) 所討論的，類可以有指定構造器和便利構造器。類的指定構造器必須初始化類中聲明的所有屬性，並且必須在調用超類構造器之前。

類可以重寫屬性、方法、下標以及構造器。重寫的屬性、方法、下標和指定構造器必須以 `override` 聲明修飾符標記。

為了要求子類去實現超類的構造器，使用 `required` 聲明修飾符標記超類的構造器。子類實現超類構造器時也必須使用 `required` 聲明修飾符。

雖然超類屬性和方法聲明可以被當前類繼承，但是超類聲明的指定構造器卻不能。即便如此，如果當前類重寫了超類的所有指定構造器，它就會繼承超類的所有便利構造器。Swift 的類並不繼承自一個通用基礎類。

有兩種方法來創建已聲明的類的實例：

* 調用類中聲明的構造器，請參閱 [構造器](../chapter2/14_Initialization.md#initializers)。

* 如果沒有聲明構造器，而且類的所有屬性都被賦予了初始值，調用類的默認構造器，請參閱 [默認構造器](../chapter2/14_Initialization.md#default_initializers)。

類實例屬性可以用點語法（`.`）來訪問，請參閱 [訪問屬性](../chapter2/09_Classes_and_Structures.md#accessing_properties)。

類是引用類型。當被賦予常量或變量，或者傳遞給函數作為參數時，類的實例會被引用，而不是被復制。關於引用類型的更多信息，請參閱 [結構體和枚舉是值類型](../chapter2/09_Classes_and_Structures.md#structures_and_enumerations_are_value_types)。

可以使用擴展聲明來擴展類的行為，請參閱 [擴展聲明](#extension_declaration)。

<a name="grammer_of_a_class_declaration"></a>
> 類聲明語法  
> <a name="class-declaration"></a>
> *類聲明* → [*特性列表*](06_Attributes.md#attributes)<sub>可選</sub> [訪問級別修飾符](#access-level-modifier)<sub>可選</sub> **class** [*類名*](#class-name) [*泛型形參子句*](08_Generic_Parameters_and_Arguments.md#generic-parameter-clause)<sub>可選</sub> [*類型繼承子句*](03_Types.md#type-inheritance-clause)<sub>可選</sub> [*類主體*](#class-body)  
> <a name="class-name"></a>
> *類名* → [*標識符*](02_Lexical_Structure.md#identifier)  
> <a name="class-body"></a>
> *類主體* → **{** [*多條聲明*](#declarations)<sub>可選</sub> **}**  

<a name="protocol_declaration"></a>
## 協議聲明

*協議聲明 (protocol declaration)* 可以為程序引入一個命名的協議類型。協議聲明只能在全局區域使用 `protocol` 關鍵字來進行聲明，並遵循如下形式：

```swift
protocol 協議名稱: 繼承的協議 {  
	協議成員聲明
}  
```

協議的主體包含零個或多個協議成員聲明，這些成員描述了任何采納該協議的類型必須滿足的一致性要求。一個協議可以聲明采納者必須實現的某些屬性、方法、構造器以及下標。協議也可以聲明各種各樣的類型別名，叫做關聯類型，它可以指定協議的不同聲明之間的關系。協議聲明不能包括類、結構體、枚舉或者其它協議的聲明。協議成員聲明會在後面進行討論。

協議類型可以繼承自任意數量的其它協議。當一個協議類型繼承自其它協議的時候，來自其它協議的所有要求會聚合在一起，而且采納當前協議的類型必須符合所有的這些要求。關於如何使用協議繼承的例子，請參閱 [協議繼承](../chapter2/22_Protocols.md#protocol_inheritance)。

> 注意  
> 也可以使用協議合成類型來聚合多個協議的一致性要求，請參閱 [協議合成類型](03_Types.md#protocol_composition_type) 和 [協議合成](../chapter2/22_Protocols.md#protocol_composition)。

可以通過類型的擴展聲明來采納協議，從而為之前聲明的類型添加協議一致性。在擴展中，必須實現所有采納協議的要求。如果該類型已經實現了所有的要求，可以讓這個擴展聲明的主體留空。

默認地，符合某個協議的類型必須實現所有在協議中聲明的屬性、方法和下標。即便如此，可以用 `optional` 聲明修飾符標注協議成員聲明，以指定它們的實現是可選的。`optional` 修飾符僅僅可以用於使用 `objc` 特性標記過的協議。因此，僅僅類類型可以采用並符合包含可選成員要求的協議。更多關於如何使用 `optional` 聲明修飾符的信息，以及如何訪問可選協議成員的指導——例如不能確定采納協議的類型是否實現了它們時——請參閱 [可選協議要求](../chapter2/22_Protocols.md#optional_protocol_requirements)

為了限制協議只能被類類型采納，需要使用 `class` 關鍵字來標記協議，將 `class` 關鍵在寫在冒號後面的繼承的協議列表的首位。例如，下面的協議只能被類類型采納：

```swift
protocol SomeProtocol: class {
	/* 這裡是協議成員 */
}
```

任何繼承自標記有 `class` 關鍵字的協議的協議也僅能被類類型采納。

> 注意  
> 如果協議已經用 `objc` 特性標記了，`class` 要求就隱式地應用於該協議，無需顯式使用 `class` 關鍵字。

協議類型是命名的類型，因此它們可以像其他命名類型一樣使用，正如 [協議作為類型](../chapter2/22_Protocols.md#protocols_as_types) 所討論的。然而，不能構造一個協議的實例，因為協議實際上不提供它們指定的要求的實現。

可以使用協議來聲明作為代理的類或者結構體應該實現的方法，正如 [委托(代理)模式](../chapter2/22_Protocols.md#delegation) 中所述。

<a name="grammer_of_a_protocol_declaration"></a>
> 協議聲明語法  

<a name="protocol-declaration"></a>
> *協議聲明* → [*特性列表*](06_Attributes.md#attributes)<sub>可選</sub> [訪問級別修飾符](#access-level-modifier)<sub>可選</sub> **protocol** [*協議名稱*](#protocol-name) [*類型繼承子句*](03_Types.html#type-inheritance-clause)<sub>可選</sub> [*協議主體*](#protocol-body)  
> <a name="protocol-name"></a>
> *協議名稱* → [*標識符*](02_Lexical_Structure.md#identifier)  
> <a name="protocol-body"></a>
> *協議主體* → **{** [*協議成員聲明列表*](#protocol-member-declarations)<sub>可選</sub> **}**  

<a name="protocol-member-declaration"></a>
> *協議成員聲明* → [*協議屬性聲明*](#protocol-property-declaration)  
> *協議成員聲明* → [*協議方法聲明*](#protocol-method-declaration)  
> *協議成員聲明* → [*協議構造器聲明*](#protocol-initializer-declaration)  
> *協議成員聲明* → [*協議下標聲明*](#protocol-subscript-declaration)  
> *協議成員聲明* → [*協議關聯類型聲明*](#protocol-associated-type-declaration)  
> <a name="protocol-member-declarations"></a>
> *協議成員聲明列表* → [*協議成員聲明*](#protocol-member-declaration) [*協議成員聲明列表*](#protocol-member-declarations)<sub>可選</sub>  

<a name="protocol_property_declaration"></a>
### 協議屬性聲明

協議可以通過在協議聲明主體中引入一個協議屬性聲明，來聲明符合的類型必須實現的屬性。協議屬性聲明有一種特殊的變量聲明形式：

```swift
var 屬性名: 類型 { get set }
```

同其它協議成員聲明一樣，這些屬性聲明僅僅針對符合該協議的類型聲明了 getter 和 setter 要求，你不能在協議中直接實現 getter 和 setter。

符合類型可以通過多種方式滿足 getter 和 setter 要求。如果屬性聲明包含 `get` 和 `set` 關鍵字，符合類型就可以用存儲型變量屬性或可讀可寫的計算型屬性來滿足此要求，但是屬性不能以常量屬性或只讀計算型屬性實現。如果屬性聲明僅僅包含 `get` 關鍵字的話，它可以作為任意類型的屬性被實現。關於如何實現協議中的屬性要求的例子，請參閱 [屬性要求](../chapter2/22_Protocols.md#property_requirements)

另請參閱 [變量聲明](#variable_declaration)。

<a name="grammer_of_an_import_declaration"></a>
> 協議屬性聲明語法  
> <a name="protocol-property-declaration"></a>
> *協議屬性聲明* → [*變量聲明頭*](#variable-declaration-head) [*變量名稱*](#variable-name) [*類型標注*](03_Types.md#type-annotation) [*getter-setter關鍵字代碼塊*](#getter-setter-keyword-block)  

<a name="protocol_method_declaration"></a>
### 協議方法聲明

協議可以通過在協議聲明主體中引入一個協議方法聲明，來聲明符合的類型必須實現的方法。協議方法聲明和函數方法聲明有著相同的形式，但有兩項例外：它們不包括函數體，也不能包含默認參數。關於如何實現協議中的方法要求的例子，請參閱 [方法要求](../chapter2/22_Protocols.md#method_requirements)。

使用 `static` 聲明修飾符可以在協議聲明中聲明一個類型方法。結構體實現這些方法時使用 `static` 聲明修飾符，類在實現這些方法時，除了使用 `static` 聲明修飾符，還可以選擇使用 `class` 聲明修飾符。通過擴展實現時亦是如此。

另請參閱 [函數聲明](#function_declaration)。

<a name="grammer_of_a_protocol_declaration"></a>
> 協議方法聲明語法  
> <a name="protocol-method-declaration"></a>
> *協議方法聲明* → [*函數頭*](#function-head) [*函數名*](#function-name) [*泛型形參子句*](08_Generic_Parameters_and_Arguments.md#generic-parameter-clause)<sub>可選</sub> [*函數簽名*](#function-signature)  

<a name="protocol_initializer_declaration"></a>
### 協議構造器聲明

協議可以通過在協議聲明主體中引入一個協議構造器聲明，來聲明符合的類型必須實現的構造器。協議構造器聲明
除了不包含實現主體外，和構造器聲明有著相同的形式。

符合類型可以通過實現一個非可失敗構造器或者 `init!` 可失敗構造器來滿足一個非可失敗協議構造器的要求，可以通過實現任意類型的構造器來滿足一個可失敗協議構造器的要求。

類在實現一個構造器去滿足一個協議的構造器要求時，如果這個類還沒有用 `final` 聲明修飾符標記，這個構造器必須用 `required` 聲明修飾符標記。

另請參閱 [構造器聲明](#initializer_declaration)。

<a name="grammer_of_a_protocol_initializer_declaration"></a>
> 協議構造器聲明語法  
> <a name="protocol-initializer-declaration"></a>
> *協議構造器聲明* → [*構造器頭*](#initializer-head) [*泛型形參子句*](08_Generic_Parameters_and_Arguments.md#generic-parameter-clause)<sub>可選</sub> [*參數子句*](#parameter-clause)  **throws**<sub>可選</sub>  
> *協議構造器聲明* → [*構造器頭*](#initializer-head) [*泛型形參子句*](08_Generic_Parameters_and_Arguments.md#generic-parameter-clause)<sub>可選</sub> [*參數子句*](#parameter-clause)  **rethrows**

<a name="protocol_subscript_declaration"></a>
### 協議下標聲明

協議可以通過在協議聲明主體中引入一個協議下標聲明，來聲明符合的類型必須實現的下標。協議下標聲明有一個特殊的下標聲明形式：

```swift
subscript (參數列表) -> 返回類型 { get set }
```

下標聲明只為符合類型聲明了 getter 和 setter 要求。如果下標聲明包含 `get` 和 `set` 關鍵字，符合類型也必須實現 getter 和 setter 子句。如果下標聲明只包含 `get` 關鍵字，符合類型必須實現 getter 子句，可以選擇是否實現 setter 子句。

另請參閱 [下標聲明](#subscript_declaration)。

<a name="grammer_of_a_protocol_subscript_declaration"></a>
> 協議下標聲明語法  
> <a name="protocol-subscript-declaration"></a>
> *協議下標聲明* → [*下標頭*](#subscript-head) [*下標結果*](#subscript-result) [*getter-setter關鍵字代碼塊*](#getter-setter-keyword-block)  

<a name="protocol_associated_type_declaration"></a>
### 協議關聯類型聲明

使用關鍵字 `associatedtype` 來聲明協議關聯類型。關聯類型為作為協議聲明的一部分，為某種類型提供了一個別名。關聯類型和泛型參數子句中的類型參數很相似，但是它們和 `Self` 一樣，用於協議中。`Self` 指代采納協議的類型。要獲得更多信息和例子，請參閱 [關聯類型](../chapter2/23_Generics.md#associated_types)。

另請參閱 [類型別名聲明](#type_alias_declaration)。

<a name="grammer_of_a_protocol_associated_type_declaration"></a>
> 協議關聯類型聲明語法  
> <a name="protocol-associated-type-declaration"></a>
> *協議關聯類型聲明* → [*類型別名頭*](#typealias-head) [*類型繼承子句*](03_Types.md#type-inheritance-clause)<sub>可選</sub> [*類型別名賦值*](#typealias-assignment)<sub>可選</sub>  

<a name="initializer_declaration"></a>
## 構造器聲明

構造器聲明會為程序中的類、結構體或枚舉引入構造器。構造器使用關鍵字 `init` 來聲明，有兩種基本形式。

結構體、枚舉、類可以有任意數量的構造器，但是類的構造器具有不同的規則和行為。不同於結構體和枚舉，類有兩種構造器，即指定構造器和便利構造器，請參閱 [構造過程](../chapter2/14_Initialization.md)。

采用如下形式聲明結構體和枚舉的構造器，以及類的指定構造器：

```swift
init(參數列表) {  
	構造語句
}  
```

類的指定構造器直接將類的所有屬性初始化。它不能調用類中的其他構造器，如果類有超類，則必須調用超類的一個指定構造器。如果該類從它的超類繼承了屬性，必須在調用超類的指定構造器後才能修改這些屬性。

指定構造器只能在類聲明中聲明，不能在擴展聲明中聲明。

結構體和枚舉的構造器可以調用其他已聲明的構造器，從而委托其他構造器來進行部分或者全部構造過程。

要為類聲明一個便利構造器，用 `convenience` 聲明修飾符來標記構造器聲明：

```swift
convenience init(參數列表) {  
	構造語句
}  
```

便利構造器可以將構造過程委托給另一個便利構造器或一個指定構造器。但是，類的構造過程必須以一個將類中所有屬性完全初始化的指定構造器的調用作為結束。便利構造器不能調用超類的構造器。

可以使用 `required` 聲明修飾符，將便利構造器和指定構造器標記為每個子類都必須實現的構造器。這種構造器的子類實現也必須使用 `required` 聲明修飾符標記。

默認情況下，超類中的構造器不會被子類繼承。但是，如果子類的所有存儲型屬性都有默認值，而且子類自身沒有定義任何構造器，它將繼承超類的構造器。如果子類重寫了超類的所有指定構造器，子類將繼承超類的所有便利構造器。

和方法、屬性和下標一樣，需要使用 `override` 聲明修飾符標記重寫的指定構造器。

> 注意  
> 如果使用 `required` 聲明修飾符標記一個構造器，在子類中重寫這種構造器時，無需使用 `override` 修飾符。

就像函數和方法，構造器也可以拋出或者重拋錯誤，你可以在構造器參數列表的圓括號之後使用 `throws` 或 `rethrows` 關鍵字來表明相應的拋出行為。

關於在不同類型中聲明構造器的例子，請參閱 [構造過程](../chapter2/14_Initialization.md)。

<a name="failable_initializers"></a>
### 可失敗構造器

可失敗構造器可以生成所屬類型的可選實例或者隱式解包可選實例，因此，這種構造器通過返回 `nil` 來指明構造過程失敗。

聲明生成可選實例的可失敗構造器時，在構造器聲明的 `init` 關鍵字後加追加一個問號（`init?`）。聲明生成隱式解包可選實例的可失敗構造器時，在構造器聲明後追加一個嘆號（`init!`）。使用 `init?` 可失敗構造器生成結構體的一個可選實例的例子如下。

```swift
struct SomeStruct {
	let string: String
	//生成一個 SomeStruct 的可選實例
	init?(input: String) {
		if input.isEmpty {
			// 丟棄 self，並返回 nil
			return nil
		}
		string = input
	}
}
```

調用 `init?` 可失敗構造器和調用非可失敗構造器的方式相同，不過你需要處理可選類型的返回值。

```swift
if let actualInstance = SomeStruct(input: "Hello") {
	// 利用 SomeStruct 實例做些事情
} else {
	// SomeStruct 實例的構造過程失敗，構造器返回了 nil
}
```

可失敗構造器可以在構造器實現中的任意位置返回 `nil`。

可失敗構造器可以委托任意種類的構造器。非可失敗可以委托其它非可失敗構造器或者 `init!` 可失敗構造器。非可失敗構造器可以委托超類的 `init?` 可失敗指定構造器，但是需要使用強制解包，例如 `super.init()!`。

構造過程失敗通過構造器委托來傳遞。具體來說，如果可失敗構造器委托的可失敗構造器構造過程失敗並返回 `nil`，那麼該可失敗構造器也會構造失敗並隱式地返回 `nil`。如果非可失敗構造器委托的 `init!` 可失敗構造器構造失敗並返回了 `nil`，那麼會發生運行時錯誤（如同使用 `!` 操作符去強制解包一個值為 `nil` 的可選值）。

子類可以用任意種類的指定構造器重寫超類的可失敗指定構造器，但是只能用非可失敗指定構造器重寫超類的非可失敗指定構造器。

更多關於可失敗構造器的信息和例子，請參閱 [可失敗構造器](../chapter2/14_Initialization.md#failable_initializers)。

<a name="grammer_of_an_initializer_declaration"></a>
> 構造器聲明語法  
> <a name="initializer-declaration"></a>
> *構造器聲明* → [*構造器頭*](#initializer-head) [*泛型形參子句*](08_Generic_Parameters_and_Arguments.md#generic-parameter-clause)<sub>可選</sub> [*參數子句*](#parameter-clause) **throws**<sub>可選</sub> [*構造器主體*](#initializer-body)  
> *構造器聲明* → [*構造器頭*](#initializer-head) [*泛型形參子句*](08_Generic_Parameters_and_Arguments.md#generic-parameter-clause)<sub>可選</sub> [*參數子句*](#parameter-clause) **rethrows**<sub>可選</sub> [*構造器主體*](#initializer-body)  
> <a name="initializer-head"></a>
> *構造器頭* → [*特性列表*](06_Attributes.md#attributes)<sub>可選</sub> [*聲明修飾符列表*](#declaration-modifiers)<sub>可選</sub> **init**  
> *構造器頭* → [*特性列表*](06_Attributes.md#attributes)<sub>可選</sub> [*聲明修飾符列表*](#declaration-modifiers)<sub>可選</sub> **init** **?**  
> *構造器頭* → [*特性列表*](06_Attributes.md#attributes)<sub>可選</sub> [*聲明修飾符列表*](#declaration-modifiers)<sub>可選</sub> **init** **!**  
> <a name="initializer-body"></a>
> *構造器主體* → [*代碼塊*](#code-block)  

<a name="deinitializer_declaration"></a>
## 析構器聲明

*析構器聲明 (deinitializer declaration)* 可以為類聲明一個析構器。析構器沒有參數，遵循如下格式：

```swift
deinit {  
	語句
}
```

當沒有任何強引用引用著類的對象，對象即將被釋放時，析構器會被自動調用。析構器只能在類主體的聲明中聲明，不能在類的擴展聲明中聲明，並且每個類最多只能有一個析構器。

子類會繼承超類的析構器，並會在子類對象將要被釋放時隱式調用。繼承鏈上的所有析構器全部調用完畢後子類對象才會被釋放。

析構器不能直接調用。

關於如何在類聲明中使用析構器的例子，請參閱 [析構過程](../chapter2/15_Deinitialization.md)。

<a name="grammer_of_a_deinitializer_declaration"></a>
> 析構器聲明語法  
> <a name="deinitializer-declaration"></a>
> *析構器聲明* → [*特性列表*](06_Attributes.md#attributes)<sub>可選</sub> **deinit** [*代碼塊*](#code-block)  

<a name="extension_declaration"></a>
## 擴展聲明

*擴展聲明 (extension declaration)* 可以擴展一個現存的類、結構體和枚舉類型的行為。擴展聲明使用關鍵字 `extension`，遵循如下格式：

```swift
extension 類型名稱: 采納的協議 {  
	聲明語句
}
```

擴展聲明體可包含零個或多個聲明語句。這些聲明語句可以包括計算型屬性、計算型類型屬性、實例方法、類型方法、構造器、下標聲明，甚至其他結構體、類和枚舉聲明。擴展聲明不能包含析構器、協議聲明、存儲型屬性、屬性觀察器或其他擴展聲明。關於擴展聲明的詳細討論，以及各種擴展聲明的例子，請參閱 [擴展](../chapter2/21_Extensions.md)。

擴展聲明可以為現存的類、結構體、枚舉添加協議一致性，但是不能為類添加超類，因此在擴展聲明的類型名稱的冒號後面僅能指定一個協議列表。

現存類型的屬性、方法、構造器不能在擴展中被重寫。

擴展聲明可以包含構造器聲明。這意味著，如果被擴展的類型在其他模塊中定義，構造器聲明必須委托另一個在那個模塊中聲明的構造器，以確保該類型能被正確地初始化。

<a name="grammer_of_an_extension_declaration"></a>
> 擴展聲明語法  
> <a name="extension-declaration"></a>
> *擴展聲明* → [訪問級別修飾符](#access-level-modifier)<sub>可選</sub> **extension** [*類型標識符*](03_Types.md#type-identifier) [*類型繼承子句*](03_Types.md#type-inheritance-clause)<sub>可選</sub> [*擴展主體*](#extension-body)  
> <a name="extension-body"></a>
> *擴展主體* → **{** [*多條聲明*](#declarations)<sub>可選</sub> **}**  

<a name="subscript_declaration"></a>
## 下標聲明

*下標聲明 (subscript declaration)* 用於為特定類型的對象添加下標支持，通常也用於為訪問集合、列表和序列中的元素提供語法便利。下標聲明使用關鍵字 `subscript`，形式如下：

```swift
subscript (參數列表) -> 返回類型 {
	get {
		語句
	}
	set(setter 名稱) {
		語句
	}
}
```

下標聲明只能出現在類、結構體、枚舉、擴展和協議的聲明中。

參數列表指定一個或多個用於在相關類型的下標表達式中訪問元素的索引（例如，表達式 `object[i]` 中的 `i`）。索引可以是任意類型，但是必須包含類型標注。返回類型指定了被訪問的元素的類型。

和計算型屬性一樣，下標聲明支持對元素的讀寫操作。getter 用於讀取值，setter 用於寫入值。setter 子句是可選的，當僅需要一個 getter 子句時，可以將二者都忽略，直接返回請求的值即可。但是，如果提供了 setter 子句，就必須提供 getter 子句。

圓括號以及其中的 setter 名稱是可選的。如果提供了 setter 名稱，它會作為 setter 的參數名稱。如果不提供 setter 名稱，那麼 setter 的參數名稱默認是 `value`。setter 名稱的類型必須與返回類型相同。

可以重載下標，只要參數列表或返回類型不同即可。還可以重寫繼承自超類的下標，此時必須使用 `override` 聲明修飾符聲明被重寫的下標。

同樣可以在協議聲明中聲明下標，正如 [協議下標聲明](#protocol_subscript_declaration) 中所述。

更多關於下標的信息和例子，請參閱 [下標](../chapter2/12_Subscripts.md)。

<a name="grammer_of_a_subscript_declaration"></a>
> 下標聲明語法  
> <a name="subscript-declaration"></a>
> *下標聲明* → [*下標頭*](#subscript-head) [*下標結果*](#subscript-result) [*代碼塊*](#code-block)  
> *下標聲明* → [*下標頭*](#subscript-head) [*下標結果*](#subscript-result) [*getter-setter代碼塊*](#getter-setter-block)  
> *下標聲明* → [*下標頭*](#subscript-head) [*下標結果*](#subscript-result) [*getter-setter關鍵字代碼塊*](#getter-setter-keyword-block)  
> <a name="subscript-head"></a>
> *下標頭* → [*特性列表*](06_Attributes.md#attributes)<sub>可選</sub> [*聲明修飾符列表*](#declaration-modifiers)<sub>可選</sub> **subscript** [*參數子句*](#parameter-clause)  
> <a name="subscript-result"></a>
> *下標結果* → **->** [*特性列表*](06_Attributes.md#attributes)<sub>可選</sub> [*類型*](03_Types.md#type)   

<a name="operator_declaration"></a>
## 運算符聲明

*運算符聲明 (operator declaration)* 會向程序中引入中綴、前綴或後綴運算符，使用關鍵字 `operator` 來聲明。

可以聲明三種不同的綴性：中綴、前綴和後綴。運算符的綴性指定了運算符與其運算對象的相對位置。

運算符聲明有三種基本形式，每種綴性各一種。運算符的綴性通過在 `operator` 關鍵字之前添加聲明修飾符 `infix`，`prefix` 或 `postfix` 來指定。每種形式中，運算符的名字只能包含 [運算符](02_Lexical_Structure.md#operators) 中定義的運算符字符。

下面的形式聲明了一個新的中綴運算符：

```swift
infix operator 運算符名稱: 優先級組
```

中綴運算符是二元運算符，置於兩個運算對象之間，例如加法運算符（`+`）位於表達式 `1 + 2` 的中間。

中綴運算符可以選擇指定優先級組。如果沒有為運算符設置優先級組,Swift會設置默認優先級組`DefaultPrecedence`,它的優先級比三目優先級`TernaryPrecedence`要高,更多內容參考[*優先級組聲明*](#precedence_group_declaration_modifiers)  

下面的形式聲明了一個新的前綴運算符：

```swift
prefix operator 運算符名稱 {}
```

出現在運算對象前邊的前綴運算符是一元運算符，例如表達式 `!a` 中的前綴非運算符（`!`）。

前綴運算符的聲明中不指定優先級，而且前綴運算符是非結合的。

下面的形式聲明了一個新的後綴運算符：

```swift
postfix operator 運算符名稱 {}
```

緊跟在運算對象後邊的後綴運算符是一元運算符，例如表達式 `a!` 中的後綴強制解包運算符（`!`）。

和前綴運算符一樣，後綴運算符的聲明中不指定優先級，而且後綴運算符是非結合的。

聲明了一個新的運算符以後，需要實現一個跟這個運算符同名的函數來實現這個運算符。如果是實現一個前綴或者後綴運算符，也必須使用相符的 `prefix` 或者 `postfix` 聲明修飾符標記函數聲明。如果是實現中綴運算符，則不需要使用 `infix` 聲明修飾符標記函數聲明。關於如何實現一個新的運算符的例子，請參閱 [自定義運算符](../chapter2/25_Advanced_Operators.md#custom_operators)。

<a name="grammer_of_an_operator_declaration"></a>
> 運算符聲明語法  

<a name="operator-declaration"></a>
> *運算符聲明* → [*前綴運算符聲明*](#prefix-operator-declaration) | [*後綴運算符聲明*](#postfix-operator-declaration) | [*中綴運算符聲明*](#infix-operator-declaration)  

<a name="prefix-operator-declaration"></a>
> *前綴運算符聲明* → **prefix** **運算符** [*運算符*](02_Lexical_Structure.md#operator) **{** **}**  
> <a name="postfix-operator-declaration"></a>
> *後綴運算符聲明* → **postfix** **運算符** [*運算符*](02_Lexical_Structure.md#operator) **{** **}**  
> <a name="infix-operator-declaration"></a>
> *中綴運算符聲明* → **infix** **運算符** [*運算符*](02_Lexical_Structure.md#operator) **{** [*中綴運算符屬性*](#infix-operator-attributes)<sub>可選</sub> **}**  

<a name="infix-operator-group"></a>
> *中綴運算符組* → [*優先級組名稱*](#precedence-group-name)


<a name="precedence_group_declaration_modifiers"></a>

## 優先級組聲明

*優先級組聲明 (A precedence group declaration)* 會向程序的中綴運算符引入一個全新的優先級組。當沒有用圓括號分組時,運算符優先級反應了運算符與它的操作數的關系的緊密程度。  
優先級組的聲明如下所示:

```swift
precedencegroup 優先級組名稱{
    higherThan: 較低優先級組的名稱
    lowerThan: 較高優先級組的名稱
    associativity: 結合性
    assignment: 賦值性
}
```  
較低優先級組和較高優先級組的名稱說明了新建的優先級組是依賴於現存的優先級組的。 `lowerThan`優先級組的屬性只可以引用當前模塊外的優先級組。當兩個運算符為同一個操作數競爭時,比如表達式`2 + 3 * 5`,優先級更高的運算符將優先參與運算。 

> 注意  
> 使用較低和較高優先級組相互聯系的優先級組必須保持單一層次關系,但它們不必是線性關系。這意味著優先級組也許會有未定義的相關優先級。這些優先級組的運算符在沒有用圓括號分組的情況下是不能緊鄰著使用的。  

Swift定義了大量的優先級組來與標准庫的運算符配合使用,例如相加(`+`)和相減(`-`)屬於`AdditionPrecedence`組,相乘(`*`)和相除(`/`)屬於` MultiplicationPrecedence`組,詳細關於Swift標准庫中一系列運算符和優先級組內容,參閱[Swift標准庫操作符參考](https://developer.apple.com/reference/swift/1851035-swift_standard_library_operators)。  

運算符的結合性表示在沒有圓括號分組的情況下,同樣優先級的一系列運算符是如何被分組的。你可以指定運算符的結合性通過上下文關鍵字`left`、`right`或者`none`,如果沒有指定結合性,默認是`none`關鍵字。左關聯性的運算符是從左至右分組的,例如,相減操作符(-)是左關聯性的,所以表達式`4 - 5 - 6`被分組為`(4 - 5) - 6`,得出結果-7。右關聯性的運算符是從右往左分組的,指定為`none`結合性的運算符就沒有結合性。同樣優先級沒有結合性的運算符不能相鄰出現,例如`<`運算符是`none`結合性,那表示`1 < 2 < 3`就不是一個有效表達式。  

優先級組的賦值性表示在包含可選鏈操作時的運算符優先級。當設為true時,與優先級組對應的運算符在可選鏈操作中使用和標准庫中賦值運算符同樣的分組規則,當設為false或者不設置,該優先級組的運算符與不賦值的運算符遵循同樣的可選鏈規則。

<a name="grammer_of_a_precedence_group_declaration"></a>
> 優先級組聲明語法  

<a name="precedence-group-declaration"></a>
> *優先級組聲明* → **precedence**[*優先級組名稱*](#precedence-group-name){[*多優先級組屬性*](#precedence-group-attributes)<sub>可選</sub> }   
<a name="precedence-group-attributes"></a>
> *優先級組屬性* → [*優先級組屬性*](#precedence-group-attribute)[*多優先級組屬性*](#precedence-group-attributes)<sub>可選</sub> **{** **}**  
<a name="precedence-group-attribute"></a>
> *優先級組屬性* → [*優先級組關系*](#precedence-group-relation)  
> *優先級組屬性* → [*優先級組賦值性*](#precedence-group-assignment)  
> *優先級組屬性* → [*優先級組相關性*](#precedence-group-associativity)  
> <a name="precedence-group-relation"></a>
> *優先級組關系* → **higherThan:**[*多優先級組名稱*](#precedence-group-names)   
> *優先級組關系* → **lowerThan:**[*多優先級組名稱*](#precedence-group-names)   
> <a name="precedence-group-assignment"></a>
> *優先級組賦值* → **assignment:**[*布爾字面值*](02_Lexical_Structure.md#boolean-literal)      
<a name="precedence-group-associativity"></a>
> *優先級組結合性* → **associativity:left**    
> *優先級組結合性* → **associativity:right**   
> *優先級組結合性* → **associativity:none**  
<a name="precedence-group-names"></a>
> *多優先級組名稱* → [*優先級組名稱*](#precedence-group-name) | [*優先級組名稱*](#precedence-group-name) | [*優先級組名稱*](#precedence-group-name)
<a name="precedence-group-name"></a>
> *優先級組名稱* →[*標識符*](02_Lexical_Structure.md#identifier) 


## 聲明修飾符

聲明修飾符都是關鍵字或上下文相關的關鍵字，可以修改一個聲明的行為或者含義。可以在聲明的特性（如果存在）和引入該聲明的關鍵字之間，利用聲明修飾符的關鍵字或上下文相關的關鍵字指定一個聲明修飾符。

`dynamic`

該修飾符用於修飾任何兼容 Objective-C 的類的成員。訪問被 `dynamic` 修飾符標記的類成員將總是由 Objective-C 運行時系統進行動態派發，而不會由編譯器進行內聯或消虛擬化。

因為被標記 `dynamic` 修飾符的類成員會由 Objective-C 運行時系統進行動態派發，所以它們會被隱式標記 `objc` 特性。

`final`

該修飾符用於修飾類或類中的屬性、方法以及下標。如果用它修飾一個類，那麼這個類不能被繼承。如果用它修飾類中的屬性、方法或下標，那麼它們不能在子類中被重寫。

`lazy`

該修飾符用於修飾類或結構體中的存儲型變量屬性，表示該屬性的初始值最多只被計算和存儲一次，且發生在它被第一次訪問時。關於如何使用 `lazy` 修飾符的例子，請參閱 [惰性存儲型屬性](../chapter2/10_Properties.md#lazy_stored_properties)。

`optional`

該修飾符用於修飾協議中的屬性、方法以及下標成員，表示符合類型可以不實現這些成員要求。

只能將 `optional` 修飾符用於被 `objc` 特性標記的協議。這樣一來，就只有類類型可以采納並符合擁有可選成員要求的協議。關於如何使用 `optional` 修飾符，以及如何訪問可選協議成員（比如，不確定符合類型是否已經實現了這些可選成員）的信息，請參閱 [可選協議要求](../chapter2/22_Protocols.md#optional_protocol_requirements)。

`required`

該修飾符用於修飾類的指定構造器或便利構造器，表示該類所有的子類都必須實現該構造器。在子類實現該構造器時，必須同樣使用 `required` 修飾符修飾該構造器。

`weak`

該修飾符用於修飾變量或存儲型變量屬性，表示該變量或屬性持有其存儲的對象的弱引用。這種變量或屬性的類型必須是可選的類類型。使用 `weak` 修飾符可避免強引用循環。關於 `weak` 修飾符的更多信息和例子，請參閱 [弱引用](../chapter2/16_Automatic_Reference_Counting.md#resolving_strong_reference_cycles_between_class_instances)。

<a name="access_control_levels"></a>
### 訪問控制級別

Swift 提供了三個級別的訪問控制：`public`、`internal` 和 `private`。可以使用以下任意一種訪問級別修飾符來指定聲明的訪問級別。訪問控制在 [訪問控制](../chapter2/24_Access_Control.md) 中有詳細討論。

`public`

該修飾符表示聲明可被同模塊的代碼訪問，只要其他模塊導入了聲明所在的模塊，也可以進行訪問。

`internal`

該修飾符表示聲明只能被同模塊的代碼訪問。默認情況下，絕大多數聲明會被隱式標記 `internal` 訪問級別修飾符。

`private`

該修飾符表示聲明只能被所在源文件的代碼訪問。

以上訪問級別修飾符都可以選擇帶上一個參數，該參數由一對圓括號和其中的 `set` 關鍵字組成（例如，`private(set)`）。使用這種形式的訪問級別修飾符來限制某個屬性或下標的 setter 的訪問級別低於其本身的訪問級別，正如 [Getter 和 Setter](../chapter2/24_Access_Control.md#getters_and_setters) 中所討論的。

<a name="grammer_of_a_declaration_modifier"></a>
> 聲明修飾符的語法

<a name="declaration-modifier"></a>
> *聲明修飾符* → **class** | **convenience**| **dynamic** | **final** | **infix** | **lazy** | **mutating** | **nonmutating** | **optional** | **override** | **postfix** | **prefix** | **required** | **static** | **unowned** | **unowned ( safe )** | **unowned ( unsafe )** | **weak**  
> 聲明修飾符 → [*訪問級別修飾符*](#access-level-modifier)  
> <a name="declaration-modifiers"></a>
> *聲明修飾符列表* → [*聲明修飾符*](#declaration-modifier) [*聲明修飾符列表*](#declaration-modifiers)<sub>可選</sub>

<a name="access-level-modifier"></a>
>訪問級別修飾符 → **internal** | **internal ( set )**  
>訪問級別修飾符 → **private** | **private ( set )**  
>訪問級別修飾符 → **public** | **public ( set )**  
