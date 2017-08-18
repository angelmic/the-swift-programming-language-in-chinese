# 詞法結構（Lexical Structure）
-----------------

> 1.0
> 翻譯：[superkam](https://github.com/superkam)
> 校對：[numbbbbb](https://github.com/numbbbbb)



> 2.0
> 翻譯+校對：[buginux](https://github.com/buginux)



> 2.1
> 翻譯：[mmoaay](https://github.com/mmoaay)



> 2.2
> 翻譯+校對：[星夜暮晨](https://github.com/semperidem)，2016-04-06


本頁包含內容：

- [空白與注釋](#whitespace_and_comments)
- [標識符](#identifiers)
- [關鍵字和標點符號](#keywords)
- [字面量](#literals)
  - [整數字面量](#integer_literals) 
  - [浮點數字面量](#floating_point_literals)
  - [字符串字面量](#string_literals)
- [運算符](#operators)

Swift 的*「詞法結構 (lexical structure)」* 描述了能構成該語言中合法符號 (token) 的字符序列。這些合法符號組成了語言中最底層的構建基塊，並在之後的章節中用於描述語言的其他部分。一個合法符號由一個標識符 (identifier)、關鍵字 (keyword)、標點符號 (punctuation)、字面量 (literal) 或運算符 (operator) 組成。

通常情況下，通過考慮輸入文本當中可能的最長子串，並且在隨後將介紹的語法約束之下，根據隨後將介紹的語法約束生成的，根據 Swift 源文件當中的字符來生成相應的「符號」。這種方法稱為*「最長匹配 (longest match)」*，或者*「最大適合(maximal munch)」*。

<a id="whitespace_and_comments"></a>
## 空白與注釋

空白 (whitespace) 有兩個用途：分隔源文件中的符號以及幫助區分運算符屬於前綴還是後綴（參見 [運算符](#operators)），在其他情況下空白則會被忽略。以下的字符會被當作空白：空格（U+0020）、換行符（U+000A）、回車符（U+000D）、水平制表符（U+0009）、垂直制表符（U+000B）、換頁符（U+000C）以及空字符（U+0000）。

注釋被編譯器當作空白處理。單行注釋由 `//` 開始直至遇到換行符（U+000A）或者回車符（U+000D）。多行注釋由 `/*` 開始，以 `*/` 結束。注釋允許嵌套，但注釋標記必須匹配。

正如 [*Markup Formatting Reference*](https://developer.apple.com/library/prerelease/ios/documentation/Xcode/Reference/xcode_markup_formatting_ref/index.html#//apple_ref/doc/uid/TP40016497) 所述，注釋可以包含附加的格式和標記。

<a id="identifiers"></a>
## 標識符

*標識符(identifier)* 可以由以下的字符開始：大寫或小寫的字母 `A` 到 `Z`、下劃線 (`_`)、基本多文種平面 (Basic Multilingual Plane) 中非字符數字組合的  Unicode 字符以及基本多文種平面以外的非個人專用區字符。在首字符之後，允許使用數字和組合 Unicode 字符。

使用保留字作為標識符，需要在其前後增加反引號 (`` ` ``)。例如，`class` 不是合法的標識符，但可以使用 `` `class` ``。反引號不屬於標識符的一部分，`` `x` `` 和 `x` 表示同一標識符。

閉包中如果沒有明確指定參數名稱，參數將被隱式命名為 `$0`、`$1`、`$2` 等等。這些命名在閉包作用域范圍內是合法的標識符。

> 標識符語法

<a id="identifier"></a>
> *標識符* → [*頭部標識符*](#identifier-head) [*標識符字符組*](#identifier-characters)<sub>可選</sub>  
> *標識符* → \`[*頭部標識符*](#identifier-head) [*標識符字符組*](#identifier-characters)<sub>可選</sub>\`  
> *標識符* → [*隱式參數名*](#implicit-parameter-name)  

<a id="identifier-list"></a>
> *標識符列表* → [*標識符*](#identifier) | [*標識符*](#identifier) **,** [*標識符列表*](#identifier-list)

<a id="identifier-head"></a>        
> *頭部標識符* → 大寫或小寫字母 A - Z  
> *頭部標識符* → _  
> *頭部標識符* → U+00A8，U+00AA，U+00AD，U+00AF，U+00B2–U+00B5，或者 U+00B7–U+00BA  
> *頭部標識符* → U+00BC–U+00BE，U+00C0–U+00D6，U+00D8–U+00F6，或者 U+00F8–U+00FF  
> *頭部標識符* → U+0100–U+02FF，U+0370–U+167F，U+1681–U+180D，或者 U+180F–U+1DBF  
> *頭部標識符* → U+1E00–U+1FFF  
> *頭部標識符* → U+200B–U+200D，U+202A–U+202E，U+203F–U+2040，U+2054，或者 U+2060–U+206F  
> *頭部標識符* → U+2070–U+20CF，U+2100–U+218F，U+2460–U+24FF，或者 U+2776–U+2793  
> *頭部標識符* → U+2C00–U+2DFF 或者 U+2E80–U+2FFF  
> *頭部標識符* → U+3004–U+3007，U+3021–U+302F，U+3031–U+303F，或者 U+3040–U+D7FF  
> *頭部標識符* → U+F900–U+FD3D，U+FD40–U+FDCF，U+FDF0–U+FE1F，或者 U+FE30–U+FE44  
> *頭部標識符* → U+FE47–U+FFFD  
> *頭部標識符* → U+10000–U+1FFFD，U+20000–U+2FFFD，U+30000–U+3FFFD，或者 U+40000–U+4FFFD  
> *頭部標識符* → U+50000–U+5FFFD，U+60000–U+6FFFD，U+70000–U+7FFFD，或者 U+80000–U+8FFFD  
> *頭部標識符* → U+90000–U+9FFFD，U+A0000–U+AFFFD，U+B0000–U+BFFFD，或者 U+C0000–U+CFFFD  
> *頭部標識符* → U+D0000–U+DFFFD 或者 U+E0000–U+EFFFD  

<a id="identifier-character"></a>
> *標識符字符* → 數值 0 - 9  
> *標識符字符* → U+0300–U+036F，U+1DC0–U+1DFF，U+20D0–U+20FF，或者 U+FE20–U+FE2F  
> *標識符字符* → [*頭部標識符*](#identifier-head)
> <a id="identifier-characters"></a>        
> *標識符字符組* → [*標識符字符*](#identifier-character) [*標識符字符組*](#identifier-characters)<sub>可選</sub>  

<a id="implicit-parameter-name"></a>    
> *隱式參數名* → **$** [*十進制數字列表*](#decimal-digits)  

<a id="keywords"></a>
## 關鍵字和標點符號

下面這些被保留的關鍵字不允許用作標識符，除非使用反引號轉義，具體描述請參考 [標識符](#identifiers)。除了 `inout`、`var` 以及 `let` 之外的關鍵字可以用作某個函數聲明或者函數調用當中的外部參數名，不用添加反引號轉義。

* 用在聲明中的關鍵字： `associatedtype`、`class`、`deinit`、`enum`、`extension`、`func`、`import`、`init`、`inout`、`internal`、`let`、`operator`、`private`、`protocol`、`public`、`static`、`struct`、`subscript`、`typealias` 以及 `var`。
* 用在語句中的關鍵字：`break`、`case`、`continue`、`default`、`defer`、`do`、`else`、`fallthrough`、`for`、`guard`、`if`、`in`、`repeat`、`return`、`switch`、`where` 以及 `while`。
* 用在表達式和類型中的關鍵字：`as`、`catch`、`dynamicType`、`false`、`is`、`nil`、`rethrows`、`super`、`self`、`Self`、`throw`、`throws`、`true`、`try`、`#column`、`#file`、`#function` 以及 `#line`。
* 用在模式中的關鍵字：`_`。
* 以井字號 (`#`) 開頭的關鍵字：`#available`、`#column`、`#else#elseif`、`#endif`、`#file`、`#function`、`#if`、`#line` 以及 `#selector`。
* 特定上下文中被保留的關鍵字： `associativity`、`convenience`、`dynamic`、`didSet`、`final`、`get`、`infix`、`indirect`、`lazy`、`left`、`mutating`、`none`、`nonmutating`、`optional`、`override`、`postfix`、`precedence`、`prefix`、`Protocol`、`required`、`right`、`set`、`Type`、`unowned`、`weak` 以及 `willSet`。這些關鍵字在特定上下文之外可以被用做標識符。

以下符號被當作保留符號，不能用於自定義運算符： `(`、`)`、`{`、`}`、`[`、`]`、`.`、`,`、`:`、`;`、`=`、`@`、`#`、`&`（作為前綴運算符）、`->`、`` ` ``、`?`、`!`（作為後綴運算符）。

<a id="literals"></a>
## 字面量

*字面量 (literal)* 用來表示源碼中某種特定類型的值，比如一個數字或字符串。

下面是字面量的一些示例：

```swift
42              // 整數字面量
3.14159         // 浮點數字面量
"Hello, world!" // 字符串字面量
true		    // 布爾值字面量
```

字面量本身並不包含類型信息。事實上，一個字面量會被解析為擁有無限的精度，然後 Swift 的類型推導會嘗試去推導出這個字面量的類型。比如，在 `let x: Int8 = 42` 這個聲明中，Swift 使用了顯式類型標注（`: Int8`）來推導出 `42` 這個整數字面量的類型是 `Int8`。如果沒有可用的類型信息， Swift 則會從標准庫中定義的字面量類型中推導出一個默認的類型。整數字面量的默認類型是 `Int`，浮點數字面量的默認類型是 `Double`，字符串字面量的默認類型是 `String`，布爾值字面量的默認類型是 `Bool`。比如，在 `let str = "Hello, world"` 這個聲明中，字符串 `"Hello, world"` 的默認推導類型就是 `String`。

當為一個字面量值指定了類型標注的時候，這個標注的類型必須能通過這個字面量值實例化。也就是說，這個類型必須符合這些 Swift 標准庫協議中的一個：整數字面量的 `IntegerLiteralConvertible` 協議、浮點數字面量的 `FloatingPointLiteralConvertible` 協議、字符串字面量的 `StringLiteralConvertible` 協議以及布爾值字面量的 `BooleanLiteralConvertible` 協議。比如，`Int8` 符合 `IntegerLiteralConvertible` 協議，因此它能在 `let x: Int8 = 42` 這個聲明中作為整數字面量 `42` 的類型標注。

> 字面量語法  
> *字面量* → [*數值字面量*](#numeric-literal) | [*字符串字面量*](#string-literal) | [*布爾值字面量*](#boolean-literal) | [*nil 字面量*](#nil-literal)    

<a id="numeric-literal"></a>    
> *數值字面量* → **-**<sub>可選</sub> [*整數字面量*](#integer-literal) | **-**<sub>可選</sub> [*浮點數字面量*](#floating-point-literal)   
> <a id="boolean-literal"></a> 
> *布爾值字面量* → **true** | **false**    
> <a id="nil-literal"></a> 
> *nil 字面量* → **nil**

<a id="integer_literals"></a>
### 整數字面量

*整數字面量 (Integer Literals)* 表示未指定精度整數的值。整數字面量默認用十進制表示，可以加前綴來指定其他的進制。二進制字面量加 `0b`，八進制字面量加 `0o`，十六進制字面量加 `0x`。

十進制字面量包含數字 `0` 至 `9`。二進制字面量只包含 `0` 或 `1`，八進制字面量包含數字 `0` 至 `7`，十六進制字面量包含數字 `0` 至 `9` 以及字母 `A` 至 `F`（大小寫均可）。

負整數的字面量在整數字面量前加負號 `-`，比如 `-42`。

整型字面面可以使用下劃線 (`_`) 來增加數字的可讀性，下劃線會被系統忽略，因此不會影響字面量的值。同樣地，也可以在數字前加 `0`，這同樣也會被系統所忽略，並不會影響字面量的值。

除非特別指定，整數字面量的默認推導類型為 Swift 標准庫類型中的 `Int`。Swift 標准庫還定義了其他不同長度以及是否帶符號的整數類型，請參考 [整數](../chapter2/01_The_Basics.html#integers)。

> 整數字面量語法  

<a id="integer-literal"></a>
> *整數字面量* → [*二進制字面量*](#binary-literal)  
> *整數字面量* → [*八進制字面量*](#octal-literal)  
> *整數字面量* → [*十進制字面量*](#decimal-literal)  
> *整數字面量* → [*十六進制字面量*](#hexadecimal-literal)  

<a id="binary-literal"></a>
> *二進制字面量* → **0b** [*二進制數字*](#binary-digit) [*二進制字面量字符組*](#binary-literal-characters)<sub>可選</sub> 
> <a id="binary-digit"></a>   
> *二進制數字* → 數值 0 到 1  
> <a id="binary-literal-character"></a> 
> *二進制字面量字符* → [*二進制數字*](#binary-digit) | _    
> <a id="binary-literal-characters"></a> 
> *二進制字面量字符組* → [*二進制字面量字符*](#binary-literal-character) [*二進制字面量字符組*](#binary-literal-characters)<sub>可選</sub>  

<a id="octal-literal"></a>    
> *八進制字面量* → **0o** [*八進字數字*](#octal-digit) [*八進制字符組*](#octal-literal-characters)<sub>可選</sub>    
> <a id="octal-digit"></a>  
> *八進字數字* → 數值 0 到 7  
> <a id="octal-literal-character"></a> 
> *八進制字符* → [*八進字數字*](#octal-digit) | _    
> <a id="octal-literal-characters"></a> 
> *八進制字符組* → [*八進制字符*](#octal-literal-character) [*八進制字符組*](#octal-literal-characters)<sub>可選</sub>

<a id="decimal-literal"></a>    
> *十進制字面量* → [*十進制數字*](#decimal-digit) [*十進制字符組*](#decimal-literal-characters)<sub>可選</sub>    
> <a id="decimal-digit"></a>
> *十進制數字* → 數值 0 到 9  
> <a id="decimal-digits"></a>
> *十進制數字組* → [*十進制數字*](#decimal-digit) [*十進制數字組*](#decimal-digits)<sub>可選</sub>  
> <a id="decimal-literal-character"></a>
> *十進制字符* → [*十進制數字*](#decimal-digit) | _    
> <a id="decimal-literal-characters"></a>
> *十進制字符組* → [*十進制字符*](#decimal-literal-character) [*十進制字符組*](#decimal-literal-characters)<sub>可選</sub> 

<a id="hexadecimal-literal"></a>    
> *十六進制字面量* → **0x** [*十六進制數字*](#hexadecimal-digit) [*十六進制字面量字符組*](#hexadecimal-literal-characters)<sub>可選</sub>  
> <a id="hexadecimal-digit"></a>
> *十六進制數字* → 數值 0 到 9, 字母 a 到 f, 或 A 到 F  
> <a id="hexadecimal-literal-character"></a>
> *十六進制字符* → [*十六進制數字*](#hexadecimal-digit) | _     
> <a id="hexadecimal-literal-characters"></a>
> *十六進制字面量字符組* → [*十六進制字符*](#hexadecimal-literal-character) [*十六進制字面量字符組*](#hexadecimal-literal-characters)<sub>可選</sub>  

<a id="floating_point_literals"></a>
### 浮點數字面量

*浮點數字面量 (Floating-point literals)* 表示未指定精度浮點數的值。

浮點數字面量默認用十進制表示（無前綴），也可以用十六進制表示（加前綴 `0x`）。

十進制浮點數字面量由十進制數字串後跟小數部分或指數部分（或兩者皆有）組成。十進制小數部分由小數點 (`.`) 後跟十進制數字串組成。指數部分由大寫或小寫字母 `e` 為前綴後跟十進制數字串組成，這串數字表示 `e` 之前的數量乘以 10 的幾次方。例如：`1.25e2` 表示 1.25 x 10²，也就是 `125.0`；同樣，`1.25e－2` 表示 1.25 x 10¯²，也就是 `0.0125`。

十六進制浮點數字面量由前綴 `0x` 後跟可選的十六進制小數部分以及十六進制指數部分組成。十六進制小數部分由小數點後跟十六進制數字串組成。指數部分由大寫或小寫字母 `p` 為前綴後跟十進制數字串組成，這串數字表示 `p` 之前的數量乘以 2 的幾次方。例如：`0xFp2` 表示 15 x 2²，也就是 `60`；同樣，`0xFp-2` 表示 15 x 2¯²，也就是 `3.75`。

負數的浮點數字面量由負號 (`-`) 和浮點數字面量組成，例如 `-42.5`。

浮點數字面量允許使用下劃線 (`_`) 來增強數字的可讀性，下劃線會被系統忽略，因此不會影響字面量的值。同樣地，也可以在數字前加 `0`，並不會影響字面量的值。

除非特別指定，浮點數字面量的默認推導類型為 Swift 標准庫類型中的 `Double`，表示 64 位浮點數。Swift 標准庫也定義了 `Float` 類型，表示 32 位浮點數。

> 浮點數字面量語法  

<a id="floating-point-literal"></a> 
> *浮點數字面量* → [*十進制字面量*](#decimal-literal) [*十進制分數*](#decimal-fraction)<sub>可選</sub> [*十進制指數*](#decimal-exponent)<sub>可選</sub>      
> *浮點數字面量* → [*十六進制字面量*](#hexadecimal-literal) [*十六進制分數*](#hexadecimal-fraction)<sub>可選</sub> [*十六進制指數*](#hexadecimal-exponent)

<a id="decimal-fraction"></a>    
> *十進制分數* → **.** [*十進制字面量*](#decimal-literal) 
> <a id="decimal-exponent"></a>  
> *十進制指數* → [*十進制指數 e*](#floating-point-e) [*正負號*](#sign)<sub>可選</sub> [*十進制字面量*](#decimal-literal)  

<a id="hexadecimal-fraction"></a>
> *十六進制分數* → **.** [*十六進制數字*](#hexadecimal-digit) [*十六進制字面量字符組*](#hexadecimal-literal-characters)<sub>可選</sub>  
> <a id="hexadecimal-exponent"></a>
> *十六進制指數* → [*十六進制指數 p*](#floating-point-p) [*正負號*](#sign)<sub>可選</sub> [*十進制字面量*](#decimal-literal)  

<a id="floating-point-e"></a>
> *十進制指數 e* → **e** | **E**  
> <a id="floating-point-p"></a>
> *十六進制指數 p* → **p** | **P**  
> <a id="sign"></a>
> *正負號* → **+** | **-**  

<a id="string_literals"></a>
### 字符串字面量

字符串字面量由被包在雙引號中的一串字符組成，形式如下：

> "`字符`"

字符串字面量中不能包含未轉義的雙引號（`"`）、未轉義的反斜線（`\`）、回車符、換行符。

可以在字符串字面量中使用的轉義特殊符號如下：

* 空字符 `\0`
* 反斜線 `\\`
* 水平制表符 `\t`
* 換行符 `\n`
* 回車符 `\r`
* 雙引號 `\"`
* 單引號 `\'`
* Unicode 標量 `\u{`n`}`，n 為一到八位的十六進制數字

字符串字面量允許在反斜杠 (`\`) 後的括號 `()` 中插入表達式的值。插入表達式可以包含字符串字面量，但不能包含未轉義的雙引號 (`"`)、未轉義的反斜線 (`\`)、回車符以及換行符。

例如，以下所有字符串字面量的值都是相同的：

```swift
"1 2 3"
"1 2 \("3")"
"1 2 \(3)"
"1 2 \(1 + 2)"
let x = 3; "1 2 \(x)"
```

字符串字面量的默認推導類型為 `String`。更多有關 `String` 類型的信息請參考 [字符串和字符](../chapter2/03_Strings_and_Characters.html) 以及 [*String Structure Reference*](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Reference/Swift_String_Structure/index.html#//apple_ref/doc/uid/TP40015181)。

用 `＋` 操作符連接的字符型字面量是在編譯時進行連接的。比如下面的 `textA` 和 `textB` 是完全一樣的，`textA` 沒有任何運行時的連接操作。

```swift
let textA = "Hello " + "world"
let textB = "Hello world"
```

> 字符串字面量語法  

<a id="string-literal"></a>
> *字符串字面量* → [*靜態字符串字面量*](#static-string-literal) | [*插值字符串字面量*](#interpolated-string-literal) 

<a id="static-string-literal"></a>
> *靜態字符串字面量* → **"**[*引用文本*](#quoted-text)<sub>可選</sub>**"**  
> <a id="quoted-text"></a> 
> *引用文本* → [*引用文本項*](#quoted-text-item) [*引用文本*](#quoted-text)<sub>可選</sub> 
> <a id="quoted-text-item"></a>  
> *引用文本項* → [*轉義字符*](#escaped-character)  
> *引用文本項* → 除了 **"**、**\\**、U+000A、U+000D 以外的所有 Unicode 字符

<a id="interpolated-string-literal"></a>
> *插值字符串字面量* → **"**[*插值文本*](#interpolated-text)<sub>可選</sub>**"**  
> <a id="interpolated-text"></a> 
> *插值文本* → [*插值文本項*](#interpolated-text-item) [*插值文本*](#interpolated-text)<sub>可選</sub> 
> <a id="interpolated-text-item"></a>  
> *插值文本項* → **\\****(**[*表達式*](./04_Expressions.html)**)** | [*引用文本項*](#quoted-text-item)

<a id="escaped-character"></a>
> *轉義字符* → **\\****0** | **\\****\\** | **\t** | **\n** | **\r** | **\\"** | **\\'**     
> *轉義字符* → **\u {** [*unicode 標量數字*](#unicode-scalar-digits) **}**  
> <a id="unicode-scalar-digits"></a>
> *unicode 標量數字* → 一到八位的十六進制數字

<a id="operators"></a>
## 運算符

Swift 標准庫定義了許多可供使用的運算符，其中大部分在 [基礎運算符](../chapter2/02_Basic_Operators.html) 和 [高級運算符](../chapter2/25_Advanced_Operators.html) 中進行了闡述。這一小節將描述哪些字符能用於自定義運算符。

自定義運算符可以由以下其中之一的 ASCII 字符 `/`、`=`、 `-`、`+`、`!`、`*`、`%`、`<`、`>`、`&`、`|`、`^`、`?` 以及 `~`，或者後面語法中規定的任一個 Unicode 字符（其中包含了*數學運算符*、*零散符號(Miscellaneous Symbols)* 以及印刷符號 (Dingbats) 之類的 Unicode 塊）開始。在第一個字符之後，允許使用組合型 Unicode 字符。

您也可以以點號 (`.`) 開頭來定義自定義運算符。這些運算符可以包含額外的點，例如 `.+.`。如果某個運算符不是以點號開頭的，那麼它就無法再包含另外的點號了。例如，`+.+` 就會被看作為一個 `+` 運算符後面跟著一個 `.+` 運算符。

雖然您可以用問號 `?` 來自定義運算符，但是這個運算符不能只包含單獨的一個問號。此外，雖然運算符可以包含一個驚嘆號 `!`，但是前綴運算符不能夠以問號或者驚嘆號開頭。

> 注意  
> 以下這些標記 `=`、`->`、`//`、`/*`、`*/`、`.`、`<`（前綴運算符）、`&`、`?`、`?`（中綴運算符）、`>`（後綴運算符）、`!` 、`?` 是被系統保留的。這些符號不能被重載，也不能用於自定義運算符。

運算符兩側的空白被用來區分該運算符是否為前綴運算符、後綴運算符或二元運算符。規則總結如下：

* 如果運算符兩側都有空白或兩側都無空白，將被看作二元運算符。例如：`a+++b` 和 `a +++ b` 當中的 `+++` 運算符會被看作二元運算符。
* 如果運算符只有左側空白，將被看作一元前綴運算符。例如 `a +++b` 中的 `+++` 運算符會被看做是一元前綴運算符。 
* 如果運算符只有右側空白，將被看作一元後綴運算符。例如 `a+++ b` 中的 `+++` 運算符會被看作是一元後綴運算符。
* 如果運算符左側沒有空白並緊跟 `.`，將被看作一元後綴運算符。例如 `a+++.b` 中的 `+++` 運算符會被視為一元後綴運算符（即上式被視為 `a+++ .b` 而不是 `a +++ .b`）。

鑒於這些規則，運算符前的字符 `(`、`[` 和 `{`，運算符後的字符 `)`、`]` 和 `}`，以及字符 `,`、`;` 和 `:` 都被視為空白。

以上規則需注意一點，如果預定義運算符 `!` 或 `?` 左側沒有空白，則不管右側是否有空白都將被看作後綴運算符。如果將 `?` 用作可選鏈式調用運算符，左側必須無空白。如果用於條件運算符 `? :`，必須兩側都有空白。

在某些特定的設計中 ，以 `<` 或 `>` 開頭的運算符會被分離成兩個或多個符號，剩余部分可能會以同樣的方式被再次分離。因此，在 `Dictionary<String, Array<Int>>` 中沒有必要添加空白來消除閉合字符 `>` 的歧義。在這個例子中， 閉合字符 `>` 不會被視為單獨的符號，因而不會被錯誤解析為 `>>` 運算符。

要學習如何自定義運算符，請參考 [自定義運算符](../chapter2/25_Advanced_Operators.html#custom_operators) 和 [運算符聲明](05_Declarations.html#operator_declaration)。要學習如何重載運算符，請參考 [運算符函數](../chapter2/25_Advanced_Operators.html#operator_functions)。

> 運算符語法  

<a id="operator"></a>
> *運算符* → [*頭部運算符*](#operator-head) [*運算符字符組*](#operator-characters)<sub>可選</sub>  
> *運算符* → [*頭部點運算符*](#dot-operator-head) [*點運算符字符組*](#dot-operator-characters)<sub>可選</sub>    

<a id="operator-head"></a>    
> *頭部運算符* → **/** | **=** | **-** | **+** | **!** | __*__ | **%** | **<** | **>** | **&** | **|** | **^** | **~** | **?**  
> *頭部運算符* → U+00A1–U+00A7    
> *頭部運算符* → U+00A9 或 U+00AB    
> *頭部運算符* → U+00AC 或 U+00AE    
> *頭部運算符* → U+00B0–U+00B1，U+00B6，U+00BB，U+00BF，U+00D7，或 U+00F7    
> *頭部運算符* → U+2016–U+2017 或 U+2020–U+2027    
> *頭部運算符* → U+2030–U+203E    
> *頭部運算符* → U+2041–U+2053    
> *頭部運算符* → U+2055–U+205E    
> *頭部運算符* → U+2190–U+23FF    
> *頭部運算符* → U+2500–U+2775    
> *頭部運算符* → U+2794–U+2BFF    
> *頭部運算符* → U+2E00–U+2E7F    
> *頭部運算符* → U+3001–U+3003    
> *頭部運算符* → U+3008–U+3030 

<a id="operator-character"></a>       
> *運算符字符* → [*頭部運算符*](#operator-head)    
> *運算符字符* → U+0300–U+036F    
> *運算符字符* → U+1DC0–U+1DFF    
> *運算符字符* → U+20D0–U+20FF    
> *運算符字符* → U+FE00–U+FE0F    
> *運算符字符* → U+FE20–U+FE2F    
> *運算符字符* → U+E0100–U+E01EF  
> <a id="operator-characters"></a>
> *運算符字符組* → [*運算符字符*](#operator-character) [*運算符字符組*](#operator-characters)<sub>可選</sub>    

<a id="dot-operator-head"></a>    
> *頭部點運算符* → **..**    
> <a id="dot-operator-character"></a> 
> *點運算符字符* → **.** | [*運算符字符*](#operator-character)    
> <a id="dot-operator-characters"></a> 
> *點運算符字符組* → [*點運算符字符*](#dot-operator-character) [*點運算符字符組*](#dot-operator-characters)<sub>可選</sub>

<a id="binary-operator"></a>
> *二元運算符* → [*運算符*](#operator)  
> <a id="prefix-operator"></a>
> *前綴運算符* → [*運算符*](#operator)  
> <a id="postfix-operator"></a>
> *後綴運算符* → [*運算符*](#operator)  
