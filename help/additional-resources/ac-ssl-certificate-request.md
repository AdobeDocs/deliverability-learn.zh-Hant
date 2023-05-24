---
title: SSL證書請求進程
description: 瞭解如何在您委託給Adobe的子域上安裝SSL證書。
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: 8a78abd3-afba-49a7-a2ae-8b2c75326749
source-git-commit: 57016f89df54d5c74755a6a108a92db45153ec18
workflow-type: tm+mt
source-wordcount: '2252'
ht-degree: 2%

---

# SSL 憑證請求流程

將域委託給Adobe以發送電子郵件(請參閱 [域名設定](/help/additional-resources/ac-domain-name-setup.md)),Adobe將建立和使用特定的子域來執行特定的功能。

例如，如果您已委託 *email.example.com* 要Adobe發送電子郵件，Adobe將建立子域，如：
* *t.email.example.com*  — 用於跟蹤連結
* *m.email.example.com*  — 鏡像頁
* *res.email.example.com*  — 用於托管資源（如影像）

建議 **通過SSL(HTTPS)保護這些域**。 事實上，無擔保連結(HTTP)易於被攔截，並會在現代瀏覽器上標出警告。

要在這些子域上安裝SSL證書，該過程包括請求CSR檔案，然後購買SSL證書以Adobe安裝或續訂。

>[!CAUTION]
>
>在安裝SSL證書之前，請確保您瞭解上列出的先決條件 [此頁](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/renew-ssl/renewing-subdomain-certificate.html?lang=zh-Hant)。
>
>Adobe最多只支援2048位證書。 尚不支援4096位證書。

## 字彙

| 詞語 | 說明 |
|--- |--- |
| CA（證書頒發機構） | SSL證書提供程式，在驗證組織或個人的身份（如DigiCert、Symantec等）後向其頒發數字證書。<ul><li>受信任的CA通常被視為發出根證書的第三方CA。</li><li>如果證書由使用證書的同一組織/公司簽名，則即使證書是SSL證書（如自簽名證書），也會將其分類為不受信任的CA。</li></ul> |
| 鏈證書 | 包括根證書和一個或多個中間證書的證書稱為鏈（或鏈）證書。 |
| CSR（證書籤名請求） | 申請SSL證書時給予證書頒發機構的編碼文本塊。 通常在安裝證書的伺服器上生成。 |
| DER（可分辨編碼規則） | 證書擴展類型。 .der擴展用於二進位DER編碼證書。 這些檔案也可能支援.cer或.crt副檔名。 |
| EV（擴展驗證）證書 | EV證書是一種新型的證書，旨在防止網路釣魚攻擊。 它需要對您的業務和訂購證書的人員進行擴展驗證。 |
| 高保證證書 | CA在驗證域名所有權和有效業務註冊後頒發高保證證書。 |
| 中間CA | 鏈式證書中包含的中間證書的證書頒發機構。 |
| 中間證書 | 證書頒發機構以樹形結構的形式發放證書。 根證書是樹中最頂層的證書。 證書和根證書之間的任何證書都稱為鏈或中間證書。 |
| 低保證證書 | 低保證證書（也稱為域驗證證書）只包括證書中的域名（而不包括業務/組織名稱）。 |
| PEM（隱私增強郵件） | 帶有.pem副檔名的證書，其中包含ASCII(Base64)資料。 此類證書以「 — — - BEGIN CERTIFICATE — - 」行開頭。 |
| 根證書 | 證書頒發機構以樹形結構的形式發放證書。 根證書是樹中最頂層的證書。 |
| SAN（主題替代名稱） | 主題替代名稱是其他主機名（站點、 IP地址、公用名稱等） 應作為單個SSL證書的一部分進行簽名。 |
| 自簽名證書 | 由建立者簽名的證書，而不是受信任的證書頒發機構。 自簽名證書可以啟用與由CA簽名的證書相同級別的加密，但存在兩個主要缺陷：<ul><li>訪問者的連接可能被劫持，使得攻擊者能夠查看發送的所有資料（從而破壞了加密連接的目的）</li><li> 無法像可信證書一樣吊銷證書。</li></ul> |
| SSL（安全套接字層） | 一種用於在Web伺服器和瀏覽器之間建立加密連結的標準安全技術。 |
| 通配符證書 | 通配符證書可以保護單個域名（如*.adobe.com）上不限數量的第一級子域。 |

