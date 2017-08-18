# Swift 初見（A Swift Tour）

---

> 1.0
> 翻譯：[numbbbbb](https://github.com/numbbbbb)
> 校對：[shinyzhu](https://github.com/shinyzhu), [stanzhai](https://github.com/stanzhai)

> 2.0
> 翻譯+校對：[xtymichael](https://github.com/xtymichael)

> 2.2
> 翻譯：[175](https://github.com/Brian175)，2016-04-09 校對：[SketchK](https://github.com/SketchK)，2016-05-11
> 
> 3.0
> 翻譯+校對：[shanks](http://codebuild.me)，2016-10-06

> 3.0.1 review: 2016-11-09
> 
> 3.1 校對: [SketchK](https://github.com/SketchK) 2017-04-08


本頁內容包括：

-   [簡單值（Simple Values）](#simple_values)
-   [控制流（Control Flow）](#control_flow)
-   [函數和閉包（Functions and Closures）](#functions_and_closures)
-   [對象和類（Objects and Classes）](#objects_and_classes)
-   [枚舉和結構體（Enumerations and Structures）](#enumerations_and_structures)
-   [協議和擴展（Protocols and Extensions）](#protocols_and_extensions)
-   [錯誤處理（Error Handling）](#error_handling)
-   [泛型（Generics）](#generics)

通常來說，編程語言教程中的第一個程序應該在屏幕上打印 「Hello, world」。在 Swift 中，可以用一行代碼實現：

```swift
print("Hello, world!")
```

如果你寫過 C 或者 Objective-C 代碼，那你應該很熟悉這種形式——在 Swift 中，這行代碼就是一個完整的程序。你不需要為了輸入輸出或者字符串處理導入一個單獨的庫。全局作用域中的代碼會被自動當做程序的入口點，所以你也不需要 `main()` 函數。你同樣不需要在每個語句結尾寫上分號。

這個教程會通過一系列編程例子來讓你對 Swift 有初步了解，如果你有什麼不理解的地方也不用擔心——任何本章介紹的內容都會在後面的章節中詳細講解到。

> 注意：  
> 在 Mac 中下載 Playground 文件並用雙擊的方式在 Xcode 中打開：[https://developer.apple.com/go/?id=swift-tour](https://developer.apple.com/go/?id=swift-tour)

<a name="simple_values"></a>
## 簡單值

使用 `let` 來聲明常量，使用 `var` 來聲明變量。一個常量的值，在編譯的時候，並不需要有明確的值，但是你只能為它賦值一次。這說明你可以用一個常量來命名一個值，一次賦值就即可在多個地方使用。

```swift
var myVariable = 42
myVariable = 50
let myConstant = 42
```

常量或者變量的類型必須和你賦給它們的值一樣。然而，你不用明確地聲明類型，聲明的同時賦值的話，編譯器會自動推斷類型。在上面的例子中，編譯器推斷出 `myVariable` 是一個整數類型（integer）因為它的初始值是整數。

如果初始值沒有提供足夠的信息（或者沒有初始值），那你需要在變量後面聲明類型，用冒號分割。

```swift
let implicitInteger = 70
let implicitDouble = 70.0
let explicitDouble: Double = 70
```

> 練習：  
> 創建一個常量，顯式指定類型為 `Float` 並指定初始值為 4。

值永遠不會被隱式轉換為其他類型。如果你需要把一個值轉換成其他類型，請顯式轉換。

```swift
let label = "The width is"
let width = 94
let widthLabel = label + String(width)
```
> 練習：  
> 刪除最後一行中的 `String`，錯誤提示是什麼？

有一種更簡單的把值轉換成字符串的方法：把值寫到括號中，並且在括號之前寫一個反斜杠。例如：

```swift
let apples = 3
let oranges = 5
let appleSummary = "I have \(apples) apples."
let fruitSummary = "I have \(apples + oranges) pieces of fruit."
```

> 練習：  
> 使用 `\()` 來把一個浮點計算轉換成字符串，並加上某人的名字，和他打個招呼。

使用方括號 `[]` 來創建數組和字典，並使用下標或者鍵（key）來訪問元素。最後一個元素後面允許有個逗號。

```swift
var shoppingList = ["catfish", "water", "tulips", "blue paint"]
shoppingList[1] = "bottle of water"

var occupations = [
    "Malcolm": "Captain",
    "Kaylee": "Mechanic",
]
occupations["Jayne"] = "Public Relations"
```

要創建一個空數組或者字典，使用初始化語法。

```swift
let emptyArray = [String]()
let emptyDictionary = [String: Float]()
```

如果類型信息可以被推斷出來，你可以用 `[]` 和 `[:]` 來創建空數組和空字典——就像你聲明變量或者給函數傳參數的時候一樣。

```swift
shoppingList = []
occupations = [:]
```

<a name="control_flow"></a>
## 控制流

使用 `if` 和 `switch` 來進行條件操作，使用 `for-in`、 `for`、 `while` 和 `repeat-while` 來進行循環。包裹條件和循環變量括號可以省略，但是語句體的大括號是必須的。

```swift
let individualScores = [75, 43, 103, 87, 12]
var teamScore = 0
for score in individualScores {
    if score > 50 {
        teamScore += 3
    } else {
        teamScore += 1
    }
}
print(teamScore)
```

在 `if` 語句中，條件必須是一個布爾表達式——這意味著像 `if score { ... }` 這樣的代碼將報錯，而不會隱形地與 0 做對比。

你可以一起使用 `if` 和 `let` 來處理值缺失的情況。這些值可由可選值來代表。一個可選的值是一個具體的值或者是 `nil` 以表示值缺失。在類型後面加一個問號來標記這個變量的值是可選的。

```swift
var optionalString: String? = "Hello"
print(optionalString == nil)

var optionalName: String? = "John Appleseed"
var greeting = "Hello!"
if let name = optionalName {
    greeting = "Hello, \(name)"
}
```

> 練習：  
> 把 `optionalName` 改成 `nil`，greeting會是什麼？添加一個 `else` 語句，當 `optionalName` 是 `nil` 時給 greeting 賦一個不同的值。

如果變量的可選值是 `nil`，條件會判斷為 `false`，大括號中的代碼會被跳過。如果不是 `nil`，會將值解包並賦給 `let` 後面的常量，這樣代碼塊中就可以使用這個值了。  
另一種處理可選值的方法是通過使用 `??` 操作符來提供一個默認值。如果可選值缺失的話，可以使用默認值來代替。   

```swift
let nickName: String? = nil
let fullName: String = "John Appleseed"
let informalGreeting = "Hi \(nickName ?? fullName)"
```

`switch` 支持任意類型的數據以及各種比較操作——不僅僅是整數以及測試相等。

```swift
let vegetable = "red pepper"
switch vegetable {
case "celery":
    print("Add some raisins and make ants on a log.")
case "cucumber", "watercress":
    print("That would make a good tea sandwich.")
case let x where x.hasSuffix("pepper"):
    print("Is it a spicy \(x)?")
default:
    print("Everything tastes good in soup.")
}
```

> 練習：  
> 刪除 `default` 語句，看看會有什麼錯誤？

注意 `let` 在上述例子的等式中是如何使用的，它將匹配等式的值賦給常量 `x`。

運行 `switch` 中匹配到的子句之後，程序會退出 `switch` 語句，並不會繼續向下運行，所以不需要在每個子句結尾寫 `break`。

你可以使用 `for-in` 來遍歷字典，需要兩個變量來表示每個鍵值對。字典是一個無序的集合，所以他們的鍵和值以任意順序迭代結束。

```swift
let interestingNumbers = [
    "Prime": [2, 3, 5, 7, 11, 13],
    "Fibonacci": [1, 1, 2, 3, 5, 8],
    "Square": [1, 4, 9, 16, 25],
]
var largest = 0
for (kind, numbers) in interestingNumbers {
    for number in numbers {
        if number > largest {
            largest = number
        }
    }
}
print(largest)
```

> 練習：  
> 添加另一個變量來記錄最大數字的種類(kind)，同時仍然記錄這個最大數字的值。

使用 `while` 來重復運行一段代碼直到不滿足條件。循環條件也可以在結尾，保證能至少循環一次。

```swift
var n = 2
while n < 100 {
    n = n * 2
}
print(n)

var m = 2
repeat {
    m = m * 2
} while m < 100
print(m)
```

你可以在循環中使用 `..<` 來表示范圍。

```swift
var total = 0
for i in 0..<4 {
	total += i
}
print(total)
```

使用 `..<` 創建的范圍不包含上界，如果想包含的話需要使用 `...`。

<a name="functions_and_closures"></a>
## 函數和閉包

使用 `func` 來聲明一個函數，使用名字和參數來調用函數。使用 `->` 來指定函數返回值的類型。

```swift
func greet(person: String, day: String) -> String {
    return "Hello \(person), today is \(day)."
}
greet(person:"Bob", day: "Tuesday")
```

> 練習：  
> 刪除 `day` 參數，添加一個參數來表示今天吃了什麼午飯。

默認情況下，函數使用它們的參數名稱作為它們參數的標簽，在參數名稱前可以自定義參數標簽，或者使用 `_` 表示不使用參數標簽。

```swift
func greet(_ person: String, on day: String) -> String {
    return "Hello \(person), today is \(day)."
}
greet("John", on: "Wednesday")
```

使用元組來讓一個函數返回多個值。該元組的元素可以用名稱或數字來表示。

```swift
func calculateStatistics(scores: [Int]) -> (min: Int, max: Int, sum: Int) {
    var min = scores[0]
    var max = scores[0]
    var sum = 0

    for score in scores {
        if score > max {
            max = score
        } else if score < min {
            min = score
        }
        sum += score
    }

    return (min, max, sum)
}
let statistics = calculateStatistics(scores:[5, 3, 100, 3, 9])
print(statistics.sum)
print(statistics.2)
```

函數可以帶有可變個數的參數，這些參數在函數內表現為數組的形式：

```swift
func sumOf(numbers: Int...) -> Int {
    var sum = 0
    for number in numbers {
        sum += number
    }
    return sum
}
sumOf()
sumOf(numbers: 42, 597, 12)
```

> 練習：  
> 寫一個計算參數平均值的函數。

函數可以嵌套。被嵌套的函數可以訪問外側函數的變量，你可以使用嵌套函數來重構一個太長或者太復雜的函數。

```swift
func returnFifteen() -> Int {
    var y = 10
    func add() {
        y += 5
    }
    add()
    return y
}
returnFifteen()
```

函數是第一等類型，這意味著函數可以作為另一個函數的返回值。

```swift
func makeIncrementer() -> ((Int) -> Int) {
    func addOne(number: Int) -> Int {
        return 1 + number
    }
    return addOne
}
var increment = makeIncrementer()
increment(7)
```

函數也可以當做參數傳入另一個函數。

```swift
func hasAnyMatches(list: [Int], condition: (Int) -> Bool) -> Bool {
    for item in list {
        if condition(item) {
            return true
        }
    }
    return false
}
func lessThanTen(number: Int) -> Bool {
    return number < 10
}
var numbers = [20, 19, 7, 12]
hasAnyMatches(list: numbers, condition: lessThanTen)
```

函數實際上是一種特殊的閉包:它是一段能之後被調取的代碼。閉包中的代碼能訪問閉包作用域中的變量和函數，即使閉包是在一個不同的作用域被執行的 - 你已經在嵌套函數的例子中看過了。你可以使用 `{}` 來創建一個匿名閉包。使用 `in` 將參數和返回值類型的聲明與閉包函數體進行分離。

```swift
numbers.map({
    (number: Int) -> Int in
    let result = 3 * number
    return result
})
```

> 練習：  
> 重寫閉包，對所有奇數返回 0。

有很多種創建更簡潔的閉包的方法。如果一個閉包的類型已知，比如作為一個代理的回調，你可以忽略參數，返回值，甚至兩個都忽略。單個語句閉包會把它語句的值當做結果返回。

```swift
let mappedNumbers = numbers.map({ number in 3 * number })
print(mappedNumbers)
```

你可以通過參數位置而不是參數名字來引用參數——這個方法在非常短的閉包中非常有用。當一個閉包作為最後一個參數傳給一個函數的時候，它可以直接跟在括號後面。當一個閉包是傳給函數的唯一參數，你可以完全忽略括號。

```swift
let sortedNumbers = numbers.sorted { $0 > $1 }
print(sortedNumbers)
```

<a name="objects_and_classes"></a>
## 對象和類

使用 `class` 和類名來創建一個類。類中屬性的聲明和常量、變量聲明一樣，唯一的區別就是它們的上下文是類。同樣，方法和函數聲明也一樣。

```swift
class Shape {
    var numberOfSides = 0
    func simpleDescription() -> String {
        return "A shape with \(numberOfSides) sides."
    }
}
```

> 練習：  
> 使用 `let` 添加一個常量屬性，再添加一個接收一個參數的方法。

要創建一個類的實例，在類名後面加上括號。使用點語法來訪問實例的屬性和方法。

```swift
var shape = Shape()
shape.numberOfSides = 7
var shapeDescription = shape.simpleDescription()
```

這個版本的 `Shape` 類缺少了一些重要的東西：一個構造函數來初始化類實例。使用 `init` 來創建一個構造器。

```swift
class NamedShape {
    var numberOfSides: Int = 0
    var name: String

    init(name: String) {
        self.name = name
    }

    func simpleDescription() -> String {
        return "A shape with \(numberOfSides) sides."
    }
}
```

注意 `self` 被用來區別實例變量 `name` 和構造器的參數 `name`。當你創建實例的時候，像傳入函數參數一樣給類傳入構造器的參數。每個屬性都需要賦值——無論是通過聲明（就像 `numberOfSides`）還是通過構造器（就像 `name`）。

如果你需要在刪除對象之前進行一些清理工作，使用 `deinit` 創建一個析構函數。

子類的定義方法是在它們的類名後面加上父類的名字，用冒號分割。創建類的時候並不需要一個標准的根類，所以你可以根據需要添加或者忽略父類。

子類如果要重寫父類的方法的話，需要用 `override` 標記——如果沒有添加 `override` 就重寫父類方法的話編譯器會報錯。編譯器同樣會檢測 `override` 標記的方法是否確實在父類中。

```swift
class Square: NamedShape {
    var sideLength: Double

    init(sideLength: Double, name: String) {
        self.sideLength = sideLength
        super.init(name: name)
        numberOfSides = 4
    }

    func area() ->  Double {
        return sideLength * sideLength
    }

    override func simpleDescription() -> String {
        return "A square with sides of length \(sideLength)."
    }
}
let test = Square(sideLength: 5.2, name: "my test square")
test.area()
test.simpleDescription()
```

> 練習：  
> 創建 `NamedShape` 的另一個子類 `Circle`，構造器接收兩個參數，一個是半徑一個是名稱，在子類 `Circle` 中實現 `area()` 和 `simpleDescription()` 方法。

除了儲存簡單的屬性之外，屬性可以有 getter 和 setter 。

```swift
class EquilateralTriangle: NamedShape {
    var sideLength: Double = 0.0

    init(sideLength: Double, name: String) {
        self.sideLength = sideLength
        super.init(name: name)
        numberOfSides = 3
    }

    var perimeter: Double {
        get {
            return 3.0 * sideLength
        }
        set {
            sideLength = newValue / 3.0
        }
    }

    override func simpleDescription() -> String {
        return "An equilateral triangle with sides of length \(sideLength)."
    }
}
var triangle = EquilateralTriangle(sideLength: 3.1, name: "a triangle")
print(triangle.perimeter)
triangle.perimeter = 9.9
print(triangle.sideLength)
```

在 `perimeter` 的 setter 中，新值的名字是 `newValue`。你可以在 `set` 之後顯式的設置一個名字。

注意 `EquilateralTriangle` 類的構造器執行了三步：

1. 設置子類聲明的屬性值
2. 調用父類的構造器
3. 改變父類定義的屬性值。其他的工作比如調用方法、getters 和 setters 也可以在這個階段完成。

如果你不需要計算屬性，但是仍然需要在設置一個新值之前或者之後運行代碼，使用 `willSet` 和 `didSet`。寫入的代碼會在屬性值發生改變時調用，但不包含構造器中發生值改變的情況。比如，下面的類確保三角形的邊長總是和正方形的邊長相同。

```swift
class TriangleAndSquare {
    var triangle: EquilateralTriangle {
        willSet {
            square.sideLength = newValue.sideLength
        }
    }
    var square: Square {
        willSet {
            triangle.sideLength = newValue.sideLength
        }
    }
    init(size: Double, name: String) {
        square = Square(sideLength: size, name: name)
        triangle = EquilateralTriangle(sideLength: size, name: name)
    }
}
var triangleAndSquare = TriangleAndSquare(size: 10, name: "another test shape")
print(triangleAndSquare.square.sideLength)
print(triangleAndSquare.triangle.sideLength)
triangleAndSquare.square = Square(sideLength: 50, name: "larger square")
print(triangleAndSquare.triangle.sideLength)
```

處理變量的可選值時，你可以在操作（比如方法、屬性和子腳本）之前加 `?`。如果 `?` 之前的值是 `nil`，`?` 後面的東西都會被忽略，並且整個表達式返回 `nil`。否則，`?` 之後的東西都會被運行。在這兩種情況下，整個表達式的值也是一個可選值。

```swift
let optionalSquare: Square? = Square(sideLength: 2.5, name: "optional square")
let sideLength = optionalSquare?.sideLength
```

<a name="enumerations_and_structure"></a>
## 枚舉和結構體

使用 `enum` 來創建一個枚舉。就像類和其他所有命名類型一樣，枚舉可以包含方法。

```swift
enum Rank: Int {
    case ace = 1
    case two, three, four, five, six, seven, eight, nine, ten
    case jack, queen, king
    func simpleDescription() -> String {
        switch self {
        case .ace:
            return "ace"
        case .jack:
            return "jack"
        case .queen:
            return "queen"
        case .king:
            return "king"
        default:
            return String(self.rawValue)
        }
    }
}
let ace = Rank.ace
let aceRawValue = ace.rawValue
```

> 練習：  
> 寫一個函數，通過比較它們的原始值來比較兩個 `Rank` 值。

默認情況下，Swift 按照從 0 開始每次加 1 的方式為原始值進行賦值，不過你可以通過顯式賦值進行改變。在上面的例子中，`Ace` 被顯式賦值為 1，並且剩下的原始值會按照順序賦值。你也可以使用字符串或者浮點數作為枚舉的原始值。使用 `rawValue` 屬性來訪問一個枚舉成員的原始值。

使用 `init?(rawValue:)` 初始化構造器來創建一個帶有原始值的枚舉成員。如果存在與原始值相應的枚舉成員就返回該枚舉成員，否則就返回 `nil`。

```swift
if let convertedRank = Rank(rawValue: 3) {
    let threeDescription = convertedRank.simpleDescription()
}
```

枚舉的關聯值是實際值，並不是原始值的另一種表達方法。實際上，如果沒有比較有意義的原始值，你就不需要提供原始值。

```swift
enum Suit {
    case spades, hearts, diamonds, clubs
    func simpleDescription() -> String {
        switch self {
        case .spades:
            return "spades"
        case .hearts:
            return "hearts"
        case .diamonds:
            return "diamonds"
        case .clubs:
            return "clubs"
        }
    }
}
let hearts = Suit.hearts
let heartsDescription = hearts.simpleDescription()
```

> 練習：  
> 給 `Suit` 添加一個 `color()` 方法，對 `spades` 和 `clubs` 返回 「black」 ，對 `hearts` 和 `diamonds` 返回 「red」 。

注意在上面的例子中用了兩種方式引用 `hearts` 枚舉成員：給 `hearts` 常量賦值時，枚舉成員 `Suit.hearts` 需要用全名來引用，因為常量沒有顯式指定類型。在 `switch` 裡，枚舉成員使用縮寫 `.hearts` 來引用，因為 `self` 已經是一個 `suit` 類型，在已知變量類型的情況下可以使用縮寫。

如果枚舉成員的實例有原始值，那麼這些值是在聲明的時候就已經決定了，這意味著不同的枚舉成員總會有一個相同的原始值。當然我們也可以為枚舉成員設定關聯值，關聯值是在創建實例時決定的。這意味著不同的枚舉成員的關聯值都可以不同。你可以把關聯值想象成枚舉成員的寄存屬性。例如，考慮從服務器獲取日出和日落的時間。服務器會返回正常結果或者錯誤信息。

```swift
enum ServerResponse {
    case result(String, String)
    case failure(String)
}
 
let success = ServerResponse.result("6:00 am", "8:09 pm")
let failure = ServerResponse.failure("Out of cheese.")
 
switch success {
case let .result(sunrise, sunset):
    print("Sunrise is at \(sunrise) and sunset is at \(sunset)")
case let .failure(message):
    print("Failure...  \(message)")
}
```

> 練習：  
> 給 `ServerResponse` 和 switch 添加第三種情況。

注意日升和日落時間是如何從 `ServerResponse` 中提取到並與 `switch` 的 `case` 相匹配的。

使用 `struct` 來創建一個結構體。結構體和類有很多相同的地方，比如方法和構造器。它們之間最大的一個區別就是結構體是傳值，類是傳引用。

```swift
struct Card {
    var rank: Rank
    var suit: Suit
    func simpleDescription() -> String {
        return "The \(rank.simpleDescription()) of \(suit.simpleDescription())"
    }
}
let threeOfSpades = Card(rank: .three, suit: .spades)
let threeOfSpadesDescription = threeOfSpades.simpleDescription()
```

> 練習：  
> 給 `Card` 添加一個方法，創建一副完整的撲克牌並把每張牌的 rank 和 suit 對應起來。


<a name="protocols_and_extensions"></a>
## 協議和擴展

使用 `protocol` 來聲明一個協議。

```swift
protocol ExampleProtocol {
    var simpleDescription: String { get }
    mutating func adjust()
}
```

類、枚舉和結構體都可以實現協議。

```swift
class SimpleClass: ExampleProtocol {
    var simpleDescription: String = "A very simple class."
    var anotherProperty: Int = 69105
    func adjust() {
        simpleDescription += "  Now 100% adjusted."
    }
}
var a = SimpleClass()
a.adjust()
let aDescription = a.simpleDescription

struct SimpleStructure: ExampleProtocol {
    var simpleDescription: String = "A simple structure"
    mutating func adjust() {
        simpleDescription += " (adjusted)"
    }
}
var b = SimpleStructure()
b.adjust()
let bDescription = b.simpleDescription
```

> 練習：  
> 寫一個實現這個協議的枚舉。

注意聲明 `SimpleStructure` 時候 `mutating` 關鍵字用來標記一個會修改結構體的方法。`SimpleClass` 的聲明不需要標記任何方法，因為類中的方法通常可以修改類屬性（類的性質）。

使用 `extension` 來為現有的類型添加功能，比如新的方法和計算屬性。你可以使用擴展讓某個在別處聲明的類型來遵守某個協議，這同樣適用於從外部庫或者框架引入的類型。

```swift
extension Int: ExampleProtocol {
    var simpleDescription: String {
        return "The number \(self)"
    }
    mutating func adjust() {
        self += 42
    }
}
print(7.simpleDescription)
```

> 練習：  
> 給 `Double` 類型寫一個擴展，添加 `absoluteValue` 功能。

你可以像使用其他命名類型一樣使用協議名——例如，創建一個有不同類型但是都實現一個協議的對象集合。當你處理類型是協議的值時，協議外定義的方法不可用。

```swift
let protocolValue: ExampleProtocol = a
print(protocolValue.simpleDescription)
// print(protocolValue.anotherProperty)  // 去掉注釋可以看到錯誤
```

即使 `protocolValue` 變量運行時的類型是 `simpleClass` ，編譯器還是會把它的類型當做`ExampleProtocol`。這表示你不能調用在協議之外的方法或者屬性。

<a name="error_handling"></a>
## 錯誤處理

使用采用 `Error` 協議的類型來表示錯誤。

```swift
enum PrinterError: Error {
	case outOfPaper
	case noToner
	case onFire
}
```

使用 `throw` 來拋出一個錯誤並使用 `throws` 來表示一個可以拋出錯誤的函數。如果在函數中拋出一個錯誤，這個函數會立刻返回並且調用該函數的代碼會進行錯誤處理。

```swift
func send(job: Int, toPrinter printerName: String) throws -> String {
    if printerName == "Never Has Toner" {
        throw PrinterError.noToner
    }
    return "Job sent"
}
```

有多種方式可以用來進行錯誤處理。一種方式是使用 `do-catch` 。在 `do` 代碼塊中，使用 `try` 來標記可以拋出錯誤的代碼。在 `catch` 代碼塊中，除非你另外命名，否則錯誤會自動命名為 `error` 。

```swift
do {
    let printerResponse = try send(job: 1040, toPrinter: "Bi Sheng")
    print(printerResponse)
} catch {
    print(error)
}
```

> 練習：  
> 將 printer name 改為 `"Never Has Toner"` 使 `send(job:toPrinter:)` 函數拋出錯誤。

可以使用多個 `catch` 塊來處理特定的錯誤。參照 switch 中的 `case` 風格來寫 `catch`。

```swift
do {
    let printerResponse = try send(job: 1440, toPrinter: "Gutenberg")
    print(printerResponse)
} catch PrinterError.onFire {
    print("I'll just put this over here, with the rest of the fire.")
} catch let printerError as PrinterError {
    print("Printer error: \(printerError).")
} catch {
    print(error)
}
```

> 練習：  
> 在 `do` 代碼塊中添加拋出錯誤的代碼。你需要拋出哪種錯誤來使第一個 `catch` 塊進行接收？怎麼使第二個和第三個 `catch` 進行接收呢？

另一種處理錯誤的方式使用 `try?` 將結果轉換為可選的。如果函數拋出錯誤，該錯誤會被拋棄並且結果為 `nil`。否則的話，結果會是一個包含函數返回值的可選值。

```swift
let printerSuccess = try? send(job: 1884, toPrinter: "Mergenthaler")
let printerFailure = try? send(job: 1885, toPrinter: "Never Has Toner")
```

使用 `defer` 代碼塊來表示在函數返回前，函數中最後執行的代碼。無論函數是否會拋出錯誤，這段代碼都將執行。使用 `defer`，可以把函數調用之初就要執行的代碼和函數調用結束時的掃尾代碼寫在一起，雖然這兩者的執行時機截然不同。

```swift
var fridgeIsOpen = false
let fridgeContent = ["milk", "eggs", "leftovers"]

func fridgeContains(_ food: String) -> Bool {
	fridgeIsOpen = true
	defer {
		fridgeIsOpen = false
	}
	
	let result = fridgeContent.contains(food)
	return result
}
fridgeContains("banana")
print(fridgeIsOpen)
```

<a name="generics"></a>
## 泛型

在尖括號裡寫一個名字來創建一個泛型函數或者類型。

```swift
func repeatItem<Item>(repeating item: Item, numberOfTimes: Int) -> [Item] {
    var result = [Item]()
    for _ in 0..<numberOfTimes {
        result.append(item)
    }
    return result
}
repeatItem(repeating: "knock", numberOfTimes:4)
```

你也可以創建泛型函數、方法、類、枚舉和結構體。

```swift
// 重新實現 Swift 標准庫中的可選類型
enum OptionalValue<Wrapped> {
    case None
    case Some(Wrapped)
}
var possibleInteger: OptionalValue<Int> = .None
possibleInteger = .Some(100)
```

在類型名後面使用 `where` 來指定對類型的需求，比如，限定類型實現某一個協議，限定兩個類型是相同的，或者限定某個類必須有一個特定的父類。

```swift
func anyCommonElements<T: Sequence, U: Sequence>(_ lhs: T, _ rhs: U) -> Bool
    where T.Iterator.Element: Equatable, T.Iterator.Element == U.Iterator.Element {
        for lhsItem in lhs {
            for rhsItem in rhs {
                if lhsItem == rhsItem {
                    return true
                }
            }
        }
        return false
}
anyCommonElements([1, 2, 3], [3])
```

> 練習：  
> 修改 `anyCommonElements(_:_:)` 函數來創建一個函數，返回一個數組，內容是兩個序列的共有元素。

`<T: Equatable>` 和 `<T> ... where T: Equatable>` 的寫法是等價的。
