---
title: '[转载]跨平台開發的一些姿勢'
date: 2019-11-25 17:52:30
tags:
---

# [跨平台開發的一些姿勢原文地址](https://medium.com/@kingapol/%E8%B7%A8%E5%B9%B3%E5%8F%B0%E9%96%8B%E7%99%BC%E7%9A%84%E4%B8%80%E4%BA%9B%E5%A7%BF%E5%8B%A2-e2a59b7849ce)

## 本文为为了照顾国内木有办法访问境外站点所以只能看转载的同学复制而来，如有侵权，联系我后立刻删除

Cordova、Xamarin、NativeScript、React Native、Electron、Flutter

# 前言

不知道是幸運還是不幸，從職業生涯早期開始就常常在做各種跨平台開發，從早期的Cordova到現在的ReactNative，從SmartTV到Android、iOS、MacOS以及Windows（還有死去的Windows Phone，我可愛的Lumia 720只能變成老媽機了QQ），雖然不敢說全部都融會貫通，但多少也累積了一些心得與想法。趁著記憶力退化忘光光之前，寫兩篇文來記錄近年來跨平台開發的發展史，順便總結一下心得。

這篇是講古篇，回味並簡介一下各種Solution，有些可能年代久遠記憶有誤或作法有更新，還請不吝指正。

# Cordova

