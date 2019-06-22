## Application Service

* The application services are the direct clients of the domain model
* Managing transaction, security, and the task of delegating process
* Use case

用 command object 當作 appliction service method 的傳入參數

Decoupled Service Output

application service 的 method 不回傳參數, 而是外部注入一個可 write 的實例
並且在 method 使用這個實例將資料寫入


