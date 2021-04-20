---
title: Campaign Classic-技術建議
description: 探索可用來提升Adobe Campaign Classic傳遞率的技術、組態和工具。
feature: Putting it in practice
topics: Deliverability
kt: null
thumbnail: null
doc-type: article
activity: understand
team: ACS
translation-type: tm+mt
source-git-commit: 1e539b5df54250a5927701009e7a9c84e5d73fae
workflow-type: tm+mt
source-wordcount: '1579'
ht-degree: 1%

---


# Campaign Classic-技術建議{#technical-recommendations}

以下列出您在使用Adobe Campaign Classic時可用來改善傳遞率的多種技術、組態和工具。

## 設定 {#configuration}

### 反向DNS {#reverse-dns}

Adobe Campaign會檢查IP位址是否提供反向DNS，且這會正確指向IP。

網路配置中的一個重要點是確保為每個傳出消息的IP地址定義了正確的反向DNS。 這表示對於給定的IP地址，存在具有匹配DNS（A記錄）的反向DNS記錄（PTR記錄），該DNS（記錄）返回初始IP地址。

反向DNS的網域選擇在處理某些ISP時會產生影響。 特別是，AOL只接受與反向DNS位於相同域的地址的反饋環（請參見[反饋環](#feedback-loop)）。

>[!NOTE]
>
>您可以使用[此外部工具](https://mxtoolbox.com/SuperTool.aspx)來驗證域的配置。

### MX規則{#mx-rules}

MX規則(Mail eXchanger)是管理傳送伺服器與接收伺服器間通訊的規則。

更精確地說，它們用於控制Adobe CampaignMTA（消息傳輸代理）向每個單獨的電子郵件域或ISP（例如hotmail.com、comcast.net）發送電子郵件的速度。 這些規則通常基於ISP發佈的限制（例如，每個SMTP連接不包含超過20條消息）。

>[!NOTE]
>
>有關Adobe Campaign ClassicMX管理的更多資訊，請參閱[本節](https://experienceleague.adobe.com/docs/campaign-classic/using/installing-campaign-classic/additional-configurations/email-deliverability.html#mx-configuration)。

### TLS {#tls}

TLS（傳輸層安全性）是一種加密協定，可用於保護兩個電子郵件伺服器之間的連接，並保護電子郵件內容不受預定收件人以外的任何人讀取。

### 發件人的域{#sender-domain}

要定義用於HELO命令的域，請編輯實例的配置檔案(conf/config-instance.xml)，並定義「localDomain」屬性，如下所示：

```
<serverConf>
  <shared>
    <dnsConfig localDomain="mydomain.net"/>
  </shared>
</serverConf>
```

MAIL FROM域是用於技術彈回消息的域。 此地址在部署嚮導中定義，或通過NmsEmail_DefaultErrorAddr選項定義。

### SPF記錄{#dns-configuration}

SPF記錄當前可以在DNS伺服器上定義為TXT類型記錄（代碼16）或SPF類型記錄（代碼99）。 SPF記錄採用字串形式。 例如：

```
v=spf1 ip4:12.34.56.78/32 ip4:12.34.56.79/32 ~all
```

將兩個IP位址（12.34.56.78和12.34.56.79）定義為授權傳送網域的電子郵件。 **~** all means that any other address should be increpted as a SoftFail.

Recommendations:

* 最後添加&#x200B;**~all**(SoftFail)或&#x200B;**-all**(Fail)以拒絕除已定義伺服器之外的所有伺服器。 若沒有此項，伺服器將能夠偽造此網域（進行中性評估）。
* 不要添加&#x200B;**ptr**（openspf.org建議使用此方法，因為成本高昂且不可靠）。

>[!NOTE]
>
>在[本節](/help/additional-resources/authentication.md#spf)中瞭解有關SPF的更多資訊。

## 驗證

>[!NOTE]
>
>瞭解[本節](/help/additional-resources/authentication.md)中電子郵件驗證的不同形式。

### DKIM {#dkim-acc}

>[!NOTE]
>
>對於托管或混合安裝，如果您已升級至[增強型MTA](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/sending-emails/sending-an-email/sending-with-enhanced-mta.html#sending-messages)，則DKIM電子郵件驗證簽署由增強型MTA針對所有網域的所有訊息完成。

搭配使用[DKIM](/help/additional-resources/authentication.md#dkim)和Adobe Campaign Classic需要以下先決條件：

**Adobe Campaign期權聲明**:在Adobe Campaign,DKIM私密金鑰是以DKIM選擇器和網域為基礎。目前無法使用不同的選擇器為相同的網域／子網域建立多個私密金鑰。 無法定義平台或電子郵件中驗證必須使用哪個選擇器網域／子網域。 平台可選擇選擇其中一個私鑰，這表示驗證有很高的失敗機率。

* 如果您已為Adobe Campaign實例配置了DomainKeys，則只需在[域管理規則](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-delivery-failures.html#email-management-rules)中選擇&#x200B;**dkim**。 否則，請遵循與DomainKeys（已取代DKIM）相同的設定步驟（私用／公開金鑰）。
* DKIM是DomainKeys的改進版本，因此無需為相同的域同時啟用DomainKeys和DKIM。
* 下列網域目前驗證DKIM:AOL,Gmail。

## 反饋環{#feedback-loop-acc}

回饋迴路可在ISP層級為用於傳送訊息的IP位址範圍宣告指定電子郵件地址。 ISP會以類似方式將郵件發送到此郵箱，即接收者報告為垃圾郵件的郵件。 應將平台設定為封鎖將來傳送給已投訴的使用者。 即使他們未使用適當的退出連結，也必須不再與他們聯絡。 基於這些抱怨， ISP將在其密鑰清單中添加IP地址。 根據ISP的不同，投訴率約為1%將導致IP地址被阻塞。

目前正在制定一個標準，以定義反饋迴路消息的格式：[濫用反饋報告格式(ARF)](https://tools.ietf.org/html/rfc6650)。

實作例項的回饋回圈需要：

* 專用於實例的郵箱，可能是彈回郵箱
* 專用於實例的IP發送地址

在Adobe Campaign實作簡單的回饋迴路，使用彈回訊息功能。 反饋循環郵箱用作彈回郵箱，並定義規則來檢測這些消息。 將郵件作為垃圾郵件報告的收件人的電子郵件地址添加到隔離清單中。

* 在&#x200B;**[!UICONTROL Administration > Campaign Management > Non deliverables Management > Mail rule sets]**&#x200B;中建立或修改彈回郵件規則&#x200B;**Feedback_loop**，其原因為&#x200B;**Recommed**&#x200B;和類型&#x200B;**Hard**。
* 如果郵箱已專門為反饋循環定義，請通過在&#x200B;**[!UICONTROL Administration > Platform > External accounts]**&#x200B;中建立新的外部「彈回郵件」帳戶來定義要訪問該郵箱的參數。

該機制立即開始運作，以處理投訴通知。 為確保此規則正常運作，您可以暫時停用帳戶，以免帳戶收集這些訊息，然後手動檢查回饋回圈信箱的內容。 在伺服器上，執行以下命令：

```
nlserver stop inMail@instance,
nlserver inMail -instance:instance -verbose.
```

如果您被迫針對多個例項使用單一回饋迴路位址，您必須：

* 複製在任意數量的郵箱上收到的消息，
* 讓每個郵箱都通過一個實例進行選擇，
* 設定例項，使其只處理與其相關的訊息：實例資訊包含在Adobe Campaign發送的消息的消息ID標題中，因此也位於反饋迴路消息中。 只需在實例配置檔案中指定&#x200B;**checkInstanceName**&#x200B;參數即可（預設情況下，實例未驗證，這可能導致某些地址被錯誤隔離）:

   ```
   <serverConf>
     <inMail checkInstanceName="true"/>
   </serverConf>
   ```

Adobe Campaign的交付能力服務管理您對以下ISP的反饋迴路服務的訂閱：AOL、BlueTie、Comcast、Cox、EarthLink、FastMail、Gmail、Hotmail、HostedEmail、Libero、Mail.ru、MailTrust、OpenSRS、QQ、RoadRunner、Synacor、Telenor、Terra、UniteOnline、USEine、USA4、USA、USINE、USA、X、XALL, Yahoo, Yandex, Zoho。

## List-Unsubscribe {#list-unsubscribe}

### 關於List-Unsubscribe {#about-list-unsubscribe}

必須添加名為&#x200B;**List-Unsubscribe**&#x200B;的SMTP標頭，以確保最佳的傳遞能力管理。

此標題可用作「以垃圾訊息形式報告」圖示的替代項目。 它會在電子郵件介面中顯示為取消訂閱連結。

使用此功能有助於保護您的聲譽，而且會以取消訂閱的方式執行意見回應。

要使用「清單取消訂閱」，必須輸入如下所示的命令行：

```
List-Unsubscribe: mailto: client@newsletter.example.com?subject=unsubscribe?body=unsubscribe
```

>[!CAUTION]
>
>上述範例以收件者表格為基礎。 如果從另一個表執行資料庫實施，請確保用正確的資訊重述命令行。

以下命令行可用於建立動態&#x200B;**List-Unsubscribe**:

```
List-Unsubscribe: mailto: %=errorAddress%?subject=unsubscribe%=message.mimeMessageId%
```

Gmail、Outlook.com和Microsoft Outlook都支援此方法，而且其介面中會直接提供取消訂閱按鈕。 這種技巧降低投訴率。

您可以通過以下任一方法實施&#x200B;**List-Unsubscribe**:

* 直接[在傳送範本中新增命令列](#adding-a-command-line-in-a-delivery-template)
* [建立類型規則](#creating-a-typology-rule)

### 在傳送範本{#adding-a-command-line-in-a-delivery-template}中新增命令列

命令行必須添加到電子郵件SMTP標題的附加部分中。

您可以在每封電子郵件或現有的傳送範本中新增。 您也可以建立包含此功能的新傳送範本。

### 建立類型規則 {#creating-a-typology-rule}

規則必須包含產生命令行的指令碼，而且必須包含在電子郵件標題中。

>[!NOTE]
>
>我們建議建立類型規則：清單取消訂閱功能會自動新增至每封電子郵件。

1. 清單取消訂閱：&lt;mailto:unsubscribe@domain.com>

   按一下&#x200B;**取消訂閱**&#x200B;連結，會開啟使用者的預設電子郵件用戶端。 此類型學規則必須新增至用於建立電子郵件的類型學中。

1. 清單取消訂閱：`<https://domain.com/unsubscribe.jsp>`

   按一下&#x200B;**unsubscribe**&#x200B;連結會將使用者重新導向至您的取消訂閱表單。

   範例：

   ![](../assets/s_tn_del_unsubscribe_param.png)

>[!NOTE]
>
>瞭解如何在[本節](https://experienceleague.adobe.com/docs/campaign-classic/using/orchestrating-campaigns/campaign-optimization/about-campaign-typologies.html#typology-rules)中建立Adobe Campaign Classic的排版規則。

## 電子郵件最佳化{#email-optimization}

### SMTP {#smtp}

SMTP（簡單郵件傳輸協定）是電子郵件傳輸的Internet標準。

規則未檢查的SMTP錯誤列在&#x200B;**[!UICONTROL Administration]** > **[!UICONTROL Campaign Management]** > **[!UICONTROL Non deliverables Management]** > **[!UICONTROL Delivery log qualification]**&#x200B;資料夾中。 預設情況下，這些錯誤消息被解釋為無法訪問的軟錯誤。

如果要正確限定來自SMTP伺服器的反饋，必須識別最常見的錯誤，並在&#x200B;**[!UICONTROL Administration]** > **[!UICONTROL Campaign Management]** > **[!UICONTROL Non deliverables Management]** > **[!UICONTROL Mail rule sets]**&#x200B;中添加相應規則。 若未執行此項作業，平台將執行不必要的重試（未知使用者的情況），或在指定數量的測試後，將某些收件者錯誤地置於隔離中。

### 專用IP {#dedicated-ips}

Adobe為每個客戶提供專屬的IP策略，以提升IP，以建立聲譽並最佳化傳送效能。
