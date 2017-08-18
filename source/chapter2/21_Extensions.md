# 擴展（Extensions）
----

> 1.0
> 翻譯：[lyuka](https://github.com/lyuka)
> 校對：[Hawstein](https://github.com/Hawstein)

> 2.0
> 翻譯+校對：[shanks](http://codebuild.me)

> 2.1
> 校對：[shanks](http://codebuild.me)
> 
> 2.2
> 翻譯+校對：[SketchK](https://github.com/SketchK) 2016-05-16   
> 3.0.1，shanks，2016-11-13

本頁包含內容：

- [擴展語法](#extension_syntax)
- [計算型屬性](#computed_properties)
- [構造器](#initializers)
- [方法](#methods)
- [下標](#subscripts)
- [嵌套類型](#nested_types)

*擴展* 就是為一個已有的類、結構體、枚舉類型或者協議類型添加新功能。這包括在沒有權限獲取原始源代碼的情況下擴展類型的能力（即 *逆向建模* ）。擴展和 Objective-C 中的分類類似。（與 Objective-C 不同的是，Swift 的擴展沒有名字。）

Swift 中的擴展可以：

- 添加計算型屬性和計算型類型屬性
- 定義實例方法和類型方法
- 提供新的構造器
- 定義下標
- 定義和使用新的嵌套類型
- 使一個已有類型符合某個協議

在 Swift 中，你甚至可以對協議進行擴展，提供協議要求的實現，或者添加額外的功能，從而可以讓符合協議的類型擁有這些功能。你可以從[協議擴展](./22_Protocols.html#protocol_extensions)獲取更多的細節。

> 注意  
擴展可以為一個類型添加新的功能，但是不能重寫已有的功能。

<a name="extension_syntax"></a>
## 擴展語法

使用關鍵字 `extension` 來聲明擴展：

```swift
extension SomeType {
    // 為 SomeType 添加的新功能寫到這裡
}
```

可以通過擴展來擴展一個已有類型，使其采納一個或多個協議。在這種情況下，無論是類還是結構體，協議名字的書寫方式完全一樣：

```swift
extension SomeType: SomeProtocol, AnotherProctocol {
    // 協議實現寫到這裡
}
```

通過這種方式添加協議一致性的詳細描述請參閱[利用擴展添加協議一致性](./22_Protocols.html#adding_protocol_conformance_with_an_extension)。

> 注意  
如果你通過擴展為一個已有類型添加新功能，那麼新功能對該類型的所有已有實例都是可用的，即使它們是在這個擴展定義之前創建的。

<a name="computed_properties"></a>
## 計算型屬性

擴展可以為已有類型添加計算型實例屬性和計算型類型屬性。下面的例子為 Swift 的內建 `Double` 類型添加了五個計算型實例屬性，從而提供與距離單位協作的基本支持：

```swift
extension Double {
    var km: Double { return self * 1_000.0 }
    var m : Double { return self }
    var cm: Double { return self / 100.0 }
    var mm: Double { return self / 1_000.0 }
    var ft: Double { return self / 3.28084 }
}
let oneInch = 25.4.mm
print("One inch is \(oneInch) meters")
// 打印 「One inch is 0.0254 meters」
let threeFeet = 3.ft
print("Three feet is \(threeFeet) meters")
// 打印 「Three feet is 0.914399970739201 meters」
```

這些計算型屬性表達的含義是把一個 `Double` 值看作是某單位下的長度值。即使它們被實現為計算型屬性，但這些屬性的名字仍可緊接一個浮點型字面值，從而通過點語法來使用，並以此實現距離轉換。

在上述例子中，`Double` 值 `1.0` 用來表示「1米」。這就是為什麼計算型屬性 `m` 返回 `self`，即表達式 `1.m` 被認為是計算 `Double` 值 `1.0`。

其它單位則需要一些單位換算。一千米等於 1,000 米，所以計算型屬性 `km` 要把值乘以 `1_000.00` 來實現千米到米的單位換算。類似地，一米有 3.28024 英尺，所以計算型屬性 `ft` 要把對應的 `Double` 值除以 `3.28024` 來實現英尺到米的單位換算。

這些屬性是只讀的計算型屬性，為了更簡潔，省略了 `get` 關鍵字。它們的返回值是 `Double`，而且可以用於所有接受 `Double` 值的數學計算中：

```swift
let aMarathon = 42.km + 195.m
print("A marathon is \(aMarathon) meters long")
// 打印 「A marathon is 42195.0 meters long」
```

> 注意  
擴展可以添加新的計算型屬性，但是不可以添加存儲型屬性，也不可以為已有屬性添加屬性觀察器。

<a name="initializers"></a>
## 構造器

擴展可以為已有類型添加新的構造器。這可以讓你擴展其它類型，將你自己的定制類型作為其構造器參數，或者提供該類型的原始實現中未提供的額外初始化選項。  

擴展能為類添加新的便利構造器，但是它們不能為類添加新的指定構造器或析構器。指定構造器和析構器必須總是由原始的類實現來提供。

> 注意  
如果你使用擴展為一個值類型添加構造器，同時該值類型的原始實現中未定義任何定制的構造器且所有存儲屬性提供了默認值，那麼我們就可以在擴展中的構造器裡調用默認構造器和逐一成員構造器。  
正如在[值類型的構造器代理](./14_Initialization.html#initializer_delegation_for_value_types)中描述的，如果你把定制的構造器寫在值類型的原始實現中，上述規則將不再適用。

下面的例子定義了一個用於描述幾何矩形的結構體 `Rect`。這個例子同時定義了兩個輔助結構體 `Size` 和 `Point`，它們都把 `0.0` 作為所有屬性的默認值：

```swift
struct Size {
    var width = 0.0, height = 0.0
}
struct Point {
    var x = 0.0, y = 0.0
}
struct Rect {
    var origin = Point()
    var size = Size()
}
```
因為結構體 `Rect` 未提供定制的構造器，因此它會獲得一個逐一成員構造器。又因為它為所有存儲型屬性提供了默認值，它又會獲得一個默認構造器。詳情請參閱[默認構造器](./14_Initialization.html#default_initializers)。這些構造器可以用於構造新的 `Rect` 實例：

```swift
let defaultRect = Rect()
let memberwiseRect = Rect(origin: Point(x: 2.0, y: 2.0),
    size: Size(width: 5.0, height: 5.0))
```

你可以提供一個額外的接受指定中心點和大小的構造器來擴展 `Rect` 結構體：

```swift
extension Rect {
    init(center: Point, size: Size) {
        let originX = center.x - (size.width / 2)
        let originY = center.y - (size.height / 2)
        self.init(origin: Point(x: originX, y: originY), size: size)
    }
}
```

這個新的構造器首先根據提供的 `center` 和 `size` 的值計算一個合適的原點。然後調用該結構體的逐一成員構造器 `init(origin:size:)`，該構造器將新的原點和大小的值保存到了相應的屬性中：

```swift
let centerRect = Rect(center: Point(x: 4.0, y: 4.0),
    size: Size(width: 3.0, height: 3.0))
// centerRect 的原點是 (2.5, 2.5)，大小是 (3.0, 3.0)
```

> 注意  
如果你使用擴展提供了一個新的構造器，你依舊有責任確保構造過程能夠讓實例完全初始化。

<a name="methods"></a>
## 方法

擴展可以為已有類型添加新的實例方法和類型方法。下面的例子為 `Int` 類型添加了一個名為 `repetitions` 的實例方法：

```swift
extension Int {
    func repetitions(task: () -> Void) {
        for _ in 0..<self {
            task()
        }
    }
}
```

這個 `repetitions(task:)` 方法接受一個 `() -> Void` 類型的單參數，表示沒有參數且沒有返回值的函數。

定義該擴展之後，你就可以對任意整數調用 `repetitions(task:)` 方法，將閉包中的任務執行整數對應的次數：

```swift
3.repetitions({
    print("Hello!")
})
// Hello!
// Hello!
// Hello!
```

可以使用尾隨閉包讓調用更加簡潔：

```swift
3.repetitions {
    print("Goodbye!")
}
// Goodbye!
// Goodbye!
// Goodbye!
```

<a name="mutating_instance_methods"></a>
### 可變實例方法

通過擴展添加的實例方法也可以修改該實例本身。結構體和枚舉類型中修改 `self` 或其屬性的方法必須將該實例方法標注為 `mutating`，正如來自原始實現的可變方法一樣。

下面的例子為 Swift 的 `Int` 類型添加了一個名為 `square` 的可變方法，用於計算原始值的平方值：

```swift
extension Int {
    mutating func square() {
        self = self * self
    }
}
var someInt = 3
someInt.square()
// someInt 的值現在是 9
```

<a name="subscripts"></a>
## 下標

擴展可以為已有類型添加新下標。這個例子為 Swift 內建類型 `Int` 添加了一個整型下標。該下標 `[n]` 返回十進制數字從右向左數的第 `n` 個數字：

- `123456789[0]` 返回 `9`
- `123456789[1]` 返回 `8`

……以此類推。

```swift
extension Int {
    subscript(digitIndex: Int) -> Int {
        var decimalBase = 1
        for _ in 0..<digitIndex {
            decimalBase *= 10
        }
        return (self / decimalBase) % 10
    }
}
746381295[0]
// 返回 5
746381295[1]
// 返回 9
746381295[2]
// 返回 2
746381295[8]
// 返回 7
```

如果該 `Int` 值沒有足夠的位數，即下標越界，那麼上述下標實現會返回 `0`，猶如在數字左邊自動補 `0`：

```swift
746381295[9]
// 返回 0，即等同於：
0746381295[9]
```

<a name="nested_types"></a>
## 嵌套類型

擴展可以為已有的類、結構體和枚舉添加新的嵌套類型：

```swift
extension Int {
    enum Kind {
        case Negative, Zero, Positive
    }
    var kind: Kind {
        switch self {
        case 0:
            return .Zero
        case let x where x > 0:
            return .Positive
        default:
            return .Negative
        }
    }
}
```

該例子為 `Int` 添加了嵌套枚舉。這個名為 `Kind` 的枚舉表示特定整數的類型。具體來說，就是表示整數是正數、零或者負數。

這個例子還為 `Int` 添加了一個計算型實例屬性，即 `kind`，用來根據整數返回適當的 `Kind` 枚舉成員。

現在，這個嵌套枚舉可以和任意 `Int` 值一起使用了：


```swift
func printIntegerKinds(_ numbers: [Int]) {
    for number in numbers {
        switch number.kind {
        case .Negative:
            print("- ", terminator: "")
        case .Zero:
            print("0 ", terminator: "")
        case .Positive:
            print("+ ", terminator: "")
        }
    }
    print("")
}
printIntegerKinds([3, 19, -27, 0, -6, 0, 7])
// 打印 「+ + - 0 - 0 + 」
```

函數 `printIntegerKinds(_:)` 接受一個 `Int` 數組，然後對該數組進行迭代。在每次迭代過程中，對當前整數的計算型屬性 `kind` 的值進行評估，並打印出適當的描述。

> 注意  
由於已知 `number.kind` 是 `Int.Kind` 類型，因此在 `switch` 語句中，`Int.Kind` 中的所有成員值都可以使用簡寫形式，例如使用 `. Negative` 而不是 `Int.Kind.Negative`。
