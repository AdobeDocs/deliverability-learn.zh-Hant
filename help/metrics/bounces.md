---
title: 跳出數
description: 瞭解不同類型的彈回數。
feature: 量度
topics: Deliverability
kt: 7047
thumbnail: kt7047.jpg
doc-type: article
activity: understand
team: ACS
translation-type: tm+mt
source-git-commit: 283f1cb2bb40818e11daa1a3753e8428b47e08ee
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 4%

---


# 跳出數

彈回數是ISP提供返回故障通知的傳送嘗試和失敗的結果。 反彈處理是清單衛生的重要部分。 在指定的電子郵件連續多次反彈後，此程式會將其標籤為要抑制。 觸發抑制所需的彈回數和類型因系統而異。 此程式會防止系統繼續傳送無效的電子郵件地址。 彈回數是ISP用來判斷IP信譽的關鍵資料之一。 留意此量度非常重要。 「已傳遞」與「已反彈」可能是衡量行銷訊息傳遞方式最常見的方式：交付率越高越好。

我們會調查兩種不同的彈回。

## 硬彈回數

硬彈回數是當ISP將寄送訂閱者位址的嘗試判斷為無法傳送後，產生的永久性失敗。 在Adobe Campaign，分類為無法傳送的硬彈回會新增至隔離區，這表示不會再嘗試。 在某些情況下，如果故障原因不明，則會忽略硬反彈。
以下是一些常見的硬彈回數範例：

* 地址不存在
* 帳戶已停用
* 語法錯誤
* 壞域

## 柔和彈回數

軟跳轉是ISP在發送郵件時遇到的臨時故障。 軟失敗將重試多次（如有差異，取決於使用自訂或現成可用的傳送設定），以嘗試成功傳送。 在嘗試重試次數上限之前（依設定而異），不會將持續軟彈回數的位址新增至隔離。 軟體彈回數的常見原因包括：

* 郵箱已滿
* 接收電子郵件伺服器關閉
* 發件人信譽問題

![反彈類型](../assets/bounce-types.png)

>[!NOTE]
>
>彈回數是聲譽問題的關鍵指標，因為它們可以強調不良資料來源（硬反彈）或ISP的信譽問題（軟反彈）。
>
>軟體彈回數通常會在傳送電子郵件時發生，而且應允許在重試處理期間解決，才能將其定性為真正的傳遞能力問題。 如果單一ISP的軟反彈率大於30%，而且在24小時內無法解決，建議您向您的Adobe Campaign交付能力顧問提出疑問。

## 產品特定資源

**Adobe Campaign Classic**

* [傳送失敗類型和原因](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-delivery-failures.html#delivery-failure-types-and-reasons)
* [彈回郵件管理](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-delivery-failures.html#bounce-mail-management)
* [非交付項和彈回報報告](https://experienceleague.adobe.com/docs/campaign-classic/using/reporting/reports-on-deliveries/global-reports.html#non-deliverables-and-bounces)

**Adobe Campaign Standard**

* [傳送失敗類型和原因](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/understanding-delivery-failures.html#delivery-failure-types-and-reasons)
* [退回郵件資格](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/understanding-delivery-failures.html#bounce-mail-qualification)
* [反彈摘要報表](https://experienceleague.adobe.com/docs/campaign-standard/using/reporting/list-of-reports/bounce-summary.html?lang=en#reporting)
