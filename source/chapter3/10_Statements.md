<a name="statement_statements"></a>
# 語句（Statements）
-----------------

> 1.0
> 翻譯：[coverxit](https://github.com/coverxit)
> 校對：[numbbbbb](https://github.com/numbbbbb), [coverxit](https://github.com/coverxit), [stanzhai](https://github.com/stanzhai)

> 2.0
> 翻譯+校對：[littledogboy](https://github.com/littledogboy)

> 2.2
> 翻譯：[chenmingbiao](https://github.com/chenmingbiao)

> 3.0
> 翻譯：[chenmingjia](https://github.com/chenmingjia)

本頁包含內容：

- [循環語句](#loop_statements)
	- [For-In 語句](#for-in_statements) 
	- [While 語句](#while_statements)
	- [Repeat-While 語句](#repeat-while_statements)
- [分支語句](#branch_statements)
	- [If 語句](#if_statements) 
	- [Guard 語句](#guard_statements)
	- [Switch 語句](#switch_statements)
- [帶標簽的語句](#labeled_statements)
- [控制轉移語句](#control_transfer_statements)
	- [Break 語句](#break_statement)
	- [Continue 語句](#continue_statement)
	- [Fallthrough 語句](#fallthrough_statements)
	- [Return 語句](#return_statements)
	- [Throw 語句](#throw_statements)
- [Defer 語句](#defer_statements)
- [Do 語句](#do_statements)
- [編譯器控制語句](#compiler_control_statements)
	- [編譯配置語句](#build_config_statements) 
	- [行控制語句](#line_control_statements)
- [可用性條件](#availability_condition)

在 Swift 中，有三種類型的語句：簡單語句、編譯器控制語句和控制流語句。簡單語句是最常見的，用於構造表達式或者聲明。編譯器控制語句允許程序改變編譯器的行為，包含編譯配置語句和行控制語句。

控制流語句則用於控制程序執行的流程，Swift 中有多種類型的控制流語句：循環語句、分支語句和控制轉移語句。循環語句用於重復執行代碼塊；分支語句用於執行滿足特定條件的代碼塊；控制轉移語句則用於改變代碼的執行順序。另外，Swift 提供了 `do` 語句，用於構建局部作用域，還用於錯誤的捕獲和處理；還提供了 `defer` 語句，用於退出當前作用域之前執行清理操作。

是否將分號（`;`）添加到語句的末尾是可選的。但若要在同一行內寫多條獨立語句，則必須使用分號。

> 語句語法  
<a name="statement"></a>
> *語句* → [*表達式*](04_Expressions.md#expression) **;**<sub>可選</sub>  
> *語句* → [*聲明*](05_Declarations.md#declaration) **;**<sub>可選</sub>  
> *語句* → [*循環語句*](#loop-statement) **;**<sub>可選</sub>  
> *語句* → [*分支語句*](#branch-statement) **;**<sub>可選</sub>  
> *語句* → [*帶標簽的語句*](#labeled-statement) **;**<sub>可選</sub>  
> *語句* → [*控制轉移語句*](#control-transfer-statement) **;**<sub>可選</sub>  
> *語句* → [*defer 語句*](#defer-statement) **;**<sub>可選</sub>  
> *語句* → [*do 語句*](#do-statement) **:**<sub>可選</sub>  
> *語句* → [*編譯器控制語句*](#compiler-control-statement)  
<a name="statements"></a>
> *多條語句* → [*語句*](#statement) [*多條語句*](#statements)<sub>可選</sub>  

<a name="loop_statements"></a>
## 循環語句

循環語句會根據特定的循環條件來重復執行代碼塊。Swift 提供三種類型的循環語句：`for-in` 語句、`while` 語句和 `repeat-while` 語句。

通過 `break` 語句和 `continue` 語句可以改變循環語句的控制流。有關這兩條語句，詳情參見 [Break 語句](#break_statement) 和 [Continue 語句](#continue_statement)。

> 循環語句語法  
<a name="loop-statement"></a>
> *循環語句* → [*for-in 語句*](#for-in-statement)  
> *循環語句* → [*while 語句*](#while-statement)  
> *循環語句* → [*repeat-while 語句*](#repeat-while-statement)  

<a name="for-in_statements"></a>
### For-In 語句

`for-in` 語句會為集合（或實現了 `SequenceType` 協議的任意類型）中的每一項執行一次代碼塊。

`for-in` 語句的形式如下：

```swift
for 項 in 集合 {  
    循環體語句  
}  
```

`for-in` 語句在循環開始前會調用集合表達式的 `generate()` 方法來獲取一個實現了 `GeneratorType` 協議的類型的值。接下來循環開始，反復調用該值的 `next()` 方法。如果其返回值不是 `None`，它將會被賦給「項」，然後執行循環體語句，執行完畢後回到循環開始處，繼續重復這一過程；否則，既不會賦值也不會執行循環體語句，`for-in` 語句至此執行完畢。

> for-in 語句語法  
<a name="for-in-statement"></a>
> *for-in 語句* → **for** **case**<sub>可選</sub> [*模式*](07_Patterns.md#pattern) **in** [*表達式*](04_Expressions.md#expression) [*where子句*](#where-clause)<sub>可選</sub> [*代碼塊*](05_Declarations.md#code-block)  

<a name="while_statements"></a>
### While 語句

只要循環條件為真，`while` 語句就會重復執行代碼塊。

`while` 語句的形式如下：

```swift
while 條件 {  
    語句 
}  
```

`while` 語句的執行流程如下：

1. 判斷條件的值。如果為 `true`，轉到第 2 步；如果為 `false`，`while` 語句至此執行完畢。
2. 執行循環體中的語句，然後重復第 1 步。

由於會在執行循環體中的語句前判斷條件的值，因此循環體中的語句可能會被執行若幹次，也可能一次也不會被執行。

條件的結果必須是Bool類型或者Bool的橋接類型。另外，條件語句也可以使用可選綁定，請參閱 [可選綁定](../chapter2/01_The_Basics.md#optional_binding)。

> while 語句語法  

<a name="while-statement"></a>
> *while 語句* → **while** [*條件子句*](#condition-clause) [*代碼塊*](05_Declarations.md#code-block)  

<a name="condition-clause"></a>
> *條件子句* → [*表達式*](04_Expressions.md#expression)  
> *條件子句* → [*表達式*](04_Expressions.md#expression) **,** [*條件列表*](#condition-list)  
> *條件子句* → [*條件列表*](#condition-list)    
> *條件子句* → [*可用性條件*](#availability-condition) **,** [*表達式*](04_Expressions.md#expression) 

<a name="condition-list"></a>
> *條件列表* → [*條件*](#condition) | [*條件*](#condition) **,** [*條件列表*](#condition-list)      
<a name="condition"></a>
> *條件* → [*表達式*](04_Expressions.md#expression) |[*可用性條件*](#availability-condition) | [*case條件*](#case-condition) | [*可選綁定條件*](#optional-binding-condition)    
<a name="case-condition"></a>
> *case 條件* → **case** [*模式*](07_Patterns.md#pattern) [*構造器*](05_Declarations.md#initializer)     

<a name="optional-binding-condition"></a>
> *可選綁定條件* →  **let** [*模式*](07_Patterns.md#pattern) [*構造器*](05_Declarations.md#initializer) | **var**  [*模式*](07_Patterns.md#pattern) [*構造器*](05_Declarations.md#initializer)    


<a name="repeat-while_statements"></a>
### Repeat-While 語句

`repeat-while` 語句至少執行一次代碼塊，之後只要循環條件為真，就會重復執行代碼塊。

`repeat-while` 語句的形式如下：

```swift
repeat {  
    語句  
} while 條件  
```

`repeat-while` 語句的執行流程如下：

1. 執行循環體中的語句，然後轉到第 2 步。
2. 判斷條件的值。如果為 `true`，重復第 1 步；如果為 `false`，`repeat-while` 語句至此執行完畢。

由於條件的值是在循環體中的語句執行後才進行判斷，因此循環體中的語句至少會被執行一次。

條件的結果必須是Bool類型或者Bool的橋接類型。另外，條件語句也可以使用可選綁定，請參閱 [可選綁定](../chapter2/01_The_Basics.md#optional_binding)。

> repeat-while 語句語法  
<a name="repeat-while-statement"></a>
> *repeat-while 語句* → **repeat** [*代碼塊*](05_Declarations.md#code-block) **while** [*表達式*](04_Expressions.md#expression)  

<a name="branch_statements"></a>
## 分支語句

分支語句會根據一個或者多個條件來執行指定部分的代碼。分支語句中的條件將會決定程序如何分支以及執行哪部分代碼。Swift 提供兩種類型的分支語句：`if` 語句和 `switch` 語句。

`if` 語句和 `switch` 語句中的控制流可以用 `break` 語句改變，請參閱 [Break 語句](#break_statement)。

> 分支語句語法  
<a name="branch-statement"></a>
> *分支語句* → [*if 語句*](#if-statement)  
> *分支語句* → [*guard 語句*](#guard-statement)  
> *分支語句* → [*switch 語句*](#switch-statement)  

<a name="if_statements"></a>
### If 語句

`if` 語句會根據一個或多個條件來決定執行哪一塊代碼。

`if` 語句有兩種基本形式，無論哪種形式，都必須有花括號。

第一種形式是當且僅當條件為真時執行代碼，像下面這樣：

```swift
if 條件 {  
    語句  
}  
```

第二種形式是在第一種形式的基礎上添加 `else` 語句，當只有一個 `else` 語句時，像下面這樣：

```swift
if 條件 {    
    若條件為真則執行這部分語句    
} else {           
    若條件為假則執行這部分語句
}
```

`else` 語句也可包含 `if` 語句，從而形成一條鏈來測試更多的條件，像下面這樣：

```swift
if 條件1 {  
    若條件1為真則執行這部分語句  
} else if 條件2 {  
    若條件2為真則執行這部分語句
} else {  
    若前兩個條件均為假則執行這部分語句
} 
```

`if` 語句中條件的結果必須是Bool類型或者Bool的橋接類型。另外，條件語句也可以使用可選綁定，請參閱 [可選綁定](../chapter2/01_The_Basics.md#optional_binding)。

> if 語句語法  
<a name="if-statement"></a>
> *if 語句* → **if** [*條件子句*](#condition-clause) [*代碼塊*](05_Declarations.md#code-block) [*else子句*](#else-clause)<sub>可選</sub>  
<a name="else-clause"></a>
> *else 子句* → **else** [*代碼塊*](05_Declarations.md#code-block) | **else** [*if語句*](#if-statement)     

<a name="guard_statements"></a>
### Guard 語句    
    
如果一個或者多個條件不成立，可用 `guard` 語句用來退出當前作用域。

`guard` 語句的格式如下：

```swift
guard 條件 else {    
    語句    
}    
```

`guard` 語句中條件的結果必須是Bool類型或者Bool的橋接類型。另外，條件也可以是一條可選綁定，請參閱 [可選綁定](../chapter2/01_The_Basics.html#optional_binding)。
 
在 `guard` 語句中進行可選綁定的常量或者變量，其可用范圍從聲明開始直到作用域結束。
 
`guard` 語句必須有 `else` 子句，而且必須在該子句中調用 `Never` 返回類型的函數，或者使用下面的語句退出當前作用域：   
 
 *  `return`
 *  `break`
 *  `continue`
 *  `throw`    
 
關於控制轉移語句，請參閱 [控制轉移語句](#control_transfer_statements)。關於`Never`返回類型的函數，請參閱 [永不返回的函數](05_Declarations.md#rethrowing_functions_and_methods)。

> guard 語句語法  
<a name="guard-statement"></a>
> *guard 語句* → **guard** [*條件子句*](#condition-clause) **else** [*代碼塊*](05_Declarations.html#code-block)

<a name="switch_statements"></a>
### Switch 語句

`switch` 語句會根據控制表達式的值來決定執行哪部分代碼。

`switch` 語句的形式如下：

```swift
switch 控制表達式 {  
case 模式1:  
    語句  
case 模式2 where 條件:  
    語句  
case 模式3 where 條件, 模式4 where 條件:  
    語句
default:  
    語句
}  
```

`switch` 語句會先計算控制表達式的值，然後與每一個 `case` 的模式進行匹配。如果匹配成功，程序將會執行對應的 `case` 中的語句。另外，每一個 `case` 都不能為空，也就是說在每一個 `case` 中必須至少有一條語句。如果你不想在匹配到的 `case` 中執行代碼，只需在該 `case` 中寫一條 `break` 語句即可。

可以用作控制表達式的值是十分靈活的。除了標量類型外，如 `Int`、`Character`，你可以使用任何類型的值，包括浮點數、字符串、元組、自定義類型的實例和可選類型。控制表達式的值還可以用來匹配枚舉類型中的成員值或是檢查該值是否包含在指定的 `Range` 中。關於如何在 `switch` 語句中使用這些類型，請參閱 [控制流](../chapter2/05_Control_Flow.md) 一章中的 [Switch](../chapter2/05_Control_Flow.html#switch)。

每個 `case` 的模式後面可以有一個 `where` 子句。`where` 子句由 `where` 關鍵字緊跟一個提供額外條件的表達式組成。因此，當且僅當控制表達式匹配一個 `case` 的模式且 `where` 子句的表達式為真時，`case` 中的語句才會被執行。在下面的例子中，控制表達式只會匹配包含兩個相等元素的元組，例如 `(1, 1)`：

```swift
case let (x, y) where x == y:
```

正如上面這個例子，也可以在模式中使用 `let`（或 `var`）語句來綁定常量（或變量）。這些常量（或變量）可以在對應的 `where` 子句以及 `case` 中的代碼中使用。但是，如果一個 `case` 中含有多個模式，所有的模式必須包含相同的常量（或變量）綁定，並且每一個綁定的常量（或變量）必須在所有的條件模式中都有相同的類型。

`switch` 語句也可以包含默認分支，使用 `default` 關鍵字表示。只有所有 `case` 都無法匹配控制表達式時，默認分支中的代碼才會被執行。一個 `switch` 語句只能有一個默認分支，而且必須在 `switch` 語句的最後面。

`switch` 語句中 `case` 的匹配順序和源代碼中的書寫順序保持一致。因此，當多個模式都能匹配控制表達式時，只有第一個匹配的 `case` 中的代碼會被執行。

#### Switch 語句不能有遺漏

在 Swift 中，`switch` 語句中控制表達式的每一個可能的值都必須至少有一個 `case` 與之對應。在某些無法面面俱到的情況下（例如，表達式的類型是 `Int`），你可以使用 `default` 分支滿足該要求。

#### 不存在隱式落入

當匹配到的 `case` 中的代碼執行完畢後，`switch` 語句會直接退出，而不會繼續執行下一個 `case` 。這就意味著，如果你想執行下一個 `case`，需要顯式地在當前 `case` 中使用 `fallthrough` 語句。關於 `fallthrough` 語句的更多信息，請參閱 [Fallthrough 語句](#fallthrough_statements)。

> switch 語句語法  

<a name="switch-statement"></a>
> *switch 語句* → **switch** [*表達式*](04_Expressions.md#expression) **{** [*switch-case列表*](#switch-cases)<sub>可選</sub> **}**  
<a name="switch-cases"></a>
> *switch case 列表* → [*switch-case*](#switch-case) [*switch-case列表*](#switch-cases)<sub>可選</sub>  
<a name="switch-case"></a>
> *switch case* → [*case標簽*](#case-label) [*多條語句*](#statements) | [*default標簽*](#default-label) [*多條語句*](#statements)  

<a name="case-label"></a>
> *case 標簽* → **case** [*case項列表*](#case-item-list) **:**  
<a name="case-item-list"></a>
> *case 項列表* → [*模式*](07_Patterns.md#pattern) [*where子句*](#where-clause)<sub>可選</sub> | [*模式*](07_Patterns.md#pattern) [*where子句*](#where-clause)<sub>可選</sub> **,** [*case項列表*](#case-item-list)  
<a name="default-label"></a>
> *default 標簽* → **default** **:**  

<a name="where-clause"></a>
> *where-clause* → **where** [*where表達式*](#where-expression)  
<a name="where-expression"></a>
> *where-expression* → [*表達式*](04_Expressions.md#expression)  

<a name="labeled_statements"></a>
## 帶標簽的語句

你可以在循環語句或 `switch` 語句前面加上標簽，它由標簽名和緊隨其後的冒號（`:`）組成。在 `break` 和 `continue` 後面跟上標簽名可以顯式地在循環語句或 `switch` 語句中改變相應的控制流。關於這兩條語句用法，請參閱 [Break 語句](#break_statement) 和 [Continue 語句](#continue_statement)。

標簽的作用域在該標簽所標記的語句內。可以嵌套使用帶標簽的語句，但標簽名必須唯一。

關於使用帶標簽的語句的例子，請參閱 [控制流](../chapter2/05_Control_Flow.md) 一章中的 [帶標簽的語句](../chapter2/05_Control_Flow.md#labeled_statements)。

> 帶標簽的語句語法  
<a name="labeled-statement"></a>
> *帶標簽的語句* → [*語句標簽*](#statement-label) [*循環語句*](#loop-statement) | [*語句標簽*](#statement-label) [*if語句*](#if-statement) | [*語句標簽*](#statement-label) [*switch語句*](#switch-statement)  
<a name="statement-label"></a>
> *語句標簽* → [*標簽名稱*](#label-name) **:**  
<a name="label-name"></a>
> *標簽名稱* → [*標識符*](02_Lexical_Structure.md#identifier)  

<a name="control_transfer_statements"></a>
## 控制轉移語句

控制轉移語句能夠無條件地把控制權從一片代碼轉移到另一片代碼，從而改變代碼執行的順序。Swift 提供五種類型的控制轉移語句：`break` 語句、`continue` 語句、`fallthrough` 語句、`return` 語句和 `throw` 語句。

> 控制轉移語句語法  
<a name="control-transfer-statement"></a>
> *控制轉移語句* → [*break 語句*](#break-statement)  
> *控制轉移語句* → [*continue 語句*](#continue-statement)  
> *控制轉移語句* → [*fallthrough 語句*](#fallthrough-statement)  
> *控制轉移語句* → [*return 語句*](#return-statement)     
> *控制轉移語句* → [*throw 語句*](#throw-statement)  

<a name="break_statement"></a>
### Break 語句

`break` 語句用於終止循環語句、`if` 語句或 `switch` 語句的執行。使用 `break` 語句時，可以只寫 `break` 這個關鍵詞，也可以在 `break` 後面跟上標簽名，像下面這樣：

> break  
> break `標簽名`

當 `break` 語句後面帶標簽名時，可用於終止由這個標簽標記的循環語句、`if` 語句或 `switch` 語句的執行。

而只寫 `break` 時，則會終止 `switch` 語句或 `break` 語句所屬的最內層循環語句的執行。不能使用 `break` 語句來終止未使用標簽的 `if` 語句。

無論哪種情況，控制權都會被轉移給被終止的控制流語句後面的第一行語句。

關於使用 `break` 語句的例子，請參閱 [控制流](../chapter2/05_Control_Flow.md) 一章的 [Break](../chapter2/05_Control_Flow.md#break) 和 [帶標簽的語句](../chapter2/05_Control_Flow.md#labeled_statements)。

> break 語句語法  
<a name="break-statement"></a>
> *break 語句* → **break** [*標簽名稱*](#label-name)<sub>可選</sub>

<a name="continue_statement"></a>
### Continue 語句

`continue` 語句用於終止循環中當前迭代的執行，但不會終止該循環的執行。使用 `continue` 語句時，可以只寫 `continue` 這個關鍵詞，也可以在 `continue` 後面跟上標簽名，像下面這樣：

> continue  
> continue `標簽名`  

當 `continue` 語句後面帶標簽名時，可用於終止由這個標簽標記的循環中當前迭代的執行。

而當只寫 `continue` 時，可用於終止 `continue` 語句所屬的最內層循環中當前迭代的執行。

在這兩種情況下，控制權都會被轉移給循環語句的條件語句。

在 `for` 語句中，`continue` 語句執行後，增量表達式還是會被計算，這是因為每次循環體執行完畢後，增量表達式都會被計算。

關於使用 `continue` 語句的例子，請參閱 [控制流](../chapter2/05_Control_Flow.md) 一章的 [Continue](../chapter2/05_Control_Flow.md#continue) 和 [帶標簽的語句](../chapter2/05_Control_Flow.md#labeled_statements)。

> continue 語句語法  
<a name="continue-statement"></a>
> *continue 語句* → **continue** [*標簽名稱*](#label-name)<sub>可選</sub>

<a name="fallthrough_statements"></a>
### Fallthrough 語句

`fallthrough` 語句用於在 `switch` 語句中轉移控制權。`fallthrough` 語句會把控制權從 `switch` 語句中的一個 `case` 轉移到下一個 `case`。這種控制權轉移是無條件的，即使下一個 `case` 的模式與 `switch` 語句的控制表達式的值不匹配。

`fallthrough` 語句可出現在 `switch` 語句中的任意 `case` 中，但不能出現在最後一個 `case` 中。同時，`fallthrough` 語句也不能把控制權轉移到使用了值綁定的 `case`。

關於在 `switch` 語句中使用 `fallthrough` 語句的例子，請參閱 [控制流](../chapter2/05_Control_Flow.md) 一章的 [控制轉移語句](../chapter2/05_Control_Flow.md#control_transfer_statements)。

> fallthrough 語句語法  
<a name="fallthrough-statement"></a>
> *fallthrough 語句* → **fallthrough**  

<a name="return_statements"></a>
### Return 語句

`return` 語句用於在函數或方法的實現中將控制權轉移到調用函數或方法，接著程序將會從調用位置繼續向下執行。

使用 `return` 語句時，可以只寫 `return` 這個關鍵詞，也可以在 `return` 後面跟上表達式，像下面這樣：

> return  
> return `表達式`  

當 `return` 語句後面帶表達式時，表達式的值將會返回給調用函數或方法。如果表達式的值的類型與函數或者方法聲明的返回類型不匹配，Swift 則會在返回表達式的值之前將表達式的值的類型轉換為返回類型。

> 注意  
> 正如 [可失敗構造器](05_Declarations.md#failable_initializers) 中所描述的，`return nil` 在可失敗構造器中用於表明構造失敗。

而只寫 `return` 時，僅僅是從該函數或方法中返回，而不返回任何值（也就是說，函數或方法的返回類型為 `Void` 或者說 `()`）。

> return 語句語法  
<a name="return-statement"></a>
> *return 語句* → **return** [*表達式*](04_Expressions.html#expression)<sub>可選</sub>
    
<a name="throw_statements"></a>
### Throw 語句    

`throw` 語句出現在拋出函數或者拋出方法體內，或者類型被 `throws` 關鍵字標記的閉包表達式體內。   
 
`throw` 語句使程序在當前作用域結束執行，並向外圍作用域傳播錯誤。拋出的錯誤會一直傳遞，直到被 `do` 語句的 `catch` 子句處理掉。    

`throw` 語句由 `throw` 關鍵字緊跟一個表達式組成，如下所示：

> throw `表達式`    

表達式的結果必須符合 `ErrorType` 協議。

關於如何使用 `throw` 語句的例子，請參閱 [錯誤處理](../chapter2/18_Error_Handling.md) 一章的 [用 throwing 函數傳遞錯誤](../chapter2/18_Error_Handling.md#propagating_errors_using_throwing_functions)。    

> throw 語句語法    
<a name="throw-statement"></a>
> *throw 語句* → **throw**  [*表達式*](04_Expressions.md#expression)

<a name="defer_statements"></a>
## Defer 語句

`defer` 語句用於在退出當前作用域之前執行代碼。    

`defer` 語句形式如下：

```swift
defer {
	語句
}
```

在 `defer` 語句中的語句無論程序控制如何轉移都會被執行。在某些情況下，例如，手動管理資源時，比如關閉文件描述符，或者即使拋出了錯誤也需要執行一些操作時，就可以使用 `defer` 語句。    

如果多個 `defer` 語句出現在同一作用域內，那麼它們執行的順序與出現的順序相反。給定作用域中的第一個 `defer` 語句，會在最後執行，這意味著代碼中最靠後的 `defer` 語句中引用的資源可以被其他 `defer` 語句清理掉。    

```swift
func f() {
    defer { print("First") }
    defer { print("Second") }
    defer { print("Third") }
}
f()
// 打印 「Third」
// 打印 「Second」
// 打印 「First」
```

`defer` 語句中的語句無法將控制權轉移到 `defer` 語句外部。

> defer 語句語法        
<a name="defer-statement"></a>
> *延遲語句* → **defer** [*代碼塊*](05_Declarations.md#code-block)

<a name="do_statements"></a>
## Do 語句    

`do` 語句用於引入一個新的作用域，該作用域中可以含有一個或多個 `catch` 子句，`catch` 子句中定義了一些匹配錯誤條件的模式。`do` 語句作用域內定義的常量和變量只能在 `do` 語句作用域內使用。    

Swift 中的 `do` 語句與 C 中限定代碼塊界限的大括號（`{}`）很相似，也並不會降低程序運行時的性能。    

`do` 語句的形式如下：

```swift
do {
	try 表達式
	語句
} catch 模式1 {
	語句
} catch 模式2 where 條件 {
	語句
}
```

如同 `switch` 語句，編譯器會判斷 `catch` 子句是否有遺漏。如果 `catch` 子句沒有遺漏，則認為錯誤已被處理。否則，錯誤會自動傳遞到外圍作用域，被某個 `catch` 子句處理掉或者被用 `throws` 關鍵字聲明的拋出函數繼續向外拋出。    

為了確保錯誤已經被處理，可以讓 `catch` 子句使用匹配所有錯誤的模式，如通配符模式（`_`）。如果一個 `catch` 子句不指定一種具體模式，`catch` 子句會匹配任何錯誤，並綁定到名為 `error` 的局部常量。有關在 `catch` 子句中使用模式的更多信息，請參閱 [模式](07_Patterns.md)。

關於如何在 `do` 語句中使用一系列 `catch` 子句的例子，請參閱 [錯誤處理](../chapter2/18_Error_Handling.md#handling_errors)。       

> do 語句語法  
<a name="do-statement"></a>
> *do 語句* → **do** [*代碼塊*](05_Declarations.md#code-block) [*多條 catch子句*](#catch-clauses)<sub>可選</sub>  
<a name="catch-clauses"></a>
> *多條 catch 子句* → [*catch子句*](#catch-clause) [*多條 catch子句*](#catch-clauses)<sub>可選</sub>   
<a name="catch-clause"></a>
> *catch 子句* → **catch** [*模式*](07_Patterns.md#pattern)<sub>可選</sub> [*where子句*](#where-clause)<sub>可選</sub> [*代碼塊*](05_Declarations.md#code-block)

<a name="compiler_control_statements"></a>
## 編譯器控制語句

編譯器控制語句允許程序改變編譯器的行為。Swift 有兩種編譯器控制語句：編譯配置語句和線路控制語句。

> 編譯器控制語句語法  
<a name="compiler-control-statement"></a>
> *編譯器控制語句* → [*編譯配置語句*](#build-config-statement)  
> *編譯器控制語句* → [*線路控制語句*](#line-control-statement)

<a name="build_config_statements"></a>
### 編譯配置語句

編譯配置語句可以根據一個或多個配置來有條件地編譯代碼。

每一個編譯配置語句都以 `#if` 開始，`#endif` 結束。如下是一個簡單的編譯配置語句：

```swift
#if 編譯配置項
	語句
#endif
```

和 `if` 語句的條件不同，編譯配置的條件是在編譯時進行判斷的。只有編譯配置在編譯時判斷為 `true` 的情況下，相應的語句才會被編譯和執行。

編譯配置可以是 `true` 和 `false` 的字面量，也可以是使用 `-D` 命令行標志的標識符，或者是下列表格中的任意一個平台檢測函數。

| 函數 | 可用參數 |
| --- | --- |
| `os()` | `OSX`, `iOS`, `watchOS`, `tvOS`, `Linux` |
| `arch()` | `i386`, `x86_64`, `arm`, `arm64` |
| `swift()` | `>=` 後跟版本號 |

`swift()`（語言版本檢測函數）的版本號參數主要由主版本號和次版本號組成並且使用點號（`.`）分隔開，`>=` 和版本號之間不能有空格。

> 注意  
> `arch(arm)` 平台檢測函數在 ARM 64 位設備上不會返回 `true`。如果代碼在 32 位的 iOS 模擬器上編譯，`arch(i386)` 平台檢測函數會返回 `true`。

你可以使用邏輯操作符 `&&`、`||` 和 `!` 來組合多個編譯配置，還可以使用圓括號來進行分組。

就像 `if` 語句一樣，你可以使用 `#elseif` 子句來添加任意多個條件分支來測試不同的編譯配置。你也可以使用 `#else` 子句來添加最終的條件分支。包含多個分支的編譯配置語句例子如下：

```swift
#if 編譯配置1
	如果編譯配置1成立則執行這部分代碼
#elseif 編譯配置2
	如果編譯配置2成立則執行這部分代碼
#else
	如果編譯配置均不成立則執行這部分代碼
#endif
```

> 注意  
> 即使沒有被編譯，編譯配置中的語句仍然會被解析。然而，唯一的例外是編譯配置語句中包含語言版本檢測函數：僅當 `Swift` 編譯器版本和語言版本檢測函數中指定的版本號匹配時，語句才會被解析。這種設定能確保舊的編譯器不會嘗試去解析新 Swift 版本的語法。

<a name="build-config-statement"></a>
> 編譯配置語句語法  

<a name="build-configuration-statement"></a>
> *單個編譯配置語句* → **#if** [*編譯配置*](#build-configuration) [*語句*](#statements)<sub>可選</sub> [*多個編譯配置elseif子句*](#build-configuration-elseif-clauses)<sub>可選</sub> **-** [*單個編譯配置else子句*](#build-configuration-else-clause)<sub>可選</sub> **#endif**  
<a name="build-configuration-elseif-clauses"></a>
> *多個編譯配置 elseif 子句* → [*單個編譯配置elseif子句*](#build-configuration-elseif-clause) [*多個編譯配置elseif子句*](build-configuration-elseif-clauses)<sub>可選</sub>  
<a name="build-configuration-elseif-clause"></a>
> *單個編譯配置 elseif 子句* → **#elseif** [*編譯配置*](#build-configuration) [*語句*](#statements)<sub>可選</sub>  
<a name="build-configuration-else-clause"></a>
> *單個編譯配置 else 子句* → **#else** [*語句*](#statements)<sub>可選</sub>

<a name="build-configuration"></a>
> *編譯配置* → [*平台檢測函數*](#platform-testing-function)  
> *編譯配置* → [*語言版本檢測函數*](#language-version-testing-function)  
> *編譯配置* → [*標識符*](02_Lexical_Structure.md#identifier)  
> *編譯配置* → [*布爾值字面量*](02_Lexical_Structure.md#boolean-literal)  
> *編譯配置* → **(** [*編譯配置*](#build-configuration) **)**  
> *編譯配置* → **!** [*編譯配置*](#build-configuration)  
> *編譯配置* → [*編譯配置*](#build-configuration) **&&** [*編譯配置*](#build-configuration)  
> *編譯配置* → [*編譯配置*](#build-configuration) **||** [*編譯配置*](#build-configuration)  

<a name="platform-testing-function"></a>
> *平台檢測函數* → **os** **(** [*操作系統*](#operating-system) **)**  
> *平台檢測函數* → **arch** **(** [*架構*](#architecture) **)**  
<a name="language-version-testing-function"></a>
> *語言版本檢測函數* → **swift** **(** **>=** [*swift版本*](#swift-version) **)**  
<a name="operating-system"></a>
> *操作系統* → **OSX** | **iOS** | **watchOS** | **tvOS**  
<a name="architecture"></a>
> *架構* → **i386** | **x86_64** | **arm** | **arm64**  
<a name="swift-version"></a>
> *swift 版本* → [*十進制數字*](02_Lexical_Structure.md#decimal-digit) ­**.** ­[*十進制數字*](02_Lexical_Structure.md#decimal-digit)

<a name="line_control_statements"></a>
### 行控制語句

行控制語句可以為被編譯的源代碼指定行號和文件名，從而改變源代碼的定位信息，以便進行分析和調試。

行控制語句形式如下：

> \#sourceLocation(file: `文件名` , line:`行號`)

> \#sourceLocation()

第一種的行控制語句會改變該語句之後的代碼中的字面量表達式 `#line` 和 `#file` 所表示的值。`行號` 是一個大於 0 的整形字面量，會改變 `#line` 表達式的值。`文件名` 是一個字符串字面量，會改變 `#file` 表達式的值。

第二種的行控制語句, `#sourceLocation()`，會將源代碼的定位信息重置回默認的行號和文件名。

<a name="line-control-statement"></a>
> 行控制語句語法  

> *行控制語句* → **#sourceLocation(file:[*文件名*](#file-name),line:[*行號*](#line-number))**  
> *行控制語句* → **#sourceLocation()**  
<a name="line-number"></a>
> *行號* → 大於 0 的十進制整數  
<a name="file-name"></a>
> *文件名* → [*靜態字符串字面量*](02_Lexical_Structure.md#static-string-literal)

<a name="availability_condition"></a>
### 可用性條件    

可用性條件可作為 `if`，`while`，`guard` 語句的條件，可以在運行時基於特定的平台參數來查詢 API 的可用性。    

可用性條件的形式如下：

```swift
if #available(平台名稱 版本, ..., *) {    
	如果 API 可用，則執行這部分語句    
} else {    
	如果 API 不可用，則執行這部分語句
}    
```

使用可用性條件來執行一個代碼塊時，取決於使用的 API 在運行時是否可用，編譯器會根據可用性條件提供的信息來決定是否執行相應的代碼塊。

可用性條件使用一系列逗號分隔的平台名稱和版本。使用 `iOS`，`OSX`，以及 `watchOS` 等作為平台名稱，並寫上相應的版本號。`*` 參數是必須寫的，用於處理未來的潛在平台。可用性條件確保了運行時的平台不低於條件中指定的平台版本時才執行代碼塊。
   
與布爾類型的條件不同，不能用邏輯運算符 `&&` 和 `||` 組合可用性條件。 

> 可用性條件語法    

<a name="availability-condition"></a>
> *可用性條件* → **#available** **(** [*可用性參數列表*](#availability-arguments) **)**   
<a name="availability-arguments"></a>
> *可用性參數列表* → [*可用性參數*](#availability-argument) | [*可用性參數*](#availability-argument) **,** [*可用性參數列表*](#availability-arguments)  
<a name="availability-argument"></a>
> *可用性參數* → [平台名稱](#platform-name) [平台版本](#platform-version)    
> *可用性條件* → __*__

<a name="platform-name"></a>
> *平台名稱* → **iOS** | **iOSApplicationExtension**  
> *平台名稱* → **OSX** | **OSXApplicationExtension**   
> *平台名稱* → **watchOS**     
<a name="platform-version"></a>
> *平台版本* → [十進制數字](02_Lexical_Structure.md#decimal-digits)     
> *平台版本* → [十進制數字](02_Lexical_Structure.md#decimal-digits) **.** [十進制數字](02_Lexical_Structure.md#decimal-digits)  
> *平台版本* → [十進制數字](02_Lexical_Structure.md#decimal-digits) **.** [十進制數字](02_Lexical_Structure.md#decimal-digits) **.** [十進制數字](02_Lexical_Structure.md#decimal-digits)


