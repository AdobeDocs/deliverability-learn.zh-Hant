---
title: SSL憑證要求程式
description: 了解如何在您委派給Adobe的子網域上安裝SSL憑證。
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: 8a78abd3-afba-49a7-a2ae-8b2c75326749
source-git-commit: d6094cd2ef0a8a7741e7d8aa4db15499fad08f90
workflow-type: tm+mt
source-wordcount: '2265'
ht-degree: 1%

---

# SSL 憑證請求流程

將域委派給Adobe以發送電子郵件後（請參閱[域名設定](/help/additional-resources/ac-domain-name-setup.md)）,Adobe將建立並使用某些子網域以執行特定功能。

例如，如果您已將&#x200B;*email.example.com*&#x200B;委派給Adobe來傳送電子郵件，則Adobe會建立子網域，例如：
* *t.email.example.com*  — 用於追蹤連結
* *m.email.example.com*  — 用於鏡像頁面
* *res.email.example.com*  — 適用於托管資源（例如影像）

建議您透過SSL(HTTPS)**保護這些網域。**&#x200B;事實上，不安全的連結(HTTP)容易遭到攔截，且會在現代瀏覽器上標示警告。

若要在這些子網域上安裝SSL憑證，此程式會要求CSR檔案，然後購買SSL憑證以供Adobe安裝或續約。

