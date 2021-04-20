---
title: 複製
description: 瞭解如何識別和限制重複項目，以改善傳遞能力。
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
source-wordcount: '388'
ht-degree: 3%

---


# 複製{#duplicates}

擁有重複的電子郵件地址可能會產生多種後果：

* 相同的訊息會多次傳送。 即使Adobe在發送之前預設執行重複資料消除過程，在拆分目標時，也沒有任何措施可以阻止具有不同內容的不同操作發送相同的消息。
* 未接受取消訂閱請求。 如果收件者在收到訊息後取消訂閱，則其重複的描述檔仍符合日後訊息的資格。

除了選擇加入程式的這種側步外，這種情況還可能導致用戶將郵件視為垃圾郵件，並在ISP觸發拒絕清單程式。

在資料庫上執行操作時必須特別謹慎：

* 必須仔細配置導入，特別是在選擇協調密鑰時。
* 變更的電子郵件地址也可以是重複的來源。 尤其是，兩個具有不同網域的地址可以被路由到同一個郵箱，例如，如果某公司更改了名稱，並且在某段時間內維護了前一個域：joe.doe@amce-co.com和joe.doe@acme-rebranded.com。
* 自動導入（無論是清單還是來自其他資料庫）是管理配置檔案時要考慮的元素。 刪除或移動另一個分區中的配置檔案時會發生什麼情況？ 例如，在下訂單時，可通過自動導入在初始分區中重新建立它。
* 可以使用視圖而不是分區來實現將配置檔案儲存在不同資料夾中。 這樣，您可以確定配置式位於同一物理分區中，同時仍能顯示和管理適當的權限。

同樣地，有時不同分區之間的複製是正常的。 例如，當傳送給第三方或不同的公司實體時，出於不同原因，同一個人是收件者是合乎邏輯的。 但是，在同一分區中查找重複項很少是正常的。

## 產品特定資源

刪除重複地址可保護您的發送信譽並確保良好的隔離管理。 進一步瞭解下列產品檔案章節：

**Adobe Campaign Classic**

* [重複資料消除活動](https://experienceleague.adobe.com/docs/campaign-classic/using/automating-with-workflows/targeting-activities/deduplication.html)
* [使用重複資料消除活動的合併功能](https://experienceleague.adobe.com/docs/campaign-classic/using/automating-with-workflows/use-cases/data-management/deduplication-merge.html)

**Adobe Campaign Standard**

* [從匯入的檔案中重複刪除資料](https://experienceleague.adobe.com/docs/campaign-standard/using/managing-processes-and-data/workflow-use-case/data-management/deduplicating-data-imported-file.html)