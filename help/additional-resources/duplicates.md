---
title: 複製
description: 了解如何識別和限制重複項目，以改善傳遞能力。
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: f89dbb38-a8d4-4294-b017-6fed72591593
source-git-commit: d6094cd2ef0a8a7741e7d8aa4db15499fad08f90
workflow-type: tm+mt
source-wordcount: '386'
ht-degree: 6%

---

# 複製 {#duplicates}

擁有重複的電子郵件地址可能會產生多種後果：

* 同一訊息會多次傳送。 即使Adobe在傳送之前預設執行重複資料刪除程式，在分割目標時，也沒有任何內容可阻止具有不同內容的不同動作所傳送的相同訊息。
* 未接受取消訂閱請求。 如果收件者在收到訊息後取消訂閱，其重複的設定檔仍符合未來訊息的資格。

除了選擇加入程式的側步以外，此情況可能會導致使用者將訊息視為垃圾訊息，並在ISP觸發封鎖清單程式。

對資料庫執行操作時，您必須特別謹慎：

* 進口必須經過精心配置，特別是在選擇調解密鑰時。
* 變更的電子郵件地址也可以是重複項目的來源。 尤其是，具有不同網域的兩個地址可能會被路由到相同的郵箱，例如，如果某公司已更改名稱並且已維護了前一個域一段時間：joe.doe@amce-co.com和joe.doe@acme-rebranded.com。
* 自動匯入（不論是清單匯入或來自其他資料庫匯入）是管理設定檔時要考慮的元素。 刪除或移動另一個分區中的配置檔案時會發生什麼情況？ 例如，在下採購訂單時，可通過自動導入在初始分區中重新建立它。
* 可將設定檔儲存在不同資料夾中，可使用檢視來實作，而非使用分區。 這樣，您就可以確定配置檔案位於同一物理分區中，同時仍能顯示和管理足夠的權限。

同樣地，在不同分區之間的重複項也是正常的情況。 例如，傳送給第三方或不同的公司實體時，由於不同原因，同一人成為收件者是合乎邏輯的。 但是，在同一分區內查找重複項很少是正常的。

## 產品特定資源

重複刪除地址可保護您的發送信譽並確保良好的隔離管理。 如需詳細資訊，請參閱下列產品檔案章節：

**Adobe Campaign Classic**

* [重複資料刪除活動](https://experienceleague.adobe.com/docs/campaign-classic/using/automating-with-workflows/targeting-activities/deduplication.html)
* [使用重複資料刪除活動的合併功能](https://experienceleague.adobe.com/docs/campaign-classic/using/automating-with-workflows/use-cases/data-management/deduplication-merge.html?lang=zh-Hant)

**Adobe Campaign Standard**

* [從匯入的檔案中重複刪除資料](https://experienceleague.adobe.com/docs/campaign-standard/using/managing-processes-and-data/workflow-use-case/data-management/deduplicating-data-imported-file.html)
