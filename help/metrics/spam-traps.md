---
title: 垃圾郵件陷阱
description: 瞭解不同類型的垃圾郵件陷阱。
feature: 量度
topics: Deliverability
kt: 7050
thumbnail: kt7050.jpg
doc-type: article
activity: understand
team: ACS
translation-type: tm+mt
source-git-commit: d42a8c3b06308fca0cf3e9db8d634a767fc0cdc6
workflow-type: tm+mt
source-wordcount: '422'
ht-degree: 3%

---


# 垃圾郵件陷阱

垃圾郵件陷阱有助於識別來自詐騙寄件者或未遵循電子郵件最佳實踐者的郵件。 垃圾郵件陷阱電子郵件地址通常不會公開發佈，幾乎無法識別。 向垃圾郵件陷阱發送電子郵件可能會根據陷阱類型和ISP的不同嚴重性影響您的聲譽。 在以下幾節中進一步瞭解不同類型的垃圾郵件陷阱。

## 回收

回收的垃圾郵件陷阱是曾經有效但不再使用的地址。 要盡可能保持清晰的清單，一個關鍵方法是定期傳送電子郵件至您的整個清單，並適當地抑制被拒收的電子郵件。 這有助於隔離已放棄的電子郵件地址，並阻止其繼續使用。

在某些情況下，地址可在30天內循環使用。 定期發送是良好清單衛生的重要方面，也是定期抑制不活躍用戶的重要方面。 **[重新參與](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/deliverability-management/re-engagement-best-practices.html?lang=en#sending-messages)** 促銷活動通常是複雜電子郵件行銷計畫的一部分。此促銷活動樣式可讓傳送者嘗試讓原本不會再寄回的使用者回來。

## Typo

錯字垃圾訊息陷阱是包含拼字錯誤或錯誤的位址。 這通常發生在Gmail等主要網域的已知錯誤拼字(例如：gmial是常見的錯字)。 ISP和其他區塊清單運算子將註冊已知的壞網域，以用作垃圾郵件陷阱，以識別垃圾郵件發送者並測量發送者的健康狀況。 防止錯誤垃圾訊息陷阱的最佳方式是使用&#x200B;**雙重選擇加入程式**&#x200B;來收集清單。

## 原始

原始垃圾郵件陷阱是指沒有最終用戶且從未有最終用戶的地址。 此地址的建立純粹是為了識別垃圾電子郵件。 這是最具影響力的垃圾郵件陷阱，因為幾乎無法識別，而且需要付出大量努力才能從清單中清除。 大多數攔截器利用原始垃圾郵件陷阱來列出信譽不佳的發件人。 避免原始垃圾訊息陷阱感染更廣泛行銷電子郵件清單的唯一方法，是使用&#x200B;**雙重加入程式**&#x200B;來收集清單。

## 產品特定資源

**Adobe Campaign Classic**

* [SpamAssassin](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/deliverability-management/spamassassin.html?lang=en#using-spamassassin)

**Adobe Campaign Standard**

* [預覽您的電子郵件和反垃圾郵件分析](https://experienceleague.adobe.com/docs/campaign-standard-learn/tutorials/designing-content/email-designer/preview-your-email.html#designing-content)
* [雙重加入程式](https://experienceleague.adobe.com/docs/campaign-standard/using/communication-channels/landing-pages/setting-up-a-double-opt-in-process.html?lang=en#communication-channels)

