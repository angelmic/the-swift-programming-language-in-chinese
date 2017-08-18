# 協議
-----------------

> 1.0
> 翻譯：[geek5nan](https://github.com/geek5nan)
> 校對：[dabing1022](https://github.com/dabing1022)

> 2.0
> 翻譯+校對：[futantan](https://github.com/futantan)

> 2.1
> 翻譯：[小鐵匠Linus](https://github.com/kevin833752)
> 校對：[shanks](http://codebuild.me)
> 
> 2.2
> 翻譯+校對：[SketchK](https://github.com/SketchK) 
> 
> 3.0
> 校對：[CMB](https://github.com/chenmingbiao)，版本日期：2016-09-13   
> 3.0.1，shanks，2016-11-13

本頁包含內容：

- [協議語法](#protocol_syntax)
- [屬性要求](#property_requirements)
- [方法要求（Method Requirements）](#method_requirements)
- [Mutating 方法要求](#mutating_method_requirements)
- [構造器要求](#initializer_requirements)
- [協議作為類型](#protocols_as_types)
- [委托（代理）模式](#delegation)
- [通過擴展添加協議一致性](#adding_protocol_conformance_with_an_extension)
- [通過擴展遵循協議](#declaring_protocol_adoption_with_an_extension)
- [協議類型的集合](#collections_of_protocol_types)
- [協議的繼承](#protocol_inheritance)
- [類類型專屬協議](#class_only_protocol)
- [協議合成](#protocol_composition)
- [檢查協議一致性](#checking_for_protocol_conformance)
- [可選的協議要求](#optional_protocol_requirements)
- [協議擴展](#protocol_extensions)

*協議* 定義了一個藍圖，規定了用來實現某一特定任務或者功能的方法、屬性，以及其他需要的東西。類、結構體或枚舉都可以遵循協議，並為協議定義的這些要求提供具體實現。某個類型能夠滿足某個協議的要求，就可以說該類型*遵循*這個協議。

除了遵循協議的類型必須實現的要求外，還可以對協議進行擴展，通過擴展來實現一部分要求或者實現一些附加功能，這樣遵循協議的類型就能夠使用這些功能。

<a name="protocol_syntax"></a>
## 協議語法

協議的定義方式與類、結構體和枚舉的定義非常相似：

```swift
protocol SomeProtocol {
	// 這裡是協議的定義部分
}
```

要讓自定義類型遵循某個協議，在定義類型時，需要在類型名稱後加上協議名稱，中間以冒號（`:`）分隔。遵循多個協議時，各協議之間用逗號（`,`）分隔：

```swift
struct SomeStructure: FirstProtocol, AnotherProtocol {
	// 這裡是結構體的定義部分
}
```

擁有父類的類在遵循協議時，應該將父類名放在協議名之前，以逗號分隔：

```swift
class SomeClass: SomeSuperClass, FirstProtocol, AnotherProtocol {
	// 這裡是類的定義部分
}
```

<a name="property_requirements"></a>
## 屬性要求

協議可以要求遵循協議的類型提供特定名稱和類型的實例屬性或類型屬性。協議不指定屬性是存儲型屬性還是計算型屬性，它只指定屬性的名稱和類型。此外，協議還指定屬性是可讀的還是可讀可寫的。

如果協議要求屬性是可讀可寫的，那麼該屬性不能是常量屬性或只讀的計算型屬性。如果協議只要求屬性是可讀的，那麼該屬性不僅可以是可讀的，如果代碼需要的話，還可以是可寫的。

協議總是用 `var` 關鍵字來聲明變量屬性，在類型聲明後加上 `{ set get }` 來表示屬性是可讀可寫的，可讀屬性則用 `{ get }` 來表示：

```swift
protocol SomeProtocol {
	var mustBeSettable: Int { get set }
	var doesNotNeedToBeSettable: Int { get }
}
```

在協議中定義類型屬性時，總是使用 `static` 關鍵字作為前綴。當類類型遵循協議時，除了 `static` 關鍵字，還可以使用 `class` 關鍵字來聲明類型屬性：

```swift
protocol AnotherProtocol {
	static var someTypeProperty: Int { get set }
}
```

如下所示，這是一個只含有一個實例屬性要求的協議：

```swift
protocol FullyNamed {
	var fullName: String { get }
}
```

`FullyNamed` 協議除了要求遵循協議的類型提供 `fullName` 屬性外，並沒有其他特別的要求。這個協議表示，任何遵循 `FullyNamed` 的類型，都必須有一個可讀的 `String` 類型的實例屬性 `fullName`。

下面是一個遵循 `FullyNamed` 協議的簡單結構體：

```swift
struct Person: FullyNamed {
	var fullName: String
}
let john = Person(fullName: "John Appleseed")
// john.fullName 為 "John Appleseed"
```

這個例子中定義了一個叫做 `Person` 的結構體，用來表示一個具有名字的人。從第一行代碼可以看出，它遵循了 `FullyNamed` 協議。

`Person` 結構體的每一個實例都有一個 `String` 類型的存儲型屬性 `fullName`。這正好滿足了 `FullyNamed` 協議的要求，也就意味著 `Person` 結構體正確地符合了協議。（如果協議要求未被完全滿足，在編譯時會報錯。）

下面是一個更為復雜的類，它適配並遵循了 `FullyNamed` 協議：

```swift
class Starship: FullyNamed {
    var prefix: String?
    var name: String
    init(name: String, prefix: String? = nil) {
        self.name = name
        self.prefix = prefix
    }
    var fullName: String {
        return (prefix != nil ? prefix! + " " : "") + name
    }
}
var ncc1701 = Starship(name: "Enterprise", prefix: "USS")
// ncc1701.fullName 是 "USS Enterprise"
```

`Starship` 類把 `fullName` 屬性實現為只讀的計算型屬性。每一個 `Starship` 類的實例都有一個名為 `name` 的非可選屬性和一個名為 `prefix` 的可選屬性。 當 `prefix` 存在時，計算型屬性 `fullName` 會將 `prefix` 插入到 `name` 之前，從而為星際飛船構建一個全名。

<a name="method_requirements"></a>
## 方法要求

協議可以要求遵循協議的類型實現某些指定的實例方法或類方法。這些方法作為協議的一部分，像普通方法一樣放在協議的定義中，但是不需要大括號和方法體。可以在協議中定義具有可變參數的方法，和普通方法的定義方式相同。但是，不支持為協議中的方法的參數提供默認值。

正如屬性要求中所述，在協議中定義類方法的時候，總是使用 `static` 關鍵字作為前綴。當類類型遵循協議時，除了 `static` 關鍵字，還可以使用 `class` 關鍵字作為前綴：

```swift
protocol SomeProtocol {
	static func someTypeMethod()
}
```

下面的例子定義了一個只含有一個實例方法的協議：

```swift
protocol RandomNumberGenerator {
	func random() -> Double
}
```

`RandomNumberGenerator` 協議要求遵循協議的類型必須擁有一個名為 `random`， 返回值類型為 `Double` 的實例方法。盡管這裡並未指明，但是我們假設返回值在 `[0.0,1.0)` 區間內。

`RandomNumberGenerator` 協議並不關心每一個隨機數是怎樣生成的，它只要求必須提供一個隨機數生成器。

如下所示，下邊是一個遵循並符合 `RandomNumberGenerator` 協議的類。該類實現了一個叫做 *線性同余生成器（linear congruential generator）* 的偽隨機數算法。

```swift
class LinearCongruentialGenerator: RandomNumberGenerator {
	var lastRandom = 42.0
	let m = 139968.0
	let a = 3877.0
	let c = 29573.0
	func random() -> Double {
		lastRandom = ((lastRandom * a + c).truncatingRemainder(dividingBy:m))
		return lastRandom / m
	}
}
let generator = LinearCongruentialGenerator()
print("Here's a random number: \(generator.random())")
// 打印 「Here's a random number: 0.37464991998171」
print("And another one: \(generator.random())")
// 打印 「And another one: 0.729023776863283」
```

<a name="mutating_method_requirements"></a>
## Mutating 方法要求

有時需要在方法中改變方法所屬的實例。例如，在值類型（即結構體和枚舉）的實例方法中，將 `mutating` 關鍵字作為方法的前綴，寫在 `func` 關鍵字之前，表示可以在該方法中修改它所屬的實例以及實例的任意屬性的值。這一過程在[在實例方法中修改值類型](./11_Methods.html#modifying_value_types_from_within_instance_methods)章節中有詳細描述。

如果你在協議中定義了一個實例方法，該方法會改變遵循該協議的類型的實例，那麼在定義協議時需要在方法前加 `mutating` 關鍵字。這使得結構體和枚舉能夠遵循此協議並滿足此方法要求。

> 注意  
> 實現協議中的 `mutating` 方法時，若是類類型，則不用寫 `mutating` 關鍵字。而對於結構體和枚舉，則必須寫 `mutating` 關鍵字。

如下所示，`Togglable` 協議只要求實現一個名為 `toggle` 的實例方法。根據名稱的暗示，`toggle()` 方法將改變實例屬性，從而切換遵循該協議類型的實例的狀態。

`toggle()` 方法在定義的時候，使用 `mutating` 關鍵字標記，這表明當它被調用時，該方法將會改變遵循協議的類型的實例：

```swift
protocol Togglable {
	mutating func toggle()
}
```

當使用枚舉或結構體來實現 `Togglable` 協議時，需要提供一個帶有 `mutating` 前綴的 `toggle()` 方法。

下面定義了一個名為 `OnOffSwitch` 的枚舉。這個枚舉在兩種狀態之間進行切換，用枚舉成員 `On` 和 `Off` 表示。枚舉的 `toggle()` 方法被標記為 `mutating`，以滿足 `Togglable` 協議的要求：

```swift
enum OnOffSwitch: Togglable {
    case off, on
    mutating func toggle() {
        switch self {
        case .off:
            self = .on
        case .on:
            self = .off
        }
    }
}
var lightSwitch = OnOffSwitch.off
lightSwitch.toggle()
// lightSwitch 現在的值為 .On
```

<a name="initializer_requirements"></a>
## 構造器要求

協議可以要求遵循協議的類型實現指定的構造器。你可以像編寫普通構造器那樣，在協議的定義裡寫下構造器的聲明，但不需要寫花括號和構造器的實體：

```swift
protocol SomeProtocol {
    init(someParameter: Int)
}
```

### 構造器要求在類中的實現

你可以在遵循協議的類中實現構造器，無論是作為指定構造器，還是作為便利構造器。無論哪種情況，你都必須為構造器實現標上 `required` 修飾符：

```swift
class SomeClass: SomeProtocol {
    required init(someParameter: Int) {
        // 這裡是構造器的實現部分
    }
}
```

使用 `required` 修飾符可以確保所有子類也必須提供此構造器實現，從而也能符合協議。

關於 `required` 構造器的更多內容，請參考[必要構造器](./14_Initialization.html#required_initializers)。

> 注意  
> 如果類已經被標記為 `final`，那麼不需要在協議構造器的實現中使用 `required` 修飾符，因為 `final` 類不能有子類。關於 `final` 修飾符的更多內容，請參見[防止重寫](./13_Inheritance.html#preventing_overrides)。

如果一個子類重寫了父類的指定構造器，並且該構造器滿足了某個協議的要求，那麼該構造器的實現需要同時標注 `required` 和 `override` 修飾符：

```swift
protocol SomeProtocol {
    init()
}

class SomeSuperClass {
    init() {
        // 這裡是構造器的實現部分
    }
}

class SomeSubClass: SomeSuperClass, SomeProtocol {
    // 因為遵循協議，需要加上 required
    // 因為繼承自父類，需要加上 override
    required override init() {
        // 這裡是構造器的實現部分
    }
}
```

### 可失敗構造器要求

協議還可以為遵循協議的類型定義可失敗構造器要求，詳見[可失敗構造器](./14_Initialization.html#failable_initializers)。

遵循協議的類型可以通過可失敗構造器（`init?`）或非可失敗構造器（`init`）來滿足協議中定義的可失敗構造器要求。協議中定義的非可失敗構造器要求可以通過非可失敗構造器（`init`）或隱式解包可失敗構造器（`init!`）來滿足。

<a name="protocols_as_types"></a>
## 協議作為類型

盡管協議本身並未實現任何功能，但是協議可以被當做一個成熟的類型來使用。

協議可以像其他普通類型一樣使用，使用場景如下：

* 作為函數、方法或構造器中的參數類型或返回值類型
* 作為常量、變量或屬性的類型
* 作為數組、字典或其他容器中的元素類型

> 注意  
> 協議是一種類型，因此協議類型的名稱應與其他類型（例如 `Int`，`Double`，`String`）的寫法相同，使用大寫字母開頭的駝峰式寫法，例如（`FullyNamed` 和 `RandomNumberGenerator`）。

下面是將協議作為類型使用的例子：

```swift
class Dice {
	let sides: Int
	let generator: RandomNumberGenerator
	init(sides: Int, generator: RandomNumberGenerator) {
		self.sides = sides
		self.generator = generator
	}
	func roll() -> Int {
		return Int(generator.random() * Double(sides)) + 1
	}
}
```

例子中定義了一個 `Dice` 類，用來代表桌游中擁有 N 個面的骰子。`Dice` 的實例含有 `sides` 和 `generator` 兩個屬性，前者是整型，用來表示骰子有幾個面，後者為骰子提供一個隨機數生成器，從而生成隨機點數。

`generator` 屬性的類型為 `RandomNumberGenerator`，因此任何遵循了 `RandomNumberGenerator` 協議的類型的實例都可以賦值給 `generator`，除此之外並無其他要求。

`Dice` 類還有一個構造器，用來設置初始狀態。構造器有一個名為 `generator`，類型為 `RandomNumberGenerator` 的形參。在調用構造方法創建 `Dice` 的實例時，可以傳入任何遵循 `RandomNumberGenerator` 協議的實例給 `generator`。

`Dice` 類提供了一個名為 `roll` 的實例方法，用來模擬骰子的面值。它先調用 `generator` 的 `random()` 方法來生成一個 `[0.0,1.0)` 區間內的隨機數，然後使用這個隨機數生成正確的骰子面值。因為 `generator` 遵循了 `RandomNumberGenerator` 協議，可以確保它有個 `random()` 方法可供調用。

下面的例子展示了如何使用 `LinearCongruentialGenerator` 的實例作為隨機數生成器來創建一個六面骰子：

```swift
var d6 = Dice(sides: 6, generator: LinearCongruentialGenerator())
for _ in 1...5 {
	print("Random dice roll is \(d6.roll())")
}
// Random dice roll is 3
// Random dice roll is 5
// Random dice roll is 4
// Random dice roll is 5
// Random dice roll is 4
```

<a name="delegation"></a>
## 委托（代理）模式

*委托*是一種設計模式，它允許類或結構體將一些需要它們負責的功能委托給其他類型的實例。委托模式的實現很簡單：定義協議來封裝那些需要被委托的功能，這樣就能確保遵循協議的類型能提供這些功能。委托模式可以用來響應特定的動作，或者接收外部數據源提供的數據，而無需關心外部數據源的類型。

下面的例子定義了兩個基於骰子游戲的協議：

```swift
protocol DiceGame {
    var dice: Dice { get }
    func play()
}
protocol DiceGameDelegate {
    func gameDidStart(_ game: DiceGame)
    func game(_ game: DiceGame, didStartNewTurnWithDiceRoll diceRoll: Int)
    func gameDidEnd(_ game: DiceGame)
}
```

`DiceGame` 協議可以被任意涉及骰子的游戲遵循。`DiceGameDelegate` 協議可以被任意類型遵循，用來追蹤 `DiceGame` 的游戲過程。

如下所示，`SnakesAndLadders` 是 [控制流](./05_Control_Flow.html) 章節引入的蛇梯棋游戲的新版本。新版本使用 `Dice` 實例作為骰子，並且實現了 `DiceGame` 和 `DiceGameDelegate` 協議，後者用來記錄游戲的過程：

```swift
class SnakesAndLadders: DiceGame {
	let finalSquare = 25
	let dice = Dice(sides: 6, generator: LinearCongruentialGenerator())
	var square = 0
	var board: [Int]
	init() {
		board = [Int](count: finalSquare + 1, repeatedValue: 0)
		board[03] = +08; board[06] = +11; board[09] = +09; board[10] = +02
		board[14] = -10; board[19] = -11; board[22] = -02; board[24] = -08
	}
 	var delegate: DiceGameDelegate?
 	func play() {
 		square = 0
 		delegate?.gameDidStart(self)
 		gameLoop: while square != finalSquare {
 			let diceRoll = dice.roll()
 			delegate?.game(self, didStartNewTurnWithDiceRoll: diceRoll)
 			switch square + diceRoll {
 			case finalSquare:
 				break gameLoop
 			case let newSquare where newSquare > finalSquare:
 				continue gameLoop
 			default:
 			    square += diceRoll
 			    square += board[square]
 			}
 		}
 		delegate?.gameDidEnd(self)
 	}
}
```

關於這個蛇梯棋游戲的詳細描述請參閱 [控制流](./05_Control_Flow.html) 章節中的 [Break](./05_Control_Flow.html#break) 部分。

這個版本的游戲封裝到了 `SnakesAndLadders` 類中，該類遵循了 `DiceGame` 協議，並且提供了相應的可讀的 `dice` 屬性和 `play()` 方法。（ `dice` 屬性在構造之後就不再改變，且協議只要求 `dice` 為可讀的，因此將 `dice` 聲明為常量屬性。）

游戲使用 `SnakesAndLadders` 類的 `init()` 構造器來初始化游戲。所有的游戲邏輯被轉移到了協議中的 `play()` 方法，`play()` 方法使用協議要求的 `dice` 屬性提供骰子搖出的值。

注意，`delegate` 並不是游戲的必備條件，因此 `delegate` 被定義為 `DiceGameDelegate` 類型的可選屬性。因為 `delegate` 是可選值，因此會被自動賦予初始值 `nil`。隨後，可以在游戲中為 `delegate` 設置適當的值。

`DicegameDelegate` 協議提供了三個方法用來追蹤游戲過程。這三個方法被放置於游戲的邏輯中，即 `play()` 方法內。分別在游戲開始時，新一輪開始時，以及游戲結束時被調用。

因為 `delegate` 是一個 `DiceGameDelegate` 類型的可選屬性，因此在 `play()` 方法中通過可選鏈式調用來調用它的方法。若 `delegate` 屬性為 `nil`，則調用方法會優雅地失敗，並不會產生錯誤。若 `delegate` 不為 `nil`，則方法能夠被調用，並傳遞 `SnakesAndLadders` 實例作為參數。

如下示例定義了 `DiceGameTracker` 類，它遵循了 `DiceGameDelegate` 協議：

```swift
class DiceGameTracker: DiceGameDelegate {
    var numberOfTurns = 0
    func gameDidStart(_ game: DiceGame) {
        numberOfTurns = 0
        if game is SnakesAndLadders {
            print("Started a new game of Snakes and Ladders")
        }
        print("The game is using a \(game.dice.sides)-sided dice")
    }
    func game(_ game: DiceGame, didStartNewTurnWithDiceRoll diceRoll: Int) {
        numberOfTurns += 1
        print("Rolled a \(diceRoll)")
    }
    func gameDidEnd(_ game: DiceGame) {
        print("The game lasted for \(numberOfTurns) turns")
    }
}
```

`DiceGameTracker` 實現了 `DiceGameDelegate` 協議要求的三個方法，用來記錄游戲已經進行的輪數。當游戲開始時，`numberOfTurns` 屬性被賦值為 `0`，然後在每新一輪中遞增，游戲結束後，打印游戲的總輪數。

`gameDidStart(_:)` 方法從 `game` 參數獲取游戲信息並打印。`game` 參數是 `DiceGame` 類型而不是 `SnakeAndLadders` 類型，所以在`gameDidStart(_:)` 方法中只能訪問 `DiceGame` 協議中的內容。當然了，`SnakeAndLadders` 的方法也可以在類型轉換之後調用。在上例代碼中，通過 `is` 操作符檢查 `game` 是否為 `SnakesAndLadders` 類型的實例，如果是，則打印出相應的消息。

無論當前進行的是何種游戲，由於 `game` 符合 `DiceGame` 協議，可以確保 `game` 含有 `dice` 屬性。因此在 `gameDidStart(_:)` 方法中可以通過傳入的 `game` 參數來訪問 `dice` 屬性，進而打印出 `dice` 的 `sides` 屬性的值。

`DiceGameTracker` 的運行情況如下所示：

```swift
let tracker = DiceGameTracker()
let game = SnakesAndLadders()
game.delegate = tracker
game.play()
// Started a new game of Snakes and Ladders
// The game is using a 6-sided dice
// Rolled a 3
// Rolled a 5
// Rolled a 4
// Rolled a 5
// The game lasted for 4 turns
```

<a name="adding_protocol_conformance_with_an_extension"></a>
## 通過擴展添加協議一致性

即便無法修改源代碼，依然可以通過擴展令已有類型遵循並符合協議。擴展可以為已有類型添加屬性、方法、下標以及構造器，因此可以符合協議中的相應要求。詳情請在[擴展](./21_Extensions.html)章節中查看。

> 注意  
> 通過擴展令已有類型遵循並符合協議時，該類型的所有實例也會隨之獲得協議中定義的各項功能。

例如下面這個 `TextRepresentable` 協議，任何想要通過文本表示一些內容的類型都可以實現該協議。這些想要表示的內容可以是實例本身的描述，也可以是實例當前狀態的文本描述：

```swift
protocol TextRepresentable {
    var textualDescription: String { get }
}
```

可以通過擴展，令先前提到的 `Dice` 類遵循並符合 `TextRepresentable` 協議：

```swift
extension Dice: TextRepresentable {
    var textualDescription: String {
		return "A \(sides)-sided dice"
	}
}
```

通過擴展遵循並符合協議，和在原始定義中遵循並符合協議的效果完全相同。協議名稱寫在類型名之後，以冒號隔開，然後在擴展的大括號內實現協議要求的內容。

現在所有 `Dice` 的實例都可以看做 `TextRepresentable` 類型：

```swift
let d12 = Dice(sides: 12, generator: LinearCongruentialGenerator())
print(d12.textualDescription)
// 打印 「A 12-sided dice」
```

同樣，`SnakesAndLadders` 類也可以通過擴展遵循並符合 `TextRepresentable` 協議：

```swift
extension SnakesAndLadders: TextRepresentable {
    var textualDescription: String {
		return "A game of Snakes and Ladders with \(finalSquare) squares"
	}
}
print(game.textualDescription)
// 打印 「A game of Snakes and Ladders with 25 squares」
```

<a name="declaring_protocol_adoption_with_an_extension"></a>
## 通過擴展遵循協議

當一個類型已經符合了某個協議中的所有要求，卻還沒有聲明遵循該協議時，可以通過空擴展體的擴展來遵循該協議：

```swift
struct Hamster {
	var name: String
   	var textualDescription: String {
		return "A hamster named \(name)"
	}
}
extension Hamster: TextRepresentable {}
```

從現在起，`Hamster` 的實例可以作為 `TextRepresentable` 類型使用：

```swift
let simonTheHamster = Hamster(name: "Simon")
let somethingTextRepresentable: TextRepresentable = simonTheHamster
print(somethingTextRepresentable.textualDescription)
// 打印 「A hamster named Simon」
```

> 注意  
> 即使滿足了協議的所有要求，類型也不會自動遵循協議，必須顯式地遵循協議。

<a name="collections_of_protocol_types"></a>
## 協議類型的集合

協議類型可以在數組或者字典這樣的集合中使用，在[協議類型](./22_Protocols.html##protocols_as_types)提到了這樣的用法。下面的例子創建了一個元素類型為 `TextRepresentable` 的數組：

```swift
let things: [TextRepresentable] = [game, d12, simonTheHamster]
```

如下所示，可以遍歷 `things` 數組，並打印每個元素的文本表示：

```swift
for thing in things {
	print(thing.textualDescription)
}
// A game of Snakes and Ladders with 25 squares
// A 12-sided dice
// A hamster named Simon
```

`thing` 是 `TextRepresentable` 類型而不是 `Dice`，`DiceGame`，`Hamster` 等類型，即使實例在幕後確實是這些類型中的一種。由於 `thing` 是 `TextRepresentable` 類型，任何 `TextRepresentable` 的實例都有一個 `textualDescription` 屬性，所以在每次循環中可以安全地訪問 `thing.textualDescription`。

<a name="protocol_inheritance"></a>
## 協議的繼承

協議能夠*繼承*一個或多個其他協議，可以在繼承的協議的基礎上增加新的要求。協議的繼承語法與類的繼承相似，多個被繼承的協議間用逗號分隔：

```swift
protocol InheritingProtocol: SomeProtocol, AnotherProtocol {
	// 這裡是協議的定義部分
}
```

如下所示，`PrettyTextRepresentable` 協議繼承了 `TextRepresentable` 協議：

```swift
protocol PrettyTextRepresentable: TextRepresentable {
	var prettyTextualDescription: String { get }
}
```

例子中定義了一個新的協議 `PrettyTextRepresentable`，它繼承自 `TextRepresentable` 協議。任何遵循 `PrettyTextRepresentable` 協議的類型在滿足該協議的要求時，也必須滿足 `TextRepresentable` 協議的要求。在這個例子中，`PrettyTextRepresentable` 協議額外要求遵循協議的類型提供一個返回值為 `String` 類型的 `prettyTextualDescription` 屬性。

如下所示，擴展 `SnakesAndLadders`，使其遵循並符合 `PrettyTextRepresentable` 協議：

```swift
extension SnakesAndLadders: PrettyTextRepresentable {
    var prettyTextualDescription: String {
        var output = textualDescription + ":\n"
        for index in 1...finalSquare {
            switch board[index] {
            case let ladder where ladder > 0:
                output += "▲ "
            case let snake where snake < 0:
                output += "▼ "
            default:
                output += "○ "
            }
        }
        return output
    }
}
```

上述擴展令 `SnakesAndLadders` 遵循了 `PrettyTextRepresentable` 協議，並提供了協議要求的 `prettyTextualDescription` 屬性。每個 `PrettyTextRepresentable` 類型同時也是 `TextRepresentable` 類型，所以在 `prettyTextualDescription` 的實現中，可以訪問 `textualDescription` 屬性。然後，拼接上了冒號和換行符。接著，遍歷數組中的元素，拼接一個幾何圖形來表示每個棋盤方格的內容：

* 當從數組中取出的元素的值大於 `0` 時，用 `▲` 表示。
* 當從數組中取出的元素的值小於 `0` 時，用 `▼` 表示。
* 當從數組中取出的元素的值等於 `0` 時，用 `○` 表示。

任意 `SankesAndLadders` 的實例都可以使用 `prettyTextualDescription` 屬性來打印一個漂亮的文本描述：

```swift
print(game.prettyTextualDescription)
// A game of Snakes and Ladders with 25 squares:
// ○ ○ ▲ ○ ○ ▲ ○ ○ ▲ ▲ ○ ○ ○ ▼ ○ ○ ○ ○ ▼ ○ ○ ▼ ○ ▼ ○
```

<a name="class_only_protocol"></a>
## 類類型專屬協議

你可以在協議的繼承列表中，通過添加 `class` 關鍵字來限制協議只能被類類型遵循，而結構體或枚舉不能遵循該協議。`class` 關鍵字必須第一個出現在協議的繼承列表中，在其他繼承的協議之前：

```swift
protocol SomeClassOnlyProtocol: class, SomeInheritedProtocol {
    // 這裡是類類型專屬協議的定義部分
}
```

在以上例子中，協議 `SomeClassOnlyProtocol` 只能被類類型遵循。如果嘗試讓結構體或枚舉類型遵循該協議，則會導致編譯錯誤。

> 注意  
> 當協議定義的要求需要遵循協議的類型必須是引用語義而非值語義時，應該采用類類型專屬協議。關於引用語義和值語義的更多內容，請查看[結構體和枚舉是值類型](./09_Classes_and_Structures.html#structures_and_enumerations_are_value_types)和[類是引用類型](./09_Classes_and_Structures.html#classes_are_reference_types)。

<a name="protocol_composition"></a>
## 協議合成

有時候需要同時遵循多個協議，你可以將多個協議采用 `SomeProtocol & AnotherProtocol` 這樣的格式進行組合，稱為 *協議合成（protocol composition）*。你可以羅列任意多個你想要遵循的協議，以與符號(`&`)分隔。

下面的例子中，將 `Named` 和 `Aged` 兩個協議按照上述語法組合成一個協議，作為函數參數的類型：

```swift
protocol Named {
    var name: String { get }
}
protocol Aged {
    var age: Int { get }
}
struct Person: Named, Aged {
    var name: String
    var age: Int
}
func wishHappyBirthday(to celebrator: Named & Aged) {
    print("Happy birthday, \(celebrator.name), you're \(celebrator.age)!")
}
let birthdayPerson = Person(name: "Malcolm", age: 21)
wishHappyBirthday(to: birthdayPerson)
// 打印 「Happy birthday Malcolm - you're 21!」
```

`Named` 協議包含 `String` 類型的 `name` 屬性。`Aged` 協議包含 `Int` 類型的 `age` 屬性。`Person` 結構體遵循了這兩個協議。

`wishHappyBirthday(to:)` 函數的參數 `celebrator` 的類型為 `Named & Aged`。這意味著它不關心參數的具體類型，只要參數符合這兩個協議即可。

上面的例子創建了一個名為 `birthdayPerson` 的 `Person` 的實例，作為參數傳遞給了 `wishHappyBirthday(to:)` 函數。因為 `Person` 同時符合這兩個協議，所以這個參數合法，函數將打印生日問候語。

> 注意  
> 協議合成並不會生成新的、永久的協議類型，而是將多個協議中的要求合成到一個只在局部作用域有效的臨時協議中。

<a name="checking_for_protocol_conformance"></a>
## 檢查協議一致性

你可以使用[類型轉換](./20_Type_Casting.html)中描述的 `is` 和 `as` 操作符來檢查協議一致性，即是否符合某協議，並且可以轉換到指定的協議類型。檢查和轉換到某個協議類型在語法上和類型的檢查和轉換完全相同：

* `is` 用來檢查實例是否符合某個協議，若符合則返回 `true`，否則返回 `false`。
* `as?` 返回一個可選值，當實例符合某個協議時，返回類型為協議類型的可選值，否則返回 `nil`。
* `as!` 將實例強制向下轉換到某個協議類型，如果強轉失敗，會引發運行時錯誤。

下面的例子定義了一個 `HasArea` 協議，該協議定義了一個 `Double` 類型的可讀屬性 `area`：

```swift
protocol HasArea {
    var area: Double { get }
}
```

如下所示，`Circle` 類和 `Country` 類都遵循了 `HasArea` 協議：

```swift
class Circle: HasArea {
    let pi = 3.1415927
    var radius: Double
    var area: Double { return pi * radius * radius }
    init(radius: Double) { self.radius = radius }
}
class Country: HasArea {
    var area: Double
    init(area: Double) { self.area = area }
}
```

`Circle` 類把 `area` 屬性實現為基於存儲型屬性 `radius` 的計算型屬性。`Country` 類則把 `area` 屬性實現為存儲型屬性。這兩個類都正確地符合了 `HasArea` 協議。

如下所示，`Animal` 是一個未遵循 `HasArea` 協議的類：

```swift
class Animal {
	var legs: Int
	init(legs: Int) { self.legs = legs }
}
```

`Circle`，`Country`，`Animal` 並沒有一個共同的基類，盡管如此，它們都是類，它們的實例都可以作為 `AnyObject` 類型的值，存儲在同一個數組中：

```swift
let objects: [AnyObject] = [
	Circle(radius: 2.0),
	Country(area: 243_610),
	Animal(legs: 4)
]
```

`objects` 數組使用字面量初始化，數組包含一個 `radius` 為 `2` 的 `Circle` 的實例，一個保存了英國國土面積的 `Country` 實例和一個 `legs` 為 `4` 的 `Animal` 實例。

如下所示，`objects` 數組可以被迭代，並對迭代出的每一個元素進行檢查，看它是否符合 `HasArea` 協議：

```swift
for object in objects {
    if let objectWithArea = object as? HasArea {
        print("Area is \(objectWithArea.area)")
    } else {
        print("Something that doesn't have an area")
    }
}
// Area is 12.5663708
// Area is 243610.0
// Something that doesn't have an area
```

當迭代出的元素符合 `HasArea` 協議時，將 `as?` 操作符返回的可選值通過可選綁定，綁定到 `objectWithArea` 常量上。`objectWithArea` 是 `HasArea` 協議類型的實例，因此 `area` 屬性可以被訪問和打印。

`objects` 數組中的元素的類型並不會因為強轉而丟失類型信息，它們仍然是 `Circle`，`Country`，`Animal` 類型。然而，當它們被賦值給 `objectWithArea` 常量時，只被視為 `HasArea` 類型，因此只有 `area` 屬性能夠被訪問。

<a name="optional_protocol_requirements"></a>
## 可選的協議要求

協議可以定義*可選要求*，遵循協議的類型可以選擇是否實現這些要求。在協議中使用 `optional` 關鍵字作為前綴來定義可選要求。可選要求用在你需要和 Objective-C 打交道的代碼中。協議和可選要求都必須帶上`@objc`屬性。標記 `@objc` 特性的協議只能被繼承自 Objective-C 類的類或者 `@objc` 類遵循，其他類以及結構體和枚舉均不能遵循這種協議。

使用可選要求時（例如，可選的方法或者屬性），它們的類型會自動變成可選的。比如，一個類型為 `(Int) -> String` 的方法會變成 `((Int) -> String)?`。需要注意的是整個函數類型是可選的，而不是函數的返回值。


協議中的可選要求可通過可選鏈式調用來使用，因為遵循協議的類型可能沒有實現這些可選要求。類似 `someOptionalMethod?(someArgument)` 這樣，你可以在可選方法名稱後加上 `?` 來調用可選方法。詳細內容可在[可選鏈式調用](./17_Optional_Chaining.html)章節中查看。

下面的例子定義了一個名為 `Counter` 的用於整數計數的類，它使用外部的數據源來提供每次的增量。數據源由 `CounterDataSource` 協議定義，包含兩個可選要求：

```swift
@objc protocol CounterDataSource {
    @objc optional func incrementForCount(count: Int) -> Int
    @objc optional var fixedIncrement: Int { get }
}
```

`CounterDataSource` 協議定義了一個可選方法 `increment(forCount:)` 和一個可選屬性 `fiexdIncrement`，它們使用了不同的方法來從數據源中獲取適當的增量值。

> 注意  
> 嚴格來講，`CounterDataSource` 協議中的方法和屬性都是可選的，因此遵循協議的類可以不實現這些要求，盡管技術上允許這樣做，不過最好不要這樣寫。

`Counter` 類含有 `CounterDataSource?` 類型的可選屬性 `dataSource`，如下所示：

```swift
class Counter {
    var count = 0
    var dataSource: CounterDataSource?
    func increment() {
        if let amount = dataSource?.incrementForCount?(count) {
            count += amount
        } else if let amount = dataSource?.fixedIncrement {
            count += amount
        }
    }
}
```

`Counter` 類使用變量屬性 `count` 來存儲當前值。該類還定義了一個 `increment` 方法，每次調用該方法的時候，將會增加 `count` 的值。

`increment()` 方法首先試圖使用 `increment(forCount:)` 方法來得到每次的增量。`increment()` 方法使用可選鏈式調用來嘗試調用 `increment(forCount:)`，並將當前的 `count` 值作為參數傳入。

這裡使用了兩層可選鏈式調用。首先，由於 `dataSource` 可能為 `nil`，因此在 `dataSource` 後邊加上了 `?`，以此表明只在 `dataSource` 非空時才去調用 `increment(forCount:)` 方法。其次，即使 `dataSource` 存在，也無法保證其是否實現了 `increment(forCount:)` 方法，因為這個方法是可選的。因此，`increment(forCount:)` 方法同樣使用可選鏈式調用進行調用，只有在該方法被實現的情況下才能調用它，所以在 `increment(forCount:)` 方法後邊也加上了 `?`。


調用 `increment(forCount:)` 方法在上述兩種情形下都有可能失敗，所以返回值為 `Int?` 類型。雖然在 `CounterDataSource` 協議中，`increment(forCount:)` 的返回值類型是非可選 `Int`。另外，即使這裡使用了兩層可選鏈式調用，最後的返回結果依舊是單層的可選類型。關於這一點的更多信息，請查閱[連接多層可選鏈式調用](./17_Optional_Chaining)

在調用 `increment(forCount:)` 方法後，`Int?` 型的返回值通過可選綁定解包並賦值給常量 `amount`。如果可選值確實包含一個數值，也就是說，數據源和方法都存在，數據源方法返回了一個有效值。之後便將解包後的 `amount` 加到 `count` 上，增量操作完成。

如果沒有從 `increment(forCount:)` 方法獲取到值，可能由於 `dataSource` 為 `nil`，或者它並沒有實現 `increment(forCount:)` 方法，那麼 `increment()` 方法將試圖從數據源的 `fixedIncrement` 屬性中獲取增量。`fixedIncrement` 是一個可選屬性，因此屬性值是一個 `Int?` 值，即使該屬性在 `CounterDataSource` 協議中的類型是非可選的 `Int`。

下面的例子展示了 `CounterDataSource` 的簡單實現。`ThreeSource` 類遵循了 `CounterDataSource` 協議，它實現了可選屬性 `fixedIncrement`，每次會返回 `3`：

```swift
class ThreeSource: NSObject, CounterDataSource {
    let fixedIncrement = 3
}
```

可以使用 `ThreeSource` 的實例作為 `Counter` 實例的數據源：

```swift
var counter = Counter()
counter.dataSource = ThreeSource()
for _ in 1...4 {
    counter.increment()
    print(counter.count)
}
// 3
// 6
// 9
// 12
```

上述代碼新建了一個 `Counter` 實例，並將它的數據源設置為一個 `ThreeSource` 的實例，然後調用 `increment()` 方法四次。和預期一樣，每次調用都會將 `count` 的值增加 `3`.

下面是一個更為復雜的數據源 `TowardsZeroSource`，它將使得最後的值變為 `0`：

```swift
@objc class TowardsZeroSource: NSObject, CounterDataSource {
    func increment(forCount count: Int) -> Int {
        if count == 0 {
            return 0
        } else if count < 0 {
            return 1
        } else {
            return -1
        }
    }
}
```

`TowardsZeroSource` 實現了 `CounterDataSource` 協議中的 `increment(forCount:) ` 方法，以 `count` 參數為依據，計算出每次的增量。如果 `count` 已經為 `0`，此方法返回 `0`，以此表明之後不應再有增量操作發生。

你可以使用 `TowardsZeroSource` 實例將 `Counter` 實例來從 `-4` 增加到 `0`。一旦增加到 `0`，數值便不會再有變動：

```swift
counter.count = -4
counter.dataSource = TowardsZeroSource()
for _ in 1...5 {
    counter.increment()
    print(counter.count)
}
// -3
// -2
// -1
// 0
// 0
```

<a name="protocol_extensions"></a>
## 協議擴展

協議可以通過擴展來為遵循協議的類型提供屬性、方法以及下標的實現。通過這種方式，你可以基於協議本身來實現這些功能，而無需在每個遵循協議的類型中都重復同樣的實現，也無需使用全局函數。

例如，可以擴展 `RandomNumberGenerator` 協議來提供 `randomBool()` 方法。該方法使用協議中定義的 `random()` 方法來返回一個隨機的 `Bool` 值：

```swift
extension RandomNumberGenerator {
    func randomBool() -> Bool {
        return random() > 0.5
    }
}
```

通過協議擴展，所有遵循協議的類型，都能自動獲得這個擴展所增加的方法實現，無需任何額外修改：

```swift
let generator = LinearCongruentialGenerator()
print("Here's a random number: \(generator.random())")
// 打印 「Here's a random number: 0.37464991998171」
print("And here's a random Boolean: \(generator.randomBool())")
// 打印 「And here's a random Boolean: true」
```

<a name="providing_default_implementations"></a>
### 提供默認實現

可以通過協議擴展來為協議要求的屬性、方法以及下標提供默認的實現。如果遵循協議的類型為這些要求提供了自己的實現，那麼這些自定義實現將會替代擴展中的默認實現被使用。

> 注意  
> 通過協議擴展為協議要求提供的默認實現和可選的協議要求不同。雖然在這兩種情況下，遵循協議的類型都無需自己實現這些要求，但是通過擴展提供的默認實現可以直接調用，而無需使用可選鏈式調用。

例如，`PrettyTextRepresentable` 協議繼承自 `TextRepresentable` 協議，可以為其提供一個默認的 `prettyTextualDescription` 屬性，只是簡單地返回 `textualDescription` 屬性的值：

```swift
extension PrettyTextRepresentable  {
    var prettyTextualDescription: String {
        return textualDescription
    }
}
```

<a name="adding_constraints_to_protocol_extensions"></a>
### 為協議擴展添加限制條件

在擴展協議的時候，可以指定一些限制條件，只有遵循協議的類型滿足這些限制條件時，才能獲得協議擴展提供的默認實現。這些限制條件寫在協議名之後，使用 `where` 子句來描述，正如[Where子句](./23_Generics.html#where_clauses)中所描述的。

例如，你可以擴展 `CollectionType` 協議，但是只適用於集合中的元素遵循了 `TextRepresentable` 協議的情況：

```swift
extension Collection where Iterator.Element: TextRepresentable {
	var textualDescription: String {
		let itemsAsText = self.map { $0.textualDescription }
		return "[" + itemsAsText.joined(separator: ", ") + "]"
	}
}
```

`textualDescription` 屬性返回整個集合的文本描述，它將集合中的每個元素的文本描述以逗號分隔的方式連接起來，包在一對方括號中。

現在我們來看看先前的 `Hamster` 結構體，它符合 `TextRepresentable` 協議，同時這裡還有個裝有 `Hamster` 的實例的數組：

```swift
let murrayTheHamster = Hamster(name: "Murray")
let morganTheHamster = Hamster(name: "Morgan")
let mauriceTheHamster = Hamster(name: "Maurice")
let hamsters = [murrayTheHamster, morganTheHamster, mauriceTheHamster]
```

因為 `Array` 符合 `CollectionType` 協議，而數組中的元素又符合 `TextRepresentable` 協議，所以數組可以使用 `textualDescription` 屬性得到數組內容的文本表示：

```swift
print(hamsters.textualDescription)
// 打印 「[A hamster named Murray, A hamster named Morgan, A hamster named Maurice]」
```

> 注意  
> 如果多個協議擴展都為同一個協議要求提供了默認實現，而遵循協議的類型又同時滿足這些協議擴展的限制條件，那麼將會使用限制條件最多的那個協議擴展提供的默認實現。
