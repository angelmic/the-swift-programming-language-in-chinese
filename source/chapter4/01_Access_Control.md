# Access Control 權限控制的黑與白

> 翻譯：[老碼團隊翻譯組-Arya](http://weibo.com/littlekok/)
> 校對：[老碼團隊翻譯組-Oberyn](http://weibo.com/u/5241713117)

如果您之前沒有接觸過權限控制，先來聽一個小故事：

>  小明是五道口工業學院的一個大一新生，最近他有點煩惱，因為同屋經常用他的熱水壺，好像那是自己家的一樣，可是礙於同學情面，又不好意思說。直到有一天，他和學姐小K吐槽。

>  學姐聽了之後，說：大學集體生活裡面，大部分東西都是默認室友可以共用的。如果你不想別人拿，我可以幫你做封印，只要打上private標記，它們就看不到你的東西，更加用不了你的東西了。

>  小明說哇靠學姐你還會妖法......


Swift語言從Xcode 6 beta 5版本起，加入了對權限控制（Access Control）的支持。其實權限控制和小明的物品一樣，你可以設定水壺是只有自己能用，還是只有宿舍裡的人能用，還是全校都可以用。

從此以後，你可以好像神盾局局長一樣，完全掌控自己的代碼塊的」保密級別「，哪些是只能在本文件引用，哪些能用在整個項目裡，你還可以發揮大愛精神，把它開源成只要導入你的框架，大家都可以使用的API。

這三種權限分別是：

- #####private 私有的

	在哪裡寫的，就在哪裡用。無論是類、變量、常量還是函數，一旦被標記為私有的，就只能在定義他們的源文件裡使用，不能為別的文件所用。

- #####internal 內部的

	標記為internal的代碼塊，在整個應用（App bundle）或者框架（framework）的范圍內都是可以訪問的。

- #####public 公開的

	標記為public的代碼塊一般用來建立API，這是最開放的權限，使得任何人只要導入這個模塊，都可以訪問使用。


如果要把所有的愛加上一個期限，噢不，是給所有的代碼塊都標記上權限，不累死才怪。還好swift裡面所有代碼實體的默認權限，都是最常用的internal。所以當你開發自己的App時，可能完全不用管權限控制的事情。

但當你需要寫一個公開API的時候，就必須對裡面的代碼塊進行「隱身對其可見」的public標記，要麼其他人是用不到的。

Private（私有級別）的權限最嚴格，它可以用來隱藏某些功能的細節實現方式。合理構築你的代碼，你就可以安全地使用extension和高級功能，又不把它們暴露給項目內的其他文件。

除了可以給整個聲明設權限，Swift還允許大家在需要的時候，把某個屬性（property）的取值權限比賦值權限設得更加開放。

#####舉個例子：
```swift
	public class ListItem {

	// ListItem這個類，有兩個公開的屬性
	public var text: String
	public var isComplete: Bool

	// 下面的代碼表示把變量UUID的賦值權限設為private，對整個app可讀，但值只能在本文件裡寫入
	private(set) var UUID: NSUUID

	public init(text: String, completed: Bool, UUID: NSUUID) {
		self.text = text
		self.isComplete = completed
		self.UUID = UUID
	}

	// 這段沒有特別標記權限，因此屬於默認的internal級別。在框架目標內可用，但對於其他目標不可用
	func refreshIdentity() {
		self.UUID = NSUUID()
	}

	public override func isEqual(object: AnyObject?) -> Bool {
		if let item = object as? ListItem {
			return self.UUID == item.UUID
		}
		return false
		}
	}
```

當我們使用Objective-C和Swift混合開發時，需要注意：

- 如果你在寫的是一個應用，Xcode會生成一個頭文件來保證兩者的可互訪性，而這個生成的頭文件會包含public和internal級別的聲明。

- 如果你的最終產品是一個Swift框架，頭文件裡只會出現標記為public級別的聲明。（因為框架的頭文件，屬於公開的Objective-C接口的一部分，只有public部分對Objective-C可用。）

雖然Swift不推薦大家傳播和使用第三方的框架，但對於建立和分享源文件形式的框架是支持的。對於需要寫框架，方便應用與多個項目的開發者來說，要記得把API標記為public級別。

如果您想了解更多關於權限控制的內容，可以查看蘋果官方最新的《The Swift Language》和《Using Swift with Cocoa and Objective-C》指南，
這兩本指南在iBooks裡面可以下載更新喔。

本文由翻譯自Apple Swift Blog ：https://developer.apple.com/swift/blog/?id=5
