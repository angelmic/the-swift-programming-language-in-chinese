# 嵌套類型
-----------------

> 1.0
> 翻譯：[Lin-H](https://github.com/Lin-H)
> 校對：[shinyzhu](https://github.com/shinyzhu)

> 2.0
> 翻譯+校對：[SergioChan](https://github.com/SergioChan)

> 2.1
> 校對：[shanks](http://codebuild.me)，2015-11-01
> 
> 2.2
> 翻譯+校對：[SketchK](https://github.com/SketchK) 2016-05-16    
> 3.0.1，shanks，2016-11-13

本頁包含內容：

- [嵌套類型實踐](#nested_types_in_action)
- [引用嵌套類型](#referring_to_nested_types)

枚舉常被用於為特定類或結構體實現某些功能。類似的，枚舉可以方便的定義工具類或結構體，從而為某個復雜的類型所使用。為了實現這種功能，Swift 允許你定義*嵌套類型*，可以在支持的類型中定義嵌套的枚舉、類和結構體。

要在一個類型中嵌套另一個類型，將嵌套類型的定義寫在其外部類型的`{}`內，而且可以根據需要定義多級嵌套。

<a name="nested_types_in_action"></a>
## 嵌套類型實踐

下面這個例子定義了一個結構體`BlackjackCard`（二十一點），用來模擬`BlackjackCard`中的撲克牌點數。`BlackjackCard`結構體包含兩個嵌套定義的枚舉類型`Suit`和`Rank`。

在`BlackjackCard`中，`Ace`牌可以表示`1`或者`11`，`Ace`牌的這一特征通過一個嵌套在`Rank`枚舉中的結構體`Values`來表示：

```swift
struct BlackjackCard {
    // 嵌套的 Suit 枚舉
    enum Suit: Character {
       case Spades = "♠", Hearts = "♡", Diamonds = "♢", Clubs = "♣"
    }

    // 嵌套的 Rank 枚舉
    enum Rank: Int {
       case Two = 2, Three, Four, Five, Six, Seven, Eight, Nine, Ten
       case Jack, Queen, King, Ace
       struct Values {
           let first: Int, second: Int?
       }
       var values: Values {
        switch self {
        case .Ace:
            return Values(first: 1, second: 11)
        case .Jack, .Queen, .King:
            return Values(first: 10, second: nil)
        default:
            return Values(first: self.rawValue, second: nil)
            }
       }
    }

    // BlackjackCard 的屬性和方法
    let rank: Rank, suit: Suit
    var description: String {
    	var output = "suit is \(suit.rawValue),"
        output += " value is \(rank.values.first)"
        if let second = rank.values.second {
            output += " or \(second)"
        }
        return output
    }
}
```

`Suit`枚舉用來描述撲克牌的四種花色，並用一個`Character`類型的原始值表示花色符號。

`Rank`枚舉用來描述撲克牌從`Ace`~`10`，以及`J`、`Q`、`K`，這`13`種牌，並用一個`Int`類型的原始值表示牌的面值。（這個`Int`類型的原始值未用於`Ace`、`J`、`Q`、`K`這`4`種牌。）

如上所述，`Rank`枚舉在內部定義了一個嵌套結構體`Values`。結構體`Values`中定義了兩個屬性，用於反映只有`Ace`有兩個數值，其余牌都只有一個數值：

- `first`的類型為`Int`
- `second`的類型為`Int?`，或者說「可選 `Int`」

`Rank`還定義了一個計算型屬性`values`，它將會返回一個`Values`結構體的實例。這個計算型屬性會根據牌的面值，用適當的數值去初始化`Values`實例。對於`J`、`Q`、`K`、`Ace`這四種牌，會使用特殊數值。對於數字面值的牌，使用枚舉實例的原始值。

`BlackjackCard`結構體擁有兩個屬性——`rank`與`suit`。它也同樣定義了一個計算型屬性`description`，`description`屬性用`rank`和`suit`中的內容來構建對撲克牌名字和數值的描述。該屬性使用可選綁定來檢查可選類型`second`是否有值，若有值，則在原有的描述中增加對`second`的描述。

因為`BlackjackCard`是一個沒有自定義構造器的結構體，在[結構體的逐一成員構造器](./14_Initialization.html#memberwise_initializers_for_structure_types)中可知，結構體有默認的成員構造器，所以你可以用默認的構造器去初始化新常量`theAceOfSpades`：

```swift
let theAceOfSpades = BlackjackCard(rank: .Ace, suit: .Spades)
print("theAceOfSpades: \(theAceOfSpades.description)")
// 打印 「theAceOfSpades: suit is ♠, value is 1 or 11」
```

盡管`Rank`和`Suit`嵌套在`BlackjackCard`中，但它們的類型仍可從上下文中推斷出來，所以在初始化實例時能夠單獨通過成員名稱（`.Ace`和`.Spades`）引用枚舉實例。在上面的例子中，`description`屬性正確地反映了黑桃A牌具有`1`和`11`兩個值。

<a name="referring_to_nested_types"></a>
## 引用嵌套類型

在外部引用嵌套類型時，在嵌套類型的類型名前加上其外部類型的類型名作為前綴：

```swift
let heartsSymbol = BlackjackCard.Suit.Hearts.rawValue
// 紅心符號為 「♡」
```

對於上面這個例子，這樣可以使`Suit`、`Rank`和`Values`的名字盡可能的短，因為它們的名字可以由定義它們的上下文來限定。
