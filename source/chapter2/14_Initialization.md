# 構造過程
-----------------

> 1.0
> 翻譯：[lifedim](https://github.com/lifedim)
> 校對：[lifedim](https://github.com/lifedim)

> 2.0
> 翻譯+校對：[chenmingbiao](https://github.com/chenmingbiao)

> 2.1
> 翻譯：[Channe](https://github.com/Channe)，[Realank](https://github.com/Realank) 
> 校對：[shanks](http://codebuild.me)，2016-1-23

> 2.2
> 翻譯：[pmst](https://github.com/colourful987)
> 翻譯+校對：[SketchK](https://github.com/SketchK) 2016-05-14   
> 3.0.1，shanks，2016-11-13

> 3.1
> 翻譯：[qhd](https://github.com/qhd) 2017-04-18  

本頁包含內容：

- [存儲屬性的初始賦值](#setting_initial_values_for_stored_properties)
- [自定義構造過程](#customizing_initialization)
- [默認構造器](#default_initializers)
- [值類型的構造器代理](#initializer_delegation_for_value_types)
- [類的繼承和構造過程](#class_inheritance_and_initialization)
- [可失敗構造器](#failable_initializers)
- [必要構造器](#required_initializers)
- [通過閉包或函數設置屬性的默認值](#setting_a_default_property_value_with_a_closure_or_function)


*構造過程*是使用類、結構體或枚舉類型的實例之前的准備過程。在新實例可用前必須執行這個過程，具體操作包括設置實例中每個存儲型屬性的初始值和執行其他必須的設置或初始化工作。

通過定義*構造器*來實現構造過程，這些構造器可以看做是用來創建特定類型新實例的特殊方法。與 Objective-C 中的構造器不同，Swift 的構造器無需返回值，它們的主要任務是保證新實例在第一次使用前完成正確的初始化。

類的實例也可以通過定義*析構器*在實例釋放之前執行特定的清除工作。想了解更多關於析構器的內容，請參考[析構過程](./15_Deinitialization.html)。

<a name="setting_initial_values_for_stored_properties"></a>
## 存儲屬性的初始賦值

類和結構體在創建實例時，必須為所有存儲型屬性設置合適的初始值。存儲型屬性的值不能處於一個未知的狀態。

你可以在構造器中為存儲型屬性賦初值，也可以在定義屬性時為其設置默認值。以下小節將詳細介紹這兩種方法。

> 注意  
當你為存儲型屬性設置默認值或者在構造器中為其賦值時，它們的值是被直接設置的，不會觸發任何屬性觀察者。

<a name="initializers"></a>
### 構造器

構造器在創建某個特定類型的新實例時被調用。它的最簡形式類似於一個不帶任何參數的實例方法，以關鍵字`init`命名：

```swift
init() {
    // 在此處執行構造過程
}
```

下面例子中定義了一個用來保存華氏溫度的結構體`Fahrenheit`，它擁有一個`Double`類型的存儲型屬性`temperature`：

```swift
struct Fahrenheit {
    var temperature: Double
    init() {
        temperature = 32.0
    }
}
var f = Fahrenheit()
print("The default temperature is \(f.temperature)° Fahrenheit")
// 打印 "The default temperature is 32.0° Fahrenheit"
```

這個結構體定義了一個不帶參數的構造器`init`，並在裡面將存儲型屬性`temperature`的值初始化為`32.0`（華氏溫度下水的冰點）。

<a name="default_property_values"></a>
### 默認屬性值

如前所述，你可以在構造器中為存儲型屬性設置初始值。同樣，你也可以在屬性聲明時為其設置默認值。

> 注意  
如果一個屬性總是使用相同的初始值，那麼為其設置一個默認值比每次都在構造器中賦值要好。兩種方法的效果是一樣的，只不過使用默認值讓屬性的初始化和聲明結合得更緊密。使用默認值能讓你的構造器更簡潔、更清晰，且能通過默認值自動推導出屬性的類型；同時，它也能讓你充分利用默認構造器、構造器繼承等特性，後續章節將講到。

你可以使用更簡單的方式在定義結構體`Fahrenheit`時為屬性`temperature`設置默認值：

```swift
struct Fahrenheit {
    var temperature = 32.0
}
```

<a name="customizing_initialization"></a>
## 自定義構造過程

你可以通過輸入參數和可選類型的屬性來自定義構造過程，也可以在構造過程中修改常量屬性。這些都將在後面章節中提到。

<a name="initialization_parameters"></a>
### 構造參數

自定義`構造過程`時，可以在定義中提供構造參數，指定所需值的類型和名字。構造參數的功能和語法跟函數和方法的參數相同。

下面例子中定義了一個包含攝氏度溫度的結構體`Celsius`。它定義了兩個不同的構造器：`init(fromFahrenheit:)`和`init(fromKelvin:)`，二者分別通過接受不同溫標下的溫度值來創建新的實例：

```swift
struct Celsius {
    var temperatureInCelsius: Double
    init(fromFahrenheit fahrenheit: Double) {
        temperatureInCelsius = (fahrenheit - 32.0) / 1.8
    }
    init(fromKelvin kelvin: Double) {
        temperatureInCelsius = kelvin - 273.15
    }
}
let boilingPointOfWater = Celsius(fromFahrenheit: 212.0)
// boilingPointOfWater.temperatureInCelsius 是 100.0
let freezingPointOfWater = Celsius(fromKelvin: 273.15)
// freezingPointOfWater.temperatureInCelsius 是 0.0
```

第一個構造器擁有一個構造參數，其外部名字為`fromFahrenheit`，內部名字為`fahrenheit`；第二個構造器也擁有一個構造參數，其外部名字為`fromKelvin`，內部名字為`kelvin`。這兩個構造器都將唯一的參數值轉換成攝氏溫度值，並保存在屬性`temperatureInCelsius`中。

<a name="local_and_external_parameter_names"></a>
### 參數的內部名稱和外部名稱

跟函數和方法參數相同，構造參數也擁有一個在構造器內部使用的參數名字和一個在調用構造器時使用的外部參數名字。

然而，構造器並不像函數和方法那樣在括號前有一個可辨別的名字。因此在調用構造器時，主要通過構造器中的參數名和類型來確定應該被調用的構造器。正因為參數如此重要，如果你在定義構造器時沒有提供參數的外部名字，Swift 會為構造器的每個參數自動生成一個跟內部名字相同的外部名。

以下例子中定義了一個結構體`Color`，它包含了三個常量：`red`、`green`和`blue`。這些屬性可以存儲`0.0`到`1.0`之間的值，用來指示顏色中紅、綠、藍成分的含量。

`Color`提供了一個構造器，其中包含三個`Double`類型的構造參數。`Color`也可以提供第二個構造器，它只包含名為`white`的`Double`類型的參數，它被用於給上述三個構造參數賦予同樣的值。

```swift
struct Color {
    let red, green, blue: Double
    init(red: Double, green: Double, blue: Double) {
        self.red   = red
        self.green = green
        self.blue  = blue
    }
    init(white: Double) {
        red   = white
        green = white
        blue  = white
    }
}
```

兩種構造器都能用於創建一個新的`Color`實例，你需要為構造器每個外部參數傳值：

```swift
let magenta = Color(red: 1.0, green: 0.0, blue: 1.0)
let halfGray = Color(white: 0.5)
```

注意，如果不通過外部參數名字傳值，你是沒法調用這個構造器的。只要構造器定義了某個外部參數名，你就必須使用它，忽略它將導致編譯錯誤：

```swift
let veryGreen = Color(0.0, 1.0, 0.0)
// 報編譯時錯誤，需要外部名稱
```

<a name="initializer_parameters_without_external_names"></a>
### 不帶外部名的構造器參數

如果你不希望為構造器的某個參數提供外部名字，你可以使用下劃線(`_`)來顯式描述它的外部名，以此重寫上面所說的默認行為。

下面是之前`Celsius`例子的擴展，跟之前相比添加了一個帶有`Double`類型參數的構造器，其外部名用`_`代替：

```swift
struct Celsius {
    var temperatureInCelsius: Double
    init(fromFahrenheit fahrenheit: Double) {
        temperatureInCelsius = (fahrenheit - 32.0) / 1.8
    }
    init(fromKelvin kelvin: Double) {
        temperatureInCelsius = kelvin - 273.15
    }
    init(_ celsius: Double){
        temperatureInCelsius = celsius
    }
}
let bodyTemperature = Celsius(37.0)
// bodyTemperature.temperatureInCelsius 為 37.0
```

調用`Celsius(37.0)`意圖明確，不需要外部參數名稱。因此適合使用`init(_ celsius: Double)`這樣的構造器，從而可以通過提供`Double`類型的參數值調用構造器，而不需要加上外部名。

<a name="optional_property_types"></a>
### 可選屬性類型

如果你定制的類型包含一個邏輯上允許取值為空的存儲型屬性——無論是因為它無法在初始化時賦值，還是因為它在之後某個時間點可以賦值為空——你都需要將它定義為`可選類型`。可選類型的屬性將自動初始化為`nil`，表示這個屬性是有意在初始化時設置為空的。

下面例子中定義了類`SurveyQuestion`，它包含一個可選字符串屬性`response`：

```swift
class SurveyQuestion {
    var text: String
    var response: String?
    init(text: String) {
        self.text = text
    }
    func ask() {
        print(text)
    }
}
let cheeseQuestion = SurveyQuestion(text: "Do you like cheese?")
cheeseQuestion.ask()
// 打印 "Do you like cheese?"
cheeseQuestion.response = "Yes, I do like cheese."
```

調查問題的答案在回答前是無法確定的，因此我們將屬性`response`聲明為`String?`類型，或者說是`可選字符串類型`。當`SurveyQuestion`實例化時，它將自動賦值為`nil`，表明此字符串暫時還沒有值。

<a name="assigning_constant_properties_during_initialization"></a>
### 構造過程中常量屬性的修改

你可以在構造過程中的任意時間點給常量屬性指定一個值，只要在構造過程結束時是一個確定的值。一旦常量屬性被賦值，它將永遠不可更改。

> 注意  
對於類的實例來說，它的常量屬性只能在定義它的類的構造過程中修改；不能在子類中修改。

你可以修改上面的`SurveyQuestion`示例，用常量屬性替代變量屬性`text`，表示問題內容`text`在`SurveyQuestion`的實例被創建之後不會再被修改。盡管`text`屬性現在是常量，我們仍然可以在類的構造器中設置它的值：

```swift
class SurveyQuestion {
    let text: String
    var response: String?
    init(text: String) {
        self.text = text
    }
    func ask() {
        print(text)
    }
}
let beetsQuestion = SurveyQuestion(text: "How about beets?")
beetsQuestion.ask()
// 打印 "How about beets?"
beetsQuestion.response = "I also like beets. (But not with cheese.)"
```

<a name="default_initializers"></a>
## 默認構造器

如果結構體或類的所有屬性都有默認值，同時沒有自定義的構造器，那麼 Swift 會給這些結構體或類提供一個默認構造器（default initializers）。這個默認構造器將簡單地創建一個所有屬性值都設置為默認值的實例。

下面例子中創建了一個類`ShoppingListItem`，它封裝了購物清單中的某一物品的屬性：名字（`name`）、數量（`quantity`）和購買狀態 `purchase state`：

```swift
class ShoppingListItem {
    var name: String?
    var quantity = 1
    var purchased = false
}
var item = ShoppingListItem()
```

由於`ShoppingListItem`類中的所有屬性都有默認值，且它是沒有父類的基類，它將自動獲得一個可以為所有屬性設置默認值的默認構造器（盡管代碼中沒有顯式為`name`屬性設置默認值，但由於`name`是可選字符串類型，它將默認設置為`nil`）。上面例子中使用默認構造器創造了一個`ShoppingListItem`類的實例（使用`ShoppingListItem()`形式的構造器語法），並將其賦值給變量`item`。

<a name="memberwise_initializers_for_structure_types"></a>
### 結構體的逐一成員構造器

除了上面提到的默認構造器，如果結構體沒有提供自定義的構造器，它們將自動獲得一個逐一成員構造器，即使結構體的存儲型屬性沒有默認值。

逐一成員構造器是用來初始化結構體新實例裡成員屬性的快捷方法。我們在調用逐一成員構造器時，通過與成員屬性名相同的參數名進行傳值來完成對成員屬性的初始賦值。

下面例子中定義了一個結構體`Size`，它包含兩個屬性`width`和`height`。Swift 可以根據這兩個屬性的初始賦值`0.0`自動推導出它們的類型為`Double`。

結構體`Size`自動獲得了一個逐一成員構造器`init(width:height:)`。你可以用它來為`Size`創建新的實例：

```swift
struct Size {
    var width = 0.0, height = 0.0
}
let twoByTwo = Size(width: 2.0, height: 2.0)
```

<a name="initializer_delegation_for_value_types"></a>

## 值類型的構造器代理

構造器可以通過調用其它構造器來完成實例的部分構造過程。這一過程稱為構造器代理，它能減少多個構造器間的代碼重復。

構造器代理的實現規則和形式在值類型和類類型中有所不同。值類型（結構體和枚舉類型）不支持繼承，所以構造器代理的過程相對簡單，因為它們只能代理給自己的其它構造器。類則不同，它可以繼承自其它類（請參考[繼承](./13_Inheritance.html)），這意味著類有責任保證其所有繼承的存儲型屬性在構造時也能正確的初始化。這些責任將在後續章節[類的繼承和構造過程](#class_inheritance_and_initialization)中介紹。

對於值類型，你可以使用`self.init`在自定義的構造器中引用相同類型中的其它構造器。並且你只能在構造器內部調用`self.init`。

如果你為某個值類型定義了一個自定義的構造器，你將無法訪問到默認構造器（如果是結構體，還將無法訪問逐一成員構造器）。這種限制可以防止你為值類型增加了一個額外的且十分復雜的構造器之後,仍然有人錯誤的使用自動生成的構造器

> 注意  
假如你希望默認構造器、逐一成員構造器以及你自己的自定義構造器都能用來創建實例，可以將自定義的構造器寫到擴展（`extension`）中，而不是寫在值類型的原始定義中。想查看更多內容，請查看[擴展](./21_Extensions.html)章節。

下面例子將定義一個結構體`Rect`，用來代表幾何矩形。這個例子需要兩個輔助的結構體`Size`和`Point`，它們各自為其所有的屬性提供了初始值`0.0`。

```swift
struct Size {
    var width = 0.0, height = 0.0
}
struct Point {
    var x = 0.0, y = 0.0
}
```

你可以通過以下三種方式為`Rect`創建實例——使用被初始化為默認值的`origin`和`size`屬性來初始化；提供指定的`origin`和`size`實例來初始化；提供指定的`center`和`size`來初始化。在下面`Rect`結構體定義中，我們為這三種方式提供了三個自定義的構造器：

```swift
struct Rect {
    var origin = Point()
    var size = Size()
    init() {}
    init(origin: Point, size: Size) {
        self.origin = origin
        self.size = size
    }
    init(center: Point, size: Size) {
        let originX = center.x - (size.width / 2)
        let originY = center.y - (size.height / 2)
        self.init(origin: Point(x: originX, y: originY), size: size)
    }
}
```

第一個`Rect`構造器`init()`，在功能上跟沒有自定義構造器時自動獲得的默認構造器是一樣的。這個構造器是一個空函數，使用一對大括號`{}`來表示，它沒有執行任何構造過程。調用這個構造器將返回一個`Rect`實例，它的`origin`和`size`屬性都使用定義時的默認值`Point(x: 0.0, y: 0.0)`和`Size(width: 0.0, height: 0.0)`：

```swift
let basicRect = Rect()
// basicRect 的 origin 是 (0.0, 0.0)，size 是 (0.0, 0.0)
```

第二個`Rect`構造器`init(origin:size:)`，在功能上跟結構體在沒有自定義構造器時獲得的逐一成員構造器是一樣的。這個構造器只是簡單地將`origin`和`size`的參數值賦給對應的存儲型屬性：

```swift
let originRect = Rect(origin: Point(x: 2.0, y: 2.0),
    size: Size(width: 5.0, height: 5.0))
// originRect 的 origin 是 (2.0, 2.0)，size 是 (5.0, 5.0)
```

第三個`Rect`構造器`init(center:size:)`稍微復雜一點。它先通過`center`和`size`的值計算出`origin`的坐標，然後再調用（或者說代理給）`init(origin:size:)`構造器來將新的`origin`和`size`值賦值到對應的屬性中：

```swift
let centerRect = Rect(center: Point(x: 4.0, y: 4.0),
    size: Size(width: 3.0, height: 3.0))
// centerRect 的 origin 是 (2.5, 2.5)，size 是 (3.0, 3.0)
```

構造器`init(center:size:)`可以直接將`origin`和`size`的新值賦值到對應的屬性中。然而，利用恰好提供了相關功能的現有構造器會更為方便，構造器`init(center:size:)`的意圖也會更加清晰。

> 注意  
如果你想用另外一種不需要自己定義`init()`和`init(origin:size:)`的方式來實現這個例子，請參考[擴展](./21_Extensions.html)。

<a name="class_inheritance_and_initialization"></a>
## 類的繼承和構造過程

類裡面的所有存儲型屬性——包括所有繼承自父類的屬性——都必須在構造過程中設置初始值。

Swift 為類類型提供了兩種構造器來確保實例中所有存儲型屬性都能獲得初始值，它們分別是指定構造器和便利構造器。

<a name="designated_initializers_and_convenience_initializers"></a>
### 指定構造器和便利構造器

*指定構造器*是類中最主要的構造器。一個指定構造器將初始化類中提供的所有屬性，並根據父類鏈往上調用父類的構造器來實現父類的初始化。

每一個類都必須擁有至少一個指定構造器。在某些情況下，許多類通過繼承了父類中的指定構造器而滿足了這個條件。具體內容請參考後續章節[構造器的自動繼承](#automatic_initializer_inheritance)。

*便利構造器*是類中比較次要的、輔助型的構造器。你可以定義便利構造器來調用同一個類中的指定構造器，並為其參數提供默認值。你也可以定義便利構造器來創建一個特殊用途或特定輸入值的實例。

你應當只在必要的時候為類提供便利構造器，比方說某種情況下通過使用便利構造器來快捷調用某個指定構造器，能夠節省更多開發時間並讓類的構造過程更清晰明了。

<a name="syntax_for_designated_and_convenience_initializers"></a>
### 指定構造器和便利構造器的語法

類的指定構造器的寫法跟值類型簡單構造器一樣：

```swift
init(parameters) {
    statements
}
```

便利構造器也采用相同樣式的寫法，但需要在`init`關鍵字之前放置`convenience`關鍵字，並使用空格將它們倆分開：

```swift
convenience init(parameters) {
    statements
}
```

<a name="initializer_delegation_for_class_types"></a>
### 類的構造器代理規則

為了簡化指定構造器和便利構造器之間的調用關系，Swift 采用以下三條規則來限制構造器之間的代理調用：

##### 規則 1
指定構造器必須調用其直接父類的的指定構造器。

##### 規則 2
便利構造器必須調用*同*類中定義的其它構造器。

##### 規則 3
便利構造器必須最終導致一個指定構造器被調用。

一個更方便記憶的方法是：

- 指定構造器必須總是*向上*代理
- 便利構造器必須總是*橫向*代理

這些規則可以通過下面圖例來說明：

![構造器代理圖](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/initializerDelegation01_2x.png)

如圖所示，父類中包含一個指定構造器和兩個便利構造器。其中一個便利構造器調用了另外一個便利構造器，而後者又調用了唯一的指定構造器。這滿足了上面提到的規則 2 和 3。這個父類沒有自己的父類，所以規則 1 沒有用到。

子類中包含兩個指定構造器和一個便利構造器。便利構造器必須調用兩個指定構造器中的任意一個，因為它只能調用同一個類裡的其他構造器。這滿足了上面提到的規則 2 和 3。而兩個指定構造器必須調用父類中唯一的指定構造器，這滿足了規則 1。

> 注意  
這些規則不會影響類的實例如何創建。任何上圖中展示的構造器都可以用來創建完全初始化的實例。這些規則只影響類定義如何實現。

下面圖例中展示了一種涉及四個類的更復雜的類層級結構。它演示了指定構造器是如何在類層級中充當「管道」的作用，在類的構造器鏈上簡化了類之間的相互關系。

![復雜構造器代理圖](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/initializerDelegation02_2x.png)

<a name="two_phase_initialization"></a>
### 兩段式構造過程

Swift 中類的構造過程包含兩個階段。第一個階段，每個存儲型屬性被引入它們的類指定一個初始值。當每個存儲型屬性的初始值被確定後，第二階段開始，它給每個類一次機會，在新實例准備使用之前進一步定制它們的存儲型屬性。

兩段式構造過程的使用讓構造過程更安全，同時在整個類層級結構中給予了每個類完全的靈活性。兩段式構造過程可以防止屬性值在初始化之前被訪問，也可以防止屬性被另外一個構造器意外地賦予不同的值。

> 注意  
Swift 的兩段式構造過程跟 Objective-C 中的構造過程類似。最主要的區別在於階段 1，Objective-C 給每一個屬性賦值`0`或空值（比如說`0`或`nil`）。Swift  的構造流程則更加靈活，它允許你設置定制的初始值，並自如應對某些屬性不能以`0`或`nil`作為合法默認值的情況。

Swift 編譯器將執行 4 種有效的安全檢查，以確保兩段式構造過程能不出錯地完成：

##### 安全檢查 1

指定構造器必須保證它所在類引入的所有屬性都必須先初始化完成，之後才能將其它構造任務向上代理給父類中的構造器。

如上所述，一個對象的內存只有在其所有存儲型屬性確定之後才能完全初始化。為了滿足這一規則，指定構造器必須保證它所在類引入的屬性在它往上代理之前先完成初始化。

##### 安全檢查 2

指定構造器必須先向上代理調用父類構造器，然後再為繼承的屬性設置新值。如果沒這麼做，指定構造器賦予的新值將被父類中的構造器所覆蓋。

##### 安全檢查 3

便利構造器必須先代理調用同一類中的其它構造器，然後再為任意屬性賦新值。如果沒這麼做，便利構造器賦予的新值將被同一類中其它指定構造器所覆蓋。

##### 安全檢查 4

構造器在第一階段構造完成之前，不能調用任何實例方法，不能讀取任何實例屬性的值，不能引用`self`作為一個值。

類實例在第一階段結束以前並不是完全有效的。只有第一階段完成後，該實例才會成為有效實例，才能訪問屬性和調用方法。

以下是兩段式構造過程中基於上述安全檢查的構造流程展示：

##### 階段 1

- 某個指定構造器或便利構造器被調用。
- 完成新實例內存的分配，但此時內存還沒有被初始化。
- 指定構造器確保其所在類引入的所有存儲型屬性都已賦初值。存儲型屬性所屬的內存完成初始化。
- 指定構造器將調用父類的構造器，完成父類屬性的初始化。
- 這個調用父類構造器的過程沿著構造器鏈一直往上執行，直到到達構造器鏈的最頂部。
- 當到達了構造器鏈最頂部，且已確保所有實例包含的存儲型屬性都已經賦值，這個實例的內存被認為已經完全初始化。此時階段 1 完成。

##### 階段 2

- 從頂部構造器鏈一直往下，每個構造器鏈中類的指定構造器都有機會進一步定制實例。構造器此時可以訪問`self`、修改它的屬性並調用實例方法等等。
- 最終，任意構造器鏈中的便利構造器可以有機會定制實例和使用`self`。

下圖展示了在假定的子類和父類之間的構造階段 1：

![構建過程階段1](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/twoPhaseInitialization01_2x.png)

在這個例子中，構造過程從對子類中一個便利構造器的調用開始。這個便利構造器此時沒法修改任何屬性，它把構造任務代理給同一類中的指定構造器。

如安全檢查 1 所示，指定構造器將確保所有子類的屬性都有值。然後它將調用父類的指定構造器，並沿著構造器鏈一直往上完成父類的構造過程。

父類中的指定構造器確保所有父類的屬性都有值。由於沒有更多的父類需要初始化，也就無需繼續向上代理。

一旦父類中所有屬性都有了初始值，實例的內存被認為是完全初始化，階段 1 完成。

以下展示了相同構造過程的階段 2：

![構建過程階段2](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/twoPhaseInitialization02_2x.png)

父類中的指定構造器現在有機會進一步來定制實例（盡管這不是必須的）。

一旦父類中的指定構造器完成調用，子類中的指定構造器可以執行更多的定制操作（這也不是必須的）。

最終，一旦子類的指定構造器完成調用，最開始被調用的便利構造器可以執行更多的定制操作。

<a name="initializer_inheritance_and_overriding"></a>
### 構造器的繼承和重寫

跟 Objective-C 中的子類不同，Swift 中的子類默認情況下不會繼承父類的構造器。Swift 的這種機制可以防止一個父類的簡單構造器被一個更精細的子類繼承，並被錯誤地用來創建子類的實例。

> 注意  
父類的構造器僅會在安全和適當的情況下被繼承。具體內容請參考後續章節[構造器的自動繼承](#automatic_initializer_inheritance)。

假如你希望自定義的子類中能提供一個或多個跟父類相同的構造器，你可以在子類中提供這些構造器的自定義實現。

當你在編寫一個和父類中指定構造器相匹配的子類構造器時，你實際上是在重寫父類的這個指定構造器。因此，你必須在定義子類構造器時帶上`override`修飾符。即使你重寫的是系統自動提供的默認構造器，也需要帶上`override`修飾符，具體內容請參考[默認構造器](#default_initializers)。

正如重寫屬性，方法或者是下標，`override`修飾符會讓編譯器去檢查父類中是否有相匹配的指定構造器，並驗證構造器參數是否正確。

> 注意  
當你重寫一個父類的指定構造器時，你總是需要寫`override`修飾符，即使你的子類將父類的指定構造器重寫為了便利構造器。

相反，如果你編寫了一個和父類便利構造器相匹配的子類構造器，由於子類不能直接調用父類的便利構造器（每個規則都在上文[類的構造器代理規則](#initializer_delegation_for_class_types)有所描述），因此，嚴格意義上來講，你的子類並未對一個父類構造器提供重寫。最後的結果就是，你在子類中「重寫」一個父類便利構造器時，不需要加`override`前綴。

在下面的例子中定義了一個叫`Vehicle`的基類。基類中聲明了一個存儲型屬性`numberOfWheels`，它是值為`0`的`Int`類型的存儲型屬性。`numberOfWheels`屬性用於創建名為`descrpiption`的`String`類型的計算型屬性：

```swift
class Vehicle {
    var numberOfWheels = 0
    var description: String {
        return "\(numberOfWheels) wheel(s)"
    }
}
```

`Vehicle`類只為存儲型屬性提供默認值，而不自定義構造器。因此，它會自動獲得一個默認構造器，具體內容請參考[默認構造器](#default_initializers)。自動獲得的默認構造器總會是類中的指定構造器，它可以用於創建`numberOfWheels`為`0`的`Vehicle`實例：

```swift
let vehicle = Vehicle()
print("Vehicle: \(vehicle.description)")
// Vehicle: 0 wheel(s)
```

下面例子中定義了一個`Vehicle`的子類`Bicycle`：

```swift
class Bicycle: Vehicle {
    override init() {
        super.init()
        numberOfWheels = 2
    }
}
```

子類`Bicycle`定義了一個自定義指定構造器`init()`。這個指定構造器和父類的指定構造器相匹配，所以`Bicycle`中的指定構造器需要帶上`override`修飾符。

`Bicycle`的構造器`init()`以調用`super.init()`方法開始，這個方法的作用是調用`Bicycle`的父類`Vehicle`的默認構造器。這樣可以確保`Bicycle`在修改屬性之前，它所繼承的屬性`numberOfWheels`能被`Vehicle`類初始化。在調用`super.init()`之後，屬性`numberOfWheels`的原值被新值`2`替換。

如果你創建一個`Bicycle`實例，你可以調用繼承的`description`計算型屬性去查看屬性`numberOfWheels`是否有改變：

```swift
let bicycle = Bicycle()
print("Bicycle: \(bicycle.description)")
// 打印 "Bicycle: 2 wheel(s)"
```

> 注意  
子類可以在初始化時修改繼承來的變量屬性，但是不能修改繼承來的常量屬性。

<a name="automatic_initializer_inheritance"></a>
### 構造器的自動繼承

如上所述，子類在默認情況下不會繼承父類的構造器。但是如果滿足特定條件，父類構造器是可以被自動繼承的。在實踐中，這意味著對於許多常見場景你不必重寫父類的構造器，並且可以在安全的情況下以最小的代價繼承父類的構造器。

假設你為子類中引入的所有新屬性都提供了默認值，以下 2 個規則適用：

##### 規則 1

如果子類沒有定義任何指定構造器，它將自動繼承所有父類的指定構造器。

##### 規則 2

如果子類提供了所有父類指定構造器的實現——無論是通過規則 1 繼承過來的，還是提供了自定義實現——它將自動繼承所有父類的便利構造器。

即使你在子類中添加了更多的便利構造器，這兩條規則仍然適用。

> 注意  
對於規則 2，子類可以將父類的指定構造器實現為便利構造器。

<a name="designated_and_convenience_initializers_in_action"></a>
### 指定構造器和便利構造器實踐

接下來的例子將在實踐中展示指定構造器、便利構造器以及構造器的自動繼承。這個例子定義了包含三個類`Food`、`RecipeIngredient`以及`ShoppingListItem`的類層次結構，並將演示它們的構造器是如何相互作用的。

類層次中的基類是`Food`，它是一個簡單的用來封裝食物名字的類。`Food`類引入了一個叫做`name`的`String`類型的屬性，並且提供了兩個構造器來創建`Food`實例：

```swift
class Food {
    var name: String
    init(name: String) {
        self.name = name
    }
    convenience init() {
        self.init(name: "[Unnamed]")
    }
}
```

下圖中展示了`Food`的構造器鏈：

![Food構造器鏈](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/initializersExample01_2x.png)

類類型沒有默認的逐一成員構造器，所以`Food`類提供了一個接受單一參數`name`的指定構造器。這個構造器可以使用一個特定的名字來創建新的`Food`實例：

```swift
let namedMeat = Food(name: "Bacon")
// namedMeat 的名字是 "Bacon"
```

`Food`類中的構造器`init(name: String)`被定義為一個指定構造器，因為它能確保`Food`實例的所有存儲型屬性都被初始化。`Food`類沒有父類，所以`init(name: String)`構造器不需要調用`super.init()`來完成構造過程。

`Food`類同樣提供了一個沒有參數的便利構造器`init()`。這個`init()`構造器為新食物提供了一個默認的占位名字，通過橫向代理到指定構造器`init(name: String)`並給參數`name`傳值`[Unnamed]`來實現：

```swift
let mysteryMeat = Food()
// mysteryMeat 的名字是 [Unnamed]
```

類層級中的第二個類是`Food`的子類`RecipeIngredient`。`RecipeIngredient`類用來表示食譜中的一項原料。它引入了`Int`類型的屬性`quantity`（以及從`Food`繼承過來的`name`屬性），並且定義了兩個構造器來創建`RecipeIngredient`實例：

```swift
class RecipeIngredient: Food {
    var quantity: Int
    init(name: String, quantity: Int) {
        self.quantity = quantity
        super.init(name: name)
    }
    override convenience init(name: String) {
        self.init(name: name, quantity: 1)
    }
}
```

下圖中展示了`RecipeIngredient`類的構造器鏈：

![RecipeIngredient構造器](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/initializersExample02_2x.png)

`RecipeIngredient`類擁有一個指定構造器`init(name: String, quantity: Int)`，它可以用來填充`RecipeIngredient`實例的所有屬性值。這個構造器一開始先將傳入的`quantity`參數賦值給`quantity`屬性，這個屬性也是唯一在`RecipeIngredient`中新引入的屬性。隨後，構造器向上代理到父類`Food`的`init(name: String)`。這個過程滿足[兩段式構造過程](#two_phase_initialization)中的安全檢查 1。

`RecipeIngredient`還定義了一個便利構造器`init(name: String)`，它只通過`name`來創建`RecipeIngredient`的實例。這個便利構造器假設任意`RecipeIngredient`實例的`quantity`為`1`，所以不需要顯式指明數量即可創建出實例。這個便利構造器的定義可以更加方便和快捷地創建實例，並且避免了創建多個`quantity`為`1`的`RecipeIngredient`實例時的代碼重復。這個便利構造器只是簡單地橫向代理到類中的指定構造器，並為`quantity`參數傳遞`1`。

注意，`RecipeIngredient`的便利構造器`init(name: String)`使用了跟`Food`中指定構造器`init(name: String)`相同的參數。由於這個便利構造器重寫了父類的指定構造器`init(name: String)`，因此必須在前面使用`override`修飾符（參見[構造器的繼承和重寫](#initializer_inheritance_and_overriding)）。

盡管`RecipeIngredient`將父類的指定構造器重寫為了便利構造器，它依然提供了父類的所有指定構造器的實現。因此，`RecipeIngredient`會自動繼承父類的所有便利構造器。

在這個例子中，`RecipeIngredient`的父類是`Food`，它有一個便利構造器`init()`。這個便利構造器會被`RecipeIngredient`繼承。這個繼承版本的`init()`在功能上跟`Food`提供的版本是一樣的，只是它會代理到`RecipeIngredient`版本的`init(name: String)`而不是`Food`提供的版本。

所有的這三種構造器都可以用來創建新的`RecipeIngredient`實例：

```swift
let oneMysteryItem = RecipeIngredient()
let oneBacon = RecipeIngredient(name: "Bacon")
let sixEggs = RecipeIngredient(name: "Eggs", quantity: 6)
```

類層級中第三個也是最後一個類是`RecipeIngredient`的子類，叫做`ShoppingListItem`。這個類構建了購物單中出現的某一種食譜原料。

購物單中的每一項總是從未購買狀態開始的。為了呈現這一事實，`ShoppingListItem`引入了一個布爾類型的屬性`purchased`，它的默認值是`false`。`ShoppingListItem`還添加了一個計算型屬性`description`，它提供了關於`ShoppingListItem`實例的一些文字描述：

```swift
class ShoppingListItem: RecipeIngredient {
    var purchased = false
    var description: String {
        var output = "\(quantity) x \(name)"
        output += purchased ? " ✔" : " ✘"
        return output
    }
}
```

> 注意  
`ShoppingListItem`沒有定義構造器來為`purchased`提供初始值，因為添加到購物單的物品的初始狀態總是未購買。

由於它為自己引入的所有屬性都提供了默認值，並且自己沒有定義任何構造器，`ShoppingListItem`將自動繼承所有父類中的指定構造器和便利構造器。

下圖展示了這三個類的構造器鏈：

![三類構造器圖](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/initializersExample03_2x.png)

你可以使用全部三個繼承來的構造器來創建`ShoppingListItem`的新實例：

```swift
var breakfastList = [
    ShoppingListItem(),
    ShoppingListItem(name: "Bacon"),
    ShoppingListItem(name: "Eggs", quantity: 6),
]
breakfastList[0].name = "Orange juice"
breakfastList[0].purchased = true
for item in breakfastList {
    print(item.description)
}
// 1 x orange juice ✔
// 1 x bacon ✘
// 6 x eggs ✘
```

如上所述，例子中通過字面量方式創建了一個數組`breakfastList`，它包含了三個`ShoppingListItem`實例，因此數組的類型也能被自動推導為`[ShoppingListItem]`。在數組創建完之後，數組中第一個`ShoppingListItem`實例的名字從`[Unnamed]`更改為`Orange juice`，並標記為已購買。打印數組中每個元素的描述顯示了它們都已按照預期被賦值。

<a name="failable_initializers"></a>
## 可失敗構造器

如果一個類、結構體或枚舉類型的對象，在構造過程中有可能失敗，則為其定義一個可失敗構造器。這裡所指的「失敗」是指，如給構造器傳入無效的參數值，或缺少某種所需的外部資源，又或是不滿足某種必要的條件等。

為了妥善處理這種構造過程中可能會失敗的情況。你可以在一個類，結構體或是枚舉類型的定義中，添加一個或多個可失敗構造器。其語法為在`init`關鍵字後面添加問號(`init?`)。

> 注意  
可失敗構造器的參數名和參數類型，不能與其它非可失敗構造器的參數名，及其參數類型相同。

可失敗構造器會創建一個類型為自身類型的可選類型的對象。你通過`return nil`語句來表明可失敗構造器在何種情況下應該「失敗」。

> 注意  
嚴格來說，構造器都不支持返回值。因為構造器本身的作用，只是為了確保對象能被正確構造。因此你只是用`return nil`表明可失敗構造器構造失敗，而不要用關鍵字`return`來表明構造成功。

例如，實現針對數字類型轉換的可失敗構造器。確保數字類型之間的轉換能保持精確的值，使用這個 `init(exactly:)` 構造器。如果類型轉換不能保持值不變，則這個構造器構造失敗。  

```
let wholeNumber: Double = 12345.0
let pi = 3.14159
 
if let valueMaintained = Int(exactly: wholeNumber) {
    print("\(wholeNumber) conversion to Int maintains value")
}
// 打印 "12345.0 conversion to Int maintains value"
 
let valueChanged = Int(exactly: pi)
// valueChanged 是 Int? 類型, 不是 Int 類型
 
if valueChanged == nil {
    print("\(pi) conversion to Int does not maintain value")
}
// 打印 "3.14159 conversion to Int does not maintain value"
```

下例中，定義了一個名為`Animal`的結構體，其中有一個名為`species`的`String`類型的常量屬性。同時該結構體還定義了一個接受一個名為`species`的`String`類型參數的可失敗構造器。這個可失敗構造器檢查傳入的參數是否為一個空字符串。如果為空字符串，則構造失敗。否則，`species`屬性被賦值，構造成功。

```swift
struct Animal {
    let species: String
    init?(species: String) {
        if species.isEmpty { return nil }
        self.species = species
    }
}
```

你可以通過該可失敗構造器來構建一個`Animal`的實例，並檢查構造過程是否成功：

```swift
let someCreature = Animal(species: "Giraffe")
// someCreature 的類型是 Animal? 而不是 Animal

if let giraffe = someCreature {
    print("An animal was initialized with a species of \(giraffe.species)")
}
// 打印 "An animal was initialized with a species of Giraffe"
```

如果你給該可失敗構造器傳入一個空字符串作為其參數，則會導致構造失敗：

```swift
let anonymousCreature = Animal(species: "")
// anonymousCreature 的類型是 Animal?, 而不是 Animal

if anonymousCreature == nil {
    print("The anonymous creature could not be initialized")
}
// 打印 "The anonymous creature could not be initialized"
```

> 注意  
空字符串（如`""`，而不是`"Giraffe"`）和一個值為`nil`的可選類型的字符串是兩個完全不同的概念。上例中的空字符串（`""`）其實是一個有效的，非可選類型的字符串。這裡我們之所以讓`Animal`的可失敗構造器構造失敗，只是因為對於`Animal`這個類的`species`屬性來說，它更適合有一個具體的值，而不是空字符串。

<a name="failable_nitializers_for_enumerations"></a>
### 枚舉類型的可失敗構造器

你可以通過一個帶一個或多個參數的可失敗構造器來獲取枚舉類型中特定的枚舉成員。如果提供的參數無法匹配任何枚舉成員，則構造失敗。

下例中，定義了一個名為`TemperatureUnit`的枚舉類型。其中包含了三個可能的枚舉成員(`Kelvin`，`Celsius`，和`Fahrenheit`)，以及一個根據`Character`值找出所對應的枚舉成員的可失敗構造器：

```swift
enum TemperatureUnit {
    case Kelvin, Celsius, Fahrenheit
    init?(symbol: Character) {
        switch symbol {
        case "K":
            self = .Kelvin
        case "C":
            self = .Celsius
        case "F":
            self = .Fahrenheit
        default:
            return nil
        }
    }
}
```

你可以利用該可失敗構造器在三個枚舉成員中獲取一個相匹配的枚舉成員，當參數的值不能與任何枚舉成員相匹配時，則構造失敗：

```swift
let fahrenheitUnit = TemperatureUnit(symbol: "F")
if fahrenheitUnit != nil {
    print("This is a defined temperature unit, so initialization succeeded.")
}
// 打印 "This is a defined temperature unit, so initialization succeeded."

let unknownUnit = TemperatureUnit(symbol: "X")
if unknownUnit == nil {
    print("This is not a defined temperature unit, so initialization failed.")
}
// 打印 "This is not a defined temperature unit, so initialization failed."
```

<a name="failable_initializers_for_enumerations_with_raw_values"></a>
### 帶原始值的枚舉類型的可失敗構造器

帶原始值的枚舉類型會自帶一個可失敗構造器`init?(rawValue:)`，該可失敗構造器有一個名為`rawValue`的參數，其類型和枚舉類型的原始值類型一致，如果該參數的值能夠和某個枚舉成員的原始值匹配，則該構造器會構造相應的枚舉成員，否則構造失敗。

因此上面的`TemperatureUnit`的例子可以重寫為：

```swift
enum TemperatureUnit: Character {
    case Kelvin = "K", Celsius = "C", Fahrenheit = "F"
}

let fahrenheitUnit = TemperatureUnit(rawValue: "F")
if fahrenheitUnit != nil {
    print("This is a defined temperature unit, so initialization succeeded.")
}
// 打印 "This is a defined temperature unit, so initialization succeeded."

let unknownUnit = TemperatureUnit(rawValue: "X")
if unknownUnit == nil {
    print("This is not a defined temperature unit, so initialization failed.")
}
// 打印 "This is not a defined temperature unit, so initialization failed."
```

<a name="propagation_of_initialization_failure"></a>
### 構造失敗的傳遞

類，結構體，枚舉的可失敗構造器可以橫向代理到類型中的其他可失敗構造器。類似的，子類的可失敗構造器也能向上代理到父類的可失敗構造器。

無論是向上代理還是橫向代理，如果你代理到的其他可失敗構造器觸發構造失敗，整個構造過程將立即終止，接下來的任何構造代碼不會再被執行。

> 注意  
可失敗構造器也可以代理到其它的非可失敗構造器。通過這種方式，你可以增加一個可能的失敗狀態到現有的構造過程中。

下面這個例子，定義了一個名為`CartItem`的`Product`類的子類。這個類建立了一個在線購物車中的物品的模型，它有一個名為`quantity`的常量存儲型屬性，並確保該屬性的值至少為`1`：


```swift
class Product {
    let name: String
    init?(name: String) {   
        if name.isEmpty { return nil }
        self.name = name
    }
}

class CartItem: Product {
    let quantity: Int
    init?(name: String, quantity: Int) {
        if quantity < 1 { return nil }
        self.quantity = quantity
        super.init(name: name)
    }
}
```

`CartItem` 可失敗構造器首先驗證接收的 `quantity` 值是否大於等於 1 。倘若 `quantity` 值無效，則立即終止整個構造過程，返回失敗結果，且不再執行余下代碼。同樣地，`Product` 的可失敗構造器首先檢查 `name` 值，假如 `name` 值為空字符串，則構造器立即執行失敗。

如果你通過傳入一個非空字符串 `name` 以及一個值大於等於 1 的 `quantity` 來創建一個 `CartItem` 實例，那麼構造方法能夠成功被執行：

```swift
if let twoSocks = CartItem(name: "sock", quantity: 2) {
    print("Item: \(twoSocks.name), quantity: \(twoSocks.quantity)")
}
// 打印 "Item: sock, quantity: 2"
```

倘若你以一個值為 0 的 `quantity` 來創建一個 `CartItem` 實例，那麼將導致 `CartItem` 構造器失敗：

```swift
if let zeroShirts = CartItem(name: "shirt", quantity: 0) {
    print("Item: \(zeroShirts.name), quantity: \(zeroShirts.quantity)")
} else {
    print("Unable to initialize zero shirts")
}
// 打印 "Unable to initialize zero shirts"
```  

同樣地，如果你嘗試傳入一個值為空字符串的 `name`來創建一個 `CartItem` 實例，那麼將導致父類 `Product` 的構造過程失敗：

```swift
if let oneUnnamed = CartItem(name: "", quantity: 1) {
    print("Item: \(oneUnnamed.name), quantity: \(oneUnnamed.quantity)")
} else {
    print("Unable to initialize one unnamed product")
}
// 打印 "Unable to initialize one unnamed product"
```

<a name="overriding_a_failable_initializer"></a>
### 重寫一個可失敗構造器

如同其它的構造器，你可以在子類中重寫父類的可失敗構造器。或者你也可以用子類的非可失敗構造器重寫一個父類的可失敗構造器。這使你可以定義一個不會構造失敗的子類，即使父類的構造器允許構造失敗。

注意，當你用子類的非可失敗構造器重寫父類的可失敗構造器時，向上代理到父類的可失敗構造器的唯一方式是對父類的可失敗構造器的返回值進行強制解包。

> 注意  
你可以用非可失敗構造器重寫可失敗構造器，但反過來卻不行。

下例定義了一個名為`Document`的類，`name`屬性的值必須為一個非空字符串或`nil`，但不能是一個空字符串：

```swift
class Document {
    var name: String?
    // 該構造器創建了一個 name 屬性的值為 nil 的 document 實例
    init() {}
    // 該構造器創建了一個 name 屬性的值為非空字符串的 document 實例
    init?(name: String) {
        self.name = name
        if name.isEmpty { return nil }
    }
}
```

下面這個例子，定義了一個`Document`類的子類`AutomaticallyNamedDocument`。這個子類重寫了父類的兩個指定構造器，確保了無論是使用`init()`構造器，還是使用`init(name:)`構造器並為參數傳遞空字符串，生成的實例中的`name`屬性總有初始`"[Untitled]"`：

```swift
class AutomaticallyNamedDocument: Document {
    override init() {
        super.init()
        self.name = "[Untitled]"
    }
    override init(name: String) {
        super.init()
        if name.isEmpty {
            self.name = "[Untitled]"
        } else {
            self.name = name
        }
    }
}
```

`AutomaticallyNamedDocument`用一個非可失敗構造器`init(name:)`重寫了父類的可失敗構造器`init?(name:)`。因為子類用另一種方式處理了空字符串的情況，所以不再需要一個可失敗構造器，因此子類用一個非可失敗構造器代替了父類的可失敗構造器。

你可以在子類的非可失敗構造器中使用強制解包來調用父類的可失敗構造器。比如，下面的`UntitledDocument`子類的`name`屬性的值總是`"[Untitled]"`，它在構造過程中使用了父類的可失敗構造器`init?(name:)`：

```swift
class UntitledDocument: Document {
    override init() {
        super.init(name: "[Untitled]")!
    }
}
```

在這個例子中，如果在調用父類的可失敗構造器`init?(name:)`時傳入的是空字符串，那麼強制解包操作會引發運行時錯誤。不過，因為這裡是通過非空的字符串常量來調用它，所以並不會發生運行時錯誤。

<a name="the_init!_failable_initializer"></a>
### 可失敗構造器 init!

通常來說我們通過在`init`關鍵字後添加問號的方式（`init?`）來定義一個可失敗構造器，但你也可以通過在`init`後面添加驚嘆號的方式來定義一個可失敗構造器（`init!`），該可失敗構造器將會構建一個對應類型的隱式解包可選類型的對象。

你可以在`init?`中代理到`init!`，反之亦然。你也可以用`init?`重寫`init!`，反之亦然。你還可以用`init`代理到`init!`，不過，一旦`init!`構造失敗，則會觸發一個斷言。

<a name="required_initializers"></a>
## 必要構造器

在類的構造器前添加`required`修飾符表明所有該類的子類都必須實現該構造器：

```swift
class SomeClass {
    required init() {
        // 構造器的實現代碼
    }
}
```

在子類重寫父類的必要構造器時，必須在子類的構造器前也添加`required`修飾符，表明該構造器要求也應用於繼承鏈後面的子類。在重寫父類中必要的指定構造器時，不需要添加`override`修飾符：

```swift
class SomeSubclass: SomeClass {
    required init() {
        // 構造器的實現代碼
    }
}
```

> 注意  
如果子類繼承的構造器能滿足必要構造器的要求，則無須在子類中顯式提供必要構造器的實現。

<a name="setting_a_default_property_value_with_a_closure_or_function"></a>
## 通過閉包或函數設置屬性的默認值

如果某個存儲型屬性的默認值需要一些定制或設置，你可以使用閉包或全局函數為其提供定制的默認值。每當某個屬性所在類型的新實例被創建時，對應的閉包或函數會被調用，而它們的返回值會當做默認值賦值給這個屬性。

這種類型的閉包或函數通常會創建一個跟屬性類型相同的臨時變量，然後修改它的值以滿足預期的初始狀態，最後返回這個臨時變量，作為屬性的默認值。

下面介紹了如何用閉包為屬性提供默認值：

```swift
class SomeClass {
    let someProperty: SomeType = {
        // 在這個閉包中給 someProperty 創建一個默認值
        // someValue 必須和 SomeType 類型相同
        return someValue
    }()
}
```

注意閉包結尾的大括號後面接了一對空的小括號。這用來告訴 Swift 立即執行此閉包。如果你忽略了這對括號，相當於將閉包本身作為值賦值給了屬性，而不是將閉包的返回值賦值給屬性。

> 注意  
如果你使用閉包來初始化屬性，請記住在閉包執行時，實例的其它部分都還沒有初始化。這意味著你不能在閉包裡訪問其它屬性，即使這些屬性有默認值。同樣，你也不能使用隱式的`self`屬性，或者調用任何實例方法。

下面例子中定義了一個結構體`Checkerboard`，它構建了西洋跳棋游戲的棋盤：

![西洋跳棋棋盤](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/chessBoard_2x.png)

西洋跳棋游戲在一副黑白格交替的 8 x 8 的棋盤中進行。為了呈現這副游戲棋盤，`Checkerboard`結構體定義了一個屬性`boardColors`，它是一個包含`100`個`Bool`值的數組。在數組中，值為`true`的元素表示一個黑格，值為`false`的元素表示一個白格。數組中第一個元素代表棋盤上左上角的格子，最後一個元素代表棋盤上右下角的格子。

`boardColor`數組是通過一個閉包來初始化並設置顏色值的：

```swift
struct Checkerboard {
    let boardColors: [Bool] = {
        var temporaryBoard = [Bool]()
        var isBlack = false
        for i in 1...8 {
            for j in 1...8 {
                temporaryBoard.append(isBlack)
                isBlack = !isBlack
            }
            isBlack = !isBlack
        }
        return temporaryBoard
    }()
    func squareIsBlackAtRow(row: Int, column: Int) -> Bool {
        return boardColors[(row * 8) + column]
    }
}
```

每當一個新的`Checkerboard`實例被創建時，賦值閉包會被執行，`boardColors`的默認值會被計算出來並返回。上面例子中描述的閉包將計算出棋盤中每個格子對應的顏色，並將這些值保存到一個臨時數組`temporaryBoard`中，最後在構建完成時將此數組作為閉包返回值返回。這個返回的數組會保存到`boardColors`中，並可以通過工具函數`squareIsBlackAtRow`來查詢：

```swift
let board = Checkerboard()
print(board.squareIsBlackAtRow(0, column: 1))
// 打印 "true"
print(board.squareIsBlackAtRow(7, column: 7))
// 打印 "false"
```
