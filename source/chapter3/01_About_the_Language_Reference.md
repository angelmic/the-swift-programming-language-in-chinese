# 關於語言參考（About the Language Reference）
-----------------

> 1.0
> 翻譯：[dabing1022](https://github.com/dabing1022)
> 校對：[numbbbbb](https://github.com/numbbbbb)

> 2.0
> 翻譯+校對：[KYawn](https://github.com/KYawn)

本頁內容包括：

- [如何閱讀語法](#how_to_read_the_grammar)

本書的這一節描述了 Swift 編程語言的形式語法。這裡描述的語法是為了幫助您更詳細地了解該語言，而不是讓您直接實現一個解析器或編譯器。

Swift 語言相對較小，這是由於 Swift 代碼中的幾乎所有常見類型、函數以及運算符都已經在 Swift 標准庫中定義了。雖然這些類型、函數和運算符並不是 Swift 語言自身的一部分，但是它們被廣泛應用於本書的討論和代碼范例中。

<a name="how_to_read_the_grammar"></a>
## 如何閱讀語法

用來描述 Swift 編程語言形式語法的符號遵循下面幾個約定：

-  箭頭（`→`）用來標記語法產式，可以理解為「可由……構成」。
-  斜體文字用來表示句法類型，並出現在一個語法產式規則兩側。
-  關鍵字和標點符號由固定寬度的粗體文本表示，只出現在一個語法產式規則的右側。
-  可供選擇的語法產式由豎線（`|`）分隔。當可選用的語法產式太多時，為了閱讀方便，它們將被拆分為多行語法產式規則。
-  少數情況下，語法產式規則的右側會有用於描述的常規字體文字。
-  可選的句法類型和字面值用尾標 `opt` 來標記。

舉個例子，getter-setter 的語法塊的定義如下：

> getter-setter 方法塊語法  
> *getter-setter 方法塊* → { [*getter 子句*](05_Declarations.html#getter-clause) [*setter 子句*](05_Declarations.html#setter-clause)<sub>可選</sub> } | { [*setter 子句*](05_Declarations.html#setter-clause) [*getter 子句*](05_Declarations.html#getter-clause) }

這個定義表明，一個 getter-setter 方法塊可以由一個 getter 子句後跟一個可選的 setter 子句構成，然後用大括號括起來，或者由一個 setter 子句後跟一個 getter 子句構成，然後用大括號括起來。下面的兩個語法產式等價於上述的語法產式，並明確指出了如何取舍：

> getter-setter 方法塊語法  
> getter-setter 方法塊 → { [*getter 子句*](05_Declarations.html#getter-clause)  [*setter 子句*](05_Declarations.html#setter-clause)<sub>可選</sub> }  
> getter-setter 方法塊 → { [*setter 子句*](05_Declarations.html#setter-clause) [*getter 子句*](05_Declarations.html#getter-clause) }
