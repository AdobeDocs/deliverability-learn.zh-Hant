---
title: 何謂傳遞能力策略及如何定義傳遞能力
description: 瞭解傳遞能力的定義、其重要性以及關鍵傳遞性指標。
topics: Deliverability
jira: KT-5255
thumbnail: kt5255.jpg
doc-type: article
activity: understand
role: Admin, Leader, User
level: Beginner
team: ACS
exl-id: 5285eda9-5099-48d5-b150-ce2c376ee549
source-git-commit: 6b312cdbba496818337c97ec4f42962aea757901
workflow-type: tm+mt
source-wordcount: '854'
ht-degree: 100%

---

# 傳遞能力策略和定義

設計成功的電子郵件行銷宣傳，必須清楚瞭解行銷目標，不論其目標是潛在客戶開發或客戶關係管理 (CRM) 計畫。這有助於確定目標對象、宣傳內容，以及何時開展外聯活動是理想的。

以下是電子郵件行銷策略目標的一些範例：

* 獲得新客戶
* 將潛在客戶轉換為首次購買者
* 與其他客戶產品建立更密切的客戶關係
* 留住忠誠客戶
* 提高客戶滿意度和品牌忠誠度
* 重新活絡失去或失效的客戶

## 傳遞能力定義

在傳遞能力的定義中，有兩個關鍵量度可發揮作用。*傳遞率*&#x200B;是 ISP 不接受且無法跳出的電子郵件的百分比。接下來是&#x200B;*收件匣位置*，這套用至 ISP 接受的訊息，並判斷電子郵件是否進入收件匣或垃圾郵件資料夾。

在測量電子郵件效能時，務必要同時瞭解傳遞率和收件匣放置率。高傳遞率並非傳遞能力的唯一方面。僅僅因為訊息是透過 ISP 的初始查核點收到，並不表示您的訂閱者實際看到並與您的通訊互動。

## 為何傳遞能力很重要

您應該了解您的電子郵件是否已送達，或者是否進入收件匣，還是進入垃圾郵件資料夾。原因如下。

在策劃和製作您的電子郵件宣傳活動時，需要花費無數時間。如果電子郵件跳出或最終落入訂閱者的垃圾訊息資料夾，您的客戶可能不會閱讀，您的行動要求 (CTA) 也不會得到認可，而且由於轉換率遺失，您將無法達到收入目標。簡言之，您無法忽視傳遞能力。這對於您電子郵件行銷工作的成功及獲利底限至關重要。

遵循傳遞能力最佳實務，可確保您的電子郵件有最佳的開啟機會、點按次數，以及最終目標——轉換。您可以撰寫精彩的主題系列，並擁有精美的影像和精彩的內容。但是，如果該電子郵件沒有傳遞出去，客戶就沒有機會轉換。總而言之，在電子郵件傳遞能力方面，郵件接受流程的每個步驟都取決於前者，以確保方案成功。

### 步驟 1：傳遞的電子郵件

傳遞的重要因素：

* **穩固的基礎架構**：IP 和網域組態、回饋迴路 (FBL) 設定（包括投訴監控和處理），以及定期的跳出處理。對於 Adobe 客戶，Adobe 代表我們的客戶負責此設定。
* **強大的身份驗證**：[!DNL Sender Policy Framework] (SPF)、 [!DNL DomainKeys Identified Mail] (DKIM)、 [!DNL Domain-based Message Authentication]、報告和符合性 (DMARC)。
* **高清單品質**：明確的選擇加入、有效的電子郵件獲得方法和參與政策。
* **一致的發送順序和體積波動的最小化**。
* **高 IP 和網域信譽**。

### 步驟 2：電子郵件收件匣位置

ISP 有獨特、複雜且不斷變更的演算法，可判斷您的電子郵件是放在收件匣、垃圾郵件或垃圾郵件資料夾中。

以下是收件箱放置的一些重要因素：

* 傳遞的電子郵件
* 高參與度
* 低投訴（總體低於 0.1%）
* 卷數一致
* 低垃圾郵件陷阱
* 低硬跳出率
* 封鎖清單問題少

### 步驟 3：電子郵件互動 - 開啟

以下是開啟率的一些重要因素：

* 電子郵件已投遞並進入收件匣
* 品牌認可
* 引人入勝的主旨行和前置標題
* 個性化
* 頻率
* 內容的相關性或價值

### 步驟 4：電子郵件互動 - 點擊

以下是點按率的一些重要因素：

* 電子郵件已傳遞、進入收件匣並開啟
* 強大的 CTA
   * 這是您想要從觀眾那裡實現的主要動作。通常只要按一下 URL 即可。確保使用者可輕鬆找到。
* 內容的相關性或價值

### 步驟 5：轉換

以下是轉換的一些重要因素：

* 以上所有
* 從電子郵件透過有效的 URL 轉換至登陸頁面或銷售頁面
* 登陸頁面體驗
* 品牌認知、認知和忠誠度

### 對收入的潛在影響

轉化是關鍵，但有哪些替代選擇？您的傳遞能力策略可以強化或破壞電子郵件行銷計畫。下表說明弱的傳遞政策可能對行銷方案造成的潛在收入損失。如前所示，對於轉換率為 2%、平均購買額為 $100 美元的企業，收件匣位置每減少 10% 就會造成近 $20,000 美元的收入損失。請記住，這些數字對每個寄件者都是獨一無二的。

| 已傳送 | 百分比 | 已傳遞 | 百分比 | 收件匣 | 不在收件匣中的數字 | 轉換率 | 丟失數 | 平均 | 丟失 |
|------|-----------|-----------|----------|-------|---------------------|-----------------|-----------------|----------|-----------|
|      | 已傳遞 |           | 收件匣 |       |                     |                 | 轉換 | 購買 | 收入 |
| 100K | 99% | 99K | 100% | 99K | - | 2% | 0 | $100 | $ - |
| 100K | 99% | 99K | 90% | 89.1K | 9,900 | 2% | 198 | $100 | $19,800 |
| 100K | 99% | 99K | 80% | 79.2K | 19,800 | 2% | 396 | $100 | $39,600 |
| 100K | 99% | 99K | 70% | 69.3K | 29,700 | 2% | 594 | $100 | $59,400 |
| 100K | 99% | 99K | 60% | 59.4K | 39,600 | 2% | 792 | $100 | $79,200 |
| 100K | 99% | 99K | 50% | 49.5K | 49,500 | 2% | 990 | $100 | $99,000 |
| 100K | 99% | 99K | 40% | 39.6K | 59,400 | 2% | 1188 | $100 | $118,800 |
| 100K | 99% | 99K | 30% | 29.7K | 69,300 | 2% | 1386 | $100 | $138,600 |
| 100K | 99% | 99K | 20% | 19.8K | 79,200 | 2% | 1584 | $100 | $158,400 |
