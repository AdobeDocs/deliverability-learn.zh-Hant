---
title: SSL憑證請求流程
description: 瞭解如何在委派給Adobe的子網域上安裝SSL憑證。
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: 8a78abd3-afba-49a7-a2ae-8b2c75326749
source-git-commit: 57016f89df54d5c74755a6a108a92db45153ec18
workflow-type: tm+mt
source-wordcount: '2124'
ht-degree: 1%

---

# SSL 憑證請求流程

一旦您將網域委派給Adobe以傳送電子郵件（請參閱[網域名稱設定](/help/additional-resources/ac-domain-name-setup.md)），Adobe將會針對特定功能建立並使用特定子網域。

例如，如果您已委派&#x200B;*email.example.com*&#x200B;給Adobe傳送電子郵件，Adobe將會建立子網域，如下所示：
* *t.email.example.com* — 用於追蹤連結
* *m.email.example.com* — 映象頁面
* *res.email.example.com* — 用於裝載的資源（例如影像）

建議透過SSL (HTTPS)**來**&#x200B;保護這些網域。 事實上，不安全的連結(HTTP)很容易遭到攔截，並會在現代瀏覽器上標示警告。

若要在這些子網域上安裝SSL憑證，此程式包括請求CSR檔案，接著購買SSL憑證以供Adobe安裝或續約。

>[!CAUTION]
>
>在安裝SSL憑證之前，請確定您知道[此頁面](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/renew-ssl/renewing-subdomain-certificate.html?lang=zh-Hant)上列出的先決條件。
>
>Adobe僅支援最多2048位元憑證。 尚未支援4096位元憑證。

## 字彙

| 詞語 | 說明 |
|--- |--- |
| CA （憑證授權單位） | SSL憑證提供者，可在驗證組織或個人（例如DigiCert、Symantec等）的身分後，向其發行數位憑證。<ul><li>受信任的CA通常被視為簽髮根憑證的第三方CA。</li><li>如果憑證是由使用該憑證的相同組織/公司簽署，則即使它們是SSL憑證（例如自我簽署憑證），也會將其分類為不受信任的CA。</li></ul> |
| 鏈結憑證 | 包含根憑證和一個或多個中間憑證的憑證稱為鏈結（或鏈結）憑證。 |
| CSR （憑證申請檔） | 申請SSL憑證時，為憑證授權單位提供的編碼文字區塊。 它通常會在安裝憑證的伺服器上產生。 |
| DER （辨別編碼規則） | 憑證延伸型別。 .der副檔名用於二進位DER編碼的憑證。 這些檔案也可能支援.cer或.crt副檔名。 |
| EV （延伸驗證）憑證 | EV憑證是一種新型憑證，專門用來防止網路釣魚攻擊。 這需要對您的企業和訂購憑證的人員進行延伸驗證。 |
| 高保證憑證 | CA在驗證網域名稱的所有權和有效的企業註冊後會核發高保證憑證。 |
| 中繼CA | 包含在鏈憑證中的中間憑證的憑證授權單位。 |
| 中繼憑證 | 憑證授權單位會以樹狀結構的形式發行憑證。 根憑證是樹狀結構的最上層憑證。 憑證與根憑證之間的任何憑證稱為鏈結或中繼憑證。 |
| 低保證憑證 | 低保證憑證（也稱為網域驗證憑證）僅包括憑證中的網域名稱（不包括企業/組織名稱）。 |
| PEM （隱私權增強郵件） | 副檔名為.pem的憑證，其中包含ASCII (Base64)資料。 這類憑證以「 — — — 開始憑證 — — — 」行開頭。 |
| 根憑證 | 憑證授權單位會以樹狀結構的形式發行憑證。 根憑證是樹狀結構的最上層憑證。 |
| SAN （主體替代名稱） | 主旨的替代名稱是其他主機名稱（網站、IP位址、通用名稱等） 應該簽署為單一SSL憑證的一部分。 |
| 自我簽署憑證 | 由建立者簽署的憑證，而非受信任的憑證授權單位。 自我簽署憑證可以啟用與CA簽署的憑證相同等級的加密，但有兩個主要缺點：<ul><li>訪客的連線可能會被劫持，使得攻擊者能夠檢視所有傳送的資料（因而降低了加密連線的目的）</li><li> 無法像受信任的憑證一樣撤銷憑證。</li></ul> |
| SSL （安全通訊端層） | 在網頁伺服器和瀏覽器之間建立加密連結的標準安全性技術。 |
| 萬用字元憑證 | 萬用字元憑證可保護單一網域名稱(例如*.adobe.com)上不限數量的一級子網域。 |

