---
title: 投訴
description: 瞭解當使用者指出收到不想要或未預期的電子郵件時會提出的投訴。
topics: Deliverability
jira: KT-7048
thumbnail: kt7048.jpg
doc-type: article
activity: understand
team: ACS
exl-id: 0343820d-f5af-4b8a-bcab-dbb47ae7aecb
source-git-commit: 9444f8601f2f349398ee5deb9d5f4d4f7abb44f5
workflow-type: tm+mt
source-wordcount: '276'
ht-degree: 100%

---

# 投訴

當使用者指出收到不想要或未預期的電子郵件時，就會提出投訴。當訂閱者按下垃圾訊息按鈕時，此訂閱者動作通常會透過訂閱者的電子郵件用戶端或透過第三方垃圾訊息報告系統來記錄。

## ISP 投訴

大部分第 1 層和部分第 2 層 ISP 都會為其使用者提供垃圾訊息報告方法，因為選擇退出和取消訂閱程式過去曾被惡意使用來驗證電子郵件地址。Adobe Campaign 透過 ISP FBL 收到這些投訴。這是在為任何提供 FBL 的 ISP 設定程式中建立的，可讓 Adobe Campaign 自動將投訴的電子郵件地址新增至隔離表以進行抑制。ISP 投訴的尖峰可能是清單品質不佳、清單收集方法不佳或參與政策不力的指標。當內容不相關時，也常會有相關投訴。

## 第三方投訴

有幾個反垃圾郵件群組允許在更廣的層級回報垃圾郵件。這些第三方使用的投訴量度可用來標示電子郵件內容以識別垃圾電子郵件。此過程也稱為指紋識別。這些第三方投訴方式的使用者通常對電子郵件比較熟悉，因此，如果沒有得到回應，他們可能會比其他投訴產生更大的影響。

>[!NOTE]
>
>ISP 會收集投訴，並使用投訴來判斷傳送者的整體信譽。所有申訴都應得到平息，按照當地法律法規並盡快與之聯繫。

## 產品特定資源

**Adobe Campaign Classic**

* [追蹤指標](https://experienceleague.adobe.com/docs/campaign-classic/using/reporting/reports-on-deliveries/delivery-reports.html?lang=zh-Hant#tracking-indicators)

**Adobe Campaign Standard**

* [投訴報告](https://experienceleague.adobe.com/docs/campaign-standard/using/reporting/list-of-reports/complaints.html?lang=zh-Hant#reporting)
