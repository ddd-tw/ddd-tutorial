# 上下文映射圖 ( Context Maps )
Context Map 是屬於在 Domain-Driven Design ( 以下簡稱 DDD ) 中的戰略建模 ( Strategy Modelig ) 的一部分，雖然不管在 DDD 之父 Eric Evans 所寫的 [Domain-Driven Design: Tackling Complexity in the Heart of Software](https://www.oreilly.com/library/view/domain-driven-design-tackling/0321125215/)，還是對於實戰落地非常有名的這本由 Vaughn Vernon 所寫的 [Implementing Domain Driven Design](https://www.oreilly.com/library/view/implementing-domain-driven-design/9780133039900/) 都只有薄薄的內容，但這個 Map 卻在扮演了很常重要的角色。

不過在介紹 Domain Driven Design 中的 Context Map 之前，我們會需要先稍稍回顧一下 Domain 與 Bounded Context 並透過 Bounded Context 來點出為何我們需要 Context Map 以及他的重要性。

## 回顧一下領域 ( Domain ) 以及限界上下文 ( Bounded Context )
在 Bounded Context 中提到，當我們面對到業務需求時，往往要把這些抽且描述性的業務需求轉化成程式碼，過程中是非常不容易的，而且因為這些業務需求往往都是 Case By Case，每個業務需求都有自己 Buniess Know How，所以即便我們知曉了許多編碼的技巧、Pattern、架構風格，但是往往要把需求轉化並對應相容進去不是一件簡單事，甚至會出現很多疑問，並聽到過有一些討論像是：「我的 Order 到底要放在哪裡？」、「雖然知道要高內聚、低耦合，但是我這個例子中的類別，到底有想些要解偶，哪些要內聚？」...等等，也就會出現像是下面這張圖的情狀：

<p align="center">
  <img src="../context-maps/images/when-architecure-meet-business-logic.png?raw=true" width="480px">
</p>
<p align="center"><a href="https://www.monkeyuser.com/2018/architecture/">Architecure - From MonkeyUser.com</a></p>

更有可能在工作或是團隊合作時，聽到每個人對於這些業務描述都知道，但是彼此的用語跟思想不一定相同，造成很多溝通甚至開發協作上的困難。

所以在 DDD 之中 Bounded Context 便是一個用來幫助我們把業務需求到程式碼這過程中，拉近的一個重要概念與思想。我們會與領域專家 ( Domain Expert ) 針對這些業務需求解析，並找出、定義出哪些是實際上與業務需求相關的需要解決的問題，我們稱為**問題空間 (Problem Space)**。

*補充一下：這些領域專家可能直接是需求提供方（如客戶）、團隊成員或企業組織內部對該場景的專業領域最熟悉的人。*

這個過程中會因為討論與釐清需求，團隊會提問而領域專家會開始解說，於是便會開始制定、歸納出一些團隊都能理解的詞彙，也就是一套團隊通用且能理解的**通用語言 ( Ubiquitous Language )**，而這些詞彙需要讓團隊有共識與相同認知，同時在往後的討論或進入到實作開發中都以這些詞彙作用語，甚至在系統的建模中也會需要實踐使用。

<p align="center">
  <img src="../context-maps/images/ubiquitous-language.png?raw=true" width="480px">
</p>
<p align="center"><a href="https://www.infoq.com/articles/ddd-contextmapping/">Ubiquitous Language - Strategic Domain Driven Design with Context Mapping</a></p>

有了問題空間之後，接著對這個空間中的業務需求區分出各業務子領域，找出什麼是與你產品最有價值或是可以為產品帶來收益的部分與區域 - **核心子領域（ Core Sub-Domain )**、協助你支撐這個核心子領域所需要的其他服務 - **支撐子領域 ( Support Sub-Domain )**，以及可以抽離你產品不需要自己開發，從外部購買就有的基礎功能服務 - **通用子領域 ( Generic Sub-Domain )**。

<p align="center">
  <img src="../context-maps/images/problem-space.png?raw=true" width="480px">
</p>
<p align="center">DDD patterns that are applicable to the problem space.<br/>From Patterns, Principles, and Practices of Domain-Driven Design</p>

再次藉著**通用語言 ( Ubiquitous Language )** 與團隊反覆討論與提煉，開始明確劃分出這些上述子領域的邊界，找出可以對應建模系統的**解決空間 ( Solution Space )**，也就是我們所需要的**限界上下文（ Bounded Context ）**

<p align="center">
  <img src="../context-maps/images/solution-space.png?raw=true" width="480px">
</p>
<p align="center">DDD patterns that are applicable to the solution space.<br/>From Patterns, Principles, and Practices of Domain-Driven Design</p>