## 主要步驟

1. 請求證書籤名請求(CSR)檔案並提供所需資訊（國家/地區、州/市、組織名稱、組織單位名稱等） Adobe。
1. 驗證由Adobe生成的CSR檔案，並驗證您提供的所有資訊是否正確。
1. 使用CSR詳細資訊生成由受信任的證書頒發機構簽名的證書<!--taking care of asking for using the subjectAltName SSL extension (SAN) if it is for several domain names, and get/purchase the resulting certificate (ideally) in PEM format for Apache server-->。
1. 驗證SSL證書並驗證它是否與CSR匹配。
1. 向Adobe提供SSL證書，由誰安裝。
1. Test成功為每個安全子域安裝SSL證書。
1. 監視SSL證書有效期。
1. 更新Adobe Campaign中的任何特定配置。

## 詳細流程

### 先決條件

您必須標識域名和功能（跟蹤、鏡像頁、Web應用等） 來保護。
>[!NOTE]
>
>Adobe可以幫助定義要涉及的域名和函式。 有關詳細資訊，請與您的Adobe帳戶團隊聯繫。

### 步驟1 — 獲取CSR檔案

要獲取CSR（證書籤名請求）檔案，請執行以下步驟。

* 如果您有權訪問 [控制面板](https://experienceleague.adobe.com/docs/control-panel/using/control-panel-home.html)，請按照 [此頁](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/renewing-subdomain-certificate.html#subdomains-and-certificates) 從「控制面板」生成並下載CSR檔案。

* 否則，通過https://adminconsole.adobe.com/建立支援票證，以從Adobe客戶關懷中獲取所需子域的CSR檔案。

以下是一些最佳做法：

* 每個委派子域提出一個請求。
* 可以將多個子域組合為單個CSR請求，但只能在同一環境中。 例如，在Campaign Classic中，市場營銷伺服器 [中間採購伺服器](https://experienceleague.adobe.com/docs/campaign-classic/using/installing-campaign-classic/install-campaign-on-prem/mid-sourcing-server.html)的 [執行實例](https://experienceleague.adobe.com/docs/campaign-classic/using/transactional-messaging/configure-transactional-messaging/configuring-instances.html#execution-instance) 是三個不同的環境。
* 在續訂任何SSL證書之前，必須獲得新的CSR。 不要使用一年或更久以前的舊CSR檔案。

您需要提供以下資訊。

>[!CAUTION]
>
>必須填寫下表中指示的所有欄位。 否則，無法處理CSR請求。

**向Adobe小組提供的資訊：**

| 要提供的資訊 | 示例值 | 注意 |
|--- |--- |--- |
| 用戶端名稱 | 我的公司 | 組織名稱。 此欄位由Adobe用於跟蹤您的請求（它將不是CSR/SSL證書的一部分）。 |
| Adobe Campaign環境URL | https://client-mid-prod1.campaign.adobe.com | Adobe Campaign實例URL。 |
| 公用名 [CN] | t.subdomain.customer.com | 這可以是任何相關域，但通常是跟蹤域。 |
| 主題替代名稱 [SAN] | t.subdomain.customer.com | 確保將跟蹤子域作為SAN包括在內。 |
| 主題替代名稱 [SAN] | m.subdomain.customer.com |
| 主題替代名稱 [SAN] | res.subdomain.customer.com |

**您的IT/SSL內部團隊提供的資訊：**

| 要提供的資訊 | 示例值 | 注意 |
|--- |--- |--- |
| 國家/地區 [C] | 美國 | 這必須是兩個字母的代碼。 訪問完整國家/地區清單 [這裡](https://www.ssl.com/csrs/country_codes/)。</br>*注：對於英國，使用GB（而非英國）。* |
| 省/自治區/直轄市/自治區名稱 [ST] | 伊利諾州 | 如果適用。 值必須是全名，而不是縮寫。 |
| 城市/地點名稱 [L] | 芝加哥 |
| 組織名稱 [O] | ACME |
| 組織單位名稱 [歐] | IT |

>[!NOTE]
>
>將&quot;subdomain.customer.com&quot;替換為您的委託子域，將其他示例值替換為相應的值。

### 步驟2 — 驗證CSR檔案

在提交您的請求並提供相關資訊後，Adobe將生成並為您提供證書籤名請求(CSR)檔案。

生成的CSR檔案中的文本必須以開頭 **&quot; — 開始證書請求 — &quot;**。

從Adobe接收到CSR檔案後，請執行以下步驟：

1. 將CSR檔案文本複製並貼上到聯機解碼器中，如https://www.sslshopper.com/csr-decoder.html <!--https://www.certlogik.com/decoder/,--> 或https://www.entrust.net/ssl-technical/csr-viewer.cfm。
或者，可以使用 *OpenSSL* 命令。
1. 驗證所有檢查是否都成功。
1. 檢查是否包含正確的參數和域名。
1. 檢查所有其他資料是否與您提交請求時提供的詳細資訊相匹配。

### 步驟3 — 生成SSL證書

提供CSR檔案後，您必須使用CSR檔案為相應域購買並生成SSL證書。

* SSL證書：
   * 必須為Apache PEM格式；
   * 不應超過2048位；
   * 必須由有效的CA（認證機構）簽署；
   * 必須包括CSR檔案中提到的所有SAN（主題替代名稱）。
* 如果有一個或多個中間證書，則必須提供根證書和所有中間證書以進行Adobe。
* 您可以設定任何證書有效期，但Adobe建議選擇足夠長（例如，兩年）。

>[!NOTE]
>
>如果您使用您自己的內部工具或CA提供的門戶來請求證書，請確保使用CSR請求中提供的相同詳細資訊，以避免證書生成過程中出現任何延遲或差異。

### 步驟4 — 驗證SSL證書

生成SSL證書後，必須先驗證它，然後才能將其發送到Adobe。 要執行此操作，請遵循下列步驟：

1. 確保證書具有.pem副檔名。 如果不是這樣，則將其轉換為PEM格式。 可以使用 *OpenSSL*。
1. 確認證書以 **&quot; — 開始證書 — &quot;**。
1. 將證書文本複製到聯機解碼器中，如https://www.sslshopper.com/certificate-decoder.html或https://www.entrust.net/ssl-technical/csr-viewer.cfm。
或者，可以使用 *OpenSSL* 命令。 有關此內容的詳細資訊，請參閱 [此外部頁](https://www.shellhacks.com/decode-ssl-certificate/)。
1. 確保證書正確解析，包括「公用名稱」、「SAN」、「頒發者」和「有效期」。
1. 如果SSL證書驗證成功，請使用 [此網站](https://www.sslshopper.com/certificate-key-matcher.html):選擇 **檢查CSR和證書是否匹配**，並在相應欄位中輸入證書和CSR。 它們應該匹配。

### 步驟5 — 請求安裝SSL證書

* 如果您有權訪問 [控制面板](https://experienceleague.adobe.com/docs/control-panel/using/control-panel-home.html)，請按照 [此頁](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/renew-ssl/renewing-subdomain-certificate.html?lang=zh-Hant) 將證書上載到「Control Panel（控制面板）」。

* 否則，通過https://adminconsole.adobe.com/建立另一個支援票證，以請求Adobe在Adobe伺服器上安裝證書。

您需要提供：

* 證書檔案、根證書和任何中間證書（附加到票證），最好採用Apache PEM格式。
* 為CSR上次提出的支援票證的編號。
* 為CSR票證提供的相同資料（包括公用名稱、實例URL、省/市/地區、組織名稱、組織單位名稱等）。

### 步驟6 -TestSSL證書安裝

一旦SSL證書被安裝並由Adobe客戶服務部確認，請確保它已成功安裝到所有URL。

在關閉SSL安裝票證之前，請執行以下test。 另請確保按照中的說明更新任何特定配置 [此部分](#update-configuration)。

在瀏覽器中導航到以下URL（用子域替換&quot;subdomain.customer.com&quot;）:

* https://subdomain.customer.com/r/test [Web應用](https://experienceleague.adobe.com/docs/campaign-classic/using/designing-content/web-applications/about-web-applications.html) 僅子域 — 不適用於電子郵件子域)
* https://t.subdomain.customer.com/r/test
* https://m.subdomain.customer.com/r/test
* https://res.subdomain.customer.com/r/test

成功的結果會提供環境資訊，而URL中的地址欄表示連接是安全的。 例如，您可以在GoogleChrome中看到以下消息：

![](../../help/assets/ssl-google-successful-result.png)

如果SSL證書安裝不正確，則顯示以下警告：

![](../../help/assets/ssl-google-unsuccessful-result.png)

### 步驟7 — 檢查證書有效期

您可以在瀏覽器中檢查證書的有效期。 例如，在GoogleChrome中，按一下 **安全** > **證書**。

檢查有效期是你的責任。 Adobe建議您實施一個過程來監視證書過期。 瞭解有關SSL證書在過期後發生的情況的詳細資訊 [這篇文章](https://www.thesslstore.com/blog/what-happens-when-your-ssl-certificate-expires/)。

* 建立支援票證，以在證書到期日期前至少兩週請求更新的證書。 除非CSR詳細資訊已更改，否則您不需要請求附加CSR。

* 如果您有權訪問 [控制面板](https://experienceleague.adobe.com/docs/control-panel/using/control-panel-home.html)，並且如果您的Adobe在AWS環境中托管，則可以使用「控制面板」在證書過期之前續訂該證書。 請參閱[此章節](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/monitoring-ssl-certificates.html#monitoring-certificates)深入瞭解。

### 步驟8 — 更新任何特定配置 {#update-configuration}

一旦您確信請求的SSL證書已正確安裝，您就可以將Adobe Campaign的所有引用從HTTP更新為HTTPS。

>[!NOTE]
>
>對於Campaign Classic，要更新的URL主要位於 [部署嚮導](https://experienceleague.adobe.com/docs/campaign-classic/using/installing-campaign-classic/initial-configuration/deploying-an-instance.html#deployment-wizard) 在 [外部帳戶](https://experienceleague.adobe.com/docs/campaign-classic/using/installing-campaign-classic/accessing-external-database/external-accounts.html) （跟蹤、鏡像頁和公共資源域）。 有關Campaign Standard，請參閱 [品牌配置](https://experienceleague.adobe.com/docs/campaign-standard/using/administrating/application-settings/branding.html#about-brand-identity)。

更新配置後，將使用HTTPS URL而不是HTTP發送新電子郵件。 要檢查URL現在是安全的，可以快速執行以下test:

* 從Adobe Campaign上傳影像。 上傳映像後，返回的URL應為HTTPS。
* 建立test電子郵件傳遞，包括鏡像頁面連結、某些影像、文本和取消訂閱連結。 將電子郵件發送到外部電子郵件ID（如您的Gmail地址）。 收到後，請開啟電子郵件並確保電子郵件內的所有連結以HTTPS形式（而非HTTP）正確開啟，而不出現任何SSL證書警告或錯誤。

## 產品特定資源

**Campaign Classic**

* [控制面板：添加SSL證書（教程）](https://experienceleague.adobe.com/docs/campaign-classic-learn/control-panel/subdomains-and-certificates/adding-ssl-certificates.html)  — 瞭解如何添加SSL證書以保護子域的安全。

**Campaign Standard**

* [控制面板：添加SSL證書（教程）](https://experienceleague.adobe.com/docs/campaign-standard-learn/control-panel/subdomains-and-certificates/adding-ssl-certificates.html)  — 瞭解如何添加SSL證書以保護子域的安全。
