---
title: 網域名稱設定
description: 瞭解如何將子域委託給Adobe Campaign。
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: 4d52d197-d20e-450c-bfcf-e4541c474be4
source-git-commit: 82f7254a9027f79d2af59aece81f032105c192d5
workflow-type: tm+mt
source-wordcount: '2061'
ht-degree: 2%

---

# 網域名稱設定

本文檔介紹域名設定和委派的業務和技術要求。 您需要選擇發送子域的電子郵件以及面向外部的子域來承載您正在使用的Adobe平台的Web元件（登錄頁、選擇退出頁）。

>[!NOTE]
>
>也可以使用「控制面板」設定新的子域（可作為測試版）。 請參閱[此章節](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/setting-up-new-subdomain.html#must-read)深入瞭解。

## 子域

通過Adobe，數字營銷可以真正成為推動您品牌客戶參與營銷計畫的上下文引擎。  電子郵件仍然是數字營銷計畫的基礎。 然而，進入收件箱變得比以往更加困難。

為電子郵件活動建立子域使品牌能夠將不同類型的通信（例如，營銷與公司）隔離到特定的IP池中，並使用特定的域，這將加快 [IP預熱方法](../../help/additional-resources/increase-reputation-with-ip-warming.md) 全面提高供應能力。 如果您共用一個域，並且該域被阻止或添加到阻止清單中，它可能會影響您的公司郵件傳遞。 但是，特定於您的電子郵件營銷通信的域上的聲譽問題或阻止只會影響電子郵件流。  將主域用作多個郵件流的發件人地址或「發件人」地址也可能會中斷電子郵件身份驗證，導致您的郵件被阻止或放置在垃圾郵件資料夾中。

### 委託

域名委派是一種允許域名所有者的方法(技術上為：DNS區域)，以委派其細分(技術上：DNS區域，可稱為子區域)。 基本上，如果客戶正在處理區域&quot;example.com&quot;，他可以將子區域&quot;marketing.example.com&quot;委託給Adobe Campaign。

這意味著Adobe Campaign的DNS伺服器將僅對該區域擁有完全權限，而不是頂級域。 Adobe Campaign的DNS伺服器將對該區域中域名的查詢提供權威答案，例如&quot;t.marketing.example.com&quot;本身，而不是&quot;www.example.com&quot;。

通過委託一個子域供Adobe Campaign使用，客戶可以依靠Adobe來維護滿足其電子郵件營銷發送域的行業標準可交付性要求所需的DNS基礎架構，同時繼續維護和控制其內部電子郵件域的DNS。  子域委派允許：

客戶通過使用DNS別名及其域名Adobe來保留其品牌形象，以自動實施所有技術最佳實踐，以在電子郵件發送期間完全優化交付能力

## DNS設定選項

為了提供基於雲的托管服務，Adobe強烈鼓勵客戶端在部署Adobe Campaign時使用子域委派。  但是，Adobe確實為客戶提供了配置DNS的備選選項 — CNAME setup。

| 選項 | 說明 | Adobe職責 | 客戶責任 |
|--- |------- |--- |--- |
| 子域委派到Adobe Campaign | 客戶端將子域(email.example.com)委託到Adobe。 在此方案中，Adobe能夠通過控制和維護DNS的所有方面來將市場活動作為托管服務進行傳遞、呈現和跟蹤電子郵件活動。 | 完全管理子域和Adobe Campaign所需的所有DNS記錄。 | 子域的適當委派給Adobe |
| CNAME 的使用 | 客戶端建立子域並使用CNAME指向特定於Adobe的記錄。  若使用此設定，Adobe 和客戶都有責任維護 DNS。 | 管理Adobe Campaign所需的DNS記錄。 | 建立和控制子域以及建立/管理Adobe Campaign所需的CNAME記錄。 |

## 所需的DNS記錄

| 記錄類型 | 用途 | 記錄/內容示例 |
|--- |--- |--- |
| MX | 為傳入郵件指定郵件伺服器 | <i>email.example.com</i></br><i>10 inbound.email.example.com</i> |
| SPF(TXT) | 發件人策略框架 | <i>email.example.com</i></br>&quot;v=spf1重定向=__spf.campig.adobe.com&quot; |
| DKIM(TXT) | DomainKeys標識的郵件 | <i>客戶端。_domainkey.email.example.com</i></br>&quot;v=DKIM1;k=rsa;&quot; &quot;DKIMPUBLICKEY HERE&quot; |
| 主機記錄(A) | 鏡像頁、映像托管和跟蹤連結，所有發送域 | m.email.example.com IN A 123.111.100.99</br>t.email.example.com IN A 123.111.100.98</br>email.example.com IN A 123.111.100.97 |
| 反向DNS(PTR) | 將客戶端IP地址映射到客戶端品牌主機名 | 18.101.100.192.in-addr.arpa域名指針r18.email.example.com |
| CNAME | 為另一個域名提供別名 | t1.email.example.com是t1.email.example.campaig.adobe.com的別名 |


建議使用基於域的消息驗證、報告和一致性(DMARC)來驗證郵件發件人並確保目標電子郵件系統信任從您的域發送的消息。

DMARC TXT記錄示例：

```
_dmarc.email.example.com

“v=DMARC1; p=none; rua=mailto:mailauth-reports@myemail.com” 
```

您可以手動實施DMARC或聯繫Adobe，以幫助您為品牌設定DMARC。

## 設定要求

### 子域委派

這要求客戶端在其DNS伺服器中建立子域，並將此子域的名稱伺服器定義為由Adobe維護的名稱伺服器。  例如，主域名為「example.com」且要將「marketing.example.com」的管理委託給Adobe以進行電子郵件遞送的客戶端，必須實現此委託以將以下類型記錄添加到其DNS中：

```
marketing.example.com. NS a.ns.campaign.adobe.com.
marketing.example.com. NS b.ns.campaign.adobe.com.
marketing.example.com. NS c.ns.campaign.adobe.com.
marketing.example.com. NS d.ns.campaign.adobe.com.
```

域名的委派意味著此域將專用於通過Adobe Campaign平台發送電子郵件，因此不能用於其他方式（例如，從其他電子郵件基礎架構發送電子郵件）。

在設定過程中，Adobe將確保域連接到Adobe傳入的電子郵件基礎結構，以便管理和處理返回到這些域的反彈電子郵件（MX類型DNS記錄配置）。

### CNAME 的使用

如果客戶端選擇使用CNAME而不是委託子域進行Adobe，則在設定階段，Adobe將提供要放置在客戶端DNS伺服器中的記錄，並將配置Adobe CampaignDNS伺服器中的相應值。

## 部署的一般要求

在實施新的企業營銷解決方案時，需要面向外部的元件。  這些包括托管登錄頁和Web表單、設定要跟蹤的連結和Web頁、顯示鏡像頁以及配置選擇退出頁。

雖然這些要求是通過由Adobe和客戶托管的元件進行管理的，但它們包括URL，這些URL可由電子郵件的收件人看到。  為了避免具有指示底層技術解決方案或托管提供商的URL，可以設定子域以使此對電子郵件的收件人透明。  例如，在查看URL(如http://www.customer.com/)時，域將為「www.customer.com」。  該子域為「www」。

### 子域要求

確定要用於Adobe Campaign應用程式中的標籤URL（鏡像頁和跟蹤URL）的子域。  還要確定在電子郵件遞送時每個子域的「發件人地址」、「發件人名稱」和「答復地址」是什麼。

填寫下表，第一行只是一個示例。

| 子網域 | 發件人地址 | 從名稱 | 回復地址 |
|--- |--- |--- |--- |
| emails.customer.com | news@emails.customer.com | Customer | customercare@customer.com |
| </br> | </br> | </br> | </br> |

>[!NOTE]
>
>* 「回復地址」欄位的目的是希望收件人回復與「發件人地址」不同的地址。  雖然不是必填欄位，但Adobe強烈建議「回復地址」有效並連結到受監視的郵箱。  此郵箱必須由客戶托管。  它可以是支援郵箱，例如，customercare@customer.com，在該郵箱中讀取和回復電子郵件。
>* 如果客戶未選擇「回復地址」，則預設地址始終為 `<tenant>-<type>-<env>@<subdomain>`。
>* 以此方式設定「回復地址」時，回復將發送到未監視的郵箱。
>* 當從Adobe Campaign發送電子郵件時，「發件人地址」郵箱不受監控，營銷用戶無法訪問此郵箱。 Adobe Campaign也不提供自動回復或自動轉發在此郵箱中收到的電子郵件的功能。
>* 「市場活動發件人」/「發件人」地址和「錯誤」地址不能是「濫用」或「郵局管理員」。


## 委派子網域

選擇用於Adobe Campaign平台的子域必須通過建立四個名稱伺服器(NS)記錄來委派。  這允許子域被正確委派到Adobe。  下面是子域委派和相應DNS說明的示例。  請將「emails.customer.com」替換為要委託的子域。  請注意，子域必須唯一，並且不能已由其他方使用（例如，現有的ESP或MSP）。

| 委託子域 | DNS說明 |
|--- |--- |
| `<subdomain>` | `<subdomain>` NS a.ns.campaign.adobe.com。 </br> `<subdomain>` NS b.ns.campaign.adobe.com。 </br> `<subdomain>` NS c.ns.campaign.adobe.com。 </br> `<subdomain>` NS d.ns.campaign.adobe.com。 |

## 跟蹤，鏡像頁，資源

一旦電子郵件發送子域被正確委派到Adobe Campaign,AdobeTechOps團隊將建立兩個或多個較低級別的域，以獨立管理跟蹤和鏡像頁面。

| 類型 | 網域 |
|--- |--- |
| 鏡像頁 | m.`<subdomain>` |
| 追蹤 | t.`<subdomain>` |
| 資源 | 雷斯。`<subdomain>` |

## 雲部署（可選）

僅當Adobe Campaign Classic完全由Adobe托管在雲中時，這才適用。  這是可選配置。

任何要開發的調查、Web表單和登錄頁都通過完全托管在雲中的Adobe Campaign進行管理。  如果需要，可將附加子域委託給Adobe（例如web.customer.com）以用於工具中的任何Web元件。  請注意，子域必須唯一，並且不能由其他方使用（例如，現有的ESP或MSP）。

| 委託子域 | DNS說明 |
|--- |--- |
| `<subdomain>` | `<subdomain>` NS a.ns.campaign.adobe.com。</br>`<subdomain>` NS b.ns.campaign.adobe.com。</br>`<subdomain>` NS c.ns.campaign.adobe.com。</br>`<subdomain>` NS d.ns.campaign.adobe.com。 |

>[!NOTE]
>
>預設情況下，工具中的任何Web元件都將使用委託用於電子郵件的初始子域。

## 雲消息部署（可選）

如果Adobe Campaign Classic營銷實例是在客戶現場托管的，則需要由客戶進行其他技術配置。

任何要開發的調查、Web表單和登錄頁都通過Adobe Campaign市場營銷實例進行管理，收件人記錄在此處。

部署由Adobe Campaign營銷實例托管的面向外部的Web元件時，需要額外的CNAME DNS配置。  這將允許Web元件（例如web.customer.com）可以公開訪問Internet，並與客戶的域一起標籤。

還需要配置防火牆，以允許訪問承載這些Web元件的Adobe Campaign市場營銷實例（埠80或443）。

**最佳實踐Recommendations:**

托管Web元件的子域對客戶可見，因此請確保正確標籤該子域，並使其易於記住，因為可能需要手動輸入該子域，例如：https://web.customer.com。
如果需要在安全頁面(HTTPS)上承載任何表單，則需要附加技術配置，如下所述。

| 委託子域 | DNS說明 |
|--- |--- |
| `<subdomain>` | `<subdomain>` CNAME `<internal customer server>` |

## 提供的服務

在這些委託之後，通過Adobe建立的基礎架構可確保為每個委託或基於CNAME的發送域執行以下服務：

* 建立郵政管理員@和濫用@收件箱
* 為委派域設定反饋循環
* 根據請求，Adobe還將配置指定的DMARC記錄。 您的交付能力顧問可以協助您為發送域設計長期DMARC策略和計畫。
由Adobe建立的參數僅在完成委派後由Adobe驗證後才有效，並且仍然有效。  所有Adobe Campaign雲產品都包括域名委派作為標準。

## 計費和實施條件

* 根據初始合同和選定的一攬子類型，除了作為標準列入的初始授權之外，還可包括其他授權，
* 除這些代表團外，還將向其他代表團收費，
* 這些額外委託的計費方法按初始合同規定的每月額外費用計算。

如果客戶端選擇專用於通過Adobe Campaign工具交付的關聯域名，並正確執行相關檔案中詳述的授權先決條件，將接受這些授權。

## 停止服務

隨時，客戶端都能夠提出書面要求，不再從委派服務中獲益，並自行進行必要的DNS配置。

如果發生這種情況，Adobe將向CLIENT提供一個估計值，詳細列出返回非域委派模式所需的服務天數。

如果客戶未能遵守上述承諾，Adobe將免除任何就採用上述可交付性比率承擔的責任。

終止Marketing Cloud服務將自動導致域委託的結束，並通過Adobe對這些域進行DNS維護。

## 使用控制面板監視子域

一旦為實例配置了子域，您就可以使用「控制面板」來監視子域。

這允許您查看您委託給Adobe Campaign的所有子域，並請求續訂其SSL證書。

如需詳細資訊，請參閱[專屬文件](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/monitoring-subdomains.html#subdomains-and-certificates)。

>[!NOTE]
>
>[控制面板](https://experienceleague.adobe.com/docs/control-panel/using/control-panel-home.html) 僅供使用Adobe Managed Services的客戶使用。
