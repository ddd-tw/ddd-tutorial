# 上下文映射圖 ( Context Maps )
Context Map 是屬於在 Domain-Driven Design ( 以下簡稱 DDD ) 中的戰略建模 ( Strategy Modelig ) 的一部分，雖然不管在 DDD 之父 Eric Evans 所寫的 [Domain-Driven Design: Tackling Complexity in the Heart of Software](https://www.oreilly.com/library/view/domain-driven-design-tackling/0321125215/)，還是對於實戰落地非常有名的這本由 Vaughn Vernon 所寫的 [Implementing Domain Driven Design](https://www.oreilly.com/library/view/implementing-domain-driven-design/9780133039900/) 都只有薄薄的內容，但這個 Map 卻在扮演了很常重要的角色。

不過在介紹 Domain Driven Design 中的 Context Map 之前，我們會需要先稍稍回顧一下 Domain 與 Bounded Context 並透過 Bounded Context 來點出為何我們需要 Context Map 以及他的重要性。

## 回顧一下領域 ( Domain ) 以及限界上下文 ( Bounded Context )
在 Bounded Context 中提到，當我們面對到業務需求時，往往要把這些抽且描述性的業務需求轉化成程式碼，過程中是非常不容易的，而且因為這些業務需求往往都是 Case By Case，每個業務需求都有自己 Buniess Know How，所以即便我們知曉了許多編碼的技巧、Pattern、架構風格，但是往往要把需求轉化並對應相容進去不是一件簡單事，甚至會出現很多疑問，並聽到過有一些討論像是：「我的 Order 到底要放在哪裡？」、「雖然知道要高內聚、低耦合，但是我這個例子中的類別，到底有想些要解偶，哪些要內聚？」...等等，也就會出現像是下面這張圖的情狀：

<p align="center">
  <img src="../context-maps/images/when-architecure-meet-business-logic.png?raw=true" width="480px">
</p>
<p align="center"><a href="https://www.monkeyuser.com/2018/architecture/">From MonkeyUser.com - Architecure</a></p>

更有可能在工作或是團隊合作時，聽到每個人對於這些業務描述都知道，但是彼此的用語跟思想不一定相同，造成很多溝通甚至開發協作上的困難。

所以在 DDD 之中，Bounded Context 便是一個用來幫助我們把業務需求到程式碼這過程中，拉近的一個重要概念與思想。在 DDD 中我們會先針對這些業務需求解析，並定義出哪些是實際上與業務需求相關的問題空間 (Problem Space)，並接著區分出各業務子領域，什麼是與你產品最有價值的核心子領域（ Core Sub-Domain )、協助你支撐這個核心子領域所需要的其他服務 ( Support Sub-Domain )，以及可以抽離你產品不需要自己開發，從外部購買的功能服務 - 通用子領域 ( Generic Sub-Domain )。

再進一步找出 Bounded Context：透過藉由討論時訂出團隊通用且理解的統一語言用詞 ( Ubiquitous Language )，藉著這些用詞劃分出這些上述子領域的邊界，訂出解決空間 ( Solution Space )，於是這些 Bounded Context 都會有著自己的通用語言以及自己的邊界，並協助在往後的實作建模，而這些邊界也會是你往後各個系統的邊界與其他的 Bounded Context 會區分開來。

<p align="center">
  <img src="../context-maps/images/domain-and-bounded-context.png?raw=true" width="480px">
</p>
<p align="center">From iDDD - The Subdomains and Bounded Contexts.</p>

因此每一個 Bounded Context 會有自己的統一通用語言。而不同的 Bounded Context 也可能會有相同的用詞，例如下圖雖然是不同的 Bounded Context，但彼此對於 Account 所表達的含義卻不同，在 Literary Context 中的 Account 所表達的是指可以留言發文的帳號，而在 Banking Context 中的 Account 卻是指銀行中的帳戶，具有存款、提款、轉帳等行為的帳號。

<p align="center">
  <img src="../context-maps/images/bounded-context-with-same-term-but-different-meaning.png?raw=true" width="480px">
</p>
<p align="center">From iDDD - Account objects in two different Bounded Contexts.</p>

而對於不同的 Bounded Context 所構成的邊界，當彼此溝通整合時，會需要翻譯彼此不同的 Context 內的語意，如下圖你會看到 Sales Context 與 Support Context 兩個 Bounded Context 都有各自 Customer 與 Product，但是雙方關注的點以及含義不相同，因此在銜接溝通交換資料時，雙方就會有需要透過一方式翻譯，而這也是等下 Context Maps 惠介紹到的部分。

<p align="center">
  <img src="../context-maps/images/different-bounded-context.png?raw=true" width="480px">
</p>
<p align="center"><a href="https://martinfowler.com/bliki/BoundedContext.html">From Martin Fowler - Bounded Context</a></p>

## 只有領域 ( Domain ) 以及限界上下文 ( Bounded Context ) 仍不足夠理解

藉由領域 ( Domain ) 以及限界上下文 ( Bounded Context ) 雖能為我們定義業務需求，找到這個業務與系統建模的邊界，然而往往現實中中並非如此的單純，你可能會面臨一些情境導致你難以進行分析、或分析的結果是事實有落差：

### 1. 越複雜的業務情境，越不清晰的 Bounded Context
當你的業務需求與及情境非常龐大複雜且時。於是伴隨著尋找，往往會發現有著許許多多的 Bounded Context，且這個過程中卻非常容易陷入聚焦於一個點，而難以見數又見林，往往的當你完成了一些 Bounded Context 時，卻在下幾個 Bounded Context 的討論中發現前幾個 Bounded Context 的邊界不太正確，於是你會改了又改調了又調，最後你可能會迷失在分析中：

