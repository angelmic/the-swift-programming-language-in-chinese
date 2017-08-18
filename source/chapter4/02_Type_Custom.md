# 造個類型不是夢-白話Swift類型創建
-----------------

> 翻譯：[老碼團隊翻譯組-Tyrion](http://weibo.com/u/5241713117)
> 校對：[老碼團隊翻譯組-Oberyn](http://weibo.com/u/5241713117)

本頁包含內容：

- [自定義原型](#prototype)
- [實現默認值](#imp-default)
- [支持基本布爾型初始化](#init-by-bool)
- [支持Bool類型判斷](#condition-by-bool)
- [支持兼容各們各派的類型](#support-all-type)
- [完善OCBool的布爾基因體系](#make-up-type)

小伙伴們，Swift中的Bool類型有著非常重要的語法功能，並支撐起了整個Swift體系中的邏輯判斷體系，經過老碼的研究和學習， Bool類型本身其實是對基礎Boolean類型封裝，小伙伴們可能咬著手指頭問老碼，怎麼一會Bool類型，一會Boolean類型，其區別在於，前者是基於枚舉的組合類型，而後者則是基本類型，只有兩種true和false。

<a name="prefix_expressions"></a>
####自定義原型
接下老碼根據Bool的思想來創建一個OCBool類型，來讓小伙伴們了解一下Swift中到底是怎麼玩兒的。
來我們先看一下OCBool的定義。

#####代碼示例如下：
```swift
enum OCBool{
case ocTrue
case ocFalse
}
```

#####注意：

- 代碼中第2行和第3行，可以合並到一行寫，如蘋果官方Blog所寫的一樣
- 代碼中命名需要注意：OCBool是類型名，所以首字母必須大寫，而case中的ocTrue和ocFalse是小類型則需要首字母小寫。

<a name="imp-default"></a>
####實現默認值
行，我們給了一個漂亮的定義，不過按照傳統語言的經驗，Bool值默認情況下是假， 所以我們的OCBool也應該如此，我們使用類型擴展技術增加這個默認特性：
```swift
extension OCBool{
     init(){
             self =.ocFalse
     }
}
```

#####注意：
- 代碼中第1行：extension關鍵字，非常強大，小伙伴們可以通過此創造出許多好玩的東西，建議各位去Github上看一個名為「Swiftz」的項目，它將擴展用到了極致。
- 代碼中第3行：self = .ocFalse語法，剛入門的小伙伴們很迷糊，為什麼會有奇怪的點語法，因為大牛Chris在Swift中增加了類型智能推斷功能，在蘋果Blog中，提到了「Context」概念，就是這個意思，因為這行語句是在枚舉OCBool中的，其上下文就是OCBool的定義體，編譯器當然知道.ocFalse就是OCBool.ocFalse了，所以這裡直接點語法，非常整齊。
現在我們可以使用如下方法使用這個Bool類型。

#####代碼示例如下：
```swift
var result:OCBool = OCBool()
var result1:OCBool = .ocTrue
```

<a name="init-by-bool"></a>
####支持基本布爾型初始化
正如上述代碼所述，我們只能通過類型或者枚舉項目賦值，這是組合類型的用法，但是編碼的日子裡，我們總是希望和true，false直接打交道，也就是說，我們希望這麼做，
代碼示例如下：
```swift
var isSuccess:OCBool = true
```

如果小伙伴們直接這麼用，則會出現如下錯誤：
```
/Users/tyrion-OldCoder/Documents/Learning/BoolType/BoolType/main.swift:30:24: Type 'OCBool' does not conform to protocol 'BooleanLiteralConvertible'
```
編譯器咆哮的原因是，我們的類型沒有遵從「布爾字面量轉換協議」，接下來修正這個問題，
#####代碼示例如下：

```swift
import Foundation

println("Hello, World!")

enum OCBool{
    case ocTrue
    case ocFalse
}


extension OCBool: BooleanLiteralConvertible{
static func convertFromBooleanLiteral( value: Bool) ->OCBool{
    return value ? ocTrue : ocFalse
    }
}

var isSuccess:OCBool = true
```

#####注意：
- 代碼中的第11行是重點，我的類型OCBool支持了BooleanLiteralConvertible協議，這個協到底是幹什麼的呢，小伙伴們在Xcode代碼編輯器，按住Command鍵，然後點擊第11行中的BooleanLiteralConvertible協議名，則會進入它的定義，
#####其定義如下：
```swift
protocol BooleanLiteralConvertible {
    typealias BooleanLiteralType
    class func convertFromBooleanLiteral(value: BooleanLiteralType) -> Self
}
```

- 這個定義中有個類方法convertFromBooleanLiteral，它的參數為BooleanLiteralType類型，也就是我傳入的Bool類型， 且返回值為實現這個協議的類型本身，在我們的OCBool類型中，其返回值就是OCBool本身。經過這個定義，我們可以直接對OCBool類型直接進行布爾字面量初始化了。

<a name="condition-by-bool"></a>
####支持Bool類型判斷
小伙伴們不安分， 肯定想著我怎麼用它實現邏輯判斷，所以如果你這麼寫，
#####代碼示例如下：
```swift
var isSuccess:OCBool = true

if isSuccess {
    println( "老碼請你吃火鍋！")
}
```

你永遠吃不到老碼的火鍋，因為這裡編譯器會咆哮：
```
/Users/tyrion-OldCoder/Documents/Learning/BoolType/BoolType/main.swift:27:4: Type 'OCBool' does not conform to protocol 'LogicValue'
```
OCBool現在只能用bool類型初始化，而不能直接返回bool型，小火把們還記得在《老碼說編程之白話Swift江湖》中，老碼多次提到，媽媽再也不擔心我們 if a = 1{}的寫法了， 因為等號不支持值返回了， 所以在if判斷是後面的條件必須有返回值，OCBool沒有，所以編譯器哭了。我們解決這個問題。

#####代碼示例如下：
```swift
import Foundation

println("Hello, World!")

enum OCBool{
    case ocTrue
    case ocFalse
}


extension OCBool: BooleanLiteralConvertible{
static func convertFromBooleanLiteral( value: Bool) ->OCBool{
    return value ? ocTrue : ocFalse
    }
}

extension OCBool: LogicValue{
    func getLogicValue() ->Bool {
        var boolValue: Bool{
        switch self{
        case .ocTrue:
            return true
        case .ocFalse:
            return false
            }
        }
        return boolValue
    }
}


var isSuccess:OCBool = true

if isSuccess {
    println( "老碼請你吃火鍋！")
}
```

####運行結果如下：
```
Hello, World!
老碼請你吃火鍋！
Program ended with exit code: 0
```
#####注意：
- 如果小伙伴們現在用的是Beta版的Xcode，注意蘋果官方Blog中，在代碼第17行如果在Xcode Beta4下是錯誤的，這裡的協議是，LogicValue而不是BooleanVue，所以記得看錯誤提示才是好習慣。
- 注意代碼第34行，完美支持if判斷，且輸出結果為「老碼請你吃火鍋」，老碼也是說說而已，請不要當真。

<a name="support-all-type"></a>

####支持兼容各們各派的類型
小伙伴們，江湖風險，門派眾多，老碼有自己的OCBool類型，可能嵩山少林有自己的SSBool類型，甚至連郭美美都可能有自己的MMBool類型，所以OCBool必須能夠識別這些類型，這些各門各派的類型，只要支持LogicValue協議，就應該可以被識別，看老碼怎麼做，

#####代碼示例如下：
```swift
extension OCBool{
    init( _ v: LogicValue )
    {
        if v.getLogicValue(){
            self = .ocTrue
        }
        else{
            self = .ocFalse
        }
    }

}

var mmResult: Bool = true
var ocResult:OCBool = OCBool(mmResult)


if ocResult {
    println( "老碼沒錢，郭美美請你吃火鍋！")
}
```

#####代碼運行結果如下：
```
Hello, World!
老碼沒錢，郭美美請你吃火鍋！
Program ended with exit code: 0
```
漂亮！我們的OCBool類型現在支持了所有的邏輯變量初始化。

#####注意：
- 代碼中第2行：「_」下橫杠的用法，這是一個功能強大的小強，在此的目的是屏蔽外部參數名，所以小伙伴們可以直接：var ocResult:OCBool = OCBool(mmResult)而不是：var ocResult:OCBool = OCBool(v: mmResult)，小伙伴們驚待了！這個init函數中本來就沒有外部參數名啊，還記得老碼在書裡說過沒，Swift的初始化函數會默認使用內部參數名，作為外部參數名。

<a name="make-up-type"></a>
####完善OCBool的布爾基因體系：
小伙伴們，bool類型的價值就是在於各種判斷，諸如==，!=, &，|,^,!，以及各種組合邏輯運算，我們OCBool也要具備這些功能，否則就會基因缺陷，且看老碼如何實現：

```swift
extension OCBool: Equatable{
}

//支持等值判斷運算符
func ==( left: OCBool, right: OCBool )->Bool{
    switch (left, right){
    case (.ocTrue, .ocTrue):
            return true
    default:
        return false
    }
}
//支持位與運算符
func &( left:OCBool, right: OCBool)->OCBool{

    if left{
        return right
    }
    else{
        return false
    }
}
//支持位或運算符
func |( left:OCBool, right: OCBool)->OCBool{

    if left{
        return true
    }
    else{
        return right
    }
}

//支持位異或運算符
func ^( left:OCBool, right: OCBool)->OCBool{
    return OCBool( left != right )
}
//支持求反運算符
@prefix func !( a:OCBool )-> OCBool{
    return a ^ true
}
//支持組合求與運算符
func &= (inout left:OCBool, right:OCBool ){
    left = left & right
}


var isHasMoney:OCBool = true
var isHasWife:OCBool = true
var isHasHealty:OCBool = true
var isHasLover:OCBool = true

isHasMoney != isHasHealty
isHasHealty == isHasMoney
isHasWife ^ isHasLover
isHasWife = !isHasLover

if (isHasMoney | isHasHealty) & isHasHealty{
    println( "人生贏家，就像老碼一樣！")
}else
{
    println("人生最苦的事事，人死了錢沒花了，人生最苦的事是，人活著，錢沒了！")
}
```

好了，到這裡就到這裡了，窗外的雷聲叫醒了老碼，現在應該去吃飯了，以上老碼給大家展示了如果制造一個自己的類型，記得老碼的示例是在Xcode6 Beta4下測試的，至於Beta5的改變還沒有涉及，小伙伴們要好生練習，以後各種自定類型都是基於這個思想。還有這個章節不是老碼的原創，老碼認真的閱讀了蘋果的官方博客，且自己的練習總結，如果小伙伴們費了吃奶的勁還是看不懂，請找度娘谷歌，還是看不懂請到老碼官方微博：http://weibo.com/u/5241713117咆哮。



本文由翻譯自Apple Swift Blog ：https://developer.apple.com/swift/blog/?id=8
