# 特性（Attributes）
-----------------

> 1.0
> 翻譯：[Hawstein](https://github.com/Hawstein)
> 校對：[numbbbbb](https://github.com/numbbbbb), [stanzhai](https://github.com/stanzhai)

> 2.0
> 翻譯+校對：[KYawn](https://github.com/KYawn)

> 2.1
> 翻譯：[小鐵匠Linus](https://github.com/kevin833752)

本頁內容包括：

- [聲明特性](#declaration_attributes)
- [Interface Builder 使用的聲明特性](#declaration_attributes_used_by_interface_builder)
- [類型特性](#type_attributes)

特性提供了有關聲明和類型的更多信息。在Swift中有兩種特性，分別用於修飾聲明和類型。

您可以通過以下方式指定一個特性:符號`@`後跟特性的名稱和特性接收的任何參數：

> @ `特性名`

> @ `特性名`（`特性參數`）

有些聲明特性通過接收參數來指定特性的更多信息以及它是如何修飾某個特定的聲明的。這些特性的參數寫在圓括號內，它們的格式由它們所屬的特性來定義。

<a name="declaration_attributes"></a>
##聲明特性
聲明特性只能應用於聲明。

`available`

將 `available` 特性用於聲明時，表示該聲明的生命周期與特定的平台和操作系統版本有關。

`available` 特性經常與參數列表一同出現，該參數列表至少有兩個特性參數，參數之間由逗號分隔。這些參數由以下這些平台名字中的一個起頭：

- iOS
- iOSApplicationExtension
- macOS
- macOSApplicationExtension
- watchOS
- watchOSApplicationExtension
- tvOS
- tvOSApplicationExtension

當然，你也可以用一個星號（*）來表示上面提到的所有平台。
其余的參數，可以按照任何順序出現，並且可以添加關於聲明生命周期的附加信息，包括重要事件。

- `unavailable`參數表示該聲明在指定的平台上是無效的。
- `introduced` 參數表示指定平台從哪一版本開始引入該聲明。格式如下：

`introduced`=`版本號`

*版本號*由一個或多個正整數構成，由句點分隔的。

- `deprecated`參數表示指定平台從哪一版本開始棄用該聲明。格式如下：

`deprecated`=`版本號`

可選的*版本號*由一個或多個正整數構成，由句點分隔的。省略版本號表示該聲明目前已棄用，當棄用出現時無需給出任何有關信息。如果你省略了版本號，冒號（:）也可省略。

- `obsoleted` 參數表示指定平台從哪一版本開始廢棄該聲明。當一個聲明被廢棄後，它就從平台中移除，不能再被使用。格式如下：

`obsoleted`=`版本號`

*版本號*由一個或多個正整數構成，由句點分隔的。

- `message` 參數用來提供文本信息。當使用被棄用或者被廢棄的聲明時，編譯器會拋出警告或錯誤信息。格式如下：

`message`=`信息內容`

信息內容由一個字符串構成。

- `renamed` 參數用來提供文本信息，用以表示被重命名的聲明的新名字。當使用聲明的舊名字時，編譯器會報錯提示新名字。格式如下：

`renamed`=`新名字`

新名字由一個字符串構成。

你可以將`renamed` 參數和 `unavailable` 參數以及類型別名聲明組合使用，以此向用戶表示某個聲明已經被重命名。當某個聲明的名字在一個框架或者庫的不同發布版本間發生變化時，這會相當有用。

```swift
// 首發版本
protocol MyProtocol {
// 這裡是協議定義
}
```

```swift
// 後續版本重命名了 MyProtocol
protocol MyRenamedProtocol {
// 這裡是協議定義
}
@available(*, unavailable, renamed:"MyRenamedProtocol")
typealias MyProtocol = MyRenamedProtocol
```

你可以在某個聲明上使用多個 `available` 特性，以指定該聲明在不同平台上的可用性。編譯器只有在當前目標平台和 `available` 特性中指定的平台匹配時，才會使用 `available` 特性。

如果 `available` 特性除了平台名稱參數外，只指定了一個 `introduced` 參數，那麼可以使用以下簡寫語法代替：

@available（平台名稱 版本號，*）

`available` 特性的簡寫語法可以簡明地表達出聲明在多個平台上的可用性。盡管這兩種形式在功能上是相同的，但請盡可能地使用簡寫語法形式。

```swift
@available(iOS 10.0, macOS 10.12, *)
class MyClass {
// 這裡是類定義
}
```

`discardableResult`

該特性用於的函數或方法聲明,以抑制編譯器中 函數或方法的返回值被調而沒有使用其結果的警告。

`GKInspectable`

應用此屬性，暴露一個自定義GameplayKit組件屬性給SpriteKit編輯器UI。

`objc`

該特性用於修飾任何可以在 Objective-C 中表示的聲明。比如，非嵌套類、協議、非泛型枚舉（僅限原始值為整型的枚舉）、類和協議中的屬性和方法（包括存取方法）、構造器、析構器以及下標運算符。`objc` 特性告訴編譯器這個聲明可以在 Objective-C 代碼中使用。

標有 `objc` 特性的類必須繼承自 Objective-C 中定義的類。如果你將 `objc` 特性應用於一個類或協議，它也會隱式地應用於類或協議中兼容 Objective-C 的成員。對於標記了 `objc` 特性的類，編譯器會隱式地為它的子類添加 `objc` 特性。標記了 `objc` 特性的協議不能繼承沒有標記 `objc` 的協議。

如果你將 `objc` 特性應用於枚舉，每一個枚舉用例都會以枚舉名稱和用例名稱組合的方式暴露在 Objective-C 代碼中。例如，在 `Planet` 枚舉中有一個名為 `Venus` 的用例，該用例暴露在 Objective-C 代碼中時叫做 `PlanetVenus`。

`objc` 特性有一個可選的參數，由標識符構成。當你想把 objc 所修飾的實體以一個不同的名字暴露給 Objective-C 時，你就可以使用這個特性參數。你可以使用這個參數來命名類、枚舉類型、枚舉用例、協議、方法、存取方法以及構造器。下面的例子把 `ExampleClass` 中的 `enabled` 屬性的取值方法暴露給 Objective-C，名字是 `isEnabled`，而不是它原來的屬性名。

```swift
@objc
class ExampleClass: NSObject {
var enabled: Bool {
@objc(isEnabled) get {
// 返回適當的值        }
}
}
```
`nonobjc`

該特性用於方法、屬性、下標、或構造器的聲明，這些聲明本可以在 Objective-C 代碼中使用，而使用 `nonobjc` 特性則告訴編譯器這個聲明不能在 Objective-C 代碼中使用。

可以使用 `nonobjc` 特性解決標有 `objc` 的類中橋接方法的循環問題，該特性還允許對標有 `objc` 的類中的構造器和方法進行重載。

標有 `nonobjc` 特性的方法不能重寫標有 `objc` 特性的方法。然而，標有 `objc` 特性的方法可以重寫標有 `nonobjc` 特性的方法。同樣，標有 `nonobjc` 特性的方法不能滿足標有 `@objc` 特性的協議中的方法要求。

`NSApplicationMain`

在類上使用該特性表示該類是應用程序委托類，使用該特性與調用 `NSApplicationMain`(\_:_:) 函數並且把該類的名字作為委托類的名字傳遞給函數的效果相同。

如果你不想使用這個特性，可以提供一個 main.swift 文件，並在代碼**頂層**調用`NSApplicationMain`(\_:_:) 函數,如下所示:

```swift
import AppKit
NSApplicationMain(CommandLine.argc, CommandLine.unsafeArgv)
```
`NSCopying`

該特性用於修飾一個類的存儲型變量屬性。該特性將使屬性的設值方法使用傳入值的副本進行賦值，這個值由傳入值的 `copyWithZone`(\_:) 方法返回。該屬性的類型必需符合 `NSCopying` 協議。

`NSCopying` 特性的行為與 Objective-C 中的 `copy` 特性相似。

`NSManaged`

該特性用於修飾 `NSManagedObject` 子類中的實例方法或存儲型變量屬性，表明它們的實現由 `Core Data` 在運行時基於相關實體描述動態提供。對於標記了 `NSManaged` 特性的屬性，`Core Data` 也會在運行時為其提供存儲。應用這個特性也意味著`objc`特性。

`testable`

在導入允許測試的編譯模塊時，該特性用於修飾 `import` 聲明，這樣就能訪問被導入模塊中的任何標有 `internal` 訪問級別修飾符的實體，猶如它們被標記了 `public` 訪問級別修飾符。測試也可以訪問使用`internal`或者`public`訪問級別修飾符標記的類和類成員,就像它們是`open`訪問修飾符聲明的。

`UIApplicationMain`

在類上使用該特性表示該類是應用程序委托類，使用該特性與調用 `UIApplicationMain`函數並且把該類的名字作為委托類的名字傳遞給函數的效果相同。

如果你不想使用這個特性，可以提供一個 main.swift 文件，並在代碼頂層調用 `UIApplicationMain`(\_:\_:\_:) 函數。比如，如果你的應用程序使用一個繼承於 UIApplication 的自定義子類作為主要類，你可以調用 `UIApplicationMain`(\_:\_:\_:) 函數而不是使用該特性。

<a name="declaration_attributes_used_by_interface_builder"></a>
###Interface Builder 使用的聲明特性
`Interface Builder` 特性是 `Interface Builder` 用來與 Xcode 同步的聲明特性。`Swift` 提供了以下的 `Interface Builder` 特性：`IBAction`，`IBOutlet`，`IBDesignable`，以及`IBInspectable` 。這些特性與 Objective-C 中對應的特性在概念上是相同的。

`IBOutlet` 和 `IBInspectable` 用於修飾一個類的屬性聲明，`IBAction` 特性用於修飾一個類的方法聲明，`IBDesignable` 用於修飾類的聲明。

`IBAction` 和 `IBOutlet` 特性都意味著`objc`特性。

<a name="type_attributes"></a>
##類型特性
類型特性只能用於修飾類型。

`autoclosure`

這個特性通過把表達式自動封裝成無參數的閉包來延遲表達式的計算。它可以修飾類型為返回表達式結果類型的無參數函數類型的函數參數。關於如何使用 autoclosure 特性的例子，請參閱 [自動閉包](http://wiki.jikexueyuan.com/project/swift/chapter2/07_Closures.html/) 和 [函數類型](http://wiki.jikexueyuan.com/project/swift/chapter3/03_Types.html)。

`convention`
該特性用於修飾函數類型，它指出了函數調用的約定。

convention 特性總是與下面的參數之一一起出現。

- `swift` 參數用於表示一個 Swift 函數引用。這是 Swift 中函數值的標准調用約定。

- `block` 參數用於表示一個 Objective-C 兼容的塊引用。函數值會作為一個塊對象的引用，塊是一種 `id` 兼容的 Objective-C 對象，其中嵌入了調用函數。調用函數使用 C 的調用約定。

- `c` 參數用於表示一個 C 函數引用。函數值沒有上下文，不具備捕獲功能，同樣使用 C 的調用約定。

使用 C 函數調用約定的函數也可用作使用 Objective-C 塊調用約定的函數，同時使用 Objective-C 塊調用約定的函數也可用作使用 Swift 函數調用約定的函數。然而，只有非泛型的全局函數、局部函數以及未捕獲任何局部變量的閉包，才可以被用作使用 C 函數調用約定的函數。

`escaping`
在函數或者方法聲明上使用該特性，它表示參數將不會被存儲以供延遲執行，這將確保參數不會超出函數調用的生命周期。在使用 `escaping` 聲明特性的函數類型中訪問屬性和方法時不需要顯式地使用 `self.`。關於如何使用 `escaping` 特性的例子，請參閱 [逃逸閉包](http://wiki.jikexueyuan.com/project/swift/chapter2/07_Closures.html)。

>特性語法

> *特性 *→ @ <font color = 0x3386c8>特性名 特性參數子句</font><sub>可選</sub>

> *特性名* → <font color = 0x3386c8>標識符

> *特性參數子句* → ( <font color = 0x3386c8>均衡令牌列表</font><sub>可選</sub> )

> *特性列表* → <font color = 0x3386c8>特性 特性列表</font><sub>可選</sub>

> *均衡令牌列表* → <font color = 0x3386c8>均衡令牌 均衡令牌列表</font><sub>可選</sub>

> *均衡令牌* → ( <font color = 0x3386c8>均衡令牌列表</font><sub>可選</sub> )

> *均衡令牌* → [ <font color = 0x3386c8>均衡令牌列表</font><sub>可選</sub> ]

> *均衡令牌* → { <font color = 0x3386c8>均衡令牌列表</font><sub>可選</sub>}

> *均衡令牌* → 任意標識符，關鍵字，字面量或運算符

> *均衡令牌* → 任意標點除了 (，)，[，]，{，或 }
