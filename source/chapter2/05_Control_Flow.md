# 控制流（Control Flow）
-----------------

> 1.0
> 翻譯：[vclwei](https://github.com/vclwei), [coverxit](https://github.com/coverxit), [NicePiao](https://github.com/NicePiao)
> 校對：[coverxit](https://github.com/coverxit), [stanzhai](https://github.com/stanzhai)

> 2.0
> 翻譯+校對：[JackAlan](https://github.com/AlanMelody)

> 2.1
> 翻譯：[Prayer](https://github.com/futantan)
> 校對：[shanks](http://codebuild.me)

> 2.2
> 翻譯：[LinusLing](https://github.com/linusling)
> 校對：[SketchK](https://github.com/SketchK)

> 3.0
> 翻譯：[Realank](https://github.com/realank) 2016-09-13  
> 3.0.1，shanks，2016-11-12

> 3.1
> 翻譯：[qhd](https://github.com/qhd) 2017-04-17  

本頁包含內容：

- [For-In 循環](#for_in_loops)
- [While 循環](#while_loops)
- [條件語句](#conditional_statement)
- [控制轉移語句（Control Transfer Statements）](#control_transfer_statements)
- [提前退出](#early_exit)
- [檢測 API 可用性](#checking_api_availability)

Swift提供了多種流程控制結構，包括可以多次執行任務的`while`循環，基於特定條件選擇執行不同代碼分支的`if`、`guard`和`switch`語句，還有控制流程跳轉到其他代碼位置的`break`和`continue`語句。

Swift 還提供了`for-in`循環，用來更簡單地遍歷數組（array），字典（dictionary），區間（range），字符串（string）和其他序列類型。

Swift 的`switch`語句比 C 語言中更加強大。在 C 語言中，如果某個 case 不小心漏寫了`break`，這個 case 就會貫穿至下一個 case，Swift 無需寫`break`，所以不會發生這種貫穿的情況。case 還可以匹配很多不同的模式，包括間隔匹配（interval match），元組（tuple）和轉換到特定類型。`switch`語句的 case 中匹配的值可以綁定成臨時常量或變量，在case體內使用，也可以用`where`來描述更復雜的匹配條件。

<a name="for_in_loops"></a>
## For-In 循環

你可以使用 `for-in` 循環來遍歷一個集合中的所有元素，例如數組中的元素、數字范圍或者字符串中的字符。

以下例子使用 `for-in` 遍歷一個數組所有元素：  

```swift
let names = ["Anna", "Alex", "Brian", "Jack"]
for name in names {
    print("Hello, \(name)!")
}
// Hello, Anna!
// Hello, Alex!
// Hello, Brian!
// Hello, Jack!
```

你也可以通過遍歷一個字典來訪問它的鍵值對。遍歷字典時，字典的每項元素會以 `(key, value)` 元組的形式返回，你可以在 `for-in` 循環中使用顯式的常量名稱來解讀 `(key, value)` 元組。下面的例子中，字典的鍵解讀為常量 `animalName`，字典的值會被解讀為常量 `legCount`：  

```swift
let numberOfLegs = ["spider": 8, "ant": 6, "cat": 4]
for (animalName, legCount) in numberOfLegs {
    print("\(animalName)s have \(legCount) legs")
}
// ants have 6 legs
// spiders have 8 legs
// cats have 4 legs
```

字典的內容本質上是無序的，遍歷元素時不能保證順序。特別地，將元素插入一個字典的順序並不會決定它們被遍歷的順序。關於數組和字典，詳情參見[集合類型](./04_Collection_Types.html)。  

`for-in` 循環還可以使用數字范圍。下面的例子用來輸出乘 5 乘法表前面一部分內容：  

```swift
for index in 1...5 {
    print("\(index) times 5 is \(index * 5)")
}
// 1 times 5 is 5
// 2 times 5 is 10
// 3 times 5 is 15
// 4 times 5 is 20
// 5 times 5 is 25
```

例子中用來進行遍歷的元素是使用閉區間操作符（`...`）表示的從 `1` 到 `5` 的數字區間。`index` 被賦值為閉區間中的第一個數字（`1`），然後循環中的語句被執行一次。在本例中，這個循環只包含一個語句，用來輸出當前 `index` 值所對應的乘 5 乘法表的結果。該語句執行後，`index` 的值被更新為閉區間中的第二個數字（`2`），之後 `print(_:separator:terminator:)` 函數會再執行一次。整個過程會進行到閉區間結尾為止。  

上面的例子中，`index` 是一個每次循環遍歷開始時被自動賦值的常量。這種情況下，`index` 在使用前不需要聲明，只需要將它包含在循環的聲明中，就可以對其進行隱式聲明，而無需使用 `let` 關鍵字聲明。  

如果你不需要區間序列內每一項的值，你可以使用下劃線（`_`）替代變量名來忽略這個值：  

```swift
let base = 3
let power = 10
var answer = 1
for _ in 1...power {
    answer *= base
}
print("\(base) to the power of \(power) is \(answer)")
// 輸出 "3 to the power of 10 is 59049"
```

這個例子計算 base 這個數的 power 次冪（本例中，是 `3` 的 `10` 次冪），從 `1`（ `3` 的 `0` 次冪）開始做 `3` 的乘法， 進行 `10` 次，使用 `1` 到 `10` 的閉區間循環。這個計算並不需要知道每一次循環中計數器具體的值，只需要執行了正確的循環次數即可。下劃線符號 `_` （替代循環中的變量）能夠忽略當前值，並且不提供循環遍歷時對值的訪問。  

在某些情況下，你可能不想使用閉區間，包括兩個端點。在一個手表上每分鍾繪制一個刻度線。要繪制 `60` 個刻度，從 `0` 分鍾開始。使用半開區間運算符（`..<`）來包含下限，但不包括上限。有關區間的更多信息，請參閱[區間運算符](./02_Basic_Operators.html#range_operators)。  

```
let minutes = 60
for tickMark in 0..<minutes {
    // 每1分鍾呈現一個刻度線（60次）
}
```

一些用戶可能在其UI中可能需要較少的刻度。他們可以每5分鍾作為一個刻度。使用 `stride(from:to:by:)` 函數跳過不需要的標記。  

```
let minuteInterval = 5
for tickMark in stride(from: 0, to: minutes, by: minuteInterval) {
    // 每5分鍾呈現一個刻度線 (0, 5, 10, 15 ... 45, 50, 55)
}
```

可以在閉區間使用 `stride(from:through:by:)` 起到同樣作用：  

```
let hours = 12
let hourInterval = 3
for tickMark in stride(from: 3, through: hours, by: hourInterval) {
    // 每3小時呈現一個刻度線 (3, 6, 9, 12)
}
```

<a name="while_loops"></a>
## While 循環

`while`循環會一直運行一段語句直到條件變成`false`。這類循環適合使用在第一次迭代前，迭代次數未知的情況下。Swift 提供兩種`while`循環形式：

* `while`循環，每次在循環開始時計算條件是否符合；
* `repeat-while`循環，每次在循環結束時計算條件是否符合。

<a name="while"></a>
### While

`while`循環從計算一個條件開始。如果條件為`true`，會重復運行一段語句，直到條件變為`false`。

下面是 `while` 循環的一般格式：

```
while condition {  
	statements
}
```

下面的例子來玩一個叫做*蛇和梯子*（也叫做*滑道和梯子*）的小游戲：

![image](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/snakesAndLadders_2x.png)

游戲的規則如下：

* 游戲盤面包括 25 個方格，游戲目標是達到或者超過第 25 個方格；
* 每一輪，你通過擲一個六面體骰子來確定你移動方塊的步數，移動的路線由上圖中橫向的虛線所示；
* 如果在某輪結束，你移動到了梯子的底部，可以順著梯子爬上去；
* 如果在某輪結束，你移動到了蛇的頭部，你會順著蛇的身體滑下去。

游戲盤面可以使用一個`Int`數組來表達。數組的長度由一個`finalSquare`常量儲存，用來初始化數組和檢測最終勝利條件。游戲盤面由 26 個 `Int` 0 值初始化，而不是 25 個（由`0`到`25`，一共 26 個）：

```swift
let finalSquare = 25
var board = [Int](repeating: 0, count: finalSquare + 1)
```

一些方格被設置成特定的值來表示有蛇或者梯子。梯子底部的方格是一個正值，使你可以向上移動，蛇頭處的方格是一個負值，會讓你向下移動：

```swift
board[03] = +08; board[06] = +11; board[09] = +09; board[10] = +02
board[14] = -10; board[19] = -11; board[22] = -02; board[24] = -08
```

3 號方格是梯子的底部，會讓你向上移動到 11 號方格，我們使用`board[03]`等於`+08`（來表示`11`和`3`之間的差值）。使用一元正運算符（`+i`）是為了和一元負運算符（`-i`）對稱，為了讓盤面代碼整齊，小於 10 的數字都使用 0 補齊（這些風格上的調整都不是必須的，只是為了讓代碼看起來更加整潔）。

玩家由左下角空白處編號為 0 的方格開始游戲。玩家第一次擲骰子後才會進入游戲盤面：

```swift
var square = 0
var diceRoll = 0
while square < finalSquare {
    // 擲骰子
    diceRoll += 1
    if diceRoll == 7 { diceRoll = 1 }
    // 根據點數移動
    square += diceRoll
    if square < board.count {
        // 如果玩家還在棋盤上，順著梯子爬上去或者順著蛇滑下去
        square += board[square]
    }
}
print("Game over!")
```

本例中使用了最簡單的方法來模擬擲骰子。 `diceRoll`的值並不是一個隨機數，而是以`0`為初始值，之後每一次`while`循環，`diceRoll`的值增加 1 ，然後檢測是否超出了最大值。當`diceRoll`的值等於 7 時，就超過了骰子的最大值，會被重置為`1`。所以`diceRoll`的取值順序會一直是 `1` ，`2`，`3`，`4`，`5`，`6`，`1`，`2` 等。

擲完骰子後，玩家向前移動`diceRoll`個方格，如果玩家移動超過了第 25 個方格，這個時候游戲將會結束，為了應對這種情況，代碼會首先判斷`square`的值是否小於`board`的`count`屬性，只有小於才會在`board[square]`上增加`square`，來向前或向後移動（遇到了梯子或者蛇）。

> 注意：   
> 如果沒有這個檢測（`square < board.count`），`board[square]`可能會越界訪問`board`數組，導致錯誤。如果`square`等於`26`， 代碼會去嘗試訪問`board[26]`，超過數組的長度。

當本輪`while`循環運行完畢，會再檢測循環條件是否需要再運行一次循環。如果玩家移動到或者超過第 25 個方格，循環條件結果為`false`，此時游戲結束。

`while` 循環比較適合本例中的這種情況，因為在 `while` 循環開始時，我們並不知道游戲要跑多久，只有在達成指定條件時循環才會結束。


<a name="repeat_while"></a>
### Repeat-While

`while`循環的另外一種形式是`repeat-while`，它和`while`的區別是在判斷循環條件之前，先執行一次循環的代碼塊。然後重復循環直到條件為`false`。

> 注意：   
> Swift語言的`repeat-while`循環和其他語言中的`do-while`循環是類似的。

下面是 `repeat-while`循環的一般格式：

```swift
repeat {
	statements
} while condition
```

還是*蛇和梯子*的游戲，使用`repeat-while`循環來替代`while`循環。`finalSquare`、`board`、`square`和`diceRoll`的值初始化同`while`循環時一樣：

``` swift
let finalSquare = 25
var board = [Int](repeating: 0, count: finalSquare + 1)
board[03] = +08; board[06] = +11; board[09] = +09; board[10] = +02
board[14] = -10; board[19] = -11; board[22] = -02; board[24] = -08
var square = 0
var diceRoll = 0
```

`repeat-while`的循環版本，循環中*第一步*就需要去檢測是否在梯子或者蛇的方塊上。沒有梯子會讓玩家直接上到第 25 個方格，所以玩家不會通過梯子直接贏得游戲。這樣在循環開始時先檢測是否踩在梯子或者蛇上是安全的。

游戲開始時，玩家在第 0 個方格上，`board[0]`一直等於 0， 不會有什麼影響：

```swift
repeat {
    // 順著梯子爬上去或者順著蛇滑下去
    square += board[square]
    // 擲骰子
    diceRoll += 1
    if diceRoll == 7 { diceRoll = 1 }
    // 根據點數移動
    square += diceRoll
} while square < finalSquare
print("Game over!")
```

檢測完玩家是否踩在梯子或者蛇上之後，開始擲骰子，然後玩家向前移動`diceRoll`個方格，本輪循環結束。

循環條件（`while square < finalSquare`）和`while`方式相同，但是只會在循環結束後進行計算。在這個游戲中，`repeat-while`表現得比`while`循環更好。`repeat-while`方式會在條件判斷`square`沒有超出後直接運行`square += board[square]`，這種方式可以去掉`while`版本中的數組越界判斷。

<a name="conditional_statement"></a>
## 條件語句

根據特定的條件執行特定的代碼通常是十分有用的。當錯誤發生時，你可能想運行額外的代碼；或者，當值太大或太小時，向用戶顯示一條消息。要實現這些功能，你就需要使用*條件語句*。

Swift 提供兩種類型的條件語句：`if`語句和`switch`語句。通常，當條件較為簡單且可能的情況很少時，使用`if`語句。而`switch`語句更適用於條件較復雜、有更多排列組合的時候。並且`switch`在需要用到模式匹配（pattern-matching）的情況下會更有用。

<a name="if"></a>
### If

`if`語句最簡單的形式就是只包含一個條件，只有該條件為`true`時，才執行相關代碼：

```swift
var temperatureInFahrenheit = 30
if temperatureInFahrenheit <= 32 {
    print("It's very cold. Consider wearing a scarf.")
}
// 輸出 "It's very cold. Consider wearing a scarf."
```

上面的例子會判斷溫度是否小於等於 32 華氏度（水的冰點）。如果是，則打印一條消息；否則，不打印任何消息，繼續執行`if`塊後面的代碼。

當然，`if`語句允許二選一執行，叫做`else`從句。也就是當條件為`false`時，執行 *else 語句*：

```swift
temperatureInFahrenheit = 40
if temperatureInFahrenheit <= 32 {
    print("It's very cold. Consider wearing a scarf.")
} else {
    print("It's not that cold. Wear a t-shirt.")
}
// 輸出 "It's not that cold. Wear a t-shirt."
```

顯然，這兩條分支中總有一條會被執行。由於溫度已升至 40 華氏度，不算太冷，沒必要再圍圍巾。因此，`else`分支就被觸發了。

你可以把多個`if`語句鏈接在一起，來實現更多分支：

```swift
temperatureInFahrenheit = 90
if temperatureInFahrenheit <= 32 {
    print("It's very cold. Consider wearing a scarf.")
} else if temperatureInFahrenheit >= 86 {
    print("It's really warm. Don't forget to wear sunscreen.")
} else {
    print("It's not that cold. Wear a t-shirt.")
}
// 輸出 "It's really warm. Don't forget to wear sunscreen."
```

在上面的例子中，額外的`if`語句用於判斷是不是特別熱。而最後的`else`語句被保留了下來，用於打印既不冷也不熱時的消息。

實際上，當不需要完整判斷情況的時候，最後的`else`語句是可選的：

```swift
temperatureInFahrenheit = 72
if temperatureInFahrenheit <= 32 {
    print("It's very cold. Consider wearing a scarf.")
} else if temperatureInFahrenheit >= 86 {
    print("It's really warm. Don't forget to wear sunscreen.")
}
```

在這個例子中，由於既不冷也不熱，所以不會觸發`if`或`else if`分支，也就不會打印任何消息。

<a name="switch"></a>
### Switch

`switch`語句會嘗試把某個值與若幹個模式（pattern）進行匹配。根據第一個匹配成功的模式，`switch`語句會執行對應的代碼。當有可能的情況較多時，通常用`switch`語句替換`if`語句。

`switch`語句最簡單的形式就是把某個值與一個或若幹個相同類型的值作比較：

```swift
switch some value to consider {
case value 1:
    respond to value 1
case value 2,
    value 3:
    respond to value 2 or 3
default:
    otherwise, do something else
}
```


`switch`語句由*多個 case* 構成，每個由`case`關鍵字開始。為了匹配某些更特定的值，Swift 提供了幾種方法來進行更復雜的模式匹配，這些模式將在本節的稍後部分提到。

與`if`語句類似，每一個 case 都是代碼執行的一條分支。`switch`語句會決定哪一條分支應該被執行，這個流程被稱作根據給定的值*切換(switching)*。

`switch`語句必須是完備的。這就是說，每一個可能的值都必須至少有一個 case 分支與之對應。在某些不可能涵蓋所有值的情況下，你可以使用默認（`default`）分支來涵蓋其它所有沒有對應的值，這個默認分支必須在`switch`語句的最後面。

下面的例子使用`switch`語句來匹配一個名為`someCharacter`的小寫字符：


```swift
let someCharacter: Character = "z"
switch someCharacter {
case "a":
    print("The first letter of the alphabet")
case "z":
    print("The last letter of the alphabet")
default:
    print("Some other character")
}
// 輸出 "The last letter of the alphabet"
```

在這個例子中，第一個 case 分支用於匹配第一個英文字母`a`，第二個 case 分支用於匹配最後一個字母`z`。
因為`switch`語句必須有一個case分支用於覆蓋所有可能的字符，而不僅僅是所有的英文字母，所以switch語句使用`default`分支來匹配除了`a`和`z`外的所有值，這個分支保證了swith語句的完備性。


<a name="no_implicit_fallthrough"></a>
#### 不存在隱式的貫穿

與 C 和 Objective-C 中的`switch`語句不同，在 Swift 中，當匹配的 case 分支中的代碼執行完畢後，程序會終止`switch`語句，而不會繼續執行下一個 case 分支。這也就是說，不需要在 case 分支中顯式地使用`break`語句。這使得`switch`語句更安全、更易用，也避免了因忘記寫`break`語句而產生的錯誤。

> 注意：
雖然在Swift中`break`不是必須的，但你依然可以在 case 分支中的代碼執行完畢前使用`break`跳出，詳情請參見[Switch 語句中的 break](#break_in_a_switch_statement)。

每一個 case 分支都*必須*包含至少一條語句。像下面這樣書寫代碼是無效的，因為第一個 case 分支是空的：

```swift
let anotherCharacter: Character = "a"
switch anotherCharacter {
case "a": // 無效，這個分支下面沒有語句
case "A":
    print("The letter A")
default:
    print("Not the letter A")
}
// 這段代碼會報編譯錯誤
```

不像 C 語言裡的`switch`語句，在 Swift 中，`switch`語句不會一起匹配`"a"`和`"A"`。相反的，上面的代碼會引起編譯期錯誤：`case "a": 不包含任何可執行語句`——這就避免了意外地從一個 case 分支貫穿到另外一個，使得代碼更安全、也更直觀。

為了讓單個case同時匹配`a`和`A`，可以將這個兩個值組合成一個復合匹配，並且用逗號分開：
```swift
let anotherCharacter: Character = "a"
switch anotherCharacter {
case "a", "A":
    print("The letter A")
default:
    print("Not the letter A")
}
// 輸出 "The letter A
```
為了可讀性，符合匹配可以寫成多行形式，詳情請參考[復合匹配](#compound_cases)

> 注意：
如果想要顯式貫穿case分支，請使用`fallthrough`語句，詳情請參考[貫穿](#fallthrough)。

<a name="interval_matching"></a>
#### 區間匹配

case 分支的模式也可以是一個值的區間。下面的例子展示了如何使用區間匹配來輸出任意數字對應的自然語言格式：

```swift
let approximateCount = 62
let countedThings = "moons orbiting Saturn"
var naturalCount: String
switch approximateCount {
case 0:
    naturalCount = "no"
case 1..<5:
    naturalCount = "a few"
case 5..<12:
    naturalCount = "several"
case 12..<100:
    naturalCount = "dozens of"
case 100..<1000:
    naturalCount = "hundreds of"
default:
    naturalCount = "many"
}
print("There are \(naturalCount) \(countedThings).")
// 輸出 "There are dozens of moons orbiting Saturn."
```

在上例中，`approximateCount`在一個`switch`聲明中被評估。每一個`case`都與之進行比較。因為`approximateCount`落在了 12 到 100 的區間，所以`naturalCount`等於`"dozens of"`值，並且此後的執行跳出了`switch`語句。



<a name="tuples"></a>
#### 元組

我們可以使用元組在同一個`switch`語句中測試多個值。元組中的元素可以是值，也可以是區間。另外，使用下劃線（`_`）來匹配所有可能的值。

下面的例子展示了如何使用一個`(Int, Int)`類型的元組來分類下圖中的點(x, y)：

```swift
let somePoint = (1, 1)
switch somePoint {
case (0, 0):
    print("(0, 0) is at the origin")
case (_, 0):
    print("(\(somePoint.0), 0) is on the x-axis")
case (0, _):
    print("(0, \(somePoint.1)) is on the y-axis")
case (-2...2, -2...2):
    print("(\(somePoint.0), \(somePoint.1)) is inside the box")
default:
    print("(\(somePoint.0), \(somePoint.1)) is outside of the box")
}
// 輸出 "(1, 1) is inside the box"
```

![image](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/coordinateGraphSimple_2x.png)

在上面的例子中，`switch`語句會判斷某個點是否是原點(0, 0)，是否在紅色的x軸上，是否在橘黃色的y軸上，是否在一個以原點為中心的4x4的藍色矩形裡，或者在這個矩形外面。

不像 C 語言，Swift 允許多個 case 匹配同一個值。實際上，在這個例子中，點(0, 0)可以匹配所有_四個 case_。但是，如果存在多個匹配，那麼只會執行第一個被匹配到的 case 分支。考慮點(0, 0)會首先匹配`case (0, 0)`，因此剩下的能夠匹配的分支都會被忽視掉。


<a name="value_bindings"></a>
#### 值綁定（Value Bindings）

case 分支允許將匹配的值綁定到一個臨時的常量或變量，並且在case分支體內使用 —— 這種行為被稱為*值綁定*（value binding），因為匹配的值在case分支體內，與臨時的常量或變量綁定。

下面的例子展示了如何在一個`(Int, Int)`類型的元組中使用值綁定來分類下圖中的點(x, y)：

```swift
let anotherPoint = (2, 0)
switch anotherPoint {
case (let x, 0):
    print("on the x-axis with an x value of \(x)")
case (0, let y):
    print("on the y-axis with a y value of \(y)")
case let (x, y):
    print("somewhere else at (\(x), \(y))")
}
// 輸出 "on the x-axis with an x value of 2"
```

![image](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/coordinateGraphMedium_2x.png)

在上面的例子中，`switch`語句會判斷某個點是否在紅色的x軸上，是否在橘黃色的y軸上，或者不在坐標軸上。

這三個 case 都聲明了常量`x`和`y`的占位符，用於臨時獲取元組`anotherPoint`的一個或兩個值。第一個 case ——`case (let x, 0)`將匹配一個縱坐標為`0`的點，並把這個點的橫坐標賦給臨時的常量`x`。類似的，第二個 case ——`case (0, let y)`將匹配一個橫坐標為`0`的點，並把這個點的縱坐標賦給臨時的常量`y`。

一旦聲明了這些臨時的常量，它們就可以在其對應的 case 分支裡使用。在這個例子中，它們用於打印給定點的類型。

請注意，這個`switch`語句不包含默認分支。這是因為最後一個 case ——`case let(x, y)`聲明了一個可以匹配余下所有值的元組。這使得`switch`語句已經完備了，因此不需要再書寫默認分支。

<a name="where"></a>
#### Where

case 分支的模式可以使用`where`語句來判斷額外的條件。

下面的例子把下圖中的點(x, y)進行了分類：

```swift
let yetAnotherPoint = (1, -1)
switch yetAnotherPoint {
case let (x, y) where x == y:
    print("(\(x), \(y)) is on the line x == y")
case let (x, y) where x == -y:
    print("(\(x), \(y)) is on the line x == -y")
case let (x, y):
    print("(\(x), \(y)) is just some arbitrary point")
}
// 輸出 "(1, -1) is on the line x == -y"
```

![image](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/coordinateGraphComplex_2x.png)

在上面的例子中，`switch`語句會判斷某個點是否在綠色的對角線`x == y`上，是否在紫色的對角線`x == -y`上，或者不在對角線上。

這三個 case 都聲明了常量`x`和`y`的占位符，用於臨時獲取元組`yetAnotherPoint`的兩個值。這兩個常量被用作`where`語句的一部分，從而創建一個動態的過濾器(filter)。當且僅當`where`語句的條件為`true`時，匹配到的 case 分支才會被執行。

就像是值綁定中的例子，由於最後一個 case 分支匹配了余下所有可能的值，`switch`語句就已經完備了，因此不需要再書寫默認分支。

<a name="compound_cases"></a>
#### 復合匹配

當多個條件可以使用同一種方法來處理時，可以將這幾種可能放在同一個`case`後面，並且用逗號隔開。當case後面的任意一種模式匹配的時候，這條分支就會被匹配。並且，如果匹配列表過長，還可以分行書寫：

```swift
let someCharacter: Character = "e"
switch someCharacter {
case "a", "e", "i", "o", "u":
    print("\(someCharacter) is a vowel")
case "b", "c", "d", "f", "g", "h", "j", "k", "l", "m",
     "n", "p", "q", "r", "s", "t", "v", "w", "x", "y", "z":
    print("\(someCharacter) is a consonant")
default:
    print("\(someCharacter) is not a vowel or a consonant")
}
// 輸出 "e is a vowel"
```

這個`switch`語句中的第一個case，匹配了英語中的五個小寫元音字母。相似的，第二個case匹配了英語中所有的小寫輔音字母。最終，`default`分支匹配了其它所有字符。
復合匹配同樣可以包含值綁定。復合匹配裡所有的匹配模式，都必須包含相同的值綁定。並且每一個綁定都必須獲取到相同類型的值。這保證了，無論復合匹配中的哪個模式發生了匹配，分支體內的代碼，都能獲取到綁定的值，並且綁定的值都有一樣的類型。

```swift
let stillAnotherPoint = (9, 0)
switch stillAnotherPoint {
case (let distance, 0), (0, let distance):
    print("On an axis, \(distance) from the origin")
default:
    print("Not on an axis")
}

// 輸出 "On an axis, 9 from the origin"

```

上面的case有兩個模式：`(let distance, 0)`匹配了在x軸上的值，`(0, let distance)`匹配了在y軸上的值。兩個模式都綁定了`distance`，並且`distance`在兩種模式下，都是整型——這意味著分支體內的代碼，只要case匹配，都可以獲取到`distance`值


<a name="control_transfer_statements"></a>
## 控制轉移語句

控制轉移語句改變你代碼的執行順序，通過它可以實現代碼的跳轉。Swift 有五種控制轉移語句：

- `continue`
- `break`
- `fallthrough`
- `return`
- `throw`

我們將會在下面討論`continue`、`break`和`fallthrough`語句。`return`語句將會在[函數](./06_Functions.html)章節討論，`throw`語句會在[錯誤拋出](./18_Error_Handling.html#throwing_errors)章節討論。

<a name="continue"></a>
### Continue

`continue`語句告訴一個循環體立刻停止本次循環，重新開始下次循環。就好像在說「本次循環我已經執行完了」，但是並不會離開整個循環體。

下面的例子把一個小寫字符串中的元音字母和空格字符移除，生成了一個含義模糊的短句：

```swift
let puzzleInput = "great minds think alike"
var puzzleOutput = ""
for character in puzzleInput.characters {
    switch character {
    case "a", "e", "i", "o", "u", " ":
        continue
    default:
        puzzleOutput.append(character)
    }
}
print(puzzleOutput)
    // 輸出 "grtmndsthnklk"
```

在上面的代碼中，只要匹配到元音字母或者空格字符，就調用`continue`語句，使本次循環結束，重新開始下次循環。這種行為使`switch`匹配到元音字母和空格字符時不做處理，而不是讓每一個匹配到的字符都被打印。

<a name="break"></a>
### Break

`break`語句會立刻結束整個控制流的執行。當你想要更早的結束一個`switch`代碼塊或者一個循環體時，你都可以使用`break`語句。

<a name="break_in_a_loop_statement"></a>
#### 循環語句中的 break

當在一個循環體中使用`break`時，會立刻中斷該循環體的執行，然後跳轉到表示循環體結束的大括號(`}`)後的第一行代碼。不會再有本次循環的代碼被執行，也不會再有下次的循環產生。

<a name="break_in_a_switch_statement"></a>
#### Switch 語句中的 break

當在一個`switch`代碼塊中使用`break`時，會立即中斷該`switch`代碼塊的執行，並且跳轉到表示`switch`代碼塊結束的大括號(`}`)後的第一行代碼。

這種特性可以被用來匹配或者忽略一個或多個分支。因為 Swift 的`switch`需要包含所有的分支而且不允許有為空的分支，有時為了使你的意圖更明顯，需要特意匹配或者忽略某個分支。那麼當你想忽略某個分支時，可以在該分支內寫上`break`語句。當那個分支被匹配到時，分支內的`break`語句立即結束`switch`代碼塊。

>注意：
當一個`switch`分支僅僅包含注釋時，會被報編譯時錯誤。注釋不是代碼語句而且也不能讓`switch`分支達到被忽略的效果。你應該使用`break`來忽略某個分支。

下面的例子通過`switch`來判斷一個`Character`值是否代表下面四種語言之一。為了簡潔，多個值被包含在了同一個分支情況中。

```swift
let numberSymbol: Character = "三"  // 簡體中文裡的數字 3
var possibleIntegerValue: Int?
switch numberSymbol {
case "1", "١", "一", "๑":
    possibleIntegerValue = 1
case "2", "٢", "二", "๒":
    possibleIntegerValue = 2
case "3", "٣", "三", "๓":
    possibleIntegerValue = 3
case "4", "٤", "四", "๔":
    possibleIntegerValue = 4
default:
    break
}
if let integerValue = possibleIntegerValue {
    print("The integer value of \(numberSymbol) is \(integerValue).")
} else {
    print("An integer value could not be found for \(numberSymbol).")
}
// 輸出 "The integer value of 三 is 3."
```

這個例子檢查`numberSymbol`是否是拉丁，阿拉伯，中文或者泰語中的`1`到`4`之一。如果被匹配到，該`switch`分支語句給`Int?`類型變量`possibleIntegerValue`設置一個整數值。

當`switch`代碼塊執行完後，接下來的代碼通過使用可選綁定來判斷`possibleIntegerValue`是否曾經被設置過值。因為是可選類型的緣故，`possibleIntegerValue`有一個隱式的初始值`nil`，所以僅僅當`possibleIntegerValue`曾被`switch`代碼塊的前四個分支中的某個設置過一個值時，可選的綁定才會被判定為成功。

在上面的例子中，想要把`Character`所有的的可能性都枚舉出來是不現實的，所以使用`default`分支來包含所有上面沒有匹配到字符的情況。由於這個`default`分支不需要執行任何動作，所以它只寫了一條`break`語句。一旦落入到`default`分支中後，`break`語句就完成了該分支的所有代碼操作，代碼繼續向下，開始執行`if let`語句。

<a name="fallthrough"></a>
### 貫穿

Swift 中的`switch`不會從上一個 case 分支落入到下一個 case 分支中。相反，只要第一個匹配到的 case 分支完成了它需要執行的語句，整個`switch`代碼塊完成了它的執行。相比之下，C 語言要求你顯式地插入`break`語句到每個 case 分支的末尾來阻止自動落入到下一個 case 分支中。Swift 的這種避免默認落入到下一個分支中的特性意味著它的`switch` 功能要比 C 語言的更加清晰和可預測，可以避免無意識地執行多個 case 分支從而引發的錯誤。

如果你確實需要 C 風格的貫穿的特性，你可以在每個需要該特性的 case 分支中使用`fallthrough`關鍵字。下面的例子使用`fallthrough`來創建一個數字的描述語句。

```swift
let integerToDescribe = 5
var description = "The number \(integerToDescribe) is"
switch integerToDescribe {
case 2, 3, 5, 7, 11, 13, 17, 19:
    description += " a prime number, and also"
    fallthrough
default:
    description += " an integer."
}
print(description)
// 輸出 "The number 5 is a prime number, and also an integer."
```

這個例子定義了一個`String`類型的變量`description`並且給它設置了一個初始值。函數使用`switch`邏輯來判斷`integerToDescribe`變量的值。當`integerToDescribe`的值屬於列表中的質數之一時，該函數在`description`後添加一段文字，來表明這個數字是一個質數。然後它使用`fallthrough`關鍵字來「貫穿」到`default`分支中。`default`分支在`description`的最後添加一段額外的文字，至此`switch`代碼塊執行完了。

如果`integerToDescribe`的值不屬於列表中的任何質數，那麼它不會匹配到第一個`switch`分支。而這裡沒有其他特別的分支情況，所以`integerToDescribe`匹配到`default`分支中。

當`switch`代碼塊執行完後，使用`print(_:separator:terminator:)`函數打印該數字的描述。在這個例子中，數字`5`被准確的識別為了一個質數。

> 注意：
> `fallthrough`關鍵字不會檢查它下一個將會落入執行的 case 中的匹配條件。`fallthrough`簡單地使代碼繼續連接到下一個 case 中的代碼，這和 C 語言標准中的`switch`語句特性是一樣的。

<a name="labeled_statements"></a>
### 帶標簽的語句

在 Swift 中，你可以在循環體和條件語句中嵌套循環體和條件語句來創造復雜的控制流結構。並且，循環體和條件語句都可以使用`break`語句來提前結束整個代碼塊。因此，顯式地指明`break`語句想要終止的是哪個循環體或者條件語句，會很有用。類似地，如果你有許多嵌套的循環體，顯式指明`continue`語句想要影響哪一個循環體也會非常有用。

為了實現這個目的，你可以使用標簽（*statement label*）來標記一個循環體或者條件語句，對於一個條件語句，你可以使用`break`加標簽的方式，來結束這個被標記的語句。對於一個循環語句，你可以使用`break`或者`continue`加標簽，來結束或者繼續這條被標記語句的執行。

聲明一個帶標簽的語句是通過在該語句的關鍵詞的同一行前面放置一個標簽，作為這個語句的前導關鍵字(introducor keyword)，並且該標簽後面跟隨一個冒號。下面是一個針對`while`循環體的標簽語法，同樣的規則適用於所有的循環體和條件語句。

> `label name`: while `condition` {
>     `statements`
> }

下面的例子是前面章節中*蛇和梯子*的適配版本，在此版本中，我們將使用一個帶有標簽的`while`循環體中調用`break`和`continue`語句。這次，游戲增加了一條額外的規則：

- 為了獲勝，你必須*剛好*落在第 25 個方塊中。

如果某次擲骰子使你的移動超出第 25 個方塊，你必須重新擲骰子，直到你擲出的骰子數剛好使你能落在第 25 個方塊中。

游戲的棋盤和之前一樣：

![image](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/snakesAndLadders_2x.png)

`finalSquare`、`board`、`square`和`diceRoll`值被和之前一樣的方式初始化：

```swift
let finalSquare = 25
var board = [Int](repeating: 0, count: finalSquare + 1)
board[03] = +08; board[06] = +11; board[09] = +09; board[10] = +02
board[14] = -10; board[19] = -11; board[22] = -02; board[24] = -08
var square = 0
var diceRoll = 0
```

這個版本的游戲使用`while`循環和`switch`語句來實現游戲的邏輯。`while`循環有一個標簽名`gameLoop`，來表明它是游戲的主循環。

該`while`循環體的條件判斷語句是`while square !=finalSquare`，這表明你必須剛好落在方格25中。

```swift
gameLoop: while square != finalSquare {
    diceRoll += 1
    if diceRoll == 7 { diceRoll = 1 }
    switch square + diceRoll {
    case finalSquare:
        // 骰子數剛好使玩家移動到最終的方格裡，游戲結束。
        break gameLoop
    case let newSquare where newSquare > finalSquare:
        // 骰子數將會使玩家的移動超出最後的方格，那麼這種移動是不合法的，玩家需要重新擲骰子
        continue gameLoop
    default:
        // 合法移動，做正常的處理
        square += diceRoll
        square += board[square]
    }
}
print("Game over!")
```

每次循環迭代開始時擲骰子。與之前玩家擲完骰子就立即移動不同，這裡使用了`switch`語句來考慮每次移動可能產生的結果，從而決定玩家本次是否能夠移動。

- 如果骰子數剛好使玩家移動到最終的方格裡，游戲結束。`break gameLoop`語句跳轉控制去執行`while`循環體後的第一行代碼，意味著游戲結束。
- 如果骰子數將會使玩家的移動超出最後的方格，那麼這種移動是不合法的，玩家需要重新擲骰子。`continue gameLoop`語句結束本次`while`循環，開始下一次循環。
- 在剩余的所有情況中，骰子數產生的都是合法的移動。玩家向前移動 `diceRoll` 個方格，然後游戲邏輯再處理玩家當前是否處於蛇頭或者梯子的底部。接著本次循環結束，控制跳轉到`while`循環體的條件判斷語句處，再決定是否需要繼續執行下次循環。

>注意：   
如果上述的`break`語句沒有使用`gameLoop`標簽，那麼它將會中斷`switch`語句而不是`while`循環。使用`gameLoop`標簽清晰的表明了`break`想要中斷的是哪個代碼塊。
同時請注意，當調用`continue gameLoop`去跳轉到下一次循環迭代時，這裡使用`gameLoop`標簽並不是嚴格必須的。因為在這個游戲中，只有一個循環體，所以`continue`語句會影響到哪個循環體是沒有歧義的。然而，`continue`語句使用`gameLoop`標簽也是沒有危害的。這樣做符合標簽的使用規則，同時參照旁邊的`break gameLoop`，能夠使游戲的邏輯更加清晰和易於理解。

<a name="early_exit"></a>
## 提前退出

像`if`語句一樣，`guard`的執行取決於一個表達式的布爾值。我們可以使用`guard`語句來要求條件必須為真時，以執行`guard`語句後的代碼。不同於`if`語句，一個`guard`語句總是有一個`else`從句，如果條件不為真則執行`else`從句中的代碼。


```swift
func greet(person: [String: String]) {
	guard let name = person["name"] else {
		return
	}
	print("Hello \(name)")
	guard let location = person["location"] else {
		print("I hope the weather is nice near you.")
		return
	}
	print("I hope the weather is nice in \(location).")
}
greet(["name": "John"])
// 輸出 "Hello John!"
// 輸出 "I hope the weather is nice near you."
greet(["name": "Jane", "location": "Cupertino"])
// 輸出 "Hello Jane!"
// 輸出 "I hope the weather is nice in Cupertino."
```

如果`guard`語句的條件被滿足，則繼續執行`guard`語句大括號後的代碼。將變量或者常量的可選綁定作為`guard`語句的條件，都可以保護`guard`語句後面的代碼。

如果條件不被滿足，在`else`分支上的代碼就會被執行。這個分支必須轉移控制以退出`guard`語句出現的代碼段。它可以用控制轉移語句如`return`,`break`,`continue`或者`throw`做這件事，或者調用一個不返回的方法或函數，例如`fatalError()`。

相比於可以實現同樣功能的`if`語句，按需使用`guard`語句會提升我們代碼的可讀性。它可以使你的代碼連貫的被執行而不需要將它包在`else`塊中，它可以使你在緊鄰條件判斷的地方，處理違規的情況。

<a name="checking_api_availability"></a>
## 檢測 API 可用性

Swift內置支持檢查 API 可用性，這可以確保我們不會在當前部署機器上，不小心地使用了不可用的API。

編譯器使用 SDK 中的可用信息來驗證我們的代碼中使用的所有 API 在項目指定的部署目標上是否可用。如果我們嘗試使用一個不可用的 API，Swift 會在編譯時報錯。

我們在`if`或`guard`語句中使用`可用性條件（availability condition)`去有條件的執行一段代碼，來在運行時判斷調用的API是否可用。編譯器使用從可用性條件語句中獲取的信息去驗證，在這個代碼塊中調用的 API 是否可用。

```swift
if #available(iOS 10, macOS 10.12, *) {
    // 在 iOS 使用 iOS 10 的 API, 在 macOS 使用 macOS 10.12 的 API
} else {
    // 使用先前版本的 iOS 和 macOS 的 API
}
```

以上可用性條件指定，在iOS中，`if`語句的代碼塊僅僅在 iOS 10 及更高的系統下運行；在 macOS中，僅在 macOS 10.12 及更高才會運行。最後一個參數，`*`，是必須的，用於指定在所有其它平台中，如果版本號高於你的設備指定的最低版本，if語句的代碼塊將會運行。

在它一般的形式中，可用性條件使用了一個平台名字和版本的列表。平台名字可以是`iOS`，`macOS`，`watchOS`和`tvOS`——請訪問[聲明屬性](../chapter3/06_Attributes.html)來獲取完整列表。除了指定像 iOS 8的主板本號，我們可以指定像iOS 8.3 以及 macOS 10.10.3的子版本號。

```swift
if #available(platform name version, ..., *) {
    APIs 可用，語句將執行
} else {
    APIs 不可用，語句將不執行
}
```

