---
title: 即時黑洞清單
description: 瞭解維護可能被垃圾郵件製造者使用的IP地址和域清單的組織。
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

一些組織維護據稱由垃圾郵件製造者使用的IP地址和域的資料庫。 咨詢這些站點對於瞭解為什麼某些郵件被拒絕為垃圾郵件非常有用。 通常可以請求刪除錯誤地添加到這些清單的地址。

這些資料庫稱為RBL（即時黑洞清單），並通過DNS機制進行查詢。 RBL有三種類型：

* 按IP地址：列出發送垃圾郵件或可能中繼垃圾郵件的IP地址。
* 按發件人域：列出發送垃圾郵件或配置不正確的發件人域（反彈郵件地址的完整域）。
* 按Web域：列出在垃圾郵件內容中包含的連結和影像的URL中找到的域（在註冊器中註冊的高級域）。 在Adobe解決方案中，要考慮的域通常是用於跟蹤的地址。

以下是使用最廣泛的RBL的清單。 有關更全面的清單，請參閱 [https://www.dnsstuff.com/](https://tools.dnsstuff.com/)。

* **斯班豪斯**

   請參閱 [https://www.spamhaus.org/](https://www.spamhaus.org/)

   資料庫更重要。 被列入這份名單通常是嚴重的情況。 如果發生這種情況，您必須立即採取行動並警告商業服務、可交付性和 [Adobe客戶關懷](https://helpx.adobe.com/tw/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html)。

* **垃圾郵件**

   請參閱 [https://www.spamcop.net/](https://www.spamcop.net/)

   它是最著名的資料庫之一。 如果您的IP地址之一被放在此清單上，這通常意味著SpamCop用戶已將您的郵件聲明為Spam或您已將郵件發送到SpamCop蜜罐。

* **烏里布爾**

   請參閱 [https://www.uribl.com/](https://www.uribl.com/)

   此清單標識定期出現在聲明為垃圾郵件的郵件中的域。 如果域出現在此清單中，則會顯著影響您的可傳送性。 您應通知交付服務和 [Adobe客戶關懷](https://helpx.adobe.com/tw/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html) 立即。

* **蘇爾布**

   請參閱 [https://surbl.org/](https://surbl.org/)

   SURBL標識定期出現垃圾郵件的網站。 如果域出現在此清單中，則會顯著影響您的可傳送性。 您應通知交付服務和 [Adobe客戶關懷](https://helpx.adobe.com/tw/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html) 立即。

* **iX馬尼圖**

   這是一份IP清單，在德國廣泛使用。 請參閱 [https://www.heise.de/ix/nixspam/](https://www.heise.de/ix/nixspam/)

<!--* SORBS

  [https://www.nl.sorbs.net](https://www.nl.sorbs.net) compiles a list of IP addresses that are reputed to be dynamic IP address (i.e. attributed temporarily to ISP subscribers) or "open relay" addresses. Certain domains check whether the IP address of a sender is not listed on this site before accepting email. Checking the IP addresses on this site can prove useful.-->
