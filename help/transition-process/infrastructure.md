---
title: 基礎架構
description: 了解適當地建構電子郵件基礎結構所需的條件。
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

成功的交付取決於堅實的基礎。 電子郵件基礎架構是核心要素。 正確構建的電子郵件基礎架構包括多個元件 — 即域和IP地址。 這些元件就像你發送的電子郵件背後的機器，它們通常是發送聲譽的錨。 可交付性顧問確保在實施過程中正確設定這些元素，但由於信譽元素，因此您必須瞭解這些基本資訊。

## 域設定和策略 {#domain-setup-and-strategy}

時代已經改變，一些ISP（如Gmail和雅虎）現在將域名作為將電子郵件聲譽附加到發件人身上的一個額外點。 您的域信譽基於您的發送域而不是IP地址。 這意味著在ISP過濾決策方面，您的品牌優先。

Adobe平台上新發件人的登錄過程包括設定發送域並確保正確建立基礎架構。 您應該與專家合作，瞭解您計畫長期使用哪些領域。 以下是一些可形成良好域策略的提示：

* 在您選擇的域中盡可能清晰和反映品牌，這樣用戶就不會錯誤地將郵件標識為垃圾郵件。 有些示例為seltrelt.foo.com 、 receets.foo.com等。
* 您不應使用您的父域或公司域，因為它可能會影響從您的組織向ISP發送郵件。
* 考慮使用父域的子域來合法化發送域。
* 將子域分隔為事務性和市場營銷消息類別。 這將幫助您以更可靠的方式處理電子郵件流量，因為ISP會尋找此發送方法，這是一種已知的電子郵件最佳做法，並強烈建議使用。

## IP策略 {#ip-strategy}

必須形成結構完善的智慧財產權戰略，幫助建立良好的聲譽。 IP的數量和設定會因您的業務模式和營銷目標而異。 與專家合作制定明確的從頭開始的戰略。 請考慮以下需要注意的事項：

* **IP過多** 可能會引發聲譽問題，因為垃圾郵件製造者的常見策略是 **雪鞋**&#x200B;這是垃圾郵件發送者使用的一種策略，在這些策略中，流量會在許多IP之間傳播，以最大程度地傳遞垃圾郵件。 即使您不是垃圾郵件製造者，如果您使用的IP過多，則您看起來可能就像垃圾郵件製造者一樣，尤其是當這些IP沒有任何先前的流量時。
* **IP太少** 可能導致吞吐量問題，並可能引發信譽問題。 吞吐量因ISP而異。 ISP願意接受的數量和速度通常取決於其基礎架構和發送信譽閾值。
* 為消息傳遞類型分離通信是關鍵。 至少，在單獨的IP池上，分開營銷和事務性郵件是非常重要的。
* 根據您的郵件策略，如果您的信譽大不相同，則最好將不同的產品或營銷流分離到不同的IP池上。 一些營銷商也按地區分類。 將IP與聲譽較低的通信分開不會解決信譽問題，但會防止您「聲譽良好」的電子郵件傳遞問題。 畢竟，你不想為了風險更高的觀眾而犧牲好的觀眾。

## 反饋循環 {#feedback-loops}

在幕後，Adobe平台正在處理有關投遞、投訴、不訂閱等的資料。 這些反饋環的設定是實現能力的一個重要方面。 投訴可能會損害聲譽，因此您應該通過電子郵件地址註冊目標受眾的投訴。 必須指出的是，Gmail沒有提供這些資料。 列出取消訂閱的郵件頭和項目過濾對Gmail訂閱者尤其重要，這些訂閱者現在佔據了大多數訂閱者資料庫。

## 驗證 {#authentication}

身份驗證是ISP用於驗證發送者身份的過程。 最常見的兩種身份驗證協定是 [!DNL Sender Policy Framework] (SPF)和 [!DNL DomainKeys Identified Mail] (DKIM)。 最終用戶無法看到這些內容，但ISP會幫助過濾來自驗證發件人的電子郵件。 [!DNL Domain-based Message Authentication Reporting and Conformance] (DMARC)正在普及，儘管其政策尚未被所有ISP納入其聲譽系統。

### SPF

[!DNL Sender Policy Framework] (SPF)是一種驗證方法，它允許域的所有者指定他們使用哪些郵件伺服器從該域發送郵件。

### DKIM

[!DNL Domain Keys Identified Mail] (DKIM)是一種用於檢測偽造的發送者地址（通常稱為欺騙）的驗證方法。 如果啟用了DKIM，則接收方可以確認發送方是否有權從該域發送郵件。

### DMARC

[!DNL Domain-based Message Authentication, Reporting and Conformance] (DMARC)是一種驗證方法，它允許域所有者保護其域免受未經授權的使用。 DMARC使用SPF或DKIM或兩者，以允許域所有者控制失敗驗證的郵件發生的情況：已傳送、隔離或拒絕。

## 產品特定資源

**Campaign**

* 瞭解如何將子域完全委託給Adobe Campaign Classic或標準 [此部分](/help/additional-resources/ac-domain-name-setup.md)。
* [控制面板：完全子域委派（教程）](https://experienceleague.adobe.com/docs/campaign-classic-learn/control-panel/subdomains-and-certificates/subdomain-delegation.html) - *瞭解如何將子域完全委託給Adobe Campaign Classic。*
* [控制面板：完全子域委派（教程）](https://experienceleague.adobe.com/docs/campaign-standard-learn/control-panel/subdomains-and-certificates/subdomain-delegation.html) - *瞭解如何將子域完全委託給Adobe Campaign Standard。*
* 瞭解有關在中為Campaign Classic實例實現反饋循環的詳細資訊 [此部分](/help/additional-resources/acc-technical-recommendations.md#feedback-loop-acc)。

## 其他資源

* 瞭解有關中的SPF、DKIM和DMARC驗證方法的詳細資訊 [此部分](/help/additional-resources/authentication.md)。
* 瞭解有關通過IP升級提高電子郵件聲譽的更多資訊 [此部分](/help/additional-resources/increase-reputation-with-ip-warming.md)。
