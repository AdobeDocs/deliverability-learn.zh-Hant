---
title: 開始新平台
description: 在與Adobe Campaign開始新平台時，瞭解有關管理可交付性的更多資訊。
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

在設定新平台以與Adobe Campaign配合使用時，維護您的域和IP地址信譽至關重要。

## 敏感步驟

在開始在新平台上發送電子郵件時，您應當非常小心，因為該平台沒有任何使用歷史，並且當發送的IP從未用於此目的時，沒有信譽。

ISP自然會懷疑從未用來發送電子郵件的IP地址，這些地址突然開始發送大量電子郵件。 事實上，垃圾郵件製造者通常使用「未知」 IP地址（從未列在密鑰清單上的地址）在檢測前發送盡可能多的郵件。

在生產階段開始時，您不能期望在輸出方面達到操作速度。 此外，您不應嘗試以此速率發送消息，因為這可能會導致ISP阻止發送地址並嚴重危害啟動階段的其餘部分。

## 主要原則

下面列出了啟動新平台時應遵循的主要原則。

* 配置專用子域，該子域特定於從Adobe發送的電子郵件活動。

* 如果你有這些資訊， **將無效地址導入隔離表**。
首次使用地址清單時，啟動平台通常會發生，這些地址清單可能未完全限定。 如果發送到無效地址或蜜罐地址，這將會降低平台的信譽。

   * 如果您有無效地址清單，則在首次發送之前將其導入到隔離表中符合您的最大利益。 隔離表可通過 **[!UICONTROL Administration > Campaign Management > Non deliverables Management > Non deliverables and addresses]** (Campaign Classic) **[!UICONTROL Administration > Channels > Quarantines > Addresses]** (Campaign Standard)菜單。

   * 儘管如此，如果您希望重新指定無效地址，則最好在建立平台信譽並逐個地這樣做，以便隨著時間的推移「稀釋」使用錯誤地址。

* **限制吞吐率** 通過限制匹配數。 有關調整此類技術設定的詳細資訊，請與Adobe Campaign管理員聯繫。

* **逐步增加發送的卷** 以免被標籤為垃圾郵件。 不要從一開始就針對整個資料庫，而是每次發送時都添加清單的一部分。 這樣，您就可以在每一步增加卷，同時降低無效地址的總體速率。 為確保啟動階段的順利開發，可以使用波。

* **定期發送**。 從某種程度上講，定期小打小打比偶打大打，
* **密切注意交貨報告**。 高錯誤指示燈可能表示技術設定配置不當。

## 其他資源

有關上述原則及其在Adobe Campaign的執行情況的更多資訊，見以下各節：

* [透過 IP 暖身提高您的電子郵件信譽](../../help/additional-resources/increase-reputation-with-ip-warming.md)
* [關於垃圾郵件陷阱](../../help/additional-resources/all-about-spam-traps.md)

**Adobe Campaign Classic**

* [通過隔離優化您的交貨](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-quarantine-management.html#optimizing-your-delivery-through-quarantines)
* [確定整個平台的隔離地址](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-quarantine-management.html#identifying-quarantined-addresses-for-the-entire-platform)
* [使用多波發送](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/key-steps-when-creating-a-delivery/steps-sending-the-delivery.html#sending-using-multiple-waves)
* [傳遞監視](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/about-delivery-monitoring.html?lang=zh-Hans#sending-messages)

**Adobe Campaign Standard**

* [通過隔離優化您的交貨](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/understanding-quarantine-management.html#optimizing-your-delivery-through-quarantines)
* [確定整個平台的隔離地址](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/understanding-quarantine-management.html)
* [監視傳遞](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/monitoring-a-delivery.html?lang=zh-Hant)
