---
title: 網域名稱設定
description: 瞭解如何將子網域委派至Adobe Campaign。
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: 4d52d197-d20e-450c-bfcf-e4541c474be4
source-git-commit: 82f7254a9027f79d2af59aece81f032105c192d5
workflow-type: tm+mt
source-wordcount: '2043'
ht-degree: 2%

---

# 網域名稱設定

本檔案說明網域名稱設定和委派的業務和技術需求。 您將需要選取電子郵件傳送子網域，以及選擇性地選取對外子網域，以託管您所使用Adobe平台的網頁元件（登陸頁面、退出頁面）。

>[!NOTE]
>
>您也可以使用「控制面板」（提供測試版）設定新子網域。 請參閱[此章節](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/setting-up-new-subdomain.html?lang=zh-Hant#must-read)深入瞭解。

## 子網域

透過Adobe，數位行銷可真正成為內容引擎，為您的品牌的客戶參與行銷方案提供動力。  電子郵件仍然是數位行銷計畫的基礎。 然而，前往收件匣已變得前所未有的困難。

建立電子郵件行銷活動的子網域可讓品牌將各種型別的流量（例如行銷與企業）隔離到特定的IP集區和具有特定網域，這將加快[IP預熱流程](../../help/additional-resources/increase-reputation-with-ip-warming.md)並改善整體的傳遞能力。 如果您共用網域，且網域遭到封鎖或新增至封鎖清單，可能會影響您的公司郵件傳送。 不過，您電子郵件行銷通訊專屬網域上的信譽問題或封鎖將只會影響該電子郵件流程。  使用您的主網域作為寄件者或多個郵件串流的「寄件者」地址也會破壞電子郵件驗證，導致您的郵件遭到封鎖或放入垃圾郵件資料夾。

### 委派

網域名稱委派是一種方法，可讓網域名稱的所有者（技術上稱為DNS區域）將其細分（技術上稱為DNS區域下的細分）委派給另一個實體。 基本上，如果客戶正在處理區域「example.com」，他可以將子區域「marketing.example.com」委派給Adobe Campaign。

這表示Adobe Campaign的DNS伺服器將僅對該區域擁有完整許可權，而不會在頂層網域上擁有完整許可權。 Adobe Campaign的DNS伺服器會針對該區域中網域名稱的查詢提供權威解答，例如「t.marketing.example.com」本身，而不是「www.example.com」。

透過委派子網域以與Adobe Campaign搭配使用，客戶可以依賴Adobe來維護所需的DNS基礎架構，以符合其電子郵件行銷傳送網域的產業標準傳遞能力要求，同時繼續維護和控制內部電子郵件網域的DNS。  子網域委派允許：

使用網域名稱的DNS別名來保留品牌形象的使用者端
自主實施所有技術最佳實務的Adobe，以在傳送電子郵件期間完全最佳化傳遞能力

## DNS設定選項

為了提供雲端型受管服務，Adobe強烈建議客戶在部署Adobe Campaign時使用子網域委派。  不過，Adobe確實為客戶提供設定DNS的替代選項 — CNAME設定。

| 選項 | 說明 | Adobe責任 | 客戶責任 |
|--- |------- |--- |--- |
| 將子網域委派給Adobe Campaign | 使用者端將子網域(email.example.com)委派給Adobe。 在此案例中，Adobe可以控制並維護傳遞、演算及追蹤電子郵件行銷活動所需的DNS的各個層面，以受管理的方式提供Campaign。 | 完整管理子網域和Adobe Campaign所需的所有DNS記錄。 | 將子網域正確委派給Adobe |
| 使用CNAME | Client會建立子網域並使用CNAME指向Adobe特定記錄。  若使用此設定，Adobe 和客戶都有責任維護 DNS。 | 管理Adobe Campaign所需的DNS記錄。 | 建立和控制子網域，以及建立/管理Adobe Campaign所需的CNAME記錄。 |

## 必要的DNS記錄

| 記錄型別 | 目的 | 範例記錄/內容 |
|--- |--- |--- |
| MX | 指定內送郵件的郵件伺服器 | <i>email.example.com</i></br><i>10 inbound.email.example.com</i> |
| SPF (TXT) | 寄件者原則架構 | <i>email.example.com</i></br>&quot;v=spf1 redirect=__spf.campaign.adobe.com&quot; |
| DKIM (TXT) | DomainKeys識別的郵件 | <i>使用者端。_domainkey.email.example.com</i></br>&quot;v=DKIM1； k=rsa；&quot; &quot;DKIMPUBLICKEY HERE&quot; |
| 主機記錄(A) | 映象頁面、影像託管和追蹤連結，所有傳送網域 | 123.111.100.99中的m.email.example.com 123.111.100.97中的</br>t.email.example.com 123.111.100.98中的</br>email.example.com |
| 反向DNS (PTR) | 將使用者端IP位址對應至使用者端品牌主機名稱 | 18.101.100.192.in-addr.arpa網域名稱指標r18.email.example.com |
| CNAME | 提供另一個網域名稱的別名 | t1.email.example.com是t1.email.example.campaign.adobe.com的別名 |


建議使用網域型訊息驗證、報告和符合性(DMARC)來驗證郵件寄件者，並確保目的地電子郵件系統信任從您的網域傳送的訊息。

DMARC TXT記錄範例：

```
_dmarc.email.example.com

“v=DMARC1; p=none; rua=mailto:mailauth-reports@myemail.com” 
```

您可以手動實作DMARC或聯絡Adobe，協助您為品牌設定DMARC。

## 設定需求

### 子網域委派

這要求使用者端在其DNS伺服器中建立子網域，並將此子網域的名稱伺服器定義為Adobe所維護的伺服器。  例如，主要網域名稱為「example.com」的使用者端，如果想要將「marketing.example.com」的管理委派給Adobe其電子郵件傳送，則必須具體化此委派，才能將下列型別記錄新增至其DNS：

```
marketing.example.com. NS a.ns.campaign.adobe.com.
marketing.example.com. NS b.ns.campaign.adobe.com.
marketing.example.com. NS c.ns.campaign.adobe.com.
marketing.example.com. NS d.ns.campaign.adobe.com.
```

委派網域名稱表示此網域將專門用於透過Adobe Campaign平台傳送電子郵件，因此不能用於其他方式（例如，從其他電子郵件基礎結構傳送電子郵件）。

在設定過程中，Adobe將確保網域附加到Adobe傳入電子郵件基礎架構，以便管理和處理傳回這些網域的反彈電子郵件（MX型別DNS記錄設定）。

### 使用CNAME

如果使用者端選擇使用CNAME而不是將子網域委派給Adobe，則在設定階段中，Adobe將提供要放置在使用者端DNS伺服器中的記錄，並將在Adobe Campaign DNS伺服器中設定對應的值。

## 部署的一般需求

實作新的企業行銷解決方案時，需要面對外部的元件。  這些功能包括託管登入頁面和網路表單、設定要追蹤的連結和網頁、顯示映象頁面，以及設定退出頁面。

雖然這些需求是透過Adobe和客戶託管的元件來管理，但它們包含電子郵件收件者可以看到的URL。  為避免具有指出基礎技術解決方案或託管提供者的URL，可以設定子網域以讓此對電子郵件的收件者透明。  例如，檢視http://www.customer.com/之類的URL時，網域將是「www.customer.com」。  其子網域會是「www」。

### 子網域需求

決定要用於Adobe Campaign應用程式中的品牌URL （映象頁面和追蹤URL）的子網域。  同時決定電子郵件傳遞的每個子網域的「寄件者地址」、「寄件者名稱」和「回覆地址」為何。

請填妥下表，第一行只是一個範例。

| 子網域 | 寄件者地址 | 來自名稱 | 回覆地址 |
|--- |--- |--- |--- |
| emails.customer.com | news@emails.customer.com | 客戶 | customercare@customer.com |
| </br> | </br> | </br> | </br> |

>[!NOTE]
>
>* 「回覆地址」欄位的用途是當您希望收件者回覆與「寄件者地址」不同的地址時。  雖然不是必要欄位，但Adobe強烈建議「回覆地址」為有效並連結至受監視的信箱。  此信箱必須由客戶代管。  它可以是支援信箱，例如customercare@customer.com，可讀取和回應電子郵件。
>* 若客戶未選擇任何「回覆地址」，則預設地址一律為`<tenant>-<type>-<env>@<subdomain>`。
>* 當以這種方式設定「回覆地址」時，回覆將會傳送至未受監視的信箱。
>* 從Adobe Campaign傳送電子郵件時，系統不會監控「寄件者地址」信箱，且行銷使用者無法存取此信箱。 Adobe Campaign也不提供自動回覆或自動轉寄此信箱中接收之電子郵件的功能。
>* 行銷活動寄件者/寄件者地址及錯誤地址不能是「濫用」或「郵遞員」。

## 委派子網域

選取用於Adobe Campaign平台的子網域必須透過建立四個名稱伺服器(NS)記錄來委派。  這可讓子網域正確委派給Adobe。  以下是子網域委派和個別DNS指示的範例。  請將「emails.customer.com」取代為您要委派的子網域。  請注意，子網域必須是唯一的，而且不能被其他合作對象使用（例如，現有的ESP或MSP）。

| 委派的子網域 | DNS指示 |
|--- |--- |
| `<subdomain>` | `<subdomain>` NS a.ns.campaign.adobe.com。</br> `<subdomain>` NS b.ns.campaign.adobe.com。</br> `<subdomain>` NS c.ns.campaign.adobe.com。</br> `<subdomain>` NS d.ns.campaign.adobe.com。 |

## 追蹤、映象頁面、資源

在電子郵件傳送子網域正確委派給Adobe Campaign後，AdobeTechOps團隊將建立兩個或多個較低層級網域，以獨立管理追蹤和映象頁面。

| 類型 | 網域 |
|--- |--- |
| 映象頁面 | m.`<subdomain>` |
| 追蹤 | t.`<subdomain>` |
| 資源 | res.`<subdomain>` |

## 雲端部署（選填）

這僅適用於Adobe Campaign Classic已由Adobe完全託管在雲端的情況。  這是選用的設定。

任何要開發的調查、網路表單和登入頁面，都會透過Adobe Campaign管理，並在雲端完全託管。  如有需要，可將其他子網域委派給Adobe (例如web.customer.com)，以用於工具內的任何Web元件。  請注意，子網域必須是唯一的，且不能由另一方使用（例如，現有的ESP或MSP）。

| 委派的子網域 | DNS指示 |
|--- |--- |
| `<subdomain>` | `<subdomain>` NS a.ns.campaign.adobe.com。</br>`<subdomain>` NS b.ns.campaign.adobe.com。</br>`<subdomain>` NS c.ns.campaign.adobe.com。</br>`<subdomain>` NS d.ns.campaign.adobe.com。 |

>[!NOTE]
>
>根據預設，工具中的任何Web元件都會使用委派用於電子郵件的初始子網域。

## 雲端傳訊部署（選填）

如果Adobe Campaign Classic行銷執行個體託管於客戶內部部署，則客戶必須完成其他技術設定。

任何要開發的調查、網路表單和登入頁面，都會透過存在收件者記錄的Adobe Campaign行銷執行個體來管理。

需要額外的CNAME DNS設定，才能部署由Adobe Campaign行銷執行個體託管的對外網路元件。  如此可讓網際網路公開存取網頁元件(例如web.customer.com)，並加上客戶網域的品牌。

防火牆也需設定為允許存取代管這些Web元件的Adobe Campaign行銷執行個體（在連線埠80或443上）。

**最佳實務Recommendations：**

客戶可以看到託管Web元件的子網域，因此請務必正確加上品牌且容易記憶，因為可能需要手動輸入，例如：https://web.customer.com。
如果需要將任何表單託管在安全頁面(HTTPS)上，則需要新增技術設定，如下所述。

| 委派的子網域 | DNS指示 |
|--- |--- |
| `<subdomain>` | `<subdomain>` CNAME `<internal customer server>` |

## 已提供的服務

在這些委派後，Adobe建立的基礎結構會確保為每個委派或CNAME別名傳送網域執行下列服務：

* 建立郵局主管@和濫用@收件匣
* 為委派網域設定意見回圈
* 在收到要求時，Adobe也會依照指定設定DMARC記錄。 您的傳遞顧問可以協助設計長期DMARC政策，並為您傳送的網域進行規劃。
由Adobe建立的引數只有在委派完成並由Adobe驗證之後才有效，並且仍可運作。  所有Adobe Campaign雲端選件都包含標準網域名稱委派。

## 帳單與實作條件

* 根據初始合約及選取的封裝型別，除了此初始委派以外的標準委派外，可能還包括其他委派，
* 除了這些包括的委派，將會向其他委派收費，
* 這些額外委派的計費方法會按照初始合約中的規定，每月額外收取費用。

如果使用者端選擇透過Adobe Campaign工具專用於傳遞的相關網域名稱，且已正確實施相關檔案中詳述的委派先決條件，則接受這些委派。

## 服務終止

使用者端隨時都可以提出書面要求，要求不再從委派服務中受益，並自行承擔必要的DNS設定。

如果發生此情況，Adobe會提供客戶端的估計值，詳細說明返回非網域委派模式所需的服務天數。

若客戶未能遵守上述承諾，上述交貨率之任何責任將免除Adobe。

終止Marketing Cloud服務將自動導致網域委派的結束，並按Adobe維護這些網域的DNS。

## 使用「控制面板」監視子網域

為您的執行個體設定子網域後，您就可以使用「控制面板」監控子網域。

這可讓您檢視委派給Adobe Campaign的所有子網域，以及要求續約其SSL憑證。

如需詳細資訊，請參閱[專屬文件](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/monitoring-subdomains.html?lang=zh-Hant#subdomains-and-certificates)。

>[!NOTE]
>
>[控制面板](https://experienceleague.adobe.com/docs/control-panel/using/control-panel-home.html?lang=zh-Hant)僅供使用AdobeManaged Services的客戶使用。
