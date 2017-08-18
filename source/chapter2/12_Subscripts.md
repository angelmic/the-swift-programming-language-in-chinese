# 下標
-----------------

> 1.0
> 翻譯：[siemenliu](https://github.com/siemenliu)
> 校對：[zq54zquan](https://github.com/zq54zquan)

> 2.0
> 翻譯+校對：[shanks](http://codebuild.me)

> 2.1
> 翻譯+校對：[shanks](http://codebuild.me)，[Realank](https://github.com/Realank)  

> 2.2
> 校對：[SketchK](https://github.com/SketchK) 2016-05-13  
> 3.0.1，shanks，2016-11-13


本頁包含內容：

- [下標語法](#subscript_syntax)
- [下標用法](#subscript_usage)
- [下標選項](#subscript_options)

*下標*可以定義在類、結構體和枚舉中，是訪問集合，列表或序列中元素的快捷方式。可以使用下標的索引，設置和獲取值，而不需要再調用對應的存取方法。舉例來說，用下標訪問一個`Array`實例中的元素可以寫作`someArray[index]`，訪問`Dictionary`實例中的元素可以寫作`someDictionary[key]`。

一個類型可以定義多個下標，通過不同索引類型進行重載。下標不限於一維，你可以定義具有多個入參的下標滿足自定義類型的需求。

<a name="subscript_syntax"></a>
## 下標語法

下標允許你通過在實例名稱後面的方括號中傳入一個或者多個索引值來對實例進行存取。語法類似於實例方法語法和計算型屬性語法的混合。與定義實例方法類似，定義下標使用`subscript`關鍵字，指定一個或多個輸入參數和返回類型；與實例方法不同的是，下標可以設定為讀寫或只讀。這種行為由 getter 和 setter 實現，有點類似計算型屬性：

```swift
subscript(index: Int) -> Int {
    get {
      // 返回一個適當的 Int 類型的值
    }

    set(newValue) {
      // 執行適當的賦值操作
    }
}
```

`newValue`的類型和下標的返回類型相同。如同計算型屬性，可以不指定 setter 的參數（`newValue`）。如果不指定參數，setter 會提供一個名為`newValue`的默認參數。

如同只讀計算型屬性，可以省略只讀下標的`get`關鍵字：

```swift
subscript(index: Int) -> Int {
    // 返回一個適當的 Int 類型的值
}
```

下面代碼演示了只讀下標的實現，這裡定義了一個`TimesTable`結構體，用來表示傳入整數的乘法表：

```swift
struct TimesTable {
    let multiplier: Int
    subscript(index: Int) -> Int {
        return multiplier * index
    }
}
let threeTimesTable = TimesTable(multiplier: 3)
print("six times three is \(threeTimesTable[6])")
// 打印 "six times three is 18"
```

在上例中，創建了一個`TimesTable`實例，用來表示整數`3`的乘法表。數值`3`被傳遞給結構體的構造函數，作為實例成員`multiplier`的值。

你可以通過下標訪問`threeTimesTable`實例，例如上面演示的`threeTimesTable[6]`。這條語句查詢了`3`的乘法表中的第六個元素，返回`3`的`6`倍即`18`。

> 注意  
> `TimesTable`例子基於一個固定的數學公式，對`threeTimesTable[someIndex]`進行賦值操作並不合適，因此下標定義為只讀的。  

<a name="subscript_usage"></a>
## 下標用法

下標的確切含義取決於使用場景。下標通常作為訪問集合，列表或序列中元素的快捷方式。你可以針對自己特定的類或結構體的功能來自由地以最恰當的方式實現下標。

例如，Swift 的`Dictionary`類型實現下標用於對其實例中儲存的值進行存取操作。為字典設值時，在下標中使用和字典的鍵類型相同的鍵，並把一個和字典的值類型相同的值賦給這個下標：

```swift
var numberOfLegs = ["spider": 8, "ant": 6, "cat": 4]
numberOfLegs["bird"] = 2
```

上例定義一個名為`numberOfLegs`的變量，並用一個包含三對鍵值的字典字面量初始化它。`numberOfLegs`字典的類型被推斷為`[String: Int]`。字典創建完成後，該例子通過下標將`String`類型的鍵`bird`和`Int`類型的值`2`添加到字典中。

更多關於`Dictionary`下標的信息請參考[讀取和修改字典](./04_Collection_Types.html#accessing_and_modifying_a_dictionary)

> 注意  
> Swift 的`Dictionary`類型的下標接受並返回可選類型的值。上例中的`numberOfLegs`字典通過下標返回的是一個`Int?`或者說「可選的int」。`Dictionary`類型之所以如此實現下標，是因為不是每個鍵都有個對應的值，同時這也提供了一種通過鍵刪除對應值的方式，只需將鍵對應的值賦值為`nil`即可。  

<a name="subscript_options"></a>
## 下標選項

下標可以接受任意數量的入參，並且這些入參可以是任意類型。下標的返回值也可以是任意類型。下標可以使用變量參數和可變參數，但不能使用輸入輸出參數，也不能給參數設置默認值。

一個類或結構體可以根據自身需要提供多個下標實現，使用下標時將通過入參的數量和類型進行區分，自動匹配合適的下標，這就是*下標的重載*。

雖然接受單一入參的下標是最常見的，但也可以根據情況定義接受多個入參的下標。例如下例定義了一個`Matrix`結構體，用於表示一個`Double`類型的二維矩陣。`Matrix`結構體的下標接受兩個整型參數：

```swift
struct Matrix {
    let rows: Int, columns: Int
    var grid: [Double]
    init(rows: Int, columns: Int) {
        self.rows = rows
        self.columns = columns
        grid = Array(count: rows * columns, repeatedValue: 0.0)
    }
    func indexIsValidForRow(row: Int, column: Int) -> Bool {
        return row >= 0 && row < rows && column >= 0 && column < columns
    }
    subscript(row: Int, column: Int) -> Double {
        get {
            assert(indexIsValidForRow(row, column: column), "Index out of range")
            return grid[(row * columns) + column]
        }
        set {
            assert(indexIsValidForRow(row, column: column), "Index out of range")
            grid[(row * columns) + column] = newValue
        }
    }
}
```

`Matrix`提供了一個接受兩個入參的構造方法，入參分別是`rows`和`columns`，創建了一個足夠容納`rows * columns`個`Double`類型的值的數組。通過傳入數組長度和初始值`0.0`到數組的構造器，將矩陣中每個位置的值初始化為`0.0`。關於數組的這種構造方法請參考[創建一個空數組](./04_Collection_Types.html#creating_an_empty_array)。

你可以通過傳入合適的`row`和`column`的數量來構造一個新的`Matrix`實例：

```swift
var matrix = Matrix(rows: 2, columns: 2)
```

上例中創建了一個`Matrix`實例來表示兩行兩列的矩陣。該`Matrix`實例的`grid`數組按照從左上到右下的閱讀順序將矩陣扁平化存儲：

![](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/subscriptMatrix01_2x.png)

將`row`和`column`的值傳入下標來為矩陣設值，下標的入參使用逗號分隔：

```swift
matrix[0, 1] = 1.5
matrix[1, 0] = 3.2
```

上面兩條語句分別調用下標的 setter 將矩陣右上角位置（即`row`為`0`、`column`為`1`的位置）的值設置為`1.5`，將矩陣左下角位置（即`row`為`1`、`column`為`0`的位置）的值設置為`3.2`：

![](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/subscriptMatrix02_2x.png)

`Matrix`下標的 getter 和 setter 中都含有斷言，用來檢查下標入參`row`和`column`的值是否有效。為了方便進行斷言，`Matrix`包含了一個名為`indexIsValidForRow(_:column:)`的便利方法，用來檢查入參`row`和`column`的值是否在矩陣范圍內：

```swift
func indexIsValidForRow(row: Int, column: Int) -> Bool {
    return row >= 0 && row < rows && column >= 0 && column < columns
}
```

斷言在下標越界時觸發：

```swift
let someValue = matrix[2, 2]
// 斷言將會觸發，因為 [2, 2] 已經超過了 matrix 的范圍
```
