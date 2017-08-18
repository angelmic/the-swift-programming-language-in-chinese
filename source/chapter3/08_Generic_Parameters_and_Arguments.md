# 泛型參數（Generic Parameters and Arguments）
---------

> 1.0
> 翻譯：[fd5788](https://github.com/fd5788)
> 校對：[yankuangshi](https://github.com/yankuangshi), [stanzhai](https://github.com/stanzhai)

> 2.0
> 翻譯+校對：[wardenNScaiyi](https:github.com/wardenNScaiyi)

> 3.0
> 翻譯+校對：[chenmingjia](https:github.com/chenmingjia)

本頁包含內容：

- [泛型形參子句](#generic_parameter)
    - [Where 子句](#where_clauses)
- [泛型實參子句](#generic_argument)

本節涉及泛型類型、泛型函數以及泛型構造器的參數，包括形參和實參。聲明泛型類型、函數或構造器時，須指定相應的類型參數。類型參數相當於一個占位符，當實例化泛型類型、調用泛型函數或泛型構造器時，就用具體的類型實參替代之。

關於 Swift 語言的泛型概述，請參閱 [泛型](../chapter2/23_Generics.md)。

<a name="generic_parameter"></a>
## 泛型形參子句

泛型形參子句指定泛型類型或函數的類型形參，以及這些參數相關的約束和要求。泛型形參子句用尖括號（`<>`）包住，形式如下：

> <`泛型形參列表`>  

泛型形參列表中泛型形參用逗號分開，其中每一個采用以下形式：

> `類型形參` : `約束`

泛型形參由兩部分組成：類型形參及其後的可選約束。類型形參只是占位符類型（如 `T`，`U`，`V`，`Key`，`Value` 等）的名字而已。你可以在泛型類型、函數的其余部分或者構造器聲明，包括函數或構造器的簽名中使用它（以及它的關聯類型）。

約束用於指明該類型形參繼承自某個類或者符合某個協議或協議組合。例如，在下面的泛型函數中，泛型形參 `T: Comparable` 表示任何用於替代類型形參 `T` 的類型實參必須滿足 `Comparable` 協議。


```swift
func simpleMax<T: Comparable>(_ x: T, _ y: T) -> T {
    if x < y {
        return y
    }
    return x
}
```

例如，因為 `Int` 和 `Double` 均滿足`Comparable`協議，所以該函數可以接受這兩種類型。與泛型類型相反，調用泛型函數或構造器時不需要指定泛型實參子句。類型實參由傳遞給函數或構造器的實參推斷而出。

```swift
simpleMax(17, 42) // T 被推斷為 Int 類型
simpleMax(3.14159, 2.71828) // T 被推斷為 Double 類型
```

<a name="where_clauses"></a>
### Where 子句

要想對類型形參及其關聯類型指定額外要求，可以在函數體或者類型的大括號之前添加 `where` 子句。`where` 子句由關鍵字 `where` 及其後的用逗號分隔的一個或多個要求組成。

> `where` : `類型要求`

`where` 子句中的要求用於指明該類型形參繼承自某個類或符合某個協議或協議組合。盡管 `where` 子句提供了語法糖使其有助於表達類型形參上的簡單約束（如 `<T: Comparable>` 等同於 `<T> where T: Comparable`，等等），但是依然可以用來對類型形參及其關聯類型提供更復雜的約束,例如你可以強制形參的關聯類型遵守協議,如，` <S: Sequence> where S.Iterator.Element: Equatable` 表示泛型類型 `S` 遵守`Sequence`協議並且關聯類型`S.Iterator.Element`遵守`Equatable`協議,這個約束確保隊列的每一個元素都是符合 `Equatable` 協議的。

也可以用操作符 `==` 來指定兩個類型必須相同。例如，泛型形參子句 ` <S1: Sequence, S2: Sequence> where S1.Iterator.Element == S2.Iterator.Element` 表示 `S1` 和 `S2` 必須都符合 `SequenceType` 協議，而且兩個序列中的元素類型必須相同。

當然，替代類型形參的類型實參必須滿足所有的約束和要求。

泛型函數或構造器可以重載，但在泛型形參子句中的類型形參必須有不同的約束或要求，抑或二者皆不同。當調用重載的泛型函數或構造器時，編譯器會根據這些約束來決定調用哪個重載函數或構造器。

更多關於泛型where從句的信息和關於泛型函數聲明的例子,可以看一看 [泛型where子句](https://github.com/numbbbbb/the-swift-programming-language-in-chinese/blob/gh-pages/source/chapter2/23_Generics.md#where_clauses)

> 泛型形參子句語法  

<a name="generic-parameter-clause"></a>
> *泛型形參子句* → **<** [*泛型形參列表*](#generic-parameter-list) [*約束子句*](#requirement-clause)<sub>可選</sub> **>**  
<a name="generic-parameter-list"></a>
> *泛型形參列表* → [*泛形形參*](#generic-parameter) | [*泛形形參*](#generic-parameter) **,** [*泛型形參列表*](#generic-parameter-list)  
<a name="generic-parameter"></a>
> *泛形形參* → [*類型名稱*](03_Types.html#type-name)  
> *泛形形參* → [*類型名稱*](03_Types.html#type-name) **:** [*類型標識符*](03_Types.html#type-identifier)  
> *泛形形參* → [*類型名稱*](03_Types.html#type-name) **:** [*協議合成類型*](03_Types.html#protocol-composition-type)  

<a name="requirement-clause"></a>
> *約束子句* → **where** [*約束列表*](#requirement-list)  
<a name="requirement-list"></a>
> *約束列表* → [*約束*](#requirement) | [*約束*](#requirement) **,** [*約束列表*](#requirement-list)  
<a name="requirement"></a>
> *約束* → [*一致性約束*](#conformance-requirement) | [*同類型約束*](#same-type-requirement)  

<a name="conformance-requirement"></a>
> *一致性約束* → [*類型標識符*](03_Types.html#type-identifier) **:** [*類型標識符*](03_Types.html#type-identifier)  
> *一致性約束* → [*類型標識符*](03_Types.html#type-identifier) **:** [*協議合成類型*](03_Types.html#protocol-composition-type)  
<a name="same-type-requirement"></a>
> *同類型約束* → [*類型標識符*](03_Types.html#type-identifier) **==** [*類型*](03_Types.html#type)  


<a name="generic_argument"></a>
## 泛型實參子句

泛型實參子句指定泛型類型的類型實參。泛型實參子句用尖括號（`<>`）包住，形式如下：

> <`泛型實參列表`>

泛型實參列表中類型實參用逗號分開。類型實參是實際具體類型的名字，用來替代泛型類型的泛型形參子句中的相應的類型形參。從而得到泛型類型的一個特化版本。例如，Swift 標准庫中的泛型字典類型的的簡化定義如下：

```swift
struct Dictionary<Key: Hashable, Value>: CollectionType, DictionaryLiteralConvertible {
    /* ... */
}
```

泛型 `Dictionary` 類型的特化版本，`Dictionary<String, Int>` 就是用具體的 `String` 和 `Int` 類型替代泛型類型 `Key: Hashable` 和 `Value` 產生的。每一個類型實參必須滿足它所替代的泛型形參的所有約束，包括任何 `where` 子句所指定的額外的關聯類型要求。上面的例子中，類型形參 `Key` 的類型必須符合 `Hashable` 協議，因此 `String` 也必須滿足 `Hashable` 協議。

可以用本身就是泛型類型的特化版本的類型實參替代類型形參（假設已滿足合適的約束和關聯類型要求）。例如，為了生成一個元素類型是整型數組的數組，可以用數組的特化版本 `Array<Int>` 替代泛型類型 `Array<T>` 的類型形參 `T` 來實現。

```swift
let arrayOfArrays: Array<Array<Int>> = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
```

如 [泛型形參子句](#generic_parameter) 所述，不能用泛型實參子句來指定泛型函數或構造器的類型實參。

> 泛型實參子句語法  
<a name="generic-argument-clause"></a>
> *泛型實參子句* → **<** [*泛型實參列表*](#generic-argument-list) **>**  
<a name="generic-argument-list"></a>
> *泛型實參列表* → [*泛型實參*](#generic-argument) | [*泛型實參*](#generic-argument) **,** [*泛型實參列表*](#generic-argument-list)  
<a name="generic-argument"></a>
> *泛型實參* → [*類型*](03_Types.html#type)  
