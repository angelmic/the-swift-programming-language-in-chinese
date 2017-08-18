# Swift 文檔修訂歷史

---

> 1.0
> 翻譯：[成都老碼團隊翻譯組-Arya](http://weibo.com/littlekok/)
> 校對：[成都老碼團隊翻譯組-Oberyn](http://weibo.com/u/5241713117)
[changkun](http://changkun.us/about/)
> 
> 1.1
> 翻譯：[成都老碼團隊翻譯組-Arya](http://weibo.com/littlekok/)
> 校對：[成都老碼團隊翻譯組-Oberyn](http://weibo.com/u/5241713117)
[changkun](http://changkun.us/about/)
> 
> 1.2
> 翻譯：[成都老碼團隊翻譯組-Arya](http://weibo.com/littlekok/)
> 校對：[成都老碼團隊翻譯組-Oberyn](http://weibo.com/u/5241713117)
[changkun](http://changkun.us/about/)
> 
> 2.0
> 翻譯+校對：[changkun](http://changkun.us/about/)
> 
> 2.1
> 翻譯+校對：[changkun](http://changkun.us/about/)
> 
> 2.2
> 翻譯+校對：[changkun](http://changkun.us/about/)
> 
> 3.0
> 翻譯+校對：[shanks](http://codebuild.me)，2016-10-06
> 
> 3.0.1
> 翻譯+校對：[shanks](http://codebuild.me)，2016-11-10

本頁面根據 [Document Revision History](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/RevisionHistory.html) 進行適配更新。

本頁內容包括：
-   [Swift 3.1 更新](#swift_3_1)
-   [Swift 3.0 更新](#swift_3_0)
-   [Swift 2.2 更新](#swift_2_2)
-   [Swift 2.1 更新](#swift_2_1)
-   [Swift 2.0 更新](#swift_2_0)
-   [Swift 1.2 更新](#swift_1_2)
-   [Swift 1.1 更新](#swift_1_1)
-   [Swift 1.0 更新](#swift_1_0)

<a name="swift_3_1"></a>
### Swift 3.1 更新

<table class="graybox" border="0" cellspacing="0" cellpadding="5">
<thead>
    <tr>
        <th scope="col" width="100">發布日期</th>
        <th scope="col">語法變更記錄</th>
    </tr>
</thead>
<tbody>
	 <tr>
    	<td scope="row">2016-10-27</td>
		<td>
			<ul class="list-bullet">
			 	<li>
	            更新至 Swift 3.0.1。
	        	</li>
	        	<li>
	            更新<a href="https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/AutomaticReferenceCounting.html#//apple_ref/doc/uid/TP40014097-CH20-ID48">自動引用計數</a>章節中關於 weak 和 unowned 引用的討論。
	        	</li>
	        	<li>
	            增加<a href="https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Declarations.html#//apple_ref/doc/uid/TP40014097-CH34-ID381">聲明標識符</a>章節中關於新的標識符`unowned`，`unowend(safe)`和`unowned(unsafe)`的描述。
	        	</li>
	        	<li>
	            增加<a href="https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/TypeCasting.html#//apple_ref/doc/uid/TP40014097-CH22-ID342">Any 和 AnyObject 的類型轉換</a>一節中關於使用類型`Any`作為可選值的描述。
	        	</li>
	        	<li>
	            更新<a href="https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Expressions.html#//apple_ref/doc/uid/TP40014097-CH32-ID383">表達式</a>章節，把括號表達式和元組表達式的描述分開。
	        	</li>
	    	</ul>
	    </td>
    </tr>
</tbody>
</table>


<a name="swift_3_0"></a>
### Swift 3.0 更新

<table class="graybox" border="0" cellspacing="0" cellpadding="5">
<thead>
    <tr>
        <th scope="col" width="100">發布日期</th>
        <th scope="col">語法變更記錄</th>
    </tr>
</thead>
<tbody>
	 <tr>
    <td scope="row">2016-09-13</td>
    <td><ul class="list-bullet">
        <li>
            更新至 Swift 3.0。
        </li>
        <li>
           更新<a href="https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Functions.html#//apple_ref/doc/uid/TP40014097-CH10-ID158">函數</a>章節中關於函數的討論，在<a href="https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Declarations.html#//apple_ref/doc/uid/TP40014097-CH34-ID362">函數定義</a>一節中，標明所有函數參數默認都有函數標簽。
        </li>
        <li>
           更新<a href="https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/AdvancedOperators.html#//apple_ref/doc/uid/TP40014097-CH27-ID28">高級操作符</a>章節中關於操作符的討論，現在你可以作為類型函數來實現，替代之前的全局函數實現方式。
        </li>
        <li>
           增加<a href="https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/AccessControl.html#//apple_ref/doc/uid/TP40014097-CH41-ID3">訪問控制</a>章節中關於對新的訪問級別描述符<code>open</code>和<code>fileprivate</code>的信息
        </li>
        <li>
           更新<a href="https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Declarations.html#//apple_ref/doc/uid/TP40014097-CH34-ID362">函數定義</a>一節中關於<code>inout</code>的討論，標明它放在參數類型的前面，替代之前放在參數名稱前面的方式。
        </li>
         <li>
           更新<a href="https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Declarations.html#//apple_ref/doc/uid/TP40014097-CH34-ID362">逃逸閉包</a>和<a href="https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Declarations.html#//apple_ref/doc/uid/TP40014097-CH34-ID362">自動閉包</a>還有<a href="https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Declarations.html#//apple_ref/doc/uid/TP40014097-CH34-ID362">屬性</a>章節中關於<code>@noescape</code>和<code>@autoclosure</code>的討論，現在他們是類型屬性，而不是定義屬性。
        </li>
         <li>
            增加<a href="https://developer.apple.com/library/prerelease/content/documentation/Swift/Conceptual/Swift_Programming_Language/AdvancedOperators.html#//apple_ref/doc/uid/TP40014097-CH27-ID28">高級操作符</a>章節中<a href="https://developer.apple.com/library/prerelease/content/documentation/Swift/Conceptual/Swift_Programming_Language/AdvancedOperators.html#//apple_ref/doc/uid/TP40014097-CH27-ID47">自定義中綴操作符的優先級</a>一節和<a href="https://developer.apple.com/library/prerelease/content/documentation/Swift/Conceptual/Swift_Programming_Language/Declarations.html#//apple_ref/doc/uid/TP40014097-CH34-ID351">定義</a>章節中<a href="https://developer.apple.com/library/prerelease/content/documentation/Swift/Conceptual/Swift_Programming_Language/Declarations.html#//apple_ref/doc/uid/TP40014097-CH34-ID550">優先級組聲明</a>一節中關於操作符優先級組的信息。

        </li>
         <li>
            更新一些討論：使用 macOS 替換掉 OS X， Error 替換掉 ErrorProtocol，和更新一些協議名稱，比如使用 ExpressibleByStringLiteral 替換掉 StringLiteralConvertible。

        </li>
 		  <li>
            更新<a href="https://developer.apple.com/library/prerelease/content/documentation/Swift/Conceptual/Swift_Programming_Language/Generics.html#//apple_ref/doc/uid/TP40014097-CH26-ID179">泛型</a>章節中<a href="https://developer.apple.com/library/prerelease/content/documentation/Swift/Conceptual/Swift_Programming_Language/Generics.html#//apple_ref/doc/uid/TP40014097-CH26-ID192">泛型 Where 語句</a>一節和<a href="https://developer.apple.com/library/prerelease/content/documentation/Swift/Conceptual/Swift_Programming_Language/GenericParametersAndArguments.html#//apple_ref/doc/uid/TP40014097-CH37-ID406">泛型形參和實參</a>章節，現在泛型的 where 語句寫在一個聲明的最後。


        </li>
        <li>
            更新<a href="https://developer.apple.com/library/prerelease/content/documentation/Swift/Conceptual/Swift_Programming_Language/Closures.html#//apple_ref/doc/uid/TP40014097-CH11-ID546">逃逸閉包</a>一節，現在閉包默認為非逃逸的(noescaping)。
        </li>
        <li>
             更新<a href="https://developer.apple.com/library/prerelease/content/documentation/Swift/Conceptual/Swift_Programming_Language/TheBasics.html#//apple_ref/doc/uid/TP40014097-CH5-ID309">基礎部分</a>章節中<a href="https://developer.apple.com/library/prerelease/content/documentation/Swift/Conceptual/Swift_Programming_Language/TheBasics.html#//apple_ref/doc/uid/TP40014097-CH5-ID333">可選綁定</a>一節和<a href="https://developer.apple.com/library/prerelease/content/documentation/Swift/Conceptual/Swift_Programming_Language/Statements.html#//apple_ref/doc/uid/TP40014097-CH33-ID428">語句</a>章節中<a href="https://developer.apple.com/library/prerelease/content/documentation/Swift/Conceptual/Swift_Programming_Language/Statements.html#//apple_ref/doc/uid/TP40014097-CH33-ID432">While 語句</a>一節，現在 if，while 和 guard 語句使用逗號分隔條件列表，不需要使用 where 語句。        
         </li>
         <li>
             增加<a href="https://developer.apple.com/library/prerelease/content/documentation/Swift/Conceptual/Swift_Programming_Language/ControlFlow.html#//apple_ref/doc/uid/TP40014097-CH9-ID120">控制流</a>章節中<a href="https://developer.apple.com/library/prerelease/content/documentation/Swift/Conceptual/Swift_Programming_Language/ControlFlow.html#//apple_ref/doc/uid/TP40014097-CH9-ID129">Switch</a>一節和<a href="https://developer.apple.com/library/prerelease/content/documentation/Swift/Conceptual/Swift_Programming_Language/Statements.html#//apple_ref/doc/uid/TP40014097-CH33-ID428">語句</a>章節中<a href="https://developer.apple.com/library/prerelease/content/documentation/Swift/Conceptual/Swift_Programming_Language/Statements.html#//apple_ref/doc/uid/TP40014097-CH33-ID436">Switch 語句</a>一節關於 switch cases 可以使用多模式的信息。        
         </li>
 		<li>
            更新<a href="https://developer.apple.com/library/prerelease/content/documentation/Swift/Conceptual/Swift_Programming_Language/Types.html#//apple_ref/doc/uid/TP40014097-CH31-ID449">函數類型</a>一節，現在函數參數標簽不包含在函數類型中。
        </li>
         <li>
             更新<a href="https://developer.apple.com/library/prerelease/content/documentation/Swift/Conceptual/Swift_Programming_Language/Protocols.html#//apple_ref/doc/uid/TP40014097-CH25-ID267">協議</a>章節中<a href="https://developer.apple.com/library/prerelease/content/documentation/Swift/Conceptual/Swift_Programming_Language/Protocols.html#//apple_ref/doc/uid/TP40014097-CH25-ID282">協議組合</a>一節和<a href="https://developer.apple.com/library/prerelease/content/documentation/Swift/Conceptual/Swift_Programming_Language/Types.html#//apple_ref/doc/uid/TP40014097-CH31-ID445">類型</a>章節中<a href="https://developer.apple.com/library/prerelease/content/documentation/Swift/Conceptual/Swift_Programming_Language/Types.html#//apple_ref/doc/uid/TP40014097-CH31-ID454">協議組合類型</a>一節關於使用新的 Protocol1 & Protocol2 語法的信息。        
         </li>
         <li>
            更新<a href="https://developer.apple.com/library/prerelease/content/documentation/Swift/Conceptual/Swift_Programming_Language/Expressions.html#//apple_ref/doc/uid/TP40014097-CH32-ID402">動態類型表達式</a>一節使用新的 type(of:) 表達式的信息。
        </li>
         <li>
            更新<a href="https://developer.apple.com/library/prerelease/content/documentation/Swift/Conceptual/Swift_Programming_Language/Statements.html#//apple_ref/doc/uid/TP40014097-CH33-ID540">行控制表達式</a>一節使用 #sourceLocation(file:line:) 表達式的信息。
        </li>
         <li>
            更新<a href="https://developer.apple.com/library/prerelease/content/documentation/Swift/Conceptual/Swift_Programming_Language/Declarations.html#//apple_ref/doc/uid/TP40014097-CH34-ID551">永不返回函數</a>一節使用 新的 Never 類型的信息。
        </li>
        <li>
            增加<a href="https://developer.apple.com/library/prerelease/content/documentation/Swift/Conceptual/Swift_Programming_Language/Expressions.html#//apple_ref/doc/uid/TP40014097-CH32-ID390">字面量表達式</a>一節關於 playground 字面量的信息。
        </li>
         <li>
            更新<a href="https://developer.apple.com/library/prerelease/content/documentation/Swift/Conceptual/Swift_Programming_Language/Declarations.html#//apple_ref/doc/uid/TP40014097-CH34-ID545">In-Out 參數</a>一節，標明只有非逃逸閉包能捕獲 in-out 參數。
        </li>
         <li>
            更新<a href="https://developer.apple.com/library/prerelease/content/documentation/Swift/Conceptual/Swift_Programming_Language/Functions.html#//apple_ref/doc/uid/TP40014097-CH10-ID169">默認參數值</a>一節，現在默認參數不能在調用時候重新排序。
        </li>
         <li>
            更新<a href="https://developer.apple.com/library/prerelease/content/documentation/Swift/Conceptual/Swift_Programming_Language/Attributes.html#//apple_ref/doc/uid/TP40014097-CH35-ID347">屬性</a>章節中關於屬性參數使用分號的說明。
        </li>
         <li>
            增加<a href="https://developer.apple.com/library/prerelease/content/documentation/Swift/Conceptual/Swift_Programming_Language/Declarations.html#//apple_ref/doc/uid/TP40014097-CH34-ID531">重新拋出函數和方法</a>一節中關於在 catch 代碼塊中拋出錯誤的重新拋出函數的信息。
        </li>
 			<li>
            增加<a href="https://developer.apple.com/library/prerelease/content/documentation/Swift/Conceptual/Swift_Programming_Language/Expressions.html#//apple_ref/doc/uid/TP40014097-CH32-ID547">Selector 表達式</a>一節中關於訪問 Objective-C 中 Selector 的 getter 和 setter 的信息。
        </li>
        <li>
            增加<a href="https://developer.apple.com/library/prerelease/content/documentation/Swift/Conceptual/Swift_Programming_Language/Declarations.html#//apple_ref/doc/uid/TP40014097-CH34-ID361">類型別名聲明</a>一節，標明函數類型作為參數類型必須使用括號包裹。
        </li>
         <li>
            增加<a href="https://developer.apple.com/library/prerelease/content/documentation/Swift/Conceptual/Swift_Programming_Language/Types.html#//apple_ref/doc/uid/TP40014097-CH31-ID449">函數類型</a>一節中關於泛型類型別名和在協議內使用類型別名的信息。
        </li>
         <li>
            更新<a href="https://developer.apple.com/library/prerelease/content/documentation/Swift/Conceptual/Swift_Programming_Language/Attributes.html#//apple_ref/doc/uid/TP40014097-CH35-ID347">屬性</a>章節，標明 @IBAction，@IBOutlet 和 @NSManaged 隱式含有 @objc 屬性。
        </li>
         <li>
            增加<a href="https://developer.apple.com/library/prerelease/content/documentation/Swift/Conceptual/Swift_Programming_Language/Attributes.html#//apple_ref/doc/uid/TP40014097-CH35-ID348">聲明屬性</a>一節中關於 @GKInspectable 的信息。
        </li>
         <li>
            更新<a href="https://developer.apple.com/library/prerelease/content/documentation/Swift/Conceptual/Swift_Programming_Language/Protocols.html#//apple_ref/doc/uid/TP40014097-CH25-ID284">可選協議要求</a>一節中關於只能在與 Objective-C 交互的代碼中才能使用可選協議要求的信息。
        </li>
<li>
            刪除<a href="https://developer.apple.com/library/prerelease/content/documentation/Swift/Conceptual/Swift_Programming_Language/Declarations.html#//apple_ref/doc/uid/TP40014097-CH34-ID362">函數聲明</a>一節中關於顯式使用 let 關鍵字作為函數參數的信息。
        </li>
        <li>
            刪除<a href="https://developer.apple.com/library/prerelease/content/documentation/Swift/Conceptual/Swift_Programming_Language/Statements.html#//apple_ref/doc/uid/TP40014097-CH33-ID428">語句</a>一節中關於 Boolean 協議的信息， 現在這個協議已經被 Swift 標准庫刪除。
        </li>
        <li>
            更正<a href="https://developer.apple.com/library/prerelease/content/documentation/Swift/Conceptual/Swift_Programming_Language/Attributes.html#//apple_ref/doc/uid/TP40014097-CH35-ID348">聲明</a>一節中關於 @NSApplicationMain 協議的信息。
        </li>
		</ul>
	</td>
	</tr>
</tbody>
</table>

<a name="swift_2_2"></a>
### Swift 2.2 更新

<table class="graybox" border="0" cellspacing="0" cellpadding="5">
<thead>
    <tr>
        <th scope="col" width="100">發布日期</th>
        <th scope="col">語法變更記錄</th>
    </tr>
</thead>
<tbody>
	 <tr>
    	</tr>
    <tr>
    <td scope="row">2016-03-21</td>
    <td><ul class="list-bullet">
        <li>
            更新至 Swift 2.2。
        </li>
        <li>
            增加了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Statements.html#//apple_ref/doc/uid/TP40014097-CH33-ID539">編譯配置語句</a>一節中關於如何根據 Swift 版本進行條件編譯。
        </li>
        <li>
            增加了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Expressions.html#//apple_ref/doc/uid/TP40014097-CH32-ID400">顯示成員表達式</a>一節中關於如何區分只有參數名不同的方法和構造器的信息。
        </li>
        <li>
            增加了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Expressions.html#//apple_ref/doc/uid/TP40014097-CH32-ID547">選擇器表達式</a>一節中針對 Objective-C 選擇器的 <code>#selector</code> 語法。
        </li>
        <li>
            更新了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Generics.html#//apple_ref/doc/uid/TP40014097-CH26-ID189">關聯類型</a>和<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Declarations.html#//apple_ref/doc/uid/TP40014097-CH34-ID374">協議關聯類型</a>聲明，使用 <code>associatedtype</code> 關鍵詞修改了對於關聯類型的討論。
        </li>
        <li>
            更新了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Initialization.html#//apple_ref/doc/uid/TP40014097-CH18-ID224">可失敗構造器</a>一節中關於當構造器在實例完全初始化之前返回 <code>nil</code>的相關信息。
        </li>
        <li>
            增加了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/BasicOperators.html#//apple_ref/doc/uid/TP40014097-CH6-ID70">比較運算符</a>一節中關於比較元組的信息。
        </li>
        <li>
            增加了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/LexicalStructure.html#//apple_ref/doc/uid/TP40014097-CH30-ID413">關鍵字和標點符號</a>一節中關於使用關鍵字作為外部參數名的信息。
        </li>
        <li>
            增加了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/LexicalStructure.html#//apple_ref/doc/uid/TP40014097-CH30-ID413">聲明特性</a>一節中關於<code>@objc</code>特性的討論，並指出枚舉(Enumeration)和枚舉用例(Enumaration Case)。
        </li>
        <li>
            增加了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/LexicalStructure.html#//apple_ref/doc/uid/TP40014097-CH30-ID418">操作符</a>一節中對於自定義運算符的討論包含了<code>.</code>。
        </li>
        <li>
            增加了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Declarations.html#//apple_ref/doc/uid/TP40014097-CH34-ID531">重新拋出錯誤的函數和方法</a>一節中關於重新拋出錯誤函數不能直接拋出錯誤的筆記。
        </li>
        <li>
            增加了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Properties.html#//apple_ref/doc/uid/TP40014097-CH14-ID262">屬性觀察器</a>一節中關於當作為 in-out 參數傳遞屬性時，屬性觀察器的調用行為。
        </li>
        <li>
            增加了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/GuidedTour.html#//apple_ref/doc/uid/TP40014097-CH2-ID1">Swift 初見</a>一節中關於錯誤處理的內容。
        </li>
        <li>
            更新了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/AutomaticReferenceCounting.html#//apple_ref/doc/uid/TP40014097-CH20-ID53">弱引用</a>一節中的圖片用以更清楚的展示重新分配過程。
        </li>
        <li>
            刪除了 C 語言風格的 <code>for</code> 循環，<code>++</code> 前綴和後綴運算符，以及<code>--</code> 前綴和後綴運算符。
        </li>
        <li>
            刪除了對變量函數參數和柯裡化函數的特殊語法的討論。
        </li>
        </ul>
    </td>
  </tr>
</tbody>
</table>

<a name="swift_2_1"></a>
### Swift 2.1 更新

<table class="graybox" border="0" cellspacing="0" cellpadding="5">
<thead>
    <tr>
        <th scope="col" width="100">發布日期</th>
        <th scope="col">語法變更記錄</th>
    </tr>
</thead>
<tbody>
    <tr>
    <td scope="row">2015-10-20</td>
    <td><ul class="list-bullet">
        <li>
            更新至 Swift 2.1。
        </li>
        <li>
            更新了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/StringsAndCharacters.html#//apple_ref/doc/uid/TP40014097-CH7-ID292">字符串插值(String Interprolation)</a>和<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/LexicalStructure.html#//apple_ref/doc/uid/TP40014097-CH30-ID417">字符串字面量(String Literals)</a>小節，現在字符串插值可包含字符串字面量。
        </li>
        <li>
            增加了在<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Closures.html#//apple_ref/doc/uid/TP40014097-CH11-ID546">非逃逸閉包(Nonescaping Closures)</a>一節中關於 <code>@noescape</code> 屬性的相關內容。
        </li>
        <li>
            更新了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Attributes.html#//apple_ref/doc/uid/TP40014097-CH35-ID348">聲明特性(Declaration Attributes)</a>和<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Statements.html#//apple_ref/doc/uid/TP40014097-CH33-ID539">編譯配置語句(Build Configuration Statement)</a>小節中與 tvOS 相關的信息。
        </li>
        <li>
            增加了 <a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Declarations.html#//apple_ref/doc/uid/TP40014097-CH34-ID545">In-Out 參數(In-Out Parameters)</a>小節中與  in-out 參數行為相關的信息。
        </li>
        <li>
            增加了在<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Expressions.html#//apple_ref/doc/uid/TP40014097-CH32-ID544">捕獲列表(Capture Lists)</a>一節中關於指定閉包捕獲列表被捕獲時捕獲值的相關內容。
        </li>
        <li>
            更新了通過<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/OptionalChaining.html#//apple_ref/doc/uid/TP40014097-CH21-ID248">可選鏈式調用訪問屬性(Accessing Properties Through Optional Chaining)</a>一節，闡明了如何通過可選鏈式調用進行賦值。
        </li>
        <li>
            改進了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Closures.html#//apple_ref/doc/uid/TP40014097-CH11-ID543">自閉包(Autoclosure)</a>一節中對自閉包的討論。
        </li>
        <li>
            在<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/GuidedTour.html#//apple_ref/doc/uid/TP40014097-CH2-ID1">Swift 初見(A Swift Tour)</a>一節中更新了一個使用<code>??</code>操作符的例子。
        </li>
        </ul>
    </td>
  </tr>
</tbody>
</table>

<a name="swift_2_0"></a>
### Swift 2.0 更新

<table class="graybox" border="0" cellspacing="0" cellpadding="5">
<thead>
    <tr>
        <th scope="col" width="100">發布日期</th>
        <th scope="col">語法變更記錄</th>
    </tr>
</thead>
<tbody>
    <tr>
    <td scope="row">2015-09-16</td>
    <td><ul class="list-bullet">
        <li>
            更新至 Swift 2.0。
        </li>
        <li>
            增加了對於錯誤處理相關內容，包括 <a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html#//apple_ref/doc/uid/TP40014097-CH42-ID508">錯誤處理</a>一章、<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Statements.html#//apple_ref/doc/uid/TP40014097-CH33-ID533">Do 語句</a>、<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Statements.html#//apple_ref/doc/uid/TP40014097-CH33-ID518">Throw 語句</a>、<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Statements.html#//apple_ref/doc/uid/TP40014097-CH33-ID532">Defer 語句</a>以及<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Expressions.html#//apple_ref/doc/uid/TP40014097-CH32-ID516">try 運算符</a> 的多個小節。
        </li>
        <li>
            更新了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html#//apple_ref/doc/uid/TP40014097-CH42-ID509">表示並拋出錯誤</a>一節，現在所有類型均可遵循 <code>ErrorType</code> 協議。
        </li>
        <li>
            增加了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html#//apple_ref/doc/uid/TP40014097-CH42-ID542">將錯誤轉換成可選值</a>一節 <code>try?</code> 關鍵字的相關信息。
        </li>
        <li>
            增加了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Enumerations.html#//apple_ref/doc/uid/TP40014097-CH12-ID145">枚舉</a>一章的<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Enumerations.html#//apple_ref/doc/uid/TP40014097-CH12-ID536">遞歸枚舉</a>一節和<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Declarations.html#//apple_ref/doc/uid/TP40014097-CH34-ID351">聲明</a>一章的<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Declarations.html#//apple_ref/doc/uid/TP40014097-CH34-ID365">任意類型用例的枚舉</a>一節中關於遞歸枚舉的內容。
        </li>
        <li>
            增加了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/ControlFlow.html#//apple_ref/doc/uid/TP40014097-CH9-ID120">控制流</a>一章中a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/ControlFlow.html#//apple_ref/doc/uid/TP40014097-CH9-ID523">檢查 API 可用性</a>一節和<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Statements.html#//apple_ref/doc/uid/TP40014097-CH33-ID428">語句</a>一章中<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Statements.html#//apple_ref/doc/uid/TP40014097-CH33-ID522">可用性條件</a>一節中關於 API 可用性檢查的內容。
        </li>
        
        <li>
            增加了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/ControlFlow.html#//apple_ref/doc/uid/TP40014097-CH9-ID120">控制流</a>一章的<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/ControlFlow.html#//apple_ref/doc/uid/TP40014097-CH9-ID525">早期退出</a>一節和<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Statements.html#//apple_ref/doc/uid/TP40014097-CH33-ID428">語句</a>一章的<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Statements.html#//apple_ref/doc/uid/TP40014097-CH33-ID524">guard語句</a>中關於新 <code>guard</code> 語句的內容。
        </li>
        <li>
            增加了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Protocols.html#//apple_ref/doc/uid/TP40014097-CH25-ID267">協議</a>一章中<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Protocols.html#//apple_ref/doc/uid/TP40014097-CH25-ID521">協議擴展</a>一節中關於協議擴展的內容。
        </li>
        <li>
            增加了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/AccessControl.html#//apple_ref/doc/uid/TP40014097-CH41-ID3">訪問控制</a>一章中<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/AccessControl.html#//apple_ref/doc/uid/TP40014097-CH41-ID519">單元測試 target 的訪問級別</a>一節中關於單元測試的訪問控制相關的內容。
        </li>
        <li>
            增加了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Patterns.html#//apple_ref/doc/uid/TP40014097-CH36-ID419">模式</a>一章中<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Patterns.html#//apple_ref/doc/uid/TP40014097-CH36-ID520">可選模式</a>一節中的新可選模式。
        </li>
        <li>
            更新了 <a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/ControlFlow.html#//apple_ref/doc/uid/TP40014097-CH9-ID126">Repeat-While</a> 一節中關於<code>repeat-while</code>循環的信息。
        </li>
        <li>
            更新了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/StringsAndCharacters.html#//apple_ref/doc/uid/TP40014097-CH7-ID285">字符串和字符</a>一章，現在<code>String</code>在 Swift 標准庫中不再遵循<code>CollectionType</code>協議。
        </li>
        <li>
            增加了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/TheBasics.html#//apple_ref/doc/uid/TP40014097-CH5-ID314">打印常量和變量</a>一節中關於新 Swift 標准庫中關於 <code>print(_:separator:terminator)</code> 的信息。
        </li>
        <li>
            增加了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Enumerations.html#//apple_ref/doc/uid/TP40014097-CH12-ID145">枚舉</a>一章中<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Enumerations.html#//apple_ref/doc/uid/TP40014097-CH12-ID535">原始值的隱式賦值</a>一節和<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Declarations.html#//apple_ref/doc/uid/TP40014097-CH34-ID351">聲明</a>一章的<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Declarations.html#//apple_ref/doc/uid/TP40014097-CH34-ID366">包含原始值類型的枚舉</a>一節中關於包含<code>String</code>原始值的枚舉用例的行為。
        </li>
        <li>
            增加了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Closures.html#//apple_ref/doc/uid/TP40014097-CH11-ID543">自閉包</a>一節中關於<code>@autoclosure</code>特性的相關信息，包括它的<code>@autoclosure(escaping)</code>形式。
        </li>
        <li>
            更新了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Attributes.html#//apple_ref/doc/uid/TP40014097-CH35-ID348">聲明特性</a>一節中關於<code>@avaliable</code>和<code>warn_unused_result</code>特性的相關內容。
        </li>
        <li>
            更新了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Attributes.html#//apple_ref/doc/uid/TP40014097-CH35-ID350">類型特性</a>一節中關於<code>@convention</code>特性的相關信息。
        </li>
        <li>
            增加了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/TheBasics.html#//apple_ref/doc/uid/TP40014097-CH5-ID333">可選綁定</a>一節中關於使用<code>where</code>子句進行多可選綁定的內容。
        </li>
        <li>
            增加了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/StringsAndCharacters.html#//apple_ref/doc/uid/TP40014097-CH7-ID292">字符串字面量</a>一節中關於在編譯時使用 <code>+</code> 運算符憑借字符串字面量的相關信息。
        </li>
        <li>
            增加了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Types.html#//apple_ref/doc/uid/TP40014097-CH31-ID455">元類型</a>一節中關於元類型值的比較和使用它們通過構造器表達式構造實例。
        </li>
        <li>
            增加了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/TheBasics.html#//apple_ref/doc/uid/TP40014097-CH5-ID336">斷言調試</a>一節中關於用戶定義斷言是被警用的相關內容。
        </li>
        <li>
            更新了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Attributes.html#//apple_ref/doc/uid/TP40014097-CH35-ID348">聲明特性</a>一節中，對<code>@NSManaged</code>特性的討論，現在這個特性可以被應用到一個確定實例方法。
        </li>
        <li>
            更新了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Functions.html#//apple_ref/doc/uid/TP40014097-CH10-ID171">可變參數</a>一節，現在可變參數可以聲明在函數參數列表的任意位置中。
        </li>
        <li>
            增加了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Initialization.html#//apple_ref/doc/uid/TP40014097-CH18-ID229">重寫可失敗構造器</a>一節中，關於非可失敗構造器相當於一個可失敗構造器通過父類構造器的結果進行強制拆包的相關內容。
        </li>
        <li>
            增加了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Declarations.html#//apple_ref/doc/uid/TP40014097-CH34-ID365">任意類型用例的枚舉</a>一節中關於枚舉用例作為函數的內容。
        </li>
        <li>
            增加了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Expressions.html#//apple_ref/doc/uid/TP40014097-CH32-ID399">構造器表達式</a>一節中關於顯式引用一個構造器的內容。
        </li>
        <li>
            更新了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Statements.html#//apple_ref/doc/uid/TP40014097-CH33-ID538">編譯控制語句</a>一節中關於編譯信息以及行控制語句的相關信息。
        </li>
        <li>
            更新了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Types.html#//apple_ref/doc/uid/TP40014097-CH31-ID455">元類型</a>一節中關於如何從元類型值中構造類實例。
        </li>
        <li>
            更新了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/AutomaticReferenceCounting.html#//apple_ref/doc/uid/TP40014097-CH20-ID53">弱引用</a>一節中關於弱引用作為緩存的顯存的不足。
        </li>
        <li>
            更新了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/StringsAndCharacters.html#//apple_ref/doc/uid/TP40014097-CH7-ID292">類型特性</a>一節，提到了存儲型特性其實是懶加載。
        </li>
        <li>
            更新了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Properties.html#//apple_ref/doc/uid/TP40014097-CH14-ID264">捕獲類型</a>一節，闡明了變量和常量在閉包中如何被捕獲。
        </li>
        <li>
            更新了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Attributes.html#//apple_ref/doc/uid/TP40014097-CH35-ID348">聲明特性</a>一節用以描述如何在類中使用<code>@objc</code>關鍵字。
        </li>
        <li>
            增加了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html#//apple_ref/doc/uid/TP40014097-CH42-ID512">錯誤處理</a>一節中關於執行<code>throw</code>語句的性能的討論。增加了 Do 語句一節中相似的信息。
        </li>
        <li>
            更新了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Statements.html#//apple_ref/doc/uid/TP40014097-CH33-ID533">類型特性</a>一節中關於類、結構體和枚舉的存儲型和計算型特性的信息。
        </li>
        <li>
            更新了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Statements.html#//apple_ref/doc/uid/TP40014097-CH33-ID441">Break 語句</a>一節中關於帶標簽的 break 語句。
        </li>
        <li>
            更新了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Properties.html#//apple_ref/doc/uid/TP40014097-CH14-ID262">屬性觀察器</a>一節，闡明了<code>willSet</code>和<code>didSet</code>觀察器的行為。
        </li>
        <li>
            增加了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/AccessControl.html#//apple_ref/doc/uid/TP40014097-CH41-ID5">訪問級</a>一節中關於<code>private</code>作用域訪問的相關信息。
        </li>
        <li>
            增加了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/AutomaticReferenceCounting.html#//apple_ref/doc/uid/TP40014097-CH20-ID53">弱引用</a>一節中關於若應用在垃圾回收系統和 ARC 之間的區別。
        </li>
        <li>
            更新了<a href="https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/StringsAndCharacters.html#//apple_ref/doc/uid/TP40014097-CH7-ID295">字符串字面量中特殊字符</a>一節中對 Unicode 標量更精確的定義。
        </li>
        </ul>
    </td>
  </tr>
</tbody>
</table>


<a name="swift_1_2"></a>
### Swift 1.2 更新
<a name="xcode6_3"></a>

<table class="graybox" border="0" cellspacing="0" cellpadding="5">
<thead>
    <tr>
        <th scope="col" width="100">發布日期</th>
        <th scope="col">語法變更記錄</th>
    </tr>
</thead>
<tbody>
    <tr>
    <td scope="row">2015-4-8</td>
    <td><ul class="list-bullet">
            <li>
                更新至 Swift 1.2。
            </li>
            <li>
                Swift現在自身提供了一個<code>Set</code>集合類型，更多信息請看<a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/CollectionTypes.html#//apple_ref/doc/uid/TP40014097-CH8-ID484">集合</a>

            </li>
            <li>
                <code>@autoclosure</code>現在是一個參數聲明的屬性，而不是參數類型的屬性。這裡還有一個新的參數聲明屬性<code>@noescape</code>。更多信息，請看<a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Attributes.html#//apple_ref/doc/uid/TP40014097-CH35-ID348">屬性聲明</a>
            </li>
            <li>
                對於類型屬性和方法現在可以使用<code>static</code>關鍵字作為聲明描述符，更多信息，請看<a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Declarations.html#//apple_ref/doc/uid/TP40014097-CH34-ID483">類型變量屬性</a>
            </li>
            <li>
                Swift現在包含一個<code>as?</code>和<code>as!</code>的向下可失敗類型轉換運算符。更多信息，請看<a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Protocols.html#//apple_ref/doc/uid/TP40014097-CH25-ID283">協議遵循性檢查</a>
            </li>
            <li>
                增加了一個新的指導章節，它是關於<a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/StringsAndCharacters.html#//apple_ref/doc/uid/TP40014097-CH7-ID495">字符串索引</a>的
            </li>
            <li>
                從<a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/AdvancedOperators.html#//apple_ref/doc/uid/TP40014097-CH27-ID37">溢出運算符</a>中移除了溢出除運算符(&/)和求余溢出運算符(&%)。
            </li>
            <li>
                更新了常量和常量屬性在聲明和構造時的規則，更多信息，請看<a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Declarations.html#//apple_ref/doc/uid/TP40014097-CH34-ID355">常量聲明</a>
            </li>
            <li>
                更新了字符串字面量中Unicode標量集的定義，請看<a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/StringsAndCharacters.html#//apple_ref/doc/uid/TP40014097-CH7-ID295">字符串字面量中的特殊字符</a>
            </li>
            <li>
                更新了<a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/BasicOperators.html#//apple_ref/doc/uid/TP40014097-CH6-ID73">區間運算符</a>章節來提示當半開區間運算符含有相同的起止索引時，其區間為空。
            </li>
            <li>
                更新了<a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Closures.html#//apple_ref/doc/uid/TP40014097-CH11-ID104">閉包引用類型</a>章節來澄清對於變量的捕獲規則
            </li>
            <li>
                更新了<a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/AdvancedOperators.html#//apple_ref/doc/uid/TP40014097-CH27-ID38">值溢出</a>章節來澄清有符號整數和無符號整數的溢出行為
            </li>
            <li>
                更新了<a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Declarations.html#//apple_ref/doc/uid/TP40014097-CH34-ID369">協議聲明</a>章節來澄清協議聲明時的作用域和成員
            </li>
            <li>
                更新了<a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/AutomaticReferenceCounting.html#//apple_ref/doc/uid/TP40014097-CH20-ID58">捕獲列表</a>章節來澄清對於閉包捕獲列表中的弱引用和無主引用的使用語法。
            </li>
            <li>
                更新了<a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/LexicalStructure.html#//apple_ref/doc/uid/TP40014097-CH30-ID418">運算符</a>章節來明確指明一些例子來說明自定義運算符所支持的特性，如數學運算符，各種符號，Unicode符號塊等
            </li>
            <li>
            在函數作用域中的常量聲明時可以不被初始化，它必須在第一次使用前被賦值。更多的信息，請看<a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Declarations.html#//apple_ref/doc/uid/TP40014097-CH34-ID355">常量聲明</a>
        </li>
        <li>
            在構造器中，常量屬性有且僅能被賦值一次。更多信息，請看<a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Initialization.html#//apple_ref/doc/uid/TP40014097-CH18-ID212">在構造過程中給常量屬性賦值</a>
        </li>
        <li>
            多個可選綁定現在可以在<code>if</code>語句後面以逗號分隔的賦值列表的方式出現，更多信息，請看<a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/TheBasics.html#//apple_ref/doc/uid/TP40014097-CH5-ID333">可選綁定</a>
        </li>
        <li>
            一個<a link="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Expressions.html#//apple_ref/doc/uid/TP40014097-CH32-ID405">可選鏈表達式</a>必須出現在後綴表達式中
        </li>
        <li>
            協議類型轉換不再局限於<code>@obj</code>修飾的協議了
        </li>
        <li>
            在運行時可能會失敗的類型轉換可以使用<code>as?</code>和<code>as!</code>運算符，而確保不會失敗的類型轉換現在使用<code>as</code>運算符。更多信息，請看<a link="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Expressions.html#//apple_ref/doc/uid/TP40014097-CH32-ID388">類型轉換運算符</a>
        </li>
        </ul>
    </td>
  </tr>
</tbody>
</table>


<a name="swift_1_1"></a>
### Swift 1.1 更新

<table class="graybox" border="0" cellspacing="0" cellpadding="5">
<thead>
    <tr>
        <th scope="col" width="100">發布日期</th>
        <th scope="col">語法變更記錄</th>
    </tr>
</thead>
<tbody>
    <tr>
    <td scope="row">2014-10-16</td>
    <td><ul class="list-bullet">
        <li>
            更新至 Swift 1.1。
        </li>
        <li>
            增加了關於<a href="http://developer.apple.com/library/etc/redirect/xcode/devtools/419f35/documentation/Swift/Conceptual/Swift_Programming_Language/Declarations.html">失敗構造器(Failable Initializers)</a>的完整章節。
        </li>
        <li>
            增加了協議中關於失敗構造器要求的描述。
        </li>
        <li>
            常量和變量的 <code>Any</code> 類型現可以包含函數實例。更新了關於 <code>Any</code> 相關的示例來展示如何在 <code>switch</code> 語句中如何檢查並轉換到一個函數類型。
        </li>
        <li>
            帶有原始值的枚舉類型增加了一個<code>rawValue</code>屬性替代<code>toRaw()</code>方法，同時使用了一個以<code>rawValue</code>為參數的失敗構造器來替代<code>fromRaw()</code>方法。更多的信息，請看<a href="http://developer.apple.com/library/etc/redirect/xcode/devtools/419f35/documentation/Swift/Conceptual/Swift_Programming_Language/Enumerations.html">原始值(Raw Values)</a>和<a href="http://developer.apple.com/library/etc/redirect/xcode/devtools/419f35/documentation/Swift/Conceptual/Swift_Programming_Language/Declarations.html">帶原始值的枚舉類型(Enumerations with Cases of a Raw-Value Type)</a>部分。
        </li>
        <li>
            自定義運算符現在可以包含`?`字符，更新的<a href="http://developer.apple.com/library/etc/redirect/xcode/devtools/419f35/documentation/Swift/Conceptual/Swift_Programming_Language/LexicalStructure.html">運算符(Operators)</a>章節描述了改進後的規則，並且從<a href="http://developer.apple.com/library/etc/redirect/xcode/devtools/419f35/documentation/Swift/Conceptual/Swift_Programming_Language/AdvancedOperators.html">自定義運算符(Custom Operators)</a>章節刪除了重復的運算符有效字符集合
        </li>
        
        </ul>
    </td>
  </tr>
</tbody>
</table>

<a name="swift_1_0"></a>
### Swift 1.0 更新

<table class="graybox" border="0" cellspacing="0" cellpadding="5">
<thead>
    <tr>
        <th scope="col" width="100">發布日期</th>
        <th scope="col">語法變更記錄</th>
    </tr>
</thead>
<tbody>
    <tr>
    <td scope="row">2014-08-18</td>
    <td><ul class="list-bullet">
        <li>
            發布新的文檔用以詳述 Swift 1.0，蘋果公司針對iOS和OS X應用的全新開發語言。
        </li>
        <li>
            在章節協議中，增加新的小節：<a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Protocols.html#//apple_ref/doc/uid/TP40014097-CH25-XID_397">對構造器的規定（Initializer Requirements）</a>
        </li>
        <li>
            在章節協議中，增加新的小節：<a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Protocols.html#//apple_ref/doc/uid/TP40014097-CH25-XID_409">類專屬協議（class-only protocols）</a>
        </li>
        <li>
            <a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/TheBasics.html#//apple_ref/doc/uid/TP40014097-CH5-XID_494">斷言(assertions)</a>現在可以使用字符串內插語法，並刪除了文檔中有沖突的注釋
        </li>
        <li>
            更新了<a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/StringsAndCharacters.html#//apple_ref/doc/uid/TP40014097-CH7-XID_428">連接字符串和字符（Concatenating Strings and Characters）</a>小節來說明一個事實，那就是字符串和字符不能再用<code>+</code>號運算符或者復合加法運算符<code>+=</code>相互連接，這兩種運算符現在只能用於字符串之間相連。請使用<code>String</code>類型的<code>append</code>方法在一個字符串的尾部增加單個字符
        </li>
        <li>
            在<a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Attributes.html#//apple_ref/doc/uid/TP40014097-CH35-XID_516">聲明特性（Declaration Attributes）</a>章節增加了關於<code>availability</code>特性的一些信息
        </li>
        <li>
            <a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/TheBasics.html#//apple_ref/doc/uid/TP40014097-CH5-XID_478">可選類型（Optionals）</a> 若有值時，不再隱式的轉換為 <code>true</code>，同樣，若無值時，也不再隱式的轉換為 <code>false</code>, 這是為了避免在判別 optional <code>Bool</code> 的值時產生困惑。 替代的方案是，用<code>==</code> 或 <code>!=</code> 運算符顯式地去判斷Optinal是否是 <code>nil</code>，以確認其是否包含值。
        </li>
        <li>
            Swift新增了一個 <a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/BasicOperators.html#//apple_ref/doc/uid/TP40014097-CH6-XID_124" data-id="//apple_ref/doc/uid/TP40014097-CH6-XID_124">Nil合並運算符（Nil Coalescing Operator）</a> (<code>a ?? b</code>), 該表達式中，如果Optional <code>a</code>的值存在，則取得它並返回，若Optional <code>a</code>為<code>nil</code>，則返回默認值 <code>b</code>
        </li>
        <li>
            更新和擴展 <a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/StringsAndCharacters.html#//apple_ref/doc/uid/TP40014097-CH7-XID_434">字符串的比較（Comparing Strings）</a> 章節，用以反映和展示'字符串和字符的比較'，以及'前綴（prefix）/後綴(postfix)比較'都開始基於擴展字符集(extended grapheme clusters)規范的等價比較.
        </li>
        <li>
            現在，你可以通過 <a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/OptionalChaining.html#//apple_ref/doc/uid/TP40014097-CH21-XID_356">可選鏈（Optional Chaining）</a>來：給屬性設值，將其賦給一個下標腳注（subscript）; 或調用一個變異（mutating）方法或運算符。對此，章節——<a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/OptionalChaining.html#//apple_ref/doc/uid/TP40014097-CH21-XID_364">通過可選鏈訪問屬性（Accessing Properties Through Optional Chaining）</a>的內容已經被相應的更新。而章節——<a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/OptionalChaining.html#//apple_ref/doc/uid/TP40014097-CH21-XID_361">通過可選鏈調用方法（Calling Methods Through Optional Chaining</a>中，關於檢查方法調用是否成功的例子，已被擴展為展示如何檢查一個屬性是否被設值成功。

        </li>
        <li>
            在章節可選鏈中，增加一個新的小節 <a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/OptionalChaining.html#//apple_ref/doc/uid/TP40014097-CH21-XID_364">訪問可選類型的下標腳注（Accessing Subscripts of Optional Type）</a>
        </li>
        <li>
            更新章節 <a href="../chapter2/04_Collection_Types.md#訪問和修改數組" data-id="訪問和修改數組">訪問和修改數組(Accessing and Modifying an Array)</a> 以標示：從該版本起，不能再通過<code>+=</code> 運算符給一個數組添加一個新的項。. 對應的替代方案是, 使<code>append</code> 方法, 或者通過<code>+=</code>運算符來添加一個<b>只有一個項的數組</b>（single-item Array）.</li>
        <li>
            添加了一個提示：在 <a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/BasicOperators.html#//apple_ref/doc/uid/TP40014097-CH6-XID_126">范圍運算符（Range Operators）</a>中，比如， <code>a...b</code> 和 <code>a..&lt;b</code> ，起始值<code>a</code>不能大於結束值<code>b</code>.
        </li>
        <li>
            重寫了<a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Inheritance.html#//apple_ref/doc/uid/TP40014097-CH17-XID_293">繼承（Inheritance）</a> 這一章：刪除了本章中關於構造器重寫的介紹性報道；轉而將更多的注意力放到新增的部分——子類的新功能，以及如何通過重寫（overrides）修改已有的功能。另外，小節 <a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Inheritance.html#//apple_ref/doc/uid/TP40014097-CH17-XID_301">重寫屬性的Getters和Setters（Overriding Property Getters and Setters）</a> 中的例子已經被替換為展示如何重寫一個 <code>description</code> 屬性. (而關於如何在子類的構造器中修改繼承屬性的默認值的例子，已經被移到 <a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Inheritance.html#//apple_ref/doc/uid/TP40014097-CH17-XID_293">構造過程（Initialization）</a> 這一章.)
        </li>
        <li>
            更新了 <a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Initialization.html#//apple_ref/doc/uid/TP40014097-CH18-XID_331">構造器的繼承與重寫（Initializer Inheritance and Overriding）</a> 小節以標示： 重寫一個特定的構造器必須使用 <code>override</code> 修飾符.
        </li>
        <li>
            更新 <a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Initialization.html#//apple_ref/doc/uid/TP40014097-CH18-XID_339"> Required構造器（Required Initializers）</a> 小節以標示：<code>required</code> 修飾符現在需要出現在所有子類的required構造器的聲明中, 而required構造器的實現，現在可以僅從父類自動繼承。
        </li>
        <li>
            中置（Infix）的 <a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/AdvancedOperators.html#//apple_ref/doc/uid/TP40014097-CH27-XID_80">運算符函數（Operator Functions）</a> 不再需要<code>@infix</code> 屬性.
        </li>
        <li>
            <a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/RevisionHistory.html#//apple_ref/doc/uid/TP40014097-CH40-XID_1631">前置和後置運算符(Prefix and Postfix Operators)</a>的<code>@prefix</code> 和 <code>@postfix</code> 屬性，已變更為 <code>prefix</code> 和 <code>postfix</code> 聲明修飾符（declaration modifiers）.
        </li>
            <li>
            增加一條注解：當Prefix和postfix運算符被作用於同一個操作數時，關於<a href="AdvancedOperators.html#//apple_ref/doc/uid/TP40014097-CH27-XID_81" data-id="//apple_ref/doc/uid/TP40014097-CH27-XID_81">前置和後置運算符(Prefix and Postfix Operators)</a>的順序(postfix運算符會先被執行)
        </li>
        <li>
            在運算符函數（Operator functions）中， <a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/AdvancedOperators.html#//apple_ref/doc/uid/TP40014097-CH27-XID_82" data-id="//apple_ref/doc/uid/TP40014097-CH27-XID_82">組合賦值運算符（Compound Assignment Operators）</a> 不再使用 <code>@assignment</code> 屬性來定義函數.
        </li>
        <li>
            在這個版本中，在定義<a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/AdvancedOperators.html#//apple_ref/doc/uid/TP40014097-CH27-XID_85">自定義操作符（Custom Operators）</a> 時，<b>修飾符（Modifiers）的出現順序發生變化</b>。比如， 現在，你該編寫 <code>prefix operator</code>， 而不是 <code>operator prefix</code>.
        </li>
        <li>
            增加信息：關於<code>dynamic</code> 聲明修飾符（declaration modifier），於章節 <a href="Declarations.html#//apple_ref/doc/uid/TP40014097-CH34-XID_705" data-id="//apple_ref/doc/uid/TP40014097-CH34-XID_705">聲明修飾符（Declaration Modifiers）</a>.
        </li>
        <li>
            增加信息：<a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/LexicalStructure.html#//apple_ref/doc/uid/TP40014097-CH30-XID_886">字面量Literals</a> 的類型推導（type inference）
        </li>
        <li>
            為章節<a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Declarations.html#//apple_ref/doc/uid/TP40014097-CH34-XID_597">Curried Functions</a>添加了更多的信息.
        </li>
        <li>
            加入新的章節  <a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/AccessControl.html#//apple_ref/doc/uid/TP40014097-CH41-XID_29">權限控制（Access Control）</a>.
        </li>
        <li>
            更新了章節 <a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/StringsAndCharacters.html#//apple_ref/doc/uid/TP40014097-CH7-XID_413">字符串和字符（Strings and Characters）</a> 用以表明，在Swift中，<code>Character</code> 類型現在代表的是擴展字符集(extended grapheme cluster)中的一個Unicode，為此，新增了小節 <a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/StringsAndCharacters.html#//apple_ref/doc/uid/TP40014097-CH7-XID_431">Extended Grapheme Clusters</a> 。同時，為小節 <a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/StringsAndCharacters.html#//apple_ref/doc/uid/TP40014097-CH7-XID_428">Unicode標量（Unicode Scalars）</a> 和 <a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/StringsAndCharacters.html#//apple_ref/doc/uid/TP40014097-CH7-XID_434">字符串比較（Comparing Strings）</a>增加了更多內容.
        </li>
        <li>
            更新章節<a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/LexicalStructure.html#//apple_ref/doc/uid/TP40014097-CH30-XID_856">字符串字面量（String Literals）</a>：在一個字符串中，Unicode標量（Unicode scalars） 以 <code>\u{n}</code>的形式來表示, <code>n</code> 是一個最大可以有8位的16進制數（hexadecimal digits）
        </li>
        <li>
            <code>NSString</code> <code>length</code> 屬性已被映射到Swift的內建 <code>String</code>類型。（注意，這兩屬性的類型是<code>utf16Count</code>,而非 <code>utf16count</code>）.
        </li>
        <li>
            Swift的內建 <code>String</code> 類型不再擁有 <code>uppercaseString</code> 和 <code>lowercaseString</code> 屬性.其對應部分在章節 <a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/StringsAndCharacters.html#//apple_ref/doc/uid/TP40014097-CH7-XID_413">字符串和字符（Strings and Characters）</a>已經被刪除, 並且各種對應的代碼用例也已被更新.
        </li>
        <li>
            加入新的章節  <a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Initialization.html#//apple_ref/doc/uid/TP40014097-CH18-XID_315">沒有外部名的構造器參數（Initializer Parameters Without External Names）</a>.
        </li>
        <li>
            加入新的章節  <a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Initialization.html#//apple_ref/doc/uid/TP40014097-CH18-XID_339"> Required構造器（Required Initializers）</a>.
        </li>
        <li>
            加入新的章節 <a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Functions.html#//apple_ref/doc/uid/TP40014097-CH10-XID_252">可選元祖（函數）返回類型 （Optional Tuple Return Types）</a>.
        </li>
        <li>
            更新章節 <a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/TheBasics.html#//apple_ref/doc/uid/TP40014097-CH5-XID_453">類型標注（Type Annotations）</a> ：多個相關變量可以用「類型標注」（type annotaion）在同一行中聲明為同一類型。
        </li>
        <li>
             <code>@optional</code>, <code>@lazy</code>, <code>@final</code>,  <code>@required</code> 等關鍵字被更新為 <code>optional</code>, <code>lazy</code>, <code>final</code>, <code>required</code> <a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Declarations.html#//apple_ref/doc/uid/TP40014097-CH34-XID_705">參見聲明修飾符（Declaration Modifiers）</a>.
        </li>
        <li>
            更新整本書 —— 引用 <code>..&lt;</code> 作為<a href="BasicOperators.html#//apple_ref/doc/uid/TP40014097-CH6-XID_128" data-id="//apple_ref/doc/uid/TP40014097-CH6-XID_128">區間運算符（Half-Open Range Operator）</a> (取代原先的<code>..</code> ).
        </li>
        <li>
            更新了小節 <a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/CollectionTypes.html#//apple_ref/doc/uid/TP40014097-CH8-XID_185">讀取和修改字典（Accessing and Modifying a Dictionary）</a>：  <code>Dictionary</code> 現在早呢更加了一個 Boolean型的屬性： <code>isEmpty</code>
        </li>
        <li>
            解釋了哪些字符（集）可被用來定義<a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/AdvancedOperators.html#//apple_ref/doc/uid/TP40014097-CH27-XID_85">自定義操作符 （Custom Operators）</a>
        </li>
        <li>
            <code>nil</code> 和布爾運算中的 <code>true</code> 和 <code>false</code> 現在被定義為字面量<a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/LexicalStructure.html#//apple_ref/doc/uid/TP40014097-CH30-XID_886">Literals</a>.
        </li>
        <li>
            Swift 中的數組 （<code>Array</code>） 類型從現在起具備了完整的值語義。具體信息被更新到 <a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/CollectionTypes.html#//apple_ref/doc/uid/TP40014097-CH8-XID_170">集合的可變性（Mutability of Collections）</a> 和 <a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/CollectionTypes.html#//apple_ref/doc/uid/TP40014097-CH8-XID_172">數組（Arrays）</a> 兩小節，以反映這個新的變化. 此外，還解釋了如何 <a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/ClassesAndStructures.html#//apple_ref/doc/uid/TP40014097-CH13-XID_150">給Strings, Arrays和Dictionaries進行賦值和拷貝 （Assignment and Copy Behavior for Strings, Arrays, and Dictionaries）</a>.
        </li>
        <li>
            <a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/CollectionTypes.html#//apple_ref/doc/uid/TP40014097-CH8-XID_173">數組類型速記語法（Array Type Shorthand Syntax）</a> 從 <code>SomeType[]</code>.更新為<code>[SomeType]</code>
        </li>
        <li>
            加入新的小節：<a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/CollectionTypes.html#//apple_ref/doc/uid/TP40014097-CH8-XID_182">字典類型的速記語法（Dictionary Type Shorthand Syntax)</a>.： <code>[KeyType: ValueType]</code>.
        </li>
        <li>
            加入新的小節：<a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/CollectionTypes.html#//apple_ref/doc/uid/TP40014097-CH8-XID_189">字典鍵類型的哈希值（Hash Values for Dictionary Key Types)</a>.
        </li>
        <li>
            例子 <a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Closures.html#//apple_ref/doc/uid/TP40014097-CH11-XID_154">閉包表達式 (Closure Expressions)</a> 中使用新的全局函數 <code>sorted</code> 取代原先的全局函數 <code>sort</code> 去展示如何返回一個全新的數組.
        </li>
        <li>
            更新關於 <a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Initialization.html#//apple_ref/doc/uid/TP40014097-CH18-XID_320">結構體逐一成員構造器 （Memberwise Initializers for Structure Types）</a> 的描述：即使結構體的成員<b>沒有默認值</b>，逐一成員構造器也可以自動獲得。
        </li>
        <li>
            <a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/BasicOperators.html#//apple_ref/doc/uid/TP40014097-CH6-XID_128">區間運算符（Half-Open Range Operator）</a>由<code>..</code>更新到<code>..&lt;</code>
        </li>
        <li>
            添加一個例子 <a href="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Generics.html#//apple_ref/doc/uid/TP40014097-CH26-XID_285">擴展一個泛型（Extending a Generic Type）</a>
        </li>
        </ul>
    </td>
    </tr>
</tbody>
</table>
