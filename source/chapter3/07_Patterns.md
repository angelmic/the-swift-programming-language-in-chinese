# 模式（Patterns）
-----------------

> 1.0
> 翻譯：[honghaoz](https://github.com/honghaoz)
> 校對：[numbbbbb](https://github.com/numbbbbb), [stanzhai](https://github.com/stanzhai)

> 2.0
> 翻譯+校對：[ray16897188](https://github.com/ray16897188),

> 2.1
> 翻譯：[BridgeQ](https://github.com/WXGBridgeQ)

本頁內容包括：

- [通配符模式（Wildcard Pattern）](#wildcard_pattern)
- [標識符模式（Identifier Pattern）](#identifier_pattern)
- [值綁定模式（Value-Binding Pattern）](#value-binding_pattern)
- [元組模式（Tuple Pattern）](#tuple_pattern)
- [枚舉用例模式（Enumeration Case Pattern）](#enumeration_case_pattern)
- [可選模式（Optional Pattern）](#optional_pattern)
- [類型轉換模式（Type-Casting Pattern）](#type-casting_patterns)
- [表達式模式（Expression Pattern）](#expression_pattern)

模式代表單個值或者復合值的結構。例如，元組 `(1, 2)` 的結構是由逗號分隔的，包含兩個元素的列表。因為模式代表一種值的結構，而不是特定的某個值，你可以利用模式來匹配各種各樣的值。比如，`(x, y)` 可以匹配元組 `(1, 2)`，以及任何含兩個元素的元組。除了利用模式匹配一個值以外，你可以從復合值中提取出部分或全部值，然後分別把各個部分的值和一個常量或變量綁定起來。

Swift 中的模式分為兩類：一種能成功匹配任何類型的值，另一種在運行時匹配某個特定值時可能會失敗。

第一類模式用於解構簡單變量、常量和可選綁定中的值。此類模式包括通配符模式、標識符模式，以及包含前兩種模式的值綁定模式和元組模式。你可以為這類模式指定一個類型標注，從而限制它們只能匹配某種特定類型的值。

第二類模式用於全模式匹配，這種情況下你試圖匹配的值在運行時可能不存在。此類模式包括枚舉用例模式、可選模式、表達式模式和類型轉換模式。你在 `switch` 語句的 `case` 標簽中，`do` 語句的 `catch` 子句中，或者在 `if`、`while`、`guard` 和 `for-in` 語句的 `case` 條件句中使用這類模式。

> 模式語法  
<a name="pattern"></a>
> *模式* → [*通配符模式*](#wildcard_pattern) [*類型標注*](03_Types.md#type-annotation)<sub>可選</sub>  
> *模式* → [*標識符模式*](#identifier_pattern) [*類型標注*](03_Types.md#type-annotation)<sub>可選</sub>  
> *模式* → [*值綁定模式*](#value-binding-pattern)  
> *模式* → [*元組模式*](#tuple-pattern) [*類型標注*](03_Types.md#type-annotation)<sub>可選</sub>  
> *模式* → [*枚舉用例模式*](#enum-case-pattern)  
> *模式* → [*可選模式*](#optional-pattern)  
> *模式* → [*類型轉換模式*](#type-casting-pattern)  
> *模式* → [*表達式模式*](#expression-pattern)  

<a name="wildcard_pattern"></a>
## 通配符模式（Wildcard Pattern）

通配符模式由一個下劃線（`_`）構成，用於匹配並忽略任何值。當你想忽略被匹配的值時可以使用該模式。例如，下面這段代碼在閉區間 `1...3` 中迭代，每次迭代都忽略該區間的當前值：

```swift
for _ in 1...3 {
    // ...
}
```

> 通配符模式語法  
<a name="wildcard-pattern"></a>
> *通配符模式* → **_**  

<a name="identifier_pattern"></a>
## 標識符模式（Identifier Pattern）

標識符模式匹配任何值，並將匹配的值和一個變量或常量綁定起來。例如，在下面的常量聲明中，`someValue` 是一個標識符模式，匹配了 `Int` 類型的 `42`：

```swift
let someValue = 42
```

當匹配成功時，`42` 被綁定（賦值）給常量 `someValue`。

如果一個變量或常量聲明的左邊是一個標識符模式，那麼這個標識符模式是值綁定模式的子模式。

> 標識符模式語法  
<a name="identifier-pattern"></a>
> *標識符模式* → [*標識符*](02_Lexical_Structure.md#identifier)  

<a name="value-binding_pattern"></a>
## 值綁定模式（Value-Binding Pattern）

值綁定模式把匹配到的值綁定給一個變量或常量。把匹配到的值綁定給常量時，用關鍵字 `let`，綁定給變量時，用關鍵字 `var`。

在值綁定模式中的標識符模式會把新命名的變量或常量與匹配到的值做綁定。例如，你可以拆開一個元組，然後把每個元素綁定到相應的標識符模式中。

```swift
let point = (3, 2)
switch point {
// 將 point 中的元素綁定到 x 和 y
case let (x, y):
    print("The point is at (\(x), \(y)).")
}
// 打印 「The point is at (3, 2).」
```

在上面這個例子中，`let` 會分配到元組模式 `(x, y)` 中的各個標識符模式。因此，`switch` 語句中 `case let (x, y):` 和 `case (let x, let y):` 的匹配效果是一樣的。

> 值綁定模式語法  
<a name="value-binding-pattern"></a>
> *值綁定模式* → **var** [*模式*](#pattern) | **let** [*模式*](#pattern)  

<a name="tuple_pattern"></a>
## 元組模式

元組模式是由逗號分隔的，具有零個或多個模式的列表，並由一對圓括號括起來。元組模式匹配相應元組類型的值。

你可以使用類型標注去限制一個元組模式能匹配哪種元組類型。例如，在常量聲明 `let (x, y): (Int, Int) = (1, 2)` 中的元組模式 `(x, y): (Int, Int)` 只匹配兩個元素都是 `Int` 類型的元組。

當元組模式被用於 `for-in` 語句或者變量和常量聲明時，它僅可以包含通配符模式、標識符模式、可選模式或者其他包含這些模式的元組模式。比如下面這段代碼就不正確，因為 `(x, 0)` 中的元素 `0` 是一個表達式模式：

```swift
let points = [(0, 0), (1, 0), (1, 1), (2, 0), (2, 1)]
// 下面的代碼是錯誤的
for (x, 0) in points {
    /* ... */
}
```

只包含一個元素的元組模式的圓括號沒有效果，模式只匹配這個單個元素的類型。舉例來說，下面的語句是等效的：

```swift
let a = 2        // a: Int = 2
let (a) = 2      // a: Int = 2
let (a): Int = 2 // a: Int = 2
```

> 元組模式語法  
<a name="tuple-pattern"></a>
> *元組模式* → **(** [*元組模式元素列表*](#tuple-pattern-element-list)<sub>可選</sub> **)**  
<a name="tuple-pattern-element-list"></a>
> *元組模式元素列表* → [*元組模式元素*](#tuple-pattern-element) | [*元組模式元素*](#tuple-pattern-element) **,** [*元組模式元素列表*](#tuple-pattern-element-list)  
<a name="tuple-pattern-element"></a>
> *元組模式元素* → [*模式*](#pattern)  

<a name="enumeration_case_pattern"></a>
## 枚舉用例模式（Enumeration Case Pattern）

枚舉用例模式匹配現有的某個枚舉類型的某個用例。枚舉用例模式出現在 `switch` 語句中的 `case` 標簽中，以及 `if`、`while`、`guard` 和 `for-in` 語句的 `case` 條件中。

如果你准備匹配的枚舉用例有任何關聯的值，則相應的枚舉用例模式必須指定一個包含每個關聯值元素的元組模式。關於使用 `switch` 語句來匹配包含關聯值的枚舉用例的例子，請參閱 [關聯值](../chapter2/08_Enumerations.md#associated_values)。

> 枚舉用例模式語法  
<a name="enum-case-pattern"></a>
> *枚舉用例模式* → [*類型標識*](03_Types.md#type-identifier)<sub>可選</sub> **.** [*枚舉用例名*](05_Declarations.md#enum-case-name) [*元組模式*](#tuple-pattern)<sub>可選</sub>  

<a name="optional_pattern"></a>
## 可選模式（Optional Pattern）

可選模式匹配包裝在一個 `Optional(Wrapped)` 或者 `ExplicitlyUnwrappedOptional(Wrapped)` 枚舉中的 `Some(Wrapped)` 用例中的值。可選模式由一個標識符模式和緊隨其後的一個問號組成，可以像枚舉用例模式一樣使用。

由於可選模式是 `Optional` 和 `ImplicitlyUnwrappedOptional` 枚舉用例模式的語法糖，下面兩種寫法是等效的：

```swift
let someOptional: Int? = 42
// 使用枚舉用例模式匹配
if case .Some(let x) = someOptional {
    print(x)
}

// 使用可選模式匹配
if case let x? = someOptional {
    print(x)
}
```

可選模式為 `for-in` 語句提供了一種迭代數組的簡便方式，只為數組中非 `nil` 的元素執行循環體。

```swift
let arrayOfOptionalInts: [Int?] = [nil, 2, 3, nil, 5]
// 只匹配非 nil 的元素
for case let number? in arrayOfOptinalInts {
    print("Found a \(number)")
}
// Found a 2
// Found a 3
// Found a 5
```

> 可選模式語法  
<a name="optional-pattern"></a>
> *可選模式* → [*標識符模式*](03_Types.md#type-identifier) **?**

<a name="type-casting_patterns"></a>
## 類型轉換模式（Type-Casting Patterns）

有兩種類型轉換模式，`is` 模式和 `as` 模式。`is` 模式只出現在 `switch` 語句中的 `case` 標簽中。`is` 模式和 `as` 模式形式如下：

> is `類型`  
> `模式` as `類型`

`is` 模式僅當一個值的類型在運行時和 `is` 模式右邊的指定類型一致，或者是其子類的情況下，才會匹配這個值。`is` 模式和 `is` 運算符有相似表現，它們都進行類型轉換，但是 `is` 模式沒有返回類型。

`as` 模式僅當一個值的類型在運行時和 `as` 模式右邊的指定類型一致，或者是其子類的情況下，才會匹配這個值。如果匹配成功，被匹配的值的類型被轉換成 `as` 模式右邊指定的類型。

關於使用 `switch` 語句配合 `is` 模式和 `as` 模式來匹配值的例子，請參閱 [Any 和 AnyObject 的類型轉換](../chapter2/19_Type_Casting.md#type_casting_for_any_and_anyobject)。

> 類型轉換模式語法  
<a name="type-casting-pattern"></a>
> *類型轉換模式* → [*is模式*](#is-pattern) | [*as模式*](#as-pattern)  
<a name="is-pattern"></a>
> *is模式* → **is** [*類型*](03_Types.md#type)  
<a name="as-pattern"></a>
> *as模式* → [*模式*](#pattern) **as** [*類型*](03_Types.md#type)  

<a name="expression_pattern"></a>
## 表達式模式（Expression Pattern）

表達式模式代表表達式的值。表達式模式只出現在 `switch` 語句中的 `case` 標簽中。

表達式模式代表的表達式會使用 Swift 標准庫中的 `~=` 運算符與輸入表達式的值進行比較。如果 `~=` 運算符返回 `true`，則匹配成功。默認情況下，`~=` 運算符使用 `==` 運算符來比較兩個相同類型的值。它也可以將一個整型數值與一個 `Range` 實例中的一段整數區間做匹配，正如下面這個例子所示：

```swift
let point = (1, 2)
switch point {
case (0, 0):
    print("(0, 0) is at the origin.")
case (-2...2, -2...2):
    print("(\(point.0), \(point.1)) is near the origin.")
default:
    print("The point is at (\(point.0), \(point.1)).")
}
// 打印 「(1, 2) is near the origin.」
```

你可以重載 `~=` 運算符來提供自定義的表達式匹配行為。比如你可以重寫上面的例子，將 `point` 表達式與字符串形式表示的點進行比較。

```swift
// 重載 ~= 運算符對字符串和整數進行比較
func ~=(pattern: String, value: Int) -> Bool {
    return pattern == "\(value)"
}

switch point {
case ("0", "0"):
    print("(0, 0) is at the origin.")
default:
    print("The point is at (\(point.0), \(point.1)).")
}
// 打印 「The point is at (1, 2).」
```

> 表達式模式語法  
<a name="expression-pattern"></a>
> *表達式模式* → [*表達式*](04_Expressions.md#expression)  
