---
title: 複製
description: 瞭解如何識別並限制重複專案，以提升傳遞能力。
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: f89dbb38-a8d4-4294-b017-6fed72591593
source-git-commit: d6094cd2ef0a8a7741e7d8aa4db15499fad08f90
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 5%

---

# 複製 {#duplicates}

擁有重複的電子郵件地址可能會有多重後果：

* 相同訊息傳送多次。 即使Adobe在傳送前依預設會執行重複資料刪除程式，在分割目標時，沒有什麼可停止具有相同內容之不同動作傳送的相同訊息。
* 未接受取消訂閱請求。 如果收件者在收到訊息後取消訂閱，則其重複的設定檔仍適用於未來的訊息。

除了這種逐步選擇加入程式外，此狀況也可能導致使用者將訊息視為垃圾訊息，並觸發ISP的封鎖清單程式。

在資料庫上執行作業時，您必須特別謹慎：

* 必須小心設定匯入，尤其是選擇調解金鑰時。
* 變更的電子郵件地址也可以是重複專案的來源。 舉例來說，如果公司變更名稱並維持先前網域特定時間，則具有不同網域的兩個位址可能會路由傳送到相同的信箱：joe.doe@amce-co.com和joe.doe@acme-rebranded.com。
* 自動匯入（不論是清單匯入還是從其他資料庫匯入）是管理設定檔時要考慮的元素。 當您刪除或移動另一個分割中的設定檔時，會發生什麼情況？ 例如，當發出採購單時，可以在初始分割區中透過自動匯入來重新建立它。
* 將設定檔儲存在不同的資料夾中，可以使用檢視而非分割來實施。 如此一來，您確定設定檔位於相同的實體分割區，同時仍可顯示和管理足夠的許可權。

同樣地，不同分割區之間也存在正常重複的情況。 例如，為第三方或不同公司實體傳送時，出於不同原因，同一人成為收件者符合邏輯。 不過，在相同分割區中尋找重複專案很少是正常現象。

## 產品特定資源

對地址進行重複資料刪除可保護您的傳送信譽，並確保良好的隔離管理。 請在下列產品檔案章節中瞭解更多資訊：

**Adobe Campaign Classic**

* [重複資料刪除活動](https://experienceleague.adobe.com/docs/campaign-classic/using/automating-with-workflows/targeting-activities/deduplication.html)
* [使用重複資料刪除活動的合併功能](https://experienceleague.adobe.com/docs/campaign-classic/using/automating-with-workflows/use-cases/data-management/deduplication-merge.html?lang=zh-Hant)

**Adobe Campaign Standard**

* [從匯入的檔案中重複刪除資料](https://experienceleague.adobe.com/docs/campaign-standard/using/managing-processes-and-data/workflow-use-case/data-management/deduplicating-data-imported-file.html)
