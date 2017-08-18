# 方法（Methods）
-----------------

> 1.0
> 翻譯：[pp-prog](https://github.com/pp-prog)
> 校對：[zqp](https://github.com/zqp)

> 2.0
> 翻譯+校對：[DianQK](https://github.com/DianQK)

> 2.1
> 翻譯：[DianQK](https://github.com/DianQK)，[Realank](https://github.com/Realank) 校對：[shanks](http://codebuild.me)，2016-01-18  
> 
> 2.2
> 校對：[SketchK](https://github.com/SketchK) 2016-05-13
> 3.0.1，shanks，2016-11-13

本頁包含內容：

- [實例方法](#instance_methods)
- [類型方法](#type_methods)

*方法*是與某些特定類型相關聯的函數。類、結構體、枚舉都可以定義實例方法；實例方法為給定類型的實例封裝了具體的任務與功能。類、結構體、枚舉也可以定義類型方法；類型方法與類型本身相關聯。類型方法與 Objective-C 中的類方法（class methods）相似。

結構體和枚舉能夠定義方法是 Swift 與 C/Objective-C 的主要區別之一。在 Objective-C 中，類是唯一能定義方法的類型。但在 Swift 中，你不僅能選擇是否要定義一個類/結構體/枚舉，還能靈活地在你創建的類型（類/結構體/枚舉）上定義方法。

<a name="instance_methods"></a>
## 實例方法 (Instance Methods)

*實例方法*是屬於某個特定類、結構體或者枚舉類型實例的方法。實例方法提供訪問和修改實例屬性的方法或提供與實例目的相關的功能，並以此來支撐實例的功能。實例方法的語法與函數完全一致，詳情參見[函數](./06_Functions.md)。

實例方法要寫在它所屬的類型的前後大括號之間。實例方法能夠隱式訪問它所屬類型的所有的其他實例方法和屬性。實例方法只能被它所屬的類的某個特定實例調用。實例方法不能脫離於現存的實例而被調用。

下面的例子，定義一個很簡單的`Counter`類，`Counter`能被用來對一個動作發生的次數進行計數：

```swift
class Counter {
    var count = 0
    func increment() {
        count += 1
    }
    func increment(by amount: Int) {
        count += amount
    }
    func reset() {
        count = 0
    }
}
```

`Counter`類定義了三個實例方法：
- `increment`讓計數器按一遞增；
- `increment(by: Int)`讓計數器按一個指定的整數值遞增；
- `reset`將計數器重置為0。

`Counter`這個類還聲明了一個可變屬性`count`，用它來保持對當前計數器值的追蹤。

和調用屬性一樣，用點語法（dot syntax）調用實例方法：

```swift
let counter = Counter()
// 初始計數值是0
counter.increment()
// 計數值現在是1
counter.increment(by: 5)
// 計數值現在是6
counter.reset()
// 計數值現在是0
```

函數參數可以同時有一個局部名稱（在函數體內部使用）和一個外部名稱（在調用函數時使用），詳情參見[指定外部參數名](./06_Functions.html#specifying_external_parameter_names)。方法參數也一樣，因為方法就是函數，只是這個函數與某個類型相關聯了。


<a name="the_self_property"></a>
### self 屬性

類型的每一個實例都有一個隱含屬性叫做`self`，`self`完全等同於該實例本身。你可以在一個實例的實例方法中使用這個隱含的`self`屬性來引用當前實例。

上面例子中的`increment`方法還可以這樣寫：

```swift
func increment() {
    self.count += 1
}
```

實際上，你不必在你的代碼裡面經常寫`self`。不論何時，只要在一個方法中使用一個已知的屬性或者方法名稱，如果你沒有明確地寫`self`，Swift 假定你是指當前實例的屬性或者方法。這種假定在上面的`Counter`中已經示范了：`Counter`中的三個實例方法中都使用的是`count`（而不是`self.count`）。

使用這條規則的主要場景是實例方法的某個參數名稱與實例的某個屬性名稱相同的時候。在這種情況下，參數名稱享有優先權，並且在引用屬性時必須使用一種更嚴格的方式。這時你可以使用`self`屬性來區分參數名稱和屬性名稱。

下面的例子中，`self`消除方法參數`x`和實例屬性`x`之間的歧義：

```swift
struct Point {
    var x = 0.0, y = 0.0
    func isToTheRightOfX(x: Double) -> Bool {
        return self.x > x
    }
}
let somePoint = Point(x: 4.0, y: 5.0)
if somePoint.isToTheRightOfX(1.0) {
    print("This point is to the right of the line where x == 1.0")
}
// 打印 "This point is to the right of the line where x == 1.0"
```

如果不使用`self`前綴，Swift 就認為兩次使用的`x`都指的是名稱為`x`的函數參數。

<a name="modifying_value_types_from_within_instance_methods"></a>
### 在實例方法中修改值類型

結構體和枚舉是*值類型*。默認情況下，值類型的屬性不能在它的實例方法中被修改。

但是，如果你確實需要在某個特定的方法中修改結構體或者枚舉的屬性，你可以為這個方法選擇`可變(mutating)`行為，然後就可以從其方法內部改變它的屬性；並且這個方法做的任何改變都會在方法執行結束時寫回到原始結構中。方法還可以給它隱含的`self`屬性賦予一個全新的實例，這個新實例在方法結束時會替換現存實例。

要使用`可變`方法，將關鍵字`mutating` 放到方法的`func`關鍵字之前就可以了：

```swift
struct Point {
    var x = 0.0, y = 0.0
    mutating func moveByX(deltaX: Double, y deltaY: Double) {
        x += deltaX
        y += deltaY
    }
}
var somePoint = Point(x: 1.0, y: 1.0)
somePoint.moveByX(2.0, y: 3.0)
print("The point is now at (\(somePoint.x), \(somePoint.y))")
// 打印 "The point is now at (3.0, 4.0)"
```

上面的`Point`結構體定義了一個可變方法 `moveByX(_:y:)` 來移動`Point`實例到給定的位置。該方法被調用時修改了這個點，而不是返回一個新的點。方法定義時加上了`mutating`關鍵字，從而允許修改屬性。

注意，不能在結構體類型的常量（a constant of structure type）上調用可變方法，因為其屬性不能被改變，即使屬性是變量屬性，詳情參見[常量結構體的存儲屬性](./10_Properties.html#stored_properties_of_constant_structure_instances)：

```swift
let fixedPoint = Point(x: 3.0, y: 3.0)
fixedPoint.moveByX(2.0, y: 3.0)
// 這裡將會報告一個錯誤
```

<a name="assigning_to_self_within_a_mutating_method"></a>
### 在可變方法中給 self 賦值

可變方法能夠賦給隱含屬性`self`一個全新的實例。上面`Point`的例子可以用下面的方式改寫：

```swift
struct Point {
    var x = 0.0, y = 0.0
    mutating func moveBy(x deltaX: Double, y deltaY: Double) {
        self = Point(x: x + deltaX, y: y + deltaY)
    }
}
```

新版的可變方法` moveBy(x:y:)`創建了一個新的結構體實例，它的 x 和 y 的值都被設定為目標值。調用這個版本的方法和調用上個版本的最終結果是一樣的。

枚舉的可變方法可以把`self`設置為同一枚舉類型中不同的成員：

```swift
enum TriStateSwitch {
    case Off, Low, High
    mutating func next() {
        switch self {
        case .Off:
            self = .Low
        case .Low:
            self = .High
        case .High:
            self = .Off
        }
    }
}
var ovenLight = TriStateSwitch.Low
ovenLight.next()
// ovenLight 現在等於 .High
ovenLight.next()
// ovenLight 現在等於 .Off
```

上面的例子中定義了一個三態開關的枚舉。每次調用`next()`方法時，開關在不同的電源狀態（`Off`，`Low`，`High`）之間循環切換。

<a name="type_methods"></a>
## 類型方法

實例方法是被某個類型的實例調用的方法。你也可以定義在類型本身上調用的方法，這種方法就叫做*類型方法*。在方法的`func`關鍵字之前加上關鍵字`static`，來指定類型方法。類還可以用關鍵字`class`來允許子類重寫父類的方法實現。

> 注意  
> 在 Objective-C 中，你只能為 Objective-C 的類類型（classes）定義類型方法（type-level methods）。在 Swift 中，你可以為所有的類、結構體和枚舉定義類型方法。每一個類型方法都被它所支持的類型顯式包含。  

類型方法和實例方法一樣用點語法調用。但是，你是在類型上調用這個方法，而不是在實例上調用。下面是如何在`SomeClass`類上調用類型方法的例子：

```swift
class SomeClass {
    class func someTypeMethod() {
        // 在這裡實現類型方法
    }
}
SomeClass.someTypeMethod()
```

在類型方法的方法體（body）中，`self`指向這個類型本身，而不是類型的某個實例。這意味著你可以用`self`來消除類型屬性和類型方法參數之間的歧義（類似於我們在前面處理實例屬性和實例方法參數時做的那樣）。

一般來說，在類型方法的方法體中，任何未限定的方法和屬性名稱，可以被本類中其他的類型方法和類型屬性引用。一個類型方法可以直接通過類型方法的名稱調用本類中的其它類型方法，而無需在方法名稱前面加上類型名稱。類似地，在結構體和枚舉中，也能夠直接通過類型屬性的名稱訪問本類中的類型屬性，而不需要前面加上類型名稱。

下面的例子定義了一個名為`LevelTracker`結構體。它監測玩家的游戲發展情況（游戲的不同層次或階段）。這是一個單人游戲，但也可以存儲多個玩家在同一設備上的游戲信息。

游戲初始時，所有的游戲等級（除了等級 1）都被鎖定。每次有玩家完成一個等級，這個等級就對這個設備上的所有玩家解鎖。`LevelTracker`結構體用類型屬性和方法監測游戲的哪個等級已經被解鎖。它還監測每個玩家的當前等級。

```swift
struct LevelTracker {
    static var highestUnlockedLevel = 1
    var currentLevel = 1
    
    static func unlock(_ level: Int) {
        if level > highestUnlockedLevel { highestUnlockedLevel = level }
    }
    
    static func isUnlocked(_ level: Int) -> Bool {
        return level <= highestUnlockedLevel
    }
    
    @discardableResult
    mutating func advance(to level: Int) -> Bool {
        if LevelTracker.isUnlocked(level) {
            currentLevel = level
            return true
        } else {
            return false
        }
    }
}
```

`LevelTracker`監測玩家已解鎖的最高等級。這個值被存儲在類型屬性`highestUnlockedLevel`中。

`LevelTracker`還定義了兩個類型方法與`highestUnlockedLevel`配合工作。第一個類型方法是`unlock(_:)`，一旦新等級被解鎖，它會更新`highestUnlockedLevel`的值。第二個類型方法是`isUnlocked(_:)`，如果某個給定的等級已經被解鎖，它將返回`true`。（注意，盡管我們沒有使用類似`LevelTracker.highestUnlockedLevel`的寫法，這個類型方法還是能夠訪問類型屬性`highestUnlockedLevel`）

除了類型屬性和類型方法，`LevelTracker`還監測每個玩家的進度。它用實例屬性`currentLevel`來監測每個玩家當前的等級。

為了便於管理`currentLevel`屬性，`LevelTracker`定義了實例方法`advance(to:)`。這個方法會在更新`currentLevel`之前檢查所請求的新等級是否已經解鎖。`advance(to:)`方法返回布爾值以指示是否能夠設置`currentLevel`。因為允許在調用`advance(to:)`時候忽略返回值，不會產生編譯警告，所以函數被標注為`@ discardableResult`屬性，更多關於屬性信息，請參考[屬性](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Attributes.html#//apple_ref/doc/uid/TP40014097-CH35-ID347)章節。

下面，`Player`類使用`LevelTracker`來監測和更新每個玩家的發展進度：

```swift
class Player {
    var tracker = LevelTracker()
    let playerName: String
    func complete(level: Int) {
        LevelTracker.unlock(level + 1)
        tracker.advance(to: level + 1)
    }
    init(name: String) {
        playerName = name
    }
}
```

`Player`類創建一個新的`LevelTracker`實例來監測這個用戶的進度。它提供了`complete(level:)`方法，一旦玩家完成某個指定等級就調用它。這個方法為所有玩家解鎖下一等級，並且將當前玩家的進度更新為下一等級。（我們忽略了`advance(to:)`返回的布爾值，因為之前調用`LevelTracker.unlock(_:)`時就知道了這個等級已經被解鎖了）。

你還可以為一個新的玩家創建一個`Player`的實例，然後看這個玩家完成等級一時發生了什麼：

```swift
var player = Player(name: "Argyrios")
player.complete(level: 1)
print("highest unlocked level is now \(LevelTracker.highestUnlockedLevel)")
// 打印 "highest unlocked level is now 2"
```

如果你創建了第二個玩家，並嘗試讓他開始一個沒有被任何玩家解鎖的等級，那麼試圖設置玩家當前等級將會失敗：

```swift
player = Player(name: "Beto")
if player.tracker.advance(to: 6) {
    print("player is now on level 6")
} else {
    print("level 6 has not yet been unlocked")
}
// 打印 "level 6 has not yet been unlocked"
```
