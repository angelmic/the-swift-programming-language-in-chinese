# 函數（Functions）
-----------------

> 1.0
> 翻譯：[honghaoz](https://github.com/honghaoz)
> 校對：[LunaticM](https://github.com/LunaticM)

> 2.0
> 翻譯+校對：[dreamkidd](https://github.com/dreamkidd)

> 2.1
> 翻譯：[DianQK](https://github.com/DianQK)
> 定稿：[shanks](http://codebuild.me)

> 2.2
> 翻譯+校對：[SketchK](https://github.com/SketchK) 2016-05-12

> 3.0
> 翻譯: [crayygy](https://github.com/crayygy) 2016-09-12
> 校對: [shanks](http://codebuild.me) 2016-09-27
> 3.0.1，shanks，2016-11-12

本頁包含內容：
- [函數定義與調用](#Defining_and_Calling_Functions)
- [函數參數與返回值](#Function_Parameters_and_Return_Values)
- [函數參數標簽和參數名稱](#Function_Argument_Labels_and_Parameter_Names)
- [函數類型](#Function_Types)
- [嵌套函數](#Nested_Functions)


*函數*是一段完成特定任務的獨立代碼片段。你可以通過給函數命名來標識某個函數的功能，這個名字可以被用來在需要的時候"調用"這個函數來完成它的任務。   

Swift 統一的函數語法非常的靈活，可以用來表示任何函數，包括從最簡單的沒有參數名字的 C 風格函數，到復雜的帶局部和外部參數名的 Objective-C 風格函數。參數可以提供默認值，以簡化函數調用。參數也可以既當做傳入參數，也當做傳出參數，也就是說，一旦函數執行結束，傳入的參數值將被修改。

在 Swift 中，每個函數都有一個由函數的參數值類型和返回值類型組成的類型。你可以把函數類型當做任何其他普通變量類型一樣處理，這樣就可以更簡單地把函數當做別的函數的參數，也可以從其他函數中返回函數。函數的定義可以寫在其他函數定義中，這樣可以在嵌套函數范圍內實現功能封裝。

<a name="Defining_and_Calling_Functions"></a>
## 函數的定義與調用
當你定義一個函數時，你可以定義一個或多個有名字和類型的值，作為函數的輸入，稱為*參數*，也可以定義某種類型的值作為函數執行結束時的輸出，稱為*返回類型*。

每個函數有個*函數名*，用來描述函數執行的任務。要使用一個函數時，用函數名來「調用」這個函數，並傳給它匹配的輸入值（稱作 *實參* ）。函數的實參必須與函數參數表裡參數的順序一致。


下面例子中的函數的名字是`greet(person:)`，之所以叫這個名字,是因為這個函數用一個人的名字當做輸入，並返回向這個人問候的語句。為了完成這個任務，你需要定義一個輸入參數——一個叫做 `person` 的 `String` 值，和一個包含給這個人問候語的 `String` 類型的返回值：


```swift
func greet(person: String) -> String {
    let greeting = "Hello, " + person + "!"
    return greeting
}
```

所有的這些信息匯總起來成為函數的*定義*，並以 `func` 作為前綴。指定函數返回類型時，用返回箭頭 `->`（一個連字符後跟一個右尖括號）後跟返回類型的名稱的方式來表示。

該定義描述了函數的功能，它期望接收什麼作為參數和執行結束時它返回的結果是什麼類型。這樣的定義使得函數可以在別的地方以一種清晰的方式被調用：

```swift
print(greet(person: "Anna"))
// 打印 "Hello, Anna!"
print(greet(person: "Brian"))
// 打印 "Hello, Brian!"
```

調用 `greet(person:)` 函數時，在圓括號中傳給它一個 `String` 類型的實參，例如 `greet(person: "Anna")`。正如上面所示，因為這個函數返回一個 `String` 類型的值，所以`greet ` 可以被包含在 `print(_:separator:terminator:)` 的調用中，用來輸出這個函數的返回值。

>注意   
`print(_:separator:terminator:)` 函數的第一個參數並沒有設置一個標簽，而其他的參數因為已經有了默認值，因此是可選的。關於這些函數語法上的變化詳見下方關於 函數參數標簽和參數名 以及 默認參數值。


在 `greet(person:)` 的函數體中，先定義了一個新的名為 `greeting` 的 `String` 常量，同時，把對 `personName` 的問候消息賦值給了 `greeting` 。然後用 `return` 關鍵字把這個問候返回出去。一旦 `return greeting` 被調用，該函數結束它的執行並返回 `greeting` 的當前值。

你可以用不同的輸入值多次調用 `greet(person:)`。上面的例子展示的是用`"Anna"`和`"Brian"`調用的結果，該函數分別返回了不同的結果。

為了簡化這個函數的定義，可以將問候消息的創建和返回寫成一句：

```swift
func greetAgain(person: String) -> String {
    return "Hello again, " + person + "!"
}
print(greetAgain(person: "Anna"))
// 打印 "Hello again, Anna!"
```

<a name="Function_Parameters_and_Return_Values"></a>
## 函數參數與返回值
函數參數與返回值在 Swift 中非常的靈活。你可以定義任何類型的函數，包括從只帶一個未名參數的簡單函數到復雜的帶有表達性參數名和不同參數選項的復雜函數。

<a name="functions_without_parameters"></a>
### 無參數函數

函數可以沒有參數。下面這個函數就是一個無參數函數，當被調用時，它返回固定的 `String` 消息：

```swift
func sayHelloWorld() -> String {
    return "hello, world"
}
print(sayHelloWorld())
// 打印 "hello, world"
```

盡管這個函數沒有參數，但是定義中在函數名後還是需要一對圓括號。當被調用時，也需要在函數名後寫一對圓括號。

<a name="functions_with_multiple_parameters"></a>
### 多參數函數

函數可以有多種輸入參數，這些參數被包含在函數的括號之中，以逗號分隔。

下面這個函數用一個人名和是否已經打過招呼作為輸入，並返回對這個人的適當問候語:

```swift
func greet(person: String, alreadyGreeted: Bool) -> String {
    if alreadyGreeted {
        return greetAgain(person: person)
    } else {
        return greet(person: person)
    }
}
print(greet(person: "Tim", alreadyGreeted: true))
// 打印 "Hello again, Tim!"
```

你可以通過在括號內使用逗號分隔來傳遞一個`String`參數值和一個標識為`alreadyGreeted`的`Bool`值，來調用`greet(person:alreadyGreeted:)`函數。注意這個函數和上面`greet(person:)`是不同的。雖然它們都有著同樣的名字`greet`，但是`greet(person:alreadyGreeted:)`函數需要兩個參數，而`greet(person:)`只需要一個參數。

<a name="functions_without_return_values"></a>
### 無返回值函數

函數可以沒有返回值。下面是 `greet(person:)` 函數的另一個版本，這個函數直接打印一個`String`值，而不是返回它：

```swift
func greet(person: String) {
    print("Hello, \(person)!")
}
greet(person: "Dave")
// 打印 "Hello, Dave!"
```

因為這個函數不需要返回值，所以這個函數的定義中沒有返回箭頭（->）和返回類型。

>注意  
嚴格上來說，雖然沒有返回值被定義，`greet(person:)` 函數依然返回了值。沒有定義返回類型的函數會返回一個特殊的`Void`值。它其實是一個空的元組（tuple），沒有任何元素，可以寫成()。


被調用時，一個函數的返回值可以被忽略：

```swift
func printAndCount(string: String) -> Int {
    print(string)
    return string.characters.count
}
func printWithoutCounting(string: String) {
    let _ = printAndCount(string: string)
}
printAndCount(string: "hello, world")
// 打印 "hello, world" 並且返回值 12
printWithoutCounting(string: "hello, world")
// 打印 "hello, world" 但是沒有返回任何值
```

第一個函數 `printAndCount(string:)`，輸出一個字符串並返回 `Int` 類型的字符數。第二個函數 `printWithoutCounting(string:)`調用了第一個函數，但是忽略了它的返回值。當第二個函數被調用時，消息依然會由第一個函數輸出，但是返回值不會被用到。

>注意:  
返回值可以被忽略，但定義了有返回值的函數必須返回一個值，如果在函數定義底部沒有返回任何值，將導致編譯時錯誤（compile-time error）。


<a name="functions_with_multiple_return_values"></a>
### 多重返回值函數
你可以用元組（tuple）類型讓多個值作為一個復合值從函數中返回。

下例中定義了一個名為 `minMax(array:)` 的函數，作用是在一個 `Int` 類型的數組中找出最小值與最大值。

```swift
func minMax(array: [Int]) -> (min: Int, max: Int) {
    var currentMin = array[0]
    var currentMax = array[0]
    for value in array[1..<array.count] {
        if value < currentMin {
            currentMin = value
        } else if value > currentMax {
            currentMax = value
        }
    }
    return (currentMin, currentMax)
}
```

 `minMax(array:)` 函數返回一個包含兩個 `Int` 值的元組，這些值被標記為 `min` 和 `max` ，以便查詢函數的返回值時可以通過名字訪問它們。

在 `minMax(array:)` 的函數體中，在開始的時候設置兩個工作變量 `currentMin` 和 `currentMax` 的值為數組中的第一個數。然後函數會遍歷數組中剩余的值並檢查該值是否比 `currentMin` 和 `currentMax` 更小或更大。最後數組中的最小值與最大值作為一個包含兩個 `Int` 值的元組返回。

因為元組的成員值已被命名，因此可以通過 `.` 語法來檢索找到的最小值與最大值：

```swift
let bounds = minMax(array: [8, -6, 2, 109, 3, 71])
print("min is \(bounds.min) and max is \(bounds.max)")
// 打印 "min is -6 and max is 109"
```

需要注意的是，元組的成員不需要在元組從函數中返回時命名，因為它們的名字已經在函數返回類型中指定了。

<a name="optional_tuple_return_types"></a>
### 可選元組返回類型

如果函數返回的元組類型有可能整個元組都「沒有值」，你可以使用可選的（ `optional` ） 元組返回類型反映整個元組可以是`nil`的事實。你可以通過在元組類型的右括號後放置一個問號來定義一個可選元組，例如 `(Int, Int)?` 或 `(String, Int, Bool)?`

>注意
可選元組類型如 `(Int, Int)?` 與元組包含可選類型如 `(Int?, Int?)` 是不同的.可選的元組類型，整個元組是可選的，而不只是元組中的每個元素值。


前面的 `minMax(array:)` 函數返回了一個包含兩個 `Int` 值的元組。但是函數不會對傳入的數組執行任何安全檢查，如果 `array` 參數是一個空數組，如上定義的 `minMax(array:)` 在試圖訪問 `array[0]` 時會觸發一個運行時錯誤(runtime error)。

為了安全地處理這個「空數組」問題，將 `minMax(array:)` 函數改寫為使用可選元組返回類型，並且當數組為空時返回 `nil`：


```swift
func minMax(array: [Int]) -> (min: Int, max: Int)? {
    if array.isEmpty { return nil }
    var currentMin = array[0]
    var currentMax = array[0]
    for value in array[1..<array.count] {
        if value < currentMin {
            currentMin = value
        } else if value > currentMax {
            currentMax = value
        }
    }
    return (currentMin, currentMax)
}
```

你可以使用可選綁定來檢查 `minMax(array:)` 函數返回的是一個存在的元組值還是 `nil`：

```swift
if let bounds = minMax(array: [8, -6, 2, 109, 3, 71]) {
    print("min is \(bounds.min) and max is \(bounds.max)")
}
// 打印 "min is -6 and max is 109"
```


<a name="Function_Argument_Labels_and_Parameter_Names"></a>
## 函數參數標簽和參數名稱

每個函數參數都有一個*參數標簽( argument label )*以及一個*參數名稱( parameter name )*。參數標簽在調用函數的時候使用；調用的時候需要將函數的參數標簽寫在對應的參數前面。參數名稱在函數的實現中使用。默認情況下，函數參數使用參數名稱來作為它們的參數標簽。

```swift
func someFunction(firstParameterName: Int, secondParameterName: Int) {
    // 在函數體內，firstParameterName 和 secondParameterName 代表參數中的第一個和第二個參數值
}
someFunction(firstParameterName: 1, secondParameterName: 2)
```

所有的參數都必須有一個獨一無二的名字。雖然多個參數擁有同樣的參數標簽是可能的，但是一個唯一的函數標簽能夠使你的代碼更具可讀性。

<a name="specifying_argument_labels"></a>
### 指定參數標簽

你可以在參數名稱前指定它的參數標簽，中間以空格分隔：

```swift
func someFunction(argumentLabel parameterName: Int) {
    // 在函數體內，parameterName 代表參數值
}
```

這個版本的 `greet(person:)` 函數，接收一個人的名字和他的家鄉，並且返回一句問候：

```swift
func greet(person: String, from hometown: String) -> String {
    return "Hello \(person)!  Glad you could visit from \(hometown)."
}
print(greet(person: "Bill", from: "Cupertino"))
// 打印 "Hello Bill!  Glad you could visit from Cupertino."
```

參數標簽的使用能夠讓一個函數在調用時更有表達力，更類似自然語言，並且仍保持了函數內部的可讀性以及清晰的意圖。

<a name="omitting_argument_labels"></a>
### 忽略參數標簽

如果你不希望為某個參數添加一個標簽，可以使用一個下劃線(`_`)來代替一個明確的參數標簽。

```swift
func someFunction(_ firstParameterName: Int, secondParameterName: Int) {
     // 在函數體內，firstParameterName 和 secondParameterName 代表參數中的第一個和第二個參數值
}
someFunction(1, secondParameterName: 2)
```

如果一個參數有一個標簽，那麼在調用的時候必須使用標簽來標記這個參數。

<a name="default_parameter_values"></a>
### 默認參數值

你可以在函數體中通過給參數賦值來為任意一個參數定義*默認值（Deafult Value）*。當默認值被定義後，調用這個函數時可以忽略這個參數。

```swift
func someFunction(parameterWithoutDefault: Int, parameterWithDefault: Int = 12) {
    // 如果你在調用時候不傳第二個參數，parameterWithDefault 會值為 12 傳入到函數體中。
}
someFunction(parameterWithoutDefault: 3, parameterWithDefault: 6) // parameterWithDefault = 6
someFunction(parameterWithoutDefault: 4) // parameterWithDefault = 12
```

將不帶有默認值的參數放在函數參數列表的最前。一般來說，沒有默認值的參數更加的重要，將不帶默認值的參數放在最前保證在函數調用時，非默認參數的順序是一致的，同時也使得相同的函數在不同情況下調用時顯得更為清晰。

<a name="variadic_parameters"></a>
### 可變參數

一個*可變參數（variadic parameter）*可以接受零個或多個值。函數調用時，你可以用可變參數來指定函數參數可以被傳入不確定數量的輸入值。通過在變量類型名後面加入（`...`）的方式來定義可變參數。

可變參數的傳入值在函數體中變為此類型的一個數組。例如，一個叫做 `numbers` 的 `Double...` 型可變參數，在函數體內可以當做一個叫 `numbers` 的 `[Double]` 型的數組常量。

下面的這個函數用來計算一組任意長度數字的 *算術平均數（arithmetic mean)*：

```swift
func arithmeticMean(_ numbers: Double...) -> Double {
    var total: Double = 0
    for number in numbers {
        total += number
    }
    return total / Double(numbers.count)
}
arithmeticMean(1, 2, 3, 4, 5)
// 返回 3.0, 是這 5 個數的平均數。
arithmeticMean(3, 8.25, 18.75)
// 返回 10.0, 是這 3 個數的平均數。
```

>注意：   
一個函數最多只能擁有一個可變參數。

<a name="in_out_parameters"></a>
### 輸入輸出參數

函數參數默認是常量。試圖在函數體中更改參數值將會導致編譯錯誤(compile-time error)。這意味著你不能錯誤地更改參數值。如果你想要一個函數可以修改參數的值，並且想要在這些修改在函數調用結束後仍然存在，那麼就應該把這個參數定義為*輸入輸出參數（In-Out Parameters）*。

定義一個輸入輸出參數時，在參數定義前加 `inout` 關鍵字。一個`輸入輸出參數`有傳入函數的值，這個值被函數修改，然後被傳出函數，替換原來的值。想獲取更多的關於輸入輸出參數的細節和相關的編譯器優化，請查看[輸入輸出參數](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Declarations.html#//apple_ref/doc/uid/TP40014097-CH34-ID545)一節。

你只能傳遞變量給輸入輸出參數。你不能傳入常量或者字面量，因為這些量是不能被修改的。當傳入的參數作為輸入輸出參數時，需要在參數名前加 `&` 符，表示這個值可以被函數修改。

>注意
輸入輸出參數不能有默認值，而且可變參數不能用 `inout` 標記。


下例中，`swapTwoInts(_:_:)` 函數有兩個分別叫做 `a` 和 `b` 的輸入輸出參數：

```swift
func swapTwoInts(_ a: inout Int, _ b: inout Int) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```

`swapTwoInts(_:_:)` 函數簡單地交換 `a` 與 `b` 的值。該函數先將 `a` 的值存到一個臨時常量 `temporaryA` 中，然後將 `b` 的值賦給 `a`，最後將 `temporaryA` 賦值給 `b`。

你可以用兩個 `Int` 型的變量來調用 `swapTwoInts(_:_:)`。需要注意的是，`someInt` 和 `anotherInt` 在傳入 `swapTwoInts(_:_:)` 函數前，都加了 `&` 的前綴：

```swift
var someInt = 3
var anotherInt = 107
swapTwoInts(&someInt, &anotherInt)
print("someInt is now \(someInt), and anotherInt is now \(anotherInt)")
// 打印 "someInt is now 107, and anotherInt is now 3"
```

從上面這個例子中，我們可以看到 `someInt` 和 `anotherInt` 的原始值在 `swapTwoInts(_:_:)` 函數中被修改，盡管它們的定義在函數體外。

>注意：  
輸入輸出參數和返回值是不一樣的。上面的 `swapTwoInts` 函數並沒有定義任何返回值，但仍然修改了 `someInt` 和 `anotherInt` 的值。輸入輸出參數是函數對函數體外產生影響的另一種方式。


<a name="Function_Types"></a>
## 函數類型

每個函數都有種特定的*函數類型*，函數的類型由函數的參數類型和返回類型組成。

例如：

```swift
func addTwoInts(_ a: Int, _ b: Int) -> Int {
    return a + b
}
func multiplyTwoInts(_ a: Int, _ b: Int) -> Int {
    return a * b
}
```

這個例子中定義了兩個簡單的數學函數：`addTwoInts` 和 `multiplyTwoInts`。這兩個函數都接受兩個 `Int` 值， 返回一個 `Int` 值。

這兩個函數的類型是 `(Int, Int) -> Int`，可以解讀為「這個函數類型有兩個 `Int` 型的參數並返回一個 `Int` 型的值。」。

下面是另一個例子，一個沒有參數，也沒有返回值的函數：

```swift
func printHelloWorld() {
    print("hello, world")
}
```

這個函數的類型是：`() -> Void`，或者叫「沒有參數，並返回 `Void` 類型的函數」。

<a name="using_function_types"></a>
### 使用函數類型

在 Swift 中，使用函數類型就像使用其他類型一樣。例如，你可以定義一個類型為函數的常量或變量，並將適當的函數賦值給它：

```swift
var mathFunction: (Int, Int) -> Int = addTwoInts
```

這段代碼可以被解讀為：

」定義一個叫做 `mathFunction` 的變量，類型是『一個有兩個 `Int` 型的參數並返回一個 `Int` 型的值的函數』，並讓這個新變量指向 `addTwoInts` 函數」。

`addTwoInts` 和 `mathFunction` 有同樣的類型，所以這個賦值過程在 Swift 類型檢查(type-check)中是允許的。

現在，你可以用 `mathFunction` 來調用被賦值的函數了：

```swift
print("Result: \(mathFunction(2, 3))")
// Prints "Result: 5"
```

有相同匹配類型的不同函數可以被賦值給同一個變量，就像非函數類型的變量一樣：


```swift
mathFunction = multiplyTwoInts
print("Result: \(mathFunction(2, 3))")
// Prints "Result: 6"
```

就像其他類型一樣，當賦值一個函數給常量或變量時，你可以讓 Swift 來推斷其函數類型：

```swift
let anotherMathFunction = addTwoInts
// anotherMathFunction 被推斷為 (Int, Int) -> Int 類型
```

<a name="function_types_as_parameter_types"></a>
### 函數類型作為參數類型

你可以用 `(Int, Int) -> Int` 這樣的函數類型作為另一個函數的參數類型。這樣你可以將函數的一部分實現留給函數的調用者來提供。

下面是另一個例子，正如上面的函數一樣，同樣是輸出某種數學運算結果：

```swift
func printMathResult(_ mathFunction: (Int, Int) -> Int, _ a: Int, _ b: Int) {
    print("Result: \(mathFunction(a, b))")
}
printMathResult(addTwoInts, 3, 5)
// 打印 "Result: 8"
```

這個例子定義了 `printMathResult(_:_:_:)` 函數，它有三個參數：第一個參數叫 `mathFunction`，類型是 `(Int, Int) -> Int`，你可以傳入任何這種類型的函數；第二個和第三個參數叫 `a` 和 `b`，它們的類型都是 `Int`，這兩個值作為已給出的函數的輸入值。

當 `printMathResult(_:_:_:)` 被調用時，它被傳入 `addTwoInts` 函數和整數 `3` 和 `5`。它用傳入 `3` 和 `5` 調用 `addTwoInts`，並輸出結果：`8`。

`printMathResult(_:_:_:)` 函數的作用就是輸出另一個適當類型的數學函數的調用結果。它不關心傳入函數是如何實現的，只關心傳入的函數是不是一個正確的類型。這使得 `printMathResult(_:_:_:)` 能以一種類型安全（type-safe）的方式將一部分功能轉給調用者實現。

<a name="function_types_as_return_types"></a>
### 函數類型作為返回類型

你可以用函數類型作為另一個函數的返回類型。你需要做的是在返回箭頭（->）後寫一個完整的函數類型。

下面的這個例子中定義了兩個簡單函數，分別是 `stepForward(_:)` 和 `stepBackward(_:)`。`stepForward(_:)`函數返回一個比輸入值大 `1` 的值。`stepBackward(_:)` 函數返回一個比輸入值小 `1` 的值。這兩個函數的類型都是 `(Int) -> Int`：

```swift
func stepForward(_ input: Int) -> Int {
    return input + 1
}
func stepBackward(_ input: Int) -> Int {
    return input - 1
}
```

如下名為 `chooseStepFunction(backward:)` 的函數，它的返回類型是 `(Int) -> Int` 類型的函數。`chooseStepFunction(backward:)` 根據布爾值 `backwards` 來返回 `stepForward(_:)` 函數或 `stepBackward(_:)` 函數：

```swift
func chooseStepFunction(backward: Bool) -> (Int) -> Int {
    return backward ? stepBackward : stepForward
}
```

你現在可以用 `chooseStepFunction(backward:)` 來獲得兩個函數其中的一個：

```swift
var currentValue = 3
let moveNearerToZero = chooseStepFunction(backward: currentValue > 0)
// moveNearerToZero 現在指向 stepBackward() 函數。
```

上面這個例子中計算出從 `currentValue` 逐漸接近到0是需要向正數走還是向負數走。`currentValue` 的初始值是 `3`，這意味著 `currentValue > 0` 為真（true），這將使得 `chooseStepFunction(_:)` 返回 `stepBackward(_:)` 函數。一個指向返回的函數的引用保存在了 `moveNearerToZero` 常量中。

現在，`moveNearerToZero`指向了正確的函數，它可以被用來數到零：


```swift
print("Counting to zero:")
// Counting to zero:
while currentValue != 0 {
    print("\(currentValue)... ")
    currentValue = moveNearerToZero(currentValue)
}
print("zero!")
// 3...
// 2...
// 1...
// zero!
```

<a name="Nested_Functions"></a>
## 嵌套函數

到目前為止本章中你所見到的所有函數都叫*全局函數（global functions）*，它們定義在全局域中。你也可以把函數定義在別的函數體中，稱作 *嵌套函數（nested functions）*。

默認情況下，嵌套函數是對外界不可見的，但是可以被它們的外圍函數（enclosing function）調用。一個外圍函數也可以返回它的某一個嵌套函數，使得這個函數可以在其他域中被使用。

你可以用返回嵌套函數的方式重寫 `chooseStepFunction(backward:)` 函數：

```swift
func chooseStepFunction(backward: Bool) -> (Int) -> Int {
    func stepForward(input: Int) -> Int { return input + 1 }
    func stepBackward(input: Int) -> Int { return input - 1 }
    return backward ? stepBackward : stepForward
}
var currentValue = -4
let moveNearerToZero = chooseStepFunction(backward: currentValue > 0)
// moveNearerToZero now refers to the nested stepForward() function
while currentValue != 0 {
    print("\(currentValue)... ")
    currentValue = moveNearerToZero(currentValue)
}
print("zero!")
// -4...
// -3...
// -2...
// -1...
// zero!
```
