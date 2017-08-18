# 閉包（Closures）
-----------------

> 1.0
> 翻譯：[wh1100717](https://github.com/wh1100717)
> 校對：[lyuka](https://github.com/lyuka)

> 2.0
> 翻譯+校對：[100mango](https://github.com/100mango)

> 2.1
> 翻譯：[100mango](https://github.com/100mango), [magicdict](https://github.com/magicdict)
> 校對：[shanks](http://codebuild.me)
>
> 2.2
> 翻譯+校對：[SketchK](https://github.com/SketchK) 2016-05-12
>
> 3.0
> 翻譯：[Lanford](https://github.com/LanfordCai) 2016-09-19   
> 3.0.1，shanks，2016-11-12

本頁包含內容：

- [閉包表達式](#closure_expressions)
- [尾隨閉包](#trailing_closures)
- [值捕獲](#capturing_values)
- [閉包是引用類型](#closures_are_reference_types)
- [逃逸閉包](#escaping_closures)
- [自動閉包](#autoclosures)

*閉包*是自包含的函數代碼塊，可以在代碼中被傳遞和使用。Swift 中的閉包與 C 和 Objective-C 中的代碼塊（blocks）以及其他一些編程語言中的匿名函數比較相似。

閉包可以捕獲和存儲其所在上下文中任意常量和變量的引用。被稱為*包裹*常量和變量。 Swift 會為你管理在捕獲過程中涉及到的所有內存操作。

> 注意
> 如果你不熟悉捕獲（capturing）這個概念也不用擔心，你可以在[值捕獲](#capturing_values)章節對其進行詳細了解。

在[函數](./06_Functions.html)章節中介紹的全局和嵌套函數實際上也是特殊的閉包，閉包采取如下三種形式之一：

* 全局函數是一個有名字但不會捕獲任何值的閉包
* 嵌套函數是一個有名字並可以捕獲其封閉函數域內值的閉包
* 閉包表達式是一個利用輕量級語法所寫的可以捕獲其上下文中變量或常量值的匿名閉包

Swift 的閉包表達式擁有簡潔的風格，並鼓勵在常見場景中進行語法優化，主要優化如下：

* 利用上下文推斷參數和返回值類型
* 隱式返回單表達式閉包，即單表達式閉包可以省略 `return` 關鍵字
* 參數名稱縮寫
* 尾隨閉包語法

<a name="closure_expressions"></a>
## 閉包表達式


[嵌套函數](./06_Functions.html#nested_function)是一個在較復雜函數中方便進行命名和定義自包含代碼模塊的方式。當然，有時候編寫小巧的沒有完整定義和命名的類函數結構也是很有用處的，尤其是在你處理一些函數並需要將另外一些函數作為該函數的參數時。

*閉包表達式*是一種利用簡潔語法構建內聯閉包的方式。閉包表達式提供了一些語法優化，使得撰寫閉包變得簡單明了。下面閉包表達式的例子通過使用幾次迭代展示了 `sorted(by:)` 方法定義和語法優化的方式。每一次迭代都用更簡潔的方式描述了相同的功能。

<a name="the_sorted_function"></a>
### sorted 方法

Swift 標准庫提供了名為 `sorted(by:)` 的方法，它會根據你所提供的用於排序的閉包函數將已知類型數組中的值進行排序。一旦排序完成，`sorted(by:)` 方法會返回一個與原數組大小相同，包含同類型元素且元素已正確排序的新數組。原數組不會被 `sorted(by:)` 方法修改。

下面的閉包表達式示例使用 `sorted(by:)` 方法對一個 `String` 類型的數組進行字母逆序排序。以下是初始數組：

```swift
let names = ["Chris", "Alex", "Ewa", "Barry", "Daniella"]
```

`sorted(by:)` 方法接受一個閉包，該閉包函數需要傳入與數組元素類型相同的兩個值，並返回一個布爾類型值來表明當排序結束後傳入的第一個參數排在第二個參數前面還是後面。如果第一個參數值出現在第二個參數值*前面*，排序閉包函數需要返回`true`，反之返回`false`。

該例子對一個 `String` 類型的數組進行排序，因此排序閉包函數類型需為 `(String, String) -> Bool`。

提供排序閉包函數的一種方式是撰寫一個符合其類型要求的普通函數，並將其作為 `sorted(by:)` 方法的參數傳入：

```swift
func backward(_ s1: String, _ s2: String) -> Bool {
    return s1 > s2
}
var reversedNames = names.sorted(by: backward)
// reversedNames 為 ["Ewa", "Daniella", "Chris", "Barry", "Alex"]
```

如果第一個字符串（`s1`）大於第二個字符串（`s2`），`backward(_:_:)` 函數會返回 `true`，表示在新的數組中 `s1` 應該出現在 `s2` 前。對於字符串中的字符來說，「大於」表示「按照字母順序較晚出現」。這意味著字母 `"B"` 大於字母 `"A"` ，字符串 `"Tom"` 大於字符串 `"Tim"`。該閉包將進行字母逆序排序，`"Barry"` 將會排在 `"Alex"` 之前。

然而，以這種方式來編寫一個實際上很簡單的表達式（`a > b`)，確實太過繁瑣了。對於這個例子來說，利用閉包表達式語法可以更好地構造一個內聯排序閉包。

<a name="closure_expression_syntax"></a>
### 閉包表達式語法

閉包表達式語法有如下的一般形式：

```swift
{ (parameters) -> returnType in
    statements
}
```

*閉包表達式參數* 可以是 in-out 參數，但不能設定默認值。也可以使用具名的可變參數（譯者注：但是如果可變參數不放在參數列表的最後一位的話，調用閉包的時時編譯器將報錯。可參考[這裡](http://stackoverflow.com/questions/39548852/swift-3-0-closure-expression-what-if-the-variadic-parameters-not-at-the-last-pl)）。元組也可以作為參數和返回值。

下面的例子展示了之前 `backward(_:_:)` 函數對應的閉包表達式版本的代碼：

```swift
reversedNames = names.sorted(by: { (s1: String, s2: String) -> Bool in
    return s1 > s2
})
```

需要注意的是內聯閉包參數和返回值類型聲明與 `backward(_:_:)` 函數類型聲明相同。在這兩種方式中，都寫成了 `(s1: String, s2: String) -> Bool`。然而在內聯閉包表達式中，函數和返回值類型都寫在*大括號內*，而不是大括號外。

閉包的函數體部分由關鍵字`in`引入。該關鍵字表示閉包的參數和返回值類型定義已經完成，閉包函數體即將開始。

由於這個閉包的函數體部分如此短，以至於可以將其改寫成一行代碼：

```swift
reversedNames = names.sorted(by: { (s1: String, s2: String) -> Bool in return s1 > s2 } )
```

該例中 `sorted(by:)` 方法的整體調用保持不變，一對圓括號仍然包裹住了方法的整個參數。然而，參數現在變成了內聯閉包。

<a name="inferring_type_from_context"></a>
### 根據上下文推斷類型

因為排序閉包函數是作為 `sorted(by:)` 方法的參數傳入的，Swift 可以推斷其參數和返回值的類型。`sorted(by:)` 方法被一個字符串數組調用，因此其參數必須是 `(String, String) -> Bool` 類型的函數。這意味著 `(String, String)` 和 `Bool` 類型並不需要作為閉包表達式定義的一部分。因為所有的類型都可以被正確推斷，返回箭頭（`->`）和圍繞在參數周圍的括號也可以被省略：

```swift
reversedNames = names.sorted(by: { s1, s2 in return s1 > s2 } )
```

實際上，通過內聯閉包表達式構造的閉包作為參數傳遞給函數或方法時，總是能夠推斷出閉包的參數和返回值類型。這意味著閉包作為函數或者方法的參數時，你幾乎不需要利用完整格式構造內聯閉包。

盡管如此，你仍然可以明確寫出有著完整格式的閉包。如果完整格式的閉包能夠提高代碼的可讀性，則我們更鼓勵采用完整格式的閉包。而在 `sorted(by:)` 方法這個例子裡，顯然閉包的目的就是排序。由於這個閉包是為了處理字符串數組的排序，因此讀者能夠推測出這個閉包是用於字符串處理的。

<a name="implicit_returns_from_single_expression_closures"></a>
### 單表達式閉包隱式返回
單行表達式閉包可以通過省略 `return` 關鍵字來隱式返回單行表達式的結果，如上版本的例子可以改寫為：

```swift
reversedNames = names.sorted(by: { s1, s2 in s1 > s2 } )
```

在這個例子中，`sorted(by:)` 方法的參數類型明確了閉包必須返回一個 `Bool` 類型值。因為閉包函數體只包含了一個單一表達式（`s1 > s2`），該表達式返回 `Bool` 類型值，因此這裡沒有歧義，`return` 關鍵字可以省略。

<a name="shorthand_argument_names"></a>
### 參數名稱縮寫

Swift 自動為內聯閉包提供了參數名稱縮寫功能，你可以直接通過 `$0`，`$1`，`$2` 來順序調用閉包的參數，以此類推。

如果你在閉包表達式中使用參數名稱縮寫，你可以在閉包定義中省略參數列表，並且對應參數名稱縮寫的類型會通過函數類型進行推斷。`in`關鍵字也同樣可以被省略，因為此時閉包表達式完全由閉包函數體構成：

```swift
reversedNames = names.sorted(by: { $0 > $1 } )
```

在這個例子中，`$0`和`$1`表示閉包中第一個和第二個 `String` 類型的參數。

<a name="operator_methods"></a>
### 運算符方法

實際上還有一種更簡短的方式來編寫上面例子中的閉包表達式。Swift 的 `String` 類型定義了關於大於號（`>`）的字符串實現，其作為一個函數接受兩個 `String` 類型的參數並返回 `Bool` 類型的值。而這正好與 `sorted(by:)` 方法的參數需要的函數類型相符合。因此，你可以簡單地傳遞一個大於號，Swift 可以自動推斷出你想使用大於號的字符串函數實現：

```swift
reversedNames = names.sorted(by: >)
```

更多關於運算符方法的內容請查看[運算符方法](./25_Advanced_Operators.html#operator_methods)。

<a name="trailing_closures"></a>
## 尾隨閉包

如果你需要將一個很長的閉包表達式作為最後一個參數傳遞給函數，可以使用*尾隨閉包*來增強函數的可讀性。尾隨閉包是一個書寫在函數括號之後的閉包表達式，函數支持將其作為最後一個參數調用。在使用尾隨閉包時，你不用寫出它的參數標簽：

```swift
func someFunctionThatTakesAClosure(closure: () -> Void) {
    // 函數體部分
}

// 以下是不使用尾隨閉包進行函數調用
someFunctionThatTakesAClosure(closure: {
    // 閉包主體部分
})

// 以下是使用尾隨閉包進行函數調用
someFunctionThatTakesAClosure() {
    // 閉包主體部分
}
```

在[閉包表達式語法](#closure_expression_syntax)一節中作為 `sorted(by:)` 方法參數的字符串排序閉包可以改寫為：

```swift
reversedNames = names.sorted() { $0 > $1 }
```

如果閉包表達式是函數或方法的唯一參數，則當你使用尾隨閉包時，你甚至可以把 `()` 省略掉：

```swift
reversedNames = names.sorted { $0 > $1 }
```

當閉包非常長以至於不能在一行中進行書寫時，尾隨閉包變得非常有用。舉例來說，Swift 的 `Array` 類型有一個 `map(_:)` 方法，這個方法獲取一個閉包表達式作為其唯一參數。該閉包函數會為數組中的每一個元素調用一次，並返回該元素所映射的值。具體的映射方式和返回值類型由閉包來指定。

當提供給數組的閉包應用於每個數組元素後，`map(_:)` 方法將返回一個新的數組，數組中包含了與原數組中的元素一一對應的映射後的值。

下例介紹了如何在 `map(_:)` 方法中使用尾隨閉包將 `Int` 類型數組 `[16, 58, 510]` 轉換為包含對應 `String` 類型的值的數組`["OneSix", "FiveEight", "FiveOneZero"]`：

```swift
let digitNames = [
    0: "Zero", 1: "One", 2: "Two",   3: "Three", 4: "Four",
    5: "Five", 6: "Six", 7: "Seven", 8: "Eight", 9: "Nine"
]
let numbers = [16, 58, 510]
```

如上代碼創建了一個整型數位和它們英文版本名字相映射的字典。同時還定義了一個准備轉換為字符串數組的整型數組。

你現在可以通過傳遞一個尾隨閉包給 `numbers` 數組的 `map(_:)` 方法來創建對應的字符串版本數組：

```swift
let strings = numbers.map {
    (number) -> String in
    var number = number
    var output = ""
    repeat {
        output = digitNames[number % 10]! + output
        number /= 10
    } while number > 0
    return output
}
// strings 常量被推斷為字符串類型數組，即 [String]
// 其值為 ["OneSix", "FiveEight", "FiveOneZero"]
```

`map(_:)` 為數組中每一個元素調用了一次閉包表達式。你不需要指定閉包的輸入參數 `number` 的類型，因為可以通過要映射的數組類型進行推斷。

在該例中，局部變量 `number` 的值由閉包中的 `number` 參數獲得，因此可以在閉包函數體內對其進行修改，(閉包或者函數的參數總是常量)，閉包表達式指定了返回類型為 `String`，以表明存儲映射值的新數組類型為 `String`。

閉包表達式在每次被調用的時候創建了一個叫做 `output` 的字符串並返回。其使用求余運算符（`number % 10`）計算最後一位數字並利用 `digitNames` 字典獲取所映射的字符串。這個閉包能夠用於創建任意正整數的字符串表示。

> 注意：  
> 字典 `digitNames` 下標後跟著一個嘆號（`!`），因為字典下標返回一個可選值（optional value），表明該鍵不存在時會查找失敗。在上例中，由於可以確定 `number % 10` 總是 `digitNames` 字典的有效下標，因此嘆號可以用於強制解包 (force-unwrap) 存儲在下標的可選類型的返回值中的`String`類型的值。

從 `digitNames` 字典中獲取的字符串被添加到 `output` 的*前部*，逆序建立了一個字符串版本的數字。（在表達式 `number % 10` 中，如果 `number` 為 `16`，則返回 `6`，`58` 返回 `8`，`510` 返回 `0`。）

`number` 變量之後除以 `10`。因為其是整數，在計算過程中未除盡部分被忽略。因此 `16` 變成了 `1`，`58` 變成了 `5`，`510` 變成了 `51`。

整個過程重復進行，直到 `number /= 10` 為 `0`，這時閉包會將字符串 `output` 返回，而 `map(_:)` 方法則會將字符串添加到映射數組中。

在上面的例子中，通過尾隨閉包語法，優雅地在函數後封裝了閉包的具體功能，而不再需要將整個閉包包裹在 `map(_:)` 方法的括號內。

<a name="capturing_values"></a>
## 值捕獲

閉包可以在其被定義的上下文中*捕獲*常量或變量。即使定義這些常量和變量的原作用域已經不存在，閉包仍然可以在閉包函數體內引用和修改這些值。

Swift 中，可以捕獲值的閉包的最簡單形式是嵌套函數，也就是定義在其他函數的函數體內的函數。嵌套函數可以捕獲其外部函數所有的參數以及定義的常量和變量。

舉個例子，這有一個叫做 `makeIncrementer` 的函數，其包含了一個叫做 `incrementer` 的嵌套函數。嵌套函數 `incrementer()` 從上下文中捕獲了兩個值，`runningTotal` 和 `amount`。捕獲這些值之後，`makeIncrementer` 將 `incrementer` 作為閉包返回。每次調用 `incrementer` 時，其會以 `amount` 作為增量增加 `runningTotal` 的值。

```swift
func makeIncrementer(forIncrement amount: Int) -> () -> Int {
    var runningTotal = 0
    func incrementer() -> Int {
        runningTotal += amount
        return runningTotal
    }
    return incrementer
}
```

`makeIncrementer` 返回類型為 `() -> Int`。這意味著其返回的是一個*函數*，而非一個簡單類型的值。該函數在每次調用時不接受參數，只返回一個 `Int` 類型的值。關於函數返回其他函數的內容，請查看[函數類型作為返回類型](./06_Functions.html#function_types_as_return_types)。

`makeIncrementer(forIncrement:)` 函數定義了一個初始值為 `0` 的整型變量 `runningTotal`，用來存儲當前總計數值。該值為 `incrementer` 的返回值。

`makeIncrementer(forIncrement:)` 有一個 `Int` 類型的參數，其外部參數名為 `forIncrement`，內部參數名為 `amount`，該參數表示每次 `incrementer` 被調用時 `runningTotal` 將要增加的量。`makeIncrementer` 函數還定義了一個嵌套函數 `incrementer`，用來執行實際的增加操作。該函數簡單地使 `runningTotal` 增加 `amount`，並將其返回。

如果我們單獨考慮嵌套函數 `incrementer()`，會發現它有些不同尋常：

```swift
func incrementer() -> Int {
    runningTotal += amount
    return runningTotal
}
```

`incrementer()` 函數並沒有任何參數，但是在函數體內訪問了 `runningTotal` 和 `amount` 變量。這是因為它從外圍函數捕獲了 `runningTotal` 和 `amount` 變量的*引用*。捕獲引用保證了 `runningTotal` 和 `amount` 變量在調用完 `makeIncrementer` 後不會消失，並且保證了在下一次執行 `incrementer` 函數時，`runningTotal` 依舊存在。

> 注意
> 為了優化，如果一個值不會被閉包改變，或者在閉包創建後不會改變，Swift 可能會改為捕獲並保存一份對值的拷貝。
> Swift 也會負責被捕獲變量的所有內存管理工作，包括釋放不再需要的變量。

下面是一個使用 `makeIncrementer` 的例子：

```swift
let incrementByTen = makeIncrementer(forIncrement: 10)
```

該例子定義了一個叫做 `incrementByTen` 的常量，該常量指向一個每次調用會將其 `runningTotal` 變量增加 `10` 的 `incrementer` 函數。調用這個函數多次可以得到以下結果：

```swift
incrementByTen()
// 返回的值為10
incrementByTen()
// 返回的值為20
incrementByTen()
// 返回的值為30
```

如果你創建了另一個 `incrementer`，它會有屬於自己的引用，指向一個全新、獨立的 `runningTotal` 變量：

```swift
let incrementBySeven = makeIncrementer(forIncrement: 7)
incrementBySeven()
// 返回的值為7
```

再次調用原來的 `incrementByTen` 會繼續增加它自己的 `runningTotal` 變量，該變量和 `incrementBySeven` 中捕獲的變量沒有任何聯系：

```swift
incrementByTen()
// 返回的值為40
```

> 注意：   
> 如果你將閉包賦值給一個類實例的屬性，並且該閉包通過訪問該實例或其成員而捕獲了該實例，你將在閉包和該實例間創建一個循環強引用。Swift 使用捕獲列表來打破這種循環強引用。更多信息，請參考[閉包引起的循環強引用](./16_Automatic_Reference_Counting.html#strong_reference_cycles_for_closures)。

<a name="closures_are_reference_types"></a>
## 閉包是引用類型

上面的例子中，`incrementBySeven` 和 `incrementByTen` 都是常量，但是這些常量指向的閉包仍然可以增加其捕獲的變量的值。這是因為函數和閉包都是*引用類型*。

無論你將函數或閉包賦值給一個常量還是變量，你實際上都是將常量或變量的值設置為對應函數或閉包的*引用*。上面的例子中，指向閉包的引用 `incrementByTen` 是一個常量，而並非閉包內容本身。

這也意味著如果你將閉包賦值給了兩個不同的常量或變量，兩個值都會指向同一個閉包：

```swift
let alsoIncrementByTen = incrementByTen
alsoIncrementByTen()
// 返回的值為50
```

<a name="escaping_closures"></a>
## 逃逸閉包

當一個閉包作為參數傳到一個函數中，但是這個閉包在函數返回之後才被執行，我們稱該閉包從函數中*逃逸*。當你定義接受閉包作為參數的函數時，你可以在參數名之前標注 `@escaping`，用來指明這個閉包是允許「逃逸」出這個函數的。  

一種能使閉包「逃逸」出函數的方法是，將這個閉包保存在一個函數外部定義的變量中。舉個例子，很多啟動異步操作的函數接受一個閉包參數作為 completion handler。這類函數會在異步操作開始之後立刻返回，但是閉包直到異步操作結束後才會被調用。在這種情況下，閉包需要「逃逸」出函數，因為閉包需要在函數返回之後被調用。例如：

```swift
var completionHandlers: [() -> Void] = []
func someFunctionWithEscapingClosure(completionHandler: @escaping () -> Void) {
    completionHandlers.append(completionHandler)
}
```

`someFunctionWithEscapingClosure(_:)` 函數接受一個閉包作為參數，該閉包被添加到一個函數外定義的數組中。如果你不將這個參數標記為 `@escaping`，就會得到一個編譯錯誤。

將一個閉包標記為 `@escaping` 意味著你必須在閉包中顯式地引用 `self`。比如說，在下面的代碼中，傳遞到 `someFunctionWithEscapingClosure(_:)` 中的閉包是一個逃逸閉包，這意味著它需要顯式地引用 `self`。相對的，傳遞到 `someFunctionWithNonescapingClosure(_:)` 中的閉包是一個非逃逸閉包，這意味著它可以隱式引用 `self`。


```swift
func someFunctionWithNonescapingClosure(closure: () -> Void) {
    closure()
}

class SomeClass {
    var x = 10
    func doSomething() {
        someFunctionWithEscapingClosure { self.x = 100 }
        someFunctionWithNonescapingClosure { x = 200 }
    }
}

let instance = SomeClass()
instance.doSomething()
print(instance.x)
// 打印出 "200"

completionHandlers.first?()
print(instance.x)
// 打印出 "100"
```

<a name="autoclosures"></a>
## 自動閉包

*自動閉包*是一種自動創建的閉包，用於包裝傳遞給函數作為參數的表達式。這種閉包不接受任何參數，當它被調用的時候，會返回被包裝在其中的表達式的值。這種便利語法讓你能夠省略閉包的花括號，用一個普通的表達式來代替顯式的閉包。

我們經常會*調用*采用自動閉包的函數，但是很少去*實現*這樣的函數。舉個例子來說，`assert(condition:message:file:line:)` 函數接受自動閉包作為它的 `condition` 參數和 `message` 參數；它的 `condition` 參數僅會在 debug 模式下被求值，它的 `message` 參數僅當 `condition` 參數為 `false` 時被計算求值。

自動閉包讓你能夠延遲求值，因為直到你調用這個閉包，代碼段才會被執行。延遲求值對於那些有副作用（Side Effect）和高計算成本的代碼來說是很有益處的，因為它使得你能控制代碼的執行時機。下面的代碼展示了閉包如何延時求值。

```swift
var customersInLine = ["Chris", "Alex", "Ewa", "Barry", "Daniella"]
print(customersInLine.count)
// 打印出 "5"

let customerProvider = { customersInLine.remove(at: 0) }
print(customersInLine.count)
// 打印出 "5"

print("Now serving \(customerProvider())!")
// Prints "Now serving Chris!"
print(customersInLine.count)
// 打印出 "4"
```

盡管在閉包的代碼中，`customersInLine` 的第一個元素被移除了，不過在閉包被調用之前，這個元素是不會被移除的。如果這個閉包永遠不被調用，那麼在閉包裡面的表達式將永遠不會執行，那意味著列表中的元素永遠不會被移除。請注意，`customerProvider` 的類型不是 `String`，而是 `() -> String`，一個沒有參數且返回值為 `String` 的函數。

將閉包作為參數傳遞給函數時，你能獲得同樣的延時求值行為。

```swift
// customersInLine is ["Alex", "Ewa", "Barry", "Daniella"]
func serve(customer customerProvider: () -> String) {
    print("Now serving \(customerProvider())!")
}
serve(customer: { customersInLine.remove(at: 0) } )
// 打印出 "Now serving Alex!"
```

上面的 `serve(customer:)` 函數接受一個返回顧客名字的顯式的閉包。下面這個版本的 `serve(customer:)` 完成了相同的操作，不過它並沒有接受一個顯式的閉包，而是通過將參數標記為 `@autoclosure` 來接收一個自動閉包。現在你可以將該函數當作接受 `String` 類型參數（而非閉包）的函數來調用。`customerProvider` 參數將自動轉化為一個閉包，因為該參數被標記了 `@autoclosure` 特性。

```swift
// customersInLine is ["Ewa", "Barry", "Daniella"]
func serve(customer customerProvider: @autoclosure () -> String) {
    print("Now serving \(customerProvider())!")
}
serve(customer: customersInLine.remove(at: 0))
// 打印 "Now serving Ewa!"
```

> 注意
> 過度使用 `autoclosures` 會讓你的代碼變得難以理解。上下文和函數名應該能夠清晰地表明求值是被延遲執行的。

如果你想讓一個自動閉包可以「逃逸」，則應該同時使用 `@autoclosure` 和 `@escaping` 屬性。`@escaping` 屬性的講解見上面的[逃逸閉包](#escaping_closures)。

```swift
// customersInLine i= ["Barry", "Daniella"]
var customerProviders: [() -> String] = []
func collectCustomerProviders(_ customerProvider: @autoclosure @escaping () -> String) {
    customerProviders.append(customerProvider)
}
collectCustomerProviders(customersInLine.remove(at: 0))
collectCustomerProviders(customersInLine.remove(at: 0))

print("Collected \(customerProviders.count) closures.")
// 打印 "Collected 2 closures."
for customerProvider in customerProviders {
    print("Now serving \(customerProvider())!")
}
// 打印 "Now serving Barry!"
// 打印 "Now serving Daniella!"
```

在上面的代碼中，`collectCustomerProviders(_:)` 函數並沒有調用傳入的 `customerProvider` 閉包，而是將閉包追加到了 `customerProviders` 數組中。這個數組定義在函數作用域范圍外，這意味著數組內的閉包能夠在函數返回之後被調用。因此，`customerProvider` 參數必須允許「逃逸」出函數作用域。

