---
title: 重新參與的最佳實務
description: 瞭解如何透過重新參與策略來改善傳遞能力。
feature: Additional resources
topics: Deliverability
kt: null
thumbnail: null
doc-type: article
activity: understand
team: ACS
translation-type: tm+mt
source-git-commit: 96ed84da391faaabd3001ddd6a411ddc1f46b033
workflow-type: tm+mt
source-wordcount: '928'
ht-degree: 1%

---


# 重新參與的最佳實務 {#re-engagement}

在實作傳送能力時，一些最佳實務是嘗試透過重新參與（或贏回）策略來維持健康的訂閱者群並改善傳送能力。

* 維持健康的用戶群，是確保良好及一致的傳送的主要方面之一。 許多可交付性問題都源自於不良的資料實踐和維護。
* 如今，行銷人員面臨的最常見問題之一是非活動的訂閱者活動（也稱為低或非參與），這可能會對電子郵件的傳送和低投資報酬率產生負面影響。

>[!NOTE]
>
>有關重新參與促銷活動策略和Adobe的可交付性服務的更多資訊，請聯絡您的可交付性顧問，或與Adobe銷售代理聯絡。

## ISP如何檢視非參與活動？{#how-do-isps-view-non-engagement-activity-}

多年來，ISP都使用使用者的參與意見回饋量度，來決定放送訊息的位置，或者他們是否應該放送訊息。 使用者[engagement](/help/engagement.md)包含正反饋和負反饋，而ISP會持續監控兩者。 沒有參與也許是消極參與的主要貢獻者之一。 從傳遞性的角度來看，一致地傳送促銷活動給未顯示參與度的使用者，也會降低IP位址和網域的整體信譽。

Gmail、Microsoft和ANSOM等ISP將不參與視為不需要的電子郵件，並開始將訊息重新導向至垃圾訊息資料夾。 此外，這些訂閱者可能不再擁有電子郵件帳戶，這可以當成「回收」垃圾訊息陷阱。 這表示該地址在一段時間內無效，且所有消息都被拒絕。 如果您的訂閱者管理系統未移除「硬性跳回」位址，寄送郵件至垃圾郵件陷阱很可能會導致重大傳送問題。

## 您應該如何對待閒置？{#how-should-you-approach-inactivity-}

使用Adobe平台的客戶可以檢視其例項中的閒置狀態，方法是檢閱開啟的資料，然後根據區段點選資料。 由於非參與可能會阻礙傳遞，因此第一個想法是將訂閱者從資料庫中移除。 然而，有時候，這可能被證明是錯誤的選擇。 因此，重新參與（也稱為贏回）策略是保留有興趣接收郵件的訂閱者，並逐步淘汰不再顯示活動的訂閱者的最佳建議。

## 重新參與宣傳活動真的有效嗎？{#do-re-engagement-campaigns-really-work-}

根據回訪路徑研究，重新參與促銷活動的結果是12%的開放率，而正常促銷活動的平均為14%。 雖然只有24%的訂閱者讀過重新參與的宣傳，但約45%的訂閱者讀過後續的訊息。

![](../../help/assets/deliverability_implementation_1.png)

## 如何建立重新參與的促銷活動？{#how-do-you-create-a-re-engagement-campaign-}

### 階段1 {#phase-1}

* 第一步是識別幾乎沒有開啟或點按活動的訂閱者，並據此根據設定的時間範圍劃分此群組。 經驗法則是檢閱過去90天內未開啟或點按過電子郵件的訂閱者。 不過，這會因業務性質（例如季節性傳送）而有所不同。
* 在定義時間框架時要記住的另一點是，ISP和Denylist公司認為，參與時間介於1.5到1.8年之間。 此外，行為活動（例如購買和網站活動）或其他接觸點（例如註冊階段或首次聯絡點時的偏好設定）。

### 階段2 {#phase-2}

* 定義區段後，下一步是根據已識別的量度建立符合訂閱者需求的重新參與促銷活動。 建立主旨行有助於提高訂閱者的興趣。 根據Return Path的研究，指出「我們想念你」的主題行和內容產生的回應率高於「我們希望你回來」。
* 此外，您還可以針對重新使用電子郵件提供獎勵。 在考慮提供折扣的優惠時，最好使用金額與百分比。 Return Path也建議您這麼做，因為這會產生較高的回應率。 最後，執行A/B分割測試以檢視回應和成功率也是有用的選項。

### 階段3 {#phase-3}

下一步是決定重新參與促銷活動的頻率。 與重新確認訊息不同，重新參與促銷活動的目的，是透過一連串的電子郵件，讓訂閱者隨時間回復。 下面的示例提供了頻率的示例。

![](../../help/assets/deliverability_implementation_2.png)

遵循開啟或點按活動而參與促銷活動的訂閱者會新增至訂閱者的參與清單。

### 階段4 {#phase-4}

* 下一階段是識別不斷顯示活動的訂閱者，並逐漸減少一段時間內寄送電子郵件給他們的次數。 如果過去一年內沒有活動，請將訂閱者的電子郵件訂閱置於擱置狀態是好事。 雖然他們對電子郵件內容不感興趣，但是透過傳送一次性重新確認促銷活動，永遠有最後機會讓他們重新啟動訂閱。
* 重新確認促銷活動是詢問長期處於非活動狀態的訂閱者是否想留在訂閱清單上的好方法。 建立促銷活動時，最好新增「按一下這裡」連結，以便他們確認動作並驗證其位址。 這樣，操作就可以記錄在資料庫中。 以下是重新確認電子郵件的範例：

   ![](../../help/assets/deliverability_implementation_3.png)

   一旦訂閱者採取行動後，就可提供登陸頁面，確認其重新訂閱。 以下是著陸頁面的範例：

   ![](../../help/assets/deliverability_implementation_4.png)

## 產品特定資源

**Adobe Campaign**

* [在Campaign Classic中追蹤記錄檔](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/delivery-dashboard.html#tracking-logs)
* [在Campaign Standard中追蹤記錄檔](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/sending-and-tracking-messages/tracking-messages.html#tracking-logs)

**Adobe客戶歷程管理**

* [訊息追蹤](https://experienceleague.adobe.com/docs/customer-journey-management/using/reporting/message-tracking.html)