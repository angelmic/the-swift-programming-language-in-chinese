# 類型轉換
-----------------

> 1.0
> 翻譯：[xiehurricane](https://github.com/xiehurricane)
> 校對：[happyming](https://github.com/happyming)

> 2.0
> 翻譯+校對：[yangsiy](https://github.com/yangsiy)

> 2.1
> 校對：[shanks](http://codebuild.me)，2015-11-01
> 
> 2.2
> 翻譯+校對：[SketchK](https://github.com/SketchK) 2016-05-16  
> 3.0.1，shanks，2016-11-13

本頁包含內容：

- [定義一個類層次作為例子](#defining_a_class_hierarchy_for_type_casting)
- [檢查類型](#checking_type)
- [向下轉型](#downcasting)
- [`Any` 和 `AnyObject` 的類型轉換](#type_casting_for_any_and_anyobject)


_類型轉換_ 可以判斷實例的類型，也可以將實例看做是其父類或者子類的實例。

類型轉換在 Swift 中使用 `is` 和 `as` 操作符實現。這兩個操作符提供了一種簡單達意的方式去檢查值的類型或者轉換它的類型。

你也可以用它來檢查一個類型是否實現了某個協議，就像在[檢驗協議的一致性](./22_Protocols.html#checking_for_protocol_conformance)部分講述的一樣。

<a name="defining_a_class_hierarchy_for_type_casting"></a>
## 定義一個類層次作為例子

你可以將類型轉換用在類和子類的層次結構上，檢查特定類實例的類型並且轉換這個類實例的類型成為這個層次結構中的其他類型。下面的三個代碼段定義了一個類層次和一個包含了這些類實例的數組，作為類型轉換的例子。

第一個代碼片段定義了一個新的基類 `MediaItem`。這個類為任何出現在數字媒體庫的媒體項提供基礎功能。特別的，它聲明了一個 `String` 類型的 `name` 屬性，和一個 `init(name:)` 初始化器。（假定所有的媒體項都有個名稱。）

```swift
class MediaItem {
    var name: String
    init(name: String) {
        self.name = name
    }
}
```

下一個代碼段定義了 `MediaItem` 的兩個子類。第一個子類 `Movie` 封裝了與電影相關的額外信息，在父類（或者說基類）的基礎上增加了一個 `director`（導演）屬性，和相應的初始化器。第二個子類 `Song`，在父類的基礎上增加了一個 `artist`（藝術家）屬性，和相應的初始化器：

```swift
class Movie: MediaItem {
    var director: String
    init(name: String, director: String) {
        self.director = director
        super.init(name: name)
    }
}

class Song: MediaItem {
    var artist: String
    init(name: String, artist: String) {
        self.artist = artist
        super.init(name: name)
    }
}
```

最後一個代碼段創建了一個數組常量 `library`，包含兩個 `Movie` 實例和三個 `Song` 實例。`library` 的類型是在它被初始化時根據它數組中所包含的內容推斷來的。Swift 的類型檢測器能夠推斷出 `Movie` 和 `Song` 有共同的父類 `MediaItem`，所以它推斷出 `[MediaItem]` 類作為 `library` 的類型：

```swift
let library = [
    Movie(name: "Casablanca", director: "Michael Curtiz"),
    Song(name: "Blue Suede Shoes", artist: "Elvis Presley"),
    Movie(name: "Citizen Kane", director: "Orson Welles"),
    Song(name: "The One And Only", artist: "Chesney Hawkes"),
    Song(name: "Never Gonna Give You Up", artist: "Rick Astley")
]
// 數組 library 的類型被推斷為 [MediaItem]
```

在幕後 `library` 裡存儲的媒體項依然是 `Movie` 和 `Song` 類型的。但是，若你迭代它，依次取出的實例會是 `MediaItem` 類型的，而不是 `Movie` 和 `Song` 類型。為了讓它們作為原本的類型工作，你需要檢查它們的類型或者向下轉換它們到其它類型，就像下面描述的一樣。

<a name="checking_type"></a>
## 檢查類型

用類型檢查操作符（`is`）來檢查一個實例是否屬於特定子類型。若實例屬於那個子類型，類型檢查操作符返回 `true`，否則返回 `false`。

下面的例子定義了兩個變量，`movieCount` 和 `songCount`，用來計算數組 `library` 中 `Movie` 和 `Song` 類型的實例數量：

```swift
var movieCount = 0
var songCount = 0

for item in library {
    if item is Movie {
        movieCount += 1
    } else if item is Song {
        songCount += 1
    }
}

print("Media library contains \(movieCount) movies and \(songCount) songs")
// 打印 「Media library contains 2 movies and 3 songs」
```

示例迭代了數組 `library` 中的所有項。每一次，`for-in` 循環設置
`item` 為數組中的下一個 `MediaItem`。

若當前 `MediaItem` 是一個 `Movie` 類型的實例，`item is Movie` 返回
`true`，否則返回 `false`。同樣的，`item is Song` 檢查 `item` 是否為 `Song` 類型的實例。在循環結束後，`movieCount` 和 `songCount` 的值就是被找到的屬於各自類型的實例的數量。

<a name="downcasting"></a>
## 向下轉型

某類型的一個常量或變量可能在幕後實際上屬於一個子類。當確定是這種情況時，你可以嘗試向下轉到它的子類型，用*類型轉換操作符*（`as?` 或 `as!`）。

因為向下轉型可能會失敗，類型轉型操作符帶有兩種不同形式。條件形式`as?` 返回一個你試圖向下轉成的類型的可選值。強制形式 `as!` 把試圖向下轉型和強制解包轉換結果結合為一個操作。

當你不確定向下轉型可以成功時，用類型轉換的條件形式（`as?`）。條件形式的類型轉換總是返回一個可選值，並且若下轉是不可能的，可選值將是 `nil`。這使你能夠檢查向下轉型是否成功。

只有你可以確定向下轉型一定會成功時，才使用強制形式（`as!`）。當你試圖向下轉型為一個不正確的類型時，強制形式的類型轉換會觸發一個運行時錯誤。

下面的例子，迭代了 `library` 裡的每一個 `MediaItem`，並打印出適當的描述。要這樣做，`item` 需要真正作為 `Movie` 或 `Song` 的類型來使用，而不僅僅是作為 `MediaItem`。為了能夠在描述中使用 `Movie` 或 `Song` 的 `director` 或 `artist` 屬性，這是必要的。

在這個示例中，數組中的每一個 `item` 可能是 `Movie` 或 `Song`。事前你不知道每個 `item` 的真實類型，所以這裡使用條件形式的類型轉換（`as?`）去檢查循環裡的每次下轉：

```swift
for item in library {
    if let movie = item as? Movie {
        print("Movie: '\(movie.name)', dir. \(movie.director)")
    } else if let song = item as? Song {
        print("Song: '\(song.name)', by \(song.artist)")
    }
}

// Movie: 'Casablanca', dir. Michael Curtiz
// Song: 'Blue Suede Shoes', by Elvis Presley
// Movie: 'Citizen Kane', dir. Orson Welles
// Song: 'The One And Only', by Chesney Hawkes
// Song: 'Never Gonna Give You Up', by Rick Astley
```

示例首先試圖將 `item` 下轉為 `Movie`。因為 `item` 是一個 `MediaItem`
類型的實例，它可能是一個 `Movie`；同樣，它也可能是一個 `Song`，或者僅僅是基類
`MediaItem`。因為不確定，`as?` 形式在試圖下轉時將返回一個可選值。`item as? Movie` 的返回值是 `Movie?` 或者說「可選 `Movie`」。

當向下轉型為 `Movie` 應用在兩個 `Song`
實例時將會失敗。為了處理這種情況，上面的例子使用了可選綁定（optional binding）來檢查可選 `Movie` 真的包含一個值（這個是為了判斷下轉是否成功。）可選綁定是這樣寫的「`if let movie = item as? Movie`」，可以這樣解讀：

「嘗試將 `item` 轉為 `Movie` 類型。若成功，設置一個新的臨時常量 `movie` 來存儲返回的可選 `Movie` 中的值」

若向下轉型成功，然後 `movie` 的屬性將用於打印一個 `Movie` 實例的描述，包括它的導演的名字 `director`。相似的原理被用來檢測 `Song` 實例，當 `Song` 被找到時則打印它的描述（包含 `artist` 的名字）。

> 注意  
> 轉換沒有真的改變實例或它的值。根本的實例保持不變；只是簡單地把它作為它被轉換成的類型來使用。

<a name="type_casting_for_any_and_anyobject"></a>
## `Any` 和 `AnyObject` 的類型轉換

Swift 為不確定類型提供了兩種特殊的類型別名：

* `Any` 可以表示任何類型，包括函數類型。
* `AnyObject` 可以表示任何類類型的實例。

只有當你確實需要它們的行為和功能時才使用 `Any` 和 `AnyObject`。在你的代碼裡使用你期望的明確類型總是更好的。

這裡有個示例，使用 `Any` 類型來和混合的不同類型一起工作，包括函數類型和非類類型。它創建了一個可以存儲 `Any` 類型的數組 `things`：

```swift
var things = [Any]()

things.append(0)
things.append(0.0)
things.append(42)
things.append(3.14159)
things.append("hello")
things.append((3.0, 5.0))
things.append(Movie(name: "Ghostbusters", director: "Ivan Reitman"))
things.append({ (name: String) -> String in "Hello, \(name)" })
```

`things` 數組包含兩個 `Int` 值，兩個 `Double` 值，一個 `String` 值，一個元組 `(Double, Double)`，一個`Movie`實例「Ghostbusters」，以及一個接受 `String` 值並返回另一個 `String` 值的閉包表達式。

你可以在 `switch` 表達式的 `case` 中使用 `is` 和 `as` 操作符來找出只知道是 `Any` 或 `AnyObject` 類型的常量或變量的具體類型。下面的示例迭代 `things` 數組中的每一項，並用 `switch` 語句查找每一項的類型。有幾個 `switch` 語句的 `case` 綁定它們匹配到的值到一個指定類型的常量，從而可以打印這些值：

```swift
for thing in things {
    switch thing {
    case 0 as Int:
        print("zero as an Int")
    case 0 as Double:
        print("zero as a Double")
    case let someInt as Int:
        print("an integer value of \(someInt)")
    case let someDouble as Double where someDouble > 0:
        print("a positive double value of \(someDouble)")
    case is Double:
        print("some other double value that I don't want to print")
    case let someString as String:
        print("a string value of \"\(someString)\"")
    case let (x, y) as (Double, Double):
        print("an (x, y) point at \(x), \(y)")
    case let movie as Movie:
        print("a movie called '\(movie.name)', dir. \(movie.director)")
    case let stringConverter as String -> String:
        print(stringConverter("Michael"))
    default:
        print("something else")
    }
}

// zero as an Int
// zero as a Double
// an integer value of 42
// a positive double value of 3.14159
// a string value of "hello"
// an (x, y) point at 3.0, 5.0
// a movie called 'Ghostbusters', dir. Ivan Reitman
// Hello, Michael
```

> 注意   
> `Any`類型可以表示所有類型的值，包括可選類型。Swift 會在你用`Any`類型來表示一個可選值的時候，給你一個警告。如果你確實想使用`Any`類型來承載可選值，你可以使用`as`操作符顯式轉換為`Any`，如下所示：
> 
```swift
let optionalNumber: Int? = 3
things.append(optionalNumber)        // 警告
things.append(optionalNumber as Any) // 沒有警告
```
