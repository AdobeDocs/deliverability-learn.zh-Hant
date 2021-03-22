---
title: SSL憑證要求程式
description: 瞭解如何在您委派給Adobe的子網域上安裝SSL憑證。
feature: 付諸實踐
topics: Deliverability
kt: null
thumbnail: null
doc-type: article
activity: understand
team: ACS
translation-type: tm+mt
source-git-commit: 96ed84da391faaabd3001ddd6a411ddc1f46b033
workflow-type: tm+mt
source-wordcount: '2269'
ht-degree: 0%

---


# SSL憑證要求程式

將域委託給Adobe發送電子郵件後（請參閱[域名設定](/help/putting-it-in-practice/ac-domain-name-setup.md)）,Adobe將建立並使用某些子域執行特定功能。

例如，如果您已將&#x200B;*email.example.com*&#x200B;委託給Adobe發送電子郵件，Adobe將建立子域，例如：
* *t.email.example.com* -用於追蹤連結
* *m.email.example.com* - for mirror pages
* *res.email.example.com* -代管資源（例如影像）

建議&#x200B;**透過SSL(HTTPS)**&#x200B;保護這些網域。 事實上，不安全的連結(HTTP)很容易遭到攔截，並會在現代瀏覽器上標示警告。

若要在這些子網域上安裝SSL憑證，此程式需要請求CSR檔案，然後購買SSL憑證以Adobe安裝或續約。

