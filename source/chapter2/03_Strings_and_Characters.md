# 字符串和字符（Strings and Characters）
---

> 1.0
> 翻譯：[wh1100717](https://github.com/wh1100717)
> 校對：[Hawstein](https://github.com/Hawstein)

> 2.0
> 翻譯+校對：[DianQK](https://github.com/DianQK)

> 2.1
> 翻譯：[DianQK](https://github.com/DianQK)
> 校對：[shanks](http://codebuild.me), [Realank](https://github.com/Realank), 

> 2.2
> 校對：[SketchK](https://github.com/SketchK)

> 3.0
> 校對：[CMB](https://github.com/chenmingbiao)，版本日期：2016-09-13   
> 3.0.1, shanks, 2016-11-11

本頁包含內容：

- [字符串字面量](#string_literals)
- [初始化空字符串](#initializing_an_empty_string)
- [字符串可變性](#string_mutability)
- [字符串是值類型](#strings_are_value_types)
- [使用字符](#working_with_characters)
- [連接字符串和字符](#concatenating_strings_and_characters)
- [字符串插值](#string_interpolation)
- [Unicode](#unicode)
- [計算字符數量](#counting_characters)
- [訪問和修改字符串](#accessing_and_modifying_a_string)
- [比較字符串](#comparing_strings)
- [字符串的 Unicode 表示形式](#unicode_representations_of_strings)

*字符串*是例如`"hello, world"`，`"albatross"`這樣的有序的`Character`（字符）類型的值的集合。通過`String`類型來表示。
一個`String`的內容可以用許多方式讀取，包括作為一個`Character`值的集合。 
     
Swift 的`String`和`Character`類型提供了快速和兼容 Unicode 的方式供你的代碼使用。創建和操作字符串的語法與 C 語言中字符串操作相似，輕量並且易讀。
字符串連接操作只需要簡單地通過`+`符號將兩個字符串相連即可。與 Swift 中其他值一樣，能否更改字符串的值，取決於其被定義為常量還是變量。你也可以在字符串內插過程中使用字符串插入常量、變量、字面量表達成更長的字符串，這樣可以很容易的創建自定義的字符串值，進行展示、存儲以及打印。  
   
盡管語法簡易，但`String`類型是一種快速、現代化的字符串實現。
每一個字符串都是由編碼無關的 Unicode 字符組成，並支持訪問字符的多種 Unicode 表示形式（representations）。

> 注意：    
> Swift 的`String`類型與 Foundation `NSString`類進行了無縫橋接。Foundation 也可以對`String`進行擴展，暴露在`NSString`中定義的方法。 這意味著，如果你在`String`中調用這些`NSString`的方法，將不用進行轉換。  
> 更多關於在 Foundation 和 Cocoa 中使用`String`的信息請查看 *[Using Swift with Cocoa and Objective-C (Swift 3.0.1)](https://developer.apple.com/library/content/documentation/Swift/Conceptual/BuildingCocoaApps/WorkingWithCocoaDataTypes.html#//apple_ref/doc/uid/TP40014216-CH6)*。


<a name="string_literals"></a>
## 字符串字面量

您可以在您的代碼中包含一段預定義的字符串值作為字符串字面量。字符串字面量是由雙引號 (`""`) 包裹著的具有固定順序的文本字符集。
字符串字面量可以用於為常量和變量提供初始值：

```swift
let someString = "Some string literal value"
```

注意`someString`常量通過字符串字面量進行初始化，Swift 會推斷該常量為`String`類型。

> 注意：
更多關於在字符串字面量中使用特殊字符的信息，請查看 [字符串字面量的特殊字符](#special_characters_in_string_literals) 。


<a name="initializing_an_empty_string"></a>
## 初始化空字符串

要創建一個空字符串作為初始值，可以將空的字符串字面量賦值給變量，也可以初始化一個新的`String`實例：

```swift
var emptyString = ""               // 空字符串字面量
var anotherEmptyString = String()  // 初始化方法
// 兩個字符串均為空並等價。
```

您可以通過檢查其`Bool`類型的`isEmpty`屬性來判斷該字符串是否為空：

```swift
if emptyString.isEmpty {
    print("Nothing to see here")
}
// 打印輸出："Nothing to see here"
```


<a name="string_mutability"></a>
## 字符串可變性

您可以通過將一個特定字符串分配給一個變量來對其進行修改，或者分配給一個常量來保證其不會被修改：

```swift
var variableString = "Horse"
variableString += " and carriage"
// variableString 現在為 "Horse and carriage"

let constantString = "Highlander"
constantString += " and another Highlander"
// 這會報告一個編譯錯誤 (compile-time error) - 常量字符串不可以被修改。
```

> 注意：   
在 Objective-C 和 Cocoa 中，您需要通過選擇兩個不同的類(`NSString`和`NSMutableString`)來指定字符串是否可以被修改。


<a name="strings_are_value_types"></a>
## 字符串是值類型

Swift 的`String`類型是*值類型*。
如果您創建了一個新的字符串，那麼當其進行常量、變量賦值操作，或在函數/方法中傳遞時，會進行值拷貝。
任何情況下，都會對已有字符串值創建新副本，並對該新副本進行傳遞或賦值操作。
值類型在 [結構體和枚舉是值類型](./09_Classes_and_Structures.html#structures_and_enumerations_are_value_types) 中進行了詳細描述。

Swift 默認字符串拷貝的方式保證了在函數/方法中傳遞的是字符串的值。
很明顯無論該值來自於哪裡，都是您獨自擁有的。
您可以確信傳遞的字符串不會被修改，除非你自己去修改它。

在實際編譯時，Swift 編譯器會優化字符串的使用，使實際的復制只發生在絕對必要的情況下，這意味著您將字符串作為值類型的同時可以獲得極高的性能。


<a name="working_with_characters"></a>
## 使用字符

您可通過`for-in`循環來遍歷字符串中的`characters`屬性來獲取每一個字符的值：

```swift
for character in "Dog!🐶".characters {
    print(character)
}
// D
// o
// g
// !
// 🐶
```

`for-in`循環在 [For 循環](./05_Control_Flow.html#for_loops) 中進行了詳細描述。

另外，通過標明一個`Character`類型並用字符字面量進行賦值，可以建立一個獨立的字符常量或變量：

```swift
let exclamationMark: Character = "!"
```
字符串可以通過傳遞一個值類型為`Character`的數組作為自變量來初始化：

```swift
let catCharacters: [Character] = ["C", "a", "t", "!", "🐱"]
let catString = String(catCharacters)
print(catString)
// 打印輸出："Cat!🐱"
```


<a name="concatenating_strings_and_characters"></a>
## 連接字符串和字符

字符串可以通過加法運算符（`+`）相加在一起（或稱「連接」）創建一個新的字符串：

```swift
let string1 = "hello"
let string2 = " there"
var welcome = string1 + string2
// welcome 現在等於 "hello there"
```

您也可以通過加法賦值運算符 (`+=`) 將一個字符串添加到一個已經存在字符串變量上：

```swift
var instruction = "look over"
instruction += string2
// instruction 現在等於 "look over there"
```

您可以用`append()`方法將一個字符附加到一個字符串變量的尾部：

```swift
let exclamationMark: Character = "!"
welcome.append(exclamationMark)
// welcome 現在等於 "hello there!"
```

> 注意：  
您不能將一個字符串或者字符添加到一個已經存在的字符變量上，因為字符變量只能包含一個字符。


<a name="string_interpolation"></a>
## 字符串插值

*字符串插值*是一種構建新字符串的方式，可以在其中包含常量、變量、字面量和表達式。
您插入的字符串字面量的每一項都在以反斜線為前綴的圓括號中：

```swift
let multiplier = 3
let message = "\(multiplier) times 2.5 is \(Double(multiplier) * 2.5)"
// message 是 "3 times 2.5 is 7.5"
```

在上面的例子中，`multiplier`作為`\(multiplier)`被插入到一個字符串常量量中。
當創建字符串執行插值計算時此占位符會被替換為`multiplier`實際的值。

`multiplier`的值也作為字符串中後面表達式的一部分。
該表達式計算`Double(multiplier) * 2.5`的值並將結果 (`7.5`) 插入到字符串中。
在這個例子中，表達式寫為`\(Double(multiplier) * 2.5)`並包含在字符串字面量中。

> 注意：     
> 插值字符串中寫在括號中的表達式不能包含非轉義反斜杠 (`\`)，並且不能包含回車或換行符。不過，插值字符串可以包含其他字面量。


<a name="unicode"></a>
## Unicode

*Unicode*是一個國際標准，用於文本的編碼和表示。
它使您可以用標准格式表示來自任意語言幾乎所有的字符，並能夠對文本文件或網頁這樣的外部資源中的字符進行讀寫操作。
Swift 的`String`和`Character`類型是完全兼容 Unicode 標准的。


<a name="unicode_scalars"></a>
### Unicode 標量

Swift 的`String`類型是基於 *Unicode 標量* 建立的。
Unicode 標量是對應字符或者修飾符的唯一的21位數字，例如`U+0061`表示小寫的拉丁字母(`LATIN SMALL LETTER A`)("`a`")，`U+1F425`表示小雞表情(`FRONT-FACING BABY CHICK`) ("`🐥`")。

> 注意：
> Unicode *碼位(code poing)* 的范圍是`U+0000`到`U+D7FF`或者`U+E000`到`U+10FFFF`。Unicode 標量不包括 Unicode *代理項(surrogate pair)* 碼位，其碼位范圍是`U+D800`到`U+DFFF`。

注意不是所有的21位 Unicode 標量都代表一個字符，因為有一些標量是留作未來分配的。已經代表一個典型字符的標量都有自己的名字，例如上面例子中的`LATIN SMALL LETTER A`和`FRONT-FACING BABY CHICK`。

<a name="special_characters_in_string_literals"></a>
### 字符串字面量的特殊字符

字符串字面量可以包含以下特殊字符：

* 轉義字符`\0`(空字符)、`\\`(反斜線)、`\t`(水平制表符)、`\n`(換行符)、`\r`(回車符)、`\"`(雙引號)、`\'`(單引號)。
* Unicode 標量，寫成`\u{n}`(u為小寫)，其中`n`為任意一到八位十六進制數且可用的 Unicode 位碼。

下面的代碼為各種特殊字符的使用示例。
`wiseWords`常量包含了兩個雙引號。
`dollarSign`、`blackHeart`和`sparklingHeart`常量演示了三種不同格式的 Unicode 標量：

```swift
let wiseWords = "\"Imagination is more important than knowledge\" - Einstein"
// "Imageination is more important than knowledge" - Enistein
let dollarSign = "\u{24}"             // $, Unicode 標量 U+0024
let blackHeart = "\u{2665}"           // ♥, Unicode 標量 U+2665
let sparklingHeart = "\u{1F496}"      // 💖, Unicode 標量 U+1F496
```

<a name="extended_grapheme_clusters"></a>
### 可擴展的字形群集

每一個 Swift 的`Character`類型代表一個*可擴展的字形群*。
一個可擴展的字形群是一個或多個可生成人類可讀的字符 Unicode 標量的有序排列。   
舉個例子，字母`é`可以用單一的 Unicode 標量`é`(`LATIN SMALL LETTER E WITH ACUTE`, 或者`U+00E9`)來表示。然而一個標准的字母`e`(`LATIN SMALL LETTER E`或者`U+0065`) 加上一個急促重音(`COMBINING ACTUE ACCENT`)的標量(`U+0301`)，這樣一對標量就表示了同樣的字母`é`。
這個急促重音的標量形象的將`e`轉換成了`é`。

在這兩種情況中，字母`é`代表了一個單一的 Swift 的`Character`值，同時代表了一個可擴展的字形群。
在第一種情況，這個字形群包含一個單一標量；而在第二種情況，它是包含兩個標量的字形群：

```swift
let eAcute: Character = "\u{E9}"                         // é
let combinedEAcute: Character = "\u{65}\u{301}"          // e 後面加上  ́
// eAcute 是 é, combinedEAcute 是 é
```

可擴展的字符群集是一個靈活的方法，用許多復雜的腳本字符表示單一的`Character`值。
例如，來自朝鮮語字母表的韓語音節能表示為組合或分解的有序排列。
在 Swift 都會表示為同一個單一的`Character`值：


```swift
let precomposed: Character = "\u{D55C}"                  // 한
let decomposed: Character = "\u{1112}\u{1161}\u{11AB}"   // ᄒ, ᅡ, ᆫ
// precomposed 是 한, decomposed 是 한
```

可拓展的字符群集可以使包圍記號(例如`COMBINING ENCLOSING CIRCLE`或者`U+20DD`)的標量包圍其他 Unicode 標量，作為一個單一的`Character`值：

```swift
let enclosedEAcute: Character = "\u{E9}\u{20DD}"
// enclosedEAcute 是 é⃝
```

地域性指示符號的 Unicode 標量可以組合成一個單一的`Character`值，例如`REGIONAL INDICATOR SYMBOL LETTER U`(`U+1F1FA`)和`REGIONAL INDICATOR SYMBOL LETTER S`(`U+1F1F8`)：


```swift
let regionalIndicatorForUS: Character = "\u{1F1FA}\u{1F1F8}"
// regionalIndicatorForUS 是 🇺🇸
```

<a name="counting_characters"></a>
## 計算字符數量

如果想要獲得一個字符串中`Character`值的數量，可以使用字符串的`characters`屬性的`count`屬性：

```swift
let unusualMenagerie = "Koala 🐨, Snail 🐌, Penguin 🐧, Dromedary 🐪"
print("unusualMenagerie has \(unusualMenagerie.characters.count) characters")
// 打印輸出 "unusualMenagerie has 40 characters"
```

注意在 Swift 中，使用可拓展的字符群集作為`Character`值來連接或改變字符串時，並不一定會更改字符串的字符數量。

例如，如果你用四個字符的單詞`cafe`初始化一個新的字符串，然後添加一個`COMBINING ACTUE ACCENT`(`U+0301`)作為字符串的結尾。最終這個字符串的字符數量仍然是`4`，因為第四個字符是`é`，而不是`e`：

```swift
var word = "cafe"
print("the number of characters in \(word) is \(word.characters.count)")
// 打印輸出 "the number of characters in cafe is 4"

word += "\u{301}"    // COMBINING ACUTE ACCENT, U+0301

print("the number of characters in \(word) is \(word.characters.count)")
// 打印輸出 "the number of characters in café is 4"
```

> 注意：   
> 可擴展的字符群集可以組成一個或者多個 Unicode 標量。這意味著不同的字符以及相同字符的不同表示方式可能需要不同數量的內存空間來存儲。所以 Swift 中的字符在一個字符串中並不一定占用相同的內存空間數量。因此在沒有獲取字符串的可擴展的字符群的范圍時候，就不能計算出字符串的字符數量。如果您正在處理一個長字符串，需要注意`characters`屬性必須遍歷全部的 Unicode 標量，來確定字符串的字符數量。
>
> 另外需要注意的是通過`characters`屬性返回的字符數量並不總是與包含相同字符的`NSString`的`length`屬性相同。`NSString`的`length`屬性是利用 UTF-16 表示的十六位代碼單元數字，而不是 Unicode 可擴展的字符群集。


<a name="accessing_and_modifying_a_string"></a>
## 訪問和修改字符串

你可以通過字符串的屬性和方法來訪問和修改它，當然也可以用下標語法完成。

<a name="string_indices"></a>
### 字符串索引

每一個`String`值都有一個關聯的索引(*index*)類型，`String.Index`，它對應著字符串中的每一個`Character`的位置。

前面提到，不同的字符可能會占用不同數量的內存空間，所以要知道`Character`的確定位置，就必須從`String`開頭遍歷每一個 Unicode 標量直到結尾。因此，Swift 的字符串不能用整數(integer)做索引。

使用`startIndex`屬性可以獲取一個`String`的第一個`Character`的索引。使用`endIndex`屬性可以獲取最後一個`Character`的後一個位置的索引。因此，`endIndex`屬性不能作為一個字符串的有效下標。如果`String`是空串，`startIndex`和`endIndex`是相等的。

通過調用 `String` 的 `index(before:)` 或 `index(after:)` 方法，可以立即得到前面或後面的一個索引。您還可以通過調用 `index(_:offsetBy:)` 方法來獲取對應偏移量的索引，這種方式可以避免多次調用 `index(before:)` 或 `index(after:)` 方法。

你可以使用下標語法來訪問 `String` 特定索引的 `Character`。

```swift
let greeting = "Guten Tag!"
greeting[greeting.startIndex]
// G
greeting[greeting.index(before: greeting.endIndex)]
// !
greeting[greeting.index(after: greeting.startIndex)]
// u
let index = greeting.index(greeting.startIndex, offsetBy: 7)
greeting[index]
// a
```

試圖獲取越界索引對應的 `Character`，將引發一個運行時錯誤。

```swift
greeting[greeting.endIndex] // error
greeting.index(after: endIndex) // error
```

使用 `characters` 屬性的 `indices` 屬性會創建一個包含全部索引的范圍(`Range`)，用來在一個字符串中訪問單個字符。

```swift
for index in greeting.characters.indices {
   print("\(greeting[index]) ", terminator: "")
}
// 打印輸出 "G u t e n   T a g ! "
```

> 注意：  
> 您可以使用 `startIndex` 和 `endIndex` 屬性或者 `index(before:)` 、`index(after:)` 和 `index(_:offsetBy:)` 方法在任意一個確認的並遵循 `Collection` 協議的類型裡面，如上文所示是使用在 `String` 中，您也可以使用在 `Array`、`Dictionary` 和 `Set`中。 

<a name="inserting_and_removing"></a>
### 插入和刪除

調用 `insert(_:at:)` 方法可以在一個字符串的指定索引插入一個字符，調用 `insert(contentsOf:at:)` 方法可以在一個字符串的指定索引插入一個段字符串。

```swift
var welcome = "hello"
welcome.insert("!", at: welcome.endIndex)
// welcome 變量現在等於 "hello!"
 
welcome.insert(contentsOf:" there".characters, at: welcome.index(before: welcome.endIndex))
// welcome 變量現在等於 "hello there!"
```

調用 `remove(at:)` 方法可以在一個字符串的指定索引刪除一個字符，調用 `removeSubrange(_:)` 方法可以在一個字符串的指定索引刪除一個子字符串。

```swift
welcome.remove(at: welcome.index(before: welcome.endIndex))
// welcome 現在等於 "hello there"
 
let range = welcome.index(welcome.endIndex, offsetBy: -6)..<welcome.endIndex
welcome.removeSubrange(range)
// welcome 現在等於 "hello"
```

> 注意：
> 您可以使用 `insert(_:at:)`、`insert(contentsOf:at:)`、`remove(at:)` 和 `removeSubrange(_:)` 方法在任意一個確認的並遵循 `RangeReplaceableCollection` 協議的類型裡面，如上文所示是使用在 `String` 中，您也可以使用在 `Array`、`Dictionary` 和 `Set` 中。 


<a name="comparing_strings"></a>
## 比較字符串

Swift 提供了三種方式來比較文本值：字符串字符相等、前綴相等和後綴相等。

<a name="string_and_character_equality"></a>
### 字符串/字符相等

字符串/字符可以用等於操作符(`==`)和不等於操作符(`!=`)，詳細描述在[比較運算符](./02_Basic_Operators.html#comparison_operators)：

```swift
let quotation = "We're a lot alike, you and I."
let sameQuotation = "We're a lot alike, you and I."
if quotation == sameQuotation {
    print("These two strings are considered equal")
}
// 打印輸出 "These two strings are considered equal"
```

如果兩個字符串（或者兩個字符）的可擴展的字形群集是標准相等的，那就認為它們是相等的。在這個情況下，即使可擴展的字形群集是有不同的 Unicode 標量構成的，只要它們有同樣的語言意義和外觀，就認為它們標准相等。

例如，`LATIN SMALL LETTER E WITH ACUTE`(`U+00E9`)就是標准相等於`LATIN SMALL LETTER E`(`U+0065`)後面加上`COMBINING ACUTE ACCENT`(`U+0301`)。這兩個字符群集都是表示字符`é`的有效方式，所以它們被認為是標准相等的：

```swift
// "Voulez-vous un café?" 使用 LATIN SMALL LETTER E WITH ACUTE
let eAcuteQuestion = "Voulez-vous un caf\u{E9}?"

// "Voulez-vous un café?" 使用 LATIN SMALL LETTER E and COMBINING ACUTE ACCENT
let combinedEAcuteQuestion = "Voulez-vous un caf\u{65}\u{301}?"

if eAcuteQuestion == combinedEAcuteQuestion {
    print("These two strings are considered equal")
}
// 打印輸出 "These two strings are considered equal"
```

相反，英語中的`LATIN CAPITAL LETTER A`(`U+0041`，或者`A`)不等於俄語中的`CYRILLIC CAPITAL LETTER A`(`U+0410`，或者`A`)。兩個字符看著是一樣的，但卻有不同的語言意義：

```swift
let latinCapitalLetterA: Character = "\u{41}"

let cyrillicCapitalLetterA: Character = "\u{0410}"

if latinCapitalLetterA != cyrillicCapitalLetterA {
    print("These two characters are not equivalent")
}
// 打印 "These two characters are not equivalent"
```

> 注意：   
> 在 Swift 中，字符串和字符並不區分地域(not locale-sensitive)。


<a name="prefix_and_suffix_equality"></a>
### 前綴/後綴相等

通過調用字符串的`hasPrefix(_:)`/`hasSuffix(_:)`方法來檢查字符串是否擁有特定前綴/後綴，兩個方法均接收一個`String`類型的參數，並返回一個布爾值。

下面的例子以一個字符串數組表示莎士比亞話劇《羅密歐與朱麗葉》中前兩場的場景位置：

```swift
let romeoAndJuliet = [
    "Act 1 Scene 1: Verona, A public place",
    "Act 1 Scene 2: Capulet's mansion",
    "Act 1 Scene 3: A room in Capulet's mansion",
    "Act 1 Scene 4: A street outside Capulet's mansion",
    "Act 1 Scene 5: The Great Hall in Capulet's mansion",
    "Act 2 Scene 1: Outside Capulet's mansion",
    "Act 2 Scene 2: Capulet's orchard",
    "Act 2 Scene 3: Outside Friar Lawrence's cell",
    "Act 2 Scene 4: A street in Verona",
    "Act 2 Scene 5: Capulet's mansion",
    "Act 2 Scene 6: Friar Lawrence's cell"
]
```

您可以調用`hasPrefix(_:)`方法來計算話劇中第一幕的場景數：

```swift
var act1SceneCount = 0
for scene in romeoAndJuliet {
    if scene.hasPrefix("Act 1 ") {
        act1SceneCount += 1
    }
}
print("There are \(act1SceneCount) scenes in Act 1")
// 打印輸出 "There are 5 scenes in Act 1"
```

相似地，您可以用`hasSuffix(_:)`方法來計算發生在不同地方的場景數：

```swift
var mansionCount = 0
var cellCount = 0
for scene in romeoAndJuliet {
    if scene.hasSuffix("Capulet's mansion") {
        mansionCount += 1
    } else if scene.hasSuffix("Friar Lawrence's cell") {
        cellCount += 1
    }
}
print("\(mansionCount) mansion scenes; \(cellCount) cell scenes")
// 打印輸出 "6 mansion scenes; 2 cell scenes"
```

> 注意：   
> `hasPrefix(_:)`和`hasSuffix(_:)`方法都是在每個字符串中逐字符比較其可擴展的字符群集是否標准相等，詳細描述在[字符串/字符相等](#string_and_character_equality)。


<a name="unicode_representations_of_strings"></a>
## 字符串的 Unicode 表示形式

當一個 Unicode 字符串被寫進文本文件或者其他儲存時，字符串中的 Unicode 標量會用 Unicode 定義的幾種`編碼格式`（encoding forms）編碼。每一個字符串中的小塊編碼都被稱`代碼單元`（code units）。這些包括 UTF-8 編碼格式（編碼字符串為8位的代碼單元）， UTF-16 編碼格式（編碼字符串位16位的代碼單元），以及 UTF-32 編碼格式（編碼字符串32位的代碼單元）。

Swift 提供了幾種不同的方式來訪問字符串的 Unicode 表示形式。
您可以利用`for-in`來對字符串進行遍歷，從而以 Unicode 可擴展的字符群集的方式訪問每一個`Character`值。
該過程在 [使用字符](#working_with_characters) 中進行了描述。

另外，能夠以其他三種 Unicode 兼容的方式訪問字符串的值：

* UTF-8 代碼單元集合 (利用字符串的`utf8`屬性進行訪問)
* UTF-16 代碼單元集合 (利用字符串的`utf16`屬性進行訪問)
* 21位的 Unicode 標量值集合，也就是字符串的 UTF-32 編碼格式 (利用字符串的`unicodeScalars`屬性進行訪問)

下面由`D`,`o`,`g`,`‼`(`DOUBLE EXCLAMATION MARK`, Unicode 標量 `U+203C`)和`🐶`(`DOG FACE`，Unicode 標量為`U+1F436`)組成的字符串中的每一個字符代表著一種不同的表示：

```swift
let dogString = "Dog‼🐶"
```

<a name="UTF-8_representation"></a>
### UTF-8 表示

您可以通過遍歷`String`的`utf8`屬性來訪問它的`UTF-8`表示。
其為`String.UTF8View`類型的屬性，`UTF8View`是無符號8位 (`UInt8`) 值的集合，每一個`UInt8`值都是一個字符的 UTF-8 表示：

<table style='text-align:center'>
 <tr height="77">
  <td>Character</td>
  <td>D<br>U+0044</td>
  <td>o<br>U+006F</td>
  <td>g<br>U+0067</td>
  <td colspan="3">‼<br>U+203C</td>
  <td colspan="4">🐶<br>U+1F436</td>
 </tr>
 <tr height="77">
  <td height="77">UTF-8<br>Code Unit</td>
  <td>68</td>
  <td>111</td>
  <td>103</td>
  <td>226</td>
  <td>128</td>
  <td>188</td>
  <td>240</td>
  <td>159</td>
  <td>144</td>
  <td>182</td>
 </tr>
 <tr>
  <td height="77">Position</td>
  <td>0</td>
  <td>1</td>
  <td>2</td>
  <td>3</td>
  <td>4</td>
  <td>5</td>
  <td>6</td>
  <td>7</td>
  <td>8</td>
  <td>9</td>
 </tr>
</table>


```swift
for codeUnit in dogString.utf8 {
    print("\(codeUnit) ", terminator: "")
}
print("")
// 68 111 103 226 128 188 240 159 144 182
```

上面的例子中，前三個10進制`codeUnit`值 (`68`, `111`, `103`) 代表了字符`D`、`o`和 `g`，它們的 UTF-8 表示與 ASCII 表示相同。
接下來的三個10進制`codeUnit`值 (`226`, `128`, `188`) 是`DOUBLE EXCLAMATION MARK`的3字節 UTF-8 表示。
最後的四個`codeUnit`值 (`240`, `159`, `144`, `182`) 是`DOG FACE`的4字節 UTF-8 表示。


<a name="UTF-16_representation"></a>
### UTF-16 表示

您可以通過遍歷`String`的`utf16`屬性來訪問它的`UTF-16`表示。
其為`String.UTF16View`類型的屬性，`UTF16View`是無符號16位 (`UInt16`) 值的集合，每一個`UInt16`都是一個字符的 UTF-16 表示：

<table style='text-align:center'>
 <tr height="77">
  <td>Character</td>
  <td>D<br>U+0044</td>
  <td>o<br>U+006F</td>
  <td>g<br>U+0067</td>
  <td>‼<br>U+203C</td>
  <td colspan="2">🐶<br>U+1F436</td>
 </tr>
 <tr height="77">
  <td height="77">UTF-16<br>Code Unit</td>
  <td>68</td>
  <td>111</td>
  <td>103</td>
  <td>8252</td>
  <td>55357</td>
  <td>56374</td>
 </tr>
 <tr>
  <td height="77">Position</td>
  <td>0</td>
  <td>1</td>
  <td>2</td>
  <td>3</td>
  <td>4</td>
  <td>5</td>
 </tr>
</table>


```swift
for codeUnit in dogString.utf16 {
    print("\(codeUnit) ", terminator: "")
}
print("")
// 68 111 103 8252 55357 56374
```

同樣，前三個`codeUnit`值 (`68`, `111`, `103`) 代表了字符`D`、`o`和`g`，它們的 UTF-16 代碼單元和 UTF-8 完全相同（因為這些 Unicode 標量表示 ASCII 字符）。

第四個`codeUnit`值 (`8252`) 是一個等於十六進制`203C`的的十進制值。這個代表了`DOUBLE EXCLAMATION MARK`字符的 Unicode 標量值`U+203C`。這個字符在 UTF-16 中可以用一個代碼單元表示。

第五和第六個`codeUnit`值 (`55357`和`56374`) 是`DOG FACE`字符的 UTF-16 表示。
第一個值為`U+D83D`(十進制值為`55357`)，第二個值為`U+DC36`(十進制值為`56374`)。

<a name="unicode_scalars_representation"></a>
### Unicode 標量表示

您可以通過遍歷`String`值的`unicodeScalars`屬性來訪問它的 Unicode 標量表示。
其為`UnicodeScalarView`類型的屬性，`UnicodeScalarView`是`UnicodeScalar`類型的值的集合。
`UnicodeScalar`是21位的 Unicode 代碼點。

每一個`UnicodeScalar`擁有一個`value`屬性，可以返回對應的21位數值，用`UInt32`來表示：


<table style='text-align:center'>
 <tr height="77">
  <td>Character</td>
  <td>D<br>U+0044</td>
  <td>o<br>U+006F</td>
  <td>g<br>U+0067</td>
  <td>‼<br>U+203C</td>
  <td>🐶<br>U+1F436</td>
 </tr>
 <tr height="77">
  <td height="77">Unicode Scalar<br>Code Unit</td>
  <td>68</td>
  <td>111</td>
  <td>103</td>
  <td>8252</td>
  <td>128054</td>
 </tr>
 <tr>
  <td height="77">Position</td>
  <td>0</td>
  <td>1</td>
  <td>2</td>
  <td>3</td>
  <td>4</td>
 </tr>
</table>


```swift
for scalar in dogString.unicodeScalars {
    print("\(scalar.value) ", terminator: "")
}
print("")
// 68 111 103 8252 128054
```

前三個`UnicodeScalar`值(`68`, `111`, `103`)的`value`屬性仍然代表字符`D`、`o`和`g`。
第四個`codeUnit`值(`8252`)仍然是一個等於十六進制`203C`的十進制值。這個代表了`DOUBLE EXCLAMATION MARK`字符的 Unicode 標量`U+203C`。

第五個`UnicodeScalar`值的`value`屬性，`128054`，是一個十六進制`1F436`的十進制表示。其等同於`DOG FACE`的 Unicode 標量`U+1F436`。

作為查詢它們的`value`屬性的一種替代方法，每個`UnicodeScalar`值也可以用來構建一個新的`String`值，比如在字符串插值中使用：

```swift
for scalar in dogString.unicodeScalars {
    print("\(scalar) ")
}
// D
// o
// g
// ‼
// 🐶
```
