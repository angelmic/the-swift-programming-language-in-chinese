# 類和結構體

> 1.0
> 翻譯：[JaySurplus](https://github.com/JaySurplus)
> 校對：[sg552](https://github.com/sg552)

> 2.0
> 翻譯+校對：[SkyJean](https://github.com/SkyJean)

> 2.1
> 校對：[shanks](http://codebuild.me)，2015-10-29

> 2.2
> 校對：[SketchK](https://github.com/SketchK) 2016-05-13
> 
> 3.0.1， shanks， 2016-11-12

本頁包含內容：

- [類和結構體對比](#comparing_classes_and_structures)
- [結構體和枚舉是值類型](#structures_and_enumerations_are_value_types)
- [類是引用類型](#classes_are_reference_types)
- [類和結構體的選擇](#choosing_between_classes_and_structures)
- [字符串、數組、和字典類型的賦值與復制行為](#assignment_and_copy_behavior_for_strings_arrays_and_dictionaries)


*類*和*結構體*是人們構建代碼所用的一種通用且靈活的構造體。我們可以使用完全相同的語法規則來為類和結構體定義屬性（常量、變量）和添加方法，從而擴展類和結構體的功能。

與其他編程語言所不同的是，Swift 並不要求你為自定義類和結構去創建獨立的接口和實現文件。你所要做的是在一個單一文件中定義一個類或者結構體，系統將會自動生成面向其它代碼的外部接口。

> 注意  
> 通常一個*類*的實例被稱為*對象*。然而在 Swift 中，類和結構體的關系要比在其他語言中更加的密切，本章中所討論的大部分功能都可以用在類和結構體上。因此，我們會主要使用*實例*。

<a name="comparing_classes_and_structures"></a>
## 類和結構體對比

Swift 中類和結構體有很多共同點。共同處在於：

* 定義屬性用於存儲值
* 定義方法用於提供功能
* 定義下標操作使得可以通過下標語法來訪問實例所包含的值
* 定義構造器用於生成初始化值
* 通過擴展以增加默認實現的功能
* 實現協議以提供某種標准功能

更多信息請參見[屬性](./10_Properties.html)，[方法](./11_Methods.html)，[下標](./12_Subscripts.html)，[構造過程](./14_Initialization.html)，[擴展](./21_Extensions.html)，和[協議](./22_Protocols.html)。

與結構體相比，類還有如下的附加功能：

* 繼承允許一個類繼承另一個類的特征
* 類型轉換允許在運行時檢查和解釋一個類實例的類型
* 析構器允許一個類實例釋放任何其所被分配的資源
* 引用計數允許對一個類的多次引用

更多信息請參見[繼承](./13_Inheritance.html)，[類型轉換](./19_Type_Casting.html)，[析構過程](./15_Deinitialization.html)，和[自動引用計數](./16_Automatic_Reference_Counting.html)。

> 注意  
> 結構體總是通過被復制的方式在代碼中傳遞，不使用引用計數。

<a name="definition_syntax"></a>
### 定義語法

類和結構體有著類似的定義方式。我們通過關鍵字`class`和`struct`來分別表示類和結構體，並在一對大括號中定義它們的具體內容：

```swift
class SomeClass {
	// 在這裡定義類
}
struct SomeStructure {
	// 在這裡定義結構體
}
```

> 注意  
> 在你每次定義一個新類或者結構體的時候，實際上你是定義了一個新的 Swift 類型。因此請使用`UpperCamelCase`這種方式來命名（如`SomeClass`和`SomeStructure`等），以便符合標准 Swift 類型的大寫命名風格（如`String`，`Int`和`Bool`）。相反的，請使用`lowerCamelCase`這種方式為屬性和方法命名（如`framerate`和`incrementCount`），以便和類型名區分。

以下是定義結構體和定義類的示例：

```swift
struct Resolution {
	var width = 0
	var height = 0
}
class VideoMode {
	var resolution = Resolution()
	var interlaced = false
	var frameRate = 0.0
	var name: String?
}
```

在上面的示例中我們定義了一個名為`Resolution`的結構體，用來描述一個顯示器的像素分辨率。這個結構體包含了兩個名為`width`和`height`的存儲屬性。存儲屬性是被捆綁和存儲在類或結構體中的常量或變量。當這兩個屬性被初始化為整數`0`的時候，它們會被推斷為`Int`類型。

在上面的示例中我們還定義了一個名為`VideoMode`的類，用來描述一個視頻顯示器的特定模式。這個類包含了四個變量存儲屬性。第一個是`分辨率`，它被初始化為一個新的`Resolution`結構體的實例，屬性類型被推斷為`Resolution`。新`VideoMode`實例同時還會初始化其它三個屬性，它們分別是，初始值為`false`的`interlaced`，初始值為`0.0`的`frameRate`，以及值為可選`String`的`name`。`name`屬性會被自動賦予一個默認值`nil`，意為「沒有`name`值」，因為它是一個可選類型。

<a name="class_and_structure_instances"></a>
### 類和結構體實例

`Resolution`結構體和`VideoMode`類的定義僅描述了什麼是`Resolution`和`VideoMode`。它們並沒有描述一個特定的分辨率（resolution）或者視頻模式（video mode）。為了描述一個特定的分辨率或者視頻模式，我們需要生成一個它們的實例。

生成結構體和類實例的語法非常相似：

```swift
let someResolution = Resolution()
let someVideoMode = VideoMode()
```

結構體和類都使用構造器語法來生成新的實例。構造器語法的最簡單形式是在結構體或者類的類型名稱後跟隨一對空括號，如`Resolution()`或`VideoMode()`。通過這種方式所創建的類或者結構體實例，其屬性均會被初始化為默認值。[構造過程](./14_Initialization.html)章節會對類和結構體的初始化進行更詳細的討論。

<a name="accessing_properties"></a>
### 屬性訪問

通過使用*點語法*，你可以訪問實例的屬性。其語法規則是，實例名後面緊跟屬性名，兩者通過點號(`.`)連接：

```swift
print("The width of someResolution is \(someResolution.width)")
// 打印 "The width of someResolution is 0"
```

在上面的例子中，`someResolution.width`引用`someResolution`的`width`屬性，返回`width`的初始值`0`。

你也可以訪問子屬性，如`VideoMode`中`Resolution`屬性的`width`屬性：

```swift
print("The width of someVideoMode is \(someVideoMode.resolution.width)")
// 打印 "The width of someVideoMode is 0"
```

你也可以使用點語法為變量屬性賦值：

```swift
someVideoMode.resolution.width = 1280
print("The width of someVideoMode is now \(someVideoMode.resolution.width)")
// 打印 "The width of someVideoMode is now 1280"
```

> 注意  
> 與 Objective-C 語言不同的是，Swift 允許直接設置結構體屬性的子屬性。上面的最後一個例子，就是直接設置了`someVideoMode`中`resolution`屬性的`width`這個子屬性，以上操作並不需要重新為整個`resolution`屬性設置新值。

<a name="memberwise_initializers_for_structure_types"></a>
### 結構體類型的成員逐一構造器
所有結構體都有一個自動生成的*成員逐一構造器*，用於初始化新結構體實例中成員的屬性。新實例中各個屬性的初始值可以通過屬性的名稱傳遞到成員逐一構造器之中：

```swift
let vga = Resolution(width:640, height: 480)
```

與結構體不同，類實例沒有默認的成員逐一構造器。[構造過程](./14_Initialization.html)章節會對構造器進行更詳細的討論。

<a name="structures_and_enumerations_are_value_types"></a>
## 結構體和枚舉是值類型

*值類型*被賦予給一個變量、常量或者被傳遞給一個函數的時候，其值會被*拷貝*。

在之前的章節中，我們已經大量使用了值類型。實際上，在 Swift 中，所有的基本類型：整數（Integer）、浮點數（floating-point）、布爾值（Boolean）、字符串（string)、數組（array）和字典（dictionary），都是值類型，並且在底層都是以結構體的形式所實現。

在 Swift 中，所有的結構體和枚舉類型都是值類型。這意味著它們的實例，以及實例中所包含的任何值類型屬性，在代碼中傳遞的時候都會被復制。

請看下面這個示例，其使用了前一個示例中的`Resolution`結構體：

```swift
let hd = Resolution(width: 1920, height: 1080)
var cinema = hd
```

在以上示例中，聲明了一個名為`hd`的常量，其值為一個初始化為全高清視頻分辨率（`1920` 像素寬，`1080` 像素高）的`Resolution`實例。

然後示例中又聲明了一個名為`cinema`的變量，並將`hd`賦值給它。因為`Resolution`是一個結構體，所以`cinema`的值其實是`hd`的一個拷貝副本，而不是`hd`本身。盡管`hd`和`cinema`有著相同的寬（width）和高（height），但是在幕後它們是兩個完全不同的實例。

下面，為了符合數碼影院放映的需求（`2048` 像素寬，`1080` 像素高），`cinema`的`width`屬性需要作如下修改：

```swift
cinema.width = 2048
```

這裡，將會顯示`cinema`的`width`屬性確已改為了`2048`：

```swift
print("cinema is now  \(cinema.width) pixels wide")
// 打印 "cinema is now 2048 pixels wide"
```

然而，初始的`hd`實例中`width`屬性還是`1920`：

```swift
print("hd is still \(hd.width) pixels wide")
// 打印 "hd is still 1920 pixels wide"
```

在將`hd`賦予給`cinema`的時候，實際上是將`hd`中所存儲的值進行拷貝，然後將拷貝的數據存儲到新的`cinema`實例中。結果就是兩個完全獨立的實例碰巧包含有相同的數值。由於兩者相互獨立，因此將`cinema`的`width`修改為`2048`並不會影響`hd`中的`width`的值。

枚舉也遵循相同的行為准則：

```swift
enum CompassPoint {
	case North, South, East, West
}
var currentDirection = CompassPoint.West
let rememberedDirection = currentDirection
currentDirection = .East
if rememberedDirection == .West {
	print("The remembered direction is still .West")
}
// 打印 "The remembered direction is still .West"
```

上例中`rememberedDirection`被賦予了`currentDirection`的值，實際上它被賦予的是值的一個拷貝。賦值過程結束後再修改`currentDirection`的值並不影響`rememberedDirection`所儲存的原始值的拷貝。

<a name="classes_are_reference_types"></a>
## 類是引用類型

與值類型不同，*引用類型*在被賦予到一個變量、常量或者被傳遞到一個函數時，其值不會被拷貝。因此，引用的是已存在的實例本身而不是其拷貝。

請看下面這個示例，其使用了之前定義的`VideoMode`類：

```swift
let tenEighty = VideoMode()
tenEighty.resolution = hd
tenEighty.interlaced = true
tenEighty.name = "1080i"
tenEighty.frameRate = 25.0
```

以上示例中，聲明了一個名為`tenEighty`的常量，其引用了一個`VideoMode`類的新實例。在之前的示例中，這個視頻模式（video mode）被賦予了HD分辨率（`1920`*`1080`）的一個拷貝（即`hd`實例）。同時設置為`interlaced`，命名為`「1080i」`。最後，其幀率是`25.0`幀每秒。

然後，`tenEighty`被賦予名為`alsoTenEighty`的新常量，同時對`alsoTenEighty`的幀率進行修改：

```swift
let alsoTenEighty = tenEighty
alsoTenEighty.frameRate = 30.0
```

因為類是引用類型，所以`tenEight`和`alsoTenEight`實際上引用的是相同的`VideoMode`實例。換句話說，它們是同一個實例的兩種叫法。

下面，通過查看`tenEighty`的`frameRate`屬性，我們會發現它正確的顯示了所引用的`VideoMode`實例的新幀率，其值為`30.0`：

```swift
print("The frameRate property of tenEighty is now \(tenEighty.frameRate)")
// 打印 "The frameRate property of theEighty is now 30.0"
```

需要注意的是`tenEighty`和`alsoTenEighty`被聲明為常量而不是變量。然而你依然可以改變`tenEighty.frameRate`和`alsoTenEighty.frameRate`，因為`tenEighty`和`alsoTenEighty`這兩個常量的值並未改變。它們並不「存儲」這個`VideoMode`實例，而僅僅是對`VideoMode`實例的引用。所以，改變的是被引用的`VideoMode`的`frameRate`屬性，而不是引用`VideoMode`的常量的值。

<a name="identity_operators"></a>
### 恆等運算符

因為類是引用類型，有可能有多個常量和變量在幕後同時引用同一個類實例。（對於結構體和枚舉來說，這並不成立。因為它們作為值類型，在被賦予到常量、變量或者傳遞到函數時，其值總是會被拷貝。）

如果能夠判定兩個常量或者變量是否引用同一個類實例將會很有幫助。為了達到這個目的，Swift 內建了兩個恆等運算符：

* 等價於（`===`）
* 不等價於（`!==`）

運用這兩個運算符檢測兩個常量或者變量是否引用同一個實例：

```swift
if tenEighty === alsoTenEighty {
	print("tenEighty and alsoTenEighty refer to the same Resolution instance.")
}
//打印 "tenEighty and alsoTenEighty refer to the same Resolution instance."
```

請注意，「等價於」（用三個等號表示，`===`）與「等於」（用兩個等號表示，`==`）的不同：

* 「等價於」表示兩個類類型（class type）的常量或者變量引用同一個類實例。
* 「等於」表示兩個實例的值「相等」或「相同」，判定時要遵照設計者定義的評判標准，因此相對於「相等」來說，這是一種更加合適的叫法。

當你在定義你的自定義類和結構體的時候，你有義務來決定判定兩個實例「相等」的標准。在章節[等價操作符](./25_Advanced_Operators.html#equivalence_operators)中將會詳細介紹實現自定義「等於」和「不等於」運算符的流程。

<a name="pointers"></a>
### 指針

如果你有 C，C++ 或者 Objective-C 語言的經驗，那麼你也許會知道這些語言使用*指針*來引用內存中的地址。一個引用某個引用類型實例的 Swift 常量或者變量，與 C 語言中的指針類似，但是並不直接指向某個內存地址，也不要求你使用星號（`*`）來表明你在創建一個引用。Swift 中的這些引用與其它的常量或變量的定義方式相同。

<a name="choosing_between_classes_and_structures"></a>
## 類和結構體的選擇

在你的代碼中，你可以使用類和結構體來定義你的自定義數據類型。

然而，結構體實例總是通過值傳遞，類實例總是通過引用傳遞。這意味兩者適用不同的任務。當你在考慮一個工程項目的數據結構和功能的時候，你需要決定每個數據結構是定義成類還是結構體。

按照通用的准則，當符合一條或多條以下條件時，請考慮構建結構體：

* 該數據結構的主要目的是用來封裝少量相關簡單數據值。
* 有理由預計該數據結構的實例在被賦值或傳遞時，封裝的數據將會被拷貝而不是被引用。
* 該數據結構中儲存的值類型屬性，也應該被拷貝，而不是被引用。
* 該數據結構不需要去繼承另一個既有類型的屬性或者行為。

舉例來說，以下情境中適合使用結構體：

* 幾何形狀的大小，封裝一個`width`屬性和`height`屬性，兩者均為`Double`類型。
* 一定范圍內的路徑，封裝一個`start`屬性和`length`屬性，兩者均為`Int`類型。
* 三維坐標系內一點，封裝`x`，`y`和`z`屬性，三者均為`Double`類型。

在所有其它案例中，定義一個類，生成一個它的實例，並通過引用來管理和傳遞。實際中，這意味著絕大部分的自定義數據構造都應該是類，而非結構體。

<a name="assignment_and_copy_behavior_for_strings_arrays_and_dictionaries"></a>
## 字符串、數組、和字典類型的賦值與復制行為

Swift 中，許多基本類型，諸如`String`，`Array`和`Dictionary`類型均以結構體的形式實現。這意味著被賦值給新的常量或變量，或者被傳入函數或方法中時，它們的值會被拷貝。

Objective-C 中`NSString`，`NSArray`和`NSDictionary`類型均以類的形式實現，而並非結構體。它們在被賦值或者被傳入函數或方法時，不會發生值拷貝，而是傳遞現有實例的引用。

> 注意  
> 以上是對字符串、數組、字典的「拷貝」行為的描述。在你的代碼中，拷貝行為看起來似乎總會發生。然而，Swift 在幕後只在絕對必要時才執行實際的拷貝。Swift 管理所有的值拷貝以確保性能最優化，所以你沒必要去回避賦值來保證性能最優化。
