---
title: Microsoft（Hotmail、Outlook、Windows Live 等）
description: Microsoft通常是第二或第三大提供商，具體取決於您清單的構成，它們處理的流量與其他ISP稍有不同。
topics: Deliverability
kt: 5319
doc-type: article
activity: understand
team: TM
exl-id: d706cb90-828a-4ab3-8f93-c9bd71553d63
source-git-commit: 68c403f915287e1a50cd276b67b3f48202f45446
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 2%

---

# [!DNL Microsoft] ([!DNL Hotmail]。 [!DNL Outlook]。 [!DNL Windows Live]等)

[!DNL Microsoft] 通常是第二或第三大提供商，具體取決於您清單的構成，它們處理的流量與其他ISP稍有不同。

以下是一些亮點：

## 哪些資料重要

[!DNL Microsoft] 重點介紹發件人信譽、投訴、用戶參與以及他們自己的受信任用戶組（也稱為發件人信譽資料或SRD），這些用戶對其進行輪詢以獲得反饋。

## 他們提供哪些資料

[!DNL Microsoft]專有發件人報告工具， [!DNL Smart Network Data Services] (SNDS)，讓您查看有關您發送的郵件數量和接受的郵件數量的指標，以及投訴和垃圾郵件陷阱。 請記住，共用的資料是一個示例，不反映確切的數字，但它最能代表 [!DNL Microsoft] 將您視為發件人。 [!DNL Microsoft] 不公開提供有關其受信任用戶組的資訊，但資料可通過 [!DNL Return Path Certification] 額外收費。

## 發件人信譽

[!DNL Microsoft] 傳統上，IP的重點是在信譽評估和篩選決策中發送IP。 他們也在積極努力擴展其發送域功能。 這兩種做法在很大程度上都受到傳統聲譽影響者的推動，比如投訴和垃圾郵件陷阱。 返迴路徑認證方案也會嚴重影響交付能力，該方案確實有具體的定量和定性方案要求。

## Insights

[!DNL Microsoft] 合併所有接收域來建立和跟蹤發送信譽。 這包括 [!DNL Hotmail]。 [!DNL Outlook], MSN, [!DNL Windows Live]、等等，以及任何公司的Office 365都承載電子郵件。 [!DNL Microsoft] 可能對卷的波動特別敏感，因此請考慮應用特定策略來從大發送端上下放大，而不是允許基於卷的突然變化。

[!DNL Microsoft] 在IP升溫的最初幾天也特別嚴格，這通常意味著大多數郵件最初都會被過濾。 大多數ISP認為發送者在被證明有罪之前是無辜的。 [!DNL Microsoft] 相反，在證明自己無罪之前，你會認為自己有罪。
