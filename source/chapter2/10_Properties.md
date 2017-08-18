# 屬性 (Properties)
---

> 1.0
> 翻譯：[shinyzhu](https://github.com/shinyzhu)
> 校對：[pp-prog](https://github.com/pp-prog) [yangsiy](https://github.com/yangsiy)


> 2.0
> 翻譯+校對：[yangsiy](https://github.com/yangsiy)


> 2.1
> 翻譯：[buginux](https://github.com/buginux)
> 校對：[shanks](http://codebuild.me)，2015-10-29


>   2.2
>   翻譯：[saitjr](https://github.com/saitjr)，2016-04-11，[SketchK](https://github.com/SketchK) 2016-05-13
> 
> 3.0.1，shanks，2016-11-12



本頁包含內容：

- [存儲屬性](#stored_properties)
- [計算屬性](#computed_properties)
- [屬性觀察器](#property_observers)
- [全局變量和局部變量](#global_and_local_variables)
- [類型屬性](#type_properties)

*屬性*將值跟特定的類、結構或枚舉關聯。存儲屬性存儲常量或變量作為實例的一部分，而計算屬性計算（不是存儲）一個值。計算屬性可以用於類、結構體和枚舉，存儲屬性只能用於類和結構體。

存儲屬性和計算屬性通常與特定類型的實例關聯。但是，屬性也可以直接作用於類型本身，這種屬性稱為類型屬性。

另外，還可以定義屬性觀察器來監控屬性值的變化，以此來觸發一個自定義的操作。屬性觀察器可以添加到自己定義的存儲屬性上，也可以添加到從父類繼承的屬性上。

<a name="stored_properties"></a>
## 存儲屬性

簡單來說，一個存儲屬性就是存儲在特定類或結構體實例裡的一個常量或變量。存儲屬性可以是*變量存儲屬性*（用關鍵字 `var` 定義），也可以是*常量存儲屬性*（用關鍵字 `let` 定義）。

可以在定義存儲屬性的時候指定默認值，請參考[默認構造器](./14_Initialization.html#default_initializers)一節。也可以在構造過程中設置或修改存儲屬性的值，甚至修改常量存儲屬性的值，請參考[構造過程中常量屬性的修改](./14_Initialization.html#assigning_constant_properties_during_initialization)一節。

下面的例子定義了一個名為 `FixedLengthRange` 的結構體，該結構體用於描述整數的范圍，且這個范圍值在被創建後不能被修改.

```swift
struct FixedLengthRange {
    var firstValue: Int
    let length: Int
}
var rangeOfThreeItems = FixedLengthRange(firstValue: 0, length: 3)
// 該區間表示整數0，1，2
rangeOfThreeItems.firstValue = 6
// 該區間現在表示整數6，7，8
```

`FixedLengthRange` 的實例包含一個名為 `firstValue` 的變量存儲屬性和一個名為 `length` 的常量存儲屬性。在上面的例子中，`length` 在創建實例的時候被初始化，因為它是一個常量存儲屬性，所以之後無法修改它的值。

<a name="stored_properties_of_constant_structure_instances"></a>
### 常量結構體的存儲屬性

如果創建了一個結構體的實例並將其賦值給一個常量，則無法修改該實例的任何屬性，即使有屬性被聲明為變量也不行：

```swift
let rangeOfFourItems = FixedLengthRange(firstValue: 0, length: 4)
// 該區間表示整數0，1，2，3
rangeOfFourItems.firstValue = 6
// 盡管 firstValue 是個變量屬性，這裡還是會報錯
```

因為 `rangeOfFourItems` 被聲明成了常量（用 `let` 關鍵字），即使 `firstValue` 是一個變量屬性，也無法再修改它了。

這種行為是由於結構體（struct）屬於*值類型*。當值類型的實例被聲明為常量的時候，它的所有屬性也就成了常量。

屬於*引用類型*的類（class）則不一樣。把一個引用類型的實例賦給一個常量後，仍然可以修改該實例的變量屬性。

<a name="lazy_stored_properties"></a>
### 延遲存儲屬性

*延遲存儲屬*性是指當第一次被調用的時候才會計算其初始值的屬性。在屬性聲明前使用 `lazy` 來標示一個延遲存儲屬性。

> 注意  
> 必須將延遲存儲屬性聲明成變量（使用 `var` 關鍵字），因為屬性的初始值可能在實例構造完成之後才會得到。而常量屬性在構造過程完成之前必須要有初始值，因此無法聲明成延遲屬性。  

延遲屬性很有用，當屬性的值依賴於在實例的構造過程結束後才會知道影響值的外部因素時，或者當獲得屬性的初始值需要復雜或大量計算時，可以只在需要的時候計算它。

下面的例子使用了延遲存儲屬性來避免復雜類中不必要的初始化。例子中定義了 `DataImporter` 和 `DataManager` 兩個類，下面是部分代碼：

```swift
class DataImporter {
    /*
    DataImporter 是一個負責將外部文件中的數據導入的類。
    這個類的初始化會消耗不少時間。
    */
    var fileName = "data.txt"
    // 這裡會提供數據導入功能
}

class DataManager {
    lazy var importer = DataImporter()
    var data = [String]()
    // 這裡會提供數據管理功能
}

let manager = DataManager()
manager.data.append("Some data")
manager.data.append("Some more data")
// DataImporter 實例的 importer 屬性還沒有被創建
```

`DataManager` 類包含一個名為 `data` 的存儲屬性，初始值是一個空的字符串（`String`）數組。這裡沒有給出全部代碼，只需知道 `DataManager` 類的目的是管理和提供對這個字符串數組的訪問即可。

`DataManager` 的一個功能是從文件導入數據。該功能由 `DataImporter` 類提供，`DataImporter` 完成初始化需要消耗不少時間：因為它的實例在初始化時可能要打開文件，還要讀取文件內容到內存。

`DataManager` 管理數據時也可能不從文件中導入數據。所以當 `DataManager` 的實例被創建時，沒必要創建一個 `DataImporter` 的實例，更明智的做法是第一次用到 `DataImporter` 的時候才去創建它。

由於使用了 `lazy` ，`importer` 屬性只有在第一次被訪問的時候才被創建。比如訪問它的屬性 `fileName` 時：

```swift
print(manager.importer.fileName)
// DataImporter 實例的 importer 屬性現在被創建了
// 輸出 "data.txt」
```

> 注意  
> 如果一個被標記為 `lazy` 的屬性在沒有初始化時就同時被多個線程訪問，則無法保證該屬性只會被初始化一次。

<a name="stored_properties_and_instance_variables"></a>
### 存儲屬性和實例變量

如果您有過 Objective-C 經驗，應該知道 Objective-C 為類實例存儲值和引用提供兩種方法。除了屬性之外，還可以使用實例變量作為屬性值的後端存儲。

Swift 編程語言中把這些理論統一用屬性來實現。Swift 中的屬性沒有對應的實例變量，屬性的後端存儲也無法直接訪問。這就避免了不同場景下訪問方式的困擾，同時也將屬性的定義簡化成一個語句。屬性的全部信息——包括命名、類型和內存管理特征——都在唯一一個地方（類型定義中）定義。

<a name="computed_properties"></a>
## 計算屬性

除存儲屬性外，類、結構體和枚舉可以定義*計算屬性*。計算屬性不直接存儲值，而是提供一個 getter 和一個可選的 setter，來間接獲取和設置其他屬性或變量的值。

```swift
struct Point {
    var x = 0.0, y = 0.0
}
struct Size {
    var width = 0.0, height = 0.0
}
struct Rect {
    var origin = Point()
    var size = Size()
    var center: Point {
        get {
            let centerX = origin.x + (size.width / 2)
            let centerY = origin.y + (size.height / 2)
            return Point(x: centerX, y: centerY)
        }
        set(newCenter) {
            origin.x = newCenter.x - (size.width / 2)
            origin.y = newCenter.y - (size.height / 2)
        }
    }
}
var square = Rect(origin: Point(x: 0.0, y: 0.0),
    size: Size(width: 10.0, height: 10.0))
let initialSquareCenter = square.center
square.center = Point(x: 15.0, y: 15.0)
print("square.origin is now at (\(square.origin.x), \(square.origin.y))")
// 打印 "square.origin is now at (10.0, 10.0)」
```

這個例子定義了 3 個結構體來描述幾何形狀：

- `Point` 封裝了一個 `(x, y)` 的坐標
- `Size` 封裝了一個 `width` 和一個 `height`
- `Rect` 表示一個有原點和尺寸的矩形

`Rect`也提供了一個名為`center` 的計算屬性。一個矩形的中心點可以從原點（`origin`）和大小（`size`）算出，所以不需要將它以顯式聲明的 `Point` 來保存。`Rect` 的計算屬性 `center` 提供了自定義的 getter 和 setter 來獲取和設置矩形的中心點，就像它有一個存儲屬性一樣。

上述例子中創建了一個名為 `square` 的 `Rect` 實例，初始值原點是 `(0, 0)`，寬度高度都是 `10`。如下圖中藍色正方形所示。

`square` 的 `center` 屬性可以通過點運算符（`square.center`）來訪問，這會調用該屬性的 getter 來獲取它的值。跟直接返回已經存在的值不同，getter 實際上通過計算然後返回一個新的 `Point` 來表示 `square` 的中心點。如代碼所示，它正確返回了中心點 `(5, 5)`。

`center` 屬性之後被設置了一個新的值 `(15, 15)`，表示向右上方移動正方形到如下圖橙色正方形所示的位置。設置屬性`center`的值會調用它的 setter 來修改屬性 `origin` 的 `x` 和 `y` 的值，從而實現移動正方形到新的位置。

<img src="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/computedProperties_2x.png" alt="Computed Properties sample" width="388" height="387" />

<a name="shorthand_setter_declaration"></a>
### 簡化 setter 聲明

如果計算屬性的 setter 沒有定義表示新值的參數名，則可以使用默認名稱 `newValue`。下面是使用了簡化 setter 聲明的 `Rect` 結構體代碼：

```swift
struct AlternativeRect {
    var origin = Point()
    var size = Size()
    var center: Point {
        get {
            let centerX = origin.x + (size.width / 2)
            let centerY = origin.y + (size.height / 2)
            return Point(x: centerX, y: centerY)
        }
        set {
            origin.x = newValue.x - (size.width / 2)
            origin.y = newValue.y - (size.height / 2)
        }
    }
}
```

<a name="readonly_computed_properties"></a>
### 只讀計算屬性

只有 getter 沒有 setter 的計算屬性就是*只讀計算屬性*。只讀計算屬性總是返回一個值，可以通過點運算符訪問，但不能設置新的值。

> 注意  
> 必須使用 `var` 關鍵字定義計算屬性，包括只讀計算屬性，因為它們的值不是固定的。`let` 關鍵字只用來聲明常量屬性，表示初始化後再也無法修改的值。

只讀計算屬性的聲明可以去掉 `get` 關鍵字和花括號：

```swift
struct Cuboid {
    var width = 0.0, height = 0.0, depth = 0.0
    var volume: Double {
    	return width * height * depth
    }
}
let fourByFiveByTwo = Cuboid(width: 4.0, height: 5.0, depth: 2.0)
print("the volume of fourByFiveByTwo is \(fourByFiveByTwo.volume)")
// 打印 "the volume of fourByFiveByTwo is 40.0"
```

這個例子定義了一個名為 `Cuboid` 的結構體，表示三維空間的立方體，包含 `width`、`height` 和 `depth` 屬性。結構體還有一個名為 `volume` 的只讀計算屬性用來返回立方體的體積。為 `volume` 提供 setter 毫無意義，因為無法確定如何修改 `width`、`height` 和 `depth` 三者的值來匹配新的 `volume`。然而，`Cuboid` 提供一個只讀計算屬性來讓外部用戶直接獲取體積是很有用的。

<a name="property_observers"></a>
## 屬性觀察器

*屬性觀察器*監控和響應屬性值的變化，每次屬性被設置值的時候都會調用屬性觀察器，即使新值和當前值相同的時候也不例外。

可以為除了延遲存儲屬性之外的其他存儲屬性添加屬性觀察器，也可以通過重寫屬性的方式為繼承的屬性（包括存儲屬性和計算屬性）添加屬性觀察器。你不必為非重寫的計算屬性添加屬性觀察器，因為可以通過它的 setter 直接監控和響應值的變化。 屬性重寫請參考[重寫](./13_Inheritance.html#overriding)。  


可以為屬性添加如下的一個或全部觀察器：

- `willSet` 在新的值被設置之前調用
- `didSet` 在新的值被設置之後立即調用

`willSet` 觀察器會將新的屬性值作為常量參數傳入，在 `willSet` 的實現代碼中可以為這個參數指定一個名稱，如果不指定則參數仍然可用，這時使用默認名稱 `newValue` 表示。

同樣，`didSet` 觀察器會將舊的屬性值作為參數傳入，可以為該參數命名或者使用默認參數名 `oldValue`。如果在 `didSet` 方法中再次對該屬性賦值，那麼新值會覆蓋舊的值。

> 注意  
> 父類的屬性在子類的構造器中被賦值時，它在父類中的 `willSet` 和 `didSet` 觀察器會被調用，隨後才會調用子類的觀察器。在父類初始化方法調用之前，子類給屬性賦值時，觀察器不會被調用。
> 有關構造器代理的更多信息，請參考[值類型的構造器代理](./14_Initialization.html#initializer_delegation_for_value_types)和[類的構造器代理規則](./14_Initialization.html#initializer_delegation_for_class_types)。

下面是一個 `willSet` 和 `didSet` 實際運用的例子，其中定義了一個名為 `StepCounter` 的類，用來統計一個人步行時的總步數。這個類可以跟計步器或其他日常鍛煉的統計裝置的輸入數據配合使用。

```swift
class StepCounter {
    var totalSteps: Int = 0 {
        willSet(newTotalSteps) {
            print("About to set totalSteps to \(newTotalSteps)")
        }
        didSet {
            if totalSteps > oldValue  {
                print("Added \(totalSteps - oldValue) steps")
            }
        }
    }
}
let stepCounter = StepCounter()
stepCounter.totalSteps = 200
// About to set totalSteps to 200
// Added 200 steps
stepCounter.totalSteps = 360
// About to set totalSteps to 360
// Added 160 steps
stepCounter.totalSteps = 896
// About to set totalSteps to 896
// Added 536 steps
```

`StepCounter` 類定義了一個 `Int` 類型的屬性 `totalSteps`，它是一個存儲屬性，包含 `willSet` 和 `didSet` 觀察器。

當 `totalSteps` 被設置新值的時候，它的 `willSet` 和 `didSet` 觀察器都會被調用，即使新值和當前值完全相同時也會被調用。

例子中的 `willSet` 觀察器將表示新值的參數自定義為 `newTotalSteps`，這個觀察器只是簡單的將新的值輸出。

`didSet` 觀察器在 `totalSteps` 的值改變後被調用，它把新值和舊值進行對比，如果總步數增加了，就輸出一個消息表示增加了多少步。`didSet` 沒有為舊值提供自定義名稱，所以默認值 `oldValue` 表示舊值的參數名。

>注意
>
>如果將屬性通過 in-out 方式傳入函數，`willSet` 和 `didSet` 也會調用。這是因為 in-out 參數采用了拷入拷出模式：即在函數內部使用的是參數的 copy，函數結束後，又對參數重新賦值。關於 in-out 參數詳細的介紹，請參考[輸入輸出參數](../chapter3/05_Declarations.html#in-out_parameters)

<a name="global_and_local_variables"></a>
## 全局變量和局部變量

計算屬性和屬性觀察器所描述的功能也可以用於*全局變量*和*局部變量*。全局變量是在函數、方法、閉包或任何類型之外定義的變量。局部變量是在函數、方法或閉包內部定義的變量。

前面章節提到的全局或局部變量都屬於存儲型變量，跟存儲屬性類似，它為特定類型的值提供存儲空間，並允許讀取和寫入。

另外，在全局或局部范圍都可以定義計算型變量和為存儲型變量定義觀察器。計算型變量跟計算屬性一樣，返回一個計算結果而不是存儲值，聲明格式也完全一樣。

> 注意  
> 全局的常量或變量都是延遲計算的，跟[延遲存儲屬性](#lazy_stored_properties)相似，不同的地方在於，全局的常量或變量不需要標記`lazy`修飾符。  
> 局部范圍的常量或變量從不延遲計算。  

<a name="type_properties"></a>
## 類型屬性

實例屬性屬於一個特定類型的實例，每創建一個實例，實例都擁有屬於自己的一套屬性值，實例之間的屬性相互獨立。

也可以為類型本身定義屬性，無論創建了多少個該類型的實例，這些屬性都只有唯一一份。這種屬性就是*類型屬性*。

類型屬性用於定義某個類型所有實例共享的數據，比如所有實例都能用的一個常量（就像 C 語言中的靜態常量），或者所有實例都能訪問的一個變量（就像 C 語言中的靜態變量）。

存儲型類型屬性可以是變量或常量，計算型類型屬性跟實例的計算型屬性一樣只能定義成變量屬性。

> 注意  
> 跟實例的存儲型屬性不同，必須給存儲型類型屬性指定默認值，因為類型本身沒有構造器，也就無法在初始化過程中使用構造器給類型屬性賦值。  
> 存儲型類型屬性是延遲初始化的，它們只有在第一次被訪問的時候才會被初始化。即使它們被多個線程同時訪問，系統也保證只會對其進行一次初始化，並且不需要對其使用 `lazy` 修飾符。

<a name="type_property_syntax"></a>
### 類型屬性語法

在 C 或 Objective-C 中，與某個類型關聯的靜態常量和靜態變量，是作為全局（*global*）靜態變量定義的。但是在 Swift 中，類型屬性是作為類型定義的一部分寫在類型最外層的花括號內，因此它的作用范圍也就在類型支持的范圍內。

使用關鍵字 `static` 來定義類型屬性。在為類定義計算型類型屬性時，可以改用關鍵字 `class` 來支持子類對父類的實現進行重寫。下面的例子演示了存儲型和計算型類型屬性的語法：

```swift
struct SomeStructure {
    static var storedTypeProperty = "Some value."
    static var computedTypeProperty: Int {
        return 1
    }
}
enum SomeEnumeration {
    static var storedTypeProperty = "Some value."
    static var computedTypeProperty: Int {
        return 6
    }
}
class SomeClass {
    static var storedTypeProperty = "Some value."
    static var computedTypeProperty: Int {
        return 27
    }
    class var overrideableComputedTypeProperty: Int {
        return 107
    }
}
```

> 注意  
> 例子中的計算型類型屬性是只讀的，但也可以定義可讀可寫的計算型類型屬性，跟計算型實例屬性的語法相同。  

<a name="querying_and_setting_type_properties"></a>
### 獲取和設置類型屬性的值

跟實例屬性一樣，類型屬性也是通過點運算符來訪問。但是，類型屬性是通過類型本身來訪問，而不是通過實例。比如：

```swift
print(SomeStructure.storedTypeProperty)
// 打印 "Some value."
SomeStructure.storedTypeProperty = "Another value."
print(SomeStructure.storedTypeProperty)
// 打印 "Another value.」
print(SomeEnumeration.computedTypeProperty)
// 打印 "6"
print(SomeClass.computedTypeProperty)
// 打印 "27"
```

下面的例子定義了一個結構體，使用兩個存儲型類型屬性來表示兩個聲道的音量，每個聲道具有 `0` 到 `10` 之間的整數音量。

下圖展示了如何把兩個聲道結合來模擬立體聲的音量。當聲道的音量是 `0`，沒有一個燈會亮；當聲道的音量是 `10`，所有燈點亮。本圖中，左聲道的音量是 `9`，右聲道的音量是 `7`：

<img src="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/staticPropertiesVUMeter_2x.png" alt="Static Properties VUMeter" width="243" height="357" />

上面所描述的聲道模型使用 `AudioChannel` 結構體的實例來表示：

```swift
struct AudioChannel {
    static let thresholdLevel = 10
    static var maxInputLevelForAllChannels = 0
    var currentLevel: Int = 0 {
        didSet {
            if currentLevel > AudioChannel.thresholdLevel {
                // 將當前音量限制在閾值之內
                currentLevel = AudioChannel.thresholdLevel
            }
            if currentLevel > AudioChannel.maxInputLevelForAllChannels {
                // 存儲當前音量作為新的最大輸入音量
                AudioChannel.maxInputLevelForAllChannels = currentLevel
            }
        }
    }
}
```

結構 `AudioChannel` 定義了 2 個存儲型類型屬性來實現上述功能。第一個是 `thresholdLevel`，表示音量的最大上限閾值，它是一個值為 `10` 的常量，對所有實例都可見，如果音量高於 `10`，則取最大上限值 `10`（見後面描述）。

第二個類型屬性是變量存儲型屬性 `maxInputLevelForAllChannels`，它用來表示所有 `AudioChannel` 實例的最大音量，初始值是`0`。

`AudioChannel` 也定義了一個名為 `currentLevel` 的存儲型實例屬性，表示當前聲道現在的音量，取值為 `0` 到 `10`。

屬性 `currentLevel` 包含 `didSet` 屬性觀察器來檢查每次設置後的屬性值，它做如下兩個檢查：

- 如果 `currentLevel` 的新值大於允許的閾值 `thresholdLevel`，屬性觀察器將 `currentLevel` 的值限定為閾值 `thresholdLevel`。
- 如果修正後的 `currentLevel` 值大於靜態類型屬性 `maxInputLevelForAllChannels` 的值，屬性觀察器就將新值保存在 `maxInputLevelForAllChannels` 中。

> 注意  
> 在第一個檢查過程中，`didSet` 屬性觀察器將 `currentLevel` 設置成了不同的值，但這不會造成屬性觀察器被再次調用。  

可以使用結構體 `AudioChannel` 創建兩個聲道 `leftChannel` 和 `rightChannel`，用以表示立體聲系統的音量：

```swift
var leftChannel = AudioChannel()
var rightChannel = AudioChannel()
```

如果將左聲道的 `currentLevel` 設置成 `7`，類型屬性 `maxInputLevelForAllChannels` 也會更新成 `7`：

```swift
leftChannel.currentLevel = 7
print(leftChannel.currentLevel)
// 輸出 "7"
print(AudioChannel.maxInputLevelForAllChannels)
// 輸出 "7"
```

如果試圖將右聲道的 `currentLevel` 設置成 `11`，它會被修正到最大值 `10`，同時 `maxInputLevelForAllChannels` 的值也會更新到 `10`：

```swift
rightChannel.currentLevel = 11
print(rightChannel.currentLevel)
// 輸出 "10"
print(AudioChannel.maxInputLevelForAllChannels)
// 輸出 "10"
```
