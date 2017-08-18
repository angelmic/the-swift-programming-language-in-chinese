# 錯誤處理
-----------------

> 2.1
> 翻譯+校對：[lyojo](https://github.com/lyojo) [ray16897188](https://github.com/ray16897188) 2015-10-23
> 校對：[shanks](http://codebuild.me) 2015-10-24  
>  
> 2.2
> 翻譯+校對：[SketchK](https://github.com/SketchK) 2016-05-15
>
> 3.0
> 翻譯+校對：[shanks](http://codebuild.me) 2016-09-24   
> 3.0.1，shanks，2016-11-13

本頁包含內容：

- [表示並拋出錯誤](#representing_and_throwing_errors)
- [處理錯誤](#handling_errors)
- [指定清理操作](#specifying_cleanup_actions)

*錯誤處理（Error handling）*是響應錯誤以及從錯誤中恢復的過程。Swift 提供了在運行時對可恢復錯誤的拋出、捕獲、傳遞和操作的一等公民支持。

某些操作無法保證總是執行完所有代碼或總是生成有用的結果。可選類型可用來表示值缺失，但是當某個操作失敗時，最好能得知失敗的原因，從而可以作出相應的應對。

舉個例子，假如有個從磁盤上的某個文件讀取數據並進行處理的任務，該任務會有多種可能失敗的情況，包括指定路徑下文件並不存在，文件不具有可讀權限，或者文件編碼格式不兼容。區分這些不同的失敗情況可以讓程序解決並處理某些錯誤，然後把它解決不了的錯誤報告給用戶。

> 注意  
Swift 中的錯誤處理涉及到錯誤處理模式，這會用到 Cocoa 和 Objective-C 中的`NSError`。關於這個類的更多信息請參見 [Using Swift with Cocoa and Objective-C (Swift 3.0.1)](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/BuildingCocoaApps/index.html#//apple_ref/doc/uid/TP40014216) 中的[錯誤處理](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/BuildingCocoaApps/AdoptingCocoaDesignPatterns.html#//apple_ref/doc/uid/TP40014216-CH7-ID10)。

<a name="representing_and_throwing_errors"></a>
## 表示並拋出錯誤

在 Swift 中，錯誤用符合`Error`協議的類型的值來表示。這個空協議表明該類型可以用於錯誤處理。

Swift 的枚舉類型尤為適合構建一組相關的錯誤狀態，枚舉的關聯值還可以提供錯誤狀態的額外信息。例如，你可以這樣表示在一個游戲中操作自動販賣機時可能會出現的錯誤狀態：

```swift
enum VendingMachineError: Error {
	case invalidSelection				     //選擇無效
	case insufficientFunds(coinsNeeded: Int) //金額不足
	case outOfStock			                 //缺貨
}
```

拋出一個錯誤可以讓你表明有意外情況發生，導致正常的執行流程無法繼續執行。拋出錯誤使用`throw`關鍵字。例如，下面的代碼拋出一個錯誤，提示販賣機還需要`5`個硬幣：

```swift
throw VendingMachineError. insufficientFunds(coinsNeeded: 5)
```

<a name="handling_errors"></a>
## 處理錯誤

某個錯誤被拋出時，附近的某部分代碼必須負責處理這個錯誤，例如糾正這個問題、嘗試另外一種方式、或是向用戶報告錯誤。

Swift 中有`4`種處理錯誤的方式。你可以把函數拋出的錯誤傳遞給調用此函數的代碼、用`do-catch`語句處理錯誤、將錯誤作為可選類型處理、或者斷言此錯誤根本不會發生。每種方式在下面的小節中都有描述。

當一個函數拋出一個錯誤時，你的程序流程會發生改變，所以重要的是你能迅速識別代碼中會拋出錯誤的地方。為了標識出這些地方，在調用一個能拋出錯誤的函數、方法或者構造器之前，加上`try`關鍵字，或者`try?`或`try!`這種變體。這些關鍵字在下面的小節中有具體講解。

> 注意  
> Swift 中的錯誤處理和其他語言中用`try`，`catch`和`throw`進行異常處理很像。和其他語言中（包括 Objective-C ）的異常處理不同的是，Swift 中的錯誤處理並不涉及解除調用棧，這是一個計算代價高昂的過程。就此而言，`throw`語句的性能特性是可以和`return`語句相媲美的。

<a name="propagating_errors_using_throwing_functions"></a>
### 用 throwing 函數傳遞錯誤

為了表示一個函數、方法或構造器可以拋出錯誤，在函數聲明的參數列表之後加上`throws`關鍵字。一個標有`throws`關鍵字的函數被稱作*throwing 函數*。如果這個函數指明了返回值類型，`throws`關鍵詞需要寫在箭頭（`->`）的前面。

```swift
func canThrowErrors() throws -> String
func cannotThrowErrors() -> String
```

一個 throwing 函數可以在其內部拋出錯誤，並將錯誤傳遞到函數被調用時的作用域。

> 注意  
只有 throwing 函數可以傳遞錯誤。任何在某個非 throwing 函數內部拋出的錯誤只能在函數內部處理。

下面的例子中，`VendingMachine`類有一個`vend(itemNamed:)`方法，如果請求的物品不存在、缺貨或者投入金額小於物品價格，該方法就會拋出一個相應的`VendingMachineError`：

```swift
struct Item {
	var price: Int
	var count: Int
}

class VendingMachine {
    var inventory = [
        "Candy Bar": Item(price: 12, count: 7),
        "Chips": Item(price: 10, count: 4),
        "Pretzels": Item(price: 7, count: 11)
    ]
    var coinsDeposited = 0
    func dispenseSnack(snack: String) {
        print("Dispensing \(snack)")
    }

    func vend(itemNamed name: String) throws {
        guard let item = inventory[name] else {
            throw VendingMachineError.invalidSelection
        }

        guard item.count > 0 else {
            throw VendingMachineError.outOfStock
        }

        guard item.price <= coinsDeposited else {
            throw VendingMachineError.insufficientFunds(coinsNeeded: item.price - coinsDeposited)
        }

        coinsDeposited -= item.price

        var newItem = item
        newItem.count -= 1
        inventory[name] = newItem

        print("Dispensing \(name)")
    }
}
```

在`vend(itemNamed:)`方法的實現中使用了`guard`語句來提前退出方法，確保在購買某個物品所需的條件中，有任一條件不滿足時，能提前退出方法並拋出相應的錯誤。由於`throw`語句會立即退出方法，所以物品只有在所有條件都滿足時才會被售出。

因為`vend(itemNamed:)`方法會傳遞出它拋出的任何錯誤，在你的代碼中調用此方法的地方，必須要麼直接處理這些錯誤——使用`do-catch`語句，`try?`或`try!`；要麼繼續將這些錯誤傳遞下去。例如下面例子中，`buyFavoriteSnack(person:vendingMachine:)`同樣是一個 throwing 函數，任何由`vend(itemNamed:)`方法拋出的錯誤會一直被傳遞到`buyFavoriteSnack(person:vendingMachine:) `函數被調用的地方。

```swift
let favoriteSnacks = [
	"Alice": "Chips",
	"Bob": "Licorice",
	"Eve": "Pretzels",
]
func buyFavoriteSnack(person: String, vendingMachine: VendingMachine) throws {
	let snackName = favoriteSnacks[person] ?? "Candy Bar"
	try vendingMachine.vend(itemNamed: snackName)
}
```

上例中，`buyFavoriteSnack(person:vendingMachine:) `函數會查找某人最喜歡的零食，並通過調用`vend(itemNamed:)`方法來嘗試為他們購買。因為`vend(itemNamed:)`方法能拋出錯誤，所以在調用的它時候在它前面加了`try`關鍵字。  

`throwing`構造器能像`throwing`函數一樣傳遞錯誤。例如下面代碼中的`PurchasedSnack`構造器在構造過程中調用了throwing函數，並且通過傳遞到它的調用者來處理這些錯誤。  

```swift
struct PurchasedSnack {
    let name: String
    init(name: String, vendingMachine: VendingMachine) throws {
        try vendingMachine.vend(itemNamed: name)
        self.name = name
    }
}
```


### 用 Do-Catch 處理錯誤

可以使用一個`do-catch`語句運行一段閉包代碼來處理錯誤。如果在`do`子句中的代碼拋出了一個錯誤，這個錯誤會與`catch`子句做匹配，從而決定哪條子句能處理它。

下面是`do-catch`語句的一般形式：

```swift
do {
    try expression
    statements
} catch pattern 1 {
    statements
} catch pattern 2 where condition {
    statements
}
```

在`catch`後面寫一個匹配模式來表明這個子句能處理什麼樣的錯誤。如果一條`catch`子句沒有指定匹配模式，那麼這條子句可以匹配任何錯誤，並且把錯誤綁定到一個名字為`error`的局部常量。關於模式匹配的更多信息請參考 [模式](../chapter3/07_Patterns.html)。

`catch`子句不必將`do`子句中的代碼所拋出的每一個可能的錯誤都作處理。如果所有`catch`子句都未處理錯誤，錯誤就會傳遞到周圍的作用域。然而，錯誤還是必須要被某個周圍的作用域處理的——要麼是一個外圍的`do-catch`錯誤處理語句，要麼是一個 throwing 函數的內部。舉例來說，下面的代碼處理了`VendingMachineError`枚舉類型的全部枚舉值，但是所有其它的錯誤就必須由它周圍的作用域處理：

```swift
var vendingMachine = VendingMachine()
vendingMachine.coinsDeposited = 8
do {
    try buyFavoriteSnack(person: "Alice", vendingMachine: vendingMachine)
} catch VendingMachineError.invalidSelection {
    print("Invalid Selection.")
} catch VendingMachineError.outOfStock {
    print("Out of Stock.")
} catch VendingMachineError.insufficientFunds(let coinsNeeded) {
    print("Insufficient funds. Please insert an additional \(coinsNeeded) coins.")
}
// 打印 「Insufficient funds. Please insert an additional 2 coins.」
```

上面的例子中，`buyFavoriteSnack(person:vendingMachine:) `函數在一個`try`表達式中調用，因為它能拋出錯誤。如果錯誤被拋出，相應的執行會馬上轉移到`catch`子句中，並判斷這個錯誤是否要被繼續傳遞下去。如果沒有錯誤拋出，`do`子句中余下的語句就會被執行。

### 將錯誤轉換成可選值

可以使用`try?`通過將錯誤轉換成一個可選值來處理錯誤。如果在評估`try?`表達式時一個錯誤被拋出，那麼表達式的值就是`nil`。例如，在下面的代碼中，`x`和`y`有著相同的數值和等價的含義：

```swift
func someThrowingFunction() throws -> Int {
    // ...
}

let x = try? someThrowingFunction()

let y: Int?
do {
    y = try someThrowingFunction()
} catch {
    y = nil
}
```

如果`someThrowingFunction()`拋出一個錯誤，`x`和`y`的值是`nil`。否則`x`和`y`的值就是該函數的返回值。注意，無論`someThrowingFunction()`的返回值類型是什麼類型，`x`和`y`都是這個類型的可選類型。例子中此函數返回一個整型，所以`x`和`y`是可選整型。

如果你想對所有的錯誤都采用同樣的方式來處理，用`try?`就可以讓你寫出簡潔的錯誤處理代碼。例如，下面的代碼用幾種方式來獲取數據，如果所有方式都失敗了則返回`nil`。

```swift
func fetchData() -> Data? {
    if let data = try? fetchDataFromDisk() { return data }
    if let data = try? fetchDataFromServer() { return data }
    return nil
}
```

### 禁用錯誤傳遞

有時你知道某個`throwing`函數實際上在運行時是不會拋出錯誤的，在這種情況下，你可以在表達式前面寫`try!`來禁用錯誤傳遞，這會把調用包裝在一個不會有錯誤拋出的運行時斷言中。如果真的拋出了錯誤，你會得到一個運行時錯誤。

例如，下面的代碼使用了`loadImage(atPath:)`函數，該函數從給定的路徑加載圖片資源，如果圖片無法載入則拋出一個錯誤。在這種情況下，因為圖片是和應用綁定的，運行時不會有錯誤拋出，所以適合禁用錯誤傳遞。

```swift
let photo = try! loadImage(atPath: "./Resources/John Appleseed.jpg")
```

<a name="specifying_cleanup_actions"></a>
## 指定清理操作

可以使用`defer`語句在即將離開當前代碼塊時執行一系列語句。該語句讓你能執行一些必要的清理工作，不管是以何種方式離開當前代碼塊的——無論是由於拋出錯誤而離開，還是由於諸如`return`或者`break`的語句。例如，你可以用`defer`語句來確保文件描述符得以關閉，以及手動分配的內存得以釋放。

`defer`語句將代碼的執行延遲到當前的作用域退出之前。該語句由`defer`關鍵字和要被延遲執行的語句組成。延遲執行的語句不能包含任何控制轉移語句，例如`break`或是`return`語句，或是拋出一個錯誤。延遲執行的操作會按照它們被指定時的順序的相反順序執行——也就是說，第一條`defer`語句中的代碼會在第二條`defer`語句中的代碼被執行之後才執行，以此類推。

```swift
func processFile(filename: String) throws {
    if exists(filename) {
        let file = open(filename)
        defer {
            close(file)
        }
        while let line = try file.readline() {
            // 處理文件。
        }
        // close(file) 會在這裡被調用，即作用域的最後。
    }
}
```

上面的代碼使用一條`defer`語句來確保`open(_:)`函數有一個相應的對`close(_:)`函數的調用。

> 注意  
> 即使沒有涉及到錯誤處理，你也可以使用`defer`語句。