於是這些有著通用語言在內的邊界區域便是 Bounded Context，每個 Bounded Context 都會有著自己的通用語言以及自己的邊界。在往後的實作建模中，這些邊界也會是與各個其他系統的邊界，其他的系統也會有自己的 Bounded Context。

如下圖是一個團隊協作專用的敏捷專案管理系統，該系統的業務場景 Bounded Context 中，因為敏捷專案管理是該產品的核心價值也是賣點，便是歸類 Core Sub-Domain；而團隊協作是支撐敏捷專案功能區塊與業務場景，所以是 Support Sub-Domain，但未來不排除可能會成為 Core Sub-Domain；但身份識別與角色權限就屬於相對基礎的建設服務與一般任何系統都會有的場景，所以歸類在通用子領域，也不排除可以使用外部服務，如整合 SSO 服務商。

<p align="center">
  <img src="../context-maps/images/domain-and-bounded-context.png?raw=true" width="480px">
</p>
<p align="center">The Subdomains and Bounded Contexts<br/>From Implementing Domain-Driven Design</p>

該案例中每個 Domain 都自成一個 Bounded Context，但現實中不一定會如此，在理想上一個 Domain 可以的話對應到一個 Bounded Context。

另外，不同的 Bounded Context 也可能會有相同的用詞，例如下圖雖然是不同的 Bounded Context，但彼此對於 Account 所表達的含義卻不同，在 Literary Context 中的 Account 所表達的是指可以留言發文的帳號，而在 Banking Context 中的 Account 卻是指銀行中的帳戶，具有存款、提款、轉帳等行為的帳號。

<p align="center">
  <img src="../context-maps/images/bounded-context-with-same-term-but-different-meaning.png?raw=true" width="480px">
</p>
<p align="center">Account objects in two different Bounded Contexts<br/>From Implementing Domain-Driven Design</p>

而對於不同的 Bounded Context 所構成的邊界，當彼此溝通整合時，會需要翻譯彼此不同的 Context 內的語意，如下圖的例子，你會看到 Sales Context 與 Support Context 兩個 Bounded Context 都有各自 Customer 與 Product，但是雙方關注的點和業務細節不同，因此含義上不相同，因此在銜接溝通交換資料時，雙方就會有需要做一層翻譯。

<p align="center">
  <img src="../context-maps/images/different-bounded-context.png?raw=true" width="480px">
</p>
<p align="center"><a href="https://martinfowler.com/bliki/BoundedContext.html">Bounded Context - From Martin Fowler</a></p>

## 為何領域 ( Domain ) 以及限界上下文 ( Bounded Context ) 仍不足

藉由領域 ( Domain ) 以及限界上下文 ( Bounded Context ) 雖能為我們定義業務需求，找到這個業務與系統建模的邊界，然而往往現實中中並非如此的單純，你可能會面臨一些情境導致你難以進行分析、或分析的結果是事實有落差：

### 1. 越複雜的業務情境，越不清晰的 Bounded Context
當你的業務需求與及情境非常龐大複雜且時。於是伴隨著尋找，往往會發現有著許許多多的 Bounded Context，且這個過程中卻非常容易陷入聚焦於一個點，而難以見數又見林，往往的當你完成了一些 Bounded Context 時，卻在下幾個 Bounded Context 的討論中發現前幾個 Bounded Context 的邊界不太正確，於是你會改了又改調了又調，最後你可能會迷失在分析中：

<p align="center">
  <img src="../context-maps/images/bounded-context-complexity.png?raw=true" width="480px">
</p>
<p align="center">Business scenario complexity - From Internet</p>

### 2. 組織的文化與企業內部的運作模式會影響 Bounded Context 真實性
這句是什麼意思呢？其實當團隊面臨業務情境與需求時，往往也會找出的許多 Bounded Contexts，但這確不太可能會由一個團隊全部完成，而會交由其他的團隊來開發，這也是在企業中常見的行為。

<p align="center">
  <img src="../context-maps/images/bounded-context-with-different-teams.png?raw=true" width="480px">
</p>
<p align="center"><a href="https://www.infoq.com/articles/ddd-contextmapping/">Different teams with different bounded contexts - Strategic Domain Driven Design with Context Mapping</a></p>

在這個時候團隊間的合作關係模式往往會因為組織的文化或是組織內對團隊間的評價地位，間接影響到跨團隊的協作溝通與開發進展，而這些在尋找 Bounded Context 時也往往難以看見。而這些現象在 DDD 中的戰略設計都是不可忽略的。

時常會發生團隊、部門間往往拿到自己的需求後便會專注在實現上，而忽視了與其他團隊之間的協作，於是進展到一半時才發生因為雙方的關係與對業務需求的銜接認知不一致，進而影響最後交付或是需求的正確性。

或是當組織內部間部門或團隊彼此是互相競爭時，在開發系統與協作勢必會更加困難，而全聽命某一個主管時，往往會增加開發協作上的時程，而這些在分析 Bounded Context 並不會特別意識到。

