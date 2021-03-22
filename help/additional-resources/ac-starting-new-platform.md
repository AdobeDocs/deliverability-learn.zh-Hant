---
title: 啟動新平台
description: 進一步瞭解在與Adobe Campaign合作建立新平台時如何管理可傳遞性。
feature: 付諸實踐
topics: Deliverability
kt: null
thumbnail: null
doc-type: article
activity: understand
team: ACS
translation-type: tm+mt
source-git-commit: 1e539b5df54250a5927701009e7a9c84e5d73fae
workflow-type: tm+mt
source-wordcount: '607'
ht-degree: 3%

---


# 啟動新平台 {#starting-new-platform}

在建立可與Adobe Campaign搭配使用的新平台時，維護您的網域和IP位址信譽至關重要。

## 敏感步驟

在開始在新平台上傳送電子郵件時，您應該非常小心，因為該平台沒有任何使用記錄，而且當傳送的IP從未用於此目的時，您也沒有信譽。

ISP自然會懷疑從未用於傳送電子郵件的IP位址，而且會突然開始傳送大量電子郵件流量。 事實上，垃圾郵件發送者通常使用「未知」的IP位址（從未列在密鑰清單上的位址），在偵測前傳送最多的訊息。

在生產階段開始時，您無法期望在產出方面達到操作速度。 此外，您不應嘗試以此速率發送消息，因為這可能導致ISP阻塞發送地址，並嚴重危害啟動階段的其他階段。

## 主要原則

以下列出啟動新平台時應遵循的主要原則。

* 設定專屬於從Adobe傳送之電子郵件促銷活動的子網域。

* 如果您有此資訊， **將無效地址導入隔離表**。
首次使用地址清單且可能無法完全限定的平台時，通常會啟動平台。 如果您傳送至無效的位址或蜜罐位址，這將會降低平台的聲譽。

   * 如果您有無效地址清單，在第一次發送前將其導入隔離表符合您的最大利益。 隔離表可通過&#x200B;**[!UICONTROL Administration > Campaign Management > Non deliverables Management > Non deliverables and addresses]**(Campaign Classic)和&#x200B;**[!UICONTROL Administration > Channels > Quarantines > Addresses]**(Campaign Standard)菜單獲得。

   * 但是，如果您希望重新命名無效地址，則最好在平台信譽建立後再逐個逐個地執行此操作，以便隨著時間的推移「稀釋」不良地址的使用。

* **通過限制匹** 配數來限制吞吐率。如需有關調整此技術設定的詳細資訊，請洽詢您的Adobe Campaign管理員。

* **逐步增加卷，以** 避免標籤為垃圾郵件。從頭開始不要將整個資料庫作為目標，而是每次傳送時新增清單的一部分。 這應可讓您在每個步驟增加音量，同時降低無效地址的總體速率。 為確保啟動階段的順利開發，您可使用波。

* **定期傳送**。從某種程度上講，定期傳送小鏡頭比偶爾傳送大型宣傳更好。
* **密切關注傳送報表**。高錯誤指示燈可能表示技術設定配置不當。

## 其他資源

有關上述原則及其與Adobe Campaign的執行情況的更多資訊，請參見以下各節：

* [透過IP變暖提高您的電子郵件聲譽](../../help/additional-resources/increase-reputation-with-ip-warming.md)
* [關於垃圾郵件陷阱](../../help/additional-resources/all-about-spam-traps.md)

**Adobe Campaign Classic**

* [透過隔離來最佳化傳送](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-quarantine-management.html#optimizing-your-delivery-through-quarantines)
* [識別整個平台的隔離地址](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-quarantine-management.html#identifying-quarantined-addresses-for-the-entire-platform)
* [使用多波發送](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/key-steps-when-creating-a-delivery/steps-sending-the-delivery.html#sending-using-multiple-waves)
* [傳送監控](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/about-delivery-monitoring.html#sending-messages)

**Adobe Campaign Standard**

* [透過隔離來最佳化傳送](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/understanding-quarantine-management.html#optimizing-your-delivery-through-quarantines)
* [識別整個平台的隔離地址](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/understanding-quarantine-management.html)
* [監視](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/monitoring-a-delivery.html)