<p align="center">
  <img src="../context-maps/images/bounded-context-complexity.png?raw=true" width="480px">
</p>
<p align="center">From Internet - Business scenario complexity</p>

### 2. 組織的文化與企業內部的運作模式會影響 Bounded Context 真實性
這句是什麼意思呢？其實當團隊面臨業務情境與需求時，往往也會找出的許多 Bounded Contexts，但這確不太可能會由一個團隊全部完成，而會交由其他的團隊來開發，這也是在企業中常見的行為。

在這個時候團隊間的合作關係模式往往會因為組織的文化或是組織內對團隊間的評價地位，間接影響到跨團隊的協作溝通與開發進展，而這些在尋找 Bounded Context 時也往往難以看見。而這些現象在 DDD 中的戰略設計都是不可忽略的。

時常會發生團隊、部門間往往拿到自己的需求後便會專注在實現上，而忽視了與其他團隊之間的協作，於是進展到一半時才發生因為雙方的關係與對業務需求的銜接認知不一致，進而影響最後交付或是需求的正確性。

或是當組織內部間部門或團隊彼此是互相競爭時，在開發系統與協作勢必會更加困難，而全聽命某一個主管時，往往會增加開發協作上的時程，而這些在分析 Bounded Context 並不會特別意識到。

<p align="center">
  <img src="../context-maps/images/bounded-context-orgnization-complexity.png?raw=true" width="480px">
</p>
<p align="center">From internet - Orgnization culture</p>

### 3. 組織龐大過於複雜、龐大的開發團隊共同開發
承第二點，所以越龐大的組織，或龐大的開發團隊，會對於 Bounded Contexts 共同協作開發上也增加困難度。著名的[康威定律 (Conway's Law)](https://zh.wikipedia.org/wiki/%E5%BA%B7%E5%A8%81%E5%AE%9A%E5%BE%8B)中也提過組織的運作與複雜度往往與系統架構或系統的設計有著關聯性，因此這些現象也是需要與多個團隊共同協作多個 Bounded Contexts 需要考量進去的。

<p align="center">
  <img src="../context-maps/images/bounded-context-multi-teams.png?raw=true" width="480px">
</p>
<p align="center">From internet - Huge company and teams</p>

除了組織的複雜性與文化外，團隊間的協作如果是地理位置上的分散式的團隊協作，或是團隊與團隊間採用遠端模式協作也都會影響。

<p align="center">
  <img src="../context-maps/images/bounded-context-distributed-team.png?raw=true" width="480px">
</p>
<p align="center">From internet - Distributed teams</p>

### 4. 外部系統對 Bounded Context 的影響性
上述是設計與分析 Bounded Context 時難以思考到的組織團隊與文化協作影響，再來讓我們回到系統面上，由於分析業務情境需求的最後目的是要建模成系統完成開發，那麼在這些過程中，可能免不了會有一些考量或遇到的現象是，組織中是否有一些已經在使用，從外部購買來的系統有要整合進這次的業務場景裡？ 
例如對於銷售管理系統來說，非常有名的 Salesforce 是許多企業中會提供給業務人員使用與管理的系統，此時如果新的需求是要開發一套自己的，是否有可能要銜接或是整合一部分使用。

<p align="center">
  <img src="../context-maps/images/bounded-context-external-system.png?raw=true" width="480px">
</p>
<p align="center">From tnternet - Connect external system</p>

這也是需要在做戰略設計時應該考量的面向，然而只專注在 Bounded Contexts 的分析時往往不一定會去考量這件事。

### 5. 可怕的 Legacy 系統
每間企業特別是歷史越悠久的企業，如銀行，都會有一些幾十年前的系統仍在使用，這些系統的因為經歷了許多歲月的調動或開發人員的替換，可能沒有人有辦法完全理解這個 Legacy 系統裡面到底有什麼，到底整合了多少其他的系統。而當開發人員去翻開這些系統的代碼時，會發現因為經歷了歷代工程師的調動，往往有著不成人形的模樣，該系統可能也找不到文件、不曉得去能詢問誰、或是因為早期的系統沒有經過測試保護，可能改了一行就整個 Crash ...

（往往的我們都會戲稱去挑戰或是要去重構 Legacy System 的人就像在屠龍一樣）

<p align="center">
  <img src="../context-maps/images/bounded-context-legacy-system.png?raw=true" width="480px">
</p>
<p align="center">From tnternet - Legacy system</p>

而這就像是前面的外部系統一樣，如果今天新的需求要整合 Legacy 系統時，我們是不是有考量這個 Legacy Sytem 與 Bounded Context 的邊界，還有如果要對接等等的問題。

因此，雖然透過 Domain 與 Bounded Context 已經能夠為我們初步從抽象的業務場景描述找到了建模的邊界，但是在戰略設計中仍不夠，因為業務場景可能很龐大，而且協作的過程中會有著團隊文化、組織協作等影響，以及是否有使用外部的系統和 Legacy System 都是會影響的。

所以在 DDD 中還需要透過 Context Maps 綁上述可能會遇到的情況透過在設計這個 Map 時幫助我們找出潛在的因素。

## 所以什麼是 Context Maps 呢
讓我們來看看 Context Maps 的定義：

> Context Maps could reflect the collaborative  or even organizational teams relationships between different Bounded Contexts in your systems. 


## The Benefit for Drawing Context Map

## The Role of Relationship in Context Map

## The Relationship Pattern of Context Map 

## The Example of Relationship of Context Map 


## 參考資料
