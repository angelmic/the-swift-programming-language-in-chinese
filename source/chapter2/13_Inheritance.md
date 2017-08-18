# 繼承
-------------------

> 1.0
> 翻譯：[Hawstein](https://github.com/Hawstein)
> 校對：[menlongsheng](https://github.com/menlongsheng)

> 2.0，2.1
> 翻譯+校對：[shanks](http://codebuild.me)
> 
> 2.2
> 校對：[SketchK](https://github.com/SketchK) 2016-05-13  
> 3.0.1，shanks，2016-11-13

本頁包含內容：

- [定義一個基類](#defining_a_base_class)
- [子類生成](#subclassing)
- [重寫](#overriding)
- [防止重寫](#preventing_overrides)

一個類可以*繼承*另一個類的方法，屬性和其它特性。當一個類繼承其它類時，繼承類叫*子類*，被繼承類叫*超類（或父類）*。在 Swift 中，繼承是區分「類」與其它類型的一個基本特征。

在 Swift 中，類可以調用和訪問超類的方法，屬性和下標，並且可以重寫這些方法，屬性和下標來優化或修改它們的行為。Swift 會檢查你的重寫定義在超類中是否有匹配的定義，以此確保你的重寫行為是正確的。

可以為類中繼承來的屬性添加屬性觀察器，這樣一來，當屬性值改變時，類就會被通知到。可以為任何屬性添加屬性觀察器，無論它原本被定義為存儲型屬性還是計算型屬性。

<a name="defining_a_base_class"></a>
## 定義一個基類

不繼承於其它類的類，稱之為*基類*。

> 注意  
Swift 中的類並不是從一個通用的基類繼承而來。如果你不為你定義的類指定一個超類的話，這個類就自動成為基類。

下面的例子定義了一個叫`Vehicle`的基類。這個基類聲明了一個名為`currentSpeed `，默認值是`0.0`的存儲屬性（屬性類型推斷為`Double`）。`currentSpeed`屬性的值被一個`String`類型的只讀計算型屬性`description`使用，用來創建車輛的描述。

`Vehicle`基類也定義了一個名為`makeNoise`的方法。這個方法實際上不為`Vehicle`實例做任何事，但之後將會被`Vehicle`的子類定制：

```swift
class Vehicle {
    var currentSpeed = 0.0
    var description: String {
        return "traveling at \(currentSpeed) miles per hour"
    }
    func makeNoise() {
        // 什麼也不做-因為車輛不一定會有噪音
    }
}
```

您可以用初始化語法創建一個`Vehicle`的新實例，即類名後面跟一個空括號：

```swift
let someVehicle = Vehicle()
```

現在已經創建了一個`Vehicle`的新實例，你可以訪問它的`description`屬性來打印車輛的當前速度：

```swift
print("Vehicle: \(someVehicle.description)")
// 打印 "Vehicle: traveling at 0.0 miles per hour"
```

`Vehicle`類定義了一個通用特性的車輛類，實際上沒什麼用處。為了讓它變得更加有用，需要完善它從而能夠描述一個更加具體類型的車輛。

<a name="subclassing"></a>
## 子類生成

*子類生成*指的是在一個已有類的基礎上創建一個新的類。子類繼承超類的特性，並且可以進一步完善。你還可以為子類添加新的特性。

為了指明某個類的超類，將超類名寫在子類名的後面，用冒號分隔：

```swift
class SomeClass: SomeSuperclass {
    // 這裡是子類的定義
}
```

下一個例子，定義一個叫`Bicycle`的子類，繼承成父類`Vehicle`：

```swift
class Bicycle: Vehicle {
    var hasBasket = false
}
```

新的`Bicycle`類自動獲得`Vehicle`類的所有特性，比如`currentSpeed`和`description`屬性，還有它的`makeNoise()`方法。

除了它所繼承的特性，`Bicycle`類還定義了一個默認值為`false`的存儲型屬性`hasBasket`（屬性推斷為`Bool`）。

默認情況下，你創建任何新的`Bicycle`實例將不會有一個籃子（即`hasBasket`屬性默認為`false`），創建該實例之後，你可以為特定的`Bicycle`實例設置`hasBasket`屬性為`ture`：

```swift
let bicycle = Bicycle()
bicycle.hasBasket = true
```

你還可以修改`Bicycle`實例所繼承的`currentSpeed`屬性，和查詢實例所繼承的`description`屬性：

```swift
bicycle.currentSpeed = 15.0
print("Bicycle: \(bicycle.description)")
// 打印 "Bicycle: traveling at 15.0 miles per hour"
```

子類還可以繼續被其它類繼承，下面的示例為`Bicycle`創建了一個名為`Tandem`（雙人自行車）的子類：

```swift
class Tandem: Bicycle {
    var currentNumberOfPassengers = 0
}
```

`Tandem`從`Bicycle`繼承了所有的屬性與方法，這又使它同時繼承了`Vehicle`的所有屬性與方法。`Tandem`也增加了一個新的叫做`currentNumberOfPassengers`的存儲型屬性，默認值為`0`。

如果你創建了一個`Tandem`的實例，你可以使用它所有的新屬性和繼承的屬性，還能查詢從`Vehicle`繼承來的只讀屬性`description`：

```swift
let tandem = Tandem()
tandem.hasBasket = true
tandem.currentNumberOfPassengers = 2
tandem.currentSpeed = 22.0
print("Tandem: \(tandem.description)")
// 打印："Tandem: traveling at 22.0 miles per hour"
```

<a name="overriding"></a>
## 重寫

子類可以為繼承來的實例方法，類方法，實例屬性，或下標提供自己定制的實現。我們把這種行為叫*重寫*。

如果要重寫某個特性，你需要在重寫定義的前面加上`override`關鍵字。這麼做，你就表明了你是想提供一個重寫版本，而非錯誤地提供了一個相同的定義。意外的重寫行為可能會導致不可預知的錯誤，任何缺少`override`關鍵字的重寫都會在編譯時被診斷為錯誤。

`override`關鍵字會提醒 Swift 編譯器去檢查該類的超類（或其中一個父類）是否有匹配重寫版本的聲明。這個檢查可以確保你的重寫定義是正確的。

### 訪問超類的方法，屬性及下標

當你在子類中重寫超類的方法，屬性或下標時，有時在你的重寫版本中使用已經存在的超類實現會大有裨益。比如，你可以完善已有實現的行為，或在一個繼承來的變量中存儲一個修改過的值。

在合適的地方，你可以通過使用`super`前綴來訪問超類版本的方法，屬性或下標：

* 在方法`someMethod()`的重寫實現中，可以通過`super.someMethod()`來調用超類版本的`someMethod()`方法。
* 在屬性`someProperty`的 getter 或 setter 的重寫實現中，可以通過`super.someProperty`來訪問超類版本的`someProperty`屬性。
* 在下標的重寫實現中，可以通過`super[someIndex]`來訪問超類版本中的相同下標。

### 重寫方法

在子類中，你可以重寫繼承來的實例方法或類方法，提供一個定制或替代的方法實現。

下面的例子定義了`Vehicle`的一個新的子類，叫`Train`，它重寫了從`Vehicle`類繼承來的`makeNoise()`方法：

```swift
class Train: Vehicle {
    override func makeNoise() {
        print("Choo Choo")
    }
}
```

如果你創建一個`Train`的新實例，並調用了它的`makeNoise()`方法，你就會發現`Train`版本的方法被調用：

```swift
let train = Train()
train.makeNoise()
// 打印 "Choo Choo"
```

### 重寫屬性

你可以重寫繼承來的實例屬性或類型屬性，提供自己定制的 getter 和 setter，或添加屬性觀察器使重寫的屬性可以觀察屬性值什麼時候發生改變。

#### 重寫屬性的 Getters 和 Setters

你可以提供定制的 getter（或 setter）來重寫任意繼承來的屬性，無論繼承來的屬性是存儲型的還是計算型的屬性。子類並不知道繼承來的屬性是存儲型的還是計算型的，它只知道繼承來的屬性會有一個名字和類型。你在重寫一個屬性時，必需將它的名字和類型都寫出來。這樣才能使編譯器去檢查你重寫的屬性是與超類中同名同類型的屬性相匹配的。

你可以將一個繼承來的只讀屬性重寫為一個讀寫屬性，只需要在重寫版本的屬性裡提供 getter 和 setter 即可。但是，你不可以將一個繼承來的讀寫屬性重寫為一個只讀屬性。

> 注意  
如果你在重寫屬性中提供了 setter，那麼你也一定要提供 getter。如果你不想在重寫版本中的 getter 裡修改繼承來的屬性值，你可以直接通過`super.someProperty`來返回繼承來的值，其中`someProperty`是你要重寫的屬性的名字。

以下的例子定義了一個新類，叫`Car`，它是`Vehicle`的子類。這個類引入了一個新的存儲型屬性叫做`gear`，默認值為整數`1`。`Car`類重寫了繼承自`Vehicle`的`description`屬性，提供包含當前檔位的自定義描述：

```swift
class Car: Vehicle {
    var gear = 1
    override var description: String {
        return super.description + " in gear \(gear)"
    }
}
```

重寫的`description`屬性首先要調用`super.description`返回`Vehicle`類的`description`屬性。之後，`Car`類版本的`description`在末尾增加了一些額外的文本來提供關於當前檔位的信息。

如果你創建了`Car`的實例並且設置了它的`gear`和`currentSpeed`屬性，你可以看到它的`description`返回了`Car`中的自定義描述：

```swift
let car = Car()
car.currentSpeed = 25.0
car.gear = 3
print("Car: \(car.description)")
// 打印 "Car: traveling at 25.0 miles per hour in gear 3"
```

<a name="overriding_property_observers"></a>
#### 重寫屬性觀察器

你可以通過重寫屬性為一個繼承來的屬性添加屬性觀察器。這樣一來，當繼承來的屬性值發生改變時，你就會被通知到，無論那個屬性原本是如何實現的。關於屬性觀察器的更多內容，請看[屬性觀察器](../chapter2/10_Properties.html#property_observers)。

> 注意  
你不可以為繼承來的常量存儲型屬性或繼承來的只讀計算型屬性添加屬性觀察器。這些屬性的值是不可以被設置的，所以，為它們提供`willSet`或`didSet`實現是不恰當。  
此外還要注意，你不可以同時提供重寫的 setter 和重寫的屬性觀察器。如果你想觀察屬性值的變化，並且你已經為那個屬性提供了定制的 setter，那麼你在 setter 中就可以觀察到任何值變化了。

下面的例子定義了一個新類叫`AutomaticCar`，它是`Car`的子類。`AutomaticCar`表示自動擋汽車，它可以根據當前的速度自動選擇合適的擋位:

```swift
class AutomaticCar: Car {
    override var currentSpeed: Double {
        didSet {
            gear = Int(currentSpeed / 10.0) + 1
        }
    }
}
```

當你設置`AutomaticCar`的`currentSpeed`屬性，屬性的`didSet`觀察器就會自動地設置`gear`屬性，為新的速度選擇一個合適的擋位。具體來說就是，屬性觀察器將新的速度值除以`10`，然後向下取得最接近的整數值，最後加`1`來得到檔位`gear`的值。例如，速度為`35.0`時，擋位為`4`：

```swift
let automatic = AutomaticCar()
automatic.currentSpeed = 35.0
print("AutomaticCar: \(automatic.description)")
// 打印 "AutomaticCar: traveling at 35.0 miles per hour in gear 4"
```

<a name="preventing_overrides"></a>
## 防止重寫

你可以通過把方法，屬性或下標標記為*`final`*來防止它們被重寫，只需要在聲明關鍵字前加上`final`修飾符即可（例如：`final var`，`final func`，`final class func`，以及`final subscript`）。

如果你重寫了帶有`final`標記的方法，屬性或下標，在編譯時會報錯。在類擴展中的方法，屬性或下標也可以在擴展的定義裡標記為 final 的。

你可以通過在關鍵字`class`前添加`final`修飾符（`final class`）來將整個類標記為 final 的。這樣的類是不可被繼承的，試圖繼承這樣的類會導致編譯報錯。
