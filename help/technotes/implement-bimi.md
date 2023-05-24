---
title: 實施Gmail品牌標誌以識別郵件
description: 瞭解如何實施BIMI
topics: Deliverability
exl-id: 6b911bcc-a531-466a-8bd3-7fa469b96cc7
source-git-commit: aca2bfff9f0315b735cf0a97f2177083c58e0875
workflow-type: tm+mt
source-wordcount: '1062'
ht-degree: 0%

---

# 實施 [!DNL Brand Indicators for Message Identification] (BIMI)

[!DNL Brand Indicators for Message Identification] (BIMI)是一種行業標準，它允許在參與平台中的發件人電子郵件旁邊顯示一個經批准的標識。

使用此標準，品牌可以確定應在郵箱提供商的收件箱中顯示的徽標。 一旦在所謂的BIMI DNS（域名系統）記錄中發佈，郵箱提供商就可能會選擇此徽標，並在滿足某些條件時將其顯示在收件箱中。

不同的提供商執行不同的實施，但好處是顯而易見的：站在收件箱裡，建立信任，並控制所顯示的內容。

BIMI並不直接提高交付能力或聲譽。 它可以幫助您與收件人建立更多信任，從而推動更多的參與。

## 看起來怎麼樣？

您可以找到不同提供商實施的一些示例，以及有關哪些提供商確實在 [BIMI Group的頁面](https://bimigroup.org/where-is-my-bimi-logo-displayed/){target="_blank"}。

## BIMI集團是誰？

BIMI小組是開發BIMI的工作組，它不僅涵蓋標識，而且涵蓋技術、法律和遵約要求。

BIMI集團由來自行業不同領域的幾個利益相關者組成：Google、雅虎、Fastmail、Proofpoint、Mailchimp、Sendgrid、Valimail和Validity。

## 誰在支援BIMI?

支援BIMI的郵箱提供商的名單正在穩步增長。 可以找到最新清單 [這裡](https://bimigroup.org/bimi-infographic/){target="_blank"} 支援提供商和考慮BIMI的提供商。

截至2023年4月，該名單包括Gmail、Yahoo、La Poste、Fastmail、Onet.pl和Zone、Proofpoint作為反垃圾郵件設備和Apple郵件(從iOS16日起)。

該名單上最知名的名字顯然是雅虎、Gmail，以及最近一位採納者：Apple和iOS。 Apple在這一組合中扮演了特殊角色，因為他們不是郵箱提供商，但是他們確實將BIMI支援添加到了他們的本地郵件應用中。 符合BIMI的郵件將被顯示為&quot;經數字認證的電子郵件&quot;，這將增強人們對該品牌的信任。

## 實作

實施BIMI確實需要幾個步驟：

1. DMARC（基於域的消息驗證、報告和一致性）在發送域及其組織域的強制級別上實施 —  [瞭解更多資訊](#dmarc)

1. 以SVGTinyPS格式建立品牌徽標 —  [瞭解更多資訊](#create-brand-logo)

1. 註冊驗證的標籤證書（僅某些提供程式需要） —  [瞭解更多資訊](#vmc)

1. 發佈帶有徽標和證書的BIMI DNS記錄 —  [瞭解更多資訊](#publish-bimi-record)

1. 名聲不錯。 [瞭解更多資訊](#good-reputation)

>[!NOTE]
>
>請注意，所有步驟都需要檢出。


### DMARC {#dmarc}

DMARC是一種標準，它允許品牌決定郵箱提供商應如何處理失敗的電子郵件 [認證](../additional-resources/authentication.md)。 所謂的策略範圍從「無」到「隔離」（垃圾郵件資料夾放置）到「拒絕」（直接阻止郵件）。 只有後兩項政策被稱為&quot;執行&quot;，符合《國際移徙問題國際協定》。 由Adobe發送的郵件正在通過身份驗證，因為SPF（發件人策略框架）和DKIM（域密鑰標識的郵件）是預設設定的。 Adobe正在請求在您的發送域上設定DMARC。

除了發送域上的DMARC外，還需要在組織域的強制級別上使用DMARC（如果發送域是news.example.com, example.com是組織域）。

### 建立品牌徽標 {#create-brand-logo}

標識的建立需要完全遵循要求。 請始終參閱 [BIMI集團的指導方針](https://bimigroup.org/creating-bimi-svg-logo-files/){target="_blank"}。

除了技術要求外，還有一些實用的建議，如有方形標誌、有實色背景等。 這些建議是最佳可視化。
請注意，不遵守規定可能會導致徽標未顯示。

### 已驗證標籤證書(VMC) {#vmc}

只有Gmail和Apple等一些郵箱提供商才需要驗證標籤證書(VMC)，因此是可選的。 我們確實建議使用VMC來真正利用BIMI。

「經驗證的標籤證書」是品牌可以使用徽標的法律驗證。 認證機構將通過註冊商標的商標局進行檢查。 此過程涉及多個法律驗證和檢查，可能需要一些時間。 目前有兩個CA（證書頒發機構）正在頒發VMC:迪吉克特和Entrust。 首批商標辦事處是美國、加拿大、歐盟、英國、德國、日本、澳大利亞和西班牙。

通常，您需要每個徽標使用一個VMC。 為您的組織域安裝VMC將覆蓋子域，並且添加的功能甚至會覆蓋不同的域。 如果您有不同的徽標，則需要多個VMC。 您選擇使用的證書頒發機構或合作夥伴將幫助您設定此設定。

>[!NOTE]
>
>請注意，VMC每年收費。

### 發佈BIMI記錄 {#publish-bimi-record}

完成其他步驟後，可以發佈BIMI DNS記錄。 記錄如下：

```
default._bimi.[domain] IN TXT "v=BIMI1; l=[SVG URL]; a=[PEM URL]
```

「PEM URL」是「已驗證標籤證書」的檔案位置。

對於您的發送域，需要通過Adobe來完成。

### 名聲不錯 {#good-reputation}

信任是BIMI的關鍵。 用戶信任其郵箱提供商只顯示合法發件人的徽標，因此郵箱提供商需要信任該品牌，而這是由發件人信譽完成的。 如果你聲譽高，一切都好，但如果你沒有，徽標將不顯示。 遺憾的是，我們沒有任何資訊或衡量標準來判斷聲譽是否足夠高。

即使是經過VMC的努力和費用，也不會奪走這部分。 如果郵箱提供商不信任該品牌，則不會顯示徽標。

## 秘訣與技巧

* BIMI集團為BIMI提供了一個方便的驗證工具。 如果您想要再次檢查是否已設定並準備就緒，或者只想查看徽標是否符合要求，請轉至 [此連結](https://bimigroup.org/bimi-generator/){target="_blank"}。 對於後者，只需按一下 **[!UICONTROL Generate BIMI]** 並輸入佔位符域，但輸入正確的徽標URL。 檢查員會告訴你徽標是否符合。

* 您可以在沒有VMC的情況下安全地開始，如果您的BIMI記錄不包含VMC URL，但標識已經可以在雅虎中顯示，那麼您的聲譽就不會受到損害。

* 在組織層面實施DMARC是一項大工程。 一些公司專門幫助品牌實現DMARC的完全採用。

* 發佈了大量常見問題清單 [這裡](https://bimigroup.org/faqs-for-senders-esps/){target="_blank"}。
