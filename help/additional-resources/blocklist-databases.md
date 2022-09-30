---
title: 即時黑洞清單
description: 了解哪些組織會維護可能被垃圾郵件製造者使用的IP位址和網域清單。
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: 4155b89f-a636-404c-8951-563c1b4d0289
source-git-commit: e7427d6109f3201affa58decde36294d1631bf5b
workflow-type: tm+mt
source-wordcount: '407'
ht-degree: 6%

---

# 即時黑洞清單

有些組織維護據稱被垃圾郵件製造者使用的IP地址和域的資料庫。 諮詢這些網站有助於了解為何某些訊息被拒絕為垃圾訊息。 通常可以請求刪除錯誤地添加到這些清單中的地址。

這些資料庫稱為RBL（即時黑洞清單），並透過DNS機制來查詢。 RBL有三種類型：

* 按IP地址：列出發送垃圾郵件或可能中繼垃圾郵件的IP地址。
* 按發件人域：列出發送垃圾郵件或配置不正確的發件人域（退信地址的完整域）。
* 按Web域：列出在垃圾郵件內容中包含的連結和影像的URL中找到的域（註冊為註冊者的高級域）。 在Adobe解決方案中，要考慮的網域通常是用於追蹤的位址。

以下是最常使用RBL的清單。 如需更完整的清單，您可以參閱 [https://www.dnsstuff.com/](https://tools.dnsstuff.com/).

* **斯帕姆豪斯**

   請參閱 [https://www.spamhaus.org/](https://www.spamhaus.org/)

   資料庫更重要。 被列入這份名單通常是一種嚴重的情況。 如果發生此情況，您必須立即採取行動，並警告商業服務、傳遞能力，以及 [Adobe客戶服務](https://helpx.adobe.com/tw/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html).

* **SpamCop**

   請參閱 [https://www.spamcop.net/](https://www.spamcop.net/)

   它是最著名的資料庫之一。 如果您的其中一個IP地址被放在此清單上，這通常表示SpamCop用戶已將您的郵件聲明為垃圾郵件，或者您已將郵件發送到SpamCop蜜罐。

* **URIBL**

   請參閱 [https://www.uribl.com/](https://www.uribl.com/)

   此清單可識別經常出現在宣告為垃圾訊息中的網域。 如果您的網域出現在此清單中，可能會顯著影響您的傳遞能力。 您應通知傳遞服務，並 [Adobe客戶服務](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html) 立即。

* **SURBL**

   請參閱 [https://surbl.org/](https://surbl.org/)

   SURBL識別經常出現垃圾郵件的網站。 如果您的網域出現在此清單中，可能會顯著影響您的傳遞能力。 您應通知傳遞服務，並 [Adobe客戶服務](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html) 立即。

* **iX馬尼圖**

   這是一份IP清單，在德國廣泛使用。 請參閱 [https://www.heise.de/ix/nixspam/](https://www.heise.de/ix/nixspam/)

<!--* SORBS

  [https://www.nl.sorbs.net](https://www.nl.sorbs.net) compiles a list of IP addresses that are reputed to be dynamic IP address (i.e. attributed temporarily to ISP subscribers) or "open relay" addresses. Certain domains check whether the IP address of a sender is not listed on this site before accepting email. Checking the IP addresses on this site can prove useful.-->
