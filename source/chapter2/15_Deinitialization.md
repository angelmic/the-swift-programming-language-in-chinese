# 析構過程
---------------------------

> 1.0
> 翻譯：[bruce0505](https://github.com/bruce0505)
> 校對：[fd5788](https://github.com/fd5788)

> 2.0
> 翻譯+校對：[chenmingbiao](https://github.com/chenmingbiao)

> 2.1
> 校對：[shanks](http://codebuild.me)，2015-10-31
> 
> 2.2
> 翻譯+校對：[SketchK](https://github.com/SketchK) 2016-05-14   
> 3.0.1，shanks，2016-11-13

本頁包含內容：

- [析構過程原理](#how_deinitialization_works)
- [析構器實踐](#deinitializers_in_action)

*析構器*只適用於類類型，當一個類的實例被釋放之前，析構器會被立即調用。析構器用關鍵字`deinit`來標示，類似於構造器要用`init`來標示。

<a name="how_deinitialization_works"></a>
## 析構過程原理

Swift 會自動釋放不再需要的實例以釋放資源。如[自動引用計數](./16_Automatic_Reference_Counting.html)章節中所講述，Swift 通過`自動引用計數（ARC）`處理實例的內存管理。通常當你的實例被釋放時不需要手動地去清理。但是，當使用自己的資源時，你可能需要進行一些額外的清理。例如，如果創建了一個自定義的類來打開一個文件，並寫入一些數據，你可能需要在類實例被釋放之前手動去關閉該文件。

在類的定義中，每個類最多只能有一個析構器，而且析構器不帶任何參數，如下所示：

```swift
deinit {
    // 執行析構過程
}
```

析構器是在實例釋放發生前被自動調用。你不能主動調用析構器。子類繼承了父類的析構器，並且在子類析構器實現的最後，父類的析構器會被自動調用。即使子類沒有提供自己的析構器，父類的析構器也同樣會被調用。

因為直到實例的析構器被調用後，實例才會被釋放，所以析構器可以訪問實例的所有屬性，並且可以根據那些屬性可以修改它的行為（比如查找一個需要被關閉的文件）。

<a name="deinitializers_in_action"></a>
## 析構器實踐

這是一個析構器實踐的例子。這個例子描述了一個簡單的游戲，這裡定義了兩種新類型，分別是`Bank`和`Player`。`Bank`類管理一種虛擬硬幣，確保流通的硬幣數量永遠不可能超過 10,000。在游戲中有且只能有一個`Bank`存在，因此`Bank`用類來實現，並使用類型屬性和類型方法來存儲和管理其當前狀態。

```swift
class Bank {
    static var coinsInBank = 10_000
    static func distribute(coins numberOfCoinsRequested: Int) -> Int {
        let numberOfCoinsToVend = min(numberOfCoinsRequested, coinsInBank)
        coinsInBank -= numberOfCoinsToVend
        return numberOfCoinsToVend
    }
    static func receive(coins: Int) {
        coinsInBank += coins
    }
}
```

`Bank`使用`coinsInBank`屬性來跟蹤它當前擁有的硬幣數量。`Bank`還提供了兩個方法，`distribute(coins:)`和`receive(coins:)`，分別用來處理硬幣的分發和收集。

`distribute(coins:)`方法在`Bank`對象分發硬幣之前檢查是否有足夠的硬幣。如果硬幣不足，`Bank`對象會返回一個比請求時小的數字（如果`Bank`對象中沒有硬幣了就返回`0`）。此方法返回一個整型值，表示提供的硬幣的實際數量。

`receive(coins:)`方法只是將`Bank`實例接收到的硬幣數目加回硬幣存儲中。

`Player`類描述了游戲中的一個玩家。每一個玩家在任意時間都有一定數量的硬幣存儲在他們的錢包中。這通過玩家的`coinsInPurse`屬性來表示：

```swift
class Player {
    var coinsInPurse: Int
    init(coins: Int) {
        coinsInPurse = Bank.distribute(coins: coins)
    }
    func win(coins: Int) {
        coinsInPurse += Bank.distribute(coins: coins)
    }
    deinit {
        Bank.receive(coins: coinsInPurse)
    }
}
```

每個`Player`實例在初始化的過程中，都從`Bank`對象獲取指定數量的硬幣。如果沒有足夠的硬幣可用，`Player`實例可能會收到比指定數量少的硬幣.

`Player`類定義了一個`win(coins:)`方法，該方法從`Bank`對象獲取一定數量的硬幣，並把它們添加到玩家的錢包。`Player`類還實現了一個析構器，這個析構器在`Player`實例釋放前被調用。在這裡，析構器的作用只是將玩家的所有硬幣都返還給`Bank`對象：

```swift
var playerOne: Player? = Player(coins: 100)
print("A new player has joined the game with \(playerOne!.coinsInPurse) coins")
// 打印 "A new player has joined the game with 100 coins"
print("There are now \(Bank.coinsInBank) coins left in the bank")
// 打印 "There are now 9900 coins left in the bank"
```

創建一個`Player`實例的時候，會向`Bank`對象請求 100 個硬幣，如果有足夠的硬幣可用的話。這個`Player`實例存儲在一個名為`playerOne`的可選類型的變量中。這裡使用了一個可選類型的變量，因為玩家可以隨時離開游戲，設置為可選使你可以追蹤玩家當前是否在游戲中。

因為`playerOne`是可選的，所以訪問其`coinsInPurse`屬性來打印錢包中的硬幣數量時，使用感嘆號（`!`）來解包：

```swift
playerOne!.win(coins: 2_000)
print("PlayerOne won 2000 coins & now has \(playerOne!.coinsInPurse) coins")
// 輸出 "PlayerOne won 2000 coins & now has 2100 coins"
print("The bank now only has \(Bank.coinsInBank) coins left")
// 輸出 "The bank now only has 7900 coins left"
```

這裡，玩家已經贏得了 2,000 枚硬幣，所以玩家的錢包中現在有 2,100 枚硬幣，而`Bank`對象只剩余 7,900 枚硬幣。

```swift
playerOne = nil
print("PlayerOne has left the game")
// 打印 "PlayerOne has left the game"
print("The bank now has \(Bank.coinsInBank) coins")
// 打印 "The bank now has 10000 coins"
```

玩家現在已經離開了游戲。這通過將可選類型的`playerOne`變量設置為`nil`來表示，意味著「沒有`Player`實例」。當這一切發生時，`playerOne`變量對`Player`實例的引用被破壞了。沒有其它屬性或者變量引用`Player`實例，因此該實例會被釋放，以便回收內存。在這之前，該實例的析構器被自動調用，玩家的硬幣被返還給銀行。
