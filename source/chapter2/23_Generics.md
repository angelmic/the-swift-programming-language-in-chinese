# 泛型

------

> 1.0
> 翻譯：[takalard](https://github.com/takalard)
> 校對：[lifedim](https://github.com/lifedim)

> 2.0
> 翻譯+校對： [SergioChan](https://github.com/SergioChan)

> 2.1
> 校對：[shanks](http://codebuild.me)，2015-11-01

> 2.2：翻譯+校對：[Lanford](https://github.com/LanfordCai)，2016-04-08 [SketchK](https://github.com/SketchK) 2016-05-16

> 3.0：翻譯+校對：[chenmingjia](https://github.com/chenmingjia)，2016-09-12   
> 3.0.1，shanks，2016-11-13

> 3.1：翻譯：[qhd](https://github.com/qhd)，2017-04-10   

本頁包含內容：

- [泛型所解決的問題](#the_problem_that_generics_solve)
- [泛型函數](#generic_functions)
- [類型參數](#type_parameters)
- [命名類型參數](#naming_type_parameters)
- [泛型類型](#generic_types)
- [擴展一個泛型類型](#extending_a_generic_type)
- [類型約束](#type_constraints)
- [關聯類型](#associated_types)
- [泛型 Where 語句](#where_clauses)
- [具有泛型 where 子句的擴展](#extensions_with_a_generic_where_clause)

*泛型代碼*讓你能夠根據自定義的需求，編寫出適用於任意類型、靈活可重用的函數及類型。它能讓你避免代碼的重復，用一種清晰和抽象的方式來表達代碼的意圖。

泛型是 Swift 最強大的特性之一，許多 Swift 標准庫是通過泛型代碼構建的。事實上，泛型的使用貫穿了整本語言手冊，只是你可能沒有發現而已。例如，Swift 的 `Array` 和 `Dictionary` 都是泛型集合。你可以創建一個 `Int` 數組，也可創建一個 `String` 數組，甚至可以是任意其他 Swift 類型的數組。同樣的，你也可以創建存儲任意指定類型的字典。

<a name="the_problem_that_generics_solve"></a>
## 泛型所解決的問題

下面是一個標准的非泛型函數 `swapTwoInts(_:_:)`，用來交換兩個 `Int` 值：

```swift
func swapTwoInts(_ a: inout Int, _ b: inout Int) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```

這個函數使用輸入輸出參數（`inout`）來交換 `a` 和 `b` 的值，請參考[輸入輸出參數](./06_Functions.html#in_out_parameters)。

`swapTwoInts(_:_:)` 函數交換 `b` 的原始值到 `a`，並交換 `a` 的原始值到 `b`。你可以調用這個函數交換兩個 `Int` 變量的值：

```swift
var someInt = 3
var anotherInt = 107
swapTwoInts(&someInt, &anotherInt)
print("someInt is now \(someInt), and anotherInt is now \(anotherInt)")
// 打印 「someInt is now 107, and anotherInt is now 3」
```

誠然，`swapTwoInts(_:_:)` 函數挺有用，但是它只能交換 `Int` 值，如果你想要交換兩個 `String` 值或者 `Double`值，就不得不寫更多的函數，例如 `swapTwoStrings(_:_:)` 和 `swapTwoDoubles(_:_:)`，如下所示：

```swift
func swapTwoStrings(_ a: inout String, _ b: inout String) {
    let temporaryA = a
    a = b
    b = temporaryA
}
 
func swapTwoDoubles(_ a: inout Double, _ b: inout Double) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```

你可能注意到 `swapTwoInts(_:_:)`、`swapTwoStrings(_:_:)` 和 `swapTwoDoubles(_:_:)` 的函數功能都是相同的，唯一不同之處就在於傳入的變量類型不同，分別是 `Int`、`String` 和 `Double`。

在實際應用中，通常需要一個更實用更靈活的函數來交換兩個任意類型的值，幸運的是，泛型代碼幫你解決了這種問題。（這些函數的泛型版本已經在下面定義好了。）

> 注意  
在上面三個函數中，`a` 和 `b` 類型相同。如果 `a` 和 `b` 類型不同，那它們倆就不能互換值。Swift 是類型安全的語言，所以它不允許一個 `String` 類型的變量和一個 `Double` 類型的變量互換值。試圖這樣做將導致編譯錯誤。

<a name="generic_functions"></a>
## 泛型函數

泛型函數可以適用於任何類型，下面的 `swapTwoValues(_:_:)` 函數是上面三個函數的泛型版本：

```swift
func swapTwoValues<T>(_ a: inout T, _ b: inout T) {
    let temporaryA = a
    a = b
    b = temporaryA
}

```

`swapTwoValues(_:_:)` 的函數主體和 `swapTwoInts(_:_:)` 函數是一樣的，它們只在第一行有點不同，如下所示：

```swift
func swapTwoInts(_ a: inout Int, _ b: inout Int)
func swapTwoValues<T>(_ a: inout T, _ b: inout T)
```

這個函數的泛型版本使用了占位類型名（在這裡用字母 `T` 來表示）來代替實際類型名（例如 `Int`、`String` 或 `Double`）。占位類型名沒有指明 `T` 必須是什麼類型，但是它指明了 `a` 和 `b` 必須是同一類型 `T`，無論 `T` 代表什麼類型。只有 `swapTwoValues(_:_:)` 函數在調用時，才能根據所傳入的實際類型決定 `T` 所代表的類型。

另外一個不同之處在於這個泛型函數名（`swapTwoValues(_:_:)`）後面跟著占位類型名（`T`），並用尖括號括起來（`<T>`）。這個尖括號告訴 Swift 那個 `T` 是 `swapTwoValues(_:_:)` 函數定義內的一個占位類型名，因此 Swift 不會去查找名為 `T` 的實際類型。

`swapTwoValues(_:_:)` 函數現在可以像 `swapTwoInts(_:_:)` 那樣調用，不同的是它能接受兩個任意類型的值，條件是這兩個值有著相同的類型。`swapTwoValues(_:_:)` 函數被調用時，`T` 所代表的類型都會由傳入的值的類型推斷出來。

在下面的兩個例子中，`T` 分別代表 `Int` 和 `String`：

```swift
var someInt = 3
var anotherInt = 107
swapTwoValues(&someInt, &anotherInt)
// someInt 現在 107, and anotherInt 現在 3

var someString = "hello"
var anotherString = "world"
swapTwoValues(&someString, &anotherString)
// someString 現在 "world", and anotherString 現在 "hello"
```

> 注意  
上面定義的 `swapTwoValues(_:_:)` 函數是受 `swap(_:_:)` 函數啟發而實現的。後者存在於 Swift 標准庫，你可以在你的應用程序中使用它。如果你在代碼中需要類似 `swapTwoValues(_:_:)` 函數的功能，你可以使用已存在的 `swap(_:_:)` 函數。

<a name="type_parameters"></a>
## 類型參數

在上面的 `swapTwoValues(_:_:)` 例子中，占位類型 `T` 是類型參數的一個例子。類型參數指定並命名一個占位類型，並且緊隨在函數名後面，使用一對尖括號括起來（例如 `<T>`）。

一旦一個類型參數被指定，你可以用它來定義一個函數的參數類型（例如 `swapTwoValues(_:_:)` 函數中的參數 `a` 和 `b`），或者作為函數的返回類型，還可以用作函數主體中的注釋類型。在這些情況下，類型參數會在函數調用時被實際類型所替換。（在上面的 `swapTwoValues(_:_:)` 例子中，當函數第一次被調用時，`T` 被 `Int` 替換，第二次調用時，被 `String` 替換。）

你可提供多個類型參數，將它們都寫在尖括號中，用逗號分開。

<a name="naming_type_parameters"></a>
## 命名類型參數

在大多數情況下，類型參數具有一個描述性名字，例如 `Dictionary<Key, Value>` 中的 `Key` 和 `Value`，以及 `Array<Element>` 中的 `Element`，這可以告訴閱讀代碼的人這些類型參數和泛型函數之間的關系。然而，當它們之間沒有有意義的關系時，通常使用單個字母來命名，例如 `T`、`U`、`V`，正如上面演示的 `swapTwoValues(_:_:)` 函數中的 `T` 一樣。

> 注意  
請始終使用大寫字母開頭的駝峰命名法（例如 `T` 和 `MyTypeParameter`）來為類型參數命名，以表明它們是占位類型，而不是一個值。

<a name="generic_types"></a>
## 泛型類型

除了泛型函數，Swift 還允許你定義泛型類型。這些自定義類、結構體和枚舉可以適用於任何類型，類似於 `Array` 和 `Dictionary`。

這部分內容將向你展示如何編寫一個名為 `Stack` （棧）的泛型集合類型。棧是一系列值的有序集合，和 `Array` 類似，但它相比 Swift 的 `Array` 類型有更多的操作限制。數組允許在數組的任意位置插入新元素或是刪除其中任意位置的元素。而棧只允許在集合的末端添加新的元素（稱之為入*棧*）。類似的，棧也只能從末端移除元素（稱之為*出*棧）。

> 注意  
棧的概念已被 `UINavigationController` 類用來構造視圖控制器的導航結構。你通過調用 `UINavigationController` 的 `pushViewController(_:animated:)` 方法來添加新的視圖控制器到導航棧，通過 `popViewControllerAnimated(_:)` 方法來從導航棧中移除視圖控制器。每當你需要一個嚴格的「後進先出」方式來管理集合，棧都是最實用的模型。

下圖展示了一個棧的入棧（push）和出棧（pop）的行為：

![此處輸入圖片的描述](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/stackPushPop_2x.png)

1. 現在有三個值在棧中。
2. 第四個值被壓入到棧的頂部。
3. 現在有四個值在棧中，最近入棧的那個值在頂部。
4. 棧中最頂部的那個值被移除，或稱之為出棧。
5. 移除掉一個值後，現在棧又只有三個值了。

下面展示了如何編寫一個非泛型版本的棧，以 `Int` 型的棧為例：

```swift
struct IntStack {
    var items = [Int]()
    mutating func push(_ item: Int) {
        items.append(item)
    }
    mutating func pop() -> Int {
        return items.removeLast()
    }
}
```

這個結構體在棧中使用一個名為 `items` 的 `Array` 屬性來存儲值。`Stack` 提供了兩個方法：`push(_:)` 和 `pop()`，用來向棧中壓入值以及從棧中移除值。這些方法被標記為 `mutating`，因為它們需要修改結構體的 `items` 數組。

上面的 `IntStack` 結構體只能用於 `Int` 類型。不過，可以定義一個泛型 `Stack` 結構體，從而能夠處理*任意*類型的值。

下面是相同代碼的泛型版本：

```swift
struct Stack<Element> {
    var items = [Element]()
    mutating func push(_ item: Element) {
        items.append(item)
    }
    mutating func pop() -> Element {
        return items.removeLast()
    }
}
```

注意，`Stack` 基本上和 `IntStack` 相同，只是用占位類型參數 `Element` 代替了實際的 `Int` 類型。這個類型參數包裹在緊隨結構體名的一對尖括號裡（`<Element>`）。

`Element` 為待提供的類型定義了一個占位名。這種待提供的類型可以在結構體的定義中通過 `Element` 來引用。在這個例子中，`Element` 在如下三個地方被用作占位符：

- 創建 `items` 屬性，使用 `Element` 類型的空數組對其進行初始化。
- 指定 `push(_:)` 方法的唯一參數 `item` 的類型必須是 `Element` 類型。
- 指定 `pop()` 方法的返回值類型必須是 `Element` 類型。

由於 `Stack` 是泛型類型，因此可以用來創建 Swift 中任意有效類型的棧，就像 `Array` 和 `Dictionary` 那樣。

你可以通過在尖括號中寫出棧中需要存儲的數據類型來創建並初始化一個 `Stack` 實例。例如，要創建一個 `String` 類型的棧，可以寫成 `Stack<String>()`：

```swift
var stackOfStrings = Stack<String>()
stackOfStrings.push("uno")
stackOfStrings.push("dos")
stackOfStrings.push("tres")
stackOfStrings.push("cuatro")
// 棧中現在有 4 個字符串
```

下圖展示了 `stackOfStrings` 如何將這四個值入棧：

![此處輸入圖片的描述](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/stackPushedFourStrings_2x.png)

移除並返回棧頂部的值 `"cuatro"`，即將其出棧：

```swift
let fromTheTop = stackOfStrings.pop()
// fromTheTop 的值為 "cuatro"，現在棧中還有 3 個字符串
```

下圖展示了 `stackOfStrings` 如何將頂部的值出棧：

![此處輸入圖片的描述](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/stackPoppedOneString_2x.png)

<a name="extending_a_generic_type"></a>
## 擴展一個泛型類型

當你擴展一個泛型類型的時候，你並不需要在擴展的定義中提供類型參數列表。原始類型定義中聲明的類型參數列表在擴展中可以直接使用，並且這些來自原始類型中的參數名稱會被用作原始定義中類型參數的引用。

下面的例子擴展了泛型類型 `Stack`，為其添加了一個名為 `topItem` 的只讀計算型屬性，它將會返回當前棧頂端的元素而不會將其從棧中移除：

```swift
extension Stack {
    var topItem: Element? {
        return items.isEmpty ? nil : items[items.count - 1]
    }
}
```

`topItem` 屬性會返回一個 `Element` 類型的可選值。當棧為空的時候，`topItem` 會返回 `nil`；當棧不為空的時候，`topItem` 會返回 `items` 數組中的最後一個元素。

注意，這個擴展並沒有定義一個類型參數列表。相反的，`Stack` 類型已有的類型參數名稱 `Element`，被用在擴展中來表示計算型屬性 `topItem` 的可選類型。

計算型屬性 `topItem` 現在可以用來訪問任意 `Stack` 實例的頂端元素且不移除它：

```swift
if let topItem = stackOfStrings.topItem {
    print("The top item on the stack is \(topItem).")
}
// 打印 「The top item on the stack is tres.」
```

<a name="type_constraints"></a>
## 類型約束

`swapTwoValues(_:_:)` 函數和 `Stack` 類型可以作用於任何類型。不過，有的時候如果能將使用在泛型函數和泛型類型中的類型添加一個特定的類型約束，將會是非常有用的。類型約束可以指定一個類型參數必須繼承自指定類，或者符合一個特定的協議或協議組合。

例如，Swift 的 `Dictionary` 類型對字典的鍵的類型做了些限制。在[字典](./04_Collection_Types.html#dictionaries)的描述中，字典的鍵的類型必須是可哈希（`hashable`）的。也就是說，必須有一種方法能夠唯一地表示它。`Dictionary` 的鍵之所以要是可哈希的，是為了便於檢查字典是否已經包含某個特定鍵的值。若沒有這個要求，`Dictionary` 將無法判斷是否可以插入或者替換某個指定鍵的值，也不能查找到已經存儲在字典中的指定鍵的值。

為了實現這個要求，一個類型約束被強制加到 `Dictionary` 的鍵類型上，要求其鍵類型必須符合 `Hashable` 協議，這是 Swift 標准庫中定義的一個特定協議。所有的 Swift 基本類型（例如 `String`、`Int`、`Double` 和 `Bool`）默認都是可哈希的。

當你創建自定義泛型類型時，你可以定義你自己的類型約束，這些約束將提供更為強大的泛型編程能力。抽象概念，例如可哈希的，描述的是類型在概念上的特征，而不是它們的顯式類型。

<a name="type_constraint_syntax"></a>
### 類型約束語法

你可以在一個類型參數名後面放置一個類名或者協議名，並用冒號進行分隔，來定義類型約束，它們將成為類型參數列表的一部分。對泛型函數添加類型約束的基本語法如下所示（作用於泛型類型時的語法與之相同）：

```swift
func someFunction<T: SomeClass, U: SomeProtocol>(someT: T, someU: U) {
    // 這裡是泛型函數的函數體部分
}
```

上面這個函數有兩個類型參數。第一個類型參數 `T`，有一個要求 `T` 必須是 `SomeClass` 子類的類型約束；第二個類型參數 `U`，有一個要求 `U` 必須符合 `SomeProtocol` 協議的類型約束。

<a name="type_constraints_in_action"></a>
### 類型約束實踐

這裡有個名為 `findIndex(ofString:in:)` 的非泛型函數，該函數的功能是在一個 `String` 數組中查找給定 `String` 值的索引。若查找到匹配的字符串，` findIndex(ofString:in:)` 函數返回該字符串在數組中的索引值，否則返回 `nil`：

```swift
func findIndex(ofString valueToFind: String, in array: [String]) -> Int? {
    for (index, value) in array.enumerated() {
        if value == valueToFind {
            return index
        }
    }
    return nil
}
```

`findIndex(ofString:in:)` 函數可以用於查找字符串數組中的某個字符串：

```swift
let strings = ["cat", "dog", "llama", "parakeet", "terrapin"]
if let foundIndex = findIndex(ofString: "llama", in: strings) {
    print("The index of llama is \(foundIndex)")
}
// 打印 「The index of llama is 2」
```

如果只能查找字符串在數組中的索引，用處不是很大。不過，你可以用占位類型 `T` 替換 `String` 類型來寫出具有相同功能的泛型函數 `findIndex(_:_:)`。

下面展示了 `findIndex(ofString:in:)` 函數的泛型版本 `findIndex(ofString:in:)`。請注意這個函數返回值的類型仍然是 `Int?`，這是因為函數返回的是一個可選的索引數，而不是從數組中得到的一個可選值。需要提醒的是，這個函數無法通過編譯，原因會在例子後面說明：

```swift
func findIndex<T>(of valueToFind: T, in array:[T]) -> Int? {
    for (index, value) in array.enumerated() {
        if value == valueToFind {
            return index
        }
    }
    return nil
}
```

上面所寫的函數無法通過編譯。問題出在相等性檢查上，即 "`if value == valueToFind`"。不是所有的 Swift 類型都可以用等式符（`==`）進行比較。比如說，如果你創建一個自定義的類或結構體來表示一個復雜的數據模型，那麼 Swift 無法猜到對於這個類或結構體而言「相等」意味著什麼。正因如此，這部分代碼無法保證適用於每個可能的類型 `T`，當你試圖編譯這部分代碼時會出現相應的錯誤。

不過，所有的這些並不會讓我們無從下手。Swift 標准庫中定義了一個 `Equatable` 協議，該協議要求任何遵循該協議的類型必須實現等式符（`==`）及不等符(`!=`)，從而能對該類型的任意兩個值進行比較。所有的 Swift 標准類型自動支持 `Equatable` 協議。

任何 `Equatable` 類型都可以安全地使用在 `findIndex(of:in:)` 函數中，因為其保證支持等式操作符。為了說明這個事實，當你定義一個函數時，你可以定義一個 `Equatable` 類型約束作為類型參數定義的一部分：

```swift
func findIndex<T: Equatable>(of valueToFind: T, in array:[T]) -> Int? {
    for (index, value) in array.enumerated() {
        if value == valueToFind {
            return index
        }
    }
    return nil
}
```

`findIndex(of:in:)` 唯一的類型參數寫做 `T: Equatable`，也就意味著「任何符合 `Equatable` 協議的類型 `T` 」。

`findIndex(of:in:)` 函數現在可以成功編譯了，並且可以作用於任何符合 `Equatable` 的類型，如 `Double` 或 `String`：

```swift
let doubleIndex = findIndex(of: 9.3, in: [3.14159, 0.1, 0.25])
// doubleIndex 類型為 Int?，其值為 nil，因為 9.3 不在數組中
let stringIndex = findIndex(of: "Andrea", in: ["Mike", "Malcolm", "Andrea"])
// stringIndex 類型為 Int?，其值為 2
```

<a name="associated_types"></a>
## 關聯類型

定義一個協議時，有的時候聲明一個或多個關聯類型作為協議定義的一部分將會非常有用。*關聯類型*為協議中的某個類型提供了一個占位名（或者說別名），其代表的實際類型在協議被采納時才會被指定。你可以通過 `associatedtype` 關鍵字來指定關聯類型。

<a name="associated_types_in_action"></a>
### 關聯類型實踐

下面例子定義了一個 `Container` 協議，該協議定義了一個關聯類型 `ItemType`：

```swift
protocol Container {
    associatedtype ItemType
    mutating func append(_ item: ItemType)
    var count: Int { get }
    subscript(i: Int) -> ItemType { get }
}
```

`Container` 協議定義了三個任何采納了該協議的類型（即容器）必須提供的功能：

- 必須可以通過 `append(_:)` 方法添加一個新元素到容器裡。
- 必須可以通過 `count` 屬性獲取容器中元素的數量，並返回一個 `Int` 值。
- 必須可以通過索引值類型為 `Int` 的下標檢索到容器中的每一個元素。

這個協議沒有指定容器中元素該如何存儲，以及元素必須是何種類型。這個協議只指定了三個任何遵從 `Container` 協議的類型必須提供的功能。遵從協議的類型在滿足這三個條件的情況下也可以提供其他額外的功能。

任何遵從 `Container` 協議的類型必須能夠指定其存儲的元素的類型，必須保證只有正確類型的元素可以加進容器中，必須明確通過其下標返回的元素的類型。

為了定義這三個條件，`Container` 協議需要在不知道容器中元素的具體類型的情況下引用這種類型。`Container` 協議需要指定任何通過 `append(_:)` 方法添加到容器中的元素和容器中的元素是相同類型，並且通過容器下標返回的元素的類型也是這種類型。

為了達到這個目的，`Container` 協議聲明了一個關聯類型 `ItemType`，寫作 `associatedtype ItemType`。這個協議無法定義 `ItemType` 是什麼類型的別名，這個信息將留給遵從協議的類型來提供。盡管如此，`ItemType` 別名提供了一種方式來引用 `Container` 中元素的類型，並將之用於 `append(_:)` 方法和下標，從而保證任何 `Container` 的行為都能夠正如預期地被執行。

下面是先前的非泛型的 `IntStack` 類型，這一版本采納並符合了 `Container` 協議：

```swift
struct IntStack: Container {
    // IntStack 的原始實現部分
    var items = [Int]()
    mutating func push(_ item: Int) {
        items.append(item)
    }
    mutating func pop() -> Int {
        return items.removeLast()
    }
    // Container 協議的實現部分
    typealias ItemType = Int
    mutating func append(_ item: Int) {
        self.push(item)
    }
    var count: Int {
        return items.count
    }
    subscript(i: Int) -> Int {
        return items[i]
    }
}
```

`IntStack` 結構體實現了 `Container` 協議的三個要求，其原有功能也不會和這些要求相沖突。

此外，`IntStack` 在實現 `Container` 的要求時，指定 `ItemType` 為 `Int` 類型，即 `typealias ItemType = Int`，從而將 `Container` 協議中抽象的 `ItemType` 類型轉換為具體的 `Int` 類型。

由於 Swift 的類型推斷，你實際上不用在 `IntStack` 的定義中聲明 `ItemType` 為 `Int`。因為 `IntStack` 符合 `Container` 協議的所有要求，Swift 只需通過 `append(_:)` 方法的 `item` 參數類型和下標返回值的類型，就可以推斷出 `ItemType` 的具體類型。事實上，如果你在上面的代碼中刪除了 `typealias ItemType = Int` 這一行，一切仍舊可以正常工作，因為 Swift 清楚地知道 `ItemType` 應該是哪種類型。

你也可以讓泛型 `Stack` 結構體遵從 `Container` 協議：

```swift
struct Stack<Element>: Container {
    // Stack<Element> 的原始實現部分
    var items = [Element]()
    mutating func push(_ item: Element) {
        items.append(item)
    }
    mutating func pop() -> Element {
        return items.removeLast()
    }
    // Container 協議的實現部分
    mutating func append(_ item: Element) {
        self.push(item)
    }
    var count: Int {
        return items.count
    }
    subscript(i: Int) -> Element {
        return items[i]
    }
}
```

這一次，占位類型參數 `Element` 被用作 `append(_:)` 方法的 `item` 參數和下標的返回類型。Swift 可以據此推斷出 `Element` 的類型即是 `ItemType` 的類型。

<a name="extending_an_existing_type_to_specify_an_associated_type"></a>
### 通過擴展一個存在的類型來指定關聯類型

[通過擴展添加協議一致性](./22_Protocols.html#adding_protocol_conformance_with_an_extension)中描述了如何利用擴展讓一個已存在的類型符合一個協議，這包括使用了關聯類型的協議。

Swift 的 `Array` 類型已經提供 `append(_:)` 方法，一個 `count` 屬性，以及一個接受 `Int` 類型索引值的下標用以檢索其元素。這三個功能都符合 `Container` 協議的要求，也就意味著你只需簡單地聲明 `Array` 采納該協議就可以擴展 `Array`，使其遵從 `Container` 協議。你可以通過一個空擴展來實現這點，正如[通過擴展采納協議](./22_Protocols.html#declaring_protocol_adoption_with_an_extension)中的描述：

```swift
extension Array: Container {}
```

如同上面的泛型 `Stack` 結構體一樣，`Array` 的 `append(_:)` 方法和下標確保了 Swift 可以推斷出 `ItemType` 的類型。定義了這個擴展後，你可以將任意 `Array` 當作 `Container` 來使用。

<a name="where_clauses"></a>
## 泛型 Where 語句

[類型約束](#type_constraints)讓你能夠為泛型函數或泛型類型的類型參數定義一些強制要求。

為關聯類型定義約束也是非常有用的。你可以在參數列表中通過 `where` 子句為關聯類型定義約束。你能通過 `where` 子句要求一個關聯類型遵從某個特定的協議，以及某個特定的類型參數和關聯類型必須類型相同。你可以通過將 `where` 關鍵字緊跟在類型參數列表後面來定義 `where` 子句，`where` 子句後跟一個或者多個針對關聯類型的約束，以及一個或多個類型參數和關聯類型間的相等關系。你可以在函數體或者類型的大括號之前添加 where 子句。

下面的例子定義了一個名為 `allItemsMatch` 的泛型函數，用來檢查兩個 `Container` 實例是否包含相同順序的相同元素。如果所有的元素能夠匹配，那麼返回 `true`，否則返回 `false`。

被檢查的兩個 `Container` 可以不是相同類型的容器（雖然它們可以相同），但它們必須擁有相同類型的元素。這個要求通過一個類型約束以及一個 `where` 子句來表示：

```swift
func allItemsMatch<C1: Container, C2: Container>
    (_ someContainer: C1, _ anotherContainer: C2) -> Bool
    where C1.ItemType == C2.ItemType, C1.ItemType: Equatable {
        
        // 檢查兩個容器含有相同數量的元素
        if someContainer.count != anotherContainer.count {
            return false
        }
        
        // 檢查每一對元素是否相等
        for i in 0..<someContainer.count {
            if someContainer[i] != anotherContainer[i] {
                return false
            }
        }
        
        // 所有元素都匹配，返回 true
        return true
}
```

這個函數接受 `someContainer` 和 `anotherContainer` 兩個參數。參數 `someContainer` 的類型為 `C1`，參數 `anotherContainer` 的類型為 `C2`。`C1` 和 `C2` 是容器的兩個占位類型參數，函數被調用時才能確定它們的具體類型。

這個函數的類型參數列表還定義了對兩個類型參數的要求：

- `C1` 必須符合 `Container` 協議（寫作 `C1: Container`）。
- `C2` 必須符合 `Container` 協議（寫作 `C2: Container`）。
- `C1` 的 `ItemType` 必須和 `C2` 的 `ItemType`類型相同（寫作 `C1.ItemType == C2.ItemType`）。
- `C1` 的 `ItemType` 必須符合 `Equatable` 協議（寫作 `C1.ItemType: Equatable`）。

第三個和第四個要求被定義為一個 `where` 子句，寫在關鍵字 `where` 後面，它們也是泛型函數類型參數列表的一部分。

這些要求意味著：

- `someContainer` 是一個 `C1` 類型的容器。
- `anotherContainer` 是一個 `C2` 類型的容器。
- `someContainer` 和 `anotherContainer` 包含相同類型的元素。
- `someContainer` 中的元素可以通過不等於操作符（`!=`）來檢查它們是否彼此不同。

第三個和第四個要求結合起來意味著 `anotherContainer` 中的元素也可以通過 `!=` 操作符來比較，因為它們和 `someContainer` 中的元素類型相同。

這些要求讓 `allItemsMatch(_:_:)` 函數能夠比較兩個容器，即使它們的容器類型不同。

`allItemsMatch(_:_:)` 函數首先檢查兩個容器是否擁有相同數量的元素，如果它們的元素數量不同，那麼一定不匹配，函數就會返回 `false`。

進行這項檢查之後，通過 `for-in` 循環和半閉區間操作符（`..<`）來迭代每個元素，檢查 `someContainer` 中的元素是否不等於 `anotherContainer` 中的對應元素。如果兩個元素不相等，那麼兩個容器不匹配，函數返回 `false`。

如果循環體結束後未發現任何不匹配的情況，表明兩個容器匹配，函數返回 `true`。

下面演示了 `allItemsMatch(_:_:)` 函數的使用：

```swift
var stackOfStrings = Stack<String>()
stackOfStrings.push("uno")
stackOfStrings.push("dos")
stackOfStrings.push("tres")

var arrayOfStrings = ["uno", "dos", "tres"]

if allItemsMatch(stackOfStrings, arrayOfStrings) {
    print("All items match.")
} else {
    print("Not all items match.")
}
// 打印 「All items match.」
```

上面的例子創建了一個 `Stack` 實例來存儲一些 `String` 值，然後將三個字符串壓入棧中。這個例子還通過數組字面量創建了一個 `Array` 實例，數組中包含同棧中一樣的三個字符串。即使棧和數組是不同的類型，但它們都遵從 `Container` 協議，而且它們都包含相同類型的值。因此你可以用這兩個容器作為參數來調用 `allItemsMatch(_:_:)` 函數。在上面的例子中，`allItemsMatch(_:_:)` 函數正確地顯示了這兩個容器中的所有元素都是相互匹配的。

<a name="extensions_with_a_generic_where_clause"></a>
## 具有泛型 where 子句的擴展

你也可以使用泛型 `where` 子句作為擴展的一部分。基於以前的例子，下面的示例擴展了泛型 `Stack` 結構體，添加一個 `isTop(_:)` 方法。  

```swift
extension Stack where Element: Equatable {
    func isTop(_ item: Element) -> Bool {
        guard let topItem = items.last else {
            return false
        }
        return topItem == item
    }
}
```

這個新的 `isTop(_:)` 方法首先檢查這個棧是不是空的，然後比較給定的元素與棧頂部的元素。如果你嘗試不用泛型 `where` 子句，會有一個問題：在 `isTop(_:)` 裡面使用了 `==` 運算符，但是 `Stack` 的定義沒有要求它的元素是符合 Equatable 協議的，所以使用 `==` 運算符導致編譯時錯誤。使用泛型 `where` 子句可以為擴展添加新的條件，因此只有當棧中的元素符合 Equatable 協議時，擴展才會添加 `isTop(_:)` 方法。  

以下是 `isTop(_:)` 方法的調用方式：  

```swift
if stackOfStrings.isTop("tres") {
    print("Top element is tres.")
} else {
    print("Top element is something else.")
}
// 打印 "Top element is tres."
```

如果嘗試在其元素不符合 Equatable 協議的棧上調用 `isTop(_:)` 方法，則會收到編譯時錯誤。  

```swift
struct NotEquatable { }
var notEquatableStack = Stack<NotEquatable>()
let notEquatableValue = NotEquatable()
notEquatableStack.push(notEquatableValue)
notEquatableStack.isTop(notEquatableValue)  // 報錯
```

你可以使用泛型 `where` 子句去擴展一個協議。基於以前的示例，下面的示例擴展了 `Container` 協議，添加一個 `startsWith(_:)` 方法。  

```swift
extension Container where Item: Equatable {
    func startsWith(_ item: Item) -> Bool {
        return count >= 1 && self[0] == item
    }
}
```

這個 `startsWith(_:)` 方法首先確保容器至少有一個元素，然後檢查容器中的第一個元素是否與給定的元素相等。任何符合 `Container` 協議的類型都可以使用這個新的 `startsWith(_:)` 方法，包括上面使用的棧和數組，只要容器的元素是符合 Equatable 協議的。  

```swift
if [9, 9, 9].startsWith(42) {
    print("Starts with 42.")
} else {
    print("Starts with something else.")
}
// 打印 "Starts with something else."
```

上述示例中的泛型 `where` 子句要求 `Item` 符合協議，但也可以編寫一個泛型 `where` 子句去要求 `Item` 為特定類型。例如：  

```swift
extension Container where Item == Double {
    func average() -> Double {
        var sum = 0.0
        for index in 0..<count {
            sum += self[index]
        }
        return sum / Double(count)
    }
}
print([1260.0, 1200.0, 98.6, 37.0].average())
// 打印 "648.9"
```

此示例將一個 `average()` 方法添加到 `Item` 類型為 `Double` 的容器中。此方法遍歷容器中的元素將其累加，並除以容器的數量計算平均值。它將數量從 `Int` 轉換為 `Double` 確保能夠進行浮點除法。  

就像可以在其他地方寫泛型 `where` 子句一樣，你可以在一個泛型 `where` 子句中包含多個條件作為擴展的一部分。用逗號分隔列表中的每個條件。  
