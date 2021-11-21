---
title: 基礎架構
description: '了解適當地建構電子郵件基礎結構所需的條件。 '
topics: Deliverability
kt: 7052
thumbnail: kt7052.jpg
doc-type: article
activity: understand
team: ACS
exl-id: 4025d95c-cc77-4e0c-9904-aaf60019b18c
source-git-commit: 68c403f915287e1a50cd276b67b3f48202f45446
workflow-type: tm+mt
source-wordcount: '910'
ht-degree: 2%

---

# 基礎架構

成功的傳遞取決於堅實的基礎。 電子郵件基礎架構是核心元素。 正確構建的電子郵件基礎結構包括多個元件 — 即域和IP地址。 這些元件就像您傳送電子郵件背後的機器，通常是傳送信譽的錨。 傳遞能力顧問可確保這些元素在實施期間正確設定，但由於信譽元素，您必須具備此基本了解。

## 域設定和策略 {#domain-setup-and-strategy}

時代已經改變，一些ISP（如Gmail和Yahoo）現在將網域信譽作為將電子郵件信譽附加至發件人的附加點。 您的網域信譽取決於您的傳送網域，而非IP位址。 這表示在ISP篩選決策方面，您的品牌優先。

Adobe平台上新發件人的上線流程包括設定您的發送域並確保正確建立您的基礎架構。 您應與專家合作，共同探討您長遠打算使用哪些網域。 以下是形成良好域策略的一些提示：

* 使用您選擇的網域，盡可能清楚並反映品牌，以免使用者將郵件誤識為垃圾訊息。 有些範例包括電子報.foo.com、receipts.foo.com等。
* 您不應使用您的父網域或公司網域，因為這可能影響從您的組織傳送郵件至ISP的作業。
* 請考慮使用父網域的子網域來將傳送網域合法化。
* 將您的子網域分隔為「交易」和「行銷」訊息類別。 這可協助您的電子郵件流量以更可靠的方式流動，因為ISP會尋找此傳送方法，這是已知的電子郵件最佳作法，並強烈建議使用。

## IP策略 {#ip-strategy}

制定結構完善的智慧財產權戰略有助於建立良好的聲譽，這一點非常重要。 IP的數量和設定會依您的商業模式和行銷目標而有所不同。 與專家合作，制定一個明確的戰略，從頭開始。 請考量下列需要注意的重要事項：

* **IP太多** 會觸發聲譽問題，因為這是垃圾郵件製造者的常見策略 **雪鞋**，此策略是垃圾郵件發送者使用的策略，在此策略中，流量會分佈在許多IP上，以最大化垃圾郵件的傳送。 即使您不是垃圾郵件發送者，如果您使用太多IP，尤其是如果這些IP之前沒有任何流量，您可能看起來就像一個垃圾郵件發送者。
* **IP太少** 可能會導致輸送量問題，並可能觸發信譽問題。 吞吐量因ISP而異。 ISP願意接受的數量和速度通常取決於其基礎架構和發送信譽閾值。
* 分離訊息類型的流量是關鍵。 至少要在單獨的IP池上獨立地分開行銷和交易式郵件，這一點非常重要。
* 根據您的郵件策略，如果您的信譽大不相同，建議您將不同的產品或營銷流分隔到不同的IP池上。 有些行銷人員也會依地區劃分。 將信譽較低的流量IP分離，不會修正信譽問題，但可防止傳送「良好」信譽的電子郵件時發生問題。 畢竟，你不想為了風險更高而犧牲好受眾。

## 反饋循環 {#feedback-loops}

在幕後，Adobe平台會處理關於退信、投訴、取消訂閱等的資料。 這些意見回圈的設定是傳遞能力的重要方面。 投訴可能會損害聲譽，因此您應該透過電子郵件地址註冊目標受眾的投訴。 請務必注意，Gmail不會提供這些資料。 清單取消訂閱的標題和參與篩選對Gmail訂閱者尤其重要，因為現在大部分訂閱者資料庫都是訂閱者。

## 驗證 {#authentication}

驗證是ISP用來驗證寄件者身分的程式。 最常見的兩種驗證協定是 [!DNL Sender Policy Framework] (SPF)和 [!DNL DomainKeys Identified Mail] (DKIM)。 最終用戶看不到這些郵件，但ISP可以過濾來自已驗證發件人的電子郵件。 [!DNL Domain-based Message Authentication Reporting and Conformance] (DMARC)越來越受歡迎，儘管其政策尚未被所有ISP納入其聲譽體系。

### SPF

[!DNL Sender Policy Framework] (SPF)是一種驗證方法，允許域的所有者指定他們使用哪些郵件伺服器從該域發送郵件。

### DKIM

[!DNL Domain Keys Identified Mail] (DKIM)是一種驗證方法，用於偵測偽造的寄件者位址（通常稱為欺騙）。 如果啟用DKIM，則允許接收者確認寄件者是否獲得從該網域傳送郵件的授權。

### DMARC

[!DNL Domain-based Message Authentication, Reporting and Conformance] (DMARC)是一種驗證方法，可讓網域擁有者保護其網域不遭未經授權的使用。 DMARC使用SPF或DKIM或兩者，以允許域所有者控制驗證失敗的郵件發生的情況：已傳送、隔離或已拒絕。

## 產品特定資源

**Campaign**

* 了解如何將子網域完全委派至Adobe Campaign Classic或Standard，於 [本節](/help/additional-resources/ac-domain-name-setup.md).
* [控制面板：完整子網域委派（教學課程）](https://experienceleague.adobe.com/docs/campaign-classic-learn/control-panel/subdomains-and-certificates/subdomain-delegation.html) - *了解如何將子網域完全委派至Adobe Campaign Classic。*
* [控制面板：完整子網域委派（教學課程）](https://experienceleague.adobe.com/docs/campaign-standard-learn/control-panel/subdomains-and-certificates/subdomain-delegation.html) - *了解如何將子網域完全委派至Adobe Campaign Standard。*
* 進一步了解如何在中為Campaign Classic例項實作意見回圈 [本節](/help/additional-resources/acc-technical-recommendations.md#feedback-loop-acc).

## 其他資源

* 深入了解SPF、DKIM和DMARC驗證方法，位於 [本節](/help/additional-resources/authentication.md).
* 進一步了解如何透過 [本節](/help/additional-resources/increase-reputation-with-ip-warming.md).
