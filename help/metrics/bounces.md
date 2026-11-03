---
title: 跳出
description: 了解不同類型的跳出。
topics: Deliverability
jira: KT-7047
thumbnail: kt7047.jpg
doc-type: article
activity: understand
team: ACS
exl-id: 6338eb67-3efd-476e-8b26-97bbb6a1d35f
TQID: https://experienceleague.adobe.com/0Gly4dAgTpfrmnFD3N-RphZf9vmctJ57fMi1hSmfMTg
product_v2:
  - id: d0a3eab4-7b10-4d96-a71e-6c0f8e7b7c87
    internal-label: CX Enterprise
  - id: dfc56824-e8b9-499e-85d4-21aedb507314
    internal-label: Campaign
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
    internal-label: Marketo Engage
feature_v2:
  - id: ea90ebee-5c84-42d9-8b21-006bdabc95a3
    internal-label: Reporting
  - id: f71e690b-4480-4b67-9ef5-88f42f9cdfdb
    internal-label: Resources
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
    internal-label: User
  - id: f8a45b24-4be7-4f1b-909b-60d06b483a20
    internal-label: Leader
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
    internal-label: Beginner
topic_v2:
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
    internal-label: Reporting
source-git-commit: e42624dbef2c89b15908bc1cf2f590b6ca347447
workflow-type: tm+mt
source-wordcount: '527'
ht-degree: 100%
---
# 跳出

退回是傳送嘗試失敗的結果，ISP 會回傳失敗通知。 退回處理是清單保健的關鍵部分。 在指定的電子郵件連續多次跳出後，此程式會將其標示為需要抑制。 觸發抑制所需的退回數量和類型因系統而異。 此過程會防止系統繼續傳送無效的電子郵件地址。 退回是 ISP 用來判斷 IP 信譽的關鍵資料之一。 留意此量度非常重要。 「已傳遞」與「已跳出」可能是衡量行銷訊息傳遞方式最常見的方式：傳遞率越高越好。

我們會調查兩種不同的跳出。

## 硬跳出

硬跳出是當 ISP 將寄送訂閱者位址的嘗試判斷為無法傳送後，產生的永久性失敗。 在 Adobe Campaign，分類為無法傳送的硬跳出會新增至隔離區，這表示不會再嘗試。 在某些情況下，如果故障原因不明，則會忽略硬跳出。
以下是一些常見的硬退回範例：

* 地址不存在
* 帳戶已停用
* 語法錯誤
* 網域錯誤

## 軟跳出

軟跳出是 ISP 在傳遞郵件時遇到的臨時故障。 軟性失敗將重試多次 (差異取決於使用自訂或現成可用的傳送設定)，以嘗試成功傳送。 在嘗試重試次數上限之前（依設定而異），不會將持續軟跳出的位址新增至隔離。 軟退回的常見原因包括：

* 郵箱已滿
* 接收電子郵件伺服器關閉
* 發件人信譽問題

![跳出類型](../assets/bounce-types.png)

>[!NOTE]
>
>退回是聲譽問題的關鍵指標，因為它們可能指出不良資料來源 (硬退回) 或 ISP 的聲譽問題 (軟退回)。
>
>軟跳出通常會在傳送電子郵件時發生，而且應允許在重試處理期間解決，才能將其定性為真正的傳遞能力問題。 如果單一 ISP 的軟跳出率大於 30%，而且在 24 小時內無法解決，建議您向您的 Adobe Campaign 傳遞顧問提出疑問。

## 產品特定資源

**Adobe Campaign Classic**

* [無法傳送的類型和原因](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-delivery-failures.html?lang=zh-Hant#delivery-failure-types-and-reasons)
* [跳出郵件管理](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-delivery-failures.html?lang=zh-Hant#bounce-mail-management)
* [無法傳遞的項目、跳出報告](https://experienceleague.adobe.com/docs/campaign-classic/using/reporting/reports-on-deliveries/global-reports.html?lang=zh-Hant#non-deliverables-and-bounces)

**Adobe Campaign Standard**

* [無法傳送的類型和原因](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/understanding-delivery-failures.html?lang=zh-Hant#delivery-failure-types-and-reasons)
* [跳出郵件鑑定](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/understanding-delivery-failures.html?lang=zh-Hant#bounce-mail-qualification)
* [跳出摘要報吿](https://experienceleague.adobe.com/docs/campaign-standard/using/reporting/list-of-reports/bounce-summary.html?lang=zh-Hant#reporting)
