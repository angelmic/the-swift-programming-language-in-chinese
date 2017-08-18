# 訪問控制
------------------

> 1.0
> 翻譯：[JaceFu](http://www.devtalking.com/)
> 校對：[ChildhoodAndy](http://childhood.logdown.com)

> 2.0
> 翻譯+校對：[mmoaay](https://github.com/mmoaay)

> 2.1
> 翻譯：[Prayer](https://github.com/futantan)
> 校對：[shanks](http://codebuild.me)，2015-11-01
> 
> 2.2 
> 翻譯+校對：[SketchK](https://github.com/SketchK) 2016-05-17   
> 3.0.1， shanks，2016-11-13

本頁內容包括：

- [模塊和源文件](#modules_and_source_files)
- [訪問級別](#access_levels)
- [訪問控制語法](#access_control_syntax)
- [自定義類型](#custom_types)
- [子類](#subclassing)
- [常量、變量、屬性、下標](#constants_variables_properties_subscripts)
- [構造器](#initializers)
- [協議](#protocols)
- [擴展](#extensions)
- [泛型](#generics)
- [類型別名](#type_aliases)

*訪問控制*可以限定其他源文件或模塊中的代碼對你的代碼的訪問級別。這個特性可以讓我們隱藏代碼的一些實現細節，並且可以為其他人可以訪問和使用的代碼提供接口。

你可以明確地給單個類型（類、結構體、枚舉）設置訪問級別，也可以給這些類型的屬性、方法、構造器、下標等設置訪問級別。協議也可以被限定在一定的范圍內使用，包括協議裡的全局常量、變量和函數。

Swift 不僅提供了多種不同的訪問級別，還為某些典型場景提供了默認的訪問級別，這樣就不需要我們在每段代碼中都申明顯式訪問級別。其實，如果只是開發一個單一目標的應用程序，我們完全可以不用顯式聲明代碼的訪問級別。

> 注意  
為了簡單起見，對於代碼中可以設置訪問級別的特性（屬性、基本類型、函數等），在下面的章節中我們會稱之為「實體」。

<a name="modules_and_source_files"></a>
## 模塊和源文件
Swift 中的訪問控制模型基於模塊和源文件這兩個概念。

模塊指的是獨立的代碼單元，框架或應用程序會作為一個獨立的模塊來構建和發布。在 Swift 中，一個模塊可以使用 `import` 關鍵字導入另外一個模塊。

在 Swift 中，Xcode 的每個目標（例如框架或應用程序）都被當作獨立的模塊處理。如果你是為了實現某個通用的功能，或者是為了封裝一些常用方法而將代碼打包成獨立的框架，這個框架就是 Swift 中的一個模塊。當它被導入到某個應用程序或者其他框架時，框架內容都將屬於這個獨立的模塊。  

*源文件*就是 Swift 中的源代碼文件，它通常屬於一個模塊，即一個應用程序或者框架。盡管我們一般會將不同的類型分別定義在不同的源文件中，但是同一個源文件也可以包含多個類型、函數之類的定義。

<a name="access_levels"></a>
## 訪問級別
Swift 為代碼中的實體提供了五種不同的*訪問級別*。這些訪問級別不僅與源文件中定義的實體相關，同時也與源文件所屬的模塊相關。

- *開放訪問*和*公開訪問*可以訪問同一模塊源文件中的任何實體，在模塊外也可以通過導入該模塊來訪問源文件裡的所有實體。通常情況下，框架中的某個接口可以被任何人使用時，你可以將其設置為開放或者公開訪問。
- *內部訪問*可以訪問同一模塊源文件中的任何實體，但是不能從模塊外訪問該模塊源文件中的實體。通常情況下，某個接口只在應用程序或框架內部使用時，你可以將其設置為內部訪問。
- *文件私有訪問*限制實體只能被所定義的文件內部訪問。當需要把這些細節被整個文件使用的時候，使用文件私有訪問隱藏了一些特定功能的實現細節。
- *私有訪問*限制實體只能在所定義的作用域內使用。需要把這些細節被整個作用域使用的時候，使用文件私有訪問隱藏了一些特定功能的實現細節。

開放訪問為最高（限制最少）訪問級別，私有訪問為最低（限制最多）訪問級別。

開放訪問只作用於類類型和類的成員，它和公開訪問的區別如下：

* 公開訪問或者其他更嚴訪問級別的類，只能在它們定義的模塊內部被繼承。
* 公開訪問或者其他更嚴訪問級別的類成員，只能在它們定義的模塊內部的子類中重寫。
* 開放訪問的類，可以在它們定義的模塊中被繼承，也可以在引用它們的模塊中被繼承。
* 開放訪問的類成員，可以在它們定義的模塊中子類中重寫，也可以在引用它們的模塊中的子類重寫。
* 
把一個類標記為開放，顯式地表明，你認為其他模塊中的代碼使用此類作為父類，然後你已經設計好了你的類的代碼了。

<a name="guiding_principle_of_access_levels"></a>
### 訪問級別基本原則

Swift 中的訪問級別遵循一個基本原則：*不可以在某個實體中定義訪問級別更低（更嚴格）的實體*。

例如：

- 一個公開訪問級別的變量，其類型的訪問級別不能是內部，文件私有或是私有類型的。因為無法保證變量的類型在使用變量的地方也具有訪問權限。
- 函數的訪問級別不能高於它的參數類型和返回類型的訪問級別。因為這樣就會出現函數可以在任何地方被訪問，但是它的參數類型和返回類型卻不可以的情況。

關於此原則的各種情況的具體實現，將在下面的細節中體現。

<a name="default_access_levels"></a>
### 默認訪問級別

如果你不為代碼中的實體顯式指定訪問級別，那麼它們默認為 `internal` 級別（有一些例外情況，稍後會進行說明）。因此，在大多數情況下，我們不需要顯式指定實體的訪問級別。

<a name="access_levels_for_single-target_apps"></a>
### 單目標應用程序的訪問級別

當你編寫一個單目標應用程序時，應用的所有功能都是為該應用服務，而不需要提供給其他應用或者模塊使用，所以我們不需要明確設置訪問級別，使用默認的訪問級別 `internal` 即可。但是，你也可以使用文件私有訪問或私有訪問級別，用於隱藏一些功能的實現細節。

<a name="access_levels_for_frameworks"></a>
### 框架的訪問級別

當你開發框架時，就需要把一些對外的接口定義為開放訪問或公開訪問級別，以便使用者導入該框架後可以正常使用其功能。這些被你定義為對外的接口，就是這個框架的 API。

> 注意  
框架依然會使用默認的內部訪問級別，也可以指定為文件私有訪問或者私有訪問級別。當你想把某個實體作為框架的 API 的時候，需顯式為其指定開放訪問或公開訪問級別。

<a name="access_levels_for_unit_test_targets"></a>
### 單元測試目標的訪問級別

當你的應用程序包含單元測試目標時，為了測試，測試模塊需要訪問應用程序模塊中的代碼。默認情況下只有開放訪問或公開訪問級別級別的實體才可以被其他模塊訪問。然而，如果在導入應用程序模塊的語句前使用 `@testable` 特性，然後在允許測試的編譯設置（`Build Options -> Enable Testability`）下編譯這個應用程序模塊，單元測試目標就可以訪問應用程序模塊中所有內部級別的實體。

<a name="access_control_syntax"></a>
## 訪問控制語法

通過修飾符 `open`，`public`，`internal`，`fileprivate`，`private` 來聲明實體的訪問級別：

```swift
public class SomePublicClass {}
internal class SomeInternalClass {}
fileprivate class SomeFilePrivateClass {}
private class SomePrivateClass {}
 
public var somePublicVariable = 0
internal let someInternalConstant = 0
fileprivate func someFilePrivateFunction() {}
private func somePrivateFunction() {}
```

除非專門指定，否則實體默認的訪問級別為內部訪問級別，可以查閱[默認訪問級別](#default_access_levels)這一節。這意味著在不使用修飾符顯式聲明訪問級別的情況下，`SomeInternalClass` 和 `someInternalConstant` 仍然擁有隱式的內部訪問級別：

```swift
class SomeInternalClass {}   // 隱式內部訪問級別
var someInternalConstant = 0 // 隱式內部訪問級別
```

<a name="custom_types"></a>
## 自定義類型

如果想為一個自定義類型指定訪問級別，在定義類型時進行指定即可。新類型只能在它的訪問級別限制范圍內使用。例如，你定義了一個文件私有級別的類，那這個類就只能在定義它的源文件中使用，可以作為屬性類型、函數參數類型或者返回類型，等等。

一個類型的訪問級別也會影響到類型*成員*（屬性、方法、構造器、下標）的默認訪問級別。如果你將類型指定為私有或者文件私有級別，那麼該類型的所有成員的默認訪問級別也會變成私有或者文件私有級別。如果你將類型指定為公開或者內部訪問級別（或者不明確指定訪問級別，而使用默認的內部訪問級別），那麼該類型的所有成員的默認訪問級別將是內部訪問。

> 重要    
上面提到，一個公開類型的所有成員的訪問級別默認為內部訪問級別，而不是公開級別。如果你想將某個成員指定為公開訪問級別，那麼你必須顯式指定。這樣做的好處是，在你定義公共接口的時候，可以明確地選擇哪些接口是需要公開的，哪些是內部使用的，避免不小心將內部使用的接口公開。

```swift
public class SomePublicClass {                  // 顯式公開類
    public var somePublicProperty = 0            // 顯式公開類成員
    var someInternalProperty = 0                 // 隱式內部類成員
    fileprivate func someFilePrivateMethod() {}  // 顯式文件私有類成員
    private func somePrivateMethod() {}          // 顯式私有類成員
}
 
class SomeInternalClass {                       // 隱式內部類
    var someInternalProperty = 0                 // 隱式內部類成員
    fileprivate func someFilePrivateMethod() {}  // 顯式文件私有類成員
    private func somePrivateMethod() {}          // 顯式私有類成員
}
 
fileprivate class SomeFilePrivateClass {        // 顯式文件私有類
    func someFilePrivateMethod() {}              // 隱式文件私有類成員
    private func somePrivateMethod() {}          // 顯式私有類成員
}
 
private class SomePrivateClass {                // 顯式私有類
    func somePrivateMethod() {}                  // 隱式私有類成員
}

```
<a name="tuple_types"></a>
### 元組類型

元組的訪問級別將由元組中訪問級別最嚴格的類型來決定。例如，如果你構建了一個包含兩種不同類型的元組，其中一個類型為內部訪問級別，另一個類型為私有訪問級別，那麼這個元組的訪問級別為私有訪問級別。

> 注意  
元組不同於類、結構體、枚舉、函數那樣有單獨的定義。元組的訪問級別是在它被使用時自動推斷出的，而無法明確指定。

<a name="function_types"></a>
### 函數類型

函數的訪問級別根據訪問級別最嚴格的參數類型或返回類型的訪問級別來決定。但是，如果這種訪問級別不符合函數定義所在環境的默認訪問級別，那麼就需要明確地指定該函數的訪問級別。

下面的例子定義了一個名為 `someFunction()` 的全局函數，並且沒有明確地指定其訪問級別。也許你會認為該函數應該擁有默認的訪問級別 `internal`，但事實並非如此。事實上，如果按下面這種寫法，代碼將無法通過編譯：

```swift
func someFunction() -> (SomeInternalClass, SomePrivateClass) {
   	// 此處是函數實現部分
}
```

我們可以看到，這個函數的返回類型是一個元組，該元組中包含兩個自定義的類（可查閱[自定義類型](#custom_types)）。其中一個類的訪問級別是 `internal`，另一個的訪問級別是 `private`，所以根據元組訪問級別的原則，該元組的訪問級別是 `private`（元組的訪問級別與元組中訪問級別最低的類型一致）。

因為該函數返回類型的訪問級別是 `private`，所以你必須使用 `private` 修飾符，明確指定該函數的訪問級別：

```swift
private func someFunction() -> (SomeInternalClass, SomePrivateClass) {
   	// 此處是函數實現部分
}
```

將該函數指定為 `public` 或 `internal`，或者使用默認的訪問級別 `internal` 都是錯誤的，因為如果把該函數當做 `public` 或 `internal` 級別來使用的話，可能會無法訪問 `private` 級別的返回值。

<a name="enumeration_types"></a>
### 枚舉類型

枚舉成員的訪問級別和該枚舉類型相同，你不能為枚舉成員單獨指定不同的訪問級別。

比如下面的例子，枚舉 `CompassPoint` 被明確指定為 `public` 級別，那麼它的成員 `North`、`South`、`East`、`West` 的訪問級別同樣也是 `public`：

```swift
public enum CompassPoint {
   	case North
   	case South
   	case East
   	case West
}
```

<a name="raw_values_and_associated_values"></a>
#### 原始值和關聯值

枚舉定義中的任何原始值或關聯值的類型的訪問級別至少不能低於枚舉類型的訪問級別。例如，你不能在一個 `internal` 訪問級別的枚舉中定義 `private` 級別的原始值類型。

<a name="nested_types"></a>
### 嵌套類型

如果在 `private` 級別的類型中定義嵌套類型，那麼該嵌套類型就自動擁有 `private` 訪問級別。如果在 `public` 或者 `internal` 級別的類型中定義嵌套類型，那麼該嵌套類型自動擁有 `internal` 訪問級別。如果想讓嵌套類型擁有 `public` 訪問級別，那麼需要明確指定該嵌套類型的訪問級別。

<a name="subclassing"></a>
## 子類

子類的訪問級別不得高於父類的訪問級別。例如，父類的訪問級別是 `internal`，子類的訪問級別就不能是 `public`。

此外，你可以在符合當前訪問級別的條件下重寫任意類成員（方法、屬性、構造器、下標等）。

可以通過重寫為繼承來的類成員提供更高的訪問級別。下面的例子中，類 `A` 的訪問級別是 `public`，它包含一個方法 `someMethod()`，訪問級別為 `private`。類 `B` 繼承自類 `A`，訪問級別為 `internal`，但是在類 `B` 中重寫了類 `A` 中訪問級別為 `private` 的方法 `someMethod()`，並重新指定為 `internal` 級別。通過這種方式，我們就可以將某類中 `private` 級別的類成員重新指定為更高的訪問級別，以便其他人使用：

```swift
public class A {
   	private func someMethod() {}
}

internal class B: A {
   	override internal func someMethod() {}
}
```

我們甚至可以在子類中，用子類成員去訪問訪問級別更低的父類成員，只要這一操作在相應訪問級別的限制范圍內（也就是說，在同一源文件中訪問父類 `private` 級別的成員，在同一模塊內訪問父類 `internal` 級別的成員）：

```swift
public class A {
    private func someMethod() {}
}

internal class B: A {
    override internal func someMethod() {
        super.someMethod()
    }
}
```

因為父類 `A` 和子類 `B` 定義在同一個源文件中，所以在子類 `B` 可以在重寫的 `someMethod()` 方法中調用 `super.someMethod()`。

<a name="constants_variables_properties_subscripts"></a>
## 常量、變量、屬性、下標

常量、變量、屬性不能擁有比它們的類型更高的訪問級別。例如，你不能定義一個 `public` 級別的屬性，但是它的類型卻是 `private` 級別的。同樣，下標也不能擁有比索引類型或返回類型更高的訪問級別。  

如果常量、變量、屬性、下標的類型是 `private` 級別的，那麼它們必須明確指定訪問級別為 `private`：

```swift
private var privateInstance = SomePrivateClass()
```

<a name="getters_and_setters"></a>
### Getter 和 Setter

常量、變量、屬性、下標的 `Getters` 和 `Setters` 的訪問級別和它們所屬類型的訪問級別相同。

`Setter` 的訪問級別可以低於對應的 `Getter` 的訪問級別，這樣就可以控制變量、屬性或下標的讀寫權限。在 `var` 或 `subscript` 關鍵字之前，你可以通過 `fileprivate(set)`，`private(set)` 或 `internal(set)` 為它們的寫入權限指定更低的訪問級別。

> 注意  
這個規則同時適用於存儲型屬性和計算型屬性。即使你不明確指定存儲型屬性的 `Getter` 和 `Setter`，Swift 也會隱式地為其創建 `Getter` 和 `Setter`，用於訪問該屬性的後備存儲。使用 `fileprivate(set)`，`private(set)` 和 `internal(set)` 可以改變 `Setter` 的訪問級別，這對計算型屬性也同樣適用。  

下面的例子中定義了一個名為 `TrackedString` 的結構體，它記錄了 `value` 屬性被修改的次數：

```swift
struct TrackedString {
    private(set) var numberOfEdits = 0
    var value: String = "" {
        didSet {
            numberOfEdits += 1
        }
    }
}
```

`TrackedString` 結構體定義了一個用於存儲 `String` 值的屬性 `value`，並將初始值設為 `""`（一個空字符串）。該結構體還定義了另一個用於存儲 `Int` 值的屬性 `numberOfEdits`，它用於記錄屬性 `value` 被修改的次數。這個功能通過屬性 `value` 的 `didSet` 觀察器實現，每當給 `value` 賦新值時就會調用 `didSet` 方法，然後將 `numberOfEdits` 的值加一。

結構體 `TrackedString` 和它的屬性 `value` 均沒有顯式指定訪問級別，所以它們都擁有默認的訪問級別 `internal`。但是該結構體的 `numberOfEdits` 屬性使用了 `private(set)` 修飾符，這意味著 `numberOfEdits` 屬性只能在定義該結構體的源文件中賦值。`numberOfEdits` 屬性的 `Getter` 依然是默認的訪問級別 `internal`，但是 `Setter` 的訪問級別是 `private`，這表示該屬性只有在當前的源文件中是可讀寫的，而在當前源文件所屬的模塊中只是一個可讀的屬性。  

如果你實例化 `TrackedString` 結構體，並多次對 `value` 屬性的值進行修改，你就會看到 `numberOfEdits` 的值會隨著修改次數而變化：

```swift
var stringToEdit = TrackedString()
stringToEdit.value = "This string will be tracked."
stringToEdit.value += " This edit will increment numberOfEdits."
stringToEdit.value += " So will this one."
print("The number of edits is \(stringToEdit.numberOfEdits)")
// 打印 「The number of edits is 3」
```

雖然你可以在其他的源文件中實例化該結構體並且獲取到 `numberOfEdits` 屬性的值，但是你不能對其進行賦值。這一限制保護了該記錄功能的實現細節，同時還提供了方便的訪問方式。

你可以在必要時為 `Getter` 和 `Setter` 顯式指定訪問級別。下面的例子將 `TrackedString` 結構體明確指定為了 `public` 訪問級別。結構體的成員（包括 `numberOfEdits` 屬性）擁有默認的訪問級別 `internal`。你可以結合 `public` 和 `private(set)` 修飾符把結構體中的 `numberOfEdits` 屬性的 `Getter` 的訪問級別設置為 `public`，而 `Setter` 的訪問級別設置為 `private`：

```swift
public struct TrackedString {
    public private(set) var numberOfEdits = 0
    public var value: String = "" {
        didSet {
            numberOfEdits += 1
        }
    }
    public init() {}
}
```

<a name="initializers"></a>
## 構造器

自定義構造器的訪問級別可以低於或等於其所屬類型的訪問級別。唯一的例外是[必要構造器](./14_Initialization.html#required_initializers)，它的訪問級別必須和所屬類型的訪問級別相同。

如同函數或方法的參數，構造器參數的訪問級別也不能低於構造器本身的訪問級別。

<a name="default_initializers"></a>
### 默認構造器

如[默認構造器](./14_Initialization.html#default_initializers)所述，Swift 會為結構體和類提供一個默認的無參數的構造器，只要它們為所有存儲型屬性設置了默認初始值，並且未提供自定義的構造器。

默認構造器的訪問級別與所屬類型的訪問級別相同，除非類型的訪問級別是 `public`。如果一個類型被指定為 `public` 級別，那麼默認構造器的訪問級別將為 `internal`。如果你希望一個 `public` 級別的類型也能在其他模塊中使用這種無參數的默認構造器，你只能自己提供一個 `public` 訪問級別的無參數構造器。

<a name="default_memberwise_initializers_for_structure_types"></a>
### 結構體默認的成員逐一構造器

如果結構體中任意存儲型屬性的訪問級別為 `private`，那麼該結構體默認的成員逐一構造器的訪問級別就是 `private`。否則，這種構造器的訪問級別依然是 `internal`。

如同前面提到的默認構造器，如果你希望一個 `public` 級別的結構體也能在其他模塊中使用其默認的成員逐一構造器，你依然只能自己提供一個 `public` 訪問級別的成員逐一構造器。

<a name="protocols"></a>
## 協議

如果想為一個協議類型明確地指定訪問級別，在定義協議時指定即可。這將限制該協議只能在適當的訪問級別范圍內被采納。

協議中的每一個要求都具有和該協議相同的訪問級別。你不能將協議中的要求設置為其他訪問級別。這樣才能確保該協議的所有要求對於任意采納者都將可用。

> 注意  
如果你定義了一個 `public` 訪問級別的協議，那麼該協議的所有實現也會是 `public` 訪問級別。這一點不同於其他類型，例如，當類型是 `public` 訪問級別時，其成員的訪問級別卻只是 `internal`。

<a name="protocol_inheritance"></a>
### 協議繼承

如果定義了一個繼承自其他協議的新協議，那麼新協議擁有的訪問級別最高也只能和被繼承協議的訪問級別相同。例如，你不能將繼承自 `internal` 協議的新協議定義為 `public` 協議。

<a name="protocol_conformance"></a>
### 協議一致性

一個類型可以采納比自身訪問級別低的協議。例如，你可以定義一個 `public` 級別的類型，它可以在其他模塊中使用，同時它也可以采納一個 `internal` 級別的協議，但是只能在該協議所在的模塊中作為符合該協議的類型使用。

采納了協議的類型的訪問級別取它本身和所采納協議兩者間最低的訪問級別。也就是說如果一個類型是 `public` 級別，采納的協議是 `internal` 級別，那麼采納了這個協議後，該類型作為符合協議的類型時，其訪問級別也是 `internal`。

如果你采納了協議，那麼實現了協議的所有要求後，你必須確保這些實現的訪問級別不能低於協議的訪問級別。例如，一個 `public` 級別的類型，采納了 `internal` 級別的協議，那麼協議的實現至少也得是 `internal` 級別。

> 注意  
Swift 和 Objective-C 一樣，協議的一致性是全局的，也就是說，在同一程序中，一個類型不可能用兩種不同的方式實現同一個協議。

<a name="extensions"></a>
## 擴展

你可以在訪問級別允許的情況下對類、結構體、枚舉進行擴展。擴展成員具有和原始類型成員一致的訪問級別。例如，你擴展了一個 `public` 或者 `internal` 類型，擴展中的成員具有默認的 `internal` 訪問級別，和原始類型中的成員一致 。如果你擴展了一個 `private` 類型，擴展成員則擁有默認的 `private` 訪問級別。

或者，你可以明確指定擴展的訪問級別（例如，`private extension`），從而給該擴展中的所有成員指定一個新的默認訪問級別。這個新的默認訪問級別仍然可以被單獨指定的訪問級別所覆蓋。

<a name="adding_protocol_conformance_with_an_extension"></a>
### 通過擴展添加協議一致性

如果你通過擴展來采納協議，那麼你就不能顯式指定該擴展的訪問級別了。協議擁有相應的訪問級別，並會為該擴展中所有協議要求的實現提供默認的訪問級別。

<a name="generics"></a>
## 泛型

泛型類型或泛型函數的訪問級別取決於泛型類型或泛型函數本身的訪問級別，還需結合類型參數的類型約束的訪問級別，根據這些訪問級別中的最低訪問級別來確定。

<a name="type_aliases"></a>
## 類型別名

你定義的任何類型別名都會被當作不同的類型，以便於進行訪問控制。類型別名的訪問級別不可高於其表示的類型的訪問級別。例如，`private` 級別的類型別名可以作為 `private`，`file-private`，`internal`，`public`或者`open`類型的別名，但是 `public` 級別的類型別名只能作為 `public` 類型的別名，不能作為 `internal`，`file-private`，或 `private` 類型的別名。

> 注意  
這條規則也適用於為滿足協議一致性而將類型別名用於關聯類型的情況。
