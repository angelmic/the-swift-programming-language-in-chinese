# 可選類型完美解決占位問題
-----------------

> 翻譯：[老碼團隊翻譯組-Tyrion](http://weibo.com/u/5241713117)  
> 校對：[老碼團隊翻譯組-Ayra](http://weibo.com/littlekok/)

本頁包含內容：

- [為Dictionary增加objectsForKeys函數](#add-function)
- [Swift中更簡便的方法](##easy-function)
- [內嵌可選類型](#nested-optional)
- [提供一個默認值](#provide-default)

可選類型是Swift中新引入的，功能很強大。在這篇博文裡討論的，是在Swift裡，如何通過可選類型來保證強類型的安全性。作為例子，我們來創建一個Objective-C API的Swift版本，但實際上Swift本身並不需要這樣的API。


<a name="#add-function"></a>
#### 為Dictionary增加objectsForKeys函數

在Objective-C中，```NSDictionary```有一個方法```-objectsForKeys:NoFoundMarker:```, 這個方法需要一個```NSArray```數組作為鍵值參數，然後返回一個包含相關值的數組。文檔裡寫到："返回數組中的第N個值，和輸入數組中的第N個值相對應"，那如果有某個鍵值在字典裡不存在呢？於是就有了```notFoundMarker```作為返回提示。比如第三個鍵值沒有找到，那麼在返回數組中第三個值就是這個```notFoundMarker```，而不是字典中的第三個值，但是這個值只是用來提醒原字典中沒有找到對應值，但在返回數組中該元素存在，且用```notFoundMarker```作為占位符，因為這個對象不能直接使用，所以在Foundation框架中有個專門的類處理這個情況：```NSNull```。

在Swift中，```Dictionary```類沒有類似```objectsForKeys```的函數，為了說明問題，我們動手加一個，並且使其成為操作字典值的通用方法。我們可以用```extension```來實現：

```swift
extension Dictionary{
	func valuesForKeys(keys:[K], notFoundMarker: V )->[V]{
		//具體實現代碼後面會寫到
	}
}
```

以上就是我們實現的Swift版本，這個和Objective-C版本有很大區別。在Swift中，因為其強類型的原因限制了返回的結果數組只能包含單一類型的元素，所以我們不能放```NSNull```在字符串數組中，但是，Swift有更好的選擇，我們可以返回一個可選類型數據。我們所有的值都封包在可選類型中，而不是```NSNull```, 我們只用```nil```就可以了。


```swift
extension Dictionary{
    func valuesForKeys(keys: [Key]) -> [Value?] {
        var result = [Value?]()
        result.reserveCapacity(keys.count)
        for key in keys{
            result.append(self[key])
        }
        return result
    }
}
```

<a name="#easy-function"></a>
#### Swift中更簡便的方法

小伙伴們可能會問，為什麼Swift中不需要實現這麼一個API呢？其實其有更簡單的實現，如下面代碼所示：

```swift
extension Dictionary {
	func valuesForKeys(keys: [Key]) -> [Value?] {
		return keys.map { self[$0] }
	}
}
```

上述方式實現的功能和最開始的方法實現的功能相同，雖然核心的功能是封裝了```map```的調用，這個例子也說明了為什麼Swift沒有提供輕量級的API接口，因為小伙伴們簡單的調用```map```就可以實現。

接下來，我們實驗幾個例子：

```swift
var dic: Dictionary = [ "1": 2, "3":3, "4":5 ]

var t = dic.valuesForKeys(["1", "4"]) 
//結果為：[Optional(2), Optional(5)]

var t = dict.valuesForKeys(["3", "9"])
// 結果為：[Optional(3), nil]

t = dic.valuesForKeys([])
//結果為：[]
```

<a name="#nested-optional"></a>
#### 內嵌可選類型

現在，如果我們為每一個結果調用```last```方法，看下結果如何？

```swift
var dic: Dictionary = [ "1": 2, "3":3, "4":5 ]

var t = dic.valuesForKeys(["1", "4"]).last //結果為：Optional(Optional(5))
// Optional(Optional("Ching"))

var t = dict.valuesForKeys(["3", "9"]).last
// 結果為：Optional(nil)

var t = dict.valuesForKeys([]).last
// 結果為：nil

```

小伙伴們立馬迷糊了，為什麼會出現兩層包含的可選類型呢？，特別對第二種情況的```Optional(nil)```，這是什麼節奏？

我們回過頭看看```last```屬性的定義：

```swift
var last:T? { get }
```

很明顯```last```屬性的類型是數組元素類型的可選類型，這種情況下，因為元素類型是```(String?)```，那麼再結合返回的類型，於是其結果就是```String??```了，這就是所謂的嵌套可選類型。但嵌套可選類型本質是什麼意思呢？

如果在Objective-C中重新調用上述方法，我們將使用```NSNull```作為占位符，Objective-C的調用語法如下所示：

```swift
[dict valuesForKeys:@[@"1", @"4"] notFoundMarker:[NSNull null]].lastObject
// 5
[dict valuesForKeys:@[@"1", @"3"] notFoundMarker:[NSNull null]].lastObject
// NSNull
[dict valuesForKeys:@[] notFoundMarker:[NSNull null]].lastObject
// nil
```

不管是Swift版本還是Objective-C版本，返回值為```nil```都意味數組是空的，所以它就沒有最後一個元素。 但是如果返回是```Optional(nil)```或者Objective-C中的```NSNull```都表示數組中的最後一個元素存在，但是元素的內容是空的。在Objective-C中只能借助```NSNull```作為占位符來達到這個目的，但是Swift卻可以語言系統類型的角度的實現。

<a name="#provide-default"></a>
#### 提供一個默認值

進一步封裝，如果我字典中的某個或某些元素不存在，我們想提供一個默認值怎麼辦呢？實現方法很簡單：

```swift
extension Dictionary {
	func valuesForKeys( keys:[Key], notFoundMarker: Value)->[Value]{
		return self.valueForKeys(kes).map{ $0 ?? notFoundMarker }
	}
}
```

```
dict.valuesForKeys(["1", "5"], notFoundMarker: "Anonymous")
```

和Objective-C相比，其需要占位符來達到占位的目的，但是Swift卻已經從語言類型系統的層面原生的支持了這種用法，同時提供了豐富的語法功能。這就是Swift可選類型的強大之處。同時注意上述例子中用到了空合運算符```??```。

-----------------
本章節不是老碼的原創，是老碼認真的閱讀了蘋果的官方博客，自己的練習總結，如果小伙伴們費了吃奶的勁還是看不懂，請找度娘谷歌。還是看不懂？請到老碼[官方微博](http://weibo.com/u/5241713117)咆哮。  

##### 本文由翻譯自Apple Swift Blog ：[Optionals Case Study: valuesForKeys](https://developer.apple.com/swift/blog/?id=12)
