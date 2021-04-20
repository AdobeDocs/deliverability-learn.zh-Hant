---
title: 即時黑洞清單
description: 瞭解維護可能由垃圾郵件發送者使用之IP位址和網域清單的組織。
feature: Additional resources
topics: Deliverability
kt: null
thumbnail: null
doc-type: article
activity: understand
team: ACS
translation-type: tm+mt
source-git-commit: 96ed84da391faaabd3001ddd6a411ddc1f46b033
workflow-type: tm+mt
source-wordcount: '409'
ht-degree: 0%

---


# 即時黑洞清單

有些組織會維護據稱由垃圾郵件發送者使用的IP位址和網域資料庫。 諮詢這些網站有助於瞭解為何某些訊息會被拒絕為垃圾訊息。 通常可請求移除錯誤地添加到這些清單的地址。

這些資料庫稱為RBL（即時黑洞清單），並透過DNS機制進行查詢。 RBL有三種類型：

* 依IP位址：列出發送垃圾郵件或可能中繼垃圾郵件的IP地址。
* 按發件人域：列出傳送垃圾郵件或設定錯誤的傳送者網域（彈回數郵件位址的完整網域）。
* 依網域：列出在垃圾郵件內容所含連結和影像的URL中找到的網域（在註冊者註冊的高階網域）。 在Adobe解決方案中，要考慮的域通常是用於跟蹤的地址。

以下是最廣泛使用的RBL清單。 如需更完整的清單，請參閱[https://www.dnsstuff.com/](https://tools.dnsstuff.com/)。

* **Spamhaus**

   請參閱[https://www.spamhaus.org/](https://www.spamhaus.org/)

   資料庫更重要。 列入這份名單通常是一種嚴重情況。 如果發生此情況，您必須立即採取行動並警告商業服務、交付性和[Adobe客戶服務](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html)。

* **SpamCop**

   請參閱[https://www.spamcop.net/](https://www.spamcop.net/)

   它是最著名的資料庫之一。 如果您的其中一個IP位址放在此清單上，這通常表示SpamCop使用者已將您的訊息宣告為垃圾訊息，或您已將訊息傳送至SpamCop蜜罐。

* **URIBL**

   請參閱[https://www.uribl.com/](https://www.uribl.com/)

   此清單標識在宣告為垃圾郵件的郵件中定期顯示的域。 如果您的網域出現在此清單中，可能會大幅影響您的傳遞能力。 您應立即通知交付能力服務和[Adobe客戶服務](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html)。

* **SURBL**

   請參閱[http://www.surbl.org/](http://www.surbl.org/)

   SURBL可識別定期出現在垃圾訊息中的網站。 如果您的網域出現在此清單中，可能會大幅影響您的傳遞能力。 您應立即通知交付能力服務和[Adobe客戶服務](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html)。

* **iX Manitu**

   這是一份IP清單，在德國廣為使用。 請參閱[https://www.heise.de/ix/nixspam/](https://www.heise.de/ix/nixspam/)

<!--* SORBS

  [https://www.nl.sorbs.net](https://www.nl.sorbs.net) compiles a list of IP addresses that are reputed to be dynamic IP address (i.e. attributed temporarily to ISP subscribers) or "open relay" addresses. Certain domains check whether the IP address of a sender is not listed on this site before accepting email. Checking the IP addresses on this site can prove useful.-->
