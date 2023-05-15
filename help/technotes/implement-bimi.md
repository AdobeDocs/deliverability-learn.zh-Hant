---
title: 實作Gmail的品牌指標以識別訊息(BIMI)
description: 了解如何實作BIMI
topics: Deliverability
exl-id: 6b911bcc-a531-466a-8bd3-7fa469b96cc7
source-git-commit: aca2bfff9f0315b735cf0a97f2177083c58e0875
workflow-type: tm+mt
source-wordcount: '1062'
ht-degree: 0%

---

# 實作 [!DNL Brand Indicators for Message Identification] (BIMI)

[!DNL Brand Indicators for Message Identification] (BIMI)是業界標準，可讓已核准的標誌出現在參與平台中寄件者的電子郵件旁邊。

使用此標準，品牌可決定信箱提供者收件匣中應顯示的標誌。 一旦在所謂的BIMI DNS（域名系統）記錄中發佈，郵箱提供商可能會選擇此徽標，並在符合某些條件時將其顯示在收件箱中。

不同的提供者會執行不同的實作，但好處是顯而易見：站在收件箱中，建立信任，並控制顯示內容。

BIMI不會直接提高傳遞能力或提升您的聲譽。 它可協助您與收件者建立更多信任，進而促進更多參與。

## 看起來怎麼樣？

您可以找到不同提供者實作的一些範例，並進一步了解何者提供者會在 [BIMI組頁面](https://bimigroup.org/where-is-my-bimi-logo-displayed/){target="_blank"}.

## BIMI集團是誰？

BIMI小組是開發BIMI的工作組，因為它不僅涵蓋徽標，而且涵蓋技術、法律和合規要求。

BIMI集團由來自行業不同領域的若干利益相關方組成：Google、Yahoo、Fastmail、Proofpoint、Mailchimp、Sendgrid、Valimail和Validity。

## 誰在支援BIMI?

支援BIMI的郵箱提供商名單正在穩步增長。 可找到最新清單 [此處](https://bimigroup.org/bimi-infographic/){target="_blank"} 為考慮BIMI的支援提供商和提供商提供。

自2023年4月起，該清單包括Gmail、Yahoo、La Poste、Fastmail、Onet.pl和Zone、作為反垃圾郵件設備的Proofpoint和Apple Mail(自iOS16年起)。

名單上最知名的名字顯然是雅虎、Gmail和最近的一位採納者：Apple和iOS16。 Apple在此組合中扮演了特殊角色，因為他們不是郵箱提供商，但是他們向其本機郵件應用程式添加了BIMI支援。 符合BIMI的郵件將顯示為「經數字認證的電子郵件」，從而提高對該品牌的信任。

## 實作

實施BIMI確實需要執行幾個步驟：

1. 針對傳送網域及其組織網域在強制層級實作DMARC（網域型訊息驗證、報告及符合性） —  [深入了解](#dmarc)

1. 以SVGTinyPS格式建立您的品牌徽標 —  [深入了解](#create-brand-logo)

1. 註冊已驗證的標籤證書（僅某些提供商需要） —  [深入了解](#vmc)

1. 使用徽標和證書發佈BIMI DNS記錄 —  [深入了解](#publish-bimi-record)

1. 名聲不錯。 [深入了解](#good-reputation)

>[!NOTE]
>
>請注意，所有步驟都需要勾選。


### DMARC {#dmarc}

DMARC是一項標準，可讓品牌決定信箱提供者應對失敗的電子郵件採取什麼行動 [驗證](../additional-resources/authentication.md). 所謂的策略範圍從「無」到「隔離」（垃圾郵件資料夾放置），再到「拒絕」（直接阻止郵件）。 只有後兩種政策被稱為「執行」，並符合BIMI的資格。 由Adobe發送的郵件正在傳遞驗證，因為SPF（發件人策略框架）和DKIM（域密鑰標識郵件）是預設設定的。 Adobe正在您的傳送網域上依要求設定DMARC。

除了在傳送網域上使用DMARC外，還需要在組織網域的執行層級使用DMARC（如果傳送網域為news.example.com, example.com為組織網域）。

### 建立您的品牌標誌 {#create-brand-logo}

徽標的建立需要完全遵循要求。 請一律參閱 [BIMI Group的指引](https://bimigroup.org/creating-bimi-svg-logo-files/){target="_blank"}.

除了技術要求外，還有一些實用的建議，如有方形標誌、有實色背景等。 這些建議是最佳視覺效果。
請注意，不遵守規定可能會導致徽標不顯示。

### 已驗證標籤證書(VMC) {#vmc}

只有Gmail和Apple等信箱提供者才需要驗證的標籤憑證(VMC)，因此為選用。 不過，我們確實建議使用VMC來真正利用BIMI。

「已驗證的標籤證書」是品牌可使用徽標的法律驗證。 認證機構將通過註冊該品牌標誌的商標局來檢查此資訊。 此過程涉及多項法律驗證和檢查，可能需要一些時間。 目前有兩個CA（憑證授權單位）正在核發VMC:Digicert和Entrust。 首批商標辦事處是美國、加拿大、歐盟、英國、德國、日本、澳大利亞和西班牙。

一般來說，每個標誌需要一個VMC。 組織網域的VMC將涵蓋子網域，而且新增的功能甚至會涵蓋不同網域。 若您有不同的標誌，則需要多個VMC。 您選擇與之合作的認證機構或合作夥伴將協助您進行此設定。

>[!NOTE]
>
>請注意，VMC有年費。

### 發佈BIMI記錄 {#publish-bimi-record}

完成其他步驟後，即可發佈BIMI DNS記錄。 記錄如下：

```
default._bimi.[domain] IN TXT "v=BIMI1; l=[SVG URL]; a=[PEM URL]
```

&quot;PEM URL&quot;是已驗證標籤證書的檔案位置。

對於您的傳送網域，此作業需由Adobe完成。

### 名聲好 {#good-reputation}

信任是BIMI的關鍵。 用戶信任其郵箱提供程式只顯示合法發件人的徽標，因此郵箱提供程式需要信任該品牌，而這是由您的發件人信譽來完成的。 如果您聲譽很高，則一切都不錯，但如果您沒有，則不會顯示徽標。 很可惜，我們無法查看任何資訊或量度來判斷信譽是否足夠高。

即使花費精力和費用，VMC也不會拿走這部分。 如果信箱提供者不信任品牌，則不會顯示標誌。

## 秘訣與技巧

* BIMI Group為BIMI提供了一個方便的驗證工具。 如果您想再次檢查所有項目是否已設定並就緒，或只想查看標誌是否符合規範，請前往 [此連結](https://bimigroup.org/bimi-generator/){target="_blank"}. 若是後者，只需按一下 **[!UICONTROL Generate BIMI]** 並輸入佔位符域，但輸入正確的徽標URL。 檢查員會告訴你徽標是否合規。

* 沒有VMC，您就可以安全地開始，如果您的BIMI記錄不包含VMC URL，但該徽標已經顯示在Yahoo中，則您的聲譽不會受到任何損害。

* 在組織層面實施DMARC是一項浩大的工作。 有些公司專門幫助品牌實現DMARC的完全採用。

* 會發佈一長串常見問題 [此處](https://bimigroup.org/faqs-for-senders-esps/){target="_blank"}.
