---
title: 實作Gmail的品牌指標以識別訊息(BIMI)
description: 了解如何實作BIMI
topics: Deliverability
hide: true
hidefromtoc: true
source-git-commit: ab1595bac7ef136eb001609b9017950a2d01cbb4
workflow-type: tm+mt
source-wordcount: '698'
ht-degree: 0%

---


# 實作Gmail的品牌指標以識別訊息(BIMI)

Gmail最近宣佈將[推出BIMI](https://cloud.google.com/blog/products/identity-security/bringing-bimi-to-gmail-in-google-workspace)的一般支援。 您必須先處理許多項目，才能善加利用，但包括：已驗證標籤證書、商標標識、格式正確的徽標、DMARC設定，並最終將BIMI記錄發佈到您的DNS。 我們將在本文中檢閱所有這些步驟。

報文標識的品牌指標(BIMI)是一個行業標準，允許在參與的平台中發送者的電子郵件旁邊顯示一個經過批准的徽標。 這種吸引眼球的做法不僅可能會促進參與，還有助於確認發送者的真實性，從而降低網路釣魚和其他垃圾郵件策略的風險。

## 已驗證標籤證書

Gmail BIMI計畫的一個關鍵組成部分是要求發件人擁有由有效證書頒發機構頒發的經驗證的標籤證書(VMC)。 目前，這些VMC只能從Entrust和DigiCert獲得，但Gmail發佈後，這些提供商的名單可能會增長。

VMC在某些方面與SSL憑證類似。 對於要顯示的每個徽標，您都需要一個VMC，因此，如果您有很多品牌，您應計畫需要多個VMC。 每個VMC在多個域之間都有效，但如果您獲得多SAN VMC。 因此，如果您希望多個傳送網域之間出現一個標誌，則只需要一個VMC。

## 標誌商標

取得VMC之前，還有另一個重要步驟必須完成：要獲取VMC，您要顯示的徽標必須註冊到8個經批准的全球商標和專利局之一。

* 美國專利和商標局(USPTO)
* 加拿大智慧財產權局
* 歐盟智慧財產權局
* 英國智慧財產權局
* 德國專利與市場
* 日本商標局
* 西班牙專利和商標局
* IP澳大利亞

如果您要顯示的徽標未註冊，或未註冊到這8個組織中的一個，則您需要與法律團隊合作解決此問題，然後才能申請VMC。

## 標誌影像格式

這也是確保您的徽標符合BIMI徽標格式要求的好時機。 它必須為SVG格式，並符合SVG Portable/Secure(SVG-P/S)設定檔。 關於如何這樣做的指導可在[BIMI工作組](https://bimigroup.org/svg-conversion-tools-released)上找到。

## DMARC

一旦您的商標標識格式正確，並且您的「已驗證的標籤證書」，您還需要確保在希望BIMI工作的任何發送域上完全配置DMARC。

這包括確保P=設定為「隔離」或「拒絕」。 如果您的DMARC使用P=None，將不符合BIMI的資格。 強烈建議使用P=None設定，以確保您知道從域傳出的郵件，並且如果您更改為「隔離」或「拒絕」，則不會意外阻止任何郵件，請將其視為測試和資訊收集階段。 在BIMI可供您使用之前，您需要超越這一限制，開始執行。

## DNS項目

隨著其他所有項目最終排齊並準備就緒，是時候使用您的BIMI更新DNS條目了。

此簡單項目應如下所示：

```
default._bimi.[domain] IN TXT “v=BIMI1; l=[SVG URL] 
```

您可以獲取該條目的詳細資訊，甚至在[BIMI工作組站點](https://bimigroup.org/implementation-guide)使用免費的BIMI檢查器。


## 主要優點

如果您是Adobe Campaign或Marketo用戶端，Adobe可協助您建立BIMI DNS更新：請連絡Adobe客戶服務以請求。 如果BIMI無法為您正常運作，Adobe也有助於疑難排解。

如需商標或已驗證標籤憑證的協助，請與您的法律團隊和授權的VMC廠商合作。

取得Gmail的BIMI設定可能不是一個快速的程式，但從行銷和安全性的角度來看，這個程式可能有顯著效益。
