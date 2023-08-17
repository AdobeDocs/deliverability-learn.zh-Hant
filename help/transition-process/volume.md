---
title: 數量 — 如何順暢轉換的秘訣
description: 您傳送的郵件量對建立良好的信譽至關重要。 瞭解如何才能順暢地轉變。
topics: Deliverability
jira: KT-7055
thumbnail: kt7055.jpg
doc-type: article
activity: understand
role: Admin,User
level: Beginner
team: ACS
exl-id: 1bc56061-0c64-4033-b49c-66618916bca6
source-git-commit: 6b312cdbba496818337c97ec4f42962aea757901
workflow-type: tm+mt
source-wordcount: '573'
ht-degree: 1%

---

# 卷

您傳送的郵件量對建立良好的信譽至關重要。 站在ISP的角度來看 — 如果您開始看到來自不認識的人的大量流量，將會令人擔憂。 立即傳送大量郵件是有風險的，並且肯定會造成通常難以解決的信譽問題。 如果您因為傳送太快而導致聲譽不佳、膨脹和封鎖問題，可能會令人沮喪、耗時和成本高昂。

數量臨界值會因ISP而異，也可能依您的平均參與量度而有所不同。 有些寄件者需要非常低且緩慢的音量增加，而有些則允許更陡的音量增加。 我們建議您與專家(例如Adobe傳遞能力顧問)合作，以開發自訂的容量計畫。

以下是如何順暢轉換的提示和秘訣清單：

* **許可權** 是任何成功電子郵件計畫的基礎。
* **低和慢**  — 從低傳送量開始，然後隨著您建立寄件者信譽而增加。
* A **串聯郵寄策略** 可讓您在使用目前的ESP逐步降低時提升Campaign的音量，而不會中斷您的電子郵件行事曆。
* **參與很重要**  — 從定期開啟並按一下您的電子郵件的訂閱者開始。
* **遵循計畫**  — 我們的建議已幫助數百名Campaign客戶成功提升其電子郵件計畫。
* **監視您的回覆電子郵件帳戶**. 使用noreply@xyz.com或不回應對您的客戶來說是不好的體驗。
* 非作用中地址可能對傳遞能力產生負面影響。 **重新啟用並重新取得您目前平台的許可權**&#x200B;而不是您的新IP。
* **網域**  — 使用傳送網域（貴公司實際網域的子網域）
   * 例如，如果您的公司網域是xyz.com，email.xyz.com為ISP提供的信譽會高於xyzemail.com
* **透明度**  — 您的電子郵件網域的註冊詳細資料應公開提供，且不應為私人。

在許多情況下，異動郵件不會遵循傳統的促銷暖身方法。 由於交易式郵件的性質，顯然很難控制其數量，因為它通常需要使用者互動來觸發電子郵件觸控。 在某些情況下，異動郵件可以簡單地在沒有正式計畫的情況下進行轉換。 在其他情況下，隨著時間轉換每個訊息型別可能更適合讓磁碟區緩慢增長。 例如，您可以依照以下步驟進行轉換：

1. 購買確認 — 一般而言參與度高
2. 購物車放棄 — 一般為中 — 高參與度
3. 歡迎電子郵件 — 高參與度，但可能包含錯誤地址，具體取決於您的清單收集方法
4. 遞回電子郵件 — 一般而言參與度較低

## 產品特定資源

**Campaign**

* 進一步瞭解在中使用Adobe Campaign啟動新平台時，如何管理傳遞能力 [本節](/help/additional-resources/ac-starting-new-platform.md).
* 瞭解如何搭配Adobe Campaign Classic使用多個波段進行傳送 [本節](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/key-steps-when-creating-a-delivery/steps-sending-the-delivery.html#sending-using-multiple-waves).
* 瞭解如何在中將子網域完全委派至Adobe Campaign Classic或Standard [本節](/help/additional-resources/ac-domain-name-setup.md).
* [控制面板：完整子網域委派（教學課程）](https://experienceleague.adobe.com/docs/campaign-classic-learn/control-panel/subdomains-and-certificates/subdomain-delegation.html) - *瞭解如何將子網域完全委派至Adobe Campaign Classic。*
* [控制面板：完整子網域委派（教學課程）](https://experienceleague.adobe.com/docs/campaign-standard-learn/control-panel/subdomains-and-certificates/subdomain-delegation.html) - *瞭解如何將子網域完全委派至Adobe Campaign Standard。*

## 其他資源

* 進一步瞭解透過IP暖身提高您的電子郵件信譽 [本節](/help/additional-resources/increase-reputation-with-ip-warming.md).