<p align="center">
  <img src="../context-maps/images/bounded-context-orgnization-complexity.png?raw=true" width="480px">
</p>
<p align="center">Orgnization culture - From internet</p>

### 3. 組織龐大過於複雜、龐大的開發團隊共同開發
承第二點，所以越龐大的組織，或龐大的開發團隊，會對於 Bounded Contexts 共同協作開發上也增加困難度。著名的[康威定律 (Conway's Law)](https://zh.wikipedia.org/wiki/%E5%BA%B7%E5%A8%81%E5%AE%9A%E5%BE%8B)中也提過組織的運作與複雜度往往與系統架構或系統的設計有著關聯性，因此這些現象也是需要與多個團隊共同協作多個 Bounded Contexts 需要考量進去的。

<p align="center">
  <img src="../context-maps/images/bounded-context-huge-organization.png?raw=true" width="480px">
</p>
<p align="center">Huge company and teams - From internet</p>

除了組織的複雜性與文化外，團隊間的協作如果是地理位置上的分散式的團隊協作，或是團隊與團隊間採用遠端模式協作也都會影響。

<p align="center">
  <img src="../context-maps/images/bounded-context-distributed-team.png?raw=true" width="480px">
</p>
<p align="center">Distributed teams - From internet</p>

### 4. 外部系統對 Bounded Context 的影響性
上述是設計與分析 Bounded Context 時難以思考到的組織團隊與文化協作影響，再來讓我們回到系統面上，由於分析業務情境需求的最後目的是要建模成系統完成開發，那麼在這些過程中，可能免不了會有一些考量或遇到的現象是，組織中是否有一些已經在使用，從外部購買來的系統有要整合進這次的業務場景裡？ 
例如對於銷售管理系統來說，非常有名的 Salesforce 是許多企業中會提供給業務人員使用與管理的系統，此時如果新的需求是要開發一套自己的，是否有可能要銜接或是整合一部分使用。

<p align="center">
  <img src="../context-maps/images/bounded-context-external-system.png?raw=true" width="480px">
</p>
<p align="center">Connect external system - From Internet</p>

這也是需要在做戰略設計時應該考量的面向，然而只專注在 Bounded Contexts 的分析時往往不一定會去考量這件事。

### 5. 可怕的 Legacy 系統
每間企業特別是歷史越悠久的企業，如銀行，都會有一些幾十年前的系統仍在使用，這些系統的因為經歷了許多歲月的調動或開發人員的替換，可能沒有人有辦法完全理解這個 Legacy 系統裡面到底有什麼，到底整合了多少其他的系統。而當開發人員去翻開這些系統的代碼時，會發現因為經歷了歷代工程師的調動，往往有著不成人形的模樣，該系統可能也找不到文件、不曉得去能詢問誰、或是因為早期的系統沒有經過測試保護，可能改了一行就整個 Crash ...

（往往的我們都會戲稱去挑戰或是要去重構 Legacy System 的人就像在屠龍一樣）

<p align="center">
  <img src="../context-maps/images/bounded-context-legacy-system.png?raw=true" width="480px">
</p>
<p align="center">Legacy system - From Internet</p>

而這就像是前面的外部系統一樣，如果今天新的需求要整合 Legacy 系統時，我們是不是有考量這個 Legacy Sytem 與 Bounded Context 的邊界，還有如果要對接等等的問題。

因此，雖然透過 Domain 與 Bounded Context 已經能夠為我們初步從抽象的業務場景描述找到了建模的邊界，但是在戰略設計中仍不夠，因為業務場景可能很龐大，而且協作的過程中會有著團隊文化、組織協作等影響，以及是否有使用外部的系統和 Legacy System 都是會影響的。

所以在 DDD 中還需要透過 Context Maps 幫上述可能會遇到的情況透過在設計這個 Map 時幫助我們找出潛在的因素。

## 所以什麼是 Context Maps 呢
讓我們先來看看維基百科的 Context Maps 定義：

> An individual bounded context leaves some problems in the absence of a global view. The context of other models may still be vague and in flux.
> 
> People on other teams won’t be very aware of the context bounds and will unknowingly make changes that blur the edges or complicate the interconnections. When connections must be made between different contexts, they tend to bleed into each other.
> 
> Therefore: Identify each model in play on the project and define its bounded context. This includes the implicit models of non-object-oriented subsystems. Name each bounded context, and make the names part of the ubiquitous language. Describe the points of contact between the models, outlining explicit translation for any communication and highlighting any sharing. Map the existing terrain.
 
如同維基百科說的他為我們點出了先前提到的各個原因，而 Context Maps 能為我們扮演銜接 Bounded Context 的橋樑，把視野看得更全面。

Context Maps 為我們找出 Bounded Context 之間的協作關係，或者可以更一步地說「反映出組織、團隊或者與合作夥伴之間的在協作上或系統銜接整合上的關係」，透過 Context Maps 我們能夠把原本存在的隱藏成本與瓶頸凸顯出來，並進一步的從溝通面和系統協作面起到保護的作用，例如事先找出會對連接串接整合的區塊，設計出保護層或定義不同之間統一通用語言的翻譯層。

<p align="center">
  <img src="../context-maps/images/Context-map-translation-map.png?raw=true" width="480px">
</p>
<p align="center">Context Maps translation mapping<br/>From Patterns, Principles, and Practices of Domain-Driven Design</p>

不過 Context Maps 並不是一種企業架構，也不是系統拓墣圖。我們可以這麼定位它 — *Context Maps 是一種高層次抽象的架構分析，能指出整合上的瓶頸、體現出組織的動態，並幫助我們識別出有礙項目進展的管理問題。*

### 初步繪製的 Context Maps

而一個初步簡易的 Context Maps 會如下圖，在圖中你會看見 Context Maps 彼此之間透過線條連起來，並且標明了 `U` 跟 `D`，這個 `U` 跟 `D` 分別代表了 **上游 ( Upstream )** 與 **下游 ( Downstream )**，也就是我們可以透過上游/下游來表達出各個 Bounded Context 之間的初步關係，這些關係也反映出了團隊以及業務場景之間的協作關係，當然還有建模之後在系統面上互動的關係。

<p align="center">
  <img src="../context-maps/images/context-maps-upstream-downstream.png?raw=true" width="480px">
</p>
<p align="center">Simple context maps<br/>From Implementing Domain-Driven Design</p>

所謂的 **上游 ( Upstream )** 表示的是這個 Bounded Context 與會對於其他的 Bounded Context 而言在業務場景上是一個供應方，需要仰賴這個業務場景所設計出的系統來運作服務的某部分；而在系統層面上可能是一個 API 開放方；若從團隊協作上思考，該上游團隊可能是需要最優先提供服務的團隊，但相對的其他下游團隊可能都需要配合該上游團隊。

而所謂的 **下游 ( Downstream )** 表示這個 Bounded Context 與會對於其他的 Bounded Context 在業務場景上是一個需求方，他需要仰賴其他的上游 Bounded Context 來使自己的業務場景與服務運作正常，也可能伴隨著上游出了狀況，下游也出了狀況；如果從系統面來想，他就是一個需要請求外部服、API 的系統；若從團隊協作上思考，該下游團隊可能需要時常配合上游團隊，也可能受上游團隊的協作與溝通影響自身開發與交付的時程。

目前這個標示上下游的 Context Maps 方式雖然只是一個初步的畫法，但是你會發現只是短短的繪製出這些圖示，便能進一步讓團隊與領域專家在思考與討論業務需求時，可以看到更多的面向，發現更多潛在的隱藏協作細節與成本，或是釐清可能有誤的解讀，同時可以提少準備預防可能的問題發生。

## 繪製 Context Maps 的一些小細節
此外，在繪製 Context Maps 時有一些需要注意的小地方與一些 Tips，透過這些細節與建議，除了能幫助團隊在繪製 Context Maps 時有正確的觀念外，也能更加完善。

### 1. 只需捕捉「當下」，再漸進式的隨變化改良 Context Maps 
在繪製 Context Maps 時有一些需要注意的地方，Context Maps 是為我們捕捉「當下」與領域專家討論分析的樣貌、現在與合作夥伴的關係、與團隊的關係，所以是目前的狀態。因此 Context Maps 是需要不斷的更新，伴隨著進一步的進入設計、項目需求變更或是添加需求項目、其他 Bounded Context 的協作團隊異動、或是有了新的領域專家協助等等，都有可能改變原先的思維與設計。

### 2. 可以適當的加入更多訊息使 Context Maps 更全面完善
除了在 Context Maps 中繪製 Bounded Contexts 的邊界、上下游關係以及需要銜接翻譯的部分外，也可以把一些特定的訊息放入進來，例如我們希望讓 Context Maps 中各個 Bounded Context 的內部更加清晰一些，可以放入後續做戰術建模 (Tactical Modeling) 時獲得的有用細節訊息（如 Aggregate (聚合)、系統的 Module (模組) 設計圖），或是放入不同 Bounded Contexts 負責的團隊資訊（如位置資訊，溝通窗口、需要的內對接業務的資訊）。

這過這些方面的資訊，能為我們從不同的角度來思考，並且這個更新後的 Context Maps 也能協助我們捕捉更明確業務場景。

但是這裡也要注意拿捏資訊是否是必要的，以及是否太過細緻、繁瑣或不重要的細節，因為 Context Maps 主要是用來幫助我們交流，已全面且抽象的角度看待業務場景、整合的瓶頸、與組織間的相互關係，所以只需要放入對交流已有價值的資訊即可。

### 3. 提供團隊之間能共同瀏覽 Context Maps 的位置
在討論、繪製或更新 Context Maps 的過程中，可以各次的訊息與細節都共同放在相同的位置保存或提供團隊觀看，你可以選擇採用文件化、並把討論中所用到的素材整理擺放到雲端保存，如 Wiki，而這樣的形式非常適合地理位置分散或遠端的團隊模式。

不過若團隊能聚在一起的話，更鼓勵採用如看板方法、User Story Mapping 方式把 Context Maps 與其細節訊息用文字、便利貼的形式共同貼在白板或牆上，方便團隊隨時能共觀看討論，也能避免沈入系統的文件大海中，找詢問文件的步驟越多，總容易使人懶得去翻開。

## 挖掘 Context Maps 中更細緻的關係
在介紹 Context Maps 以及繪製 Context Maps 時，我們提到了上游 `U` 與下游 `D` 能夠為描述並發現每個 Bounded Contexts 彼此之間的關係，這些關係反映出了團隊以及業務場景之間的協作關係，還有建模之後在系統面上互動的關係。

然而這個上下游只是初步的 Context Maps ，就像我們在真實生活中的關係一樣其實可能是非常的複雜的，即便是上游與下游也可能包含了不同程度的關係。

<p align="center">
  <img src="../context-maps/images/realworld-relationship.png?raw=true" width="480px">
</p>
<p align="center">Real World Relationship - From Internet</p>

而這些關係也是可以透過繪製 Context Maps 表達呈現出來的，在 DDD 中把常見的幾種 Context Maps 關係稱作關係模式 ( Relationship Patterns )，這些關係模式包含了組織協作關係以及系統整合關係，以下我們分別介紹。

### 1. 組織協關係模式 - 夥伴關係 ( Partnership )
當兩個團隊彼此負責不同的 Bounded Contexts 與各自的業務場景，但在會彼此業務相連交互的場景需求上願意一起互相討論溝通協助，一起朝相同的交付目標前進，並制訂出一個雙方可行的開發方案與系統整合管理，這個現場就會稱做夥伴關係 ( Partnership )。因為雙方是平等關係、相同的地位，因此並**沒有區分上游與下游的關係**。

<p align="center">
  <img src="../context-maps/images/context-maps-partnership.png?raw=true" width="320px">
</p>
<p align="center">Partnership - From Internet</p>

夥伴關係 ( Partnership )的合作可以涵蓋技術介面、API 接口或共同使用的功能，同時需要協調雙方的部署、測試的時機點，以及會影響的範圍，以便滿足兩個團隊的利益與目標。

### 2. 組織關係模式 - 客戶與供應方 ( Customer － Supplier )
當負責不同 Bounded Contexts 的團隊之間屬於上游 `U` 與下游 `D`，且上游的 Bounded Context 團隊在與下游的 Bounded Context 團隊合作時，下游團隊可以對上游團隊提出業務合作的需要，但最終的決定仍會由上游開出並制定規格時，此上下游模式便是客戶與供應方 ( Customer － Supplier )。

這時的上游方便被稱作供應方 ( Supplier ) ，而下游方稱作 ( Customer )。在這個情形下，下游團隊需要試著與上游團隊溝通與協調來共同制定計劃與規格，使上游提供下游合適的端點或接口規格，並嘗試把雙方的合作模式轉換成夥伴關係 ( Partnership )，來避免上游團隊開出下游團隊不期望的需求規格。

<p align="center">
  <img src="../context-maps/images/context-maps-customer-supplier.png?raw=true" width="640px">
</p>
<p align="center">Customer － Supplier<br/>From Patterns, Principles, and Practices of Domain-Driven Design</p>


### 3. 組織關係模式 - 尊奉者 ( Conformist )
一樣的兩個有著彼此各自的 Bounded Contexts 團隊，並且是以上游 `U` 與 下游 `D` 的關係存在，但上游沒有任何動機與理由去需要滿足下游並提供下游需要的規格與合作方式，或是上游已經無法再做出任何改變，在這種情形下，下游只能遵從上游的規範與規則。

會發生此現象通常是上游為一些外部無法干涉到的系統，或是老舊的 Legacy 系統。

<p align="center">
  <img src="../context-maps/images/context-maps-conformist.png?raw=true" width="360px">
</p>
<p align="center">Conformist - From Internet</p>

### 1. 整合關係模式 - 共享內核 ( Shared Kernel )
當不同的團隊在同一個系統的應用程式中有彼此各自的 Bounded Context 與業務場景，但卻在程式中有許多的領域概念和邏輯有互相交集時，甚至有相同的模型 Model 需要共用便會稱作共享內核 ( Shared Kernel )。

如此下圖顯示 ERP 系統包含共享 Employee Model 的工資表 ( Payroll ) Context 與人力資源 ( Human Resource ) Context，便是共享內核 ( Shared Kernel )。

<p align="center">
  <img src="../context-maps/images/context-maps-sharedkernel.png?raw=true" width="480px">
</p>
<p align="center">Shared Kernel<br/>From Patterns, Principles, and Practices of Domain-Driven Design</p>

在共享內核 ( Shared Kernel ) 的模式下會需要共享一部分模型 Models 來指定一個顯示的邊界，並且在沒有和其他團隊協調的情況下，是不能改變其共享內核 ( Shared Kernel ) 模型與程式碼，因為隨時有可能造成另一方出錯，進而使整個系統 Crash，並且部署、測試與更新都可能會影響另一方。

因此這樣的模型和程式碼的共享將產生一種緊密的依賴關係，不一定比較好。那你可能會問這樣什麼時候適合使用共享內核呢？ 如果在做戰略設計時發現同一個子域 ( Sub-Domain ) 中有兩個 Bounded Contexts 時，且初期業務場景仍不明確，可以先採用此種模式來開發。

但當團隊在兩個 Bounded Contexts 之間採用共享內核 ( Shared Kernel ) 後，則會建議團隊以夥伴關係 ( Partnership ) 來協作與執行，讓彼此有好的合作與目標，未來若發現業務明確且複雜時，可以再以共享內核的模型邊界拆分出去。

### 2. 整合關係模式 - 各行其道 ( Separate Way )
如果由於技術複雜性或組織的團隊合作複雜性導致 Bounded Contexts 之間的整合的成本太高，或如果確定兩套系統之間沒有顯著的關係或是需求時，則可以做出決定，兩個 Bounded Contexts 之間不做整合並完全解耦。

讓雙方的團隊可以聚焦在自己的 Bounded Context 與業務場景與功能，並尋找其他方法解決問題，例如各自在自己的 Bounded Context 建立屬於自己的特殊解決方案。這樣的情形在 Context Maps 中被稱作各行其道 ( Separate Way )。


<p align="center">
  <img src="../context-maps/images/context-maps-separate-way.png?raw=true" width="360px">
</p>
<p align="center">Separate way - From Internet</p>


### 3. 整合關係模式 - 大泥球 ( Big Ball of Mud )
大泥球 ( Big Ball of Mud )，簡稱 BBoM 是指一個單體系統 ( Monolithic System ) 中混雜了非常多邊界模糊的 Model，並且有許多業務邊界可能在該單體系統裡重疊，難以看清業務需求與功能的全貌，甚至難以拆解。

<p align="center">
  <img src="../context-maps/images/context-maps-bbom.png?raw=true" width="480px">
</p>
<p align="center">Big Ball of Mud - From Internet</p>


另外這樣 BBoM 情形非常容易發生在老舊且許多時間沒有在維護，經歷過許多代的功能添加，且沒有人能夠裡清楚內部業務場景或全部功能的 Legacy 系統。

要處理大泥球的現象需要透過測試保護、重構不斷的漸漸調整與拆解，並去逐漸理解找其中的領域關係與業務場景，找出 Bounded Contexts，但是切記不要一次就想要解決所有的問題，雃是要一小塊一小塊的處理，從一個 Bounded Context 再到下一個 Bounded Context，並為各個系統繪製邊界。

<p align="center">
  <img src="../context-maps/images/context-maps-eat-an-elephant.png?raw=true" width="480px">
</p>
<p align="center">Eat an Elephant - From InfoQ : Rethinking Legacy and Monolithic Systems</p>


另外非常推薦觀看 InfoQ 上一篇 iDDD 作者 Vaughn Vernon 分享的 [Rethinking Legacy and Monolithic Systems](https://www.infoq.com/presentations/monolith-legacy-rethinking/) 內容，會一步步的告知你如何處理 BBoM。


### 4. 整合關係模式 - 防腐層 ( Anticorruption Layer )
防腐層 ( Anticorruption Layer ) 縮寫 ACL 是一種隔離下游 Bounded Context 避免因上游 Bounded Context 對外規格的更改而連帶造成己方的內部業務功能影響的保護層，同時也作為一個翻譯層，把上游的 Bounded Context 通用語言轉換成下游 Bounded Context 的通用語言。

防腐層 ( Anticorruption Layer ) 通常發生在 「共享內核」、「夥伴關係」 或 「客戶 - 供應方」 無法順利的實現運作而導致邊界與邊界間語言不一致或容易因變動造成影響時（ 或是像發生遵奉者的案例 ）所提供給下游 Bounded Context 處理方式。

使用防腐層 ACL ，下游的 Bounded Context 團隊需要根據自己的領域模型建立一個翻譯接口層與請求取得資料集的模型，委託這個翻譯接口層作為代理，來與上游層的外部 Bounded Context 系統互動。讓這個防腐層接口與外部系統交互，便可以使得內部變動與修改降到很低，也不會弄髒內部的領域模型。

<p align="center">
  <img src="../context-maps/images/context-maps-acl.png?raw=true" width="480px">
</p>
<p align="center">Anticorruption Layer ( ACL )<br/>From Patterns, Principles, and Practices of Domain-Driven Design</p>


### 5. 整合關係模式 - 開放主機服務 ( Open Host Service ) 與 發佈語言 ( Published Language )
開放主機服務 ( Open Host Service ) 縮寫 OHS，會定義一套協議或接口，讓 Bounded Context 可以被當作一組服務被訪問，這個協議是公開的，使所有下游的 Bounded Context 或外部的系統可以進行整合並使用。並且需要更動時，可以在該現有的協議接口上調整或擴展更新，並更新相對對應的文件訊息讓外部系統，與下游方可以也跟著做調整。

<p align="center">
  <img src="../context-maps/images/context-maps-ohs.png?raw=true" width="480px">
</p>
<p align="center">Open Host Service ( OHS )<br/>From Patterns, Principles, and Practices of Domain-Driven Design</p>

發佈語言 ( Published Language ) 縮寫 PL，作為 Bounded Contexts 之間作為翻譯模型所用的公共語言，當有任何下游或外部系統要使用時，可以讀取此發佈語言的內容並自行轉換成內部使用的通用語言，因此使用該發佈出來的共享語言可以協助完成系統的整合。

OHS 與 PL 通常是上游 Bounded Context 或開放方所使用的整合模式，且 OHS 與 PL 通常會一起搭配使用，使得下游 Bounded Context 能輕易與上游方互動，但為了避免未來上游方的規範更動而影響下游方，所以下游方會使用 ACL 保護自身。

<br/>
## 一起來試著練習繪製 Context Maps
坐而言不如起而行，輪到你來試著練習繪製 Context Maps 了，這是一個保險公司的產品服務，在這個服務的業務場景中，有著六個 Bounded Context 需要分別完成，以下分別簡述：

1. **Customer Management（ 客戶管理 Context )**
此 Bounded Context 負責管理保險公司客戶 (Customers) 的所有數據。因此它通常是一個核心的 Bounded Context ，許多其他的 Bounded Context 會與它有關聯互動。

2. **Customer Self-Service Management ( 客戶自助服務管理 Context )**
此 Bounded Context 為一個 Web 的應用程序，該 Context 允許客戶登錄和更改基本客戶數據記錄，如地址。

3. **Policy Management ( 保單管理 Context )**
此 Bounded Context 管理客戶的合約 (Contracts) 和保單 (Polices)。它與 Risk Management Context 一起合作，因為它需要客戶的風險相關數據來計算客戶的保單費率。此外，它有一個與 Debt Collection Context 共享的內核。

4. **Debt Collection ( 收帳 Context )**
此 Bounded Context 依據對應的合約  (Contracts) 和保單 (Polices) 數據來收取帳務，以負責保險公司的財務收入。 

5. **Risk Management ( 風險管理 Context )**
此 Bounded Context 與 Policy Management Context 密切相關並且會計算影響合約 (Contracts) 與保單 (Polices) 的風險因素。

6. **Printing Context ( 影印 Context )**
此 Bounded Context 為一個提供給內部其他 Bounded Context 由 API 接口訪問的外部系統。它處理必須影印文件的相關行為，例如影印帳務 (Debit)，保險單 (Polices) 和客戶 (Customers) 的數據資料等。

那麼，現在就來動手繪製看看吧！試著在 30 分鐘內思考或是跟團隊、朋友討論看看，試著理解這個業務場境。另外，這並沒有標準答案，因為每個團隊都有著自己對領域的解讀，也會因為不同的組織與團隊組成，因此其他的 Bounded Context 的關係也會不同。

不過，如果想要得知該練習的範例結果，可以點擊這個 [連結](https://github.com/ContextMapper/context-mapper-examples/tree/master/src/main/resources/insurance-example) 來參考看看。

<br/>
## 更近一步的認識 Context Maps 的實現面
在看完例子與小試身手練習後，接下來我們將要更進一步，針對最常被使用到的防腐層 ( ACL ) 以及開放主機服務 ( OHS ) 與 發佈語言 ( PL ) 來介紹一下在實現上使次用什麼技術實現。

### 實現防腐層 ( ACL ) 的方式

在技術實現上，會為每個防腐層定義各自的領域服務（ Domain Service ）或資源庫（ Repository ) 接口來執行，並委託透過 [Adapter](https://en.wikipedia.org/wiki/Adapter_pattern) 建立接口翻譯層，把來自上游的資料集以 Data Transfer Object (DTO) 方式封裝後，在 Adapter 內翻譯成下游己方 Bounded Context 內的領域模型。

如下圖是 Implementing Domain-Driven Design 書中的例子，你會看到這裡有兩個 Domain Service ，並且透過這個 Domain Service 去委託 Adapter 處理與外部的 Collaboration Bounded Context 調用資源回來。

<p align="center">
  <img src="../context-maps/images/context-maps-acl-implementation.png?raw=true" width="480px">
</p>
<p align="center">The Domain Service and Adapter Class Relationship of Anticorruption Layer<br/>Implementing Domain-Driven Design</p>

### 實現開放主機服務 ( OHS ) 與 發佈語言 ( PL ) 的方式
由於 OHS 與 PL 是一體兩面的，所以我們會放在一起討論。在實現上可能有些人已經從文中猜到 OHS 因為提及協議與 API 接口，所以最能讓我們最先聯想到的，便是現在很常使用的 Restful HTTP 協議，而在 Restful HTTP 的協議中會常被使用 PL 便是 Json Schema。

<p align="center">
  <img src="../context-maps/images/context-maps-restful-http.png?raw=true" width="480px">
</p>
<p align="center">Restful HTTP<br/>From Domain-Driven Design Distilled</p>


除此之外也可以透過 RPC 遠程調用方式來作為協議接口，如基於 SOAP 協議的 RPC 方式並搭配 XML Schema 的發佈語言作為 Bounded Contexts 系統間的互動，或是透過 Message 機制交換資料。

<p align="center">
  <img src="../context-maps/images/context-maps-rpc-soap-xml.png?raw=true" width="480px">
</p>
<p align="center">RPC - SOAP/XML <br/>From Domain-Driven Design Distilled</p>

甚至最土法煉鋼的檔案、資料庫也是屬於在 OHS/PL，這通常也發生在早期的大型系統中採用的形式，如銀行內部會時常看見，當然以現在來說這也是最不推薦的一種模式。並且除了上述 OHS 搭配所提到的發布語言形式外，也可以採用 HTML 格式以用戶界面的形式呈現，或 Google 提出的 Protobuf 作為資料交換的格式。

<p align="center">
  <img src="../context-maps/images/publish-language-protobuf.png?raw=true" width="480px">
</p>
<p align="center"><a href="https://developers.google.com/protocol-buffers/docs/overview">Protobuf - From Google Developer Guide</a></p>

也可以透過使用事件驅動 ( Event-Driven ) 架構，在 DDD 中會藉由領域事件 ( Domain Event ) 發佈事件並以 Message Queue 來作為 OHS/PL，如下圖：

<p align="center">
  <img src="../context-maps/images/context-maps-event-message-queue.png?raw=true" width="480px">
</p>
<p align="center">Event-Driven with Message Queue<br/>From Domain-Driven Design Distilled</p>
 
## 小結
希望透過這篇介紹，能對 Context Maps 有更多的理解，Context Maps 看似抽象單純，但其實這個抽象中卻包含了許多的訊息可以拿來關注。另外 Context Maps 的組織關係模式和整合關係模式，也能幫助團隊在分析 Context Maps 有一個方向來參考。
如同上述提到的 Context Maps 是一種高層次抽象的架構分析，能指出整合上的瓶頸、體現出組織的動態，並幫助我們識別出有礙項目進展的管理問題。


## 參考資料
1. Eric Evans. (Aug 2003). Domain-Driven Design: Tackling Complexity in the Heart of Software, Addison-Wesley. https://www.oreilly.com/library/view/domain-driven-design-tackling/0321125215/
2. Vaughn Vernon. (Feb, 2013). Implementing Domain Driven Design, Addison-Wesley. https://www.oreilly.com/library/view/implementing-domain-driven-design/9780133039900/
3. Scott Millett & Nick Tune. (Apr 20, 2015). From Patterns, Principles, and Practices of Domain-Driven Design https://www.amazon.com/Patterns-Principles-Practices-Domain-Driven-Design/dp/1118714709
4. Martin Fowler. (Jan 15, 2014). "Bounded Context". https://martinfowler.com/bliki/BoundedContext.html
5. Domain-driven design. Wikipedia. https://en.wikipedia.org/wiki/Domain-driven_design
6. Vernon Vaughn. (Jun 16, 2016). “Ch. 4, Stragetic Design with Context Mapping.” Domain-Driven Design Distilled, Addison-Wesley.  https://www.amazon.com/Domain-Driven-Design-Distilled-Vaughn-Vernon/dp/0134434420
7. Alberto Brandolini. (Nov 25, 2009) Strategic Domain Driven Design with Context Mapping. InfoQ. https://www.infoq.com/articles/ddd-contextmapping/
8. Developer Guide. Protocol Buffers, Google. https://developers.google.com/protocol-buffers/docs/overview)