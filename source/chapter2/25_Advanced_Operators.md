# 高級運算符
-----------------

> 1.0
> 翻譯：[xielingwang](https://github.com/xielingwang)
> 校對：[numbbbbb](https://github.com/numbbbbb)

> 2.0
> 翻譯+校對：[buginux](https://github.com/buginux)

> 2.1
> 校對：[shanks](http://codebuild.me)，2015-11-01
> 
> 2.2 
> 翻譯+校對：[SketchK](https://github.com/SketchK) 2016-05-17
> 
> 3.0
> 翻譯+校對：[mmoaay](https://github.com/mmoaay) 2016-09-20   
> 3.0.1：shanks，2016-11-13

本頁內容包括：

- [位運算符](#bitwise_operators)
- [溢出運算符](#overflow_operators)
- [優先級和結合性](#precedence_and_associativity)
- [運算符函數](#operator_functions)
- [自定義運算符](#custom_operators)

除了在之前介紹過的[基本運算符](./02_Basic_Operators.html)，Swift 中還有許多可以對數值進行復雜運算的高級運算符。這些高級運算符包含了在 C 和 Objective-C 中已經被大家所熟知的位運算符和移位運算符。

與 C 語言中的算術運算符不同，Swift 中的算術運算符默認是不會溢出的。所有溢出行為都會被捕獲並報告為錯誤。如果想讓系統允許溢出行為，可以選擇使用 Swift 中另一套默認支持溢出的運算符，比如溢出加法運算符（`&+`）。所有的這些溢出運算符都是以 `&` 開頭的。

自定義結構體、類和枚舉時，如果也為它們提供標准 Swift 運算符的實現，將會非常有用。在 Swift 中自定義運算符非常簡單，運算符也會針對不同類型使用對應實現。

我們不用被預定義的運算符所限制。在 Swift 中可以自由地定義中綴、前綴、後綴和賦值運算符，以及相應的優先級與結合性。這些運算符在代碼中可以像預定義的運算符一樣使用，我們甚至可以擴展已有的類型以支持自定義的運算符。

<a name="bitwise_operators"></a>
## 位運算符

*位運算符*可以操作數據結構中每個獨立的比特位。它們通常被用在底層開發中，比如圖形編程和創建設備驅動。位運算符在處理外部資源的原始數據時也十分有用，比如對自定義通信協議傳輸的數據進行編碼和解碼。

Swift 支持 C 語言中的全部位運算符，接下來會一一介紹。

<a name="bitwise_not_operator"></a>
### 按位取反運算符

*按位取反運算符（`~`）*可以對一個數值的全部比特位進行取反：


![Art/bitwiseNOT_2x.png](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/bitwiseNOT_2x.png)

按位取反運算符是一個前綴運算符，需要直接放在運算的數之前，並且它們之間不能添加任何空格：

```swift
let initialBits: UInt8 = 0b00001111
let invertedBits = ~initialBits // 等於 0b11110000
```

`UInt8` 類型的整數有 8 個比特位，可以存儲 `0 ~ 255` 之間的任意整數。這個例子初始化了一個 `UInt8` 類型的整數，並賦值為二進制的 `00001111`，它的前 4 位都為 `0`，後 4 位都為 `1`。這個值等價於十進制的 `15`。

接著使用按位取反運算符創建了一個名為 `invertedBits` 的常量，這個常量的值與全部位取反後的 `initialBits` 相等。即所有的 `0` 都變成了 `1`，同時所有的 `1` 都變成 `0`。`invertedBits` 的二進制值為 `11110000`，等價於無符號十進制數的 `240`。

<a name="bitwise_and_operator"></a>
### 按位與運算符

*按位與運算符（`&`）*可以對兩個數的比特位進行合並。它返回一個新的數，只有當兩個數的對應位都為 `1` 的時候，新數的對應位才為 `1`：

![Art/bitwiseAND_2x.png](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/bitwiseAND_2x.png)

在下面的示例當中，`firstSixBits` 和 `lastSixBits` 中間 4 個位的值都為 `1`。按位與運算符對它們進行了運算，得到二進制數值 `00111100`，等價於無符號十進制數的 `60`：

```swift
let firstSixBits: UInt8 = 0b11111100
let lastSixBits: UInt8  = 0b00111111
let middleFourBits = firstSixBits & lastSixBits // 等於 00111100
```

<a name="bitwise_or_operator"></a>
### 按位或運算符

*按位或運算符（`|`）*可以對兩個數的比特位進行比較。它返回一個新的數，只要兩個數的對應位中有任意一個為 `1` 時，新數的對應位就為 `1`：

![Art/bitwiseOR_2x.png](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/bitwiseOR_2x.png "Art/bitwiseOR_2x.png")

在下面的示例中，`someBits` 和 `moreBits` 不同的位會被設置為 `1`。接位或運算符對它們進行了運算，得到二進制數值 `11111110`，等價於無符號十進制數的 `254`：

```swift
let someBits: UInt8 = 0b10110010
let moreBits: UInt8 = 0b01011110
let combinedbits = someBits | moreBits // 等於 11111110
```

<a name="bitwise_xor_operator"></a>
### 按位異或運算符

*按位異或運算符（`^`）*可以對兩個數的比特位進行比較。它返回一個新的數，當兩個數的對應位不相同時，新數的對應位就為 `1`：

![Art/bitwiseXOR_2x.png](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/bitwiseXOR_2x.png "Art/bitwiseXOR_2x.png")

在下面的示例當中，`firstBits` 和 `otherBits` 都有一個自己的位為 `1` 而對方的對應位為 `0` 的位。 按位異或運算符將新數的這兩個位都設置為 `1`，同時將其它位都設置為 `0`：

```swift
let firstBits: UInt8 = 0b00010100
let otherBits: UInt8 = 0b00000101
let outputBits = firstBits ^ otherBits // 等於 00010001
```

<a name="bitwise_left_and_right_shift_operators"></a>
### 按位左移、右移運算符

*按位左移運算符（`<<`）*和*按位右移運算符（`>>`）*可以對一個數的所有位進行指定位數的左移和右移，但是需要遵守下面定義的規則。

對一個數進行按位左移或按位右移，相當於對這個數進行乘以 2 或除以 2 的運算。將一個整數左移一位，等價於將這個數乘以 2，同樣地，將一個整數右移一位，等價於將這個數除以 2。

<a name="shifting_behavior_for_unsigned_integers"></a>
#### 無符號整數的移位運算

對無符號整數進行移位的規則如下：

1. 已經存在的位按指定的位數進行左移和右移。
2. 任何因移動而超出整型存儲范圍的位都會被丟棄。
3. 用 `0` 來填充移位後產生的空白位。

這種方法稱為*邏輯移位*。

以下這張圖展示了 `11111111 << 1`（即把 `11111111` 向左移動 `1` 位），和 `11111111 >> 1`（即把 `11111111` 向右移動 `1` 位）的結果。藍色的部分是被移位的，灰色的部分是被拋棄的，橙色的部分則是被填充進來的：

![Art/bitshiftUnsigned_2x.png](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/bitshiftUnsigned_2x.png "Art/bitshiftUnsigned_2x.png")

下面的代碼演示了 Swift 中的移位運算：

```swift
let shiftBits: UInt8 = 4 // 即二進制的 00000100
shiftBits << 1           // 00001000
shiftBits << 2           // 00010000
shiftBits << 5           // 10000000
shiftBits << 6           // 00000000
shiftBits >> 2           // 00000001
```

可以使用移位運算對其他的數據類型進行編碼和解碼：

```swift
let pink: UInt32 = 0xCC6699
let redComponent = (pink & 0xFF0000) >> 16  // redComponent 是 0xCC，即 204
let greenComponent = (pink & 0x00FF00) >> 8 // greenComponent 是 0x66， 即 102
let blueComponent = pink & 0x0000FF         // blueComponent 是 0x99，即 153
```

這個示例使用了一個命名為 `pink` 的 `UInt32` 型常量來存儲 CSS 中粉色的顏色值。該 CSS 的十六進制顏色值 `#CC6699`，在 Swift 中表示為 `0xCC6699`。然後利用按位與運算符（`&`）和按位右移運算符（`>>`）從這個顏色值中分解出紅（`CC`）、綠（`66`）以及藍（`99`）三個部分。

紅色部分是通過對 `0xCC6699` 和 `0xFF0000` 進行按位與運算後得到的。`0xFF0000` 中的 `0` 部分「掩蓋」了 `OxCC6699` 中的第二、第三個字節，使得數值中的 `6699` 被忽略，只留下 `0xCC0000`。

然後，再將這個數按向右移動 16 位（`>> 16`）。十六進制中每兩個字符表示 8 個比特位，所以移動 16 位後 `0xCC0000` 就變為 `0x0000CC`。這個數和`0xCC`是等同的，也就是十進制數值的 `204`。

同樣的，綠色部分通過對 `0xCC6699` 和 `0x00FF00` 進行按位與運算得到 `0x006600`。然後將這個數向右移動 8 位，得到 `0x66`，也就是十進制數值的 `102`。

最後，藍色部分通過對 `0xCC6699` 和 `0x0000FF` 進行按位與運算得到 `0x000099`。這裡不需要再向右移位，所以結果為 `0x99` ，也就是十進制數值的 `153`。

<a name="shifting_behavior_for_signed_integers"></a>
#### 有符號整數的移位運算

對比無符號整數，有符號整數的移位運算相對復雜得多，這種復雜性源於有符號整數的二進制表現形式。（為了簡單起見，以下的示例都是基於 8 比特位的有符號整數的，但是其中的原理對任何位數的有符號整數都是通用的。）

有符號整數使用第 1 個比特位（通常被稱為符號位）來表示這個數的正負。符號位為 `0` 代表正數，為 `1` 代表負數。

其余的比特位（通常被稱為數值位）存儲了實際的值。有符號正整數和無符號數的存儲方式是一樣的，都是從 `0` 開始算起。這是值為 `4` 的 `Int8` 型整數的二進制位表現形式：

![Art/bitshiftSignedFour_2x.png](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/bitshiftSignedFour_2x.png "Art/bitshiftSignedFour_2x.png")

符號位為 `0`，說明這是一個正數，另外 7 位則代表了十進制數值 `4` 的二進制表示。

負數的存儲方式略有不同。它存儲的值的絕對值等於 `2` 的 `n` 次方減去它的實際值（也就是數值位表示的值），這裡的 `n` 為數值位的比特位數。一個 8 比特位的數有 7 個比特位是數值位，所以是 `2` 的 `7` 次方，即 `128`。

這是值為 `-4` 的 `Int8` 型整數的二進制位表現形式：

![Art/bitshiftSignedMinusFour_2x.png](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/bitshiftSignedMinusFour_2x.png "Art/bitshiftSignedMinusFour_2x.png")

這次的符號位為 `1`，說明這是一個負數，另外 7 個位則代表了數值 `124`（即 `128 - 4`）的二進制表示：

![Art/bitshiftSignedMinusFourValue_2x.png](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/bitshiftSignedMinusFourValue_2x.png "Art/bitshiftSignedMinusFourValue_2x.png")

負數的表示通常被稱為*二進制補碼*表示。用這種方法來表示負數乍看起來有點奇怪，但它有幾個優點。

首先，如果想對 `-1` 和 `-4` 進行加法運算，我們只需要將這兩個數的全部 8 個比特位進行相加，並且將計算結果中超出 8 位的數值丟棄：

![Art/bitshiftSignedAddition_2x.png](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/bitshiftSignedAddition_2x.png "Art/bitshiftSignedAddition_2x.png")

其次，使用二進制補碼可以使負數的按位左移和右移運算得到跟正數同樣的效果，即每向左移一位就將自身的數值乘以 2，每向右一位就將自身的數值除以 2。要達到此目的，對有符號整數的右移有一個額外的規則：

* 當對整數進行按位右移運算時，遵循與無符號整數相同的規則，但是對於移位產生的空白位使用*符號位*進行填充，而不是用 `0`。

![Art/bitshiftSigned_2x.png](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/bitshiftSigned_2x.png "Art/bitshiftSigned_2x.png")

這個行為可以確保有符號整數的符號位不會因為右移運算而改變，這通常被稱為*算術移位*。

由於正數和負數的特殊存儲方式，在對它們進行右移的時候，會使它們越來越接近 `0`。在移位的過程中保持符號位不變，意味著負整數在接近 `0` 的過程中會一直保持為負。

<a name="overflow_operators"></a>
## 溢出運算符

在默認情況下，當向一個整數賦予超過它容量的值時，Swift 默認會報錯，而不是生成一個無效的數。這個行為為我們在運算過大或著過小的數的時候提供了額外的安全性。

例如，`Int16` 型整數能容納的有符號整數范圍是 `-32768` 到 `32767`，當為一個 `Int16` 型變量賦的值超過這個范圍時，系統就會報錯：

```swift
var potentialOverflow = Int16.max
// potentialOverflow 的值是 32767，這是 Int16 能容納的最大整數
potentialOverflow += 1
// 這裡會報錯
```

為過大或者過小的數值提供錯誤處理，能讓我們在處理邊界值時更加靈活。

然而，也可以選擇讓系統在數值溢出的時候采取截斷處理，而非報錯。可以使用 Swift 提供的三個溢出運算符來讓系統支持整數溢出運算。這些運算符都是以 `&` 開頭的：

* 溢出加法 `&+`
* 溢出減法 `&-`
* 溢出乘法 `&*`

<a name="value_overflow"></a>
### 數值溢出

數值有可能出現上溢或者下溢。

這個示例演示了當我們對一個無符號整數使用溢出加法（`&+`）進行上溢運算時會發生什麼：

```swift
var unsignedOverflow = UInt8.max
// unsignedOverflow 等於 UInt8 所能容納的最大整數 255
unsignedOverflow = unsignedOverflow &+ 1
// 此時 unsignedOverflow 等於 0
```

`unsignedOverflow` 被初始化為 `UInt8` 所能容納的最大整數（`255`，以二進制表示即 `11111111`）。然後使用了溢出加法運算符（`&+`）對其進行加 `1` 運算。這使得它的二進制表示正好超出 `UInt8` 所能容納的位數，也就導致了數值的溢出，如下圖所示。數值溢出後，留在 `UInt8` 邊界內的值是 `00000000`，也就是十進制數值的 `0`。

![Art/overflowAddition_2x.png](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/overflowAddition_2x.png "Art/overflowAddition_2x.png")

同樣地，當我們對一個無符號整數使用溢出減法（`&-`）進行下溢運算時也會產生類似的現象：

```swift
var unsignedOverflow = UInt8.min
// unsignedOverflow 等於 UInt8 所能容納的最小整數 0
unsignedOverflow = unsignedOverflow &- 1
// 此時 unsignedOverflow 等於 255
```

`UInt8` 型整數能容納的最小值是 `0`，以二進制表示即 `00000000`。當使用溢出減法運算符對其進行減 `1` 運算時，數值會產生下溢並被截斷為 `11111111`， 也就是十進制數值的 `255`。

![Art/overflowUnsignedSubtraction_2x.png](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/overflowUnsignedSubtraction_2x.png "Art/overflowAddition_2x.png")

溢出也會發生在有符號整型數值上。在對有符號整型數值進行溢出加法或溢出減法運算時，符號位也需要參與計算，正如[按位左移、右移運算符](#bitwise_left_and_right_shift_operators)所描述的。

```swift
var signedOverflow = Int8.min
// signedOverflow 等於 Int8 所能容納的最小整數 -128
signedOverflow = signedOverflow &- 1
// 此時 signedOverflow 等於 127
```

`Int8` 型整數能容納的最小值是 `-128`，以二進制表示即 `10000000`。當使用溢出減法運算符對其進行減 `1` 運算時，符號位被翻轉，得到二進制數值 `01111111`，也就是十進制數值的 `127`，這個值也是 `Int8` 型整數所能容納的最大值。

![Art/overflowSignedSubtraction_2x.png](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/overflowSignedSubtraction_2x.png "Art/overflowSignedSubtraction_2x.png")

對於無符號與有符號整型數值來說，當出現上溢時，它們會從數值所能容納的最大數變成最小的數。同樣地，當發生下溢時，它們會從所能容納的最小數變成最大的數。

<a name="precedence_and_associativity"></a>
## 優先級和結合性

運算符的*優先級*使得一些運算符優先於其他運算符，高優先級的運算符會先被計算。

*結合性*定義了相同優先級的運算符是如何結合的，也就是說，是與左邊結合為一組，還是與右邊結合為一組。可以將這意思理解為「它們是與左邊的表達式結合的」或者「它們是與右邊的表達式結合的」。

在復合表達式的運算順序中，運算符的優先級和結合性是非常重要的。舉例來說，運算符優先級解釋了為什麼下面這個表達式的運算結果會是 `17`。

```swift
2 + 3 % 4 * 5
// 結果是 17
```

如果完全從左到右進行運算，則運算的過程是這樣的：

- 2 + 3 = 5
- 5 % 4 = 1
- 1 * 5 = 5

但是正確答案是 `17` 而不是 `5`。優先級高的運算符要先於優先級低的運算符進行計算。與 C 語言類似，在 Swift 中，乘法運算符（`*`）與取余運算符（`%`）的優先級高於加法運算符（`+`）。因此，它們的計算順序要先於加法運算。

而乘法與取余的優先級相同。這時為了得到正確的運算順序，還需要考慮結合性。乘法與取余運算都是左結合的。可以將這考慮成為這兩部分表達式都隱式地加上了括號：

```swift
2 + ((3 % 4) * 5)
```

`(3 % 4)` 等於 `3`，所以表達式相當於：

```swift
2 + (3 * 5)
```

`3 * 5` 等於 `15`，所以表達式相當於：

```swift
2 + 15
```

因此計算結果為 `17`。

如果想查看完整的 Swift 運算符優先級和結合性規則，請參考[表達式](../chapter3/04_Expressions.html)。如果想查看 Swift 標准庫提供所有的運算符，請查看 [Swift Standard Library Operators Reference](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Reference/Swift_StandardLibrary_Operators/index.html#//apple_ref/doc/uid/TP40016054)。

> 注意  
> 相對 C 語言和 Objective-C 來說，Swift 的運算符優先級和結合性規則更加簡潔和可預測。但是，這也意味著它們相較於 C 語言及其衍生語言並不是完全一致的。在對現有的代碼進行移植的時候，要注意確保運算符的行為仍然符合你的預期。

<a name="operator_functions"></a>
## 運算符函數

類和結構體可以為現有的運算符提供自定義的實現，這通常被稱為*運算符重載*。

下面的例子展示了如何為自定義的結構體實現加法運算符（`+`）。算術加法運算符是一個雙目運算符，因為它可以對兩個值進行運算，同時它還是中綴運算符，因為它出現在兩個值中間。

例子中定義了一個名為 `Vector2D` 的結構體用來表示二維坐標向量 `(x, y)`，緊接著定義了一個可以對兩個 `Vector2D` 結構體進行相加的運算符函數：

```swift
struct Vector2D {
    var x = 0.0, y = 0.0
}
 
extension Vector2D {
    static func + (left: Vector2D, right: Vector2D) -> Vector2D {
        return Vector2D(x: left.x + right.x, y: left.y + right.y)
    }
}
```

該運算符函數被定義為 `Vector2D` 上的一個類方法，並且函數的名字與它要進行重載的 `+` 名字一致。因為加法運算並不是一個向量必需的功能，所以這個類方法被定義在 `Vector2D` 的一個擴展中，而不是 `Vector2D` 結構體聲明內。而算術加法運算符是雙目運算符，所以這個運算符函數接收兩個類型為 `Vector2D` 的參數，同時有一個 `Vector2D` 類型的返回值。

在這個實現中，輸入參數分別被命名為 `left` 和 `right`，代表在 `+` 運算符左邊和右邊的兩個 `Vector2D` 實例。函數返回了一個新的 `Vector2D` 實例，這個實例的 `x` 和 `y` 分別等於作為參數的兩個實例的 `x` 和 `y` 的值之和。

這個類方法可以在任意兩個 `Vector2D` 實例中間作為中綴運算符來使用：

```swift
let vector = Vector2D(x: 3.0, y: 1.0)
let anotherVector = Vector2D(x: 2.0, y: 4.0)
let combinedVector = vector + anotherVector
// combinedVector 是一個新的 Vector2D 實例，值為 (5.0, 5.0)
```

這個例子實現兩個向量 `(3.0，1.0)` 和 `(2.0，4.0)` 的相加，並得到新的向量 `(5.0，5.0)`。這個過程如下圖示：

![Art/vectorAddition_2x.png](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/vectorAddition_2x.png "Art/vectorAddition_2x.png")

<a name="prefix_and_postfix_operators"></a>
### 前綴和後綴運算符

上個例子演示了一個雙目中綴運算符的自定義實現。類與結構體也能提供標准*單目運算符*的實現。單目運算符只運算一個值。當運算符出現在值之前時，它就是*前綴*的（例如 `-a`），而當它出現在值之後時，它就是*後綴*的（例如 `b!`）。

要實現前綴或者後綴運算符，需要在聲明運算符函數的時候在 `func` 關鍵字之前指定 `prefix` 或者 `postfix` 修飾符：

```swift
extension Vector2D {
    static prefix func - (vector: Vector2D) -> Vector2D {
        return Vector2D(x: -vector.x, y: -vector.y)
    }
}
```

這段代碼為 `Vector2D` 類型實現了單目負號運算符。由於該運算符是前綴運算符，所以這個函數需要加上 `prefix` 修飾符。

對於簡單數值，單目負號運算符可以對它們的正負性進行改變。對於 `Vector2D` 來說，該運算將其 `x` 和 `y` 屬性的正負性都進行了改變：

```swift
let positive = Vector2D(x: 3.0, y: 4.0)
let negative = -positive
// negative 是一個值為 (-3.0, -4.0) 的 Vector2D 實例
let alsoPositive = -negative
// alsoPositive 是一個值為 (3.0, 4.0) 的 Vector2D 實例
```
<a name="compound_assignment_operators"></a>
### 復合賦值運算符

*復合賦值運算符*將賦值運算符（`=`）與其它運算符進行結合。例如，將加法與賦值結合成加法賦值運算符（`+=`）。在實現的時候，需要把運算符的左參數設置成 `inout` 類型，因為這個參數的值會在運算符函數內直接被修改。

```swift
extension Vector2D {
    static func += (left: inout Vector2D, right: Vector2D) {
        left = left + right
    }
}
```

因為加法運算在之前已經定義過了，所以在這裡無需重新定義。在這裡可以直接利用現有的加法運算符函數，用它來對左值和右值進行相加，並再次賦值給左值：

```swift
var original = Vector2D(x: 1.0, y: 2.0)
let vectorToAdd = Vector2D(x: 3.0, y: 4.0)
original += vectorToAdd
// original 的值現在為 (4.0, 6.0)
```


> 注意  
> 不能對默認的賦值運算符（`=`）進行重載。只有組合賦值運算符可以被重載。同樣地，也無法對三目條件運算符 （`a ? b : c`） 進行重載。

<a name="equivalence_operators"></a>
### 等價運算符

自定義的類和結構體沒有對*等價運算符*進行默認實現，等價運算符通常被稱為「相等」運算符（`==`）與「不等」運算符（`!=`）。對於自定義類型，Swift 無法判斷其是否「相等」，因為「相等」的含義取決於這些自定義類型在你的代碼中所扮演的角色。

為了使用等價運算符能對自定義的類型進行判等運算，需要為其提供自定義實現，實現的方法與其它中綴運算符一樣：

```swift
extension Vector2D {
    static func == (left: Vector2D, right: Vector2D) -> Bool {
        return (left.x == right.x) && (left.y == right.y)
    }
    static func != (left: Vector2D, right: Vector2D) -> Bool {
        return !(left == right)
    }
}
```

上述代碼實現了「相等」運算符（`==`）來判斷兩個 `Vector2D` 實例是否相等。對於 `Vector2D` 類型來說，「相等」意味著「兩個實例的 `x` 屬性和 `y` 屬性都相等」，這也是代碼中用來進行判等的邏輯。示例裡同時也實現了「不等」運算符（`!=`），它簡單地將「相等」運算符的結果進行取反後返回。

現在我們可以使用這兩個運算符來判斷兩個 `Vector2D` 實例是否相等：

```swift
let twoThree = Vector2D(x: 2.0, y: 3.0)
let anotherTwoThree = Vector2D(x: 2.0, y: 3.0)
if twoThree == anotherTwoThree {
    print("These two vectors are equivalent.")
}
// 打印 「These two vectors are equivalent.」
```

<a name="custom_operators"></a>
## 自定義運算符

除了實現標准運算符，在 Swift 中還可以聲明和實現*自定義運算符*。可以用來自定義運算符的字符列表請參考[運算符](../chapter3/02_Lexical_Structure.html#operators)。

新的運算符要使用 `operator` 關鍵字在全局作用域內進行定義，同時還要指定 `prefix`、`infix` 或者 `postfix` 修飾符：

```swift
prefix operator +++
```

上面的代碼定義了一個新的名為 `+++` 的前綴運算符。對於這個運算符，在 Swift 中並沒有意義，因此我們針對 `Vector2D` 的實例來定義它的意義。對這個示例來講，`+++` 被實現為「前綴雙自增」運算符。它使用了前面定義的復合加法運算符來讓矩陣對自身進行相加，從而讓 `Vector2D` 實例的 `x` 屬性和 `y` 屬性的值翻倍。實現 `+++` 運算符的方式如下：

```swift
extension Vector2D {
    static prefix func +++ (vector: inout Vector2D) -> Vector2D {
        vector += vector
        return vector
    }
}


var toBeDoubled = Vector2D(x: 1.0, y: 4.0)
let afterDoubling = +++toBeDoubled
// toBeDoubled 現在的值為 (2.0, 8.0)
// afterDoubling 現在的值也為 (2.0, 8.0)
```

<a name="precedence_and_associativity_for_custom_infix_operators"></a>
### 自定義中綴運算符的優先級

每個自定義中綴運算符都屬於某個優先級組。這個優先級組指定了這個運算符和其他中綴運算符的優先級和結合性。[優先級和結合性](#precedence_and_associativity)中詳細闡述了這兩個特性是如何對中綴運算符的運算產生影響的。

而沒有明確放入優先級組的自定義中綴運算符會放到一個默認的優先級組內，其優先級高於三元運算符。

以下例子定義了一個新的自定義中綴運算符 `+-`，此運算符屬於 `AdditionPrecedence` 優先組：

```swift
infix operator +-: AdditionPrecedence
extension Vector2D {
    static func +- (left: Vector2D, right: Vector2D) -> Vector2D {
        return Vector2D(x: left.x + right.x, y: left.y - right.y)
    }
}
let firstVector = Vector2D(x: 1.0, y: 2.0)
let secondVector = Vector2D(x: 3.0, y: 4.0)
let plusMinusVector = firstVector +- secondVector
// plusMinusVector 是一個 Vector2D 實例，並且它的值為 (4.0, -2.0)
```

這個運算符把兩個向量的 `x` 值相加，同時用第一個向量的 `y` 值減去第二個向量的 `y` 值。因為它本質上是屬於「相加型」運算符，所以將它放置 `+` 和 `-` 等默認的中綴「相加型」運算符相同的優先級組中。關於 Swift 標准庫提供的運算符，以及完整的運算符優先級組和結合性設置，請參考 [Swift Standard Library Operators Reference](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Reference/Swift_StandardLibrary_Operators/index.html#//apple_ref/doc/uid/TP40016054)。而更多關於優先級組以及自定義操作符和優先級組的語法，請參考[運算符聲明](#operator_declaration)

> 注意  
> 當定義前綴與後綴運算符的時候，我們並沒有指定優先級。然而，如果對同一個值同時使用前綴與後綴運算符，則後綴運算符會先參與運算。
