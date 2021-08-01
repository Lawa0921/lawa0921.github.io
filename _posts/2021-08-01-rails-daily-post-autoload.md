---
title: Ruby on Rails 每日一文 - 關於 ruby 與 rails 載入檔案的機制
layout: post
author: lawa0921
category: blog
date: '2021-08-01 13:38:30'
tag:
- Rails
- Ruby
- Autoload
---

我們在開發 rails 時  
我們時常下意識在各個檔案中寫下各個 Class name  
比如我們可能在 `app/models/user.rb` 裡面寫到像是  
  
```
def get_orders
  Order.where( ... )
end
```
  
這個是在哪邊被載入的呢？  
  
### 關於 ruby 的載入機制
  
一般我們如果需要引入某個檔案  
我們必須要手動使用 `require` 來引入檔案比如： `require 'awesome_print'`  
這邊就有個疑問了  
為什麼我們可以不用寫路徑  
單純寫 gem 的名字就引入檔案呢？  
  
那是因為 ruby 有一個 `$LOAD_PATH` 的全域變數來控制我們可以 require 到什麼  
只要打開 irb 並執行 $LOAD_PATH 就可以看到目前可以載入的路徑了  
  
如果想進一步確認目前 require 了些什麼檔案  
也有個 `$LOADED_PATH` 來確認目前所載入的檔案有哪些  
而這些檔案也就是在你當前環境當中所有可以用到的 method 或是 const 的來源  
  
到這邊我們只解決了載入檔案的問題  
這代表著我們在執行時 `$LOAD_PATH` 裡面應該是有某個檔案裡有著 Order 這個 const 的  
那 rails 又是如何在 user.rb 檔案裡面找到 Order 這個 const 的呢？  
  
### rails 的 autoload
  
rails 所採用的搜尋方法的根源是來自 const_missing  
與 method_missing 很像  
只是一個是查找 method 後找不到所呼叫的方法  
一個是查找 const 後找不到所呼叫的方法  
它覆寫了這個 method 的邏輯  
在沒有找到 const 的情況下  
利用了一些 rails 預設的邏輯來查找 const  
你只要照著 rails 的預設邏輯來放置這些檔案  
實現與 rails 的約定  
rails 就會神奇地幫你把東西都載入完成了  
  
如果對於更詳的載入過程有興趣  
可以參考 這篇詳細的 [autoload](https://www.urbanautomaton.com/blog/2013/08/27/rails-autoloading-hell/) 文章
