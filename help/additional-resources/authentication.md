---
title: Authentication
description: 進一步瞭解SPF、DKIM和DMARC驗證方法。
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: 03609139-b39b-4051-bcde-9ac7c5358b87
TQID: https://experienceleague.adobe.com/zuhBmNWmF8CoCSNofsg3FKCcQFLOFfZmRutB2P1L4-U
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
  - id: d0a3eab4-7b10-4d96-a71e-6c0f8e7b7c87
  - id: dfc56824-e8b9-499e-85d4-21aedb507314
feature_v2:
  - id: ea90ebee-5c84-42d9-8b21-006bdabc95a3
  - id: f71e690b-4480-4b67-9ef5-88f42f9cdfdb
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
  - id: f8a45b24-4be7-4f1b-909b-60d06b483a20
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2:
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: 75df8537199680e5f1fc4b98cefdb05220fee7bf
workflow-type: tm+mt
source-wordcount: 769
ht-degree: 0%

---

# Authentication

## SPF {#spf}

SPF (Sender Policy Framework)是一種電子郵件驗證標準，允許網域的擁有者指定允許哪些電子郵件伺服器代表該網域傳送電子郵件。 此標準使用電子郵件的「Return-Path」標頭中的網域（也稱為「Envelope From」地址）。

>[!NOTE]
>
>您可以使用[這個外部工具](https://www.kitterman.com/spf/validate.html)來驗證SPF記錄。

SPF是一種技術，可讓您在某種程度上確保電子郵件中所使用的網域名稱不是偽造的。 從網域收到訊息時，會查詢網域的DNS伺服器。 回應是簡短記錄（SPF記錄），詳細說明哪些伺服器有權從此網域傳送電子郵件。 如果我們假設只有網域擁有者才有辦法變更此記錄，可以認為此技術不允許偽造寄件者地址，至少不允許偽造來自&quot;@&quot;右側的部分。

在最終的[RFC 4408規格](https://www.rfc-editor.org/info/rfc4408)中，訊息的兩個元素是用來判斷被視為寄件者的網域：由SMTP「HELO」（或「EHLO」）命令指定的網域，以及由「Return-Path」（或「MAIL FROM」）標頭位址（也是退信地址）指定的網域。 不同的考量使得僅考量這些值之一成為可能；我們建議確保兩個來源都指定相同的網域。

檢查SPF可提供寄件者網域有效性的評估：

* **無**：無法執行評估。
* **Neutral**：查詢的網域未啟用評估。
* **通過**：此網域被視為真實網域。
* **失敗**：網域是偽造的，應該拒絕郵件。
* **SoftFail**：網域可能是偽造的，但訊息不應僅根據此結果拒絕。
* **TempError**：暫時錯誤已停止評估。 可拒絕此訊息。
* **PermError**：網域的SPF記錄無效。

值得一提的是，在DNS伺服器層級建立的記錄最多可能需要48小時的時間才會列入考量。 此延遲取決於接收伺服器的DNS快取重新整理的頻率。

## DKIM {#dkim}

DKIM (DomainKeys Indified Mail)驗證是SPF的後續驗證。 它會使用公開金鑰加密技術，讓接收電子郵件伺服器來確認訊息是否確實是由其聲稱傳送者之人員或實體所傳送，以及訊息內容在最初傳送時間（和DKIM「簽署」）與接收時間之間是否有變更。 此標準通常使用「寄件者」或「寄件者」標題中的網域。

DKIM來自DomainKeys、Yahoo！ 和Cisco識別的網際網路郵件驗證原則，用於檢查寄件者網域的真實性，並保證訊息的完整性。

DKIM已取代&#x200B;**DomainKeys**&#x200B;驗證。

使用DKIM需要一些先決條件：

* **安全性**：加密是DKIM的金鑰元素。 為確保DKIM的安全性等級，建議的最佳實務加密大小為1024b。 大部分存取提供者不會將下層DKIM金鑰視為有效。
* **信譽**：信譽是以IP和/或網域為基礎，但較不透明的DKIM選取器也是要考慮的關鍵元素。 選擇選擇器相當重要：請避免保留「預設」選擇器，此選擇器可供任何使用者使用，因此信譽不佳。 您必須針對&#x200B;**保留與贏取通訊**&#x200B;以及驗證實作不同的選擇器。

在[本節](/help/additional-resources/acc-technical-recommendations.md#dkim-acc)中進一步瞭解使用Campaign Classic時的DKIM必要條件。

## DMARC {#dmarc}

DMARC (Domain-based Message Authentication, Reporting and Conformance)是最新的電子郵件驗證形式，其需仰賴SPF和DKIM驗證來判斷電子郵件是否通過。 DMARC獨一無二且功能強大，有兩個重要方面：

* 符合性 — 這可讓傳送者指示ISP如何處理無法驗證的任何訊息（例如：不要接受）。
* 報告 — 提供傳送者詳細的報告，顯示所有DMARC驗證失敗的訊息，以及每個訊息使用的「寄件者」網域和IP位址。 這可讓公司識別未通過驗證且需要某種「修正」型別的合法電子郵件（例如，將IP位址新增至其SPF記錄），以及其電子郵件網域上網路釣魚企圖來源和普遍程度。

>[!NOTE]
>
>DMARC可善用[250ok](https://250ok.com/)產生的報告。
