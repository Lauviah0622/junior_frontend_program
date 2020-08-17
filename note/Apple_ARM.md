# Apple, ARM, and What It Means

https://zh.ifixit.com/News/42949/apple-arm-and-what-it-means

- MAC 因為 Intel 的 CPU 發展緩慢而受限制，所以在想要試著使用 ARM　架構
- 68K => 1990： power PC（by IBM） => 2006 Intel X86


指令集架構(ISA)是甚麼？
其實想一下，電腦 或者說 CPU 只是很複雜的機器而已。電腦只看得懂 0 跟 1。但是我們寫程式不會用0 跟 1 ，就算是最底層的組合語言也不會用，因為組合語言會被編譯成 opcode，而有哪些 opcode 就是所謂的 ISA。

ISA 也就是定義了有哪些  opcodes
![](2020-08-15-10-44-29.png)

現在的電腦基本上就只有兩種 ISA 被廣泛運用了，一種是 ARM，另一種就是 x86

x86 運用一個極致廣泛，從小型筆電到大型 server 都在用阿，其實它的性能非常強悍，不過這個架構從 1978 年就開始使用了，也因為歷史悠久，所以裡面有很多的坑，沒錯，非常非常多的坑，而且整個架構非常複雜。

相較於 x86，ARM 就小而美了許多，而且 ARM 功號低、成本低、效率也比 x86 還要好上許多，雖然效能比不上 x86 ，但是其實也不算差。

最近有些資料中心已經開始使用 ARM 架構的處理器，雖然 ARM 架構處理器的效能不及 x86，但是功耗比 x86 低上許多，而且對資料中心而言，單線程的效能並不是最需要的，而是總資料流量 (total throughput，不太知道怎麼翻) 還有功耗。

有些筆電已經開始使用 ARM 架構了（注：據我所知有 surface pro X）

# ARM 與蘋果

大部分的廠家都只是使用 ARM 給的架構，然後照著做出 CPU，所以大家都差不多速度差不多快，but 有些廠商不一樣，他們取得了授權並且做出完全客製化的 CPU ，蘋果就是其中之一阿，從 A6 開始就發展自已的 CPU。看起來蠻屌的拉，不過一開始幾代其實並沒有多誇張，也和其他的 ARM 架構 CPU 一樣，低功耗、低成本，但是效能平平。但是到最近，如果你有一台 iphone 11，他的單線程效能已經跟上筆記型電腦，但卻還是很低的功耗。

如果把這顆 CPU 塞到筆電裡面呢？

筆電有更大的電池，也有更好的散熱，意味著我們可以讓 CPU 的功率更上層樓。Iphone 11 有 11.47Wh 的電量，但是新的 MacBook 卻有 100Wh 的電量，八倍阿兄弟！雖然螢幕也大上許多所以需要消耗更多的電，但是仍然有更高的電能可以供給給 CPU，更別說如果插電，那功耗就完全不成問題，在筆電上使用 ARM 架構不就得飛上天？

而且還不只這個問題，Mac 目前採用的是 Intel 的 x86 架構處理器，換成 ARM 架構的原因，除了 ARM 的吸力，還有 Intel 本身的推力。

# Intel

大家都笑說 Intel 是牙膏廠不是沒有原因的喔，Intel這幾年所推出的 CPU 基本上都是換腳不換藥，並沒有飛躍性的突破。Intel 卡在 14nm 的製程已經四年了，而且他們的微架構從根本上破壞了安全性，更糟的是，他們已經不再完全了解自己的晶片。

Intel 發布了 14nm 14nm+ 14nm++ ，但是其他人（像是 AMD）已經在 7nm 的世界了，更進步的製程代表更低的功耗以及更高的效能，Intel 已經失去了過去在處理器的龍頭地位。

而在安全性方面，發現了 Meltdown 還有 Spectre 的漏洞，這些漏洞都是因為最底層的 CPU 漏洞，即使 Intel 使用各種方式補了漏洞，但是因為這些補丁的關係而可能拖累處理器的速度，也因此你現在的電腦可能比你一年前買的時候還要慢。

最後為什麼說他們不太了解自己的晶片呢？我也不太懂，，所以看原文

> I do know that quite regularly, some microarchitectural vulnerability comes out that rips open their “secure” SGX enclaves—again. In the worst of them, L1 Terminal Fault/Foreshadow, the fix is simple—flush the L1 cache on enclave transition. The hit on processing speed isn’t a dealbreaker, but the point is that Intel didn’t know it was a problem until they were told

總之因為上面的各種問題，讓 Apple 繼續跟 Intel 合作可能不是個好主意，難道 Apple 會選擇另一個 x86 架構的 CPU - AMD 上嗎？

# Apple 使用 ARM 的好處

Apple 自己使用 ARM 有甚麼好處？如果 Apple 開發自己的 ARM Cpu，那麼他就可以把自家的 T2 安全晶片集合進 CPU 裡面，甚至他們可以把 GPU 合併進進主 CPU。更小的晶片代表著更低的工耗還有更好的效能。

此外，還記得 macbook 的電量嗎？ 100wh，為什麼是這個數字呢？因為在飛機上能[隨身攜帶裝置的電池容量](https://www.rainbow88shop.com/blog/posts/%E8%A1%8C%E5%8B%95%E9%9B%BB%E6%BA%90%E8%83%BD%E5%90%A6%E7%99%BB%E6%A9%9F%EF%BC%8C%E7%9C%8B%E7%9C%8B%E9%80%99%E8%A3%A1%E4%BD%A0%E5%B0%B1%E7%9F%A5%E9%81%93%E4%BA%86%EF%BC%81%E6%90%9E%E6%B8%85%E6%A5%9A%E4%BB%80%E9%BA%BC%E5%8F%AB%E5%81%9A%E7%93%A6%E7%89%B9%E5%B0%8F%E6%99%82)必須要低於 100wh，雖然 apple 標榜 100wh 但是他們有聲明他們的實際電量為 99.8 wh，因此他們不可能讓電池電量超越這個數字。那 apple 想要增加 macbook 使用時間的唯一道路就是減少裝置的功耗，嘿嘿，Apple 的 ARM 晶片可以提供一樣的效能但是更低的耗電，還不香嗎？

除此之外，從商業上的角度，這會更加區分 mac 和其他的 laptop 產品，其他使用 ARM 架構的筆電目前都 GG 了，而 Linux 架構的電腦除了 Chrome book 以外並沒有很好的成績。而且蘋果更可以把他在 Iphone 上的各種功能搬進來 macbook ，像是  CPU 上的機器學習加速器, 神經網路引擎, 語音辨識的硬體，甚至可已讓 mac 有 cellular modems，可以直接連線行動網路。

# 
不過 apple 現在遇到的問題是，要怎麼度過 x86 架構到 ARM 轉換的過渡期，雖然說是問題，但是他們在這上面已經有很多經驗，從 68K -> PowerPC -> x86。在轉換的期間最重要的就是模擬，你必須用 ARM 的架構去模擬 x86 的指令集，讓過去使用 x86 指令集的程式可以運行，不過其實 aplle 已經完成這點。

後面還有提到甚麼庫調用甚麼的，不熟阿...真的不熟阿，完全不懂那個是甚麼意思



