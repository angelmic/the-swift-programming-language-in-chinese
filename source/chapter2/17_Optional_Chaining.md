# 可選鏈式調用

-----------------

> 1.0
> 翻譯：[Jasonbroker](https://github.com/Jasonbroker)
> 校對：[numbbbbb](https://github.com/numbbbbb), [stanzhai](https://github.com/stanzhai)

> 2.0
> 翻譯+校對：[lyojo](https://github.com/lyojo)

> 2.1
> 校對：[shanks](http://codebuild.me)，2015-10-31  
>  
> 2.2
> 翻譯+校對：[SketchK](https://github.com/SketchK) 2016-05-15   
> 3.0.1，shanks，2016-11-13

本頁包含內容：

- [使用可選鏈式調用代替強制展開](#optional_chaining_as_an_alternative_to_forced_unwrapping)
- [為可選鏈式調用定義模型類](#defining_model_classes_for_optional_chaining)
- [通過可選鏈式調用訪問屬性](#accessing_properties_through_optional_chaining)
- [通過可選鏈式調用調用方法](#calling_methods_through_optional_chaining)
- [通過可選鏈式調用訪問下標](#accessing_subscripts_through_optional_chaining)
- [連接多層可選鏈式調用](#linking_multiple_levels_of_chaining)
- [在方法的可選返回值上進行可選鏈式調用](#chaining_on_methods_with_optional_return_values)

*可選鏈式調用*是一種可以在當前值可能為`nil`的可選值上請求和調用屬性、方法及下標的方法。如果可選值有值，那麼調用就會成功；如果可選值是`nil`，那麼調用將返回`nil`。多個調用可以連接在一起形成一個調用鏈，如果其中任何一個節點為`nil`，整個調用鏈都會失敗，即返回`nil`。

> 注意  
Swift 的可選鏈式調用和 Objective-C 中向`nil`發送消息有些相像，但是 Swift 的可選鏈式調用可以應用於任意類型，並且能檢查調用是否成功。

<a name="optional_chaining_as_an_alternative_to_forced_unwrapping"></a>
## 使用可選鏈式調用代替強制展開

通過在想調用的屬性、方法、或下標的可選值後面放一個問號（`?`），可以定義一個可選鏈。這一點很像在可選值後面放一個嘆號（`!`）來強制展開它的值。它們的主要區別在於當可選值為空時可選鏈式調用只會調用失敗，然而強制展開將會觸發運行時錯誤。

為了反映可選鏈式調用可以在空值（`nil`）上調用的事實，不論這個調用的屬性、方法及下標返回的值是不是可選值，它的返回結果都是一個可選值。你可以利用這個返回值來判斷你的可選鏈式調用是否調用成功，如果調用有返回值則說明調用成功，返回`nil`則說明調用失敗。

特別地，可選鏈式調用的返回結果與原本的返回結果具有相同的類型，但是被包裝成了一個可選值。例如，使用可選鏈式調用訪問屬性，當可選鏈式調用成功時，如果屬性原本的返回結果是`Int`類型，則會變為`Int?`類型。

下面幾段代碼將解釋可選鏈式調用和強制展開的不同。

首先定義兩個類`Person`和`Residence`：

```swift
class Person {
	var residence: Residence?
}

class Residence {
	var numberOfRooms = 1
}
```

`Residence`有一個`Int`類型的屬性`numberOfRooms`，其默認值為`1`。`Person`具有一個可選的`residence`屬性，其類型為`Residence?`。  

假如你創建了一個新的`Person`實例,它的`residence`屬性由於是是可選型而將初始化為`nil`,在下面的代碼中,`john`有一個值為`nil`的`residence`屬性：

```swift
let john = Person()
```

如果使用嘆號（`!`）強制展開獲得這個`john`的`residence`屬性中的`numberOfRooms`值，會觸發運行時錯誤，因為這時`residence`沒有可以展開的值：

```swift
let roomCount = john.residence!.numberOfRooms
// 這會引發運行時錯誤
```

`john.residence`為非`nil`值的時候，上面的調用會成功，並且把`roomCount`設置為`Int`類型的房間數量。正如上面提到的，當`residence`為`nil`的時候上面這段代碼會觸發運行時錯誤。

可選鏈式調用提供了另一種訪問`numberOfRooms`的方式，使用問號（`?`）來替代原來的嘆號（`!`）：

```swift
if let roomCount = john.residence?.numberOfRooms {
    print("John's residence has \(roomCount) room(s).")
} else {
    print("Unable to retrieve the number of rooms.")
}
// 打印 「Unable to retrieve the number of rooms.」
```

在`residence`後面添加問號之後，Swift 就會在`residence`不為`nil`的情況下訪問`numberOfRooms`。

因為訪問`numberOfRooms`有可能失敗，可選鏈式調用會返回`Int?`類型，或稱為「可選的 `Int`」。如上例所示，當`residence`為`nil`的時候，可選的`Int`將會為`nil`，表明無法訪問`numberOfRooms`。訪問成功時，可選的`Int`值會通過可選綁定展開，並賦值給非可選類型的`roomCount`常量。

要注意的是，即使`numberOfRooms`是非可選的`Int`時，這一點也成立。只要使用可選鏈式調用就意味著`numberOfRooms`會返回一個`Int?`而不是`Int`。

可以將一個`Residence`的實例賦給`john.residence`，這樣它就不再是`nil`了：

```swift
john.residence = Residence()
```

`john.residence`現在包含一個實際的`Residence`實例，而不再是`nil`。如果你試圖使用先前的可選鏈式調用訪問`numberOfRooms`，它現在將返回值為`1`的`Int?`類型的值：

```swift
if let roomCount = john.residence?.numberOfRooms {
	print("John's residence has \(roomCount) room(s).")
} else {
	print("Unable to retrieve the number of rooms.")
}
// 打印 「John's residence has 1 room(s).」
```

<a name="defining_model_classes_for_optional_chaining"></a>
## 為可選鏈式調用定義模型類

通過使用可選鏈式調用可以調用多層屬性、方法和下標。這樣可以在復雜的模型中向下訪問各種子屬性，並且判斷能否訪問子屬性的屬性、方法或下標。

下面這段代碼定義了四個模型類，這些例子包括多層可選鏈式調用。為了方便說明，在`Person`和`Residence`的基礎上增加了`Room`類和`Address`類，以及相關的屬性、方法以及下標。

`Person`類的定義基本保持不變：

```swift
class Person {
    var residence: Residence?
}
```

`Residence`類比之前復雜些，增加了一個名為`rooms`的變量屬性，該屬性被初始化為`[Room]`類型的空數組：

```swift
class Residence {
    var rooms = [Room]()
    var numberOfRooms: Int {
        return rooms.count
    }
    subscript(i: Int) -> Room {
        get {
            return rooms[i]
        }
        set {
            rooms[i] = newValue
        }
    }
    func printNumberOfRooms() {
        print("The number of rooms is \(numberOfRooms)")
    }
    var address: Address?
}
```

現在`Residence`有了一個存儲`Room`實例的數組，`numberOfRooms`屬性被實現為計算型屬性，而不是存儲型屬性。`numberOfRooms`屬性簡單地返回`rooms`數組的`count`屬性的值。

`Residence`還提供了訪問`rooms`數組的快捷方式，即提供可讀寫的下標來訪問`rooms`數組中指定位置的元素。

此外，`Residence`還提供了`printNumberOfRooms`方法，這個方法的作用是打印`numberOfRooms`的值。

最後，`Residence`還定義了一個可選屬性`address`，其類型為`Address?`。`Address`類的定義在下面會說明。

`Room`類是一個簡單類，其實例被存儲在`rooms`數組中。該類只包含一個屬性`name`，以及一個用於將該屬性設置為適當的房間名的初始化函數：

```swift
class Room {
    let name: String
    init(name: String) { self.name = name }
}
```

最後一個類是`Address`，這個類有三個`String?`類型的可選屬性。`buildingName`以及`buildingNumber`屬性分別表示某個大廈的名稱和號碼，第三個屬性`street`表示大廈所在街道的名稱：

```swift
class Address {
    var buildingName: String?
    var buildingNumber: String?
    var street: String?
    func buildingIdentifier() -> String? {
        if buildingName != nil {
            return buildingName
        } else if buildingNumber != nil && street != nil {
            return "\(buildingNumber) \(street)"
        } else {
            return nil
        }
    }
}
```

`Address`類提供了`buildingIdentifier()`方法，返回值為`String?`。 如果`buildingName`有值則返回`buildingName`。或者，如果`buildingNumber`和`street`均有值則返回`buildingNumber`。否則，返回`nil`。

<a name="accessing_properties_through_optional_chaining"></a>
## 通過可選鏈式調用訪問屬性

正如[使用可選鏈式調用代替強制展開](#optional_chaining_as_an_alternative_to_forced_unwrapping)中所述，可以通過可選鏈式調用在一個可選值上訪問它的屬性，並判斷訪問是否成功。

下面的代碼創建了一個`Person`實例，然後像之前一樣，嘗試訪問`numberOfRooms`屬性：

```swift
let john = Person()
if let roomCount = john.residence?.numberOfRooms {
    print("John's residence has \(roomCount) room(s).")
} else {
    print("Unable to retrieve the number of rooms.")
}
// 打印 「Unable to retrieve the number of rooms.」
```

因為`john.residence`為`nil`，所以這個可選鏈式調用依舊會像先前一樣失敗。

還可以通過可選鏈式調用來設置屬性值：

```swift
let someAddress = Address()
someAddress.buildingNumber = "29"
someAddress.street = "Acacia Road"
john.residence?.address = someAddress
```

在這個例子中，通過`john.residence`來設定`address`屬性也會失敗，因為`john.residence`當前為`nil`。

上面代碼中的賦值過程是可選鏈式調用的一部分，這意味著可選鏈式調用失敗時，等號右側的代碼不會被執行。對於上面的代碼來說，很難驗證這一點，因為像這樣賦值一個常量沒有任何副作用。下面的代碼完成了同樣的事情，但是它使用一個函數來創建`Address`實例，然後將該實例返回用於賦值。該函數會在返回前打印「Function was called」，這使你能驗證等號右側的代碼是否被執行。

```swift
func createAddress() -> Address {
    print("Function was called.")

    let someAddress = Address()
    someAddress.buildingNumber = "29"
    someAddress.street = "Acacia Road"

    return someAddress
}
john.residence?.address = createAddress()
```

沒有任何打印消息，可以看出`createAddress()`函數並未被執行。

<a name="calling_methods_through_optional_chaining"></a>
## 通過可選鏈式調用調用方法

可以通過可選鏈式調用來調用方法，並判斷是否調用成功，即使這個方法沒有返回值。

`Residence`類中的`printNumberOfRooms()`方法打印當前的`numberOfRooms`值，如下所示：

```swift
func printNumberOfRooms() {
    print("The number of rooms is \(numberOfRooms)")
}
```

這個方法沒有返回值。然而，沒有返回值的方法具有隱式的返回類型`Void`，如[無返回值函數](./06_Functions.html#functions_without_return_values)中所述。這意味著沒有返回值的方法也會返回`()`，或者說空的元組。

如果在可選值上通過可選鏈式調用來調用這個方法，該方法的返回類型會是`Void?`，而不是`Void`，因為通過可選鏈式調用得到的返回值都是可選的。這樣我們就可以使用`if`語句來判斷能否成功調用`printNumberOfRooms()`方法，即使方法本身沒有定義返回值。通過判斷返回值是否為`nil`可以判斷調用是否成功：

```swift
if john.residence?.printNumberOfRooms() != nil {
    print("It was possible to print the number of rooms.")
} else {
    print("It was not possible to print the number of rooms.")
}
// 打印 「It was not possible to print the number of rooms.」
```

同樣的，可以據此判斷通過可選鏈式調用為屬性賦值是否成功。在上面的[通過可選鏈式調用訪問屬性](#accessing_properties_through_optional_chaining)的例子中，我們嘗試給`john.residence`中的`address`屬性賦值，即使`residence`為`nil`。通過可選鏈式調用給屬性賦值會返回`Void?`，通過判斷返回值是否為`nil`就可以知道賦值是否成功：

```swift
if (john.residence?.address = someAddress) != nil {
	print("It was possible to set the address.")
} else {
	print("It was not possible to set the address.")
}
// 打印 「It was not possible to set the address.」
```

<a name="accessing_subscripts_through_optional_chaining"></a>
## 通過可選鏈式調用訪問下標

通過可選鏈式調用，我們可以在一個可選值上訪問下標，並且判斷下標調用是否成功。

> 注意  
通過可選鏈式調用訪問可選值的下標時，應該將問號放在下標方括號的前面而不是後面。可選鏈式調用的問號一般直接跟在可選表達式的後面。

下面這個例子用下標訪問`john.residence`屬性存儲的`Residence`實例的`rooms`數組中的第一個房間的名稱，因為`john.residence`為`nil`，所以下標調用失敗了：

```swift
if let firstRoomName = john.residence?[0].name {
    print("The first room name is \(firstRoomName).")
} else {
    print("Unable to retrieve the first room name.")
}
// 打印 「Unable to retrieve the first room name.」
```

在這個例子中，問號直接放在`john.residence`的後面，並且在方括號的前面，因為`john.residence`是可選值。

類似的，可以通過下標，用可選鏈式調用來賦值：

```swift
john.residence?[0] = Room(name: "Bathroom")
```

這次賦值同樣會失敗，因為`residence`目前是`nil`。

如果你創建一個`Residence`實例，並為其`rooms`數組添加一些`Room`實例，然後將`Residence`實例賦值給`john.residence`，那就可以通過可選鏈和下標來訪問數組中的元素：

```swift
let johnsHouse = Residence()
johnsHouse.rooms.append(Room(name: "Living Room"))
johnsHouse.rooms.append(Room(name: "Kitchen"))
john.residence = johnsHouse

if let firstRoomName = john.residence?[0].name {
	print("The first room name is \(firstRoomName).")
} else {
	print("Unable to retrieve the first room name.")
}
// 打印 「The first room name is Living Room.」
```

<a name="accessing_subscripts_of_optional_type"></a>
### 訪問可選類型的下標

如果下標返回可選類型值，比如 Swift 中`Dictionary`類型的鍵的下標，可以在下標的結尾括號後面放一個問號來在其可選返回值上進行可選鏈式調用：

```swift
var testScores = ["Dave": [86, 82, 84], "Bev": [79, 94, 81]]
testScores["Dave"]?[0] = 91
testScores["Bev"]?[0] += 1
testScores["Brian"]?[0] = 72
// "Dave" 數組現在是 [91, 82, 84]，"Bev" 數組現在是 [80, 94, 81]
```

上面的例子中定義了一個`testScores`數組，包含了兩個鍵值對，把`String`類型的鍵映射到一個`Int`值的數組。這個例子用可選鏈式調用把`"Dave"`數組中第一個元素設為`91`，把`"Bev"`數組的第一個元素`+1`，然後嘗試把`"Brian"`數組中的第一個元素設為`72`。前兩個調用成功，因為`testScores`字典中包含`"Dave"`和`"Bev"`這兩個鍵。但是`testScores`字典中沒有`"Brian"`這個鍵，所以第三個調用失敗。

<a name="linking_multiple_levels_of_chaining"></a>
## 連接多層可選鏈式調用

可以通過連接多個可選鏈式調用在更深的模型層級中訪問屬性、方法以及下標。然而，多層可選鏈式調用不會增加返回值的可選層級。

也就是說：

+ 如果你訪問的值不是可選的，可選鏈式調用將會返回可選值。
+ 如果你訪問的值就是可選的，可選鏈式調用不會讓可選返回值變得「更可選」。

因此：

+ 通過可選鏈式調用訪問一個`Int`值，將會返回`Int?`，無論使用了多少層可選鏈式調用。
+ 類似的，通過可選鏈式調用訪問`Int?`值，依舊會返回`Int?`值，並不會返回`Int??`。

下面的例子嘗試訪問`john`中的`residence`屬性中的`address`屬性中的`street`屬性。這裡使用了兩層可選鏈式調用，`residence`以及`address`都是可選值：

```swift
if let johnsStreet = john.residence?.address?.street {
    print("John's street name is \(johnsStreet).")
} else {
    print("Unable to retrieve the address.")
}
// 打印 「Unable to retrieve the address.」
```

`john.residence`現在包含一個有效的`Residence`實例。然而，`john.residence.address`的值當前為`nil`。因此，調用`john.residence?.address?.street`會失敗。

需要注意的是，上面的例子中，`street`的屬性為`String?`。`john.residence?.address?.street`的返回值也依然是`String?`，即使已經使用了兩層可選鏈式調用。

如果為`john.residence.address`賦值一個`Address`實例，並且為`address`中的`street`屬性設置一個有效值，我們就能過通過可選鏈式調用來訪問`street`屬性：

```swift
let johnsAddress = Address()
johnsAddress.buildingName = "The Larches"
johnsAddress.street = "Laurel Street"
john.residence?.address = johnsAddress
	
if let johnsStreet = john.residence?.address?.street {
    print("John's street name is \(johnsStreet).")
} else {
    print("Unable to retrieve the address.")
}
// 打印 「John's street name is Laurel Street.」
```

在上面的例子中，因為`john.residence`包含一個有效的`Residence`實例，所以對`john.residence`的`address`屬性賦值將會成功。

<a name="chaining_on_methods_with_optional_return_values"></a>
## 在方法的可選返回值上進行可選鏈式調用

上面的例子展示了如何在一個可選值上通過可選鏈式調用來獲取它的屬性值。我們還可以在一個可選值上通過可選鏈式調用來調用方法，並且可以根據需要繼續在方法的可選返回值上進行可選鏈式調用。

在下面的例子中，通過可選鏈式調用來調用`Address`的`buildingIdentifier()`方法。這個方法返回`String?`類型的值。如上所述，通過可選鏈式調用來調用該方法，最終的返回值依舊會是`String?`類型：
 
```swift
if let buildingIdentifier = john.residence?.address?.buildingIdentifier() {
    print("John's building identifier is \(buildingIdentifier).")
}
// 打印 「John's building identifier is The Larches.」
```

如果要在該方法的返回值上進行可選鏈式調用，在方法的圓括號後面加上問號即可：

```swift
if let beginsWithThe =
	john.residence?.address?.buildingIdentifier()?.hasPrefix("The") {
		if beginsWithThe {
			print("John's building identifier begins with \"The\".")
		} else {
			print("John's building identifier does not begin with \"The\".")
		}
}
// 打印 「John's building identifier begins with "The".」
```

> 注意  
在上面的例子中，在方法的圓括號後面加上問號是因為你要在`buildingIdentifier()`方法的可選返回值上進行可選鏈式調用，而不是方法本身。
