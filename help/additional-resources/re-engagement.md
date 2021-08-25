---
title: 重新參與的最佳實務
description: 了解如何透過重新參與策略來改善傳遞能力。
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

在實作傳遞能力時，一些最佳實務是嘗試透過重新參與（或贏回）策略來維持健康的訂閱者基礎並改善傳遞能力。

* 維持健康的用戶群是確保良好及一致交付的主要方面之一。 許多傳遞能力問題都源自於糟糕的資料實踐和維護。
* 現今行銷人員最常遇到的問題之一，是訂閱者活動不活躍（又稱為低參與或非參與），這可能對電子郵件的傳送和低投資報酬率造成負面影響。

>[!NOTE]
>
>有關重新參與促銷活動策略和Adobe的傳遞能力服務的詳細資訊，請聯繫您的傳遞能力顧問，或與Adobe銷售代理聯繫。

## ISP如何檢視非參與活動？ {#how-do-isps-view-non-engagement-activity-}

多年來，ISP一直使用使用者的參與回饋量度來決定要放置訊息的位置，或者決定要放置訊息的位置。 使用者[參與](/help/engagement.md)包含正反饋和負反饋，ISP會持續監控兩者。 沒有參與也許是消極參與的主要貢獻者之一。 從傳遞能力的角度來看，持續傳送行銷活動給未顯示參與的使用者，也會降低IP位址和網域的整體信譽。

Gmail、Microsoft®和OATH等ISP會將非參與視為不需要的電子郵件，並開始將郵件重新導向至垃圾郵件資料夾。 此外，這些訂閱者可能不再擁有電子郵件帳戶，這可作為「回收」垃圾訊息陷阱。 這表示該地址在某段時間內無效，並且所有消息都被拒絕。 如果您的訂閱者管理系統未刪除「硬退信」地址，則可能會寄送至垃圾郵件陷阱，而這可能導致重大的傳送問題。

## 您應該如何處於閒置狀態？ {#how-should-you-approach-inactivity-}

使用Adobe平台的客戶可以檢閱開啟的頁面，並根據區段按一下資料，以檢視其例項內的閒置狀態。 由於非參與可能會阻礙傳遞，因此第一個想法是從資料庫中移除訂閱者。 然而，有時候這可能被證明是錯誤的選擇。 因此，重新參與（也稱為回合）策略是保留對接收郵件感興趣的訂閱者，並逐步淘汰不再顯示活動的訂閱者的最佳建議。

## 重新參與促銷活動真的有用嗎？ {#do-re-engagement-campaigns-really-work-}

根據回訪路徑研究，重新參與促銷活動的結果是12%的開放率，而正常促銷活動的平均開放率為14%。 雖然只有24%的訂閱者閱讀了重新參與促銷活動，但約45%的訂閱者閱讀了後續的訊息。

![](../../help/assets/deliverability_implementation_1.png)

## 如何建立重新參與的促銷活動？ {#how-do-you-create-a-re-engagement-campaign-}

### 階段1 {#phase-1}

* 第一步是識別幾乎沒有開啟或點按活動的訂閱者，並據此根據設定的時間範圍劃分此群組。 經驗法則是檢閱過去90天內未開啟或點按電子郵件的訂閱者。 不過，這會因業務性質（例如季節性傳送）而異。
* 定義時間範圍時要記住的另一點是，ISP和拒絕上市公司認為，參與時間介於1.5到1.8年之間。 此外，行為活動（例如購買和網站活動）或其他接觸點（例如註冊階段或第一聯絡點期間的偏好設定）。

### 階段2 {#phase-2}

* 定義區段後，下一步是建立重新參與促銷活動，根據已識別的量度來迎合訂閱者需求。 建立主旨行有助於提高訂閱者的興趣。 根據回訪路徑的研究，指出「我們想念你」的主題行和內容產生的回應率比「我們希望你回來」高。
* 您也可以獎勵重新使用電子郵件。 在考慮提供折扣的優惠方案時，最好使用金額與百分比。 Return Path還建議您執行此操作，因為它會產生更高的回應率。 最後，執行A/B分割測試以檢閱回應和成功率，也是很實用的選項。

### 階段3 {#phase-3}

下一步是決定重新參與促銷活動的頻率。 與重新確認訊息不同，重新參與促銷活動的目的是隨著時間，以一系列電子郵件贏回訂閱者。 以下範例提供頻率的範例。

![](../../help/assets/deliverability_implementation_2.png)

依照開啟或點按活動與促銷活動互動的訂閱者會新增至訂閱者的參與清單。

### 階段4 {#phase-4}

* 下一階段是識別持續不顯示任何活動的訂閱者，並逐步減少一段時間內傳送電子郵件給他們的次數。 如果過去一年內沒有任何活動，最好將訂閱者電子郵件訂閱擱置。 雖然他們對電子郵件內容並不感興趣，但總是有最後一次機會透過傳送一次性重新確認促銷活動，讓他們重新啟用訂閱。
* 若要詢問訂閱者是否想要保留在訂閱清單上，再確認促銷活動是詢問他們是否在長時間內未活動的好方法。 建立促銷活動時，建議您新增「按一下這裡」連結，以便他們確認動作並驗證其位址。 這樣，動作便可記錄在資料庫中。 以下是重新確認電子郵件的範例：

   ![](../../help/assets/deliverability_implementation_3.png)

   一旦訂閱者採取動作後，即可提供登錄頁面並確認其重新訂閱。 以下是登錄頁面的範例：

   ![](../../help/assets/deliverability_implementation_4.png)

## 產品專屬資源

**Adobe Campaign**

* [在Campaign Classic中追蹤記錄](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/delivery-dashboard.html#tracking-logs)
* [在Campaign Standard中追蹤記錄](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/sending-and-tracking-messages/tracking-messages.html#tracking-logs)

**Adobe客戶歷程管理**

* [訊息追蹤](https://experienceleague.adobe.com/docs/journey-optimizer/using/reporting/message-tracking.html?lang=zh-Hant)
