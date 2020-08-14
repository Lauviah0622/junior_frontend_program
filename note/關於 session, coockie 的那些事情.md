## 從 server 還有  browser 說起
Server 傳送給 Browser 的 response 都是沒有狀態的（stateless），每次只要 Request，他就只會給你對應的 Response ，這是什麼意思？

簡單說 server ，就像一台販賣機，為什麼是販賣機呢？販賣機不像早餐店的阿姨，當你去你家巷子口的美而美的時候，老闆娘遠遠看到你就會大聲呼喊：「帥哥，今天還是一樣的薯餅蛋餅要胡椒不要油膏加大冰奶嗎？」，甚至當你們已經心有靈犀的時候：「今天老樣子嗎？」老闆娘總是記起來你吃甚麼，你昨天吃什麼，但販賣機不會。販賣機就是依照你所選的按鈕給你東西而已，永遠不會記說哪個客人選了幾號的飲料，而這就跟 server 一樣。server 只會依照你 request 所傳送的資料給你對應的 response。

但是有些時候我們會有這樣的需要：我們會希望 server 對於自己電腦所送出的 request 都有一個狀態。就像如果你是一個販賣機狂人，你可能會希望販賣機像早餐店老闆娘一樣，只要確定是你，就直接跳出你每次都選擇的那個飲料。

## 讓 server 記住這個 Request 是誰
要怎麼讓 server 可以記住我們 request 的狀態？

感覺可以試著想想，要怎麼讓販賣機可以跳出你最愛喝的那個飲料？很簡單嘛，這時候就該參考一下隔壁產業 - 湯姆熊裡面的街機，還記得頭文字 D 嗎？你可以買卡片來儲存你的紀錄，每次要玩之前都刷一下卡，就可以繼續上次的紀錄。

販賣機也可以模仿一下這個商業模式，給你一張販賣機的會員卡，或者是直接把資料存在你的悠遊卡裡面，當你要買飲料的時候就逼一下，販賣機就會跳出你喜歡的飲料了，不用猶豫的老半天，是不是很棒。

那假如販賣機真的可以做到這種事情，那資料應該會是怎麼流動的呢？我們先假設你有拿著一張空卡（沒有資料的卡）去販賣機。流程應該會是這樣：

1-1. 刷卡，然後點自己的飲料
1-2. 販賣機收到你點飲料的訊號了，讓你點的飲料掉下來，而在你的卡裡面說：這傢伙喜歡喝舒跑

然後你又去販賣機買了飲料

2-1. 刷卡，但是你知道說上次你資料在卡裡面了，所以不用點飲料
2-2. 販賣機收到你卡裡面的資料，直接掉出舒跑。 

其實 browser 還有 server 做的事情跟這個有 87% 像。暫且聽我娓娓道來，我們以最常使用狀態的理由：登入購物網站。

1-1. 當我們第一次登入購物網站的時候，我們輸入自己的帳號密碼
1-2. server 收到你的帳號密碼之後，確認你是會員，並且在你的 browser 中做一個記號說：我是

然後你去逛這個購物網站的其他頁面時

2-1. 傳送 request 要求商品的頁面，而且會把 server 做的記號也一併傳回去（販賣機的卡片）
2-2. server 收到你 request 中的記號，他知道你是金城武，是會員。所以就顯示下單的選項

剛剛講的內容中一直講到記號，那到底是什麼記號？那就是我們很常聽到的東西：cookie

## Cookie

![[Pasted image.png]]

應該每個人都有看過這樣標示吧？

裡面所提到的 cookies 就是一個儲存狀態的機制，也就是販賣機的會員卡，也就是剛剛購物網站做的記號。那 cookie 實際上是怎麼設置的呢？


剛剛提到 server 確認會員之後在你的 browser 做記號，實際上由兩個步驟所組成

1. response 的 header 中標記 Set-cookie 這個 header，格式是這樣 `Set-Cookie: <cookie-name>=<cookie-value>` 。

	```
	HTTP/2.0 200 OK
	Content-Type: text/html
	Set-Cookie: yummy_cookie=choco
	Set-Cookie: tasty_cookie=strawberry

	[page content]
	```
1. 當 browser 接收到 header ，發現裡面有 Set-cookie 後，會在儲存 cookies 在電腦上。

到這裡 server 就相當於儲存一個狀態在我們的電腦上了。

3. 再來，當我們傳送 Request 到 **相同 domain** 時，browser 會在 request 中設置對應的 header。

	```
	GET /sample_page.html HTTP/2.0
	Host: www.example.org
	Cookie: yummy_cookie=choco; tasty_cookie=strawberry
	```

4. 瀏覽器就會依照 cookie 的內容來知道說，喔~這個 ip 所傳送過來的 request 是有一些狀態的，來 response 相對東西。

大概是這樣，過程其實跟剛剛上面有會員卡機制的販賣機差不多。想要看看 cookie 的廬山真面目，我們可以打開 `devTools > Application`。可以看到這些就是 server 在我們 browser 儲存的狀態。

![[Pasted image 1.png]]

我們可以看到表格中有 Name 跟 Value，而這就是 cookie 儲存的格式，另外值得注意的是 Expires，coockie 是會過期的，就像 cookie 一樣。在Set-Cookie 的時候會設置一個 lifetime。

```
Set-Cookie: id=a3fWa; Expires=Wed, 31 Oct 2021 07:28:00 GMT;
```

到期後 browser 就會自動把 cookie 刪除。

### cookie 會拿來做什麼呢？
最常見的就是會員機制了，最常見的就是會員機制了。

想像一下，當你輸入帳號密碼之後，然後到了會員中心之後，你要看一下購物車。因為 request 是沒有狀態的，你能夠進入會員中心是因為你在登入的 request 輸入了帳號跟密碼，但是你在購物車的頁面並沒有，所以你必須再點一次都入，再輸入一次帳號密碼。

但是這樣的使用者體驗極差啊，怎麼可能跳頁就輸入一次帳號密碼。所以才會利用 cookie 儲存會員的資料，server 只要看到說 request 裡面有 `user` 這個 cookie 就知道說這個 request 的主人已經登入過了。就不用重新登入。

其他運用這個原理的還有例如：主題的切換、個人設定、資料庫、遊戲分數等等。還有一樣很重要的：用戶資料追蹤。

可以看[這個影片](https://www.youtube.com/watch?v=QWw7Wd2gUJk) 了解

## session 機制
sessnion 機制又是甚麼？在了解 session 機制的原理，或者是怎麼時做之前，我們必須先知道這個名詞是甚麼。首先必須要知道，sessnion 機制並不是確切的一個技術，應該是一個統稱。不像剛剛講的 cookie 一樣有明確的指令或者是規範告訴你用甚麼東西（cookie 用 http 的 header）儲存什麼類型的資料（key, value）。

就像我們在講「機車」這個詞，只要你是有手把、你可以騎著他到處跑的交通工具，我們都稱他為「機車」，不管你電動的、幾個輪子、碟煞還是鼓煞，這些都是「機車」。session 機制可以用很多不同的方式來實作，像是 server 端可能用不同語言、或者是用 cookie 或其他技術、甚至你夠哈扣你也可以幹一個 session 而不用語言內建的也可以（如果沒意外，像是懶惰之類的狀況等等應該會這樣做）。







## 在 PHP 如何實作 Session 機制

## 參考資料
https://www.ruanyifeng.com/blog/2019/09/cookie-samesite.html
https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie/SameSite
