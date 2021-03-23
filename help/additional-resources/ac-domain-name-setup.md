---
title: 網域名稱設定
description: 瞭解如何將子網域委派給Adobe Campaign。
feature: 付諸實踐
topics: Deliverability
kt: null
thumbnail: null
doc-type: article
activity: understand
team: ACS
translation-type: tm+mt
source-git-commit: 1e539b5df54250a5927701009e7a9c84e5d73fae
workflow-type: tm+mt
source-wordcount: '2032'
ht-degree: 1%

---


# 網域名稱設定

本檔案說明網域名稱設定與委派的商業與技術需求。 您將需要選擇傳送子網域的電子郵件，以及（可選）對外的子網域，以代管您所使用之Adobe平台的Web元件（登陸頁面、選擇退出頁面）。

>[!NOTE]
>
>您也可以使用「控制面板」（以測試版形式提供）來設定新的子網域。 進一步瞭解[本節](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/setting-up-new-subdomain.html#must-read)。

## 子網域

借助Adobe，數位行銷可真正成為推動品牌客戶互動行銷計畫的情境式引擎。  電子郵件仍是數位行銷計畫的基礎。 但是，進入收件匣變得比以往更加困難。

為電子郵件促銷活動建立子網域可讓品牌將各種類型的流量（例如行銷與公司）隔離到特定IP池和特定網域，以加速[IP變暖程式](../../help/additional-resources/increase-reputation-with-ip-warming.md)並改善整體傳遞能力。 如果您共用網域，而網域遭到封鎖或新增至封鎖清單，可能會影響您的公司郵件傳送。 不過，您的電子郵件行銷通訊特定網域的信譽問題或封鎖只會影響電子郵件的流程。  使用您的主網域作為多個郵件串流的傳送者或「寄件者」位址，也可能會中斷電子郵件驗證，導致您的訊息被封鎖或放在垃圾訊息資料夾中。

### 委派

域名委派是一種允許域名所有者(技術上：域名系統區域)，以委派其細分(技術上：DNS區域，可稱為子區域)。 基本上，如果客戶正在處理區域&quot;example.com&quot;，他可以將子區域&quot;marketing.example.com&quot;委派給Adobe Campaign。

這表示Adobe Campaign的DNS伺服器將只擁有該區域的完整權限，而非最上層網域。 Adobe Campaign的DNS伺服器將針對該區域中的網域名稱查詢提供權威答案，例如&quot;t.marketing.example.com&quot;本身，但不提供&quot;www.example.com&quot;。

通過委託子域用於Adobe Campaign，客戶可以依靠Adobe來維護滿足其電子郵件行銷發送域行業標準傳遞能力要求所需的DNS基礎架構，同時繼續維護和控制其內部電子郵件域的DNS。  子域委派允許：

客戶使用DNS別名及其域名來保持其品牌形象
Adobe自主實作所有技術最佳實務，以在電子郵件傳送期間完全最佳化傳遞能力

## DNS設定選項

為了提供基於雲的托管服務，Adobe強烈建議客戶在部署Adobe Campaign時使用子域委託。  但是，Adobe確實為客戶提供了配置DNS的替代選項- CNAME設定。

| 選項 | 說明 | Adobe責任 | 客戶責任 |
|--- |------- |--- |--- |
| 向Adobe Campaign派遣子網域代表團 | 客戶將子網域(email.example.com)委派給Adobe。 在此情況下，Adobe可以控制並維護傳送、轉譯及追蹤電子郵件促銷活動所需的DNS的所有方面，將促銷活動以受管理的服務形式提供。 | 完整管理Adobe Campaign所需的子網域和所有DNS記錄。 | 適當委派子網域至Adobe |
| CNAME 的使用 | 客戶端建立子域，並使用CNAME指向Adobe特定記錄。  若使用此設定，Adobe 和客戶都有責任維護 DNS。 | 管理Adobe Campaign所需的DNS記錄。 | 建立和控制子網域，並建立／管理Adobe Campaign所需的CNAME記錄。 |

## 需要的DNS記錄

| 記錄類型 | 用途 | 範例記錄／內容 |
|--- |--- |--- |
| MX | 為傳入郵件指定郵件伺服器 | <i>email.example.com</i></br><i>10 inbound.email.example.com</i> |
| SPF(TXT) | 發件人策略框架 | <i>email.example.com</i></br>&quot;v=spf1 redirect=__spf.campaign.adobe.com&quot; |
| DKIM(TXT) | DomainKeys已識別的郵件 | <i>客戶端。_domainkey.email.example.com</i></br>&quot;v=DKIM1;k=rsa;&quot; &quot;DKIMPUBLICKEY HERE&quot; |
| DMARC(TXT) | 基於域的消息驗證 | 報告與符合性 | _dmarc.email.example.com</br>&quot;v=DMARC1;p=none;rua=mailto:mailauth-reports@myemail.com&quot; |
| 主機記錄(A) | 鏡像頁面、影像托管和追蹤連結，所有傳送網域 | m.email.example.com中的A 123.111.100.99</br>t.email.example.com中的A 123.111.100.98</br>email.example.com中的A 123.111.100.97 |
| 反向DNS(PTR) | 將客戶機IP地址映射到客戶機品牌主機名 | 18.101.100.192.in-addr.arpa域名指針r18.email.example.com |
| CNAME | 為另一個域名提供別名 | t1.email.example.com是 | t1.email.example.campaign.adobe.com |

## 設定需求

### 子域委派

這要求客戶機在其DNS伺服器中建立子域，並為此子域定義由Adobe維護的名稱伺服器。  例如，主網域名為&quot;example.com&quot;且想要將&quot;marketing.example.com&quot;的管理委派給Adobe傳送電子郵件的客戶，必須具體化此委派，才能將下列類型記錄新增至其DNS:

```
marketing.example.com. NS a.ns.campaign.adobe.com.
marketing.example.com. NS b.ns.campaign.adobe.com.
marketing.example.com. NS c.ns.campaign.adobe.com.
marketing.example.com. NS d.ns.campaign.adobe.com.
```

指定域名意味著此域將專用於通過Adobe Campaign平台發送電子郵件，因此不能用於其他方式（例如，從其他電子郵件基礎架構發送電子郵件）。

在設定程式中，Adobe將確保網域已附加至Adobe傳入的電子郵件基礎架構，以管理並處理回傳至這些網域的反彈電子郵件（MX類型DNS記錄設定）。

### CNAME 的使用

如果客戶端選擇使用CNAME，而不是將子域委派給Adobe，在設定階段，Adobe將提供要放置在客戶端DNS伺服器中的記錄，並將配置Adobe CampaignDNS伺服器中的相應值。

## 部署的一般需求

實作新的企業行銷解決方案時，對外部元件有需求。  這些功能包括托管著陸頁面和Web表單、設定要追蹤的連結和網頁、顯示鏡像頁面，以及設定選擇退出頁面。

雖然這些需求是透過Adobe和客戶托管的元件來管理，但它們包含URL，讓電子郵件的收件者可以看到。  為避免URL顯示基礎技術解決方案或代管供應商，可以設定子網域，讓電子郵件的收件者能夠透明化此URL。  例如，在查看URL(例如http://www.customer.com/)時，網域會是「www.customer.com」。  此網域的子網域為「www」。

### 子網域需求

確定要用於來自Adobe Campaign應用程式之品牌化URL（鏡像頁面和追蹤URL）的子網域。  此外，也可決定電子郵件傳送的每個子網域的「寄件者位址」、「寄件者名稱」和「回覆者位址」。

請填妥下表，第一行只是範例。

| 子網域 | 寄件者地址 | 從名稱 | 回覆地址 |
|--- |--- |--- |--- |
| emails.customer.com | news@emails.customer.com | 客戶 | customercare@customer.com |
| </br> | </br> | </br> | </br> |

>[!NOTE]
>
>* 「回覆地址」欄位的目的，在於您希望收件者回覆「寄件者地址」以外的其他位址。  雖然不是必填欄位，但Adobe強烈建議「回覆地址」有效並連結到受監視的郵箱。  此郵箱必須由客戶托管。  它可能是支援郵箱，例如customercare@customer.com，可供閱讀和回覆電子郵件。
>* 如果客戶未選擇「回覆地址」，則預設地址一律為`<tenant>-<type>-<env>@<subdomain>`。
>* 以此方式設定「回覆地址」時，將向未監控的郵箱發送回覆。
>* 當從Adobe Campaign發送電子郵件時，「寄件者地址」郵箱不受監控，而行銷用戶無法訪問此郵箱。 Adobe Campaign也不提供自動回覆或自動轉寄在此郵箱中收到的電子郵件的功能。
>* 促銷活動寄件者／寄件者位址和錯誤位址不能是「濫用」或「郵遞員」。


## 委派子網域

選擇用於Adobe Campaign平台的子域必須通過建立四個名稱伺服器(NS)記錄來委派。  這可讓子網域正確委派給Adobe。  以下是子域委派和各自DNS指示的範例。  請將「emails.customer.com」取代為您要委派的子網域。  請注意，子網域必須是唯一的，且不能由另一方使用（例如，現有的ESP或MSP）。

| 委派子網域 | DNS指示 |
|--- |--- |
| `<subdomain>` | `<subdomain>` NS a.ns.campaign.adobe.com。  </br> `<subdomain>` NS b.ns.campaign.adobe.com。  </br> `<subdomain>` NS c.ns.campaign.adobe.com。  </br> `<subdomain>` NS d.ns.campaign.adobe.com。 |

## 追蹤、鏡像頁面、資源

在電子郵件傳送子網域正確委派給Adobe Campaign後，AdobeTechOps團隊將建立兩個或兩個以上較低層級的網域，以獨立管理追蹤和鏡像頁面。

| 類型 | 網域 |
|--- |--- |
| 鏡像頁 | m.`<subdomain>` |
| 追蹤 | t.`<subdomain>` |
| 資源 | res.`<subdomain>` |

## 雲端部署（可選）

只有當Adobe Campaign Classic完全由Adobe托管在雲中時，這才適用。  這是可選配置。

任何要開發的調查、Web表格和登陸頁面都可透過完全托管在雲端的Adobe Campaign進行管理。  如有需要，可將其他子網域委派給Adobe（例如web.customer.com），以用於工具中的任何Web元件。  請注意，子網域必須是唯一的，其他方（例如現有的ESP或MSP）無法使用。

| 委派子網域 | DNS指示 |
|--- |--- |
| `<subdomain>` | `<subdomain>` NS a.ns.campaign.adobe.com。</br>`<subdomain>` NS b.ns.campaign.adobe.com。</br>`<subdomain>` NS c.ns.campaign.adobe.com。</br>`<subdomain>` NS d.ns.campaign.adobe.com。 |

>[!NOTE]
>
>依預設，工具中的任何Web元件都會使用委派用於電子郵件的初始子網域。

## 雲端訊息部署（選用）

如果Adobe Campaign Classic行銷實例是在客戶現場代管，則客戶將需要進行其他技術配置。

任何要開發的調查、Web表格和登陸頁面都會透過收件者記錄所在的Adobe Campaign行銷實例進行管理。

部署由Adobe Campaign行銷實例代管的對外Web元件時，需要額外的CNAME DNS設定。  這可讓網路元件（例如web.customer.com）公開存取網際網路，並加上客戶網域的品牌。

此外，還需要配置防火牆，以允許訪問托管這些Web元件的Adobe Campaign行銷實例（在埠80或443）。

**最佳實踐Recommendations:**

代管Web元件的子網域對客戶而言是可見的，因此請務必使其品牌正確且易於記住，因為可能需要手動輸入，例如：https://web.customer.com。
如果需要在安全頁面(HTTPS)上托管任何表格，則需要額外的技術設定，如下所述。

| 委派子網域 | DNS指示 |
|--- |--- |
| `<subdomain>` | `<subdomain>` CNAME  `<internal customer server>` |

## 提供的服務

在這些委派後，Adobe建立的基礎架構可確保對每個委派或基於CNAME的發送域執行以下服務：

* 建立postmaster@和ubuse@ inbox
* 為委託域設定反饋環
* 如有要求，Adobe也會依指定設定DMARC記錄。 您的交付能力顧問可協助您設計長期的DMARC政策，並規劃傳送網域。
由Adobe建立的參數僅在完成委託後由Adobe驗證時有效，並且仍然有效。  所有Adobe Campaign雲端優惠都包含以網域名稱委派為標準。

## 帳單與實作條件

* 根據初始合同和選定的一攬子方案類型，除了作為標準列入的其他代表團之外，還可包括作為標準的其他代表團，
* 除這些代表團外，還將向其他代表團收費，
* 這些額外委託的付款方式需額外支付每月費用，如初始合約所述。

如果客戶端選擇通過Adobe Campaign工具交付的專用相關域名，並正確執行相關檔案中詳述的委派先決條件，將接受這些代表團。

## 中止服務

CLIENT隨時都能提出書面要求，不再從委派服務獲益，並自行承擔必要的DNS配置。

如果發生這種情況，Adobe將向CLIENT提供估計值，詳細說明返回非域委派模式所需的服務天數。

如客戶未能遵守上述承諾，將免除Adobe接受上述交付率的任何責任。

終止Marketing Cloud服務將自動導致域委託的結束，並根據Adobe對這些域進行DNS維護。

## 使用「控制面板」監控子網域

為實例配置子域後，可使用「控制面板」監視子域。

這可讓您檢視您委派給Adobe Campaign的所有子網域，並要求續約其SSL憑證。

如需詳細資訊，請參閱[專屬文件](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/monitoring-subdomains.html#subdomains-and-certificates)。

>[!NOTE]
>
>[Control ](https://experienceleague.adobe.com/docs/control-panel/using/control-panel-home.html) Panel僅適用於使用Adobe Managed Services的客戶。