![cordova-icon](https://blog-1256223865.cos.ap-shanghai.myqcloud.com/blog/platform-cors/cordova.png)

說到跨平台，一般人好像都會先想到這隻小機器人
Cordova是很早期的跨平台solution，它是從更早期的PhoneGap分支出來的。在我剛開始接觸跨平台開發的那個年代（大概2012年），他是最熱門的跨平台Solution。為什麼呢？因為他簡單好上手，而且那時候他的對手是Xamarin跟 Titanium，兩者都是要付費的。

![cordova-structor](https://blog-1256223865.cos.ap-shanghai.myqcloud.com/blog/platform-cors/cordorva_structure.png)

Cordova的架構很簡單，就是一個簡單的Activity（為了避免麻煩，底下的架構都用Android的觀點來解釋，絕對不是我iOS不熟喔），上面搭載一個CordovaWebView Component，他是一個改造過的WebView，加裝了一些Cordova API，讓你藉此對Native的部分做溝通。你的App基本上就是一個Mobile Web，只是多了一些跟Native溝通的能力。這樣簡單爽快的設計給Cordova帶來了幾項優點，第一就是好上手：前端工程師（或Web工程師，那個年代還不是大家都聽過前端這個詞）只要知道怎麼做Mobile Web，就可以無痛的build出一個跨平台的App，聽起來是不是很吸引人呢？

不過，事情總是沒有想像中的美好。Cordova這種簡單明瞭的架構是個兩面刃：好的方向來說，他是個Web，你怎麼搞Web就能怎麼搞他；壞的方向來說，他只是個Web，你怎麼搞都只是個Web，就像我再怎麼打扮也不會變成劉德華。那個年代的Mobile device硬體水準還不是非常的好（還在1GB RAM的時代），Mobile Web的效能差距跟Native View比起來十分明顯，特別是在UI的回饋感這件事情上，Android上的Web UI跟Native UI比起來差異很大，一般的使用者幾乎都分辨得出來（不過相同硬體條件下，iOS device上就沒有那麼明顯的差距，這是基於兩個平台的Rendering架構不同所產生的差異，詳情請洽專業人士）。

當然，Web工程師們也很努力地想要改變這樣的UX落差，因此用了很多的trick（大部分是關於GPU加速的）來改善Android上的UI操作體驗，其中做得最好的當屬Ionic了。當時正值智慧型手機發展迅速的年代，Mobile web UI framework如雨後春筍般地冒出，像JQuery Mobile、Sencha Touch、Kendo UI、Bootstrap等等，而Ionic是其中的後起之秀，他們在當時能異軍突起的最大原因就是＂質感＂。Ionic團隊並沒有提供特別多的UI Component（應該說，相對的比別人少很多），但是他們發佈出來的組件都非常的順暢，而且會為了一些十分微小的UX細節不斷的改進，再加上跟ng1的緊密結合，以及專門為了Cordova WebView做的細節調校，都讓他成為Cordova app的搭配首選。不過很遺憾的，不管Ionic team以及Web開發者們多麼的努力，終究無法突破架構上所帶來的瓶頸，這也催生了下一代跨平台solution的發展。

（題外話：由於Cordova在架構及硬體環境下產生的UX瓶頸，再加上他又是當時最普遍的跨平台solution，使得大部分的人產生了＂跨平台開發=效能不佳，使用者體驗差＂的觀感，這也使得後來的跨平台solution在推廣時最容易碰到的問題就是：＂我之前用過Cordova，那個效能balabalabala…＂，然後你就要花上一番唇舌跟他解釋新一代的solution跟Cordova有什麼不同…Orz）

# Xamarin

![xamarin](https://blog-1256223865.cos.ap-shanghai.myqcloud.com/blog/platform-cors/xma.png)
我大C#王朝榮光永存！
在被微軟收購之前，Xamarin就已經是小有名氣的跨平台Solution了，不過跟開源的Cordova不同，Xamarin當時是需要收費的，這也是他為什麼熱門度在當時遠不及Cordova。而另一個Xamarin沒有紅起來的原因是：他使用C#當開發語言。我並不是說C#不好，事實上我也是從C#開始寫程式的，我很習慣也很喜歡這個語言，只不過在.NET Core還沒出現之前（ Oracle大大也還沒開始吉人之前），C#的族群就是相對的比較封閉，開源的資源也比較少（相較於當時猛烈竄起的Javascript來說），而這就造就了Xmamrin先天上的劣勢。

Xamarin的架構比Cordova複雜多了，他實現了很多事，包括一組完整的Android & Java binding，還有一個跨平台的.NET runtime — Mono（其實應該是先有Mono才出現Xamarin，不過背後是同一間公司，而且當時Mono最大的應用好像也就是Xamarin…）。Xamarin大致上分成幾個部分：Xamarin.Android、Xamarin.iOS、Xamarin.Mac（後來才出現的）以及Xamarin.Forms。因為微軟收購了Xamarin之後充分的展現了他們寫文件的專業（Facebook學學好嗎？），這裡就大概提一下這個架構的優缺點就好，詳情請參考官方文件。

Xamarin架構的優點是：他的效能損耗比較少，應該說比起Cordova壓倒性的少。Cordova必須承擔Web Rendering所帶來的巨大損耗，但Xamarin只有跨語言介面的損耗而已，相較起來相當輕微，這也給他在UX上帶來優勢。不過這個架構當然也是有缺點的，最大的問題就是，新的Native API必須等待官方的封裝才能使用，不過現在Xamarin也OpenSource了（感謝網友指正），所以等不及的話或許可以自己來。另外就是，他挺肥的，不過接下來出場的都是胖子，所以其實也還好。

# Titanium

![titanium](https://blog-1256223865.cos.ap-shanghai.myqcloud.com/blog/platform-cors/xma.png)
安安你好，很高興認識你
對不起，我跟他老兄不熟，只記得他當初也是要收費的，然後走的是JS to Native Binding的形式，可是聽說雷超級多，就沒有去踩了XD。某方面來說他也算是個先烈（？

# NativeScript
![NativeScript](https://blog-1256223865.cos.ap-shanghai.myqcloud.com/blog/platform-cors/nativeScript.png)
生不逢時的孩子QQ
從NativeScript開始，應該可以算是第二代的跨平台solution了。NativeScript是Telerik（做過Kendo UI）推出的，後來轉成開源的形式。他們的想法跟titanium類似，都是想做js跟native之間的binding，而他們採用的是映射（reflection）的形式。我們知道v8有開了很多的API讓你去對Javascript runtime做一些色色的事情，例如Node.js就是利用這個方式實現了commenjs，NativeScript也做了類似的事，他們把Android / iOS上的API都映射到NativeScript runtime裡，讓開發者可以直接以Javascript調用這些native API。這種做法給NativeScript帶來超級強大的擴充性，因為你可以使用“任何”原生app可以使用的api，包括第三方lib，而且不需要做另外的動作；當平台有API變更時，所有的變更也都可以馬上被使用（除非reflection的機制在更新中被破壞了），不像Xamarin要等待團隊去做更新。超猛的，對吧？

（有一點要注意的是，NativeScript並不是直接用Java的reflection去動態查找API，因為效能不佳，所以他會預先生成一個Metadata在apk裡用來做查找跟mapping）

NativeScript在View上也做得不錯，除了原生的Component之外，他還提供了一個CSS subset作為styling使用，這讓他在UI開發上比Native app更加方便（題外話，NativeScript沒有用ReactNative的Yoga來做CSS Compile我覺得還蠻可惜的，兩邊統一一下資源才能做更多事啊）。NativeScript一開始是使用Angular來做UI，不過後來他們分離出了三種開發方式：基本的js/ts、Angular、Vue（是的，這就是為什麼我沒打算研究Weex的原因），而且三種方式都可以享受到data-binding的爽感。超棒的，對吧？

NativeScript還有一個很大的優點，就是他把TypeScript當成一等語言（本體也是用TypeScript寫成的）。不過想想這也是理所當然，畢竟在使用reflection機制的情況下，如果沒有TypeScript輔助，光是API refactor就會搞死人（沒接觸過TypeScript的朋友，就試想一下用記事本寫Java的感覺吧XD）。

不過，這樣的架構也為NativeScript帶來了一些缺陷，首先就是：開發者要有一定程度的Native知識。因為NativeScript runtime是基於v8/JavaScriptCore，而不是Webkit，所以你沒有辦法使用任何的WebAPI（除非有人額外幫你實現）。舉例來說，常見的XmlHttpRequest或fetch是屬於Webkit這層的實現，在NativeScript裡就需要使用native平台上的實現來做包裝（題外話，在找實現的時候偶然翻到Vj（ReactNative的核心開發者）在NativeScript開過一些issue，看來兩邊對於一些實現的想法不太一樣），這也加重了開發者對於Native相關知識的依賴。不過在這方面NativeScript還是相對簡單好做的，因為第三方lib都可以被映射進javascript裡面，只需要寫個.d.ts就可以了，不像ReactNative每次都要自己寫Bridging。

總的來說，我認為NativeScript是一個相當好的跨平台solution。他們用映射的方式達到了極大的擴充性，UI上也跟現代的Angular / Vue做結合，帶來了很高的開發效率。他之所以沒有紅，主要的原因是出現的時間點有點差。NativeScript剛推出沒幾個月，還來不及成熟的時候，ReactNative就夾帶著React社群的超人氣橫空出世了。如果他能夠早一點出現，也許現今的版圖分佈會不太一樣…。

# React Native
![react-native](https://blog-1256223865.cos.ap-shanghai.myqcloud.com/blog/platform-cors/rn.png)
弟兄們，讓他們見識見識，什麼叫人多就是力量！
人多就是力量，這真的是ReactNative最好的寫照XD。在2018年提到跨平台開發，除了少數還停留在Cordova時代的人（好吧，可能不是少數），比較有在追蹤相關資訊的應該都會提到ReactNative這個東西，剛好小弟也是第一批把ReactNative用在production上的人（當時是0.0.3版，第一個public版，想想我當時膽子還真大XD），下面就簡單的介紹一下這玩意兒到底在紅幾點的吧。

首先，ReactNative在架構思路上跟NativeScript有些類似，想做的都是＂用Javascript控制Native＂這件事，只是他們採取了不同的做法。ReactNative沒有像NativeScript一樣做映射，而是建構了一組RCTBridge，用來做js跟native之間的溝通，要使用native API或native Component的話都要在native層做bridging，然後export到js裡面（類似你寫Node的C++ addon的感覺）。這樣的好處是包出來的檔案會比較小，架構實現上也比較單純；壞處就是每個native module都要自己橋接一次…。坦白講我覺得關於這部分NativeScript的作法好很多，只是ReactNative這麼做一定有他的想法，不過目前沒有找到官方的討論串有在說這件事，也許改天去發個issue問一下XD

ReactNative能有今天的聲勢，很大一部分是得利於React生態圈的整體發展優勢，以及有Facebook已經在他們的app上做過產品實踐，這對很多人而言是一劑強心針，再加上諸如Airbnb，Uber等大型公司也紛紛採用，更造成了推波助瀾的效果。不過ReactNative目前其實還不算是在一個穩定的狀態，他們每兩週都會更新一次，而且常常有breaking change，這也是開發時要注意的一點。

另外一個開發ReactNative時要注意的點是：你必須擁有native platform相關的知識。應該說，自從Cordova之後的主流跨平台solution都會要求要具備一定程度的平台原生相關知識。雖然說比起NativeScript，ReactNative在沒有任何的原生知識的情況下也是可以build出一個可用的app，但是這會大大的限縮了你的可擴展性（沒辦法自己寫plugin，要依賴第三方或是官方），而且在不了解原生的渲染以及運作方式的情況下，可能會不小心把效能給炸了（看過很多抱怨RN效能有問題的人，都是沒搞清楚整個api背後的原理，然後做出一些很變態的舉動…）。

ReactNative當初出現時打的口號是“learn once, write everywhere”，開發團隊沒有追求理想中的“write once, run everywhere”，而是讓不同平台上保有不同的平台特性。這個中心思想所帶來的架構設計，讓ReactNative可以很輕易的擴展到其他平台上，例如第三方開發者實現的react-native-macos、react-native-appletv（後來被整合進官方裡）、微軟官方開發的react-native-windows（原本是叫uwp啦，不過隨著uwp逐漸尷尬，就改名叫windows了…實際上還是uwp）、Samsung官方也參一腳弄了個react-native-tizen，幾乎可以說所有平台上都可以看到ReactNative尬上一腳（不過成熟度又是另一回事了）。

整體而言，RN算是一個頗為可靠的跨平台solution，雖然目前還不到相當穩定的階段，但是他有著良好的核心架構、高擴展性以及龐大的社群支撐，目前看不出什麼值得擔憂的大毛病，如果想接觸跨平台開發的話，學個ReactNative總是不虧的。

# Electron

![electorn](https://blog-1256223865.cos.ap-shanghai.myqcloud.com/blog/platform-cors/electron.png)
等等…有奇怪的東西混進來了？
（嗯？你問我為什麼冒出來一個桌面端的東西？啊我標題是打＂跨平台＂，又沒限定Android跟iOS，你咬我啊笨蛋。）

Electron的前身叫做Atom-Shell，本來是GitHub發布開源編輯器Atom時一併發布的副產物，但是後來這個副產物的影響力遠遠的超過了Atom editor本身，於是便改名為一個獨立專案，也就是現在的Electron。Electron的本質很簡單，就是 Chromium+Node.js的組合，兩者之間透過ipc通訊（其實還有一個更早出現的專案叫Node-Webkit也是使用類似的模式，不過我沒用過，因此就不介紹了）。

關於Electron，我有點不知道該寫些什麼，因為他實在是太成熟了。在2018年的現在，開發Electron app對js有一定水平的工程師而言就跟喝開水一樣簡單，前端是一個完整的Web環境，而且還是兼容性高、走在時代前端的Chromium，可以使用各種先進的語法以及WebAPI；而後端是大家的好朋友Node.js，而且版本也相當高，不用擔心語法或模組支援不足。Electron也不需要擔心跟平台的互動性不足，得利於豐沛的Node.js生態圈，大部分你需要的功能都可以找到模組，而像自動更新跟打包發佈這些大多數App都需要的東西，Electron跟他的生態圈也都幫你搞定了。就如同官網的標語，Electron幫你處理了許多阿哩不搭的瑣事，你只要專心在核心功能的開發上就好了，it just works！

硬要說Electron的缺點的話，大概就是…肥。這好像是跨平台Solution擺脫不了的宿命了（苦笑）。因為你必須把完整的Chromium帶進去，所以最小的HellowWord壓縮起來也要幾十mb，不過這其實對桌面軟體而言不成問題，問題比較大的是另外兩個面向：記憶體用量、以及啟動時間。

因為前端用的是知名吃貨Chromium的關係，Electron在記憶體用量上一直為人所詬病，基本上開起來就吃個1xx mb是很正常的事，應用複雜度高的話數字也會猛烈攀升。基本上這鍋完全就是Chromium的，Electron團隊應該是無能為力，只能看Chromium未來能不能改善這個問題了。啟動時間這部分，其實微軟的VSCode做了一個相當好的範例，他們透過漂亮的lazy require技巧，在一個功能頗多的app上壓榨出了一個很好的啟動時間（比起Atom而言）。不過這仍然有極限在，也就是Chromium的啟動時間會成為一個瓶頸，所以可以看到VSCode的開啟速度仍然遜於SublimeText。不過在大多數的場合這並不是太重要，因為在HDD上差距大約1秒左右而已，SSD則幾乎感覺不出差距。

題外話，最近有個東西叫 Proton Native，光看名子就知道是來打對台的。他的想法是把Chromium用輕量的libui來取代，然後透過RN的binding方式來操作。這看起來是個很不錯的想法，可以大大的輕量化整個app，不過目前還太草創，建議先觀望就好。

來說個結論吧，如果今天要開發一個桌面為主的跨平台app，那Electron可以說是當仁不讓的首選。不論是整體的成熟度、社群的熱門程度、可用的資源、旗艦級的showcase（我幾乎可以肯定你的電腦裡有Electron base的app。如果現在沒有，以後也會有XD），從各方面來看都令人安心。其他的選擇例如CEF，NW.js，react-native-windows&react-native-macos雖然各有所長，但整體而言Electron仍然是第一選擇。

# Flutter
![flutter](https://blog-1256223865.cos.ap-shanghai.myqcloud.com/blog/platform-cors/flutter.png)
好的東西不一定會紅喔
在我寫這篇文章的時候，Flutter還在beta階段，而且我也不是很熟，但是我還是想把它放進來大概講一下，因為這東西真的很有趣，可以說是一個新時代的展望。

Flutter是Google推出的一個跨平台Solution，以Dart作為主要語言，可以在Android／iOS／Fuchsia上面運作。其實這東西已經醞釀很久了（大概2~3年），他當初叫Sky project，發表的時候很寒酸，沒幾個人知道，後來Google在這半年才開始用心推廣它。

為什麼說Flutter是一個新世代的產物呢？因為他有幾個地方跟之前的Solution完全不一樣。首先，他拋棄了JacaScript。這些年來比較著名的跨平台Solution幾乎都圍繞著JavaScript打轉，為的就是想取用這個資源豐沛的生態圈，但是Flutter卻沒有這麼做，而是選擇了一個相對冷門的語言－Dart。關於Flutter選擇Dart的理由，這篇文章中做了相當詳細的闡述，這邊就不再贅述。拋棄JavaScript這件事有好有壞，壞處就是捨棄了現在世界上前幾名熱門且活躍的生態圈資源，對於初期開發者而言會需要自己打造許多輪子。但選擇Dart的好處看起來也是很巨大的，我們先撇除語言本身的語法特性不談，Dart具有可以同時做JIT以及AOT編譯的特性，這讓開發者可以在開發的時候享受到JIT編譯的快捷靈活，在真正發佈的時候用AOT編譯來大幅提升效能。Dart能做AOT編譯這項特性讓Flutter在很根本的層面上解決了ReactNative或是NativeScript在效能上的瓶頸，也就是較長的啟動時間，以及跟native platform溝通時，runtime JS bridge因為context switch帶來的效能損失。

Flutter還有另一個很重要的特色，就是它自帶了一個渲染引擎Skia。Skia是一個Google買下後開源的渲染引擎，廣泛地用於各種地方，包括Chrome、FireFox、Android等等。Flutter裡面所有的UI元件都是經由Skia繪製，跟platform上面的UI Component毫無瓜葛，也就是說Flutter從真正意義上實現了UI的跨平台使用。聽起來超酷的，不過這件事其實有好有壞，好處是你不必像ReactNative一樣硬去把兩個不同平台的UI Component做出同一個抽象層，Flutter所有UI元件在不同平台上的spec會完全相同，而且原生渲染可以帶來非常高的效能。壞處就是你拋棄了原有的、發展多年的平台組件，而選擇自己重新打造一組。

以現在的狀況來看，Flutter的想法很明確而激進。在必需有所取捨時，Flutter選擇追求架構上的優美以及高效能，而放棄了取用現有的豐富生態圈。這種做法我覺得野心很大，也挺危險的。就像我在圖片下面的comment一樣，好的東西不一定會紅，這在資訊界已經有太多前例了。Flutter不可能一輩子靠Google support，在這麼大刀闊斧的汰舊換新之下，該如何吸引一般開發者來投入這個生態圈會是最重要的課題。

總結來說，在Fuchsia還沒正式發佈的現在，Flutter作為一個單純的跨平台UI solution並不是相當的成熟。他的優點是架構上帶來不同層次的高效能，並且兼顧良好的開發體驗；缺點則是生態圈的匱乏，以及沒有Google Adwords以外的大型showcase。到底Flutter的未來會如何呢？讓我們繼續看下去。

# 結語
我不相信有人會從頭到尾看完這篇，你一定是直接滑到底對吧（笑）。我打到一半時就在想了，為什麼我不分幾篇打呢？為什麼要這樣折磨自己又折磨別人呢？為什麼我要打這篇落落長又沒什麼人在乎的文章呢？為什麼沒有人愛我呢？為什麼我要活在這世上呢…

![这篇文章的精华](https://blog-1256223865.cos.ap-shanghai.myqcloud.com/blog/platform-cors/ruhecoding.jpeg)
雖然我打了很多，不過還沒有結束。下一篇會寫一些比較輕鬆的話題，關於跨平台開發的通論與想法。

