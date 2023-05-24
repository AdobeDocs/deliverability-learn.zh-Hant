---
title: 重新參與的最佳實務
description: 瞭解如何通過重新參與策略提高交付能力。
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: 30118706-d4c0-4bd8-8c9b-50c26b8374ef
source-git-commit: d6094cd2ef0a8a7741e7d8aa4db15499fad08f90
workflow-type: tm+mt
source-wordcount: '924'
ht-degree: 1%

---

# 重新參與的最佳實務 {#re-engagement}

在實施可交付性的同時，一些最佳做法包括嘗試通過重新參與（或雙贏）戰略來維護健康的用戶群並改進可交付性。

* 維護健康的用戶群是確保良好和一致交付的主要方面之一。 許多可交付性問題都源於糟糕的資料實踐和維護。
* 營銷人員今天面臨的最常見問題之一是非活動的訂戶活動（也稱為低或非參與），這可能對電子郵件的發送和低ROI產生不利影響。

>[!NOTE]
>
>有關重新參與市場活動策略和Adobe的可交付性服務的詳細資訊，請與您的可交付性顧問聯繫，或與您的Adobe銷售代理聯繫。

## ISP如何查看非參與活動？ {#how-do-isps-view-non-engagement-activity-}

多年來， ISP一直使用用戶的項目反饋指標來決定在何處放置消息，或者他們是否應該發送消息。 用戶 [參與](/help/engagement.md) 包括正反饋和負反饋以及ISP監控。 沒有參與也許是消極參與的主要貢獻者之一。 從可交付性的角度來看，持續向未顯示參與的用戶發送市場活動也會降低IP地址和域的整體信譽。

Gmail、Microsoft®和OATH等ISP將非參與視為不需要的電子郵件，並開始將郵件重定向到垃圾郵件資料夾。 此外，這些訂閱者可能不再擁有電子郵件帳戶，這可以用作「回收」垃圾郵件陷阱。 這意味著該地址在一段時間內無效，並且所有消息都被拒絕。 如果您的訂戶管理系統未刪除「硬跳轉」地址，則可能會發送垃圾郵件陷阱，導致嚴重的傳遞問題。

## 您應該如何處理不活動？ {#how-should-you-approach-inactivity-}

使用Adobe平台的客戶可以通過查看開啟的頁面並根據頁面按一下資料來查看其實例中的非活動狀態。 由於非參與可能會妨礙交付，因此第一個想法是從資料庫中刪除訂閱者。 然而，有時這可能被證明是一個錯誤的選擇。 因此，重新參與（也稱為「雙贏」）策略是最好的建議，可以保留對接收郵件感興趣的訂閱者，並逐步淘汰那些不再顯示活動的訂閱者。

## 重新參與活動真的有效嗎？ {#do-re-engagement-campaigns-really-work-}

根據Return Path的一項研究，重新參與活動的結果是開放率為12 % ，而正常活動的平均開放率為14 % 。 儘管只有24%的訂閱者閱讀過重新參與活動，但約45%的訂閱者閱讀了隨後的消息。

![](../../help/assets/deliverability_implementation_1.png)

## 如何建立重新參與活動？ {#how-do-you-create-a-re-engagement-campaign-}

### 階段1 {#phase-1}

* 第一步是識別很少或沒有開啟或按一下活動的訂閱者，並據此根據設定的時間框架對此組進行分段。 經驗法則是查看過去90天內未開啟或按一下電子郵件的訂閱者。 但是，這會因業務性質（例如季節性發送）而有所不同。
* 在定義時間框架時要記住的另一點是，ISP和Denylist公司認為參與時間介於1.5到1.8年之間。 此外，諸如購買和網站活動等行為活動，或其他觸點，如在註冊階段或第一聯繫點期間的首選項。

### 階段2 {#phase-2}

* 一旦定義了段，下一步是根據已經識別的度量建立迎合訂戶的重新參與活動。 建立主題線有助於增加訂戶的興趣。 根據Return Path的一項研究，列出「We miss you」的主題行和內容生成的響應率比「We want you rece」高。
* 還可以為重新使用電子郵件提供激勵。 在考慮提供折扣時，最好使用美元金額與百分比之比。 Return Path還建議在引起較高響應時執行此操作。 最後，執行A/B拆分test以審查響應和成功率也是一個有用的選擇。

### 階段3 {#phase-3}

下一步是確定重新參與活動的頻率。 與重新確認消息不同，重新參與活動的目的是隨著時間的推移，通過一系列電子郵件來贏回訂閱者。 以下示例提供了頻率的示例。

![](../../help/assets/deliverability_implementation_2.png)

通過遵循開啟或點擊活動而參與該活動的訂戶被添加回預訂者清單中。

### 階段4 {#phase-4}

* 下一階段是確定那些持續不顯示任何活動的訂閱者，並逐漸減少一段時間內向他們發送電子郵件的次數。 如果過去一年沒有活動，則最好將訂閱用戶的電子郵件訂閱擱置。 儘管他們對電子郵件內容沒有表現出任何興趣，但通過發送一次性的重新確認活動，他們始終有最後一次機會重新激活訂閱。
* 重新確認市場活動是詢問長期處於非活動狀態的訂閱者是否希望保留在訂閱清單中的良好方法。 建立市場活動時，最好添加「按一下這裡」連結，以便他們確認活動並驗證其地址。 這樣，操作就可以記錄在資料庫中。 以下是重新確認電子郵件的示例：

   ![](../../help/assets/deliverability_implementation_3.png)

   一旦用戶採取了操作，就可以提供一個登錄頁，其中確認了他們重新訂閱。 下面是登錄頁的示例：

   ![](../../help/assets/deliverability_implementation_4.png)

## 特定於產品的資源

**Adobe Campaign**

* [跟蹤Campaign Classic中的日誌](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/delivery-dashboard.html#tracking-logs)
* [跟蹤Campaign Standard中的日誌](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/sending-and-tracking-messages/tracking-messages.html#tracking-logs)

**Adobe客戶行程管理**

* [訊息追蹤](https://experienceleague.adobe.com/docs/journey-optimizer/using/reporting/message-tracking.html?lang=zh-Hant)
