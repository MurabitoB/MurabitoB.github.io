---
title: Entity Framework Identity 插入資料時自動獲得id
date: 2020-11-24
categories:
- .Net Framework
tag:
- .Net Framewrok
---

## EntityFramework Identity 插入資料時自動獲得id

### 環境
- Visual Studio 2019
- .Net Framework : 4.7.2
- Entity Framework : 6.4.4
- MySql.Data.EntityFramework: 8.0.18
- DB : MariadDB
- - -
### 問題
在專案上有需要MariadDB的Table做資料維護，而Database是甲方爸爸開的，所以是必須以Database First的模式開發。

甲方爸爸的欄位皆以int做主鍵id並以auto increment做欄位的自動增值。  

資料維護需要以Ajax的方式在新增之後馬上顯示在前端的頁面中，  
被新增的資料有可能在同頁面中馬上被刪除，因此需要新增資料後要馬上取得該欄位的id並回傳給前端。  

此外在插入資料的時候，並沒有直接指定值，而是由資料庫自動產生對應的id，這就造成了無法知道生成的資料的id。

### 解法

Entity Framework會依照映射的模型有沒有identity欄位來決定新增後會不會自動取值回來。

可以透過Entity Framework的Log的Action去指定輸出生成的SQL語法用的method來觀察轉換出來的SQL語法

```C#
context.Database.Log = Console.WriteLine;
```
在INSERT後端包含了SELECT的語法，並且由於對應的資料庫是MariadDB，所以輸出的SQL CODE中用到了MySQL的 last_insert_id()的函式，如果環境是對應到Sql Server，那生成的sql code會變成 scope_identity()。

```SQL
SELECT
`sid`
FROM `target_data`
WHERE  row_count() > 0 AND `sid`= last_insert_id()
```



### 參考資料：  
https://blog.csdn.net/wangzl1163/article/details/52037714  
https://dotblogs.com.tw/yc421206/2020/06/27/ef_insert_after_query_identity_field