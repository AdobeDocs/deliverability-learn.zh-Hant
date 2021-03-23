---
title: 基礎架構
description: '瞭解正確建構電子郵件基礎架構所需的內容。 '
feature: 轉換程式
topics: Deliverability
kt: 7052
thumbnail: kt7052.jpg
doc-type: article
activity: understand
team: ACS
translation-type: tm+mt
source-git-commit: 1e539b5df54250a5927701009e7a9c84e5d73fae
workflow-type: tm+mt
source-wordcount: '912'
ht-degree: 0%

---


# 基礎架構

成功的交付能力取決於堅實的基礎。 電子郵件基礎架構是核心元素。 構建得當的電子郵件基礎結構包括多個元件——即域和IP地址。 這些元件就像您傳送電子郵件背後的機器，通常是傳送聲譽的錨。 可交付性顧問可確保在實施過程中正確設定這些元素，但由於聲譽因素，因此您必須具備這一基本理解。

## 域設定和策略{#domain-setup-and-strategy}

時代已經改變，一些網際網路服務提供商（如Gmail和雅虎）現在將域名信譽作為將電子郵件信譽附加到發件人身上的附加條件。 您的網域信譽取決於您的傳送網域，而非您的IP位址。 這表示在ISP篩選決策時，您的品牌優先。

Adobe平台上新寄件者的上線程式包括設定您的傳送網域，以及確保您的基礎架構已正確建立。 您應與專家合作，瞭解您打算長期使用的網域。 以下是形成良好域策略的一些提示：

* 在您選擇的網域中盡可能清楚反映品牌，讓使用者不會將郵件誤識為垃圾郵件。 例如newsletter.foo.com、receepts.foo.com等。
* 您不應使用您的父級或公司網域，因為這會影響從您的組織傳送郵件至ISP的效果。
* 請考慮使用父網域的子網域來使傳送網域合法化。
* 區隔您的子網域以劃分交易和行銷訊息類別。 當ISP尋找此傳送方法時，這將協助您的電子郵件流量更可靠，這是已知的電子郵件最佳實務，並強烈建議您這麼做。

## IP策略{#ip-strategy}

制定結構完善的智慧財產權戰略有助於建立良好的聲譽，這一點非常重要。 IP的數量和設定會視您的業務模式和行銷目標而有所不同。 與專家合作，制定清楚的策略，從頭開始。 請考慮以下重要事項：

* **太多** IPscan會觸發信譽問題，因為它是垃圾郵件發送者對 ****&#x200B;雪鞋的常見策略，此策略是垃圾郵件發送者使用的策略，其中流量會在許多IP之間傳播，以最大化垃圾郵件的發送。即使您不是垃圾郵件發送者，但如果您使用過多的IP，尤其是當這些IP之前沒有任何流量時，您看起來可能就像一個垃圾郵件發送者。
* **IPscan太少會** 導致吞吐量問題，並可能觸發信譽問題。吞吐量因ISP而異。 ISP願意接受的速度和速度通常取決於其基礎架構和發送信譽閾值。
* 隔離傳訊類型的流量是關鍵。 至少在裸機上，在單獨的IP池上分別使用行銷和事務性郵件非常重要。
* 根據您的郵件策略，如果您的信譽大不相同，也建議您在不同IP池上分隔不同的產品或行銷流。 部分行銷人員也依地區劃分。 將信譽較低的流量IP區隔開，並不能解決信譽問題，但能防止「信譽良好」的電子郵件傳送問題。 畢竟，您不想為了風險更高的受眾而犧牲好的受眾。

## 反饋循環{#feedback-loops}

在幕後，Adobe平台正在處理有關回報、投訴、取消訂閱等資料。 這些反饋迴路的設定是實現能力的一個重要方面。 投訴可能會損及聲譽，因此您應以電子郵件寄送來自目標受眾的投訴。 請務必注意，Gmail無法提供這些資料。 清單取消訂閱標題和參與篩選對於Gmail訂閱者尤其重要，因為Gmail訂閱者目前佔大部分訂閱者資料庫。

## 驗證{#authentication}

身份驗證是ISP用於驗證發送者身份的過程。 最常見的兩種身份驗證協定是[!DNL Sender Policy Framework](SPF)和[!DNL DomainKeys Identified Mail](DKIM)。 使用者無法看到這些內容，但確實可協助ISP篩選經驗證寄件者寄送的電子郵件。 [!DNL Domain-based Message Authentication Reporting and Conformance] (DMARC)越來越受歡迎，不過其政策尚未納入所有ISP的聲譽系統。

### SPF

[!DNL Sender Policy Framework] (SPF)是一種驗證方法，允許域的所有者指定他們使用哪些郵件伺服器從該域發送郵件。

### DKIM

[!DNL Domain Keys Identified Mail] (DKIM)是一種驗證方法，用來偵測偽造的傳送者位址（通常稱為欺騙）。如果啟用DKIM，則允許接收者確認是否授權發件人從該域發送郵件。

### DMARC

[!DNL Domain-based Message Authentication, Reporting and Conformance] (DMARC)是一種驗證方法，可讓網域擁有者保護其網域不受未經授權的使用。DMARC使用SPF或DKIM或兩者，以允許域所有者控制驗證失敗的郵件的情況：已傳送、隔離或拒絕。

## 產品特定資源

**Campaign**

* 瞭解如何在[本節](/help/additional-resources/ac-domain-name-setup.md)中將子網域完全委派給Adobe Campaign Classic或標準。
* [控制面板：完整子網域委派（教學課程）](https://experienceleague.corp.adobe.com/docs/campaign-classic-learn/control-panel/subdomains-and-certificates/subdomain-delegation.html) - *瞭解如何將子網域完全委派給Adobe Campaign Classic。*
* [控制面板：完整子網域委派（教學課程）](https://experienceleague.corp.adobe.com/docs/campaign-standard-learn/control-panel/subdomains-and-certificates/subdomain-delegation.html) - *瞭解如何將子網域完全委派給Adobe Campaign Standard。*
* 進一步瞭解如何在[本節](/help/additional-resources/acc-technical-recommendations.md#feedback-loop-acc)中實作Campaign Classic例項的回饋回圈。

## 其他資源

* 在[本節](/help/additional-resources/authentication.md)中進一步瞭解SPF、DKIM和DMARC驗證方法。
* 在[本節](/help/additional-resources/increase-reputation-with-ip-warming.md)中，進一步瞭解如何提高您電子郵件的知名度。
