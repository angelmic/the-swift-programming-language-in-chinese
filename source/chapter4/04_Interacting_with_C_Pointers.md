# Swift與C語言指針友好合作
-----------------

> 翻譯：[老碼團隊翻譯組-Relly](http://weibo.com/penguinliong/)  
> 校對：[老碼團隊翻譯組-Tyrion](http://weibo.com/u/5241713117) 

本頁包含內容：

- [用以輸入/輸出的參數指針](#inout-para-pointer)
- [作為數組使用的參數指針](#array-as-para-pointer)
- [用作字符串參數的指針](#string-as-para-pointer)
- [指針參數轉換的安全性](#security-of-pointer-cast)

Objective-C和C的API常常會需要用到指針。Swift中的數據類型都原生支持基於指針的Cocoa API，不僅如此，Swift會自動處理部分最常用的將指針作為參數傳遞的情況。這篇文章中，我們將著眼於在Swift中讓C語言指針與變量、數組和字符串共同工作。

####用以輸入/輸出的參數指針
C和Objective-C並不支持多返回值，所以Cocoa API中常常將指針作為一種在方法間傳遞額外數據的方式。Swift允許指針被當作`inout`參數使用，所以你可以用符號`&`將對一個變量的引用作為指針參數傳遞。舉例來說：`UIColor`中的`getRed(_:green:blue:alpha:)`方法需要四個`CGFloat*`指針來接收顏色的組成信息，我們使用`&`來將這些組成信息捕獲為本地變量：
```swift
var r: CGFloat = 0, g: CGFloat = 0, b: CGFloat = 0, a: CGFloat = 0
color.getRed(&r, green: &g, blue: &b, alpha: &a)
```
另一種常見的情況是Cocoa中`NSError`的習慣用法。許多方法會使用一個`NSError**`參數來儲存可能的錯誤的信息。舉例來說：我們用`NSFileManager`的`contentOfDirectoryAtPath(_:error:)`方法來將目錄下的內容列表，並將潛在的錯誤指向一個`NSError?`變量：
```swift
var maybeError: NSError?
if let contents = NSFileManager.defaultManager()
	.contentsOfDirectoryAtPath("/usr/bin", error: &maybeError) {
	// Work with the directory contents
} else if let error = maybeError {
	// Handle the error
}
```
為了安全性，Swift要求被使用`&`傳遞的變量已經初始化。因為無法確定這個方法會不會在寫入數據前嘗試從指針中讀取數據。

####作為數組使用的參數指針
在C語言中，數組和指針的聯系十分緊密，而Swift允許數組能夠作為指針使用，從而與基於數組的C語言API協同工作更加簡單。一個固定的數組可以使用一個常量指針直接傳遞，一個變化的數組可以用`&`運算符將一個非常量指針傳遞。就和輸入/輸出參數指針一樣。舉例來說：我們可以用Accelerate框架中的`vDSP_vadd`方法讓兩個數組`a`和`b`相加，並將結果寫入第三個數組`result`。
```swift
import Accelerate

let a: [Float] = [1, 2, 3, 4]
let b: [Float] = [0.5, 0.25, 0.125, 0.0625]
var result: [Float] = [0, 0, 0, 0]

vDSP_vadd(a, 1, b, 1, &result, 1, 4)

// result now contains [1.5, 2.25, 3.125, 4.0625]
```

#用作字符串參數的指針
C語言中用`cont char*`指針來作為傳遞字符串的基本方式。Swift中的`String`可以被當作一個無限長度UTF-8編碼的`const char*`指針來傳遞給方法。舉例來說：我們可以直接傳遞一個字符串給一個標准C和POSIX庫方法
```swift
puts("Hello from libc")
let fd = open("/tmp/scratch.txt", O_WRONLY|O_CREAT, 0o666)

if fd < 0 {
	perror("could not open /tmp/scratch.txt")
} else {
	let text = "Hello World"
	write(fd, text, strlen(text))
	close(fd)
}
```

#指針參數轉換的安全性
Swift很努力地使與C語言指針的交互更加便利，因為它們廣泛地存在於Cocoa之中，同時保持一定的安全性。然而，相比你的其他Swift代碼與C語言的指針交互具有潛在的不安全性，所以務必要小心使用。其中特別要注意：
- 如果被調用者為了在其返回值之後再次使用而保存了C指針的數據，那麼這些轉換使用起來並不安全。轉換後的指針僅在調用期間保證有效。甚至你將同樣的變量、數組或字符串作為多指針參數再次傳遞，你每次都會收到一個不同的指針。這個異常將全局或靜態地儲存為變量。你可以安全地將這段地址當作永久唯一的指針使用。例如：作為一個KVO上下文參數使用的時候。

- 當指針類型為`Array`或`String`時，溢出檢查不是強制進行的。 基於C語言的API無法增加數組和字符串大小，所以在你將其傳遞到基於C語言的API之前，你必須確保數組或字符的大小正確。

如果你需要使用基於指針的API時沒有遵守以上指導，或是你重寫了接受指針參數的Cocoa方法，於是你可以在Swift中直接用不安全的指針來使用未經處理的內存。在未來的文章中我們將著眼於更加高級的情況。