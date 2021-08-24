---
title: 網域名稱設定
description: 了解如何將子網域委派至Adobe Campaign。
topics: Deliverability
kt: null
thumbnail: null
doc-type: article
activity: understand
team: ACS
exl-id: 4d52d197-d20e-450c-bfcf-e4541c474be4
source-git-commit: 68c403f915287e1a50cd276b67b3f48202f45446
workflow-type: tm+mt
source-wordcount: '2028'
ht-degree: 2%

---

# 網域名稱設定

本檔案說明網域名稱設定和委派的業務與技術需求。 您將需要選取電子郵件傳送子網域，以及（可選）外部子網域，以托管您所使用之Adobe平台的Web元件（登錄頁面、選擇退出頁面）。

>[!NOTE]
>
>您也可以使用「控制面板」（以測試版形式提供）來設定新的子網域。 進一步了解[本節](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/setting-up-new-subdomain.html#must-read)。

## 子網域

透過Adobe，數位行銷可真正成為情境式引擎，為您品牌的客戶互動行銷計畫提供動力。  電子郵件仍是數位行銷計畫的基礎。 但是，到達收件匣變得比以往更加困難。

為電子郵件行銷活動建立子網域可讓品牌將各種類型的流量（行銷與公司等）隔離至特定IP池和特定網域，借此加速[IP暖化程式](../../help/additional-resources/increase-reputation-with-ip-warming.md)並改善整體傳遞能力。 如果您共用網域，且該網域遭到封鎖或新增至封鎖清單，可能會影響您的公司郵件傳送。 不過，您電子郵件行銷通訊專屬網域的信譽問題或區塊，只會影響電子郵件的流程。  將主域用作多個郵件流的發件人地址或「發件人」地址也可能會中斷電子郵件身份驗證，從而導致郵件被阻止或放在垃圾郵件資料夾中。

### 委派

域名委派是一種允許域名所有者的方法(技術上：DNS區域)，以委派DNS的細分(技術上：其下的DNS區域，可稱為子區域)，傳至其他實體。 基本上，如果客戶處理區域「example.com」，他可以將子區域「marketing.example.com」委派給Adobe Campaign。

這表示Adobe Campaign的DNS伺服器只會在該區域上擁有完整權限，而不會擁有頂層網域。 Adobe Campaign的DNS伺服器將針對該區域中的網域名稱查詢提供權威答案，例如&quot;t.marketing.example.com&quot;本身，但不提供&quot;www.example.com&quot;。

透過委派子網域以與Adobe Campaign搭配使用，客戶可仰賴Adobe來維護必要的DNS基礎架構，以符合其電子郵件行銷傳送網域的業界標準傳遞能力需求，同時持續維護和控制其內部電子郵件網域的DNS。  子網域委派允許：

使用域名的DNS別名來保留其品牌形象的用戶端
Adobe自主實施所有技術最佳實務，以在電子郵件傳送期間完全最佳化傳遞能力

## DNS設定選項

為了提供雲端型托管服務，Adobe強烈建議用戶端在部署Adobe Campaign時使用子網域委派。  不過，Adobe確實為用戶端提供設定DNS的替代選項 — CNAME設定。

| 選項 | 說明 | Adobe責任 | 客戶責任 |
|--- |------- |--- |--- |
| 子網域委派至Adobe Campaign | 用戶端委派子網域(email.example.com)以Adobe。 在此案例中，Adobe可控制並維護傳送、呈現及追蹤電子郵件促銷活動所需的DNS的所有層面，以托管服務的形式提供促銷活動。 | 完整管理Adobe Campaign所需的子網域和所有DNS記錄。 | 正確委派子網域以Adobe |
| CNAME 的使用 | 用戶端會建立子網域，並使用CNAME指向Adobe特定記錄。  若使用此設定，Adobe 和客戶都有責任維護 DNS。 | 管理Adobe Campaign所需的DNS記錄。 | 建立和控制子網域，以及建立/管理Adobe Campaign所需的CNAME記錄。 |

## 所需的DNS記錄

| 記錄類型 | 用途 | 範例記錄/內容 |
|--- |--- |--- |
| MX | 為傳入郵件指定郵件伺服器 | <i>email.example.com</i></br><i>10 inbound.email.example.com</i> |
| SPF(TXT) | 發件人策略框架 | <i>email.example.com</i></br>&quot;v=spf1 redirect=__spf.campaign.adobe.com&quot; |
| DKIM(TXT) | 已標識的DomainKeys Mail | <i>用戶端。_domainkey.email.example.com</i></br>&quot;v=DKIM1;k=rsa;&quot; &quot;DKIMPUBLICKEY HERE&quot; |
| DMARC(TXT) | 基於域的消息驗證 | 報告與符合 | _dmarc.email.example.com</br>&quot;v=DMARC1;p=none;rua=mailto:mailauth-reports@myemail.com |
| 主機記錄(A) | 鏡像頁面、影像托管和追蹤連結，所有傳送網域 | m.email.example.com，位於123.111.100.99</br>t.email.example.com，位於123.111.100.98</br>email.example.com，位於123.111.100.97 |
| 反向DNS(PTR) | 將客戶機IP地址映射到客戶機品牌主機名 | 18.101.100.192.in-addr.arpa域名指針r18.email.example.com |
| CNAME | 為其他域名提供別名 | t1.email.example.com是 | t1.email.example.campaign.adobe.com |

## 設定需求

### 子網域委派

這要求用戶端在其DNS伺服器中建立子網域，並將此子網域的名稱伺服器定義為由Adobe維護的伺服器。  例如，如果客戶的主要網域名稱為「example.com」，且想要將「marketing.example.com」的管理委派給Adobe以進行電子郵件傳送，則必須將此委派具體化，以將下列類型記錄新增至其DNS:

```
marketing.example.com. NS a.ns.campaign.adobe.com.
marketing.example.com. NS b.ns.campaign.adobe.com.
marketing.example.com. NS c.ns.campaign.adobe.com.
marketing.example.com. NS d.ns.campaign.adobe.com.
```

委派網域名稱表示此網域將專用於透過Adobe Campaign平台傳送電子郵件，因此無法用於其他方式（例如從其他電子郵件基礎架構傳送電子郵件）。

在設定過程中，Adobe將確保域已連接到Adobe傳入的電子郵件基礎結構，以便管理和處理返回到這些域（MX類型DNS記錄配置）的反彈電子郵件。

### CNAME 的使用

如果用戶端選擇使用CNAME而非委派子網域以Adobe，在設定階段期間，Adobe會提供要放置在用戶端DNS伺服器中的記錄，並在Adobe Campaign DNS伺服器中設定對應的值。

## 部署的一般要求

實作新的企業行銷解決方案時，需要對外面的元件。  這些功能包括托管登錄頁面和網頁表單、設定要追蹤的連結和網頁、顯示鏡像頁面以及設定選擇退出頁面。

雖然這些需求是透過Adobe和客戶托管的元件來管理，但其中包含URL，可供電子郵件的收件者看到。  為避免有指出基礎技術解決方案或托管提供者的URL，您可以設定子網域，讓這對電子郵件的收件者透明。  例如，查看URL(例如http://www.customer.com/)時，網域會是「www.customer.com」。  此的子網域會是「www」。

### 子網域需求

判斷要用於Adobe Campaign應用程式中品牌URL（鏡像頁面和追蹤URL）的子網域。  另外，決定傳送電子郵件時每個子網域的「寄件者地址」、「寄件者名稱」和「回覆地址」。

填妥下表，第一行只是範例。

| 子網域 | 寄件者地址 | 從名稱 | 回復地址 |
|--- |--- |--- |--- |
| emails.customer.com | news@emails.customer.com | Customer | customercare@customer.com |
| </br> | </br> | </br> | </br> |

>[!NOTE]
>
>* 「回復地址」欄位的目的是讓收件者回覆「寄件者地址」以外的其他地址。  雖然不是必填欄位，但Adobe強烈建議「回復地址」有效並連結到受監控的郵箱。  此郵箱必須由客戶托管。  可能是支援信箱，例如customercare@customer.com，可讀取和回應電子郵件。
>* 如果客戶未選擇「回復地址」，則預設地址始終為`<tenant>-<type>-<env>@<subdomain>`。
>* 以此方式設定「回復地址」時，會將回復發送到未監視的郵箱。
>* 從Adobe Campaign傳送電子郵件時，不會監控「寄件者地址」信箱，且行銷使用者無法存取此信箱。 Adobe Campaign也不提供自動回覆或自動轉送電子郵件功能。
>* 促銷活動寄件者/寄件者位址和錯誤位址不能是「濫用」或「郵遞員」。


## 委派子網域

必須建立四個名稱伺服器(NS)記錄，以委派所選用於Adobe Campaign平台的子網域。  這可讓子網域正確委派給Adobe。  以下是子網域委派和個別DNS指示的範例。  請以您要委派的子網域取代「emails.customer.com」。  請注意，子網域必須是唯一的，且其他方（例如現有的ESP或MSP）無法使用。

| 委派的子網域 | DNS指示 |
|--- |--- |
| `<subdomain>` | `<subdomain>` NS a.ns.campaign.adobe.com。  </br> `<subdomain>` NS b.ns.campaign.adobe.com。  </br> `<subdomain>` NS c.ns.campaign.adobe.com。  </br> `<subdomain>` NS d.ns.campaign.adobe.com。 |

## 追蹤，鏡像頁面，資源

將電子郵件傳送子網域正確委派給Adobe Campaign後，AdobeTechOps團隊將建立兩個或兩個以上較低層級的網域，以獨立管理追蹤和鏡像頁面。

| 類型 | 網域 |
|--- |--- |
| 鏡像頁面 | m.`<subdomain>` |
| 追蹤 | t.`<subdomain>` |
| 資源 | res.`<subdomain>` |

## 雲端部署（可選）

唯有Adobe Campaign Classic完全由Adobe托管於雲端時，才適用。  這是選用的設定。

任何要開發的調查、網頁表單和登錄頁面，都可透過完全托管於雲端的Adobe Campaign來管理。  如有需要，可將其他子網域委派給Adobe（例如web.customer.com），以用於工具內的任何Web元件。  請注意，子網域必須是唯一的，且無法供其他方使用（例如現有的ESP或MSP）。

| 委派的子網域 | DNS指示 |
|--- |--- |
| `<subdomain>` | `<subdomain>` NS a.ns.campaign.adobe.com。</br>`<subdomain>` NS b.ns.campaign.adobe.com。</br>`<subdomain>` NS c.ns.campaign.adobe.com。</br>`<subdomain>` NS d.ns.campaign.adobe.com。 |

>[!NOTE]
>
>依預設，工具中的任何Web元件都會使用委派用於電子郵件的初始子網域。

## 雲端訊息部署（選用）

如果Adobe Campaign Classic行銷執行個體是由客戶內部托管，則需要由客戶進行其他技術設定。

任何要開發的調查、網頁表單和登錄頁面，都可透過Adobe Campaign行銷執行個體管理，且收件者記錄存在。

部署由Adobe Campaign行銷執行個體托管的對外Web元件時，需要額外的CNAME DNS設定。  這可讓Web元件（例如web.customer.com）可公開供Internet存取，並加上客戶網域的品牌。

也需要將防火牆設定為允許存取托管這些Web元件的Adobe Campaign行銷執行個體（位於連接埠80或443）。

**最佳實務Recommendations:**

托管Web元件的子網域將會顯示給客戶，因此請務必使其品牌正確且易於記住，因為可能需要手動輸入，例如：https://web.customer.com。
如果需要在安全頁面(HTTPS)上托管任何表單，則需要額外的技術設定，如下所述。

| 委派的子網域 | DNS指示 |
|--- |--- |
| `<subdomain>` | `<subdomain>` CNAME  `<internal customer server>` |

## 已提供服務

在這些委派後，由Adobe建立的基礎架構可確保為每個委派或基於CNAME的傳送網域執行下列服務：

* 建立Postmaster@和Ubuse@收件箱
* 為委派的域設定反饋循環
* Adobe也會根據要求設定DMARC記錄。 您的傳遞能力顧問可協助您為傳送網域設計長期的DMARC政策和規劃。
由Adobe建立的參數僅從完成委派後，再由Adobe驗證時有效，並且保持正常。  所有Adobe Campaign雲端選件都包含將網域名稱委派為標準。

## 帳單與實作條件

* 根據初始合同和所選擇的一攬子類型，除作為標準列入的代表團之外，還可列入其他代表團，
* 除這些代表團外，還將向其他代表團收費，
* 如初始合同所述，這些額外委託的計費方法每月額外收費。

如果CLIENT選擇專用於通過Adobe Campaign工具進行傳送的關聯域名，並且正確實施相關文檔中詳述的委派先決條件，則將接受這些委派。

## 停止服務

CLIENT隨時都能提出書面要求，不再從委派服務中受益，並自行進行必要的DNS設定。

如果發生這種情況，Adobe將向CLIENT提供估計值，詳細說明返回到非域委派模式所需的服務天數。

如Adobe未能遵守上述承諾，則將免除接受上述傳遞率的任何責任。

終止Marketing Cloud服務會自動導致網域委派結束，並依Adobe維護這些網域的DNS。

## 使用「控制面板」監控子網域

為您的執行個體設定子網域後，您就可以使用「控制面板」來監控子網域。

這可讓您檢視您委派給Adobe Campaign的所有子網域，並要求續約其SSL憑證。

如需詳細資訊，請參閱[專屬文件](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/monitoring-subdomains.html#subdomains-and-certificates)。

>[!NOTE]
>
>[控制](https://experienceleague.adobe.com/docs/control-panel/using/control-panel-home.html?lang=zh-Hant) 面板僅適用於使用Adobe Managed Services的客戶。
