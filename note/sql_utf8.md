在 SQL 裡面會看到有一個 編碼的選項，裡面有一堆~~~編碼，稍微看看說這些編碼有甚麼區別。


### SQL 中的utf8 & utf8mb4
有一個東西叫做 unicode，也就是把全世界所有文字化成編號讓大家對應，所以我們打的字才得以顯現出來。而這只是一個理論上的對應表而已，在電腦中的實際儲存方式則需要交給 UTF 編碼，其中目前最廣泛使用的就是 UTF8。

UTF8 每個字用 1~4 個 Byte，在 unicode 中比較前面的文字（在前面表示比較早被加入，某種程度上算是比較常用到的文字），像是我們平常打的英文或者是數字， UTF8 就只需要 1 個 Byte 來儲存，中文才需要用到 2 個或 3 個 Byte。

講完 UTF8 編碼是甚麼了，那我們來講講 SQL 中的 utf8。還記得我們剛剛講說 UTF8 需要用到 1~4 個 Byte 來儲存，但是 MySQL 中的 utf8 是偷懶版！最多只支援到 3 個 Byte 的儲存空間（後來官方已經將 utf8 改成 utf8mb3 ，用三個 Byte 的意思），使用 utf8 可能會有缺字問題。

因此，現在都應該使用 utf8mb4 才對。

### 排列

當我們要搜尋文字的時候，比對的方式就很重要了，甚麼文字先比對，甚麼文字會被放在後面，這會大大的影響搜尋的速度。所以同樣的是 utf8mb4 ，下面才會分這麼多種，這些差異代表著字的不同校對方式。

![](2020-08-15-00-14-36.png)

例如：可能在英語系國家裡面，裡面文字這可能都會被當成一樣的字做比對：

```
Ä = A
Ö = O
Ü = U
```

但是可能在其他語言中，這些字的意思可能非常不一樣，所以就需要做區分，因此我們才會在這些編碼裡面看到很多國家的名稱，正是針對不同的語言做特別的校對方式。

不過除了不同語言的校對差異，還有一些不同的校對邏輯差異。

#### general vs. unicode
這兩種校對方不依語言而特製的校對方式，所以是最普遍的，這兩種的詳細的差異可以看[這邊](https://dev.mysql.com/doc/refman/8.0/en/charset-unicode-sets.html#_general_ci%20Versus%20_unicode_ci%20Collations)，大致上來說就是在 general 裡面，單字的校對會比較粗糙，字元可能會判斷錯誤，但就是比較快。unicode 就是完完全全依照標準來，比較精細但是比較慢。

效能基本上差異不大，比較建議用 unicode。

#### ci vs. bin vs. cs

ci 是指 case-insensitive，utf8mb4_unicode_ci 在校對上是不分大小寫的。也就是 `a = A, b = B` 

bin 的話會用直接用文字儲存重記憶體的的二進位數據比對，例如 utf8mb4_bin 會區分大小寫的且也會區分 Ä 和 A 的不同

[參考資料](https://khiav223577.github.io/blog/2019/06/30/MySQL-%E7%B7%A8%E7%A2%BC%E6%8C%91%E9%81%B8%E8%88%87%E5%B7%AE%E7%95%B0%E6%AF%94%E8%BC%83/
)中還有提到 cs 不過 MarilDB 似乎沒有這個選項...  
cs 是指 case-sensitive，例如 utf8mb4_unicode_cs 是會區分大小寫的


### 結論
沒毛病就選 utf8mb4_unicode_ci

## 參考文章
https://khiav223577.github.io/blog/2019/06/30/MySQL-%E7%B7%A8%E7%A2%BC%E6%8C%91%E9%81%B8%E8%88%87%E5%B7%AE%E7%95%B0%E6%AF%94%E8%BC%83/

https://zh.wikipedia.org/wiki/UTF-8#MySQL

官方文件：  
https://dev.mysql.com/doc/refman/8.0/en/charset-unicode-sets.html

https://stackoverflow.com/questions/766809/whats-the-difference-between-utf8-general-ci-and-utf8-unicode-ci/15170166#15170166