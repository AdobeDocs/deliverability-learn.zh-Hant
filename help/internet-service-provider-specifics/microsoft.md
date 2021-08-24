---
title: Microsoft（Hotmail、Outlook、Windows Live 等）
description: Microsoft通常是第二或第三大提供商，具體取決於清單的組成，它們處理的流量與其他ISP稍有不同。
topics: Deliverability
kt: 5319
doc-type: article
activity: understand
team: TM
exl-id: d706cb90-828a-4ab3-8f93-c9bd71553d63
source-git-commit: 68c403f915287e1a50cd276b67b3f48202f45446
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 1%

---

# [!DNL Microsoft] ([!DNL Hotmail]、  [!DNL Outlook]、  [!DNL Windows Live]等)

[!DNL Microsoft] 通常是第二或第三大提供者，端視您的清單組成，而且他們處理的流量確實與其他ISP稍有不同。

以下是幾項重點：

## 哪些資料很重要

[!DNL Microsoft] 著重於傳送者信譽、投訴、使用者參與，以及其自行民調問答的受信任使用者群組（也稱為傳送者信譽資料或SRD）。

## 它們提供哪些資料

[!DNL Microsoft]專屬的寄件者報告工 [!DNL Smart Network Data Services] 具(SNDS)可讓您查看您要傳送多少郵件、接受多少郵件的量度，以及投訴和垃圾郵件陷阱。請記住，共用的資料是範例，不會反映確切數字，但最能代表[!DNL Microsoft]將您視為傳送者的方式。 [!DNL Microsoft] 不會公開提供其受信任使用者群組的資訊，但該資料可透過程 [!DNL Return Path Certification] 式取得，需額外付費。

## 發件人信譽

[!DNL Microsoft] 在聲譽評估和篩選決策中，傳統上都專注於傳送IP。他們也正積極致力於擴充其傳送網域功能。 這兩者在很大程度上都受到傳統聲譽影響者的推動，比如投訴和垃圾郵件陷阱。 傳遞能力也會受到「回訪路徑認證」計畫的嚴重影響，該計畫確實有特定的定量和定性計畫要求。

## 前瞻分析

[!DNL Microsoft] 結合其所有接收網域來建立及追蹤傳送信譽。這包括[!DNL Hotmail]、[!DNL Outlook]、MSN、[!DNL Windows Live]等，以及任何公司Office 365托管電子郵件。 [!DNL Microsoft] 容量波動尤其敏感，因此，請考慮應用特定策略來從大發送進行上下調，而不是允許基於容量的突然變化。

[!DNL Microsoft] 在IP加熱的最初幾天也特別嚴格，這通常意味著大多數郵件最初都會被過濾。大多數ISP認為發送者在被證明有罪之前是無辜的。 [!DNL Microsoft] 相反，你會認為你有罪，直到你證明自己是無辜的。
