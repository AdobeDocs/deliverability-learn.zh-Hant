---
title: Campaign Classic - 技術建議
description: 瞭解可用於提高Adobe Campaign Classic交付率的技術、配置和工具。
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: 39ed3773-18bf-4653-93b6-ffc64546406b
source-git-commit: d6094cd2ef0a8a7741e7d8aa4db15499fad08f90
workflow-type: tm+mt
source-wordcount: '1575'
ht-degree: 1%

---

# Campaign Classic - 技術建議 {#technical-recommendations}

下面列出了在使用Adobe Campaign Classic時可用於提高交付率的幾種技術、配置和工具。

## 設定 {#configuration}

### 反向DNS {#reverse-dns}

Adobe Campaign檢查是否為IP地址提供了反向DNS，並且這正確指向IP。

網路配置中的一個重要點是確保為每個傳出消息的IP地址定義了正確的反向DNS。 這意味著對於給定的IP地址，存在一個反向DNS記錄（PTR記錄），其中匹配的DNS（A記錄）循環回初始IP地址。

反向DNS的域選擇在處理某些ISP時會產生影響。 特別是，AOL只接受與反向DNS在同一域中具有地址的反饋循環(請參見 [反饋循環](#feedback-loop))。

>[!NOTE]
>
>您可以使用 [這個外部工具](https://mxtoolbox.com/SuperTool.aspx) 驗證域的配置。

### MX規則 {#mx-rules}

MX規則（郵件eXchanger）是管理發送伺服器與接收伺服器之間通信的規則。

更準確地說，它們用於控制Adobe CampaignMTA（郵件傳輸代理）向每個電子郵件域或ISP（例如，hotmail.com、comcast.net）發送電子郵件的速度。 這些規則通常基於ISP發佈的限制（例如，每個SMTP連接不包含超過20封郵件）。

>[!NOTE]
>
>有關Adobe Campaign ClassicMX管理的詳細資訊，請參閱 [此部分](https://experienceleague.adobe.com/docs/campaign-classic/using/installing-campaign-classic/additional-configurations/email-deliverability.html#mx-configuration)。

### TLS {#tls}

TLS（傳輸層安全性）是一種加密協定，可用於保護兩個電子郵件伺服器之間的連接，並保護電子郵件內容不被目標收件人以外的任何人讀取。

### 發件人的域 {#sender-domain}

要定義用於HELO命令的域，請編輯實例的配置檔案(conf/config-instance.xml)，並按如下方式定義「localDomain」屬性：

```
<serverConf>
  <shared>
    <dnsConfig localDomain="mydomain.net"/>
  </shared>
</serverConf>
```

MAIL FROM域是用於技術退回消息的域。 此地址在部署嚮導中或通過NmsEmail_DefaultErrorAddr選項定義。

### SPF記錄 {#dns-configuration}

SPF記錄當前可以在DNS伺服器上定義為TXT類型記錄（代碼16）或SPF類型記錄（代碼99）。 SPF記錄採用字串形式。 例如：

```
v=spf1 ip4:12.34.56.78/32 ip4:12.34.56.79/32 ~all
```

將兩個IP地址(12.34.56.78和12.34.56.79)定義為有權為域發送電子郵件。 **全部** 表示應將任何其他地址解釋為SoftFail。

Recommendations，用於定義SPF記錄：

* 添加 **全部** （軟失敗）或 **— 全部** （失敗）最終拒絕除已定義伺服器之外的所有伺服器。 如果沒有這一點，伺服器將能夠偽造此域（使用中性評估）。
* 不添加 **指針** （openspf.org建議不要這樣做，因為成本高，不可靠）。

>[!NOTE]
>
>瞭解有關SPF的詳細資訊，請參閱 [此部分](/help/additional-resources/authentication.md#spf)。

## 驗證

>[!NOTE]
>
>瞭解有關中不同形式的電子郵件身份驗證的更多資訊 [此部分](/help/additional-resources/authentication.md)。

### DKIM {#dkim-acc}

>[!NOTE]
>
>對於托管或混合安裝，如果已升級到 [增強的MTA](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/sending-emails/sending-an-email/sending-with-enhanced-mta.html#sending-messages), DKIM電子郵件身份驗證簽名由增強MTA對所有域中的所有郵件執行。

使用 [DKIM](/help/additional-resources/authentication.md#dkim) 與Adobe Campaign Classic合作需要以下先決條件：

**Adobe Campaign期權聲明**:在Adobe Campaign,DKIM私鑰基於DKIM選擇器和域。 當前無法為具有不同選擇器的同一域/子域建立多個私鑰。 無法定義在平台或電子郵件中驗證必須使用哪個選擇器域/子域。 平台可選擇選擇一個私鑰，這意味著驗證失敗的可能性很大。

* 如果已為Adobe Campaign實例配置了DomainKeys，則只需選擇 **數** 的 [域管理規則](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-delivery-failures.html#email-management-rules)。 否則，請執行與DomainKeys（已替換DKIM）相同的配置步驟（專用/公鑰）。
* 無需為DKIM是DomainKeys的改進版本，而為同一域同時啟用DomainKeys和DKIM。
* 以下域當前驗證DKIM:AOL,Gmail。

## 反饋循環 {#feedback-loop-acc}

反饋循環通過在ISP級別為用於發送消息的IP地址範圍聲明給定的電子郵件地址來工作。 ISP將以類似方式向此郵箱發送郵件，即接收者報告為垃圾郵件的郵件。 該平台應配置為阻止將來向已投訴的用戶發送。 即使他們沒有使用適當的「退出」連結，也必須不再與他們聯繫。 根據這些抱怨， ISP將在其密文清單中添加IP地址。 根據ISP的不同，投訴率約為1%將導致IP地址被阻塞。

目前正在制定一個標準來定義反饋循環消息的格式：這樣 [濫用反饋報告格式(ARF)](https://tools.ietf.org/html/rfc6650)。

為實例實現反饋循環需要：

* 專用於實例的郵箱，可能是彈出郵箱
* 專用於實例的IP發送地址

在Adobe Campaign實施簡單的反饋循環使用彈回消息功能。 反饋循環郵箱用作反彈郵箱，並定義規則來檢測這些消息。 將將報告郵件為垃圾郵件的收件人的電子郵件地址添加到隔離清單中。

* 建立或修改退信郵件規則， **反饋環**, **[!UICONTROL Administration > Campaign Management > Non deliverables Management > Mail rule sets]** 有理由 **拒絕** 類型 **硬**。
* 如果已專門為反饋循環定義了郵箱，請定義參數以通過在中建立新的外部Bounce Mails帳戶來訪問它 **[!UICONTROL Administration > Platform > External accounts]**。

該機制立即開始運作，以處理投訴通知。 為確保此規則正常工作，您可以暫時停用帳戶，以便他們不收集這些郵件，然後手動檢查反饋循環郵箱的內容。 在伺服器上，執行以下命令：

```
nlserver stop inMail@instance,
nlserver inMail -instance:instance -verbose.
```

如果您被迫對多個實例使用一個反饋循環地址，則必須：

* 複製在實例數量上接收的郵件，
* 讓每個郵箱由一個實例拾取，
* 配置實例，以便它們只處理與它們相關的消息：實例資訊包含在Adobe Campaign發送的消息的消息ID報頭中，因此也位於反饋循環消息中。 只需指定 **checkInstanceName** 實例配置檔案中的參數（預設情況下，實例不會被驗證，這可能導致某些地址被錯誤隔離）:

   ```
   <serverConf>
     <inMail checkInstanceName="true"/>
   </serverConf>
   ```

Adobe Campaign的可交付性服務管理您對以下ISP的反饋循環服務的訂購：AOL、BlueTie、Comcast、Cox、EarthLink、FastMail、Gmail、Hotmail、HostedEmail、Libero、Mail.ru、MailTrust、OpenSRS、QQ、RoadRunner、Synacor、Terra、UniteUe、USA、USAnine、USA、USA、X、XX、USAX、XX、X、S全部，雅虎，Yandex,Zoho。

## 清單 — 取消訂閱 {#list-unsubscribe}

### 關於List-Unsubscribe {#about-list-unsubscribe}

添加名為的SMTP頭 **清單 — 取消訂閱** 是確保最佳可交付性管理的必需項。

此報頭可用作「報告為SPAM」表徵圖的替代項。 它將在電子郵件介面中顯示為未訂閱連結。

使用此功能有助於保護您的信譽，並且反饋將作為未訂閱執行。

要使用List-Unsubscribe，必須輸入類似於以下命令行：

```
List-Unsubscribe: mailto: client@newsletter.example.com?subject=unsubscribe?body=unsubscribe
```

>[!CAUTION]
>
>上例基於收件人表。 如果資料庫實現是從另一個表完成的，請確保用正確的資訊重述命令行。

以下命令行可用於建立動態 **清單 — 取消訂閱**:

```
List-Unsubscribe: mailto: %=errorAddress%?subject=unsubscribe%=message.mimeMessageId%
```

Gmail、Outlook.com和MicrosoftOutlook支援此方法，並且其介面中直接提供取消訂閱按鈕。 這種技術降低了投訴率。

您可以實施 **清單 — 取消訂閱** 按以下任一：

* 直接 [在傳遞模板中添加命令行](#adding-a-command-line-in-a-delivery-template)
* [建立類型規則](#creating-a-typology-rule)

### 在傳遞模板中添加命令行 {#adding-a-command-line-in-a-delivery-template}

必須在電子郵件的SMTP頭的其他部分中添加命令行。

可以在每封電子郵件或現有的傳遞模板中完成此添加。 您還可以建立包含此功能的新交貨模板。

### 建立類型規則 {#creating-a-typology-rule}

規則必須包含生成命令行的指令碼，並且必須包含在電子郵件標題中。

>[!NOTE]
>
>我們建議建立一個類型規則：List-Unsubscribe功能將自動添加到每封電子郵件中。

1. List-Unsubscribe（取消訂閱清單）: &lt;mailto:unsubscribe domain.com=&quot;&quot;>

   按一下 **取消訂閱** 連結開啟用戶的預設電子郵件客戶端。 此類型規則必須添加到用於建立電子郵件的類型中。

1. List-Unsubscribe（取消訂閱清單）: `<https://domain.com/unsubscribe.jsp>`

   按一下 **取消訂閱** 連結將用戶重定向到您的未訂閱表單。

   範例：

   ![](../assets/s_tn_del_unsubscribe_param.png)

>[!NOTE]
>
>瞭解如何在Adobe Campaign Classic建立分類規則 [此部分](https://experienceleague.adobe.com/docs/campaign-classic/using/orchestrating-campaigns/campaign-optimization/about-campaign-typologies.html#typology-rules)。

## 電子郵件優化 {#email-optimization}

### SMTP {#smtp}

SMTP（簡單郵件傳輸協定）是電子郵件傳輸的Internet標準。

未由規則檢查的SMTP錯誤列在 **[!UICONTROL Administration]** > **[!UICONTROL Campaign Management]** > **[!UICONTROL Non deliverables Management]** > **[!UICONTROL Delivery log qualification]** 的子菜單。 預設情況下，這些錯誤消息被解釋為無法訪問的軟錯誤。

必須標識最常見的錯誤並在中添加相應規則 **[!UICONTROL Administration]** > **[!UICONTROL Campaign Management]** > **[!UICONTROL Non deliverables Management]** > **[!UICONTROL Mail rule sets]** 如果要正確限定來自SMTP伺服器的反饋。 否則，平台將執行不必要的重試（未知用戶的情況），或在給定數量的test後錯誤地將某些收件人置於隔離狀態。

### 專用IP {#dedicated-ips}

Adobe為每個客戶提供專用的IP策略，並增加IP，以建立信譽並優化交付效能。
