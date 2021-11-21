---
title: 開始新平台
description: 進一步了解在使用Adobe Campaign啟動新平台時如何管理傳遞能力。
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: 6c9ade01-3052-4311-af80-888294820024
source-git-commit: d6094cd2ef0a8a7741e7d8aa4db15499fad08f90
workflow-type: tm+mt
source-wordcount: '603'
ht-degree: 8%

---

# 開始新平台 {#starting-new-platform}

設定要與Adobe Campaign搭配使用的新平台時，必須維護網域和IP位址信譽。

## 敏感步驟

開始在新平台上傳送電子郵件時，請務必小心，因為該平台沒有任何使用記錄，而且若傳送的IP從未用於此目的，則沒有信譽。

ISP自然會懷疑從未用於傳送電子郵件的IP位址，而且會突然開始傳送大量電子郵件流量。 事實上，垃圾郵件製造者通常使用「未知」的IP地址（從未列入封鎖清單的地址），在檢測前發送盡可能多的郵件。

在生產階段開始時，您無法期望在輸出方面達到操作速度。 此外，您不應嘗試以此速率傳送訊息，因為這可能會導致ISP封鎖傳送地址，並嚴重損害啟動階段的其餘部分。

## 主要原則

以下列出啟動新平台時應遵循的主要原則。

* 設定專用的子網域，專用於從Adobe傳送的電子郵件促銷活動。

* 如果你有這些資訊， **將無效地址導入隔離表中**.
首次使用可能未完全限定的地址清單時，通常會啟動平台。 如果您傳送至無效地址或Honeypot地址，這將有助於降低平台的信譽。

   * 如果您有無效地址的清單，在首次發送前將其導入隔離表中符合您的最佳利益。 隔離表可通過 **[!UICONTROL Administration > Campaign Management > Non deliverables Management > Non deliverables and addresses]** (Campaign Classic)和 **[!UICONTROL Administration > Channels > Quarantines > Addresses]** (Campaign Standard)功能表。

   * 儘管如此，如果您希望重新確認無效地址，則最好在建立平台的信譽後再逐個地執行此操作，以便隨著時間的推移「稀釋」壞地址的使用。

* **限制吞吐量速率** 限制母親數量。 如需調整此類技術設定的詳細資訊，請聯絡您的Adobe Campaign管理員。

* **逐步增加發送的卷** 以避免被標示為垃圾訊息。 從頭開始，請勿鎖定整個資料庫，而是每次您傳送時，新增清單的額外部分。 這應可讓您在每個步驟增加卷，同時降低無效地址的總體速率。 為確保啟動階段的順利開發，可使用波。

* **定期傳送**. 從某種程度上說，定期傳送小鏡頭比偶爾傳送大型行銷活動更好。
* **請密切注意傳送報表**. 錯誤指標高可能表示技術設定配置錯誤。

## 其他資源

如需上述原則及其於Adobe Campaign的實作詳細資訊，請參閱下列章節：

* [透過 IP 暖身提高您的電子郵件信譽](../../help/additional-resources/increase-reputation-with-ip-warming.md)
* [關於垃圾郵件陷阱](../../help/additional-resources/all-about-spam-traps.md)

**Adobe Campaign Classic**

* [透過隔離來最佳化傳送](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-quarantine-management.html#optimizing-your-delivery-through-quarantines)
* [識別整個平台的隔離地址](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-quarantine-management.html#identifying-quarantined-addresses-for-the-entire-platform)
* [使用多波次傳送](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/key-steps-when-creating-a-delivery/steps-sending-the-delivery.html#sending-using-multiple-waves)
* [傳遞監視](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/about-delivery-monitoring.html#sending-messages)

**Adobe Campaign Standard**

* [透過隔離來最佳化傳送](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/understanding-quarantine-management.html#optimizing-your-delivery-through-quarantines)
* [識別整個平台的隔離地址](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/understanding-quarantine-management.html)
* [監視傳遞](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/monitoring-a-delivery.html)