>[!CAUTION]
>
>安裝SSL憑證之前，請務必了解[本頁面](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/renewing-subdomain-certificate.html#installing-ssl-certificate)所列的必要條件。
>
>Adobe最多只支援2048位元憑證。 尚不支援4096位元憑證。

## 字彙

| 詞語 | 說明 |
|--- |--- |
| CA（證書頒發機構） | SSL憑證提供者，可在驗證組織或個人身分（例如DigiCert、Symantec等）後，向其核發數位憑證。<ul><li>受信任的CA通常被視為發行根證書的第三方CA。</li><li>如果憑證是由使用憑證的相同組織/公司簽署，則即使憑證是SSL憑證（例如自行簽署的憑證），仍會分類為不受信任的CA。</li></ul> |
| 鏈證書 | 包括根證書和一個或多個中間證書的證書稱為鏈（或鏈）證書。 |
| CSR（憑證申請檔） | 申請SSL憑證時，提供給憑證授權單位的編碼文字區塊。 它通常在安裝憑證的伺服器上產生。 |
| DER（可分辨編碼規則） | 證書擴展類型。 .der副檔名用於二進位DER編碼憑證。 這些檔案也可能支援.cer或.crt副檔名。 |
| EV（延伸驗證）憑證 | EV證書是一種新型證書，旨在防止網路釣魚攻擊。 需要對您的業務和訂購證書的人員進行擴展驗證。 |
| 高保證證書 | 在驗證域名所有權和有效業務註冊後，CA頒發高保證證書。 |
| 中間CA | 鏈式證書中包含的中間證書的證書頒發機構。 |
| 中繼憑證 | 憑證授權單位會以樹狀結構的形式發行憑證。 根證書是樹中最頂端的證書。 您的憑證和根憑證之間的任何憑證都稱為鏈結或中繼憑證。 |
| 低保證證書 | 低保證證書（也稱為域驗證證書）僅包括證書中的域名（而不包括業務/組織名稱）。 |
| PEM（隱私增強郵件） | 副檔名為.pem的憑證，包含ASCII(Base64)資料。 此類證書以「 — — — BEGIN CERTIFICATE — — - 」行開頭。 |
| 根憑證 | 憑證授權單位會以樹狀結構的形式發行憑證。 根證書是樹中最頂端的證書。 |
| SAN（主題替代名稱） | 主題替代名稱是其他主機名稱（網站、IP位址、通用名稱等） 應作為單一SSL憑證的一部分簽署。 |
| 自簽名證書 | 由建立者簽署的憑證，而非受信任的憑證授權單位。 自簽名證書可以啟用與CA簽名的證書相同級別的加密，但有兩個主要缺點：<ul><li>訪客的連線可能被劫持，使得攻擊者能夠查看所發送的所有資料（從而破壞了加密連接的目的）</li><li> 無法像受信任的證書一樣撤銷證書。</li></ul> |
| SSL（安全套接字層） | 用於在Web伺服器和瀏覽器之間建立加密連結的標準安全技術。 |
| 萬用字元憑證 | 萬用字元憑證可保護單一網域名稱（例如*.adobe.com）上不限數量的第一層子網域。 |

## 主要步驟

1. 要求憑證簽署要求(CSR)檔案，並提供必要的資訊（國家、州、城市、組織名稱、組織單位名稱等） Adobe。
1. 驗證由Adobe產生的CSR檔案，並確認您提供的所有資訊皆正確無誤。
1. 使用CSR詳細資訊生成由受信任的證書頒發機構<!--taking care of asking for using the subjectAltName SSL extension (SAN) if it is for several domain names, and get/purchase the resulting certificate (ideally) in PEM format for Apache server-->簽名的證書。
1. 驗證SSL憑證並確認其符合CSR。
1. 提供SSL憑證給Adobe，由誰安裝。
1. 測試每個安全子網域是否已成功安裝SSL憑證。
1. 監控SSL憑證有效期。
1. 更新Adobe Campaign中的任何特定設定。

## 詳細流程

### 先決條件

您必須識別網域名稱和函式（追蹤、鏡像頁面、網頁應用程式等） 來保護。
>[!NOTE]
>
>Adobe有助於定義要涉及的網域名稱和函式。 如需詳細資訊，請連絡您的Adobe客戶成功經理。

### 步驟1 — 取得CSR檔案

若要取得CSR（憑證簽署要求）檔案，請遵循下列步驟。

* 如果您可以存取[控制面板](https://experienceleague.adobe.com/docs/control-panel/using/control-panel-home.html?lang=zh-Hant)，請依照[本頁面](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/renewing-subdomain-certificate.html#subdomains-and-certificates)上的指示，從控制面板產生和下載CSR檔案。

* 否則，請透過https://adminconsole.adobe.com/建立支援票證，以向Adobe客戶服務取得必要子網域的CSR檔案。

以下是一些最佳實務可遵循：

* 為每個委派的子網域提出一個要求。
* 您可以將多個子網域合併為單一CSR請求，但僅限在相同環境中。 例如，在Campaign Classic中，行銷伺服器、[中間來源伺服器](https://experienceleague.adobe.com/docs/campaign-classic/using/installing-campaign-classic/install-campaign-on-prem/mid-sourcing-server.html)和[執行例項](https://experienceleague.adobe.com/docs/campaign-classic/using/transactional-messaging/configure-transactional-messaging/configuring-instances.html#execution-instance)是三個不同的環境。
* 您必須先取得新CSR，才能續約任何SSL憑證。 請勿使用一年前或更久以前的CSR檔案。

您需要提供下列資訊。

>[!CAUTION]
>
>必須填入下表中指出的所有欄位。 否則，將無法處理CSR要求。

**提供Adobe小組協助的資訊：**

| 要提供的資訊 | 範例值 | 注意 |
|--- |--- |--- |
| 用戶端名稱 | My Company Inc. | 組織名稱。 此欄位供Adobe用來追蹤您的請求（不屬於CSR/SSL憑證）。 |
| Adobe Campaign環境URL | https://client-mid-prod1.campaign.adobe.com | Adobe Campaign例項URL。 |
| 公用名[CN] | t.subdomain.customer.com | 這可以是任何相關網域，但通常是追蹤網域。 |
| 主題替代名稱[SAN] | t.subdomain.customer.com | 請務必將追蹤子網域納入為SAN。 |
| 主題替代名稱[SAN] | m.subdomain.customer.com |
| 主題替代名稱[SAN] | res.subdomain.customer.com |

**IT/SSL內部團隊要提供的資訊：**

| 要提供的資訊 | 範例值 | 注意 |
|--- |--- |--- |
| 國家/地區[C] | US | 這必須是兩個字母的代碼。 在此處](https://www.ssl.com/csrs/country_codes/)訪問完整的國家/地區清單。[</br>*注意：若為英國，請使用GB（而非UK）。* |
| 州（或省名）[ST] | 伊利諾州 | 如果適用。 值必須是完整名稱，而非縮寫。 |
| 城市/地點名稱[L] | 芝加哥 |
| 組織名稱[O] | ACME |
| 組織單位名稱[OU] | IT |

>[!NOTE]
>
>將「subdomain.customer.com」取代為您委派的子網域，而其他範例值則取代為適當的值。

### 步驟2 — 驗證CSR檔案

在提交您的要求並附上相關資訊後，Adobe會產生憑證申請檔(CSR)檔案，並提供您。

產生的CSR檔案中的文本必須以&#x200B;**&quot;—BEGIN CERTIFICATE REQUEST—&quot;**&#x200B;開頭。

從Adobe收到CSR檔案後，請遵循下列步驟：

1. 將CSR檔案文字複製並貼到線上解碼器中，例如https://www.sslshopper.com/csr-decoder.html、<!--https://www.certlogik.com/decoder/,-->或https://www.entrust.net/ssl-technical/csr-viewer.cfm。
或者，您也可以在Linux電腦上本地使用*OpenSSL*&#x200B;命令。 有關詳細資訊，請參閱[此外部頁面](https://www.question-defense.com/2009/09/22/use-openssl-to-verify-the-contents-of-a-csr-before-submitting-for-a-ssl-certificate)。
1. 確認所有檢查均成功。
1. 檢查是否包含正確的參數和域名。
1. 檢查所有其他資料是否符合您在提交請求時提供的詳細資訊。

### 步驟3 — 產生SSL憑證

提供CSR檔案後，您必須使用CSR檔案購買並產生適當網域的SSL憑證。

* SSL憑證：
   * 必須為Apache PEM格式；
   * 長度不應超過2048位元；
   * 必須由有效的CA（認證機構）簽署；
   * 必須包括CSR檔案中提及的所有SAN（主體替代名稱）。
* 如果有一或多個中間憑證，您必須提供根憑證和所有中間憑證以Adobe。
* 您可以設定任何憑證有效期，但Adobe建議您選擇足夠長的憑證（例如兩年）。

>[!NOTE]
>
>如果您使用自己的內部工具或CA提供的入口網站來要求憑證，請務必使用與CSR要求中提供的相同詳細資訊，以避免憑證產生程式中出現任何延遲或差異。

### 步驟4 — 驗證SSL憑證

產生SSL憑證後，您必須先驗證它，才能將其傳送至Adobe。 要執行此操作，請遵循下列步驟：

1. 確認憑證具有.pem副檔名。 若非如此，請將其轉換為PEM格式。 您可以使用&#x200B;*OpenSSL*&#x200B;進行轉換。
1. 確認憑證開頭為&#x200B;**&quot;—BEGIN CERTIFICATE—&quot;**。
1. 將憑證文字複製到線上解碼器，例如https://www.sslshopper.com/certificate-decoder.html或https://www.entrust.net/ssl-technical/csr-viewer.cfm。
或者，您也可以在Linux電腦上本地使用*OpenSSL*&#x200B;命令。 有關詳細資訊，請參閱[此外部頁面](https://www.shellhacks.com/decode-ssl-certificate/)。
1. 請確保證書正確解析，包括「公用名稱」、「SAN」、「頒發者」和「有效期」。
1. 如果SSL憑證驗證成功，請使用[此網站](https://www.sslshopper.com/certificate-key-matcher.html)檢查憑證是否符合CSR:選取&#x200B;**檢查CSR和憑證是否符合**，然後在對應欄位中輸入憑證和CSR。 它們應該匹配。

### 步驟5 — 要求安裝SSL憑證

* 如果您可以存取[控制面板](https://experienceleague.adobe.com/docs/control-panel/using/control-panel-home.html)，請依照[本頁面](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/renewing-subdomain-certificate.html#installing-ssl-certificate)上的指示，將憑證上傳至控制面板。

* 否則，請通過https://adminconsole.adobe.com/建立另一個支援票證，以請求Adobe在Adobe伺服器上安裝證書。

您需要提供：

* 證書檔案、根證書和任何中間證書（附加到票證），最好採用Apache PEM格式。
* 為CSR引發的先前支援票證的數量。
* 為CSR票證提供的相同資料（包括通用名稱、執行個體URL、州、城市/地點、組織名稱、組織單位名稱等）。

### 步驟6 — 測試SSL憑證安裝

安裝SSL憑證並經Adobe客戶服務確認後，請確定所有URL都已成功安裝。

在關閉SSL安裝票證之前，請執行以下測試。 也請務必依照[本區段](#update-configuration)的指示更新任何特定設定。

在您的瀏覽器中導覽至下列URL（將「subdomain.customer.com」取代為子網域）:

* https://subdomain.customer.com/r/test（僅適用於[web應用程式](https://experienceleague.adobe.com/docs/campaign-classic/using/designing-content/web-applications/about-web-applications.html)子網域 — 不適用於電子郵件子網域）
* https://t.subdomain.customer.com/r/test
* https://m.subdomain.customer.com/r/test
* https://res.subdomain.customer.com/r/test

成功的結果會提供環境資訊，而URL中的位址清單示連線安全。 例如，您可以在Google Chrome中看到下列訊息：

![](../../help/assets/ssl-google-successful-result.png)

如果未正確安裝SSL憑證，則會顯示下列警告：

![](../../help/assets/ssl-google-unsuccessful-result.png)

### 步驟7 — 檢查憑證有效期

您可以在瀏覽器中檢查憑證的有效期。 例如，在Google Chrome中，按一下&#x200B;**Secure** > **Certificate**。

您有責任檢查有效期。 Adobe建議您實作程式以監控憑證過期。 深入了解當您的SSL憑證在[中過期時會發生什麼事。本文](https://www.thesslstore.com/blog/what-happens-when-your-ssl-certificate-expires/)

* 建立支援票證，以在憑證到期日期前至少兩週請求更新的憑證。 除非CSR詳細資訊已變更，否則您不需要請求額外的CSR。

* 如果您可以存取[控制面板](https://experienceleague.adobe.com/docs/control-panel/using/control-panel-home.html)，並且您的環境是由AWS環境中的Adobe托管，則您可以使用控制面板在憑證過期之前續約憑證。 進一步了解[本節](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/monitoring-ssl-certificates.html#monitoring-certificates)。

### 步驟8 — 更新任何特定設定 {#update-configuration}

一旦您確定請求的SSL憑證已正確安裝，您就可以從HTTP更新Adobe Campaign中的所有參考，改為HTTPS。

>[!NOTE]
>
>對於Campaign Classic，要更新的URL主要位於[部署嚮導](https://experienceleague.adobe.com/docs/campaign-classic/using/installing-campaign-classic/initial-configuration/deploying-an-instance.html#deployment-wizard)和[外部帳戶](https://experienceleague.adobe.com/docs/campaign-classic/using/installing-campaign-classic/accessing-external-database/external-accounts.html)（跟蹤、鏡像頁和公共資源域）中。 如需Campaign Standard，請參閱[品牌設定](https://experienceleague.adobe.com/docs/campaign-standard/using/administrating/application-settings/branding.html#about-brand-identity)。

更新設定後，新電子郵件將會以HTTPS URL傳送，而非HTTP。 若要檢查URL現在是否安全，您可以快速執行下列測試：

* 從Adobe Campaign上傳影像。 上傳影像後，傳回的URL應為HTTPS。
* 建立測試電子郵件傳送，包括鏡像頁面連結、某些影像、文字和取消訂閱連結。 傳送電子郵件至外部電子郵件ID（例如您的Gmail地址）。 收到訊息後，請開啟電子郵件，並確認電子郵件內的所有連結都以HTTPS格式（非HTTP）正確開啟，且沒有任何SSL憑證警告或錯誤。

## 產品特定資源

**Campaign Classic**

* [控制面板：新增SSL憑證（教學課程）](https://experienceleague.adobe.com/docs/campaign-classic-learn/control-panel/subdomains-and-certificates/adding-ssl-certificates.html)  — 了解如何新增SSL憑證，以保護您的子網域。

**Campaign Standard**

* [控制面板：新增SSL憑證（教學課程）](https://experienceleague.adobe.com/docs/campaign-standard-learn/control-panel/subdomains-and-certificates/adding-ssl-certificates.html)  — 了解如何新增SSL憑證，以保護您的子網域。
