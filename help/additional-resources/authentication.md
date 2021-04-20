---
title: 驗證
description: 進一步瞭解SPF、DKIM和DMARC驗證方法。
feature: Additional resources
topics: Deliverability
kt: null
thumbnail: null
doc-type: article
activity: understand
team: ACS
translation-type: tm+mt
source-git-commit: 1e539b5df54250a5927701009e7a9c84e5d73fae
workflow-type: tm+mt
source-wordcount: '764'
ht-degree: 0%

---


# 驗證

## SPF {#spf}

SPF（發件人策略框架）是一種電子郵件驗證標準，允許域的所有者指定允許代表該域發送電子郵件的電子郵件伺服器。 此標準使用電子郵件「回傳路徑」標題（也稱為「封套寄件者」位址）中的網域。

>[!NOTE]
>
>您可以使用[此外部工具](https://www.kitterman.com/spf/validate.html)來驗證SPF記錄。

SPF是一種技術，在某種程度上，它使您能夠確保電子郵件中使用的域名不偽造。 當從域收到消息時，查詢域的DNS伺服器。 回應是簡短記錄（SPF記錄），詳細列出哪些伺服器有權從此網域傳送電子郵件。 如果我們假定只有域的所有者有更改此記錄的方法，我們可以認為此技術不允許偽造發送者地址，至少不允許偽造&quot;@&quot;右側的部分。

在最終的[RFC 4408規範](https://www.rfc-editor.org/info/rfc4408)中，消息的兩個元素用於確定被視為發送方的域：由SMTP &quot;HELO&quot;（或&quot;EHLO&quot;）命令指定的域和由&quot;Return-Path&quot;（或&quot;MAIL FROM&quot;）標題地址指定的域，也就是彈回地址。 不同的考慮使得只考慮其中一個值成為可能；我們建議確保兩個來源都指定相同的網域。

檢查SPF可評估發件人域的有效性：

* **無**:無法執行任何評估。
* **中立**:查詢的域不啟用評估。
* **通過**:該域被認為是真實的。
* **失敗**:域是偽造的，消息應被拒絕。
* **SoftFail**:域可能是偽造的，但消息不應僅基於此結果而拒絕。
* **TempError**:暫時錯誤會停止評估。可以拒絕消息。
* **PermError**:域的SPF記錄無效。

值得注意的是，在DNS伺服器級別記錄可能需要48小時才能考慮在內。 此延遲取決於接收伺服器的DNS快取刷新頻率。

## DKIM {#dkim}

DKIM(DomainKeys Indified Mail)身份驗證是SPF的後繼身份。 它使用公開金鑰加密，允許接收的電子郵件伺服器確認訊息確實是由其聲稱其所傳送之個人或實體所傳送，以及訊息內容是否在最初傳送（以及DKIM「簽署」）與接收之間發生變更。 此標準通常使用「寄件者」或「寄件者」標題中的網域。

DKIM來自DomainKeys、Yahoo! 和Cisco Identified Internet Mail身份驗證原則，用於檢查發件人域的真實性並保證消息的完整性。

DKIM已更換&#x200B;**DomainKeys**&#x200B;驗證。

使用DKIM需要一些必要條件：

* **安全性**:加密是DKIM的關鍵元素。為確保DKIM的安全級別，建議使用1024b作為最佳做法。 大部分的存取提供者不認為低DKIM金鑰有效。
* **聲譽**:信譽是以IP和／或網域為基礎，但較不透明的DKIM選擇器也是需要考慮的關鍵元素。選擇選擇器很重要：避免保留任何人都能使用的「違約」，因此名聲不佳。 您必須為&#x200B;**保留與贏取通訊**&#x200B;實施不同的選擇器，以進行驗證。

在[本節](/help/additional-resources/acc-technical-recommendations.md#dkim-acc)中使用Campaign Classic時，進一步瞭解DKIM先決條件。

## DMARC {#dmarc}

DMARC（基於域的消息驗證、報告和符合性）是最新的電子郵件驗證形式，它依賴SPF和DKIM驗證來確定電子郵件是否通過或失敗。 DMARC在兩個重要方面獨一無二且功能強大：

* 符合性——允許發送方指示ISP如何處理任何無法驗證的消息(例如：不接受)。
* 報告——它為發送者提供詳細報告，顯示所有未通過DMARC驗證的消息，以及每個消息所使用的「發件人」域和IP地址。 這可讓公司識別驗證失敗且需要某些類型「修正」（例如，將IP位址新增至其SPF記錄）的合法電子郵件，以及其電子郵件網域上網路釣魚企圖的來源和普遍性。

>[!NOTE]
>
>DMARC可運用由[250ok](https://250ok.com/)產生的報表。
