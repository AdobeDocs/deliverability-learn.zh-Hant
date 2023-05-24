---
title: 複製
description: 瞭解如何識別和限制重複項以提高傳輸能力。
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

* 同一郵件正在多次發送。 即使Adobe在發送之前預設執行重複資料消除過程，也無法阻止在拆分目標時具有不同內容的不同操作發送相同的消息。
* 未處理取消訂閱請求。 如果收件人在收到郵件後取消訂閱，則其重複的配置檔案仍適合將來的郵件。

除了選擇加入過程的這一側步之外，這種情況可能會導致用戶將郵件視為垃圾郵件，並在ISP觸發一個拒絕清單過程。

對資料庫執行操作時必須特別謹慎：

* 必須精心配置導入，特別是在選擇協調密鑰時。
* 更改的電子郵件地址也可以是重複的來源。 特別是，兩個具有不同域的地址可以被路由到同一郵箱，例如，如果公司更改了名稱並維護了以前的域一段時間，則：joe.doe@amce-co.com和joe.doe@acme-rebranded.com。
* 自動導入是管理配置檔案時要考慮的元素，無論是清單還是來自其他資料庫。 刪除或移動另一個分區中的配置檔案時會發生什麼情況？ 例如，當下達採購訂單時，可以通過自動導入在初始分區中重新建立它。
* 可以使用視圖而不是分區來實現將配置檔案儲存在不同資料夾中。 這樣，您就可以確定配置檔案位於同一物理分區中，同時仍然能夠顯示和管理適當的權限。

同樣，在不同分區之間存在重複項的情況是正常的。 例如，在為第三方或不同的公司實體發送時，出於不同原因，同一人成為收件人是合乎邏輯的。 但是，在同一分區中查找重複項的情況很少正常。

## 產品特定資源

消除重複地址可保護您的發送信譽並確保良好的隔離管理。 瞭解以下產品文檔部分的詳細資訊：

**Adobe Campaign Classic**

* [重複資料消除活動](https://experienceleague.adobe.com/docs/campaign-classic/using/automating-with-workflows/targeting-activities/deduplication.html)
* [使用重複資料消除活動的合併功能](https://experienceleague.adobe.com/docs/campaign-classic/using/automating-with-workflows/use-cases/data-management/deduplication-merge.html?lang=zh-Hant)

**Adobe Campaign Standard**

* [從匯入的檔案中重複刪除資料](https://experienceleague.adobe.com/docs/campaign-standard/using/managing-processes-and-data/workflow-use-case/data-management/deduplicating-data-imported-file.html)
