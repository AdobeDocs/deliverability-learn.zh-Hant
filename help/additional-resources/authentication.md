---
title: 驗證
description: 進一步瞭解SPF、DKIM和DMARC驗證方法。
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

SPF (Sender Policy Framework)是一種電子郵件驗證標準，允許網域的擁有者指定允許哪些電子郵件伺服器代表該網域傳送電子郵件。 此標準使用電子郵件的「Return-Path」標頭中的網域（也稱為「Envelope From」地址）。

>[!NOTE]
>
>您可以使用 [此外部工具](https://www.kitterman.com/spf/validate.html) 驗證SPF記錄。

SPF是一種技術，可讓您在某種程度上確保電子郵件中所使用的網域名稱不是偽造的。 從網域收到訊息時，會查詢網域的DNS伺服器。 回應是簡短記錄（SPF記錄），詳細說明哪些伺服器有權從此網域傳送電子郵件。 如果我們假設只有網域擁有者才有辦法變更此記錄，可以認為此技術不允許偽造寄件者地址，至少不允許偽造來自&quot;@&quot;右側的部分。

在最後 [RFC 4408規格](https://www.rfc-editor.org/info/rfc4408)，會使用郵件的兩個元素來判斷當作寄件者的網域：SMTP &quot;HELO&quot; （或&quot;EHLO&quot;）命令所指定的網域，以及&quot;Return-Path&quot; （或&quot;MAIL FROM&quot;）標頭位址（也是退信地址）所指定的網域。 不同的考量使得僅考量這些值之一成為可能；我們建議確保兩個來源都指定相同的網域。

檢查SPF可提供寄件者網域有效性的評估：

* **無**：無法執行評估。
* **中性**：查詢的網域未啟用評估。
* **通過**：此網域視為正版。
* **失敗**：網域是偽造的，應該拒絕訊息。
* **SoftFail**：網域可能是偽造的，但訊息不應僅根據此結果遭到拒絕。
* **TempError**：暫時錯誤已停止評估。 可拒絕此訊息。
* **PermError**：網域的SPF記錄無效。

值得一提的是，在DNS伺服器層級建立的記錄最多可能需要48小時的時間才會列入考量。 此延遲取決於接收伺服器的DNS快取重新整理的頻率。

## DKIM {#dkim}

DKIM (DomainKeys Indified Mail)驗證是SPF的後續驗證。 它使用公開金鑰加密技術，讓接收電子郵件伺服器可驗證訊息是否確實是由其聲稱傳送者所傳送的個人或實體所傳送，以及訊息內容在最初傳送時間（和DKIM「簽署」）與接收時間之間是否有變更。 此標準通常使用「寄件者」或「寄件者」標題中的網域。

DKIM來自DomainKeys、Yahoo！ 和Cisco識別的網際網路郵件驗證原則，用於檢查寄件者網域的真實性，並保證訊息的完整性。

已取代DKIM **網域索引鍵** 驗證。

使用DKIM需要一些先決條件：

* **安全性**：加密是DKIM的關鍵元素。 為確保DKIM的安全性等級，1024b是建議的最佳實務加密大小。 大多數存取提供者不會將下層DKIM金鑰視為有效。
* **信譽**：信譽是以IP和/或網域為基礎，但較不透明的DKIM選取器也是要考慮的關鍵元素。 選擇選擇器相當重要：請避免保留「預設」選擇器，此選擇器可供任何使用者使用，因此信譽不佳。 您必須實作另一個選擇器，用於 **保留與贏取通訊** 和用於驗證。

瞭解更多有關在中使用Campaign Classic時的DKIM先決條件 [本節](/help/additional-resources/acc-technical-recommendations.md#dkim-acc).

## DMARC {#dmarc}

DMARC （網域型訊息驗證、報告和符合性）是電子郵件驗證的最新形式，它依賴SPF和DKIM驗證來判斷電子郵件通過還是失敗。 DMARC獨一無二，功能強大，有兩個重要方面：

* 符合性 — 這可讓傳送者指示ISP如何處理無法驗證的任何訊息（例如：不要接受）。
* 報告 — 它提供傳送者詳細的報告，顯示所有失敗DMARC驗證的訊息，以及每個訊息使用的「寄件者」網域和IP位址。 這可讓公司識別未通過驗證且需要某種「修正」型別的合法電子郵件（例如，將IP位址新增至其SPF記錄），以及其電子郵件網域上網路釣魚企圖來源和普遍程度。

>[!NOTE]
>
>DMARC可善用以下程式碼產生的報告： [250ok](https://250ok.com/).
