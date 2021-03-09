---
title: 投訴
description: '瞭解當使用者指出電子郵件不想要或未預期時，會註冊的投訴。 '
feature: 量度
topics: Deliverability
kt: 7048
thumbnail: kt7048.jpg
doc-type: article
activity: understand
team: ACS
translation-type: tm+mt
source-git-commit: d42a8c3b06308fca0cf3e9db8d634a767fc0cdc6
workflow-type: tm+mt
source-wordcount: '276'
ht-degree: 2%

---


# 投訴

當使用者指出電子郵件不需要或未預期時，就會註冊投訴。 當訂閱者按下垃圾訊息按鈕時，此訂閱者動作通常會透過訂閱者的電子郵件用戶端或透過第三方垃圾訊息報告系統來記錄。

## ISP投訴

大部分第1層和部分第2層ISP都會為其使用者提供垃圾訊息報告方法，因為選擇退出和取消訂閱程式過去曾惡意使用來驗證電子郵件地址。 Adobe Campaign透過ISP FBL收到這些投訴。 這是在為任何提供FBL的ISP設定程式中建立的，可讓Adobe Campaign自動將投訴的電子郵件地址新增至隔離表以進行抑制。 ISP投訴的尖峰可能是清單品質不佳、清單收集方法不佳或參與政策不力的指標。 當內容不相關時，也常會注意到這些內容。

## 第三方投訴

有幾個反垃圾郵件群組允許在更廣的層級報告垃圾郵件。 這些第三方使用的投訴量度可用來標籤電子郵件內容以識別垃圾電子郵件。 此程式也稱為指紋。 這些第三方投訴方式的使用者通常對電子郵件比較熟悉，因此，如果沒有得到回應，他們可能會比其他投訴產生更大的影響。

>[!NOTE]
>
>ISP會收集投訴，並使用投訴來判斷傳送者的整體信譽。 所有申訴都應受到壓制，不應再按照當地法律法規盡快與之聯繫。

## 產品特定資源

**Adobe Campaign Standard**

* [投訴](https://experienceleague.adobe.com/docs/campaign-standard/using/reporting/list-of-reports/complaints.html#reporting)