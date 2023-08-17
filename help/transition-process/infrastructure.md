---
title: 基礎架構
description: 了解適當地建構電子郵件基礎結構所需的條件。
topics: Deliverability
jira: KT-7052
thumbnail: kt7052.jpg
doc-type: article
activity: understand
role: Admin, Leader
level: Beginner
team: ACS
exl-id: 4025d95c-cc77-4e0c-9904-aaf60019b18c
source-git-commit: 6b312cdbba496818337c97ec4f42962aea757901
workflow-type: tm+mt
source-wordcount: '910'
ht-degree: 2%

---

# 基礎架構

成功傳送取決於堅實的基礎。 電子郵件基礎架構是核心元素。 正確建構的電子郵件基礎結構包括多個元件，即網域和IP位址。 這些元件就像您傳送之電子郵件背後的機器，通常是傳送信譽的錨點。 傳遞能力顧問會確保在實作期間已正確設定這些元素，但由於信譽元素，因此您必須瞭解此基本知識。

## 網域設定和策略 {#domain-setup-and-strategy}

時代變了，有些ISP （例如Gmail和Yahoo）現在將網域信譽合併為附加至寄件者電子郵件信譽的額外點。 您的網域信譽是以您的傳送網域為基礎，而不是以您的IP位址為基礎。 這表示在ISP篩選決定方面，您的品牌優先。

Adobe平台上新寄件者的上線流程包括設定您的傳送網域，並確保您的基礎架構已正確建立。 您應與專家合作，瞭解您計畫長期使用哪些網域。 以下是一些可塑造良好網域策略的秘訣：

* 在您選擇的網域中儘可能清晰地反映品牌，讓使用者不會將郵件錯誤地識別為垃圾郵件。 範例包括newsletter.foo.com、receipts.foo.com等。
* 您不應使用您的上層或公司網域，因為這會影響從您的組織傳送郵件給ISP。
* 請考慮使用上層網域的子網域，將您的傳送網域合法化。
* 針對異動和行銷訊息類別，區分您的子網域。 當ISP尋找此傳送方法時，這有助於以更可靠的方式提高您的電子郵件流量流程，這是已知的電子郵件最佳實務，強烈建議使用。

## IP策略 {#ip-strategy}

形成結構良好的IP策略很重要，這樣有助於建立良好的信譽。 IP和設定的數量會因您的業務模式和行銷目標而異。 與專家合作，制定明確的策略，從一開始就行。 請注意下列重要事項：

* **IP過多** 可能會觸發信譽問題，因為這是垃圾郵件傳送者的常見策略， **雪鞋**，這是垃圾郵件傳送者使用的策略，其流量會散佈在許多IP上，以將垃圾郵件傳送最大化。 即使您不是垃圾郵件傳送者，但如果您使用太多IP，特別是當這些IP先前沒有任何流量時，您可能會看起來像一個。
* **IP太少** 可能導致輸送量問題並可能觸發信譽問題。 輸送量因ISP而異。 ISP願意接受的程度和速度通常取決於其基礎架構和傳送信譽閾值。
* 區分傳訊型別的流量是關鍵。 至少必須在單獨的IP集區上單獨行銷和交易式郵件。
* 如果您的信譽非常不同，建議您根據您的郵件策略，在不同的IP集區上分隔不同的產品或行銷資料流。 有些行銷人員也依地區細分。 為信譽較低的流量分離IP將不會解決信譽問題，但這將防止您的「良好」信譽電子郵件傳遞出現問題。 畢竟，您不想為了風險更高的對象而犧牲您的優秀對象。

## 回饋迴路 {#feedback-loops}

在幕後，Adobe平台會處理有關退信、投訴、取消訂閱等資料。 這些意見回圈的設定是傳遞能力的重要方面。 投訴可能會損害聲譽，因此您應該使用電子郵件地址向目標受眾提出投訴。 請務必注意，Gmail不會將這些資料提供回來。 清單取消訂閱標題和參與篩選對Gmail訂閱者尤其重要，他們現在構成了訂閱者資料庫的大多數。

## 驗證 {#authentication}

驗證是ISP用來驗證傳送者身分的程式。 最常見的兩種驗證通訊協定為 [!DNL Sender Policy Framework] (SPF)和 [!DNL DomainKeys Identified Mail] (DKIM)。 一般使用者看不到這些訊息，但可協助ISP篩選來自已驗證寄件者的電子郵件。 [!DNL Domain-based Message Authentication Reporting and Conformance] (DMARC)越來越受歡迎，雖然其政策尚未被所有ISP納入其聲譽系統。

### SPF

[!DNL Sender Policy Framework] (SPF)是一種驗證方法，可讓網域擁有者指定他們用來從該網域傳送郵件的郵件伺服器。

### DKIM

[!DNL Domain Keys Identified Mail] (DKIM)是一種驗證方法，用於偵測偽造的寄件者地址（通常稱為欺騙）。 如果DKIM已啟用，接收者可以確認寄件者是否獲得授權，可以從該網域傳送郵件。

### DMARC

[!DNL Domain-based Message Authentication, Reporting and Conformance] (DMARC)是一種驗證方法，可讓網域擁有者保護其網域免受未經授權的使用。 DMARC使用SPF或DKIM （或兩者）來允許網域擁有者控制驗證失敗的郵件發生的情況：傳送、隔離或拒絕。

## 產品特定資源

**Campaign**

* 瞭解如何在中將子網域完全委派至Adobe Campaign Classic或Standard [本節](/help/additional-resources/ac-domain-name-setup.md).
* [控制面板：完整子網域委派（教學課程）](https://experienceleague.adobe.com/docs/campaign-classic-learn/control-panel/subdomains-and-certificates/subdomain-delegation.html) - *瞭解如何將子網域完全委派至Adobe Campaign Classic。*
* [控制面板：完整子網域委派（教學課程）](https://experienceleague.adobe.com/docs/campaign-standard-learn/control-panel/subdomains-and-certificates/subdomain-delegation.html) - *瞭解如何將子網域完全委派至Adobe Campaign Standard。*
* 進一步瞭解在中為Campaign Classic執行個體實作回饋迴路 [本節](/help/additional-resources/acc-technical-recommendations.md#feedback-loop-acc).

## 其他資源

* 在中進一步瞭解SPF、DKIM和DMARC驗證方法 [本節](/help/additional-resources/authentication.md).
* 進一步瞭解透過IP暖身提高您的電子郵件信譽 [本節](/help/additional-resources/increase-reputation-with-ip-warming.md).