>[!CAUTION]
>
>在安裝SSL憑證之前，請務必注意本頁[所列的必要條件。](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/renewing-subdomain-certificate.html#installing-ssl-certificate)
>
>Adobe僅支援最多2048位元憑證。 4096位元憑證尚未受支援。

## 字彙表

| 詞語 | 說明 |
|--- |--- |
| CA（認證機構） | SSL憑證提供者，在驗證組織或個人身分（例如DigiCert、Symantec等）後，向其核發數位憑證。<ul><li>受信任的CA通常被視為發行根證書的第三方CA。</li><li>如果憑證是由使用憑證的同一組織／公司所簽署，則即使憑證是SSL憑證（例如自簽證），也會將其分類為不受信任的CA。</li></ul> |
| 鏈條證書 | 包括根證書和一個或多個中間證書的證書稱為鏈（或鏈）證書。 |
| CSR（認證簽署要求） | 申請SSL憑證時提供給憑證授權機構的編碼文字區塊。 它通常在安裝憑證的伺服器上產生。 |
| DER（可分辨編碼規則） | 證書擴展類型。 .der擴充名用於二進位DER編碼證書。 這些檔案也可能支援。cer或。crt副檔名。 |
| EV（擴展驗證）證書 | EV證書是一種新型的證書，旨在防止網路釣魚攻擊。 這需要對您的業務和訂購證書的人員進行擴展驗證。 |
| 高保證證書 | CA在驗證網域名稱的所有權和有效的業務註冊後，會核發高保證證書。 |
| 中級CA | 鏈證書中包含的中間證書的證書頒發機構。 |
| 中級憑證 | 憑證授權機構以樹狀結構的形式發行憑證。 根證書是樹的最頂端證書。 您的憑證和根憑證之間的任何憑證都稱為鏈式或中間憑證。 |
| 低保證憑證 | 低保證證書（也稱為域驗證證書）僅包括證書中的域名（而不包括業務／組織名稱）。 |
| PEM（隱私權增強郵件） | 副檔名為。pem的憑證，包含ASCII(Base64)資料。 此類證書以「 - - - - - - - - BEGIN CERTIFICATE - - - - - 」行開頭。 |
| 根證書 | 憑證授權機構以樹狀結構的形式發行憑證。 根證書是樹的最頂端證書。 |
| SAN（主題替代名稱） | 主題替代名稱是其他主機名稱（站點、IP地址、公用名稱等） 應作為單一SSL憑證的一部分進行簽署。 |
| 自簽名憑證 | 由建立證書的人簽署的證書，而不是受信任的證書頒發機構。 自簽名憑證可啟用與CA簽署之憑證相同的加密等級，但有兩個主要缺點：<ul><li>訪客的連線可能遭到劫持，攻擊者可檢視所傳送的所有資料（因此無法加密連線）</li><li> 無法像受信任證書那樣撤銷證書。</li></ul> |
| SSL（安全通訊端層） | 在Web伺服器和瀏覽器之間建立加密連結的標準安全技術。 |
| 萬用字元憑證 | 萬用字元憑證可保護單一網域名稱（例如*.adobe.com）上不限數量的一級子網域。 |

## 主要步驟

1. 索取憑證簽署要求(CSR)檔案，並提供必要資訊（國家、州、城市、組織名稱、組織單位名稱等） Adobe。
1. 驗證由Adobe生成的CSR檔案，並驗證您提供的所有資訊是否正確。
1. 使用CSR詳細資訊產生由受信任的認證機構<!--taking care of asking for using the subjectAltName SSL extension (SAN) if it is for several domain names, and get/purchase the resulting certificate (ideally) in PEM format for Apache server-->簽署的憑證。
1. 驗證SSL憑證並驗證其是否符合CSR。
1. 將SSL憑證提供給Adobe，由誰安裝。
1. 測試每個安全子網域的SSL憑證是否已成功安裝。
1. 監控SSL憑證的有效期。
1. 更新Adobe Campaign的任何特定配置。

## 詳細程式

### 先決條件

您必須識別網域名稱和函式（追蹤、鏡像頁面、網頁應用程式等） 安全。
>[!NOTE]
>
>Adobe有助於定義要涉及的域名和函式。 如需詳細資訊，請連絡您的Adobe客戶成功經理。

### 步驟1 —— 取得CSR檔案

若要取得CSR（認證簽署要求）檔案，請遵循下列步驟。

* 如果您可以存取[控制面板](https://experienceleague.adobe.com/docs/control-panel/using/control-panel-home.html)，請依照本頁[的指示，從控制面板產生並下載CSR檔案。](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/renewing-subdomain-certificate.html#subdomains-and-certificates)

* 否則，請透過https://adminconsole.adobe.com/建立支援票證，以從Adobe客戶服務取得必要子網域的CSR檔案。

以下是一些最佳實務：

* 每個委派子網域提出一個請求。
* 您可以將多個子網域合併為單一CSR要求，但僅限於同一個環境。 例如，在Campaign Classic中，行銷伺服器、中間採購伺服器](https://experienceleague.adobe.com/docs/campaign-classic/using/installing-campaign-classic/additional-configurations/mid-sourcing-server.html)和[執行例項](https://experienceleague.adobe.com/docs/campaign-classic/using/transactional-messaging/instance-configuration/creating-a-shared-connection.html)是三個不同的環境。[
* 您必須在續約任何SSL憑證之前取得新的CSR。 請勿使用一年前或以上的舊CSR檔案。

您需要提供下列資訊。

>[!CAUTION]
>
>必須填入下表中指示的所有欄位。 否則，無法處理CSR請求。

**向Adobe小組提供的資訊：**

| 提供的資訊 | 範例值 | 注意 |
|--- |--- |--- |
| 用戶端名稱 | My Company Inc. | 組織名稱。 此欄位由Adobe用來追蹤您的請求（它不是CSR/SSL憑證的一部分）。 |
| Adobe Campaign環境網址 | https://client-mid-prod1.campaign.adobe.com | Adobe Campaign實例URL。 |
| 公用名稱[CN] | t.subdomain.customer.com | 這可以是任何相關網域，但通常是追蹤網域。 |
| 主題替代名稱[SAN] | t.subdomain.customer.com | 確保將跟蹤子域作為SAN包括在內。 |
| 主題替代名稱[SAN] | m.subdomain.customer.com |
| 主題替代名稱[SAN] | res.subdomain.customer.com |

**您的IT/SSL內部團隊提供的資訊：**

| 提供的資訊 | 範例值 | 注意 |
|--- |--- |--- |
| 國家[C] | 美國 | 這必須是雙字母代碼。 存取完整的國家／地區清單[這裡](https://www.ssl.com/csrs/country_codes/)。</br>*注意：對於英國，請使用GB（而非英國）。* |
| 州（或省名）[ST] | 伊利諾州 | 如果適用。 值必須是完整名稱，而非縮寫。 |
| 城市／地區名稱[L] | 芝加哥 |
| 組織名稱[O] | ACME |
| 組織單位名稱[OU] | IT |

>[!NOTE]
>
>將&quot;subdomain.customer.com&quot;取代為您的委派子網域，並將其他範例值取代為適當的值。

### 步驟2 —— 驗證CSR檔案

在提交您的要求並附上相關資訊後，Adobe會產生並提供您憑證簽署要求(CSR)檔案。

產生的CSR檔案中的文字必須以&#x200B;**&quot; - BEGIN CERTIFICATE REQUEST—&quot;**&#x200B;開頭。

從Adobe收到CSR檔案後，請遵循以下步驟：

1. 將CSR檔案文字複製並貼至線上解碼器，例如https://www.sslshopper.com/csr-decoder.html、<!--https://www.certlogik.com/decoder/,-->或https://www.entrust.net/ssl-technical/csr-viewer.cfm。
或者，您也可以在Linux機器上本機使用*OpenSSL*&#x200B;命令。 有關詳細資訊，請參閱[此外部頁](https://www.question-defense.com/2009/09/22/use-openssl-to-verify-the-contents-of-a-csr-before-submitting-for-a-ssl-certificate)。
1. 驗證所有檢查是否成功。
1. 檢查是否包含正確的參數和域名。
1. 檢查所有其他資料是否符合您在提交請求時提供的詳細資訊。

### 步驟3 —— 產生SSL憑證

提供CSR檔案後，您必須使用CSR檔案為適當的網域購買並產生SSL憑證。

* SSL憑證：
   * 必須為Apache PEM格式；
   * 不應超過2048位；
   * 必須由有效的CA（認證授權機構）簽署；
   * 必須包括CSR檔案中提及的所有SAN（主題替代名稱）。
* 如果有一個或多個中間證書，則必須提供根證書和所有中間證書以Adobe。
* 您可以設定任何憑證有效期，但Adobe建議選擇足夠長的憑證（例如兩年）。

>[!NOTE]
>
>如果您使用您自己的內部工具或CA提供的入口網站來要求憑證，請務必使用與CSR要求中提供的相同詳細資訊，以避免憑證產生程式出現任何延遲或不一致。

### 步驟4 —— 驗證SSL憑證

SSL憑證產生後，您必須先驗證它，才能將它傳送至Adobe。 要執行此操作，請遵循下列步驟：

1. 請確定憑證具有。pem副檔名。 如果不是這樣，請將其轉換為PEM格式。 您可以使用&#x200B;*OpenSSL*&#x200B;進行轉換。
1. 確認證書以&#x200B;**&quot; -BEGIN CERTIFICATE—&quot;**&#x200B;開頭。
1. 將憑證文字複製至線上解碼器，例如https://www.sslshopper.com/certificate-decoder.html或https://www.entrust.net/ssl-technical/csr-viewer.cfm。
或者，您也可以在Linux機器上本機使用*OpenSSL*&#x200B;命令。 有關詳細資訊，請參閱[此外部頁](https://www.shellhacks.com/decode-ssl-certificate/)。
1. 請確定憑證能正確解析，包括通用名稱、SAN、發行者和有效期。
1. 如果SSL憑證驗證成功，請使用[此網站](https://www.sslshopper.com/certificate-key-matcher.html)檢查憑證是否符合CSR:選擇&#x200B;**檢查CSR和憑證是否符合**，然後在對應欄位中輸入憑證和CSR。 它們應該相符。

### 步驟5 —— 要求安裝SSL憑證

* 如果您有權訪問[控制面板](https://experienceleague.adobe.com/docs/control-panel/using/control-panel-home.html)，請按照[本頁](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/renewing-subdomain-certificate.html#installing-ssl-certificate)中的說明將證書上傳到控制面板。

* 否則，請通過https://adminconsole.adobe.com/建立另一個支援票證，以請求Adobe在Adobe伺服器上安裝證書。

您需要提供：

* 證書檔案、根證書和任何中間證書（附加到票證上），優選為Apache PEM格式。
* 為CSR提出的前一個支援票證的數量。
* 為CSR票證提供的相同資料（包括公用名稱、例項URL、州、城市／地點、組織名稱、組織單位名稱等）。

### 步驟6 —— 測試SSL憑證安裝

在安裝SSL憑證並經Adobe客戶服務確認後，請確定所有URL皆已成功安裝。

在關閉SSL安裝票證之前，請執行以下測試。 另請務必依照[本節](#update-configuration)的說明更新任何特定組態。

導覽至瀏覽器中的下列URL（以子網域取代&quot;subdomain.customer.com&quot;）:

* https://subdomain.customer.com/r/test（僅適用於[網頁應用程式](https://experienceleague.adobe.com/docs/campaign-classic/using/designing-content/web-applications/about-web-applications.html)子網域——不適用於電子郵件子網域）
* https://t.subdomain.customer.com/r/test
* https://m.subdomain.customer.com/r/test
* https://res.subdomain.customer.com/r/test

成功的結果會提供環境資訊，而URL中的位址列則表示連線安全。 例如，您可在Google Chrome中看到下列訊息：

![](../../help/assets/ssl-google-successful-result.png)

如果SSL憑證未正確安裝，則會顯示下列警告：

![](../../help/assets/ssl-google-unsuccessful-result.png)

### 步驟7 —— 檢查證書有效期

您可以在瀏覽器中檢查憑證的有效期。 例如，在Google Chrome中，按一下「**Secure** > **Certificate**」。

您有責任檢查有效期。 Adobe建議您實作程式以監控憑證過期。 進一步瞭解當您的SSL憑證在[本文](https://www.thesslstore.com/blog/what-happens-when-your-ssl-certificate-expires/)中過期時會發生什麼情況。

* 建立支援票證，以在憑證到期日前至少兩週請求更新的憑證。 除非CSR詳細資訊已變更，否則您不需要請求額外的CSR。

* 如果您可以訪問[控制面板](https://experienceleague.adobe.com/docs/control-panel/using/control-panel-home.html)，並且您的環境由AWS環境中的Adobe托管，則可以使用控制面板在證書過期之前續訂該證書。 進一步瞭解[本節](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/monitoring-ssl-certificates.html#monitoring-certificates)。

### 步驟8 —— 更新任何特定配置{#update-configuration}

在您確信所請求的SSL憑證已正確安裝後，您就可以將Adobe Campaign的所有參考從HTTP更新為HTTPS。

>[!NOTE]
>
>對於Campaign Classic，要更新的URL主要位於[部署嚮導](https://experienceleague.adobe.com/docs/campaign-classic/using/installing-campaign-classic/initial-configuration/deploying-an-instance.html#deployment-wizard)和[外部帳戶](https://experienceleague.adobe.com/docs/campaign-classic/using/installing-campaign-classic/additional-configurations/external-accounts.html#installing-campaign-classic)（跟蹤、鏡像頁和公共資源域）中。 有關Campaign Standard，請參閱[品牌配置](https://experienceleague.adobe.com/docs/campaign-standard/using/administrating/application-settings/branding.html#about-brand-identity)。

一旦更新設定，就會以HTTPS URL而非HTTP傳送新電子郵件。 若要檢查URL現在是否安全，您可以快速執行下列測試：

* 從Adobe Campaign上傳影像。 上傳影像後，傳回的URL應為HTTPS。
* 建立測試電子郵件傳送，包括鏡像頁面連結、部分影像、文字和取消訂閱連結。 將電子郵件傳出至外部電子郵件ID（例如您的Gmail位址）。 收到電子郵件後，請開啟電子郵件，並確保電子郵件內的所有連結都以其HTTPS格式（非HTTP）正確開啟，而無任何SSL憑證警告或錯誤。

## 產品特定資源

**Campaign Classic**

* [控制面板：新增SSL憑證（教學課程）](https://experienceleague.adobe.com/docs/campaign-classic-learn/control-panel/subdomains-and-certificates/adding-ssl-certificates.html) -瞭解如何新增SSL憑證以保護您的子網域。

**Campaign Standard**

* [控制面板：新增SSL憑證（教學課程）](https://experienceleague.adobe.com/docs/campaign-standard-learn/control-panel/subdomains-and-certificates/adding-ssl-certificates.html) -瞭解如何新增SSL憑證以保護您的子網域。