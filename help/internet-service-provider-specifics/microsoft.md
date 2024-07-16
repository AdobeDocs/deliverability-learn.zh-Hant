---
title: Microsoft（Hotmail、Outlook、Windows Live 等）
description: 根據您清單的組成，Microsoft通常是第二或第三大提供者，而且他們處理的流量與其他ISP略有不同。
topics: Deliverability
jira: KT-5319
doc-type: article
activity: understand
role: Admin, Leader, User
level: Beginner
team: TM
exl-id: d706cb90-828a-4ab3-8f93-c9bd71553d63
source-git-commit: 6b312cdbba496818337c97ec4f42962aea757901
workflow-type: tm+mt
source-wordcount: '335'
ht-degree: 1%

---

# [!DNL Microsoft] （[!DNL Hotmail]、[!DNL Outlook]、[!DNL Windows Live]等）

根據您清單的組成，[!DNL Microsoft]通常為第二大或第三大提供者，他們的確處理的是與其他ISP稍微不同的流量。

以下是一些重點事項：

## 哪些資料重要

[!DNL Microsoft]著重於寄件者信譽、投訴、使用者參與，以及他們自己輪詢意見的受信任使用者群組（也稱為寄件者信譽資料或SRD）。

## 他們提供哪些資料

[!DNL Microsoft]的專屬寄件者報告工具[!DNL Smart Network Data Services] (SNDS)可讓您檢視關於您傳送多少郵件、接受多少郵件，以及投訴和垃圾郵件陷阱的量度。 請記住，共用的資料是範例，不會反映確切數字，但最能代表[!DNL Microsoft]如何將您視為寄件者。 [!DNL Microsoft]未公開提供其受信任使用者群組的資訊，但可透過[!DNL Return Path Certification]程式取得該資料，但需額外付費。

## 寄件者信譽

[!DNL Microsoft]傳統上專注於在他們的信譽評估和篩選決定中傳送IP。 他們也在積極擴展其傳送網域功能。 兩者主要受到投訴和垃圾郵件陷阱等傳統信譽影響者的推動。 Return Path Certification計畫也會嚴重影響傳遞能力，該計畫具有特定的定量和定性計畫要求。

## Insights

[!DNL Microsoft]結合其所有接收網域，以建立及追蹤傳送信譽。 這包括[!DNL Hotmail]、[!DNL Outlook]、MSN、[!DNL Windows Live]等，以及任何公司Office 365裝載的電子郵件。 [!DNL Microsoft]可能對磁碟區的波動特別敏感，所以請考慮套用特定策略來從大型傳送中提升和降低，而不是允許以磁碟區為基礎的突然變更。

[!DNL Microsoft]在IP暖機的最初幾天也特別嚴格，這通常表示大多數郵件一開始都會被篩選。 大部分的ISP都會將寄件者視為無罪，直到被證實有罪為止。 [!DNL Microsoft]則相反，在您證明自己無罪之前，會認為您有罪。
