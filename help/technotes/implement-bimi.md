---
title: 實施郵件識別的Gmail品牌指標(BIMI)
description: 瞭解如何實作BIMI
topics: Deliverability
role: Admin
level: Beginner
exl-id: 6b911bcc-a531-466a-8bd3-7fa469b96cc7
source-git-commit: 6b312cdbba496818337c97ec4f42962aea757901
workflow-type: tm+mt
source-wordcount: '1062'
ht-degree: 0%

---

# 實作 [!DNL Brand Indicators for Message Identification] (BIMI)

[!DNL Brand Indicators for Message Identification] (BIMI)是業界標準，可讓核准的標誌出現在參與平台中寄件者的電子郵件旁邊。

透過此標準，品牌可決定應顯示在信箱提供者收件匣中的標誌。 在所謂的BIMI DNS （網域名稱系統）記錄中發佈後，信箱提供者可能會挑選此標誌，並在符合特定條件時顯示在收件匣中。

不同的提供者會進行不同的實作，但優點很明顯：在收件匣中脫穎而出、建立信任並控制顯示的內容。

BIMI不會直接改善傳遞能力或您的聲譽。 這有助於與收件者建立更多信任，因此可促進更多互動。

## 看起來如何？

您可以找到不同提供者的實施範例，並深入瞭解哪些提供者在網站上確實顯示標誌 [BIMI群組頁面](https://bimigroup.org/where-is-my-bimi-logo-displayed/){target="_blank"}.

## BIMI群組是誰？

BIMI小組是開發BIMI的工作小組，它不僅涵蓋標誌，也涵蓋技術、法律和法規要求。

BIMI Group由業界不同領域的幾個利害關係人組成：Google、Yahoo、Fastmail、Proofpoint、Mailchimp、Sendgrid、Valimail和Validity。

## 誰支援BIMI？

支援BIMI的信箱提供者清單正在穩步成長。 可以找到最新清單 [此處](https://bimigroup.org/bimi-infographic/){target="_blank"} 支援提供者以及考慮BIMI的提供者。

截至2023年4月，此清單包括Gmail、Yahoo、La Poste、Fastmail、Onet.pl和Zone、Proofpoint as a anti-spam appliance和Apple Mail (自iOS 16起)。

名單上最顯眼的名字顯然是Yahoo、Gmail，以及最近採用的人之一：Apple與iOS 16。 Apple在混合中扮演特殊角色，因為他們不是信箱提供者，但他們確實將BIMI支援新增至其原生郵件應用程式。 符合BIMI規範的郵件將顯示為「數位認證的電子郵件」，以提升品牌的信任。

## 實作

實作BIMI有幾個步驟：

1. DMARC （網域型訊息驗證、報告和一致性）對傳送網域及其組織網域的執行層級實施 —  [瞭解更多](#dmarc)

1. 以SVGTinyPS格式建立您的品牌標誌 —  [瞭解更多](#create-brand-logo)

1. 註冊已驗證的標籤憑證（僅某些提供者需要） - [瞭解更多](#vmc)

1. 發佈包含標誌和憑證的BIMI DNS記錄 —  [瞭解更多](#publish-bimi-record)

1. 擁有良好的聲譽 —  [瞭解更多](#good-reputation)

>[!NOTE]
>
>請注意，所有步驟都必須勾選。


### DMARC {#dmarc}

DMARC是一種標準，可讓品牌決定信箱提供者應該如何處理失敗的電子郵件 [authentication](../additional-resources/authentication.md). 所謂的原則範圍從「無」到「隔離」（垃圾郵件資料夾位置）到「拒絕」（完全封鎖郵件）。 只有後兩項政策被稱為「強制執行」，且符合BIMI的資格。 由Adobe傳送的郵件正在傳遞驗證，因為SPF (Sender Policy Framework)和DKIM (Domain Keys Identified Mail)是依預設設定的。 Adobe正在您的傳送網域上依請求設定DMARC。

除了在傳送網域上使用DMARC之外，還需要在組織網域的強制層級上使用DMARC (如果傳送網域是news.example.com，example.com是組織網域)。

### 建立您的品牌標誌 {#create-brand-logo}

商標的建立必須完全符合要求。 請一律參考 [BIMI Group准則](https://bimigroup.org/creating-bimi-svg-logo-files/){target="_blank"}.

除了技術需求之外，還有一些實用的建議，例如使用方形標誌、以純色作為背景等等。 這些建議是最佳視覺化效果。
請注意，不符合規範可能會導致標誌無法顯示。

### 已驗證標籤憑證(VMC) {#vmc}

只有部分信箱提供者(例如Gmail和Apple)才需要驗證標籤憑證(VMC)，因此這是選擇性的。 我們建議您使用VMC，以真正運用BIMI。

已驗證的商標憑證是合法的驗證，該品牌可以使用標誌。 憑證授權單位會透過註冊品牌圖志的商標辦事處進行檢查。 此程式涉及數項法律驗證和檢查，可能需要一些時間。 目前有兩個CA （憑證授權單位）正在發行VMC：Digicert和Entrust。 第一組商標辦事處為美國、加拿大、歐盟、英國、德國、日本、澳洲和西班牙。

一般來說，每個標誌需要一個VMC。 為您的組織網域安裝VMC將涵蓋子網域，而且即使不同的網域也會增加一項功能。 如果您有不同的圖志，則需要多個VMC。 您選擇使用的憑證授權單位或合作夥伴將協助您進行此設定。

>[!NOTE]
>
>請注意，VMC需每年付費。

### 發佈BIMI記錄 {#publish-bimi-record}

完成其他步驟後，即可發佈BIMI DNS記錄。 記錄看起來像這樣：

```
default._bimi.[domain] IN TXT "v=BIMI1; l=[SVG URL]; a=[PEM URL]
```

「PEM URL」是「已驗證標籤憑證」的檔案位置。

針對您的傳送網域，這必須由Adobe完成。

### 良好的信譽 {#good-reputation}

信任是BIMI的關鍵。 使用者正在信任其信箱提供者，以便只顯示合法寄件者的標誌，因此信箱提供者需要信任品牌，而這是由您的寄件者信譽所完成。 如果您有很高的信譽，則一切都是好的，但如果您沒有信譽，則不會顯示標誌。 很遺憾，我們沒有任何資訊或量度可檢視，以判斷信譽是否足夠高。

即使完成VMC的工作和費用，也不會失去這個部分。 如果信箱提供者不信任品牌，將不會顯示標誌。

## 秘訣與技巧

* BIMI Group為BIMI提供便利的驗證工具。 若您想要再次檢查所有專案是否已設定妥當且準備就緒，或只想檢視標誌是否符合標準，請前往 [此連結](https://bimigroup.org/bimi-generator/){target="_blank"}. 如果是後者，只需按一下 **[!UICONTROL Generate BIMI]** 並輸入預留位置網域，但需輸入正確的標誌URL。 檢視員會告訴您標誌是否合規。

* 您可以安全地開始使用VMC，如果您的BIMI記錄不包含VMC URL，但標誌已可在Yahoo中顯示，則不會對您的信譽造成傷害。

* 在組織層面實施DMARC是一項浩大的工程。 有些公司專門協助品牌實現完全採用DMARC。

* 我們公佈了常見問題的詳盡清單 [此處](https://bimigroup.org/faqs-for-senders-esps/){target="_blank"}.
