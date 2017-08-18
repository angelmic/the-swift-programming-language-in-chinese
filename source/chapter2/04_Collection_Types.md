# 集合類型 (Collection Types)
-----------------

> 1.0
> 翻譯：[zqp](https://github.com/zqp)
> 校對：[shinyzhu](https://github.com/shinyzhu), [stanzhai](https://github.com/stanzhai), [feiin](https://github.com/feiin)

> 2.0
> 翻譯+校對：[JackAlan](https://github.com/AlanMelody)

> 2.1
> 校對：[shanks](http://codebuild.me)  

> 2.2
> 校對：[SketchK](https://github.com/SketchK) 2016-05-11
>
> 3.0
> 校對：[shanks](http://codebuild.me) ，2016-10-09   
> 3.0.1，shanks，2016-11-12


本頁包含內容：

- [集合的可變性](#mutability_of_collections)
- [數組](#arrays)
- [集合](#sets)
- [集合操作](#performing_set_operations)
- [字典](#dictionaries)

Swift 語言提供`Arrays`、`Sets`和`Dictionaries`三種基本的*集合類型*用來存儲集合數據。數組（Arrays）是有序數據的集。集合（Sets）是無序無重復數據的集。字典（Dictionaries）是無序的鍵值對的集。

![](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/CollectionTypes_intro_2x.png)

Swift 語言中的`Arrays`、`Sets`和`Dictionaries`中存儲的數據值類型必須明確。這意味著我們不能把不正確的數據類型插入其中。同時這也說明我們完全可以對取回值的類型非常自信。

> 注意：  
Swift 的`Arrays`、`Sets`和`Dictionaries`類型被實現為*泛型集合*。更多關於泛型類型和集合，參見 [泛型](./23_Generics.html)章節。

<a name="mutability_of_collections"></a>
## 集合的可變性

如果創建一個`Arrays`、`Sets`或`Dictionaries`並且把它分配成一個變量，這個集合將會是*可變的*。這意味著我們可以在創建之後添加更多或移除已存在的數據項，或者改變集合中的數據項。如果我們把`Arrays`、`Sets`或`Dictionaries`分配成常量，那麼它就是*不可變的*，它的大小和內容都不能被改變。

> 注意：  
在我們不需要改變集合的時候創建不可變集合是很好的實踐。如此 Swift 編譯器可以優化我們創建的集合。

<a name="arrays"></a>
## 數組(Arrays)

*數組*使用有序列表存儲同一類型的多個值。相同的值可以多次出現在一個數組的不同位置中。

> 注意:
 Swift 的`Array`類型被橋接到`Foundation`中的`NSArray`類。
 更多關於在`Foundation`和`Cocoa`中使用`Array`的信息，參見 [*Using Swift with Cocoa and Obejective-C(Swift 3.0.1)*](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/BuildingCocoaApps/index.html#//apple_ref/doc/uid/TP40014216) 中[使用 Cocoa 數據類型](https://developer.apple.com/library/content/documentation/Swift/Conceptual/BuildingCocoaApps/WorkingWithCocoaDataTypes.html#//apple_ref/doc/uid/TP40014216-CH6)部分。

<a name="array_type_shorthand_syntax"></a>
### 數組的簡單語法

寫 Swift 數組應該遵循像`Array<Element>`這樣的形式，其中`Element`是這個數組中唯一允許存在的數據類型。我們也可以使用像`[Element]`這樣的簡單語法。盡管兩種形式在功能上是一樣的，但是推薦較短的那種，而且在本文中都會使用這種形式來使用數組。

<a name="creating_an_empty_array"></a>
### 創建一個空數組

我們可以使用構造語法來創建一個由特定數據類型構成的空數組：

```swift
var someInts = [Int]()
print("someInts is of type [Int] with \(someInts.count) items.")
// 打印 "someInts is of type [Int] with 0 items."
```

注意，通過構造函數的類型，`someInts`的值類型被推斷為`[Int]`。

或者，如果代碼上下文中已經提供了類型信息，例如一個函數參數或者一個已經定義好類型的常量或者變量，我們可以使用空數組語句創建一個空數組，它的寫法很簡單：`[]`（一對空方括號）：

```swift
someInts.append(3)
// someInts 現在包含一個 Int 值
someInts = []
// someInts 現在是空數組，但是仍然是 [Int] 類型的。
```

<a name="creating_an_array_with_a_default_value"></a>
### 創建一個帶有默認值的數組

Swift 中的`Array`類型還提供一個可以創建特定大小並且所有數據都被默認的構造方法。我們可以把准備加入新數組的數據項數量（`count`）和適當類型的初始值（`repeating`）傳入數組構造函數：

```swift
var threeDoubles = Array(repeating: 0.0, count: 3)
// threeDoubles 是一種 [Double] 數組，等價於 [0.0, 0.0, 0.0]
```

<a name="creating_an_array_by_adding_two_arrays_together"></a>
### 通過兩個數組相加創建一個數組

我們可以使用加法操作符（`+`）來組合兩種已存在的相同類型數組。新數組的數據類型會被從兩個數組的數據類型中推斷出來：

```swift
var anotherThreeDoubles = Array(repeating: 2.5, count: 3)
// anotherThreeDoubles 被推斷為 [Double]，等價於 [2.5, 2.5, 2.5]

var sixDoubles = threeDoubles + anotherThreeDoubles
// sixDoubles 被推斷為 [Double]，等價於 [0.0, 0.0, 0.0, 2.5, 2.5, 2.5]
```

<a name="creating_an_array_with_an_array_literals"></a>
### 用數組字面量構造數組

我們可以使用*數組字面量*來進行數組構造，這是一種用一個或者多個數值構造數組的簡單方法。數組字面量是一系列由逗號分割並由方括號包含的數值：

`[value 1, value 2, value 3]`。

下面這個例子創建了一個叫做`shoppingList`並且存儲`String`的數組：

```swift
var shoppingList: [String] = ["Eggs", "Milk"]
// shoppingList 已經被構造並且擁有兩個初始項。
```

`shoppingList`變量被聲明為「字符串值類型的數組「，記作`[String]`。 因為這個數組被規定只有`String`一種數據結構，所以只有`String`類型可以在其中被存取。 在這裡，`shoppingList`數組由兩個`String`值（`"Eggs"` 和`"Milk"`）構造，並且由數組字面量定義。

> 注意：  
`shoppingList`數組被聲明為變量（`var`關鍵字創建）而不是常量（`let`創建）是因為以後可能會有更多的數據項被插入其中。

在這個例子中，字面量僅僅包含兩個`String`值。匹配了該數組的變量聲明（只能包含`String`的數組），所以這個字面量的分配過程可以作為用兩個初始項來構造`shoppingList`的一種方式。

由於 Swift 的類型推斷機制，當我們用字面量構造只擁有相同類型值數組的時候，我們不必把數組的類型定義清楚。 `shoppingList`的構造也可以這樣寫：

```swift
var shoppingList = ["Eggs", "Milk"]
```

因為所有數組字面量中的值都是相同的類型，Swift 可以推斷出`[String]`是`shoppingList`中變量的正確類型。

<a name="accessing_and_modifying_an_array"></a>
### 訪問和修改數組

我們可以通過數組的方法和屬性來訪問和修改數組，或者使用下標語法。

可以使用數組的只讀屬性`count`來獲取數組中的數據項數量：

```swift
print("The shopping list contains \(shoppingList.count) items.")
// 輸出 "The shopping list contains 2 items."（這個數組有2個項）
```

使用布爾屬性`isEmpty`作為一個縮寫形式去檢查`count`屬性是否為`0`：

```swift
if shoppingList.isEmpty {
    print("The shopping list is empty.")
} else {
    print("The shopping list is not empty.")
}
// 打印 "The shopping list is not empty."（shoppinglist 不是空的）
```

也可以使用`append(_:)`方法在數組後面添加新的數據項：

```swift
shoppingList.append("Flour")
// shoppingList 現在有3個數據項，有人在攤煎餅
```

除此之外，使用加法賦值運算符（`+=`）也可以直接在數組後面添加一個或多個擁有相同類型的數據項：

```swift
shoppingList += ["Baking Powder"]
// shoppingList 現在有四項了
shoppingList += ["Chocolate Spread", "Cheese", "Butter"]
// shoppingList 現在有七項了
```

可以直接使用下標語法來獲取數組中的數據項，把我們需要的數據項的索引值放在直接放在數組名稱的方括號中：

```swift
var firstItem = shoppingList[0]
// 第一項是 "Eggs"
```

> 注意：  
第一項在數組中的索引值是`0`而不是`1`。 Swift 中的數組索引總是從零開始。

我們也可以用下標來改變某個已有索引值對應的數據值：

```swift
shoppingList[0] = "Six eggs"
// 其中的第一項現在是 "Six eggs" 而不是 "Eggs"
```

還可以利用下標來一次改變一系列數據值，即使新數據和原有數據的數量是不一樣的。下面的例子把`"Chocolate Spread"`，`"Cheese"`，和`"Butter"`替換為`"Bananas"`和 `"Apples"`：

```swift
shoppingList[4...6] = ["Bananas", "Apples"]
// shoppingList 現在有6項
```

> 注意：  
不可以用下標訪問的形式去在數組尾部添加新項。


調用數組的`insert(_:at:)`方法來在某個具體索引值之前添加數據項：

```swift
shoppingList.insert("Maple Syrup", at: 0)
// shoppingList 現在有7項
// "Maple Syrup" 現在是這個列表中的第一項
```

這次`insert(_:at:)`方法調用把值為`"Maple Syrup"`的新數據項插入列表的最開始位置，並且使用`0`作為索引值。

類似的我們可以使用`remove(at:)`方法來移除數組中的某一項。這個方法把數組在特定索引值中存儲的數據項移除並且返回這個被移除的數據項（我們不需要的時候就可以無視它）：

```swift
let mapleSyrup = shoppingList.remove(at: 0)
// 索引值為0的數據項被移除
// shoppingList 現在只有6項，而且不包括 Maple Syrup
// mapleSyrup 常量的值等於被移除數據項的值 "Maple Syrup"
```
> 注意：  
如果我們試著對索引越界的數據進行檢索或者設置新值的操作，會引發一個運行期錯誤。我們可以使用索引值和數組的`count`屬性進行比較來在使用某個索引之前先檢驗是否有效。除了當`count`等於 0 時（說明這是個空數組），最大索引值一直是`count - 1`，因為數組都是零起索引。

數據項被移除後數組中的空出項會被自動填補，所以現在索引值為`0`的數據項的值再次等於`"Six eggs"`：

```swift
firstItem = shoppingList[0]
// firstItem 現在等於 "Six eggs"
```

如果我們只想把數組中的最後一項移除，可以使用`removeLast()`方法而不是`remove(at:)`方法來避免我們需要獲取數組的`count`屬性。就像後者一樣，前者也會返回被移除的數據項：

```swift
let apples = shoppingList.removeLast()
// 數組的最後一項被移除了
// shoppingList 現在只有5項，不包括 Apples
// apples 常量的值現在等於 "Apples" 字符串
```

<a name="iterating_over_an_array"></a>
### 數組的遍歷

我們可以使用`for-in`循環來遍歷所有數組中的數據項：

```swift
for item in shoppingList {
    print(item)
}
// Six eggs
// Milk
// Flour
// Baking Powder
// Bananas
```

如果我們同時需要每個數據項的值和索引值，可以使用`enumerated()`方法來進行數組遍歷。`enumerated()`返回一個由每一個數據項索引值和數據值組成的元組。我們可以把這個元組分解成臨時常量或者變量來進行遍歷：

```swift
for (index, value) in shoppingList. enumerated() {
    print("Item \(String(index + 1)): \(value)")
}
// Item 1: Six eggs
// Item 2: Milk
// Item 3: Flour
// Item 4: Baking Powder
// Item 5: Bananas
```

更多關於`for-in`循環的介紹請參見[for 循環](05_Control_Flow.html#for_loops)。

<a name="sets"></a>
## 集合（Sets）

*集合(Set)*用來存儲相同類型並且沒有確定順序的值。當集合元素順序不重要時或者希望確保每個元素只出現一次時可以使用集合而不是數組。

> 注意：  
> Swift的`Set`類型被橋接到`Foundation`中的`NSSet`類。  
> 關於使用`Foundation`和`Cocoa`中`Set`的知識，參見 [*Using Swift with Cocoa and Obejective-C(Swift 3.0.1)*](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/BuildingCocoaApps/index.html#//apple_ref/doc/uid/TP40014216) 中[使用 Cocoa 數據類型](https://developer.apple.com/library/content/documentation/Swift/Conceptual/BuildingCocoaApps/WorkingWithCocoaDataTypes.html#//apple_ref/doc/uid/TP40014216-CH6)部分。

<a name="hash_values_for_set_types"></a>
#### 集合類型的哈希值

一個類型為了存儲在集合中，該類型必須是*可哈希化*的--也就是說，該類型必須提供一個方法來計算它的*哈希值*。一個哈希值是`Int`類型的，相等的對象哈希值必須相同，比如`a==b`,因此必須`a.hashValue == b.hashValue`。

Swift 的所有基本類型(比如`String`,`Int`,`Double`和`Bool`)默認都是可哈希化的，可以作為集合的值的類型或者字典的鍵的類型。沒有關聯值的枚舉成員值(在[枚舉](./08_Enumerations.html)有講述)默認也是可哈希化的。

> 注意：  
> 你可以使用你自定義的類型作為集合的值的類型或者是字典的鍵的類型，但你需要使你的自定義類型符合 Swift 標准庫中的`Hashable`協議。符合`Hashable`協議的類型需要提供一個類型為`Int`的可讀屬性`hashValue`。由類型的`hashValue`屬性返回的值不需要在同一程序的不同執行周期或者不同程序之間保持相同。  

> 因為`Hashable`協議符合`Equatable`協議，所以遵循該協議的類型也必須提供一個"是否相等"運算符(`==`)的實現。這個`Equatable`協議要求任何符合`==`實現的實例間都是一種相等的關系。也就是說，對於`a,b,c`三個值來說，`==`的實現必須滿足下面三種情況：

> * `a == a`(自反性)
> * `a == b`意味著`b == a`(對稱性)
> * `a == b && b == c`意味著`a == c`(傳遞性)

關於遵循協議的更多信息，請看[協議](./22_Protocols.html)。

<a name="set_type_syntax"></a>
### 集合類型語法

Swift 中的`Set`類型被寫為`Set<Element>`，這裡的`Element`表示`Set`中允許存儲的類型，和數組不同的是，集合沒有等價的簡化形式。

<a name="creating_and_initalizing_an_empty_set"></a>
### 創建和構造一個空的集合

你可以通過構造器語法創建一個特定類型的空集合：

```swift
var letters = Set<Character>()
print("letters is of type Set<Character> with \(letters.count) items.")
// 打印 "letters is of type Set<Character> with 0 items."
```

> 注意：  
> 通過構造器，這裡的`letters`變量的類型被推斷為`Set<Character>`。

此外，如果上下文提供了類型信息，比如作為函數的參數或者已知類型的變量或常量，我們可以通過一個空的數組字面量創建一個空的`Set`：

```swift
letters.insert("a")
// letters 現在含有1個 Character 類型的值
letters = []
// letters 現在是一個空的 Set, 但是它依然是 Set<Character> 類型
```

<a name="creating_a_set_with_an_array_literal"></a>
### 用數組字面量創建集合

你可以使用數組字面量來構造集合，並且可以使用簡化形式寫一個或者多個值作為集合元素。

下面的例子創建一個稱之為`favoriteGenres`的集合來存儲`String`類型的值：

```swift
var favoriteGenres: Set<String> = ["Rock", "Classical", "Hip hop"]
// favoriteGenres 被構造成含有三個初始值的集合
```

這個`favoriteGenres`變量被聲明為「一個`String`值的集合」，寫為`Set<String>`。由於這個特定的集合含有指定`String`類型的值，所以它只允許存儲`String`類型值。這裡的`favoriteGenres`變量有三個`String`類型的初始值(`"Rock"`，`"Classical"`和`"Hip hop"`)，並以數組字面量的方式出現。

> 注意：  
> `favoriteGenres`被聲明為一個變量(擁有`var`標示符)而不是一個常量(擁有`let`標示符),因為它裡面的元素將會在下面的例子中被增加或者移除。

一個`Set`類型不能從數組字面量中被單獨推斷出來，因此`Set`類型必須顯式聲明。然而，由於 Swift 的類型推斷功能，如果你想使用一個數組字面量構造一個`Set`並且該數組字面量中的所有元素類型相同，那麼你無須寫出`Set`的具體類型。`favoriteGenres`的構造形式可以采用簡化的方式代替：

```swift
var favoriteGenres: Set = ["Rock", "Classical", "Hip hop"]
```

由於數組字面量中的所有元素類型相同，Swift 可以推斷出`Set<String>`作為`favoriteGenres`變量的正確類型。

<a name="accesing_and_modifying_a_set"></a>
### 訪問和修改一個集合

你可以通過`Set`的屬性和方法來訪問和修改一個`Set`。

為了找出一個`Set`中元素的數量，可以使用其只讀屬性`count`：

```swift
print("I have \(favoriteGenres.count) favorite music genres.")
// 打印 "I have 3 favorite music genres."
```

使用布爾屬性`isEmpty`作為一個縮寫形式去檢查`count`屬性是否為`0`：

```swift
if favoriteGenres.isEmpty {
    print("As far as music goes, I'm not picky.")
} else {
    print("I have particular music preferences.")
}
// 打印 "I have particular music preferences."
```

你可以通過調用`Set`的`insert(_:)`方法來添加一個新元素：

```swift
favoriteGenres.insert("Jazz")
// favoriteGenres 現在包含4個元素
```

你可以通過調用`Set`的`remove(_:)`方法去刪除一個元素，如果該值是該`Set`的一個元素則刪除該元素並且返回被刪除的元素值，否則如果該`Set`不包含該值，則返回`nil`。另外，`Set`中的所有元素可以通過它的`removeAll()`方法刪除。

```swift
if let removedGenre = favoriteGenres.remove("Rock") {
    print("\(removedGenre)? I'm over it.")
} else {
    print("I never much cared for that.")
}
// 打印 "Rock? I'm over it."
```

使用`contains(_:)`方法去檢查`Set`中是否包含一個特定的值：

```swift
if favoriteGenres.contains("Funk") {
    print("I get up on the good foot.")
} else {
    print("It's too funky in here.")
}
// 打印 "It's too funky in here."
```

<a name="iterating_over_a_set"></a>
### 遍歷一個集合

你可以在一個`for-in`循環中遍歷一個`Set`中的所有值。

```swift
for genre in favoriteGenres {
    print("\(genre)")
}
// Classical
// Jazz
// Hip hop
```

更多關於`for-in`循環的信息，參見[For 循環](./05_Control_Flow.html#for_loops)。

Swift 的`Set`類型沒有確定的順序，為了按照特定順序來遍歷一個`Set`中的值可以使用`sorted()`方法，它將返回一個有序數組，這個數組的元素排列順序由操作符'<'對元素進行比較的結果來確定.

```swift
for genre in favoriteGenres.sorted() {
    print("\(genre)")
}
// prints "Classical"
// prints "Hip hop"
// prints "Jazz
```

<a name="performing_set_operations"></a>
## 集合操作

你可以高效地完成`Set`的一些基本操作，比如把兩個集合組合到一起，判斷兩個集合共有元素，或者判斷兩個集合是否全包含，部分包含或者不相交。

<a name="fundamental_set_operations"></a>
### 基本集合操作

下面的插圖描述了兩個集合-`a`和`b`-以及通過陰影部分的區域顯示集合各種操作的結果。

![](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/setVennDiagram_2x.png)

* 使用`intersection(_:)`方法根據兩個集合中都包含的值創建的一個新的集合。
* 使用`symmetricDifference(_:)`方法根據在一個集合中但不在兩個集合中的值創建一個新的集合。
* 使用`union(_:)`方法根據兩個集合的值創建一個新的集合。
* 使用`subtracting(_:)`方法根據不在該集合中的值創建一個新的集合。

```swift
let oddDigits: Set = [1, 3, 5, 7, 9]
let evenDigits: Set = [0, 2, 4, 6, 8]
let singleDigitPrimeNumbers: Set = [2, 3, 5, 7]

oddDigits.union(evenDigits).sorted()
// [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
oddDigits. intersection(evenDigits).sorted()
// []
oddDigits.subtracting(singleDigitPrimeNumbers).sorted()
// [1, 9]
oddDigits. symmetricDifference(singleDigitPrimeNumbers).sorted()
// [1, 2, 9]
```

<a name="set_membership_and_equality"></a>
### 集合成員關系和相等

下面的插圖描述了三個集合-`a`,`b`和`c`,以及通過重疊區域表述集合間共享的元素。集合`a`是集合`b`的父集合，因為`a`包含了`b`中所有的元素，相反的，集合`b`是集合`a`的子集合，因為屬於`b`的元素也被`a`包含。集合`b`和集合`c`彼此不關聯，因為它們之間沒有共同的元素。

![](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/setEulerDiagram_2x.png)

* 使用「是否相等」運算符(`==`)來判斷兩個集合是否包含全部相同的值。
* 使用`isSubset(of:)`方法來判斷一個集合中的值是否也被包含在另外一個集合中。
* 使用`isSuperset(of:)`方法來判斷一個集合中包含另一個集合中所有的值。
* 使用`isStrictSubset(of:)`或者`isStrictSuperset(of:)`方法來判斷一個集合是否是另外一個集合的子集合或者父集合並且兩個集合並不相等。
* 使用`isDisjoint(with:)`方法來判斷兩個集合是否不含有相同的值(是否沒有交集)。

```swift
let houseAnimals: Set = ["🐶", "🐱"]
let farmAnimals: Set = ["🐮", "🐔", "🐑", "🐶", "🐱"]
let cityAnimals: Set = ["🐦", "🐭"]

houseAnimals.isSubset(of: farmAnimals)
// true
farmAnimals.isSuperset(of: houseAnimals)
// true
farmAnimals.isDisjoint(with: cityAnimals)
// true
```

<a name="dictionaries"></a>
## 字典

*字典*是一種存儲多個相同類型的值的容器。每個值（value）都關聯唯一的鍵（key），鍵作為字典中的這個值數據的標識符。和數組中的數據項不同，字典中的數據項並沒有具體順序。我們在需要通過標識符（鍵）訪問數據的時候使用字典，這種方法很大程度上和我們在現實世界中使用字典查字義的方法一樣。

> 注意：  
> Swift 的`Dictionary`類型被橋接到`Foundation`的`NSDictionary`類。  
> 更多關於在`Foundation`和`Cocoa`中使用`Dictionary`類型的信息，參見 [*Using Swift with Cocoa and Obejective-C(Swift 3.0.1)*](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/BuildingCocoaApps/index.html#//apple_ref/doc/uid/TP40014216) 中[使用 Cocoa 數據類型](https://developer.apple.com/library/content/documentation/Swift/Conceptual/BuildingCocoaApps/WorkingWithCocoaDataTypes.html#//apple_ref/doc/uid/TP40014216-CH6)部分。

<a name="dictionary_type_shorthand_syntax"></a>
### 字典類型簡化語法

Swift 的字典使用`Dictionary<Key, Value>`定義，其中`Key`是字典中鍵的數據類型，`Value`是字典中對應於這些鍵所存儲值的數據類型。

> 注意：  
> 一個字典的`Key`類型必須遵循`Hashable`協議，就像`Set`的值類型。

我們也可以用`[Key: Value]`這樣簡化的形式去創建一個字典類型。雖然這兩種形式功能上相同，但是後者是首選，並且這本指導書涉及到字典類型時通篇采用後者。

<a name="creating_an_empty_dictionary"></a>
### 創建一個空字典

我們可以像數組一樣使用構造語法創建一個擁有確定類型的空字典：

```swift
var namesOfIntegers = [Int: String]()
// namesOfIntegers 是一個空的 [Int: String] 字典
```

這個例子創建了一個`[Int: String]`類型的空字典來儲存整數的英語命名。它的鍵是`Int`型，值是`String`型。

如果上下文已經提供了類型信息，我們可以使用空字典字面量來創建一個空字典，記作`[:]`（中括號中放一個冒號）：

```swift
namesOfIntegers[16] = "sixteen"
// namesOfIntegers 現在包含一個鍵值對
namesOfIntegers = [:]
// namesOfIntegers 又成為了一個 [Int: String] 類型的空字典
```

<a name="creating_a_dictionary_with_a_dictionary_literal"></a>
## 用字典字面量創建字典

我們可以使用*字典字面量*來構造字典，這和我們剛才介紹過的數組字面量擁有相似語法。字典字面量是一種將一個或多個鍵值對寫作`Dictionary`集合的快捷途徑。

一個鍵值對是一個`key`和一個`value`的結合體。在字典字面量中，每一個鍵值對的鍵和值都由冒號分割。這些鍵值對構成一個列表，其中這些鍵值對由方括號包含、由逗號分割：

```swift
[key 1: value 1, key 2: value 2, key 3: value 3]
```

下面的例子創建了一個存儲國際機場名稱的字典。在這個字典中鍵是三個字母的國際航空運輸相關代碼，值是機場名稱：

```swift
var airports: [String: String] = ["YYZ": "Toronto Pearson", "DUB": "Dublin"]
```

`airports`字典被聲明為一種`[String: String]`類型，這意味著這個字典的鍵和值都是`String`類型。

> 注意：  
> `airports`字典被聲明為變量（用`var`關鍵字）而不是常量（`let`關鍵字）因為後來更多的機場信息會被添加到這個示例字典中。

`airports`字典使用字典字面量初始化，包含兩個鍵值對。第一對的鍵是`YYZ`，值是`Toronto Pearson`。第二對的鍵是`DUB`，值是`Dublin`。

這個字典語句包含了兩個`String: String`類型的鍵值對。它們對應`airports`變量聲明的類型（一個只有`String`鍵和`String`值的字典）所以這個字典字面量的任務是構造擁有兩個初始數據項的`airport`字典。

和數組一樣，我們在用字典字面量構造字典時，如果它的鍵和值都有各自一致的類型，那麼就不必寫出字典的類型。
`airports`字典也可以用這種簡短方式定義：

```swift
var airports = ["YYZ": "Toronto Pearson", "DUB": "Dublin"]
```

因為這個語句中所有的鍵和值都各自擁有相同的數據類型，Swift 可以推斷出`Dictionary<String, String>`是`airports`字典的正確類型。

<a name="accessing_and_modifying_a_dictionary"></a>
### 訪問和修改字典

我們可以通過字典的方法和屬性來訪問和修改字典，或者通過使用下標語法。

和數組一樣，我們可以通過字典的只讀屬性`count`來獲取某個字典的數據項數量：

```swift
print("The dictionary of airports contains \(airports.count) items.")
// 打印 "The dictionary of airports contains 2 items."（這個字典有兩個數據項）
```

使用布爾屬性`isEmpty`作為一個縮寫形式去檢查`count`屬性是否為`0`：

```swift
if airports.isEmpty {
    print("The airports dictionary is empty.")
} else {
    print("The airports dictionary is not empty.")
}
// 打印 "The airports dictionary is not empty."
```

我們也可以在字典中使用下標語法來添加新的數據項。可以使用一個恰當類型的鍵作為下標索引，並且分配恰當類型的新值：

```swift
airports["LHR"] = "London"
// airports 字典現在有三個數據項
```

我們也可以使用下標語法來改變特定鍵對應的值：

```swift
airports["LHR"] = "London Heathrow"
// "LHR"對應的值 被改為 "London Heathrow
```

作為另一種下標方法，字典的`updateValue(_:forKey:)`方法可以設置或者更新特定鍵對應的值。就像上面所示的下標示例，`updateValue(_:forKey:)`方法在這個鍵不存在對應值的時候會設置新值或者在存在時更新已存在的值。和上面的下標方法不同的，`updateValue(_:forKey:)`這個方法返回更新值之前的原值。這樣使得我們可以檢查更新是否成功。

`updateValue(_:forKey:)`方法會返回對應值的類型的可選值。舉例來說：對於存儲`String`值的字典，這個函數會返回一個`String?`或者「可選 `String`」類型的值。

如果有值存在於更新前，則這個可選值包含了舊值，否則它將會是`nil`。

```swift
if let oldValue = airports.updateValue("Dublin Airport", forKey: "DUB") {
    print("The old value for DUB was \(oldValue).")
}
// 輸出 "The old value for DUB was Dublin."
```

我們也可以使用下標語法來在字典中檢索特定鍵對應的值。因為有可能請求的鍵沒有對應的值存在，字典的下標訪問會返回對應值的類型的可選值。如果這個字典包含請求鍵所對應的值，下標會返回一個包含這個存在值的可選值，否則將返回`nil`：

```swift
if let airportName = airports["DUB"] {
    print("The name of the airport is \(airportName).")
} else {
    print("That airport is not in the airports dictionary.")
}
// 打印 "The name of the airport is Dublin Airport."
```

我們還可以使用下標語法來通過給某個鍵的對應值賦值為`nil`來從字典裡移除一個鍵值對：

```swift
airports["APL"] = "Apple Internation"
// "Apple Internation" 不是真的 APL 機場, 刪除它
airports["APL"] = nil
// APL 現在被移除了
```

此外，`removeValue(forKey:)`方法也可以用來在字典中移除鍵值對。這個方法在鍵值對存在的情況下會移除該鍵值對並且返回被移除的值或者在沒有值的情況下返回`nil`：

```swift
if let removedValue = airports. removeValue(forKey: "DUB") {
    print("The removed airport's name is \(removedValue).")
} else {
    print("The airports dictionary does not contain a value for DUB.")
}
// prints "The removed airport's name is Dublin Airport."
```

<a name="iterating_over_a_dictionary"></a>
### 字典遍歷

我們可以使用`for-in`循環來遍歷某個字典中的鍵值對。每一個字典中的數據項都以`(key, value)`元組形式返回，並且我們可以使用臨時常量或者變量來分解這些元組：

```swift
for (airportCode, airportName) in airports {
    print("\(airportCode): \(airportName)")
}
// YYZ: Toronto Pearson
// LHR: London Heathrow
```

更多關於`for-in`循環的信息，參見[For 循環](./05_Control_Flow.html#for_loops)。

通過訪問`keys`或者`values`屬性，我們也可以遍歷字典的鍵或者值：

```swift
for airportCode in airports.keys {
    print("Airport code: \(airportCode)")
}
// Airport code: YYZ
// Airport code: LHR

for airportName in airports.values {
    print("Airport name: \(airportName)")
}
// Airport name: Toronto Pearson
// Airport name: London Heathrow
```

如果我們只是需要使用某個字典的鍵集合或者值集合來作為某個接受`Array`實例的 API 的參數，可以直接使用`keys`或者`values`屬性構造一個新數組：

```swift
let airportCodes = [String](airports.keys)
// airportCodes 是 ["YYZ", "LHR"]

let airportNames = [String](airports.values)
// airportNames 是 ["Toronto Pearson", "London Heathrow"]
```

Swift 的字典類型是無序集合類型。為了以特定的順序遍歷字典的鍵或值，可以對字典的`keys`或`values`屬性使用`sorted()`方法。
