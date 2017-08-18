# 語法總結（Summary of the Grammar）
-----

> 1.0
> 翻譯：[stanzhai](https://github.com/stanzhai)
> 校對：[xielingwang](https://github.com/xielingwang)

> 2.0
> 翻譯+校對：[miaosiqi](https://github.com/miaosiqi)

本頁包含內容：

* [語句（Statements）](#statements)
* [泛型參數（Generic Parameters and Arguments）](#generic_parameters_and_arguments)
* [聲明（Declarations）](#declarations)
* [模式（Patterns）](#patterns)
* [屬性（Attributes）](#attributes)
* [表達式（Expressions）](#expressions)
* [詞法結構（Lexical Structure）](#lexical_structure)
* [類型（Types）](#types)

<a name="statements"></a>
## 語句

> 語句語法  
> *語句* → [*表達式*](../chapter3/04_Expressions.html#expression) **;** _可選_  
> *語句* → [*聲明*](../chapter3/05_Declarations.html#declaration) **;** _可選_  
> *語句* → [*循環語句*](../chapter3/10_Statements.html#loop_statement) **;** _可選_  
> *語句* → [*分支語句*](../chapter3/10_Statements.html#branch_statement) **;** _可選_  
> *語句* → [*標記語句(Labeled Statement)*](../chapter3/10_Statements.html#labeled_statement)  
> *語句* → [*控制轉移語句*](../chapter3/10_Statements.html#control_transfer_statement) **;** _可選_  
> *語句* → [*延遲語句*](TODO) **;** _可選_ 

> *語句* → [*執行語句*](TODO) **;** _可選_ 

> *多條語句(Statements)* → [*語句*](../chapter3/10_Statements.html#statement) [*多條語句(Statements)*](../chapter3/10_Statements.html#statements) _可選_  

<!-- -->

> 循環語句語法  
> *循環語句* → [*for語句*](../chapter3/10_Statements.html#for_statement)  
> *循環語句* → [*for-in語句*](../chapter3/10_Statements.html#for_in_statement)  
> *循環語句* → [*while語句*](../chapter3/10_Statements.html#wheetatype類型ile_statement)  
> *循環語句* → [*repeat-while語句*](../chapter3/10_Statements.html#do_while_statement)  

<!-- -->

> For 循環語法  
> *for語句* → **for** [*for初始條件*](../chapter3/10_Statements.html#for_init) _可選_ **;** [*表達式*](../chapter3/04_Expressions.html#expression) _可選_ **;** [*表達式*](../chapter3/04_Expressions.html#expression) _可選_ [*代碼塊*](../chapter3/05_Declarations.html#code_block)  
> *for語句* → **for** **(** [*for初始條件*](../chapter3/10_Statements.html#for_init) _可選_ **;** [*表達式*](../chapter3/04_Expressions.html#expression) _可選_ **;** [*表達式*](../chapter3/04_Expressions.html#expression) _可選_ **)** [*代碼塊*](../chapter3/05_Declarations.html#code_block)  
> *for初始條件* → [*變量聲明*](../chapter3/05_Declarations.html#variable_declaration) | [*表達式集*](../chapter3/04_Expressions.html#expression_list)  

<!-- -->

> For-In 循環語法  
> *for-in語句* → **for case** _可選_ [*模式*](../chapter3/07_Patterns.html#pattern) **in** [*表達式*](../chapter3/04_Expressions.html#expression) [*代碼塊*](../chapter3/05_Declarations.html#code_block) [*where從句*](TODO) _可選_

<!-- -->

> While 循環語法  
> *while語句* → **while** [*條件從句*](../chapter3/10_Statements.html#while_condition) [*代碼塊*](../chapter3/05_Declarations.html#code_block)  
> *條件從句* → [*表達式*](TODO)

> *條件從句* → [*表達式*](TODO) *,* [*表達式集*]()

>*條件從句* → [*表達式集*](TODO)

> *條件從句* → [*可用條件 (availability-condition*)](TODO) *|* [*表達式集*]()

> *條件集* → [*條件*](TODO) *|* [*條件*](TODO) *,* [*條件集*]()

> *條件* → [*可用條件(availability-condition)*](TODO) *|* [*個例條件(case-condition)*](TODO) *|* [*可選綁定條件(optional-binding-condition)*](TODO)

> *個例條件(case-condition)* → **case** [*模式*](TODO) [*構造器*](TODO) [*where從句*](TODO)_可選_ 

> *可選綁定條件(optional-binding-condition)* → [*可選綁定頭(optional-binding-head)*](TODO) [*可選綁定連續集(optional-binding-continuation-list)*](TODO) _可選_ [*where從句*](TODO) _可選_

> *可選綁定頭(optional-binding-head)* → **let** [*模式 構造器*](TODO) *|* **var**  [*模式 構造器*](TODO)

> *可選綁定連續集(optional-binding-contiuation-list)* → [*可選綁定連續(optional-binding-contiuation)*](TODO) *|* [*可選綁定連續(optional-binding-contiuation)*](TODO) *，* [*可選綁定連續集(optional-binding-contiuation-list)*](TODO)

> *可選綁定連續(optional-binding-continuation)* → [*模式 構造器*](TODO) *|* [*可選綁定頭(optional-binding-head)*](TODO) 

<!-- -->
> Repeat-While語句語法
*repeat-while-statement* → **repeat** [*代碼塊*](TODO) **while** [*表達式*](TODO)  

<!-- -->

> 分支語句語法  
> *分支語句* → [*if語句*](../chapter3/10_Statements.html#if_statement) 

> *分支語句* → [*guard語句*](TODO)  

> *分支語句* → [*switch語句*](../chapter3/10_Statements.html#switch_statement)  

<!-- -->

> If語句語法  
> *if語句* → **if** [*條件從句*](TODO) [*代碼塊*](TODO) [*else從句(Clause)*](TODO) _可選_  
 
> *else從句(Clause)* → **else** [*代碼塊*](../chapter3/05_Declarations.html#code_block) | **else** [*if語句*](../chapter3/10_Statements.html#if_statement)  


<!-- -->
>Guard 語句語法
>*guard語句* → **guard** [*條件從句*](TODO) **else** [*代碼塊*](TODO) 

 
<!-- -->

> Switch語句語法  
> *switch語句* → **switch** [*表達式*](../chapter3/04_Expressions.html#expression) **{** [*SwitchCase*](../chapter3/10_Statements.html#switch_cases) _可選_ **}**  
> *SwitchCase集* → [*SwitchCase*](../chapter3/10_Statements.html#switch_case) [*SwitchCase集*](../chapter3/10_Statements.html#switch_cases) _可選_  
> *SwitchCase* → [*case標簽*](../chapter3/10_Statements.html#case_label) [*多條語句(Statements)*](../chapter3/10_Statements.html#statements) | [*default標簽*](../chapter3/10_Statements.html#default_label) [*多條語句(Statements)*](../chapter3/10_Statements.html#statements)  
> *SwitchCase* → [*case標簽*](../chapter3/10_Statements.html#case_label) **;** | [*default標簽*](../chapter3/10_Statements.html#default_label) **;**  
> *case標簽* → **case** [*case項集*](../chapter3/10_Statements.html#case_item_list) **:**  
> *case項集* → [*模式*](../chapter3/07_Patterns.html#pattern) [*where-clause*](../chapter3/10_Statements.html#guard_clause) _可選_ | [*模式*](../chapter3/07_Patterns.html#pattern) [*where-clause*](../chapter3/10_Statements.html#guard_clause) _可選_ **,** [*case項集*](../chapter3/10_Statements.html#case_item_list)  
> *default標簽* → **default** **:**  
> *where從句* → **where** [*where表達式*](TODO)  
> *where表達式* → [*表達式*](TODO)  

<!-- -->

> 標記語句語法  
> *標記語句(Labeled Statement)* → [*語句標簽*](../chapter3/10_Statements.html#statement_label) [*循環語句*](../chapter3/10_Statements.html#loop_statement) | [*語句標簽*](../chapter3/10_Statements.html#statement_label) [*if語句*](../chapter3/10_Statements.html#switch_statement) | [*語句標簽*](TODY) [*switch語句*](TODY) 
> *語句標簽* → [*標簽名稱*](../chapter3/10_Statements.html#label_name) **:**  
> *標簽名稱* → [*標識符*](../chapter3/02_Lexical_Structure.html#identifier)  

<!-- -->

> 控制傳遞語句(Control Transfer Statement) 語法  
> *控制傳遞語句* → [*break語句*](../chapter3/10_Statements.html#break_statement)  
> *控制傳遞語句* → [*continue語句*](../chapter3/10_Statements.html#continue_statement)  
> *控制傳遞語句* → [*fallthrough語句*](../chapter3/10_Statements.html#fallthrough_statement)  
> *控制傳遞語句* → [*return語句*](../chapter3/10_Statements.html#return_statement) 
> *控制傳遞語句* → [*throw語句*](TODO) 

<!-- -->

> Break 語句語法  
> *break語句* → **break** [*標簽名稱*](../chapter3/10_Statements.html#label_name) _可選_  

<!-- -->

> Continue 語句語法  
> *continue語句* → **continue** [*標簽名稱*](../chapter3/10_Statements.html#label_name) _可選_  

<!-- -->

> Fallthrough 語句語法  
> *fallthrough語句* → **fallthrough**  

<!-- -->

> Return 語句語法  
> *return語句* → **return** [*表達式*](../chapter3/04_Expressions.html#expression) _可選_  

<!-- -->
>可用條件(Availability Condition)語法

>*可用條件(availability-condition)* → **#available** **(** [*多可用參數*(availability-arguments)](TODO) **)** 

>*多可用參數(availability- arguments)* → [*可用參數(availability-argument)*](TODO)|[*可用參數(availability-argument)*](TODO) , [多可用參數(availability-arguments)](TODO)

>*可用參數(availability- argument)* → [*平台名(platform-name)*](TODO) [*平台版本(platform-version)*](TODO)

>*可用參數(availability- argument)* → *

>*平台名* → **iOS** | **iOSApplicationExtension**

>*平台名* → **OSX** | **OSXApplicationExtension**

>*平台名* → **watchOS** 

>*平台版本* → [*十進制數(decimal-digits)*](TODO)

>*平台版本* → [*十進制數(decimal-digits)*](TODO) . [*十進制數(decimal-digits)*](TODO) 

>*平台版本* → [*十進制數(decimal-digits)*](TODO) **.** [*十進制數(decimal-digits)*](TODO) **.** [*十進制數（decimal-digits)*](TODO))

<!-- -->
>拋出語句(Throw Statement)語法

>*拋出語句(throw-statement)* → **throw** [*表達式(expression)*](TODO)

<!-- -->
>延遲語句 (defer-statement)語法

>*延遲語句(defer-statement)* → **defer** [*代碼塊*](TODO)

<!-- -->
>執行語句(do-statement)語法

>*執行語句(do-statement)* → **do** [*代碼塊*](TODO) [*catch-clauses*](TODO) _可選_

>*catch-clauses* → [*catch-clause*](TODO) [*catch-clauses*](TODO) _可選_

>*catch-clauses* → **catch** [*模式(pattern)*](TODO) _可選_  [*where-clause*](TODO) _可選_ [*代碼塊(code-block)*](TODO) _可選_



<a name="generic_parameters_and_arguments"></a>
## 泛型參數

> 泛型形參從句(Generic Parameter Clause) 語法  
> *泛型參數從句* → **<** [*泛型參數集*](GenericParametersAndArguments.html#generic_parameter_list) [*約束從句*](GenericParametersAndArguments.html#requirement_clause) _可選_ **>**  
> *泛型參數集* → [*泛形參數*](GenericParametersAndArguments.html#generic_parameter) | [*泛形參數*](GenericParametersAndArguments.html#generic_parameter) **,** [*泛型參數集*](GenericParametersAndArguments.html#generic_parameter_list)  
> *泛形參數* → [*類型名稱*](../chapter3/03_Types.html#type_name)  
> *泛形參數* → [*類型名稱*](../chapter3/03_Types.html#type_name) **:** [*類型標識*](../chapter3/03_Types.html#type_identifier)  
> *泛形參數* → [*類型名稱*](../chapter3/03_Types.html#type_name) **:** [*協議合成類型*](../chapter3/03_Types.html#protocol_composition_type)  
> *約束從句* → **where** [*約束集*](GenericParametersAndArguments.html#requirement_list)  
> *約束集* → [*約束*](GenericParametersAndArguments.html#requirement) | [*約束*](GenericParametersAndArguments.html#requirement) **,** [*約束集*](GenericParametersAndArguments.html#requirement_list)  
> *約束* → [*一致性約束*](GenericParametersAndArguments.html#conformance_requirement) | [*同類型約束*](GenericParametersAndArguments.html#same_type_requirement)  
> *一致性約束* → [*類型標識*](../chapter3/03_Types.html#type_identifier) **:** [*類型標識*](../chapter3/03_Types.html#type_identifier)  
> *一致性約束* → [*類型標識*](../chapter3/03_Types.html#type_identifier) **:** [*協議合成類型*](../chapter3/03_Types.html#protocol_composition_type)  
> *同類型約束* → [*類型標識*](../chapter3/03_Types.html#type_identifier) **==** [*類型*](../chapter3/03_Types.html#type_identifier)  

<!-- -->

> 泛型實參從句語法  
> *(泛型參數從句Generic Argument Clause)* → **<** [*泛型參數集*](GenericParametersAndArguments.html#generic_argument_list) **>**  
> *泛型參數集* → [*泛型參數*](GenericParametersAndArguments.html#generic_argument) | [*泛型參數*](GenericParametersAndArguments.html#generic_argument) **,** [*泛型參數集*](GenericParametersAndArguments.html#generic_argument_list)  
> *泛型參數* → [*類型*](../chapter3/03_Types.html#type)  

<a name="declarations"></a>
## 聲明 (Declarations) 

> 聲明語法  
> *聲明* → [*導入聲明*](../chapter3/05_Declarations.html#import_declaration)  
> *聲明* → [*常量聲明*](../chapter3/05_Declarations.html#constant_declaration)  
> *聲明* → [*變量聲明*](../chapter3/05_Declarations.html#variable_declaration)  
> *聲明* → [*類型別名聲明*](../chapter3/05_Declarations.html#typealias_declaration)  
> *聲明* → [*函數聲明*](../chapter3/05_Declarations.html#function_declaration)  
> *聲明* → [*枚舉聲明*](../chapter3/05_Declarations.html#enum_declaration)  
> *聲明* → [*結構體聲明*](../chapter3/05_Declarations.html#struct_declaration)  
> *聲明* → [*類聲明*](../chapter3/05_Declarations.html#class_declaration)  
> *聲明* → [*協議聲明*](../chapter3/05_Declarations.html#protocol_declaration)  
> *聲明* → [*構造器聲明*](../chapter3/05_Declarations.html#initializer_declaration)  
> *聲明* → [*析構器聲明*](../chapter3/05_Declarations.html#deinitializer_declaration)  
> *聲明* → [*擴展聲明*](../chapter3/05_Declarations.html#extension_declaration)  
> *聲明* → [*下標聲明*](../chapter3/05_Declarations.html#subscript_declaration)  
> *聲明* → [*運算符聲明*](../chapter3/05_Declarations.html#operator_declaration)  
> *聲明(Declarations)集* → [*聲明*](../chapter3/05_Declarations.html#declaration) [*聲明(Declarations)集*](../chapter3/05_Declarations.html#declarations) _可選_  
 

<!-- -->

> 頂級(Top Level) 聲明語法  
> *頂級聲明* → [*多條語句(Statements)*](../chapter3/10_Statements.html#statements) _可選_  

<!-- -->

> 代碼塊語法  
> *代碼塊* → **{** [*多條語句(Statements)*](../chapter3/10_Statements.html#statements) _可選_ **}**  

<!-- -->

> 導入(Import)聲明語法  
> *導入聲明* → [*屬性(Attributes)集*](../chapter3/06_Attributes.html#attributes) _可選_ **import** [*導入類型*](../chapter3/05_Declarations.html#import_kind) _可選_ [*導入路徑*](../chapter3/05_Declarations.html#import_path)  
> *導入類型* → **typealias** | **struct** | **class** | **enum** | **protocol** | **var** | **func**  
> *導入路徑* → [*導入路徑標識符*](../chapter3/05_Declarations.html#import_path_identifier) | [*導入路徑標識符*](../chapter3/05_Declarations.html#import_path_identifier) **.** [*導入路徑*](../chapter3/05_Declarations.html#import_path)  
> *導入路徑標識符* → [*標識符*](../chapter3/02_Lexical_Structure.html#identifier) | [*運算符*](../chapter3/02_Lexical_Structure.html#operator)  

<!-- -->

> 常數聲明語法  
> *常量聲明* → [*屬性(Attributes)集*](../chapter3/06_Attributes.html#attributes) _可選_ [*聲明修改符(Modifiers)集*](../chapter3/05_Declarations.html#declaration_specifiers) _可選_ **let** [*模式構造器集*](../chapter3/05_Declarations.html#pattern_initializer_list)  
> *模式構造器集* → [*模式構造器*](../chapter3/05_Declarations.html#pattern_initializer) | [*模式構造器*](../chapter3/05_Declarations.html#pattern_initializer) **,** [*模式構造器集*](../chapter3/05_Declarations.html#pattern_initializer_list)  
> *模式構造器* → [*模式*](../chapter3/07_Patterns.html#pattern) [*構造器*](../chapter3/05_Declarations.html#initializer) _可選_  
> *構造器* → **=** [*表達式*](../chapter3/04_Expressions.html#expression)  

<!-- -->

> 變量聲明語法  
> *變量聲明* → [*變量聲明頭(Head)*](../chapter3/05_Declarations.html#variable_declaration_head) [*模式構造器集*](../chapter3/05_Declarations.html#pattern_initializer_list)  
> *變量聲明* → [*變量聲明頭(Head)*](../chapter3/05_Declarations.html#variable_declaration_head) [*變量名*](../chapter3/05_Declarations.html#variable_name) [*類型注解*](../chapter3/03_Types.html#type_annotation) [*代碼塊*](../chapter3/05_Declarations.html#code_block)  
> *變量聲明* → [*變量聲明頭(Head)*](../chapter3/05_Declarations.html#variable_declaration_head) [*變量名*](../chapter3/05_Declarations.html#variable_name) [*類型注解*](../chapter3/03_Types.html#type_annotation) [*getter-setter塊*](../chapter3/05_Declarations.html#getter_setter_block)  
> *變量聲明* → [*變量聲明頭(Head)*](../chapter3/05_Declarations.html#variable_declaration_head) [*變量名*](../chapter3/05_Declarations.html#variable_name) [*類型注解*](../chapter3/03_Types.html#type_annotation) [*getter-setter關鍵字(Keyword)塊*](../chapter3/05_Declarations.html#getter_setter_keyword_block)  
> *變量聲明* → [*變量聲明頭(Head)*](../chapter3/05_Declarations.html#variable_declaration_head) [*變量名*](../chapter3/05_Declarations.html#variable_name) [*類型注解*](../chapter3/03_Types.html#type_annotation) [*構造器*](../chapter3/05_Declarations.html#initializer) _可選_ [*willSet-didSet代碼塊*](../chapter3/05_Declarations.html#willSet_didSet_block)  
> *變量聲明頭(Head)* → [*屬性(Attributes)集*](../chapter3/06_Attributes.html#attributes) _可選_ [*聲明修改符(Modifers)集*](../chapter3/05_Declarations.html#declaration_specifiers) _可選_ **var**  
> *變量名稱* → [*標識符*](../chapter3/02_Lexical_Structure.html#identifier)  
> *getter-setter塊* → **{** [*getter從句*](../chapter3/05_Declarations.html#getter_clause) [*setter從句*](../chapter3/05_Declarations.html#setter_clause) _可選_ **}**  
> *getter-setter塊* → **{** [*setter從句*](../chapter3/05_Declarations.html#setter_clause) [*getter從句*](../chapter3/05_Declarations.html#getter_clause) **}**  
> *getter從句* → [*屬性(Attributes)集*](../chapter3/06_Attributes.html#attributes) _可選_ **get** [*代碼塊*](../chapter3/05_Declarations.html#code_block)  
> *setter從句* → [*屬性(Attributes)集*](../chapter3/06_Attributes.html#attributes) _可選_ **set** [*setter名稱*](../chapter3/05_Declarations.html#setter_name) _可選_ [*代碼塊*](../chapter3/05_Declarations.html#code_block)  
> *setter名稱* → **(** [*標識符*](../chapter3/02_Lexical_Structure.html#identifier) **)**  
> *getter-setter關鍵字(Keyword)塊* → **{** [*getter關鍵字(Keyword)從句*](../chapter3/05_Declarations.html#getter_keyword_clause) [*setter關鍵字(Keyword)從句*](../chapter3/05_Declarations.html#setter_keyword_clause) _可選_ **}**  
> *getter-setter關鍵字(Keyword)塊* → **{** [*setter關鍵字(Keyword)從句*](../chapter3/05_Declarations.html#setter_keyword_clause) [*getter關鍵字(Keyword)從句*](../chapter3/05_Declarations.html#getter_keyword_clause) **}**  
> *getter關鍵字(Keyword)從句* → [*屬性(Attributes)集*](../chapter3/06_Attributes.html#attributes) _可選_ **get**  
> *setter關鍵字(Keyword)從句* → [*屬性(Attributes)集*](../chapter3/06_Attributes.html#attributes) _可選_ **set**  
> *willSet-didSet代碼塊* → **{** [*willSet從句*](../chapter3/05_Declarations.html#willSet_clause) [*didSet從句*](../chapter3/05_Declarations.html#didSet_clause) _可選_ **}**  
> *willSet-didSet代碼塊* → **{** [*didSet從句*](../chapter3/05_Declarations.html#didSet_clause) [*willSet從句*](../chapter3/05_Declarations.html#willSet_clause) **}**  
> *willSet從句* → [*屬性(Attributes)集*](../chapter3/06_Attributes.html#attributes) _可選_ **willSet** [*setter名稱*](../chapter3/05_Declarations.html#setter_name) _可選_ [*代碼塊*](../chapter3/05_Declarations.html#code_block)  
> *didSet從句* → [*屬性(Attributes)集*](../chapter3/06_Attributes.html#attributes) _可選_ **didSet** [*setter名稱*](../chapter3/05_Declarations.html#setter_name) _可選_ [*代碼塊*](../chapter3/05_Declarations.html#code_block)  

<!-- -->

> 類型別名聲明語法  
> *類型別名聲明* → [*類型別名頭(Head)*](../chapter3/05_Declarations.html#typealias_head) [*類型別名賦值*](../chapter3/05_Declarations.html#typealias_assignment)  
> *類型別名頭(Head)* → [*屬性*](TODO) _可選_ [*訪問級別修改符(access-level-modifier)*](TODO) **typealias** [*類型別名名稱*](../chapter3/05_Declarations.html#typealias_name)  
> *類型別名名稱* → [*標識符*](../chapter3/02_Lexical_Structure.html#identifier)  
> *類型別名賦值* → **=** [*類型*](../chapter3/03_Types.html#type)  

<!-- -->

> 函數聲明語法  
> *函數聲明* → [*函數頭*](../chapter3/05_Declarations.html#function_head) [*函數名*](../chapter3/05_Declarations.html#function_name) [*泛型參數從句*](GenericParametersAndArguments.html#generic_parameter_clause) _可選_ [*函數簽名(Signature)*](../chapter3/05_Declarations.html#function_signature) [*函數體*](../chapter3/05_Declarations.html#function_body)  
> *函數頭* → [*屬性(Attributes)集*](../chapter3/06_Attributes.html#attributes) _可選_ [*聲明描述符(Specifiers)集*](../chapter3/05_Declarations.html#declaration_specifiers) _可選_ **func**  
> *函數名* → [*標識符*](../chapter3/02_Lexical_Structure.html#identifier) | [*運算符*](../chapter3/02_Lexical_Structure.html#operator)  
> *函數簽名(Signature)* → [*parameter-clauses*](../chapter3/05_Declarations.html#parameter_clauses) **throws** _可選_ [*函數結果*](../chapter3/05_Declarations.html#function_result) _可選_ 

> *函數簽名(Signature)* → [*parameter-clauses*](../chapter3/05_Declarations.html#parameter_clauses) **rethrows** [*函數結果*](../chapter3/05_Declarations.html#function_result) _可選_  
> *函數結果* → **->** [*屬性(Attributes)集*](../chapter3/06_Attributes.html#attributes) _可選_ [*類型*](../chapter3/03_Types.html#type)  
> *函數體* → [*代碼塊*](../chapter3/05_Declarations.html#code_block)  
> *參數從句* → [*參數從句*](../chapter3/05_Declarations.html#parameter_clause) [*parameter-clauses*](../chapter3/05_Declarations.html#parameter_clauses) _可選_  
> *參數從句* → **(** **)** | **(** [*參數集*](../chapter3/05_Declarations.html#parameter_list) **...** _可選_ **)**  
> *參數集* → [*參數*](../chapter3/05_Declarations.html#parameter) | [*參數*](../chapter3/05_Declarations.html#parameter) **,** [*參數集*](../chapter3/05_Declarations.html#parameter_list)  
> *參數* → **inout** _可選_ **let** _可選_ [*外部參數名*](../chapter3/05_Declarations.html#parameter_name) _可選_ [*本地參數名*](../chapter3/05_Declarations.html#local_parameter_name) _可選_ [*類型注解*](../chapter3/03_Types.html#type_annotation) [*默認參數從句*](../chapter3/05_Declarations.html#default_argument_clause) _可選_  
> *參數* → **inout** _可選_ **var** [*外部參數名*](../chapter3/05_Declarations.html#parameter_name) [*本地參數名*](../chapter3/05_Declarations.html#local_parameter_name) _可選_ [*類型注解*](../chapter3/03_Types.html#type_annotation) [*默認參數從句*](../chapter3/05_Declarations.html#default_argument_clause) _可選_  
> *參數* → [*屬性(Attributes)集*](../chapter3/06_Attributes.html#attributes) _可選_ [*類型*](../chapter3/03_Types.html#type)  
> *外部參數名* → [*標識符*](../chapter3/02_Lexical_Structure.html#identifier) | **_**  
> *本地參數名* → [*標識符*](../chapter3/02_Lexical_Structure.html#identifier) | **_**  
> *默認參數從句* → **=** [*表達式*](../chapter3/04_Expressions.html#expression)  

<!-- -->

> 枚舉聲明語法  
> *枚舉聲明* → [*屬性(Attributes)集*](../chapter3/06_Attributes.html#attributes) _可選_ [*訪問級別修改器(access-level-modifier)*](TODO) _可選_ [*聯合式枚舉*](../chapter3/05_Declarations.html#union_style_enum) 

> *枚舉聲明* → [*屬性(Attributes)集*](../chapter3/06_Attributes.html#attributes) _可選_ [*訪問級別修改器(access-level-modifier)*](TODO) _可選_ [*原始值式枚舉(raw-value-style-enum)*](TODO)

> *聯合式枚舉* → **enum** [*枚舉名*](../chapter3/05_Declarations.html#enum_name) [*泛型參數從句*](GenericParametersAndArguments.html#generic_parameter_clause) _可選_ [*類型繼承從句(type-inheritance-clause)*](TODO) _可選_ **{** [*聯合樣式枚舉成員*](../chapter3/05_Declarations.html#union_style_enum_members) _可選_ **}**  

> *聯合樣式枚舉成員* → [*union-style-enum-member*](../chapter3/05_Declarations.html#union_style_enum_member) [*聯合樣式枚舉成員*](../chapter3/05_Declarations.html#union_style_enum_members) _可選_  

> *聯合樣式枚舉成員* → [*聲明*](../chapter3/05_Declarations.html#declaration) | [*聯合式(Union Style)的枚舉case從句*](../chapter3/05_Declarations.html#union_style_enum_case_clause)  

> *聯合式(Union Style)的枚舉case從句* → [*屬性(Attributes)集*](../chapter3/06_Attributes.html#attributes) _可選_ **case** [*聯合式(Union Style)的枚舉case集*](../chapter3/05_Declarations.html#union_style_enum_case_list)  
> *聯合式(Union Style)的枚舉case集* → [*聯合式(Union Style)的case*](../chapter3/05_Declarations.html#union_style_enum_case) | [*聯合式(Union Style)的case*](../chapter3/05_Declarations.html#union_style_enum_case) **,** [*聯合式(Union Style)的枚舉case集*](../chapter3/05_Declarations.html#union_style_enum_case_list)  
> *聯合式(Union Style)的枚舉case* → [*枚舉的case名*](../chapter3/05_Declarations.html#enum_case_name) [*元組類型*](../chapter3/03_Types.html#tuple_type) _可選_  
> *枚舉名* → [*標識符*](../chapter3/02_Lexical_Structure.html#identifier)  
> *枚舉的case名* → [*標識符*](../chapter3/02_Lexical_Structure.html#identifier)  
> *原始值式枚舉* → **enum** [*枚舉名*](../chapter3/05_Declarations.html#enum_name) [*泛型參數從句*](GenericParametersAndArguments.html#generic_parameter_clause) _可選_ **:** [*類型標識*](../chapter3/03_Types.html#type_identifier) **{** [*原始值式枚舉成員集*](../chapter3/05_Declarations.html#raw_value_style_enum_members) _可選_ **}**  
> *原始值式枚舉成員集* → [*原始值式枚舉成員*](../chapter3/05_Declarations.html#raw_value_style_enum_member) [*原始值式枚舉成員集*](../chapter3/05_Declarations.html#raw_value_style_enum_members) _可選_  
> *原始值式枚舉成員* → [*聲明*](../chapter3/05_Declarations.html#declaration) | [*原始值式枚舉case從句*](../chapter3/05_Declarations.html#raw_value_style_enum_case_clause)  
> *原始值式枚舉case從句* → [*屬性(Attributes)集*](../chapter3/06_Attributes.html#attributes) _可選_ **case** [*原始值式枚舉case集*](../chapter3/05_Declarations.html#raw_value_style_enum_case_list)  
> *原始值式枚舉case集* → [*原始值式枚舉case*](../chapter3/05_Declarations.html#raw_value_style_enum_case) | [*原始值式枚舉case*](../chapter3/05_Declarations.html#raw_value_style_enum_case) **,** [*原始值式枚舉case集*](../chapter3/05_Declarations.html#raw_value_style_enum_case_list)  
> *原始值式枚舉case* → [*枚舉的case名*](../chapter3/05_Declarations.html#enum_case_name) [*原始值賦值*](../chapter3/05_Declarations.html#raw_value_assignment) _可選_  
> *原始值賦值* → **=** [*字面量*](../chapter3/02_Lexical_Structure.html#literal) 
> *原始值字面量(raw-value-literal)* → [*數值字面量*](TODO) | [*字符串字面量*](TODO) | [*布爾字面量*](TODO)

<!-- -->

> 結構體聲明語法  
> *結構體聲明* → [*屬性(Attributes)集*](../chapter3/06_Attributes.html#attributes) _可選_ [*訪問級別修改器(access-level-modifier)*](TODO) _可選_ **struct** [*結構體名稱*](../chapter3/05_Declarations.html#struct_name) [*泛型參數從句*](GenericParametersAndArguments.html#generic_parameter_clause) _可選_ [*類型繼承從句*](../chapter3/03_Types.html#type_inheritance_clause) _可選_ [*結構體主體*](../chapter3/05_Declarations.html#struct_body)  
> *結構體名稱* → [*標識符*](../chapter3/02_Lexical_Structure.html#identifier)  
> *結構體主體* → **{** [*聲明(Declarations)集*](../chapter3/05_Declarations.html#declarations) _可選_ **}**  

<!-- -->

> 類聲明語法  
> *類聲明* → [*屬性(Attributes)集*](../chapter3/06_Attributes.html#attributes) _可選_ [*訪問級別修改器(access-level-modifier)*](TODO) **class** [*類名*](../chapter3/05_Declarations.html#class_name) [*泛型參數從句*](GenericParametersAndArguments.html#generic_parameter_clause) _可選_ [*類型繼承從句*](../chapter3/03_Types.html#type_inheritance_clause) _可選_ [*類主體*](../chapter3/05_Declarations.html#class_body)  
> *類名* → [*標識符*](../chapter3/02_Lexical_Structure.html#identifier)  
> *類主體* → **{** [*聲明(Declarations)集*](../chapter3/05_Declarations.html#declarations) _可選_ **}**  

<!-- -->

> 協議(Protocol)聲明語法  
> *協議聲明* → [*屬性(Attributes)集*](../chapter3/06_Attributes.html#attributes) _可選_[*訪問級別修改器(access-level-modifier)*](TODO)  **protocol** [*協議名*](../chapter3/05_Declarations.html#protocol_name) [*類型繼承從句*](../chapter3/03_Types.html#type_inheritance_clause) _可選_ [*協議主體*](../chapter3/05_Declarations.html#protocol_body)  
> *協議名* → [*標識符*](../chapter3/02_Lexical_Structure.html#identifier)  
> *協議主體* → **{** [*協議成員聲明(Declarations)集*](../chapter3/05_Declarations.html#protocol_member_declarations) _可選_ **}**  
> *協議成員聲明* → [*協議屬性聲明*](../chapter3/05_Declarations.html#protocol_property_declaration)  
> *協議成員聲明* → [*協議方法聲明*](../chapter3/05_Declarations.html#protocol_method_declaration)  
> *協議成員聲明* → [*協議構造器聲明*](../chapter3/05_Declarations.html#protocol_initializer_declaration)  
> *協議成員聲明* → [*協議下標聲明*](../chapter3/05_Declarations.html#protocol_subscript_declaration)  
> *協議成員聲明* → [*協議關聯類型聲明*](../chapter3/05_Declarations.html#protocol_associated_type_declaration)  
> *協議成員聲明(Declarations)集* → [*協議成員聲明*](../chapter3/05_Declarations.html#protocol_member_declaration) [*協議成員聲明(Declarations)集*](../chapter3/05_Declarations.html#protocol_member_declarations) _可選_  

<!-- -->

> 協議屬性聲明語法  
> *協議屬性聲明* → [*變量聲明頭(Head)*](../chapter3/05_Declarations.html#variable_declaration_head) [*變量名*](../chapter3/05_Declarations.html#variable_name) [*類型注解*](../chapter3/03_Types.html#type_annotation) [*getter-setter關鍵字(Keyword)塊*](../chapter3/05_Declarations.html#getter_setter_keyword_block)  

<!-- -->

> 協議方法聲明語法  
> *協議方法聲明* → [*函數頭*](../chapter3/05_Declarations.html#function_head) [*函數名*](../chapter3/05_Declarations.html#function_name) [*泛型參數從句*](GenericParametersAndArguments.html#generic_parameter_clause) _可選_ [*函數簽名(Signature)*](../chapter3/05_Declarations.html#function_signature)  

<!-- -->

> 協議構造器聲明語法  
> *協議構造器聲明* → [*構造器頭(Head)*](../chapter3/05_Declarations.html#initializer_head) [*泛型參數從句*](GenericParametersAndArguments.html#generic_parameter_clause) _可選_ [*參數從句*](../chapter3/05_Declarations.html#parameter_clause)  

<!-- -->

> 協議下標聲明語法  
> *協議下標聲明* → [*下標頭(Head)*](../chapter3/05_Declarations.html#subscript_head) [*下標結果(Result)*](../chapter3/05_Declarations.html#subscript_result) [*getter-setter關鍵字(Keyword)塊*](../chapter3/05_Declarations.html#getter_setter_keyword_block)  

<!-- -->

> 協議關聯類型聲明語法  
> *協議關聯類型聲明* → [*類型別名頭(Head)*](../chapter3/05_Declarations.html#typealias_head) [*類型繼承從句*](../chapter3/03_Types.html#type_inheritance_clause) _可選_ [*類型別名賦值*](../chapter3/05_Declarations.html#typealias_assignment) _可選_  

<!-- -->

> 構造器聲明語法  
> *構造器聲明* → [*構造器頭(Head)*](../chapter3/05_Declarations.html#initializer_head) [*泛型參數從句*](GenericParametersAndArguments.html#generic_parameter_clause) _可選_ [*參數從句*](../chapter3/05_Declarations.html#parameter_clause) [*構造器主體*](../chapter3/05_Declarations.html#initializer_body)  
> *構造器頭(Head)* → [*屬性(Attributes)集*](../chapter3/06_Attributes.html#attributes) _可選_ [*聲明修改器集(declaration-modifiers)*](TODO) _可選_  **init**  
> *構造器頭(Head)* → [*屬性(Attributes)集*](../chapter3/06_Attributes.html#attributes) _可選_ [*聲明修改器集(declaration-modifiers)*](TODO) _可選_  **init ?**

> *構造器頭(Head)* → [*屬性(Attributes)集*](../chapter3/06_Attributes.html#attributes) _可選_ [*聲明修改器集(declaration-modifiers)*](TODO) _可選_  **init !**    

> *構造器主體* → [*代碼塊*](../chapter3/05_Declarations.html#code_block)  

<!-- -->

> 析構器聲明語法  
> *析構器聲明* → [*屬性(Attributes)集*](../chapter3/06_Attributes.html#attributes) _可選_ **deinit** [*代碼塊*](../chapter3/05_Declarations.html#code_block)  

<!-- -->

> 擴展(Extension)聲明語法  
> *擴展聲明* → [*訪問級別修改器*](TODO) _可選_ **extension** [*類型標識*](../chapter3/03_Types.html#type_identifier) [*類型繼承從句*](../chapter3/03_Types.html#type_inheritance_clause) _可選_ [*extension-body*](../chapter3/05_Declarations.html#extension_body)  
> *extension-body* → **{** [*聲明(Declarations)集*](../chapter3/05_Declarations.html#declarations) _可選_ **}**  

<!-- -->

> 下標聲明語法  
> *下標聲明* → [*下標頭(Head)*](../chapter3/05_Declarations.html#subscript_head) [*下標結果(Result)*](../chapter3/05_Declarations.html#subscript_result) [*代碼塊*](../chapter3/05_Declarations.html#code_block)  
> *下標聲明* → [*下標頭(Head)*](../chapter3/05_Declarations.html#subscript_head) [*下標結果(Result)*](../chapter3/05_Declarations.html#subscript_result) [*getter-setter塊*](../chapter3/05_Declarations.html#getter_setter_block)  
> *下標聲明* → [*下標頭(Head)*](../chapter3/05_Declarations.html#subscript_head) [*下標結果(Result)*](../chapter3/05_Declarations.html#subscript_result) [*getter-setter關鍵字(Keyword)塊*](../chapter3/05_Declarations.html#getter_setter_keyword_block)  
> *下標頭(Head)* → [*屬性(Attributes)集*](../chapter3/06_Attributes.html#attributes) _可選_ [*聲明修改器(declaration-modifiers)*](TODO) _可選_ **subscript** [*參數從句*](../chapter3/05_Declarations.html#parameter_clause)  
> *下標結果(Result)* → **->** [*屬性(Attributes)集*](../chapter3/06_Attributes.html#attributes) _可選_ [*類型*](../chapter3/03_Types.html#type)  

<!-- -->

> 運算符聲明語法  
> *運算符聲明* → [*前置運算符聲明*](../chapter3/05_Declarations.html#prefix_operator_declaration) | [*後置運算符聲明*](../chapter3/05_Declarations.html#postfix_operator_declaration) | [*中置運算符聲明*](../chapter3/05_Declarations.html#infix_operator_declaration)  
> *前置運算符聲明* → **prefix** **運算符**  [*運算符*](../chapter3/02_Lexical_Structure.html#operator) **{** **}**  
> *後置運算符聲明* → **postfix** **運算符**  [*運算符*](../chapter3/02_Lexical_Structure.html#operator) **{** **}**  
> *中置運算符聲明* → **infix** **運算符**  [*運算符*](../chapter3/02_Lexical_Structure.html#operator) **{** [*中置運算符屬性集*](../chapter3/05_Declarations.html#infix_operator_attributes) _可選_ **}**  
> *中置運算符屬性集* → [*優先級從句*](../chapter3/05_Declarations.html#precedence_clause) _可選_ [*結和性從句*](../chapter3/05_Declarations.html#associativity_clause) _可選_  
> *優先級從句* → **precedence** [*優先級水平*](../chapter3/05_Declarations.html#precedence_level)  
> *優先級水平* → 數值 0 到 255，首末項包括在內 
> *結和性從句* → **associativity** [*結和性*](../chapter3/05_Declarations.html#associativity)  
> *結和性* → **left** | **right** | **none**  

<!-- -->
聲明修改器語法
> *聲明修改器* → **類** | **便捷(convenience)** | **動態(dynamic)** | **final** | **中置(infix)** | **lazy** | **可變(mutating)** | **不可變(nonmutating)** | **可選(optional)** | **改寫(override)** | **後置** | **前置** | **required** | **static** | **unowned** | **unowned(safe)** | **unowned(unsafe)** | **弱(weak)**

> *聲明修改器* → [*訪問級別聲明器(access-level-modifier)*](TODO)

> *聲明修改集* → [*聲明修改器*](TODO) [*聲明修改器集*](TODO) _可選_

> *訪問級別修改器* → **內部的** | **內部的(set)** 

> *訪問級別修改器* → **私有的** | **私有的(set)** 

> *訪問級別修改器* → **公共的** 
| **公共的(set)** 

> *訪問級別修改器集* →[*訪問級別修改器*](TODO) [*訪問級別修改器集*](TODO) _可選_ 

<a name="patterns"></a>
## 模式

> 模式(Patterns) 語法  
> *模式* → [*通配符模式*](../chapter3/07_Patterns.html#wildcard_pattern) [*類型注解*](../chapter3/03_Types.html#type_annotation) _可選_  
> *模式* → [*標識符模式*](../chapter3/07_Patterns.html#identifier_pattern) [*類型注解*](../chapter3/03_Types.html#type_annotati Value Bindingon ) _可選_  
> *模式* → [*值綁定模式*](../chapter3/07_Patterns.html#value_binding_pattern)  
> *模式* → [*元組模式*](../chapter3/07_Patterns.html#tuple_pattern) [*類型注解*](../chapter3/03_Types.html#type_annotation) _可選_  
 
> *模式* → [*枚舉個例模式*](../chapter3/07_Patterns.html#enum_case_pattern)  
> *模式* → [*可選模式*](TODO) 
> *模式* → [*類型轉換模式*](../chapter3/07_Patterns.html#type_casting_pattern)  
> *模式* → [*表達式模式*](../chapter3/07_Patterns.html#expression_pattern)  

<!-- -->

> 通配符模式語法  
> *通配符模式* → **_**  

<!-- -->

> 標識符模式語法  
> *標識符模式* → [*標識符*](../chapter3/02_Lexical_Structure.html#identifier)  

<!-- -->

> 值綁定(Value Binding)模式語法  
> *值綁定模式* → **var** [*模式*](../chapter3/07_Patterns.html#pattern) | **let** [*模式*](../chapter3/07_Patterns.html#pattern)  

<!-- -->

> 元組模式語法  
> *元組模式* → **(** [*元組模式元素集*](../chapter3/07_Patterns.html#tuple_pattern_element_list) _可選_ **)**  
> *元組模式元素集* → [*元組模式元素*](../chapter3/07_Patterns.html#tuple_pattern_element) | [*元組模式元素*](../chapter3/07_Patterns.html#tuple_pattern_element) **,** [*元組模式元素集*](../chapter3/07_Patterns.html#tuple_pattern_element_list)  
> *元組模式元素* → [*模式*](../chapter3/07_Patterns.html#pattern)  

<!-- -->

> 枚舉用例模式語法  
> *enum-case-pattern* → [*類型標識*](../chapter3/03_Types.html#type_identifier) _可選_ **.** [*枚舉的case名*](../chapter3/05_Declarations.html#enum_case_name) [*元組模式*](../chapter3/07_Patterns.html#tuple_pattern) _可選_ 

<!-- -->
> 可選模式語法
> *可選模式* → [*識別符模式*](TODO) **?**


<!-- -->

> 類型轉換模式語法  
> *類型轉換模式(type-casting-pattern)* → [*is模式*](../chapter3/07_Patterns.html#is_pattern) | [*as模式*](../chapter3/07_Patterns.html#as_pattern)  
> *is模式* → **is** [*類型*](../chapter3/03_Types.html#type)  
> *as模式* → [*模式*](../chapter3/07_Patterns.html#pattern) **as** [*類型*](../chapter3/03_Types.html#type)  

<!-- -->

> 表達式模式語法  
> *表達式模式* → [*表達式*](../chapter3/04_Expressions.html#expression)  

<a name="attributes"></a>
## 屬性

> 屬性語法  
> *屬性* → **@** [*屬性名*](../chapter3/06_Attributes.html#attribute_name) [*屬性參數從句*](../chapter3/06_Attributes.html#attribute_argument_clause) _可選_  
> *屬性名* → [*標識符*](../chapter3/02_Lexical_Structure.html#identifier)  
> *屬性參數從句* → **(** [*平衡令牌集*](../chapter3/06_Attributes.html#balanced_tokens) _可選_ **)**  
> *屬性(Attributes)集* → [*屬性*](../chapter3/06_Attributes.html#attribute) [*屬性(Attributes)集*](../chapter3/06_Attributes.html#attributes) _可選_  
> *平衡令牌集* → [*平衡令牌*](../chapter3/06_Attributes.html#balanced_token) [*平衡令牌集*](../chapter3/06_Attributes.html#balanced_tokens) _可選_  
> *平衡令牌* → **(** [*平衡令牌集*](../chapter3/06_Attributes.html#balanced_tokens) _可選_ **)**  
> *平衡令牌* → **[** [*平衡令牌集*](../chapter3/06_Attributes.html#balanced_tokens) _可選_ **]**  
> *平衡令牌* → **{** [*平衡令牌集*](../chapter3/06_Attributes.html#balanced_tokens) _可選_ **}**  
> *平衡令牌* → **任意標識符, 關鍵字, 字面量或運算符**  
> *平衡令牌* → **任意標點除了(, ), [, ], {, 或 }**

<a name="expressions"></a>
## 表達式

> 表達式語法  
> *表達式* → [*try-operator*](TODO) _可選_ [*前置表達式*](../chapter3/04_Expressions.html#prefix_expression) [*二元表達式集*](../chapter3/04_Expressions.html#binary_expressions) _可選_  
> *表達式集* → [*表達式*](../chapter3/04_Expressions.html#expression) | [*表達式*](../chapter3/04_Expressions.html#expression) **,** [*表達式集*](../chapter3/04_Expressions.html#expression_list)  

<!-- -->

> 前置表達式語法  
> *前置表達式* → [*前置運算符*](../chapter3/02_Lexical_Structure.html#prefix_operator) _可選_ [*後置表達式*](../chapter3/04_Expressions.html#postfix_expression)  
> *前置表達式* → [*寫入寫出(in-out)表達式*](../chapter3/04_Expressions.html#in_out_expression)  
> *寫入寫出(in-out)表達式* → **&** [*標識符*](../chapter3/02_Lexical_Structure.html#identifier) 

<!-- -->
> try表達式語法
> *try-operator* → **try** | **try !**  


<!-- -->

> 二元表達式語法  
> *二元表達式* → [*二元運算符*](../chapter3/02_Lexical_Structure.html#binary_operator) [*前置表達式*](../chapter3/04_Expressions.html#prefix_expression)  
> *二元表達式* → [*賦值運算符*](../chapter3/04_Expressions.html#assignment_operator) [*try運算符*](TODO) _可選_ [*前置表達式*](../chapter3/04_Expressions.html#prefix_expression)  
> *二元表達式* → [*條件運算符*](../chapter3/04_Expressions.html#conditional_operator) [*try運算符*](TODO) _可選_ [*前置表達式*](../chapter3/04_Expressions.html#prefix_expression)  
> *二元表達式* → [*類型轉換運算符*](../chapter3/04_Expressions.html#type_casting_operator)  
> *二元表達式集* → [*二元表達式*](../chapter3/04_Expressions.html#binary_expression) [*二元表達式集*](../chapter3/04_Expressions.html#binary_expressions) _可選_  

<!-- -->

> 賦值運算符語法  
> *賦值運算符* → **=**  

<!-- -->

> 三元條件運算符語法  
> *三元條件運算符* → **?** [*表達式*](../chapter3/04_Expressions.html#expression) **:**  

<!-- -->

> 類型轉換運算符語法  
> *類型轉換運算符* → **is** [*類型*](../chapter3/03_Types.html#type) 

> *類型轉換運算符* → **as** [*類型*](../chapter3/03_Types.html#type) 

> *類型轉換運算符* → **as ?** [*類型*](../chapter3/03_Types.html#type) 

> *類型轉換運算符* → **as !** [*類型*](../chapter3/03_Types.html#type) 



<!-- -->

> 主表達式語法  
> *主表達式* → [*標識符*](../chapter3/02_Lexical_Structure.html#identifier) [*泛型參數從句*](GenericParametersAndArguments.html#generic_argument_clause) _可選_  
> *主表達式* → [*字面量表達式*](../chapter3/04_Expressions.html#literal_expression)  
> *主表達式* → [*self表達式*](../chapter3/04_Expressions.html#self_expression)  
> *主表達式* → [*超類表達式*](../chapter3/04_Expressions.html#superclass_expression)  
> *主表達式* → [*閉包表達式*](../chapter3/04_Expressions.html#closure_expression)  
> *主表達式* → [*圓括號表達式*](../chapter3/04_Expressions.html#parenthesized_expression)  
> *主表達式* → [*隱式成員表達式*](../chapter3/04_Expressions.html#implicit_member_expression)  
> *主表達式* → [*通配符表達式*](../chapter3/04_Expressions.html#wildcard_expression)  

<!-- -->

> 字面量表達式語法  
> *字面量表達式* → [*字面量*](../chapter3/02_Lexical_Structure.html#literal)  
> *字面量表達式* → [*數組字面量*](../chapter3/04_Expressions.html#array_literal) | [*字典字面量*](../chapter3/04_Expressions.html#dictionary_literal)  
> *字面量表達式* → **&#95;&#95;FILE&#95;&#95;** | **&#95;&#95;LINE&#95;&#95;** | **&#95;&#95;COLUMN&#95;&#95;** | **&#95;&#95;FUNCTION&#95;&#95;**  
> *數組字面量* → **[** [*數組字面量項集*](../chapter3/04_Expressions.html#array_literal_items) _可選_ **]**  
> *數組字面量項集* → [*數組字面量項*](../chapter3/04_Expressions.html#array_literal_item) **,** _可選_ | [*數組字面量項*](../chapter3/04_Expressions.html#array_literal_item) **,** [*數組字面量項集*](../chapter3/04_Expressions.html#array_literal_items)  
> *數組字面量項* → [*表達式*](../chapter3/04_Expressions.html#expression)  
> *字典字面量* → **[** [*字典字面量項集*](../chapter3/04_Expressions.html#dictionary_literal_items) **]** | **[** **:** **]**  
> *字典字面量項集* → [*字典字面量項*](../chapter3/04_Expressions.html#dictionary_literal_item) **,** _可選_ | [*字典字面量項*](../chapter3/04_Expressions.html#dictionary_literal_item) **,** [*字典字面量項集*](../chapter3/04_Expressions.html#dictionary_literal_items)  
> *字典字面量項* → [*表達式*](../chapter3/04_Expressions.html#expression) **:** [*表達式*](../chapter3/04_Expressions.html#expression)  

<!-- -->

> Self 表達式語法  
> *self表達式* → **self**  
> *self表達式* → **self** **.** [*標識符*](../chapter3/02_Lexical_Structure.html#identifier)  
> *self表達式* → **self** **[** [*表達式*](../chapter3/04_Expressions.html#expression) **]**  
> *self表達式* → **self** **.** **init**  

<!-- -->

> 超類表達式語法  
> *超類表達式* → [*超類方法表達式*](../chapter3/04_Expressions.html#superclass_method_expression) | [*超類下標表達式*](../chapter3/04_Expressions.html#超類下標表達式) | [*超類構造器表達式*](../chapter3/04_Expressions.html#superclass_initializer_expression)  
> *超類方法表達式* → **super** **.** [*標識符*](../chapter3/02_Lexical_Structure.html#identifier)  
> *超類下標表達式* → **super** **[** [*表達式*](../chapter3/04_Expressions.html#expression) **]**  
> *超類構造器表達式* → **super** **.** **init**  

<!-- -->

> 閉包表達式語法  
> *閉包表達式* → **{** [*閉包簽名(Signational)*](../chapter3/04_Expressions.html#closure_signature) _可選_ [*多條語句(Statements)*](../chapter3/10_Statements.html#statements) **}**  
> *閉包簽名(Signational)* → [*參數從句*](../chapter3/05_Declarations.html#parameter_clause) [*函數結果*](../chapter3/05_Declarations.html#function_result) _可選_ **in**  
> *閉包簽名(Signational)* → [*標識符集*](../chapter3/02_Lexical_Structure.html#identifier_list) [*函數結果*](../chapter3/05_Declarations.html#function_result) _可選_ **in**  
> *閉包簽名(Signational)* → [*捕獲(Capature)集*](../chapter3/04_Expressions.html#capture_list) [*參數從句*](../chapter3/05_Declarations.html#parameter_clause) [*函數結果*](../chapter3/05_Declarations.html#function_result) _可選_ **in**  
> *閉包簽名(Signational)* → [*捕獲(Capature)集*](../chapter3/04_Expressions.html#capture_list) [*標識符集*](../chapter3/02_Lexical_Structure.html#identifier_list) [*函數結果*](../chapter3/05_Declarations.html#function_result) _可選_ **in**  
> *閉包簽名(Signational)* → [*捕獲(Capature)集*](../chapter3/04_Expressions.html#capture_list) **in**  
> *捕獲(Capature)集* → **[** [*捕獲(Capature)說明符*](../chapter3/04_Expressions.html#capture_specifier) [*表達式*](../chapter3/04_Expressions.html#expression) **]**  
> *捕獲(Capature)說明符* → **weak** | **unowned** | **unowned(safe)** | **unowned(unsafe)**  

<!-- -->

> 隱式成員表達式語法  
> *隱式成員表達式* → **.** [*標識符*](../chapter3/02_Lexical_Structure.html#identifier)  

<!-- -->

> 圓括號表達式(Parenthesized Expression)語法  
> *圓括號表達式* → **(** [*表達式元素集*](../chapter3/04_Expressions.html#expression_element_list) _可選_ **)**  
> *表達式元素集* → [*表達式元素*](../chapter3/04_Expressions.html#expression_element) | [*表達式元素*](../chapter3/04_Expressions.html#expression_element) **,** [*表達式元素集*](../chapter3/04_Expressions.html#expression_element_list)  
> *表達式元素* → [*表達式*](../chapter3/04_Expressions.html#expression) | [*標識符*](../chapter3/02_Lexical_Structure.html#identifier) **:** [*表達式*](../chapter3/04_Expressions.html#expression)  

<!-- -->

> 通配符表達式語法  
> *通配符表達式* → **_**  

<!-- -->

> 後置表達式語法  
> *後置表達式* → [*主表達式*](../chapter3/04_Expressions.html#primary_expression)  
> *後置表達式* → [*後置表達式*](../chapter3/04_Expressions.html#postfix_expression) [*後置運算符*](../chapter3/02_Lexical_Structure.html#postfix_operator)  
> *後置表達式* → [*函數調用表達式*](../chapter3/04_Expressions.html#function_call_expression)  
> *後置表達式* → [*構造器表達式*](../chapter3/04_Expressions.html#initializer_expression)  
> *後置表達式* → [*顯示成員表達式*](../chapter3/04_Expressions.html#explicit_member_expression)  
> *後置表達式* → [*後置self表達式*](../chapter3/04_Expressions.html#postfix_self_expression)  
> *後置表達式* → [*動態類型表達式*](../chapter3/04_Expressions.html#dynamic_type_expression)  
> *後置表達式* → [*下標表達式*](../chapter3/04_Expressions.html#subscript_expression)  
> *後置表達式* → [*強制取值(Forced Value)表達式*](../chapter3/04_Expressions.html#forced_value_expression)  
> *後置表達式* → [*可選鏈(Optional Chaining)表達式*](../chapter3/04_Expressions.html#optional_chaining_expression)  

<!-- -->

> 函數調用表達式語法  
> *函數調用表達式* → [*後置表達式*](../chapter3/04_Expressions.html#postfix_expression) [*圓括號表達式*](../chapter3/04_Expressions.html#parenthesized_expression)  
> *函數調用表達式* → [*後置表達式*](../chapter3/04_Expressions.html#postfix_expression) [*圓括號表達式*](../chapter3/04_Expressions.html#parenthesized_expression) _可選_ [*後置閉包(Trailing Closure)*](../chapter3/04_Expressions.html#trailing_closure)  
> *後置閉包(Trailing Closure)* → [*閉包表達式*](../chapter3/04_Expressions.html#closure_expression)  

<!-- -->

> 構造器表達式語法  
> *構造器表達式* → [*後置表達式*](../chapter3/04_Expressions.html#postfix_expression) **.** **init**  

<!-- -->

> 顯式成員表達式語法  
> *顯示成員表達式* → [*後置表達式*](../chapter3/04_Expressions.html#postfix_expression) **.** [*十進制數字*](../chapter3/02_Lexical_Structure.html#decimal_digit)  
> *顯示成員表達式* → [*後置表達式*](../chapter3/04_Expressions.html#postfix_expression) **.** [*標識符*](../chapter3/02_Lexical_Structure.html#identifier) [*泛型參數從句*](GenericParametersAndArguments.html#generic_argument_clause) _可選_  

<!-- -->

> 後置Self 表達式語法  
> *後置self表達式* → [*後置表達式*](../chapter3/04_Expressions.html#postfix_expression) **.** **self**  

<!-- -->

> 動態類型表達式語法  
> *動態類型表達式* → [*後置表達式*](../chapter3/04_Expressions.html#postfix_expression) **.** **dynamicType**  

<!-- -->

> 附屬腳本表達式語法  
> *附屬腳本表達式* → [*後置表達式*](../chapter3/04_Expressions.html#postfix_expression) **[** [*表達式集*](../chapter3/04_Expressions.html#expression_list) **]**  

<!-- -->

> 強制取值(Forced Value)語法  
> *強制取值(Forced Value)表達式* → [*後置表達式*](../chapter3/04_Expressions.html#postfix_expression) **!**  

<!-- -->

> 可選鏈表達式語法  
> *可選鏈表達式* → [*後置表達式*](../chapter3/04_Expressions.html#postfix_expression) **?**  

<a name="lexical_structure"></a>
## 詞法結構

> 標識符語法  
> *標識符* → [*標識符頭(Head)*](../chapter3/02_Lexical_Structure.html#identifier_head) [*標識符字符集*](../chapter3/02_Lexical_Structure.html#identifier_characters) _可選_  
> *標識符* → [*標識符頭(Head)*](../chapter3/02_Lexical_Structure.html#identifier_head) [*標識符字符集*](../chapter3/02_Lexical_Structure.html#identifier_characters) _可選_  
> *標識符* → [*隱式參數名*](../chapter3/02_Lexical_Structure.html#implicit_parameter_name)  
> *標識符集* → [*標識符*](../chapter3/02_Lexical_Structure.html#identifier) | [*標識符*](../chapter3/02_Lexical_Structure.html#identifier) **,** [*標識符集*](../chapter3/02_Lexical_Structure.html#identifier_list)  
> *標識符頭(Head)* → Upper- or lowercase letter A through Z

> *標識符頭(Head)* → _

> *標識符頭(Head)* → U+00A8, U+00AA, U+00AD, U+00AF, U+00B2–U+00B5, or U+00B7–U+00BA  
> *標識符頭(Head)* → U+00BC–U+00BE, U+00C0–U+00D6, U+00D8–U+00F6, or U+00F8–U+00FF  
> *標識符頭(Head)* → U+0100–U+02FF, U+0370–U+167F, U+1681–U+180D, or U+180F–U+1DBF  
> *標識符頭(Head)* → U+1E00–U+1FFF  
> *標識符頭(Head)* → U+200B–U+200D, U+202A–U+202E, U+203F–U+2040, U+2054, or U+2060–U+206F  
> *標識符頭(Head)* → U+2070–U+20CF, U+2100–U+218F, U+2460–U+24FF, or U+2776–U+2793  
> *標識符頭(Head)* → U+2C00–U+2DFF or U+2E80–U+2FFF  
> *標識符頭(Head)* → U+3004–U+3007, U+3021–U+302F, U+3031–U+303F, or U+3040–U+D7FF  
> *標識符頭(Head)* → U+F900–U+FD3D, U+FD40–U+FDCF, U+FDF0–U+FE1F, or U+FE30–U+FE44  
> *標識符頭(Head)* → U+FE47–U+FFFD  
> *標識符頭(Head)* → U+10000–U+1FFFD, U+20000–U+2FFFD, U+30000–U+3FFFD, or U+40000–U+4FFFD  
> *標識符頭(Head)* → U+50000–U+5FFFD, U+60000–U+6FFFD, U+70000–U+7FFFD, or U+80000–U+8FFFD  
> *標識符頭(Head)* → U+90000–U+9FFFD, U+A0000–U+AFFFD, U+B0000–U+BFFFD, or U+C0000–U+CFFFD  
> *標識符頭(Head)* → U+D0000–U+DFFFD or U+E0000–U+EFFFD  
> *標識符字符* → 數值 0 到 9  
> *標識符字符* → U+0300–U+036F, U+1DC0–U+1DFF, U+20D0–U+20FF, or U+FE20–U+FE2F  
> *標識符字符* → [*標識符頭(Head)*](../chapter3/02_Lexical_Structure.html#identifier_head)  
> *標識符字符集* → [*標識符字符*](../chapter3/02_Lexical_Structure.html#identifier_character) [*標識符字符集*](../chapter3/02_Lexical_Structure.html#identifier_characters) _可選_  
> *隱式參數名* → **$** [*十進制數字集*](../chapter3/02_Lexical_Structure.html#decimal_digits)  

<!-- -->

> 字面量語法  
> *字面量* → [*數值型字面量*](../chapter3/02_Lexical_Structure.html#integer_literal) | [*字符串字面量*](../chapter3/02_Lexical_Structure.html#floating_point_literal) | [*布爾字面量*](../chapter3/02_Lexical_Structure.html#string_literal) | [*空字面量*](TODO) 

> *數值型字面量* → **-** _可選_ [*整形字面量*](TODO) | **-** _可選_ [*浮點型字面量*](TODO)

> *布爾字面量* → **true** | **false**

> *空字面量* → **nil**

<!-- -->

> 整型字面量語法  
> *整型字面量* → [*二進制字面量*](../chapter3/02_Lexical_Structure.html#binary_literal)  
> *整型字面量* → [*八進制字面量*](../chapter3/02_Lexical_Structure.html#octal_literal)  
> *整型字面量* → [*十進制字面量*](../chapter3/02_Lexical_Structure.html#decimal_literal)  
> *整型字面量* → [*十六進制字面量*](../chapter3/02_Lexical_Structure.html#hexadecimal_literal)  
> *二進制字面量* → **0b** [*二進制數字*](../chapter3/02_Lexical_Structure.html#binary_digit) [*二進制字面量字符集*](../chapter3/02_Lexical_Structure.html#binary_literal_characters) _可選_  
> *二進制數字* → 數值 0 到 1  
> *二進制字面量字符* → [*二進制數字*](../chapter3/02_Lexical_Structure.html#binary_digit) | **_**  
> *二進制字面量字符集* → [*二進制字面量字符*](../chapter3/02_Lexical_Structure.html#binary_literal_character) [*二進制字面量字符集*](../chapter3/02_Lexical_Structure.html#binary_literal_characters) _可選_  
> *八進制字面量* → **0o** [*八進制數字*](../chapter3/02_Lexical_Structure.html#octal_digit) [*八進制字符集*](../chapter3/02_Lexical_Structure.html#octal_literal_characters) _可選_  
> *八進字數字* → 數值 0 到 7  
> *八進制字符* → [*八進制數字*](../chapter3/02_Lexical_Structure.html#octal_digit) | **_**  
> *八進制字符集* → [*八進制字符*](../chapter3/02_Lexical_Structure.html#octal_literal_character) [*八進制字符集*](../chapter3/02_Lexical_Structure.html#octal_literal_characters) _可選_  
> *十進制字面量* → [*十進制數字*](../chapter3/02_Lexical_Structure.html#decimal_digit) [*十進制字符集*](../chapter3/02_Lexical_Structure.html#decimal_literal_characters) _可選_  
> *十進制數字* → 數值 0 到 9  
> *十進制數字集* → [*十進制數字*](../chapter3/02_Lexical_Structure.html#decimal_digit) [*十進制數字集*](../chapter3/02_Lexical_Structure.html#decimal_digits) _可選_  
> *十進制字面量字符* → [*十進制數字*](../chapter3/02_Lexical_Structure.html#decimal_digit) | **_**  
> *十進制字面量字符集* → [*十進制字面量字符*](../chapter3/02_Lexical_Structure.html#decimal_literal_character) [*十進制字面量字符集*](../chapter3/02_Lexical_Structure.html#decimal_literal_characters) _可選_  
> *十六進制字面量* → **0x** [*十六進制數字*](../chapter3/02_Lexical_Structure.html#hexadecimal_digit) [*十六進制字面量字符集*](../chapter3/02_Lexical_Structure.html#hexadecimal_literal_characters) _可選_  
> *十六進制數字* → 數值 0 到 9, a through f, or A through F  
> *十六進制字符* → [*十六進制數字*](../chapter3/02_Lexical_Structure.html#hexadecimal_digit) | **_**  
> *十六進制字面量字符集* → [*十六進制字符*](../chapter3/02_Lexical_Structure.html#hexadecimal_literal_character) [*十六進制字面量字符集*](../chapter3/02_Lexical_Structure.html#hexadecimal_literal_characters) _可選_  

<!-- -->

> 浮點型字面量語法  
> *浮點數字面量* → [*十進制字面量*](../chapter3/02_Lexical_Structure.html#decimal_literal) [*十進制分數*](../chapter3/02_Lexical_Structure.html#decimal_fraction) _可選_ [*十進制指數*](../chapter3/02_Lexical_Structure.html#decimal_exponent) _可選_  
> *浮點數字面量* → [*十六進制字面量*](../chapter3/02_Lexical_Structure.html#hexadecimal_literal) [*十六進制分數*](../chapter3/02_Lexical_Structure.html#hexadecimal_fraction) _可選_ [*十六進制指數*](../chapter3/02_Lexical_Structure.html#hexadecimal_exponent)  
> *十進制分數* → **.** [*十進制字面量*](../chapter3/02_Lexical_Structure.html#decimal_literal)  
> *十進制指數* → [*浮點數e*](../chapter3/02_Lexical_Structure.html#floating_point_e) [*正負號*](../chapter3/02_Lexical_Structure.html#sign) _可選_ [*十進制字面量*](../chapter3/02_Lexical_Structure.html#decimal_literal)  
> *十六進制分數* → **.** [*十六進制數*](../chapter3/02_Lexical_Structure.html#hexadecimal_literal)   
 [*十六進制字面量字符集*](TODO)_可選_  
> *十六進制指數* → [*浮點數p*](../chapter3/02_Lexical_Structure.html#floating_point_p) [*正負號*](../chapter3/02_Lexical_Structure.html#sign) _可選_ [*十六進制字面量*](../chapter3/02_Lexical_Structure.html#hexadecimal_literal)  
> *浮點數e* → **e** | **E**  
> *浮點數p* → **p** | **P**  
> *正負號* → **+** | **-**  

<!-- -->

> 字符串型字面量語法  
> *字符串字面量* → **"** [*引用文本*](../chapter3/02_Lexical_Structure.html#quoted_text) **"**  
> *引用文本* → [*引用文本條目*](../chapter3/02_Lexical_Structure.html#quoted_text_item) [*引用文本*](../chapter3/02_Lexical_Structure.html#quoted_text) _可選_  
> *引用文本條目* → [*轉義字符*](../chapter3/02_Lexical_Structure.html#escaped_character)  
> *引用文本條目* → **(** [*表達式*](../chapter3/04_Expressions.html#expression) **)**  
> *引用文本條目* → 除了"­, \­, U+000A, or U+000D的所有Unicode的字符  
> *轉義字符* → **/0** | **\\** | **\t** | **\n** | **\r** | **\"** | **\'**  
> *轉義字符* → **\u** **{** [*十六進制標量數字集*](TODO) **}**   
> *unicode標量數字集* → Between one and eight hexadecimal digits

<!-- -->

> 運算符語法語法  
> *運算符* → [*運算符頭*](../chapter3/02_Lexical_Structure.html#operator_character) [*運算符字符集*](../chapter3/02_Lexical_Structure.html#operator) _可選_
> *運算符* → [*點運算符頭*](TODO) [*點運算符字符集*](TODO) _可選_  
> *運算符字符* → **/** | **=** | **-** | **+** | **!** | **&#42;** | **%** | **<** | **>** | **&** | **|** | **^** | **~** | **?**  
> *運算符頭* → U+00A1–U+00A7

> *運算符頭* → U+00A9 or U+00AB

> *運算符頭* →  U+00AC or U+00AE

> *運算符頭* → U+00B0–U+00B1, U+00B6, U+00BB, U+00BF, U+00D7, or U+00F7

> *運算符頭* → U+2016–U+2017 or U+2020–U+2027

> *運算符頭* → U+2030–U+203E

> *運算符頭* → U+2041–U+2053

> *運算符頭* → U+2055–U+205E

> *運算符頭* → U+2190–U+23FF

> *運算符頭* → U+2500–U+2775

> *運算符頭* → U+2794–U+2BFF

> *運算符頭* → U+2E00–U+2E7F

> *運算符頭* → U+3001–U+3003

> *運算符頭* → U+3008–U+3030

> *運算符字符* → [*運算符頭*](TODO)

> *運算符字符* → U+0300–U+036F

> *運算符字符* → U+1DC0–U+1DFF

> *運算符字符* → U+20D0–U+20FF

> *運算符字符* → U+FE00–U+FE0F

> *運算符字符* → U+FE20–U+FE2F

> *運算符字符* → U+E0100–U+E01EF

> *運算符字符集* → [*運算符字符*](TODO) [*運算符字符集*](TODO)_可選_

> *點運算符頭* → **..** 

> *點運算符字符* → **.** | [*運算符字符*](TODO)

> *點運算符字符集* → [*點運算符字符*](TODO) [*點運算符字符集*](TODO) _可選_
 
> *二元運算符* → [*運算符*](../chapter3/02_Lexical_Structure.html#operator)  
> *前置運算符* → [*運算符*](../chapter3/02_Lexical_Structure.html#operator)  
> *後置運算符* → [*運算符*](../chapter3/02_Lexical_Structure.html#operator)  

<a name="types"></a>
## 類型

> 類型語法  
> *類型* → [*數組類型*](../chapter3/03_Types.html#array_type) | [*字典類型*](TODO) | [*函數類型*](../chapter3/03_Types.html#function_type) | [*類型標識符*](../chapter3/03_Types.html#type_identifier) | [*元組類型*](../chapter3/03_Types.html#tuple_type) | [*可選類型*](../chapter3/03_Types.html#optional_type) | [*隱式解析可選類型*](../chapter3/03_Types.html#implicitly_unwrapped_optional_type) | [*協議合成類型*](../chapter3/03_Types.html#protocol_composition_type) | [*元型類型*](../chapter3/03_Types.html#metatype_type)  

<!-- -->

> 類型注解語法  
> *類型注解* → **:** [*屬性(Attributes)集*](../chapter3/06_Attributes.html#attributes) _可選_ [*類型*](../chapter3/03_Types.html#type)  

<!-- -->

> 類型標識語法  
> *類型標識* → [*類型名稱*](../chapter3/03_Types.html#type_name) [*泛型參數從句*](GenericParametersAndArguments.html#generic_argument_clause) _可選_ | [*類型名稱*](../chapter3/03_Types.html#type_name) [*泛型參數從句*](GenericParametersAndArguments.html#generic_argument_clause) _可選_ **.** [*類型標識符*](../chapter3/03_Types.html#type_identifier)  
> *類型名* → [*標識符*](../chapter3/02_Lexical_Structure.html#identifier)  

<!-- -->

> 元組類型語法  
> *元組類型* → **(** [*元組類型主體*](../chapter3/03_Types.html#tuple_type_body) _可選_ **)**  
> *元組類型主體* → [*元組類型的元素集*](../chapter3/03_Types.html#tuple_type_element_list) **...** _可選_  
> *元組類型的元素集* → [*元組類型的元素*](../chapter3/03_Types.html#tuple_type_element) | [*元組類型的元素*](../chapter3/03_Types.html#tuple_type_element) **,** [*元組類型的元素集*](../chapter3/03_Types.html#tuple_type_element_list)  
> *元組類型的元素* → [*屬性(Attributes)集*](../chapter3/06_Attributes.html#attributes) _可選_ **inout** _可選_ [*類型*](../chapter3/03_Types.html#type) | **inout** _可選_ [*元素名*](../chapter3/03_Types.html#element_name) [*類型注解*](../chapter3/03_Types.html#type_annotation)  
> *元素名* → [*標識符*](../chapter3/02_Lexical_Structure.html#identifier)  

<!-- -->

> 函數類型語法  
> *函數類型* → [*類型*](../chapter3/03_Types.html#type)  **throws** _可選_ **->** [*類型*](../chapter3/03_Types.html#type)  
> *函數類型* → [*類型*](TODO)  **rethrows** **->** [*類型*](TODO) 

<!-- -->

> 數組類型語法  
> *數組類型* → **[** [*類型*](../chapter3/03_Types.html#array_type) **]**  

<!-- -->
> 字典類型語法
> *字典類型* → **[** [*類型 **:** 類型*](TODO) **]**

<!-- -->

> 可選類型語法  
> *可選類型* → [*類型*](../chapter3/03_Types.html#type) **?**  

<!-- -->

> 隱式解析可選類型(Implicitly Unwrapped Optional Type)語法  
> *隱式解析可選類型* → [*類型*](../chapter3/03_Types.html#type) **!**  

<!-- -->

> 協議合成類型語法  
> *協議合成類型* → **protocol** **<** [*協議標識符集*](../chapter3/03_Types.html#protocol_identifier_list) _可選_ **>**  
> *協議標識符集* → [*協議標識符*](../chapter3/03_Types.html#protocol_identifier) | [*協議標識符*](../chapter3/03_Types.html#protocol_identifier) **,** [*協議標識符集*](../chapter3/03_Types.html#protocol_identifier_list)  
> *協議標識符* → [*類型標識符*](../chapter3/03_Types.html#type_identifier)  

<!-- -->

> 元(Metatype)類型語法  
> *元類型* → [*類型*](../chapter3/03_Types.html#type) **.** **Type** | [*類型*](../chapter3/03_Types.html#type) **.** **Protocol**  

<!-- -->

> 類型繼承從句語法  

> *類型繼承從句* → **:** [*類條件(class-requirement))*](TODO) **,** [*類型繼承集*](../chapter3/03_Types.html#type_inheritance_list) 

> *類型繼承從句* → **:** [*類條件(class-requirement))*](TODO) 

> *類型繼承從句* → **:** [*類型繼承集*](TODO)  

> *類型繼承集* → [*類型標識符*](../chapter3/03_Types.html#type_identifier) | [*類型標識符*](../chapter3/03_Types.html#type_identifier) **,** [*類型繼承集*](../chapter3/03_Types.html#type_inheritance_list)

> *類條件* → **class** 
