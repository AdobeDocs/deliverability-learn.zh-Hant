---
title: Campaign Classic - 技術建議
description: 探索可用來改善Adobe Campaign Classic傳遞率的技術、設定和工具。
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

以下列出您在使用Adobe Campaign Classic時可用來改善傳遞率的數種技術、設定和工具。

## 設定 {#configuration}

### 反向DNS {#reverse-dns}

Adobe Campaign會檢查是否已為IP位址提供反向DNS，且這會正確指向回IP。

網路配置中的一個重要點是確保為每個傳出消息的IP地址定義正確的反向DNS。 這表示對於給定IP地址，存在一個反向DNS記錄（PTR記錄），該記錄具有循環到初始IP地址的匹配DNS（A記錄）。

處理特定ISP時，反向DNS的網域選擇會有影響。 尤其是，AOL只接受具有與反向DNS相同域中地址的反饋環（請參閱[反饋環](#feedback-loop)）。

>[!NOTE]
>
>您可以使用[此外部工具](https://mxtoolbox.com/SuperTool.aspx)來驗證域的配置。

### MX規則 {#mx-rules}

MX規則(Mail eXchanger)是管理發送伺服器與接收伺服器之間通信的規則。

更精確地說，它們可用來控制Adobe Campaign MTA（訊息轉移代理）傳送電子郵件至每個個別電子郵件網域或ISP（例如hotmail.com、comcast.net）的速度。 這些規則通常以ISP發佈的限制為基礎（例如，每個SMTP連線不包含超過20則郵件）。

>[!NOTE]
>
>有關Adobe Campaign Classic中MX管理的詳細資訊，請參閱[此區段](https://experienceleague.adobe.com/docs/campaign-classic/using/installing-campaign-classic/additional-configurations/email-deliverability.html#mx-configuration)。

### TLS {#tls}

TLS（傳輸層安全性）是加密通訊協定，可用來保護兩個電子郵件伺服器之間的連線，並保護電子郵件內容不受目標收件者以外的任何人讀取。

### 發件人的域 {#sender-domain}

要定義用於HELO命令的域，請編輯實例的配置檔案(conf/config-instance.xml)，並定義「localDomain」屬性，如下所示：

```
<serverConf>
  <shared>
    <dnsConfig localDomain="mydomain.net"/>
  </shared>
</serverConf>
```

MAIL FROM網域是技術退信中使用的網域。 此地址在部署嚮導中定義，或通過NmsEmail_DefaultErrorAddr選項定義。

### SPF記錄 {#dns-configuration}

SPF記錄當前可以在DNS伺服器上定義為TXT類型記錄（代碼16）或SPF類型記錄（代碼99）。 SPF記錄採用字串形式。 例如：

```
v=spf1 ip4:12.34.56.78/32 ip4:12.34.56.79/32 ~all
```

會定義兩個IP位址： 12.34.56.78和12.34.56.79，經授權可針對網域傳送電子郵件。 **~** ally表示任何其他位址應被解譯為SoftFail。

Recommendations，用於定義SPF記錄：

* 添加&#x200B;**~all**(SoftFail)或&#x200B;**-all**（失敗），在結尾處拒絕所有伺服器，而不是定義的伺服器。 若無此項，伺服器將能夠偽造此域（進行中立評估）。
* 請勿添加&#x200B;**ptr**（openspf.org建議不要添加ptr，因為這樣做成本高昂且不可靠）。

>[!NOTE]
>
>在[此小節](/help/additional-resources/authentication.md#spf)了解有關SPF的更多資訊。

## 驗證

>[!NOTE]
>
>在[本小節](/help/additional-resources/authentication.md)中深入了解不同形式的電子郵件驗證。

### DKIM {#dkim-acc}

>[!NOTE]
>
>對於托管或混合安裝，如果您已升級至[Enhanced MTA](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/sending-emails/sending-an-email/sending-with-enhanced-mta.html#sending-messages)，則DKIM電子郵件驗證簽署會由Enhanced MTA針對所有網域的所有訊息完成。

搭配Adobe Campaign Classic使用[DKIM](/help/additional-resources/authentication.md#dkim)需要下列先決條件：

**Adobe Campaign選項聲明**:在Adobe Campaign中，DKIM私密金鑰是以DKIM選取器和網域為基礎。目前無法使用不同的選取器為相同網域/子網域建立多個私密金鑰。 無法定義平台或電子郵件中都必須使用哪個選取器網域/子網域來進行驗證。 平台可選擇選取其中一個私密金鑰，這表示驗證有很高的失敗機率。

* 如果您已設定Adobe Campaign例項的DomainKeys，您只需在[網域管理規則](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-delivery-failures.html#email-management-rules)中選取&#x200B;**dkim**&#x200B;即可。 否則，請依照與DomainKeys（已取代DKIM）相同的設定步驟（私密/公開金鑰）。
* 無需為與DKIM是DomainKeys的改進版本相同的域同時啟用DomainKeys和DKIM。
* 下列網域目前驗證DKIM:AOL,Gmail。

## 反饋迴路 {#feedback-loop-acc}

在ISP層級為用於傳送訊息的IP位址範圍宣告指定的電子郵件地址，即可產生回饋回圈。 ISP將以類似於退回郵件的方式發送到此郵箱，這些郵件由收件人報告為垃圾郵件。 平台應經過設定，以封鎖未來傳送給已投訴的使用者。 即使他們未使用正確的選擇退出連結，仍請務必不再聯絡。 ISP會根據這些抱怨將IP位址新增至封鎖清單。 根據ISP，投訴率約1%會導致封鎖IP位址。

目前正在起草一項標準，以定義反饋迴路報文的格式：[濫用反饋報告格式(ARF)](https://tools.ietf.org/html/rfc6650)。

為執行個體實作回饋回圈需要：

* 專用於執行個體的信箱，可能是退信信箱
* 專用於執行個體的IP傳送地址

在Adobe Campaign中實作簡單的意見回圈會使用退信功能。 反饋循環郵箱用作退信郵箱，並定義一個規則來檢測這些郵件。 會將回報郵件為垃圾訊息之收件者的電子郵件地址新增至隔離清單。

* 在&#x200B;**[!UICONTROL Administration > Campaign Management > Non deliverables Management > Mail rule sets]**&#x200B;中建立或修改退信規則&#x200B;**Feedback_loop**，原因為&#x200B;**Recommended**&#x200B;和類型&#x200B;**Hard**。
* 如果已專門為反饋循環定義了郵箱，請在&#x200B;**[!UICONTROL Administration > Platform > External accounts]**&#x200B;中建立新的外部「退信郵件」帳戶，以定義要訪問該郵箱的參數。

該機制正在立即運作，以處理投訴通知。 若要確保此規則正常運作，您可以暫時停用帳戶，使其不收集這些訊息，然後手動檢查回饋回圈信箱的內容。 在伺服器上，執行以下命令：

```
nlserver stop inMail@instance,
nlserver inMail -instance:instance -verbose.
```

如果您被強制對多個實例使用一個反饋循環地址，則必須：

* 複製在任意數量的郵箱上收到的消息，
* 讓每個郵箱由一個實例拾取，
* 設定例項，使其只處理與其相關的訊息：例項資訊包含在Adobe Campaign傳送之訊息的Message-ID標題中，因此也位於回饋回圈訊息中。 只需在執行個體設定檔案中指定&#x200B;**checkInstanceName**&#x200B;參數即可（依預設，執行個體未經過驗證，而這可能會導致某些位址被錯誤隔離）:

   ```
   <serverConf>
     <inMail checkInstanceName="true"/>
   </serverConf>
   ```

Adobe Campaign的傳遞服務可管理您對以下ISP的反饋迴路服務訂購：AOL、BlueTie、Comcast、Cox、EarthLink、FastMail、Gmail、HostedEmail、Libero、Mail.ru、MailTrust、OpenSRS、QQ、RoadRunner、Synacor、Telenor、Terra、UniteOnline、美國、XS4ALL、Yahoo、Yaho。

## 清單 — 取消訂閱 {#list-unsubscribe}

### 關於清單取消訂閱 {#about-list-unsubscribe}

必須新增名為&#x200B;**List-Unsubscribe**&#x200B;的SMTP標頭，以確保進行最佳的傳遞能力管理。

此標題可作為「以垃圾郵件形式報告」表徵圖的替代項。 它會在電子郵件介面中顯示為取消訂閱連結。

使用此功能有助於保護您的信譽，且會以取消訂閱的方式執行意見。

要使用「清單 — 取消訂閱」，必須輸入如下所示的命令行：

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

Gmail、Outlook.com和Microsoft Outlook支援此方法，且其介面中直接提供取消訂閱按鈕。 這種方法能降低投訴率。

您可以透過下列任一方實作&#x200B;**List-Unsubscribe**:

* 直接[在傳遞範本中新增命令列](#adding-a-command-line-in-a-delivery-template)
* [建立類型規則](#creating-a-typology-rule)

### 在傳遞範本中新增命令列 {#adding-a-command-line-in-a-delivery-template}

命令列必須新增至電子郵件SMTP標題的其他區段中。

您可以在每封電子郵件或現有的傳送範本中新增內容。 您也可以建立包含此功能的新傳遞範本。

### 建立類型規則 {#creating-a-typology-rule}

規則必須包含可產生命令列的指令碼，且必須包含在電子郵件標題中。

>[!NOTE]
>
>建議您建立類型規則：每封電子郵件會自動新增清單取消訂閱功能。

1. 清單 — 取消訂閱：&lt;mailto:unsubscribe@domain.com>

   按一下&#x200B;**取消訂閱**&#x200B;連結會開啟使用者的預設電子郵件用戶端。 必須將此類型規則新增至用於建立電子郵件的類型。

1. 清單 — 取消訂閱：`<https://domain.com/unsubscribe.jsp>`

   按一下&#x200B;**取消訂閱**&#x200B;連結會將使用者重新導向至您的取消訂閱表單。

   範例：

   ![](../assets/s_tn_del_unsubscribe_param.png)

>[!NOTE]
>
>了解如何在[本區段](https://experienceleague.adobe.com/docs/campaign-classic/using/orchestrating-campaigns/campaign-optimization/about-campaign-typologies.html#typology-rules)的Adobe Campaign Classic中建立類型規則。

## 電子郵件最佳化 {#email-optimization}

### SMTP {#smtp}

SMTP（簡單郵件傳輸協定）是電子郵件傳輸的Internet標準。

未由規則檢查的SMTP錯誤列在&#x200B;**[!UICONTROL Administration]** > **[!UICONTROL Campaign Management]** > **[!UICONTROL Non deliverables Management]** > **[!UICONTROL Delivery log qualification]**&#x200B;資料夾中。 預設情況下，這些錯誤訊息會解譯為無法連線的軟錯誤。

如果要正確限定來自SMTP伺服器的反饋，則必須識別最常見的錯誤，並在&#x200B;**[!UICONTROL Administration]** > **[!UICONTROL Campaign Management]** > **[!UICONTROL Non deliverables Management]** > **[!UICONTROL Mail rule sets]**&#x200B;中添加相應的規則。 若未執行此操作，平台將執行不必要的重試（未知使用者的情況），或在指定數量的測試後將某些收件者錯誤置於隔離區。

### 專用IP {#dedicated-ips}

Adobe為每位客戶提供專屬的IP策略，並提供提升的IP，以建立信譽並最佳化傳送效能。
