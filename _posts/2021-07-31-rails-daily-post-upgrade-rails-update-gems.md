---
title: Ruby on Rails 每日一文 - 升級 rails 時如何找到應該升級的 gem 們
layout: post
author: lawa0921
category: blog
date: '2021-07-31 20:39:29'
tag:
- Ruby
- Rails
- Rails upgrade
---

近日正在嘗試將公司專案從 Rails 5.2.6 升級至 Rails 6.1.4  
在升級時第一件事情就是執行 `bundle update rails` 來升級 rails  
但是中間會有很多 gem 的相依性  
因此很有可能會出現錯誤  
這邊就是想簡單紀錄一下升級套件時遇到的問題  
並記錄下該如何判斷哪些套件需要升級  
哪些不用  
  
這是第一次執行 `bundle update rails` 時得到的結果  
  
![]({{ 'assets/images/截圖 2021-07-31 下午8.36.49.png' | relative_url }})  
  
需要特別注意的是  
這邊的意思絕對不是這些 gem 全部都要更新  
仔細看會發現只有 `swipebox` 所支援的 rails 版本為 `(>= 3.1, < 6.0)` 而我們這次所升級的版本已經超過該 Gem 當前版本的支援範圍  
這時候你有三個做法  
  
1. 上 [ruby gems](https://rubygems.org/?locale=zh-TW) 去找這個 gem 有沒有支援更新的版本  
2. 如果沒有，自己包一個 gem 回來，改寫掉他的 dependency  
3. 砍了他，找一個功能類似的 gem 代替  
  
這邊的優先度是 1 > 3 > 2  
有人持續維護的 gem 預期上都會有跟上目前比較新的版本  
但是要是像筆者這樣  
剛好使用了這個沒有支援新版本的 gem 的話你就只剩下 2 跟 3 了  
這邊筆者檢查了一下發現目前的專案內並沒有非要採用這個 `swipebox` 不可的理由  
因此就將其移除了  
  
這邊需要特別注意的是  
並不是每次 dependency 都會寫 `rails`   
也有很多情況依賴於 rails 相關的其他 Gem  
比如 `activerecord` 、 `railties`  、 `activesupport` 等等  
這些 gem 的版本基本上都與 rails 相同  
比如你使用 rails 6.1.4 這些核心套件就也是 6.1.4  
因此你需要注意的是依賴範圍不在你更新版本內的 Gem  
  
![]({{ 'assets/images/截圖 2021-07-31 下午8.56.31.png' | relative_url }})  
  
以這張圖的例子來說  
筆者需要升級的 gem 就是 `rails-i18n` 這個  
因為目前版本依賴於 railties 5.0 至 6.0 之間  
並不支援 6.1.4
