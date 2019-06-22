## Domain Service

### 什麼不是 Domain Service

如果一個 Service 它包含了複雜的商業邏輯操作、呼叫一些遠端的資源，交互作用，像是 SOA, RPCs 等，
這樣子的 Service 這裡的定義不是一個 Domain Service，Domain Service 只專注在核心領域

### 什麼是 Domain Service

* 當無法很明確的將某個操作的責任歸屬於特定的 Entity 或 Value Object 時，可以考慮引入 Domain Service，
* 特別注意 Domain Service 必須是 stateless 的
* Domain Service 不管理 transaction，那是 Application Service 的責任
* 對於 Domain Service 來說 Application Service 是一個理所當然的 client 端

### 什麼時候需要使用 Domain Service

* 處理一個複雜的商業邏輯
* 將一個 Domain Object 轉換成另一個 Domain Object
* 計算多個 Domain Object 的值

### 關於 interface 

因為架構分層可能考慮將 service 為一個 interface 存在於 domain 中，由 infra 層來定義實際行為，
但這沒有一定答案要這樣子做，其中一個考量是，也許對於實作的命名我們會以 interface + impl 或 default + interface
的方式來命名，通常這樣的名命都值得我們重新思考，是否需要用 interface

使用 Dependency injection 或 Factory 都可以達到 testable 的效果，不是非得要使用 interface

### 值得討論

A service in the domain is welcome to use Repositoryies as needed,
but accessing Repositories from an Aggregate instance is not a recommended practice





