# 枚舉（Enumerations）
---

> 1.0
> 翻譯：[yankuangshi](https://github.com/yankuangshi)
> 校對：[shinyzhu](https://github.com/shinyzhu)

> 2.0
> 翻譯+校對：[futantan](https://github.com/futantan)

> 2.1
> 翻譯：[Channe](https://github.com/Channe)
> 校對：[shanks](http://codebuild.me)

> 2.2
> 翻譯+校對：[SketchK](https://github.com/SketchK) 2016-05-13


> 3.0
> 翻譯+校對：[shanks](https://codebuild.me) 2016-09-24   
> 3.0.1，shanks，2016-11-12

本頁內容包含：

- [枚舉語法](#enumeration_syntax)
- [使用 Switch 語句匹配枚舉值](#matching_enumeration_values_with_a_switch_statement)
- [關聯值](#associated_values)
- [原始值](#raw_values)
- [遞歸枚舉](#recursive_enumerations)

*枚舉*為一組相關的值定義了一個共同的類型，使你可以在你的代碼中以類型安全的方式來使用這些值。

如果你熟悉 C 語言，你會知道在 C 語言中，枚舉會為一組整型值分配相關聯的名稱。Swift 中的枚舉更加靈活，不必給每一個枚舉成員提供一個值。如果給枚舉成員提供一個值（稱為「原始」值），則該值的類型可以是字符串，字符，或是一個整型值或浮點數。

此外，枚舉成員可以指定*任意*類型的關聯值存儲到枚舉成員中，就像其他語言中的聯合體（unions）和變體（variants）。你可以在一個枚舉中定義一組相關的枚舉成員，每一個枚舉成員都可以有適當類型的關聯值。

在 Swift 中，枚舉類型是一等（first-class）類型。它們采用了很多在傳統上只被類（class）所支持的特性，例如計算屬性（computed properties），用於提供枚舉值的附加信息，實例方法（instance methods），用於提供和枚舉值相關聯的功能。枚舉也可以定義構造函數（initializers）來提供一個初始值；可以在原始實現的基礎上擴展它們的功能；還可以遵循協議（protocols）來提供標准的功能。

想了解更多相關信息，請參見[屬性](./10_Properties.html)，[方法](./11_Methods.html)，[構造過程](./14_Initialization.html)，[擴展](./21_Extensions.html)和[協議](./22_Protocols.html)。

<a name="enumeration_syntax"></a>
## 枚舉語法

使用`enum`關鍵詞來創建枚舉並且把它們的整個定義放在一對大括號內：

```swift
enum SomeEnumeration {
	// 枚舉定義放在這裡
}
```

下面是用枚舉表示指南針四個方向的例子：

```swift
enum CompassPoint {
	case north
	case south
	case east
	case west
}
```

枚舉中定義的值（如 `north `，`south`，`east`和`west`）是這個枚舉的*成員值*（或*成員*）。你可以使用`case`關鍵字來定義一個新的枚舉成員值。

> 注意   
> 與 C 和 Objective-C 不同，Swift 的枚舉成員在被創建時不會被賦予一個默認的整型值。在上面的`CompassPoint`例子中，`north`，`south`，`east`和`west`不會被隱式地賦值為`0`，`1`，`2`和`3`。相反，這些枚舉成員本身就是完備的值，這些值的類型是已經明確定義好的`CompassPoint`類型。

多個成員值可以出現在同一行上，用逗號隔開：

```swift
enum Planet {
	case mercury, venus, earth, mars, jupiter, saturn, uranus, neptune
}
```

每個枚舉定義了一個全新的類型。像 Swift 中其他類型一樣，它們的名字（例如`CompassPoint`和`Planet`）應該以一個大寫字母開頭。給枚舉類型起一個單數名字而不是復數名字，以便於讀起來更加容易理解：

```swift
var directionToHead = CompassPoint.west
```

`directionToHead`的類型可以在它被`CompassPoint`的某個值初始化時推斷出來。一旦`directionToHead`被聲明為`CompassPoint`類型，你可以使用更簡短的點語法將其設置為另一個`CompassPoint`的值：

```swift
directionToHead = .east
```

當`directionToHead`的類型已知時，再次為其賦值可以省略枚舉類型名。在使用具有顯式類型的枚舉值時，這種寫法讓代碼具有更好的可讀性。

<a name="matching_enumeration_values_with_a_switch_statement"></a>
## 使用 Switch 語句匹配枚舉值

你可以使用`switch`語句匹配單個枚舉值：

```swift
directionToHead = .south
switch directionToHead {
	case .north:
	    print("Lots of planets have a north")
	case .south:
	    print("Watch out for penguins")
	case .east:
	    print("Where the sun rises")
	case .west:
	    print("Where the skies are blue")
}
// 打印 "Watch out for penguins」
```

你可以這樣理解這段代碼：

「判斷`directionToHead`的值。當它等於`.north`，打印`「Lots of planets have a north」`。當它等於`.south`，打印`「Watch out for penguins」`。」

……以此類推。

正如在[控制流](./05_Control_Flow.html)中介紹的那樣，在判斷一個枚舉類型的值時，`switch`語句必須窮舉所有情況。如果忽略了`.west`這種情況，上面那段代碼將無法通過編譯，因為它沒有考慮到`CompassPoint`的全部成員。強制窮舉確保了枚舉成員不會被意外遺漏。

當不需要匹配每個枚舉成員的時候，你可以提供一個`default`分支來涵蓋所有未明確處理的枚舉成員：

```swift
let somePlanet = Planet.earth
switch somePlanet {
case .earth:
    print("Mostly harmless")
default:
    print("Not a safe place for humans")
}
// 打印 "Mostly harmless」
```

<a name="associated_values"></a>
## 關聯值

上一小節的例子演示了如何定義和分類枚舉的成員。你可以為`Planet.earth`設置一個常量或者變量，並在賦值之後查看這個值。然而，有時候能夠把其他類型的*關聯值*和成員值一起存儲起來會很有用。這能讓你連同成員值一起存儲額外的自定義信息，並且你每次在代碼中使用該枚舉成員時，還可以修改這個關聯值。

你可以定義 Swift 枚舉來存儲任意類型的關聯值，如果需要的話，每個枚舉成員的關聯值類型可以各不相同。枚舉的這種特性跟其他語言中的可識別聯合（discriminated unions），標簽聯合（tagged unions），或者變體（variants）相似。

例如，假設一個庫存跟蹤系統需要利用兩種不同類型的條形碼來跟蹤商品。有些商品上標有使用`0`到`9`的數字的 UPC 格式的一維條形碼。每一個條形碼都有一個代表「數字系統」的數字，該數字後接五位代表「廠商代碼」的數字，接下來是五位代表「產品代碼」的數字。最後一個數字是「檢查」位，用來驗證代碼是否被正確掃描：

<img width="252" height="120" alt="" src="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/barcode_UPC_2x.png">

其他商品上標有 QR 碼格式的二維碼，它可以使用任何 ISO 8859-1 字符，並且可以編碼一個最多擁有 2,953 個字符的字符串：

<img width="169" height="169" alt="" src="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/barcode_QR_2x.png">

這便於庫存跟蹤系統用包含四個整型值的元組存儲 UPC 碼，以及用任意長度的字符串儲存 QR 碼。

在 Swift 中，使用如下方式定義表示兩種商品條形碼的枚舉：

```swift
enum Barcode {
	case upc(Int, Int, Int, Int)
	case qrCode(String)
}
```

以上代碼可以這麼理解：

「定義一個名為`Barcode`的枚舉類型，它的一個成員值是具有`(Int，Int，Int，Int)`類型關聯值的`upc`，另一個成員值是具有`String`類型關聯值的`qrCode`。」

這個定義不提供任何`Int`或`String`類型的關聯值，它只是定義了，當`Barcode`常量和變量等於`Barcode.upc`或`Barcode.qrCode`時，可以存儲的關聯值的類型。

然後可以使用任意一種條形碼類型創建新的條形碼，例如：

```swift
var productBarcode = Barcode.upc(8, 85909, 51226, 3)
```

上面的例子創建了一個名為`productBarcode`的變量，並將`Barcode.upc`賦值給它，關聯的元組值為`(8, 85909, 51226, 3)`。

同一個商品可以被分配一個不同類型的條形碼，例如：

```swift
productBarcode = .qrCode("ABCDEFGHIJKLMNOP")
```

這時，原始的`Barcode.upc`和其整數關聯值被新的`Barcode.qrCode`和其字符串關聯值所替代。`Barcode`類型的常量和變量可以存儲一個`.upc`或者一個`.qrCode`（連同它們的關聯值），但是在同一時間只能存儲這兩個值中的一個。

像先前那樣，可以使用一個 switch 語句來檢查不同的條形碼類型。然而，這一次，關聯值可以被提取出來作為 switch 語句的一部分。你可以在`switch`的 case 分支代碼中提取每個關聯值作為一個常量（用`let`前綴）或者作為一個變量（用`var`前綴）來使用：

```swift
switch productBarcode {
case .upc(let numberSystem, let manufacturer, let product, let check):
    print("UPC: \(numberSystem), \(manufacturer), \(product), \(check).")
case .qrCode(let productCode):
    print("QR code: \(productCode).")
}
// 打印 "QR code: ABCDEFGHIJKLMNOP."
```

如果一個枚舉成員的所有關聯值都被提取為常量，或者都被提取為變量，為了簡潔，你可以只在成員名稱前標注一個`let`或者`var`：

```swift
switch productBarcode {
case let .upc(numberSystem, manufacturer, product, check):
    print("UPC: \(numberSystem), \(manufacturer), \(product), \(check).")
case let .qrCode(productCode):
    print("QR code: \(productCode).")
}
// 輸出 "QR code: ABCDEFGHIJKLMNOP."
```

<a name="raw_values"></a>
## 原始值

在[關聯值](#associated_values)小節的條形碼例子中，演示了如何聲明存儲不同類型關聯值的枚舉成員。作為關聯值的替代選擇，枚舉成員可以被默認值（稱為*原始值*）預填充，這些原始值的類型必須相同。

這是一個使用 ASCII 碼作為原始值的枚舉：

```swift
enum ASCIIControlCharacter: Character {
    case tab = "\t"
    case lineFeed = "\n"
    case carriageReturn = "\r"
}
```

枚舉類型`ASCIIControlCharacter`的原始值類型被定義為`Character`，並設置了一些比較常見的 ASCII 控制字符。`Character`的描述詳見[字符串和字符](./03_Strings_and_Characters.html)部分。


原始值可以是字符串，字符，或者任意整型值或浮點型值。每個原始值在枚舉聲明中必須是唯一的。

> 注意    
> 原始值和關聯值是不同的。原始值是在定義枚舉時被預先填充的值，像上述三個 ASCII 碼。對於一個特定的枚舉成員，它的原始值始終不變。關聯值是創建一個基於枚舉成員的常量或變量時才設置的值，枚舉成員的關聯值可以變化。

<a name="implicitly_assigned_raw_values"></a>
### 原始值的隱式賦值

在使用原始值為整數或者字符串類型的枚舉時，不需要顯式地為每一個枚舉成員設置原始值，Swift 將會自動為你賦值。

例如，當使用整數作為原始值時，隱式賦值的值依次遞增`1`。如果第一個枚舉成員沒有設置原始值，其原始值將為`0`。

下面的枚舉是對之前`Planet`這個枚舉的一個細化，利用整型的原始值來表示每個行星在太陽系中的順序：

```swift
enum Planet: Int {
    case mercury = 1, venus, earth, mars, jupiter, saturn, uranus, neptune
}
```

在上面的例子中，`Plant.mercury`的顯式原始值為`1`，`Planet.venus`的隱式原始值為`2`，依次類推。

當使用字符串作為枚舉類型的原始值時，每個枚舉成員的隱式原始值為該枚舉成員的名稱。

下面的例子是`CompassPoint`枚舉的細化，使用字符串類型的原始值來表示各個方向的名稱：

```swift
enum CompassPoint: String {
    case north, south, east, west
}
```

上面例子中，`CompassPoint.south`擁有隱式原始值`south`，依次類推。

使用枚舉成員的`rawValue`屬性可以訪問該枚舉成員的原始值：

```swift
let earthsOrder = Planet.earth.rawValue
// earthsOrder 值為 3

let sunsetDirection = CompassPoint.west.rawValue
// sunsetDirection 值為 "west"
```

<a name="initializing_from_a_raw_value"></a>
### 使用原始值初始化枚舉實例
如果在定義枚舉類型的時候使用了原始值，那麼將會自動獲得一個初始化方法，這個方法接收一個叫做`rawValue`的參數，參數類型即為原始值類型，返回值則是枚舉成員或`nil`。你可以使用這個初始化方法來創建一個新的枚舉實例。

這個例子利用原始值`7`創建了枚舉成員`uranus`：

```swift
let possiblePlanet = Planet(rawValue: 7)
// possiblePlanet 類型為 Planet? 值為 Planet.uranus
```

然而，並非所有`Int`值都可以找到一個匹配的行星。因此，原始值構造器總是返回一個*可選*的枚舉成員。在上面的例子中，`possiblePlanet`是`Planet?`類型，或者說「可選的`Planet`」。

> 注意  
> 原始值構造器是一個可失敗構造器，因為並不是每一個原始值都有與之對應的枚舉成員。更多信息請參見[可失敗構造器](../chapter3/05_Declarations.html#failable_initializers)

如果你試圖尋找一個位置為`11`的行星，通過原始值構造器返回的可選`Planet`值將是`nil`：

```swift
let positionToFind = 11
if let somePlanet = Planet(rawValue: positionToFind) {
    switch somePlanet {
    case .earth:
        print("Mostly harmless")
    default:
        print("Not a safe place for humans")
    }
} else {
    print("There isn't a planet at position \(positionToFind)")
}
// 輸出 "There isn't a planet at position 11
```

這個例子使用了可選綁定（optional binding），試圖通過原始值`11`來訪問一個行星。`if let somePlanet = Planet(rawValue: 11)`語句創建了一個可選`Planet`，如果可選`Planet`的值存在，就會賦值給`somePlanet`。在這個例子中，無法檢索到位置為`11`的行星，所以`else`分支被執行。

<a name="recursive_enumerations"></a>
## 遞歸枚舉


*遞歸枚舉*是一種枚舉類型，它有一個或多個枚舉成員使用該枚舉類型的實例作為關聯值。使用遞歸枚舉時，編譯器會插入一個間接層。你可以在枚舉成員前加上`indirect`來表示該成員可遞歸。  

例如，下面的例子中，枚舉類型存儲了簡單的算術表達式：

```swift
enum ArithmeticExpression {
    case number(Int)
    indirect case addition(ArithmeticExpression, ArithmeticExpression)
    indirect case multiplication(ArithmeticExpression, ArithmeticExpression)
}
```

你也可以在枚舉類型開頭加上`indirect`關鍵字來表明它的所有成員都是可遞歸的：

```swift
indirect enum ArithmeticExpression {
    case number(Int)
    case addition(ArithmeticExpression, ArithmeticExpression)
    case multiplication(ArithmeticExpression, ArithmeticExpression)
}
```

上面定義的枚舉類型可以存儲三種算術表達式：純數字、兩個表達式相加、兩個表達式相乘。枚舉成員`addition`和`multiplication`的關聯值也是算術表達式——這些關聯值使得嵌套表達式成為可能。例如，表達式`(5 + 4) * 2`，乘號右邊是一個數字，左邊則是另一個表達式。因為數據是嵌套的，因而用來存儲數據的枚舉類型也需要支持這種嵌套——這意味著枚舉類型需要支持遞歸。下面的代碼展示了使用`ArithmeticExpression `這個遞歸枚舉創建表達式`(5 + 4) * 2`

```swift
let five = ArithmeticExpression.number(5)
let four = ArithmeticExpression.number(4)
let sum = ArithmeticExpression.addition(five, four)
let product = ArithmeticExpression.multiplication(sum, ArithmeticExpression.number(2))
```

要操作具有遞歸性質的數據結構，使用遞歸函數是一種直截了當的方式。例如，下面是一個對算術表達式求值的函數：

```swift
func evaluate(_ expression: ArithmeticExpression) -> Int {
    switch expression {
    case let .number(value):
        return value
    case let .addition(left, right):
        return evaluate(left) + evaluate(right)
    case let .multiplication(left, right):
        return evaluate(left) * evaluate(right)
    }
}

print(evaluate(product))
// 打印 "18"
```

該函數如果遇到純數字，就直接返回該數字的值。如果遇到的是加法或乘法運算，則分別計算左邊表達式和右邊表達式的值，然後相加或相乘。
