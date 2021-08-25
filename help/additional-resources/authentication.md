---
title: 驗證
description: 進一步了解SPF、DKIM和DMARC驗證方法。
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: 03609139-b39b-4051-bcde-9ac7c5358b87
source-git-commit: d6094cd2ef0a8a7741e7d8aa4db15499fad08f90
workflow-type: tm+mt
source-wordcount: '762'
ht-degree: 0%

---

# 驗證

## SPF {#spf}

SPF（發件人策略框架）是電子郵件驗證標準，允許域的所有者指定允許哪些電子郵件伺服器代表該域發送電子郵件。 此標準使用電子郵件「回傳路徑」標題中的網域（亦稱為「封套寄件者」地址）。

>[!NOTE]
>
>您可以使用[此外部工具](https://www.kitterman.com/spf/validate.html)來驗證SPF記錄。

SPF是一種技術，在某種程度上，它使您能夠確保電子郵件中使用的域名不偽造。 從域接收消息時，將查詢該域的DNS伺服器。 回應是簡短記錄（SPF記錄），詳細說明哪些伺服器已獲授權從此網域傳送電子郵件。 如果我們假定只有域的所有者有更改此記錄的手段，我們可以認為此技術不允許偽造發件人地址，至少不允許偽造「@」右側的部分。

在最終的[RFC 4408規範](https://www.rfc-editor.org/info/rfc4408)中，消息的兩個元素用於確定被視為發送者的域：由SMTP &quot;HELO&quot;（或&quot;EHLO&quot;）命令指定的域以及由&quot;Return-Path&quot;（或&quot;MAIL FROM&quot;）標頭的地址指定的域，也是退信地址。 不同的考量使得僅考慮這些值之一；建議確保兩個來源指定相同的網域。

檢查SPF可評估發送者域的有效性：

* **無**:無法執行評估。
* **中性**:查詢的域不啟用評估。
* **通過**:域被認為是真的。
* **失敗**:域已偽造，應拒絕消息。
* **SoftFail**:域可能是偽造的，但不應僅基於此結果拒絕消息。
* **TempError**:暫時錯誤停止了評估。可以拒絕該消息。
* **PermError**:域的SPF記錄無效。

值得注意的是，在DNS伺服器級別進行的記錄最多需要48小時才能列入考量。 此延遲取決於接收伺服器的DNS快取刷新頻率。

## DKIM {#dkim}

DKIM(DomainKeys Indified Mail)身份驗證是SPF的後繼身份驗證。 它使用公鑰密碼，允許接收電子郵件伺服器驗證消息實際上是由其聲稱由其發送的個人或實體發送的，以及消息內容在最初發送時（和DKIM「簽名」）和接收時間之間是否發生了更改。 此標準通常使用「寄件者」或「寄件者」標題中的網域。

DKIM來自DomainKeys、Yahoo! 和Cisco識別的Internet Mail驗證原則，用於檢查發件人域的真實性並保證郵件的完整性。

DKIM已取代&#x200B;**DomainKeys**&#x200B;驗證。

使用DKIM需要一些必要條件：

* **安全性**:加密是DKIM的關鍵元素。為確保DKIM的安全級別，1024b是建議的最佳加密大小。 大多數訪問提供商不認為較低的DKIM密鑰有效。
* **聲譽**:信譽以IP和/或網域為基礎，但較不透明的DKIM選取器也是需要考慮的關鍵元素。選擇選取器很重要：避免保留任何人都可以使用的「預設」，因此聲譽不佳。 您必須針對&#x200B;**保留與贏取通訊**&#x200B;實作不同的選取器，並進行驗證。

在[此區段](/help/additional-resources/acc-technical-recommendations.md#dkim-acc)中使用Campaign Classic時，進一步了解DKIM必備條件。

## DMARC {#dmarc}

DMARC（基於域的郵件驗證、報告和符合性）是最新的電子郵件驗證形式，它依賴SPF和DKIM驗證來判斷電子郵件是否通過。 DMARC在兩個重要方面獨一無二且功能強大：

* 符合性 — 可讓寄件者指示ISP如何處理任何無法驗證的訊息(例如：不要接受)。
* 報告 — 它向發送方提供詳細報告，顯示所有失敗的DMARC驗證郵件，以及每個郵件所使用的「發件人」域和IP地址。 這可讓公司識別驗證失敗且需要某種類型的「修正」（例如，在其SPF記錄中添加IP地址）的合法電子郵件，以及其電子郵件域上網路釣魚嘗試的來源和流行性。

>[!NOTE]
>
>DMARC可以利用[250ok](https://250ok.com/)產生的報表。
