---
title: 卷 — 如何平滑過渡的提示
description: 您發送的郵件數量對於建立良好信譽至關重要。 瞭解您可以做什麼來順利過渡。
topics: Deliverability
kt: 7055
thumbnail: kt7055.jpg
doc-type: article
activity: understand
team: ACS
exl-id: 1bc56061-0c64-4033-b49c-66618916bca6
source-git-commit: 68c403f915287e1a50cd276b67b3f48202f45446
workflow-type: tm+mt
source-wordcount: '573'
ht-degree: 1%

---

# 卷

您發送的郵件數量對於建立良好信譽至關重要。 把自己放在ISP的角度 — 如果你開始看到你不認識的人交了一大堆車，那會很令人警惕。 立即發送大量郵件是有風險的，並且肯定會導致通常難以解決的聲譽問題。 讓自己擺脫糟糕的聲譽、膨脹和阻止因過早發送過多而導致的問題，可能會讓人沮喪、費時且成本高昂。

卷閾值因ISP而異，也可能因您的平均項目指標而異。 有些發送器需要非常低且速度較慢的音量斜度，而另一些則允許音量斜度較陡。 我們建議與專家(如Adobe交付性顧問)合作，以制定自定義的卷計畫。

以下是有關如何平滑過渡的提示和提示清單：

* **權限** 是任何成功的電子郵件程式的基礎。
* **低和慢**  — 從低發送量開始，然後在您建立發件人信譽時增加。
* A **串聯郵寄策略** 允許您在使用當前ESP進行縮減時提高市場活動的容量，而不中斷電子郵件日曆。
* **參與事項**  — 從經常開啟並點擊電子郵件的訂閱者開始。
* **遵循計畫**  — 我們的建議幫助數百個「活動」客戶成功地改進了他們的電子郵件程式。
* **監視您的回復電子郵件帳戶**。 您的客戶使用noreply@xyz.com或不響應是一種糟糕的體驗。
* 非活動地址可能會產生負面的傳遞性影響。 **在當前平台上重新激活和重新權限**，而不是新IP。
* **域**  — 使用作為公司實際域的子域的發送域
   * 例如，如果您的公司域是xyz.com ，則email.xyz.com比xyzemail.com為ISP提供了更多的信譽
* **透明度**  — 您的電子郵件域的註冊詳細資訊應公開提供，不應是私有的。

在許多情況下，事務性郵件不遵循傳統的促銷升溫方法。 由於事務性郵件的性質，顯然很難控制郵件中的卷，因為通常需要用戶交互來觸發電子郵件觸碰。 在某些情況下，事務性郵件可以在沒有正式計畫的情況下進行轉換。 在其他情況下，最好隨著時間的推移轉換每種消息類型以緩慢地增大卷。 例如，您可能希望按如下方式進行過渡：

1. 購買確認 — 一般情況下訂購量較高
2. 購物車放棄 — 中 — 高訂約通常
3. 歡迎電子郵件 — 高預訂，但可能包含錯誤地址，具體取決於清單收集方法
4. Winback電子郵件 — 通常降低訂約

## 產品特定資源

**Campaign**

* 瞭解有關在與Adobe Campaign合作啟動新平台時管理可交付性的更多資訊 [此部分](/help/additional-resources/ac-starting-new-platform.md)。
* 瞭解如何使用多波次發送Adobe Campaign Classic [此部分](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/key-steps-when-creating-a-delivery/steps-sending-the-delivery.html#sending-using-multiple-waves)。
* 瞭解如何將子域完全委託給Adobe Campaign Classic或標準 [此部分](/help/additional-resources/ac-domain-name-setup.md)。
* [控制面板：完全子域委派（教程）](https://experienceleague.adobe.com/docs/campaign-classic-learn/control-panel/subdomains-and-certificates/subdomain-delegation.html) - *瞭解如何將子域完全委託給Adobe Campaign Classic。*
* [控制面板：完全子域委派（教程）](https://experienceleague.adobe.com/docs/campaign-standard-learn/control-panel/subdomains-and-certificates/subdomain-delegation.html) - *瞭解如何將子域完全委託給Adobe Campaign Standard。*

## 其他資源

* 瞭解有關通過IP升級提高電子郵件聲譽的更多資訊 [此部分](/help/additional-resources/increase-reputation-with-ip-warming.md)。