## 主要步驟

1. 要求憑證簽署要求(CSR)檔案，並提供必要資訊（國家、州、城市、組織名稱、組織單位名稱等） 以Adobe。
1. 驗證Adobe產生的CSR檔案，並驗證您提供的所有資訊是否正確。
1. 使用CSR詳細資料來產生由受信任的憑證授權單位<!--taking care of asking for using the subjectAltName SSL extension (SAN) if it is for several domain names, and get/purchase the resulting certificate (ideally) in PEM format for Apache server-->簽署的憑證。
1. 驗證SSL憑證，並確認其符合CSR。
1. 提供SSL憑證給Adobe，由他們進行安裝。
1. 測試每個安全子網域的SSL憑證是否已成功安裝。
1. 監視SSL憑證有效期。
1. 更新Adobe Campaign中的任何特定設定。

## 詳細程式

### 先決條件

您必須識別網域名稱和功能（追蹤、映象頁面、網頁應用程式等） 以保護。
>[!NOTE]
>
>Adobe可協助定義要涉及的網域名稱和函式。 如需詳細資訊，請聯絡您的Adobe客戶團隊。

### 步驟1 — 取得CSR檔案

若要取得CSR （憑證申請檔）檔案，請遵循下列步驟。

* 如果您可以存取[控制面板](https://experienceleague.adobe.com/docs/control-panel/using/control-panel-home.html?lang=zh-Hant)，請依照[此頁面](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/renew-ssl/renewing-subdomain-certificate.html?lang=zh-Hant#subdomains-and-certificates)上的指示，從「控制面板」產生並下載CSR檔案。

* 否則，請透過https://adminconsole.adobe.com/建立支援票證，以從Adobe客戶服務取得所需子網域的CSR檔案。

以下是一些要遵循的最佳實務：

* 對每個委派的子網域引發一個請求。
* 您可以將多個子網域合併為單一CSR請求，但僅限於相同環境中。 例如，在Campaign Classic中，行銷伺服器、[中間來源伺服器](https://experienceleague.adobe.com/docs/campaign-classic/using/installing-campaign-classic/install-campaign-on-prem/mid-sourcing-server.html?lang=zh-Hant)和[執行執行個體](https://experienceleague.adobe.com/docs/campaign-classic/using/transactional-messaging/configure-transactional-messaging/configuring-instances.html?lang=zh-Hant#execution-instance)是三個不同的環境。
* 在續約SSL憑證之前，您必須先取得新的CSR。 請勿使用一年或更久以前的舊CSR檔案。

您需要提供下列資訊。

>[!CAUTION]
>
>必須填寫下表中的所有欄位。 否則，將無法處理CSR請求。

**要提供Adobe小組協助的資訊：**

| 要提供的資訊 | 範例值 | 備註 |
|--- |--- |--- |
| 用戶端名稱 | 我的公司 | 您的組織名稱。 Adobe會使用此欄位來追蹤您的請求（不會成為CSR/SSL憑證的一部分）。 |
| Adobe Campaign環境URL | https://client-mid-prod1.campaign.adobe.com | Adobe Campaign執行個體URL。 |
| 一般名稱[CN] | t.subdomain.customer.com | 這可以是任何相關的網域，但通常是追蹤網域。 |
| 主體替代名稱[SAN] | t.subdomain.customer.com | 請務必加入追蹤子網域做為SAN。 |
| 主體替代名稱[SAN] | m.subdomain.customer.com |
| 主體替代名稱[SAN] | res.subdomain.customer.com |

**要由您的IT/SSL內部團隊提供的資訊：**

| 要提供的資訊 | 範例值 | 備註 |
|--- |--- |--- |
| 國家/地區[C] | US | 這必須是雙字母代碼。 在[這裡](https://www.ssl.com/csrs/country_codes/)存取完整的國家清單。</br>*注意：如果是英國，請使用GB （不是UK）。* |
| 州/省（名稱） [ST] | 伊利諾 | 若適用。 值必須是完整名稱，而非縮寫。 |
| 城市/地區名稱[L] | 芝加哥 |
| 組織名稱[O] | ACME |
| 組織單位名稱[OU] | IT |

>[!NOTE]
>
>將「subdomain.customer.com」取代為您委派的子網域，並將其他範例值取代為適當的值。

### 步驟2 — 驗證CSR檔案

提交具有相關資訊的請求後，Adobe會產生憑證簽署請求(CSR)檔案並提供給您。

產生的CSR檔案中的文字必須以&#x200B;**&quot;-----BEGIN CERTIFICATE REQUEST-----&quot;**&#x200B;開頭。

從Adobe收到CSR檔案後，請遵循以下步驟：

1. 將CSR檔案文字複製並貼到線上解碼器，例如https://www.sslshopper.com/csr-decoder.html、<!--https://www.certlogik.com/decoder/,-->或https://www.entrust.net/ssl-technical/csr-viewer.cfm。
或者，您也可以在Linux機器上本機使用*OpenSSL*&#x200B;命令。
1. 確認所有檢查都成功。
1. 檢查是否包含正確的引數和網域名稱。
1. 檢查所有其他資料是否符合您在提交請求時提供的詳細資料。

### 步驟3 — 產生SSL憑證

提供CSR檔案後，您必須使用CSR檔案為適當的網域購買並產生SSL憑證。

* SSL憑證：
   * 必須為Apache PEM格式；
   * 長度不應超過2048位元；
   * 必須由有效的CA （憑證授權單位）簽署；
   * 必須包括CSR檔案中提到的所有SAN （主體替代名稱）。
* 如果有一個或多個中間憑證，您必須提供根憑證和所有要Adobe的中間憑證。
* 您可以設定任何憑證有效期，但Adobe建議選擇足夠長的時間（例如兩年）。

>[!NOTE]
>
>如果您使用自己的內部工具或CA提供的入口網站來要求憑證，請務必使用與CSR要求中相同的詳細資料，以避免憑證產生程式的任何延遲或差異。

### 步驟4 — 驗證SSL憑證

產生SSL憑證後，您必須先驗證該憑證，才能將其傳送到Adobe。 要執行此操作，請遵循下列步驟：

1. 請確定憑證的副檔名為.pem。 如果不是這種情況，請將其轉換為PEM格式。 您可以使用&#x200B;*OpenSSL*&#x200B;進行轉換。
1. 確認憑證的開頭為&#x200B;**&quot;-----BEGIN CERTIFICATE-----&quot;**。
1. 將憑證文字複製到線上解碼器中，例如https://www.sslshopper.com/certificate-decoder.html或https://www.entrust.net/ssl-technical/csr-viewer.cfm。
或者，您也可以在Linux機器上本機使用*OpenSSL*&#x200B;命令。 如需詳細資訊，請參閱[此外部頁面](https://www.shellhacks.com/decode-ssl-certificate/)。
1. 請確定憑證正確解析，包括一般名稱、SAN、簽發者和有效期。
1. 如果SSL憑證驗證成功，請使用[此網站](https://www.sslshopper.com/certificate-key-matcher.html)檢查憑證是否與CSR相符：選取&#x200B;**檢查CSR和憑證是否相符**，並在對應的欄位中輸入您的憑證和您的CSR。 它們應該相符。

### 步驟5 — 要求SSL憑證安裝

* 如果您可以存取[控制面板](https://experienceleague.adobe.com/docs/control-panel/using/control-panel-home.html?lang=zh-Hant)，請依照[此頁面](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/renew-ssl/renewing-subdomain-certificate.html?lang=zh-Hant)上的指示，將憑證上傳至控制面板。

* 否則，請透過https://adminconsole.adobe.com/建立另一個支援票證，以要求Adobe在Adobe伺服器上安裝憑證。

您需要提供：

* 憑證檔案、根憑證和任何中間憑證（附加到票證），最好是Apache PEM格式。
* 為CSR引發的上一個支援票證編號。
* 與CSR票證提供的資料相同（包括一般名稱、執行個體URL、州、城市/地區、組織名稱、組織單位名稱等）。

### 步驟6 — 測試SSL憑證安裝

安裝SSL憑證並由Adobe客戶服務確認後，請確定已針對所有URL成功安裝SSL憑證。

在關閉SSL安裝票證之前，請執行以下測試。 也請確定您依照[此區段](#update-configuration)中的指示更新任何特定組態。

導覽至瀏覽器中的下列URL （將「subdomain.customer.com」取代為您的子網域）：

* https://subdomain.customer.com/r/test （僅適用於[網頁應用程式](https://experienceleague.adobe.com/docs/campaign-classic/using/designing-content/web-applications/about-web-applications.html?lang=zh-Hant)子網域 — 不適用於電子郵件子網域）
* https://t.subdomain.customer.com/r/test
* https://m.subdomain.customer.com/r/test
* https://res.subdomain.customer.com/r/test

成功的結果會提供環境資訊，而URL中的位址列則表示連線是安全的。 例如，您會在Google Chrome中看到下列訊息：

![](../../help/assets/ssl-google-successful-result.png)

如果SSL憑證未正確安裝，會顯示下列警告：

![](../../help/assets/ssl-google-unsuccessful-result.png)

### 步驟7 — 檢查憑證有效期

您可以在瀏覽器中檢查憑證的有效期。 例如，在Google Chrome中，按一下&#x200B;**安全** > **憑證**。

您有責任檢查有效期間。 Adobe建議您實作程式來監視憑證到期。 深入瞭解當您的SSL憑證於[本文](https://www.thesslstore.com/blog/what-happens-when-your-ssl-certificate-expires/)到期時會發生什麼事情。

* 建立支援票證，以便在憑證到期日之前至少兩週要求更新的憑證。 除非CSR詳細資料已變更，否則您不需要請求其他CSR。

* 如果您有[控制面板](https://experienceleague.adobe.com/docs/control-panel/using/control-panel-home.html?lang=zh-Hant)的存取權，而且您的環境是由AWS環境中的Adobe所託管，您可以使用「控制面板」在憑證過期前更新憑證。 請參閱[此章節](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/monitoring-ssl-certificates.html?lang=zh-Hant#monitoring-certificates)深入瞭解。

### 步驟8 — 更新任何特定設定 {#update-configuration}

在您確信請求的SSL憑證已正確安裝後，您就可以將Adobe Campaign中的所有參考從HTTP更新為HTTPS。

>[!NOTE]
>
>對於Campaign Classic，要更新的URL主要位於[部署精靈](https://experienceleague.adobe.com/docs/campaign-classic/using/installing-campaign-classic/initial-configuration/deploying-an-instance.html?lang=zh-Hant#deployment-wizard)和[外部帳戶](https://experienceleague.adobe.com/docs/campaign-classic/using/installing-campaign-classic/accessing-external-database/external-accounts.html?lang=zh-Hant) （追蹤、映象頁面和公用資源網域）中。 如需Campaign Standard，請參閱[品牌組態](https://experienceleague.adobe.com/docs/campaign-standard/using/administrating/application-settings/branding.html?lang=zh-Hant#about-brand-identity)。

更新設定後，新電子郵件將使用HTTPS URL而不是HTTP傳送。 若要檢查URL現在是否安全，您可以快速執行下列測試：

* 從Adobe Campaign上傳影像。 上傳影像後，傳回的URL應該是HTTPS。
* 建立測試電子郵件傳遞，包括映象頁面連結、某些影像、文字和取消訂閱連結。 將電子郵件傳送至外部電子郵件ID （例如您的Gmail地址）。 收到後，請開啟電子郵件並確保電子郵件內的所有連結都以其HTTPS表單（而非HTTP）正確開啟，不會出現任何SSL憑證警告或錯誤。

## 產品特定資源

**Campaign Classic**

* [控制面板：新增SSL憑證（教學課程）](https://experienceleague.adobe.com/docs/campaign-classic-learn/control-panel/subdomains-and-certificates/adding-ssl-certificates.html?lang=zh-Hant) — 瞭解如何新增SSL憑證來保護您的子網域。

**Campaign Standard**

* [控制面板：新增SSL憑證（教學課程）](https://experienceleague.adobe.com/docs/campaign-standard-learn/control-panel/subdomains-and-certificates/adding-ssl-certificates.html?lang=zh-Hant) — 瞭解如何新增SSL憑證來保護您的子網域。
