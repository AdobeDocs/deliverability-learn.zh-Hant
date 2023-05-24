---
title: 驗證
description: 瞭解有關SPF、DKIM和DMARC驗證方法的詳細資訊。
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

SPF（發件人策略框架）是電子郵件身份驗證標準，允許域的所有者指定允許代表該域發送電子郵件的電子郵件伺服器。 此標準使用電子郵件「返迴路徑」標題（也稱為「信封發件人」地址）中的域。

>[!NOTE]
>
>您可以使用 [這個外部工具](https://www.kitterman.com/spf/validate.html) 驗證SPF記錄。

SPF是一種技術，在某種程度上，它使您能夠確保電子郵件中使用的域名未偽造。 從域接收消息時，查詢域的DNS伺服器。 響應是一個簡短記錄（SPF記錄），它詳細列出了哪些伺服器有權從此域發送電子郵件。 如果我們假定只有域的所有者有更改此記錄的方法，則我們可以認為此技術不允許偽造發件人地址，至少不允許偽造「@」右側的部分。

在決賽 [RFC 4408規範](https://www.rfc-editor.org/info/rfc4408)，消息的兩個元素用於確定被視為發送者的域：由SMTP &quot;HELO&quot;（或&quot;EHLO&quot;）命令指定的域和由&quot;Return-Path&quot;（或&quot;MAIL FROM&quot;）標頭的地址指定的域，也是彈出地址。 不同的考慮使得只考慮其中一個價值成為可能；我們建議確保兩個源指定相同的域。

檢查SPF可評估發送方域的有效性：

* **無**:無法執行評估。
* **中性**:查詢的域未啟用評估。
* **通過**:該域被認為是真實的。
* **失敗**:域是偽造的，應拒絕該消息。
* **軟失敗**:域可能是偽造的，但不應僅基於此結果拒絕消息。
* **臨時錯誤**:臨時錯誤停止了評估。 可以拒絕該消息。
* **錯誤**:域的SPF記錄無效。

值得注意的是，在DNS伺服器級別上記錄的時間可能長達48小時。 此延遲取決於接收伺服器的DNS快取刷新頻率。

## DKIM {#dkim}

DKIM(DomainKeys Indifed Mail)身份驗證是SPF的後繼身份。 它使用公鑰密碼技術，使接收電子郵件伺服器能夠驗證消息實際上是由其聲稱其發送者或實體發送的，以及消息內容是否在最初發送（和DKIM「簽名」）和接收時間之間發生了更改。 此標準通常使用「發件人」或「發件人」標題中的域。

DKIM來自DomainKeys和Yahoo! 和Cisco Identified Internet Mail驗證原則，用於檢查發送方域的真實性並保證消息的完整性。

DKIM已更換 **域密鑰** 驗證。

使用DKIM需要一些先決條件：

* **安全**:加密是DKIM的關鍵元素。 為確保DKIM的安全級別，建議採用最佳加密大小。 大多數訪問提供商認為低DKIM密鑰無效。
* **聲譽**:信譽基於IP和/或域，但不太透明的DKIM選擇器也是需要考慮的關鍵元素。 選擇選擇器非常重要：避免保留「預設」，因為「預設」是任何人都可以使用的，因此名聲不佳。 必須為 **保留與購買通信** 和驗證。

在中使用Campaign Classic時瞭解有關DKIM先決條件的詳細資訊 [此部分](/help/additional-resources/acc-technical-recommendations.md#dkim-acc)。

## DMARC {#dmarc}

DMARC（基於域的消息驗證、報告和一致性）是最新的電子郵件驗證形式，它依靠SPF和DKIM驗證來確定電子郵件是否通過。 DMARC在兩個重要方面都獨一無二且功能強大：

* 符合性 — 它允許發送方指示ISP如何處理任何未通過身份驗證的消息(例如：不要接受)。
* 報告 — 它向發送者提供詳細報告，顯示DMARC驗證失敗的所有郵件，以及每個郵件使用的「發件人」域和IP地址。 這使公司能夠識別身份驗證失敗且需要某種類型的「修復」（例如，將IP地址添加到其SPF記錄）的合法電子郵件，以及其電子郵件域上網路釣魚嘗試的來源和流行程度。

>[!NOTE]
>
>DMARC可以利用 [250ok](https://250ok.com/)。
