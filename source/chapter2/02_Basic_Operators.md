# 基本運算符（Basic Operators）
-----------------

> 1.0
> 翻譯：[XieLingWang](https://github.com/xielingwang)
> 校對：[EvilCome](https://github.com/Evilcome)

> 2.0
> 翻譯+校對：[JackAlan](https://github.com/AlanMelody)

> 2.1
> 校對：[shanks](http://codebuild.me)

> 2.2
> 翻譯+校對：[Cee](https://github.com/Cee) 校對：[SketchK](https://github.com/SketchK)，2016-05-11    
> 3.0.1，shanks，2016-11-11

本頁包含內容：

- [術語](#terminology)
- [賦值運算符](#assignment_operator)
- [算術運算符](#arithmetic_operators)
- [組合賦值運算符](#compound_assignment_operators)
- [比較運算符](#comparison_operators)
- [三目運算符](#ternary_conditional_operator)
- [空合運算符](#nil_coalescing_operator)
- [區間運算符](#range_operators)
- [邏輯運算符](#logical_operators)

*運算符*是檢查、改變、合並值的特殊符號或短語。例如，加號（`+`）將兩個數相加（如 `let i = 1 + 2`）。更復雜的運算例子包括邏輯與運算符 `&&`（如 `if enteredDoorCode && passedRetinaScan`）。

Swift 支持大部分標准 C 語言的運算符，且改進許多特性來減少常規編碼錯誤。如：賦值符（`=`）不返回值，以防止把想要判斷相等運算符（`==`）的地方寫成賦值符導致的錯誤。算術運算符（`+`，`-`，`*`，`/`，`%`等）會檢測並不允許值溢出，以此來避免保存變量時由於變量大於或小於其類型所能承載的范圍時導致的異常結果。當然允許你使用 Swift 的溢出運算符來實現溢出。詳情參見[溢出運算符](../chapter2/25_Advanced_Operators.html#overflow_operators)。

Swift 還提供了 C 語言沒有的表達兩數之間的值的區間運算符（`a..<b` 和 `a...b`），這方便我們表達一個區間內的數值。

本章節只描述了 Swift 中的基本運算符，[高級運算符](../chapter2/25_Advanced_Operators.html)這章會包含 Swift 中的高級運算符，及如何自定義運算符，及如何進行自定義類型的運算符重載。

<a name="terminology"></a>
## 術語

運算符分為一元、二元和三元運算符:

- *一元*運算符對單一操作對象操作（如 `-a`）。一元運算符分前置運算符和後置運算符，*前置運算符*需緊跟在操作對象之前（如 `!b`），*後置運算符*需緊跟在操作對象之後（如 `c!`）。
- *二元*運算符操作兩個操作對象（如 `2 + 3`），是*中置*的，因為它們出現在兩個操作對象之間。
- *三元*運算符操作三個操作對象，和 C 語言一樣，Swift 只有一個三元運算符，就是三目運算符（`a ? b : c`）。

受運算符影響的值叫*操作數*，在表達式 `1 + 2` 中，加號 `+` 是二元運算符，它的兩個操作數是值 `1` 和 `2`。

<a name="assignment_operator"></a>
## 賦值運算符

*賦值運算符*（`a = b`），表示用 `b` 的值來初始化或更新 `a` 的值：

```swift
let b = 10
var a = 5
a = b
// a 現在等於 10
```

如果賦值的右邊是一個多元組，它的元素可以馬上被分解成多個常量或變量：

```swift
let (x, y) = (1, 2)
// 現在 x 等於 1，y 等於 2
```

與 C 語言和 Objective-C 不同，Swift 的賦值操作並不返回任何值。所以以下代碼是錯誤的：

```swift
if x = y {
	// 此句錯誤, 因為 x = y 並不返回任何值
}
```

這個特性使你無法把（`==`）錯寫成（`=`），由於 `if x = y` 是錯誤代碼，Swift 能幫你避免此類錯誤發生。

<a name="arithmetic_operators"></a>
## 算術運算符

Swift 中所有數值類型都支持了基本的四則*算術運算符*：

- 加法（`+`）
- 減法（`-`）
- 乘法（`*`）
- 除法（`/`）

```swift
1 + 2       // 等於 3
5 - 3       // 等於 2
2 * 3       // 等於 6
10.0 / 2.5  // 等於 4.0
```

與 C 語言和 Objective-C 不同的是，Swift 默認情況下不允許在數值運算中出現溢出情況。但是你可以使用 Swift 的溢出運算符來實現溢出運算（如 `a &+ b`）。詳情參見[溢出運算符](../chapter2/25_Advanced_Operators.html#overflow_operators)。

加法運算符也可用於 `String` 的拼接：

```swift
"hello, " + "world"  // 等於 "hello, world"
```

### 求余運算符

*求余運算符*（`a % b`）是計算 `b` 的多少倍剛剛好可以容入`a`，返回多出來的那部分（余數）。

> 注意：  
求余運算符（`%`）在其他語言也叫*取模運算符*。然而嚴格說來，我們看該運算符對負數的操作結果，「求余」比「取模」更合適些。

我們來談談取余是怎麼回事，計算 `9 % 4`，你先計算出 `4` 的多少倍會剛好可以容入 `9` 中：

![Art/remainderInteger_2x.png](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/remainderInteger_2x.png "Art/remainderInteger_2x.png")

你可以在 `9` 中放入兩個 `4`，那余數是 1（用橙色標出）。

在 Swift 中可以表達為：

```swift
9 % 4    // 等於 1
```

為了得到 `a % b` 的結果，`%` 計算了以下等式，並輸出`余數`作為結果：

	a = (b × 倍數) + 余數

當`倍數`取最大值的時候，就會剛好可以容入 `a` 中。

把 `9` 和 `4` 代入等式中，我們得 `1`：

	9 = (4 × 2) + 1

同樣的方法，我們來計算 `-9 % 4`：

```swift
-9 % 4   // 等於 -1
```

把 `-9` 和 `4` 代入等式，`-2` 是取到的最大整數：

	-9 = (4 × -2) + -1

余數是 `-1`。

在對負數 `b` 求余時，`b` 的符號會被忽略。這意味著 `a % b` 和 `a % -b` 的結果是相同的。


### 一元負號運算符

數值的正負號可以使用前綴 `-`（即*一元負號符*）來切換：

```swift
let three = 3
let minusThree = -three       // minusThree 等於 -3
let plusThree = -minusThree   // plusThree 等於 3, 或 "負負3"
```

一元負號符（`-`）寫在操作數之前，中間沒有空格。

### 一元正號運算符

*一元正號符*（`+`）不做任何改變地返回操作數的值：

```swift
let minusSix = -6
let alsoMinusSix = +minusSix  // alsoMinusSix 等於 -6
```

雖然一元正號符什麼都不會改變，但當你在使用一元負號來表達負數時，你可以使用一元正號來表達正數，如此你的代碼會具有對稱美。


<a name="compound_assignment_operators"></a>
## 組合賦值運算符

如同 C 語言，Swift 也提供把其他運算符和賦值運算（`=`）組合的*組合賦值運算符*，組合加運算（`+=`）是其中一個例子：

```swift
var a = 1
a += 2
// a 現在是 3
```

表達式 `a += 2` 是 `a = a + 2` 的簡寫，一個組合加運算就是把加法運算和賦值運算組合成進一個運算符裡，同時完成兩個運算任務。

> 注意：  
復合賦值運算沒有返回值，`let b = a += 2`這類代碼是錯誤。這不同於上面提到的自增和自減運算符。

在[Swift 標准庫運算符參考](https://developer.apple.com/reference/swift/1851035-swift_standard_library_operators)章節裡有復合運算符的完整列表。
‌
<a name="comparison_operators"></a>
## 比較運算符（Comparison Operators）

所有標准 C 語言中的*比較運算符*都可以在 Swift 中使用：

- 等於（`a == b`）
- 不等於（`a != b`）
- 大於（`a > b`）
- 小於（`a < b`）
- 大於等於（`a >= b`）
- 小於等於（`a <= b`）

> 注意：
Swift 也提供恆等（`===`）和不恆等（`!==`）這兩個比較符來判斷兩個對象是否引用同一個對象實例。更多細節在[類與結構](../chapter2/09_Classes_and_Structures.html)。

每個比較運算都返回了一個標識表達式是否成立的布爾值：

```swift
1 == 1   // true, 因為 1 等於 1
2 != 1   // true, 因為 2 不等於 1
2 > 1    // true, 因為 2 大於 1
1 < 2    // true, 因為 1 小於2
1 >= 1   // true, 因為 1 大於等於 1
2 <= 1   // false, 因為 2 並不小於等於 1
```

比較運算多用於條件語句，如`if`條件：

```swift
let name = "world"
if name == "world" {
	print("hello, world")
} else {
	print("I'm sorry \(name), but I don't recognize you")
}
// 輸出 "hello, world", 因為 `name` 就是等於 "world"
```

關於 `if` 語句，請看[控制流](../chapter2/05_Control_Flow.html)。

當元組中的值可以比較時，你也可以使用這些運算符來比較它們的大小。例如，因為 `Int` 和 `String` 類型的值可以比較，所以類型為 `(Int, String)` 的元組也可以被比較。相反，`Bool` 不能被比較，也意味著存有布爾類型的元組不能被比較。

比較元組大小會按照從左到右、逐值比較的方式，直到發現有兩個值不等時停止。如果所有的值都相等，那麼這一對元組我們就稱它們是相等的。例如：

```swift
(1, "zebra") < (2, "apple")   // true，因為 1 小於 2
(3, "apple") < (3, "bird")    // true，因為 3 等於 3，但是 apple 小於 bird
(4, "dog") == (4, "dog")      // true，因為 4 等於 4，dog 等於 dog
```

在上面的例子中，你可以看到，在第一行中從左到右的比較行為。因為`1`小於`2`，所以`(1, "zebra")`小於`(2, "apple")`，不管元組剩下的值如何。所以`"zebra"`小於`"apple"`沒有任何影響，因為元組的比較已經被第一個元素決定了。不過，當元組的第一個元素相同時候，第二個元素將會用作比較-第二行和第三行代碼就發生了這樣的比較。

>注意：    
Swift 標准庫只能比較七個以內元素的元組比較函數。如果你的元組元素超過七個時，你需要自己實現比較運算符。

<a name="ternary_conditional_operator"></a>
## 三目運算符（Ternary Conditional Operator）

*三目運算符*的特殊在於它是有三個操作數的運算符，它的形式是 `問題 ? 答案 1 : 答案 2`。它簡潔地表達根據 `問題`成立與否作出二選一的操作。如果 `問題` 成立，返回 `答案 1` 的結果；反之返回 `答案 2` 的結果。

三目運算符是以下代碼的縮寫形式：

```swift
if question {
	answer1
} else {
	answer2
}
```

這裡有個計算表格行高的例子。如果有表頭，那行高應比內容高度要高出 50 點；如果沒有表頭，只需高出 20 點：

```swift
let contentHeight = 40
let hasHeader = true
let rowHeight = contentHeight + (hasHeader ? 50 : 20)
// rowHeight 現在是 90
```

上面的寫法比下面的代碼更簡潔：

```swift
let contentHeight = 40
let hasHeader = true
var rowHeight = contentHeight
if hasHeader {
	rowHeight = rowHeight + 50
} else {
	rowHeight = rowHeight + 20
}
// rowHeight 現在是 90
```

第一段代碼例子使用了三目運算，所以一行代碼就能讓我們得到正確答案。這比第二段代碼簡潔得多，無需將 `rowHeight` 定義成變量，因為它的值無需在 `if` 語句中改變。

三目運算提供有效率且便捷的方式來表達二選一的選擇。需要注意的事，過度使用三目運算符會使簡潔的代碼變的難懂。我們應避免在一個組合語句中使用多個三目運算符。

<a name="nil_coalescing_operator"></a>
## 空合運算符（Nil Coalescing Operator）

*空合運算符*（`a ?? b`）將對可選類型 `a` 進行空判斷，如果 `a` 包含一個值就進行解封，否則就返回一個默認值 `b`。表達式 `a` 必須是 Optional 類型。默認值 `b` 的類型必須要和 `a` 存儲值的類型保持一致。

空合運算符是對以下代碼的簡短表達方法：

```swift
a != nil ? a! : b
```

上述代碼使用了三目運算符。當可選類型 `a` 的值不為空時，進行強制解封（`a!`），訪問 `a` 中的值；反之返回默認值 `b`。無疑空合運算符（`??`）提供了一種更為優雅的方式去封裝條件判斷和解封兩種行為，顯得簡潔以及更具可讀性。

> 注意：
如果 `a` 為非空值（`non-nil`），那麼值 `b` 將不會被計算。這也就是所謂的*短路求值*。

下文例子采用空合運算符，實現了在默認顏色名和可選自定義顏色名之間抉擇：

```swift
let defaultColorName = "red"
var userDefinedColorName: String?   //默認值為 nil

var colorNameToUse = userDefinedColorName ?? defaultColorName
// userDefinedColorName 的值為空，所以 colorNameToUse 的值為 "red"
```

`userDefinedColorName` 變量被定義為一個可選的 `String` 類型，默認值為 `nil`。由於 `userDefinedColorName` 是一個可選類型，我們可以使用空合運算符去判斷其值。在上一個例子中，通過空合運算符為一個名為 `colorNameToUse` 的變量賦予一個字符串類型初始值。
由於 `userDefinedColorName` 值為空，因此表達式 `userDefinedColorName ?? defaultColorName` 返回 `defaultColorName` 的值，即 `red`。

另一種情況，分配一個非空值（`non-nil`）給 `userDefinedColorName`，再次執行空合運算，運算結果為封包在 `userDefaultColorName` 中的值，而非默認值。

```swift
userDefinedColorName = "green"
colorNameToUse = userDefinedColorName ?? defaultColorName
// userDefinedColorName 非空，因此 colorNameToUse 的值為 "green"
```

<a name="range_operators"></a>
## 區間運算符（Range Operators）

Swift 提供了兩個方便表達一個區間的值的*區間運算符*。

### 閉區間運算符
*閉區間運算符*（`a...b`）定義一個包含從 `a` 到 `b`（包括 `a` 和 `b`）的所有值的區間。`a` 的值不能超過 `b`。
‌
閉區間運算符在迭代一個區間的所有值時是非常有用的，如在 `for-in` 循環中：

```swift
for index in 1...5 {
	print("\(index) * 5 = \(index * 5)")
}
// 1 * 5 = 5
// 2 * 5 = 10
// 3 * 5 = 15
// 4 * 5 = 20
// 5 * 5 = 25
```

關於 `for-in`，請看[控制流](../chapter2/05_Control_Flow.html)。

### 半開區間運算符

*半開區間運算符*（`a..<b`）定義一個從 `a` 到 `b` 但不包括 `b` 的區間。
之所以稱為*半開區間*，是因為該區間包含第一個值而不包括最後的值。

半開區間的實用性在於當你使用一個從 0 開始的列表（如數組）時，非常方便地從0數到列表的長度。

```swift
let names = ["Anna", "Alex", "Brian", "Jack"]
let count = names.count
for i in 0..<count {
	print("第 \(i + 1) 個人叫 \(names[i])")
}
// 第 1 個人叫 Anna
// 第 2 個人叫 Alex
// 第 3 個人叫 Brian
// 第 4 個人叫 Jack
```

數組有 4 個元素，但 `0..<count` 只數到3（最後一個元素的下標），因為它是半開區間。關於數組，請查閱[數組](../chapter2/04_Collection_Types.html#arrays)。

<a name="logical_operators"></a>
## 邏輯運算符（Logical Operators）

*邏輯運算符*的操作對象是邏輯布爾值。Swift 支持基於 C 語言的三個標准邏輯運算。

- 邏輯非（`!a`）
- 邏輯與（`a && b`）
- 邏輯或（`a || b`）

### 邏輯非運算符

*邏輯非運算符*（`!a`）對一個布爾值取反，使得 `true` 變 `false`，`false` 變 `true`。

它是一個前置運算符，需緊跟在操作數之前，且不加空格。讀作 `非 a` ，例子如下：

```swift
let allowedEntry = false
if !allowedEntry {
	print("ACCESS DENIED")
}
// 輸出 "ACCESS DENIED"
```

`if !allowedEntry` 語句可以讀作「如果非 allowedEntry」，接下一行代碼只有在「非 allowedEntry」為 `true`，即 `allowEntry` 為 `false` 時被執行。

在示例代碼中，小心地選擇布爾常量或變量有助於代碼的可讀性，並且避免使用雙重邏輯非運算，或混亂的邏輯語句。

### 邏輯與運算符

*邏輯與運算符*（`a && b`）表達了只有 `a` 和 `b` 的值都為 `true` 時，整個表達式的值才會是 `true`。

只要任意一個值為 `false`，整個表達式的值就為 `false`。事實上，如果第一個值為 `false`，那麼是不去計算第二個值的，因為它已經不可能影響整個表達式的結果了。這被稱做*短路計算（short-circuit evaluation）*。

以下例子，只有兩個 `Bool` 值都為 `true` 的時候才允許進入 if：

```swift
let enteredDoorCode = true
let passedRetinaScan = false
if enteredDoorCode && passedRetinaScan {
	print("Welcome!")
} else {
	print("ACCESS DENIED")
}
// 輸出 "ACCESS DENIED"
```

### 邏輯或運算符

邏輯或運算符（`a || b`）是一個由兩個連續的 `|` 組成的中置運算符。它表示了兩個邏輯表達式的其中一個為 `true`，整個表達式就為 `true`。

同邏輯與運算符類似，邏輯或也是「短路計算」的，當左端的表達式為 `true` 時，將不計算右邊的表達式了，因為它不可能改變整個表達式的值了。

以下示例代碼中，第一個布爾值（`hasDoorKey`）為 `false`，但第二個值（`knowsOverridePassword`）為 `true`，所以整個表達是 `true`，於是允許進入：

```swift
let hasDoorKey = false
let knowsOverridePassword = true
if hasDoorKey || knowsOverridePassword {
	print("Welcome!")
} else {
	print("ACCESS DENIED")
}
// 輸出 "Welcome!"
```

### 邏輯運算符組合計算

我們可以組合多個邏輯運算符來表達一個復合邏輯：

```swift
if enteredDoorCode && passedRetinaScan || hasDoorKey || knowsOverridePassword {
	print("Welcome!")
} else {
	print("ACCESS DENIED")
}
// 輸出 "Welcome!"
```

這個例子使用了含多個 `&&` 和 `||` 的復合邏輯。但無論怎樣，`&&` 和 `||` 始終只能操作兩個值。所以這實際是三個簡單邏輯連續操作的結果。我們來解讀一下：

如果我們輸入了正確的密碼並通過了視網膜掃描，或者我們有一把有效的鑰匙，又或者我們知道緊急情況下重置的密碼，我們就能把門打開進入。

前兩種情況，我們都不滿足，所以前兩個簡單邏輯的結果是 `false`，但是我們是知道緊急情況下重置的密碼的，所以整個復雜表達式的值還是 `true`。

> 注意：
Swift 邏輯操作符 `&&` 和 `||` 是左結合的，這意味著擁有多元邏輯操作符的復合表達式優先計算最左邊的子表達式。

### 使用括號來明確優先級

為了一個復雜表達式更容易讀懂，在合適的地方使用括號來明確優先級是很有效的，雖然它並非必要的。在上個關於門的權限的例子中，我們給第一個部分加個括號，使它看起來邏輯更明確：

```swift
if (enteredDoorCode && passedRetinaScan) || hasDoorKey || knowsOverridePassword {
	print("Welcome!")
} else {
	print("ACCESS DENIED")
}
// 輸出 "Welcome!"
```

這括號使得前兩個值被看成整個邏輯表達中獨立的一個部分。雖然有括號和沒括號的輸出結果是一樣的，但對於讀代碼的人來說有括號的代碼更清晰。可讀性比簡潔性更重要，請在可以讓你代碼變清晰的地方加個括號吧！
