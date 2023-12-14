---
title: 以下網站所宣佈的變更指引： [!DNL Google] 和 [!DNL Yahoo]
description: 以下網站所宣佈的變更指引： [!DNL Google] 和 [!DNL Yahoo]
role: Admin
level: Experienced
doc-type: Article
last-substantial-update: 2023-11-06T00:00:00Z
jira: KT-14320
thumbnail: KT-14320.jpeg
exl-id: 879e9124-3cfe-4d85-a7d1-64ceb914a460
source-git-commit: 16ff60cdcb1ca1558b8021d27b235b6977c2f40a
workflow-type: tm+mt
source-wordcount: '1565'
ht-degree: 0%

---

# 以下網站所宣佈的變更指引： [!DNL Google] 和 [!DNL Yahoo]

在10月3日，兩者皆有 [!DNL Google] 和 [!DNL Yahoo] 透過部落格共同宣佈計畫變更。 這些變更旨在從2024年2月1日起，強制實行某些常見業界最佳實務作為新要求，以讓使用者更安全，並提供更佳的電子郵件體驗。

[https://blog.google/products/gmail/gmail-security-authentication-spam-protection/](https://blog.google/products/gmail/gmail-security-authentication-spam-protection/){target="_blank"}

![[!DNL Google] 宣告_](/help/assets/Gmail.png)

[https://blog.postmaster.yahooinc.com/post/730172167494483968/more-secure-less-spam](https://blog.postmaster.yahooinc.com/post/730172167494483968/more-secure-less-spam){target="_blank"}

![[!DNL Yahoo] 宣告](/help/assets/Yahoo.png)

Adobe的電子郵件傳遞能力專家已閱讀這些部落格和所有連結檔案，因此您不必這麼做。 以下是主要要點。

## 那麼，到底是什麼 [!DNL Google] 和 [!DNL Yahoo] 正在進行？

在電子郵件領域中，存在著法律要求、實際要求和一般最佳實務。 不同地點的法律要求差異很大，不屬於本主題。 而是 [!DNL Google] 和 [!DNL Yahoo] 採用最佳實務，並將其變成實際需求。

無任何專案 [!DNL Google] 和 [!DNL Yahoo] 2月開始推出這項新功能，這通常是多年來的最佳實務建議，但業界採用速度緩慢且參差不齊。 這是 [!DNL Google] 和 [!DNL Yahoo]這是協助推行採用程式的方法，方法是：「如果您想要將電子郵件部署給我們的使用者（這或許代表您電子郵件清單的重要部分，在某些情況下高達70%，視地區和行業而定），您就需要執行這些操作。」

## 有哪些詳細資訊？

如果您是Adobe客戶，他們的大多數要求都是您設定的一部分，但有一些關鍵專案在技術上屬於選用性，並且您想要確定您的組織在2024年2月之前已正確採用和設定這些專案。

## DMARC：

[!DNL Google] 和 [!DNL Yahoo] 都需要您擁有DMARC記錄，才能存取您用來傳送電子郵件給他們的任何網域。 這些設定目前不要求p=reject或p=quarantine設定，因此p=none （通常稱為「監視」設定）的設定是完全可以接受的。 這不會改變您電子郵件的處理方式，他們會照常不使用DMARC的方式處理。 設定此設定是使用DMARC保護自己的第一步，此外還有協助您傳送電子郵件給的新優點 [!DNL Google] 和 [!DNL Yahoo] 它還可以協助您檢視電子郵件生態系統中是否有驗證問題。

DMARC的規則不會變更，這表示除非設定為避免，否則父系網域上的DMARC記錄(例如adobe.com)將會繼承並涵蓋任何子網域(例如email.adobe.com)。 您的子網域不需要不同的DMARC記錄，除非您出於各種商業原因想要或需要新增它們。

Adobe目前完全支援DMARC，但並非必要。 使用任何免費的DMARC檢查器來檢視您的子網域是否有DMARC設定，若沒有，請洽詢您的Adobe支援團隊，以瞭解如何以最佳方式完成該設定。

您也可以找到有關DMARC及其實作方法的詳細資訊 [此處](https://experienceleague.adobe.com/docs/deliverability-learn/deliverability-best-practice-guide/additional-resources/technotes/implement-dmarc.html?lang=zh-Hant){target="_blank"} for Adobe Campaign or [here](https://experienceleague.adobe.com/docs/marketo/using/getting-started-with-marketo/setup/configure-protocols-for-marketo.html){target="_blank"} 用於Marketo Engage。

## 按一下（清單）取消訂閱：

不要驚慌。 [!DNL Google] 和 [!DNL Yahoo] 不是在談論您的電子郵件內文或頁尾中的取消訂閱連結，這些連結可能被安全機器人在履行其職責或發生意外時點選。 這表示「mailto」或「http/URL」版本的List-Unsubscribe標題功能。 此為中的函式 [!DNL Yahoo] 和Gmail UI，使用者可在此按一下取消訂閱。 Gmail甚至會提示按一下「回報垃圾訊息」的使用者，檢視他們是否想要取消訂閱，這可以透過將他們轉換為取消訂閱（不會損害您的聲譽）來減少您收到的投訴數量（投訴會損害您的聲譽）。

請務必注意 [!DNL Google] 和 [!DNL Yahoo] 兩者都是以名稱「1-Click」指代「http/URL」選項，這是刻意為之。 技術上，原始的「http/URL」選項可讓您將收件者重新導向至網站。 這不是 [!DNL Yahoo] 和 [!DNL Google]，兩者都參照更新的 [RFC8058](https://datatracker.ietf.org/doc/html/rfc8058){target="_blank"} 此方法的重點在於透過HTTPSPOST請求（而非網站）處理取消訂閱，使其成為「一鍵式」。

今天，Gmail接受「mailto」清單取消訂閱選項。 Gmail表示，「mailto」未達到他們未來的期望，傳送者需要啟用「發佈」清單取消訂閱選項。 已具備某些型別的清單取消訂閱的寄件者，在2024年6月1日之前，必須具備「1鍵式」清單取消訂閱功能。

[!DNL Yahoo] 表示會暫時繼續遵守「mailto」選項，但日後也會要求「post」。

Adobe建議同時使用「mailto」和「post/1-Click」清單取消訂閱選項。 Adobe正致力於在所有電子郵件傳送平台上啟用「貼文」支援，以支援使用者符合這些需求，並進一步更新內容。

交易式電子郵件不需要清單取消訂閱標頭。 請注意，已捨棄購物車等觸發式訊息及訂閱者未產生的類似通訊，會被視為信箱提供者（例如）的行銷 [!DNL Google] 和 [!DNL Yahoo] 而那些則需要清單取消訂閱。

[!DNL Google] 和 [!DNL Yahoo] 兩人都知道，在某些情況下，收件者將取消訂閱，然後在稍後重新訂閱。 雖然他們不願意分享他們如何識別這些情況的秘密秘密，但他們正在努力尋找方法，以避免在這些情況下錯誤地懲罰發件人。

>[!INFO]
> 如需如何實作list-unsubscribe解決方案的詳細資訊，請檢視：
> * [!DNL Adobe Campaign Classic]： [技術建議](https://experienceleague.adobe.com/docs/deliverability-learn/deliverability-best-practice-guide/additional-resources/campaign/acc-technical-recommendations.html?lang=en#list-unsubscribe){target="_blank"}
>* [!DNL Adobe Campaign Standard]： [什麼是List-Unsubscribe標頭？ 如何在ACS中實作此步驟？](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-14778.html?lang=en){target="_blank"}
>* [!DNL Adobe Journey Optimizer]： [電子郵件選擇退出管理](https://experienceleague.adobe.com/docs/journey-optimizer/using/email/email-opt-out.html?lang=en){target="_blank"}
>
> 或隨時聯絡Adobe客戶支援團隊。


## 處理在2天內取消訂閱：

我們建議您採取此最佳作法一段時間，因為您部署給已取消訂閱之人員的每封電子郵件通常都會造成垃圾郵件投訴，因此越早停止傳送電子郵件越好。 同樣地，在某些情況下，法律要求可能更長，但 [!DNL Google] 和 [!DNL Yahoo] 將會知道其使用者已透過List-Unsubscribe取消訂閱，而您仍在第3天傳送電子郵件給他們，他們已表示不允許進行此操作的寄件者繼續傳送電子郵件給其任何使用者。

此2天需求適用於透過各種清單取消訂閱方法的任何取消訂閱。 在某些情況下（例如「mailto」），這表示Adobe會處理這些專案。 Adobe會在收到要求後立即處理所有取消訂閱要求，並在2天限制內處理。 在其他情況下，您可能會處理這些變數。 如果您正在處理這些請求，則可能需要在端進行變更以符合此2天時間表。

## 投訴率：

長久以來，將低投訴率維持在0.2%以下都是最佳作法。 [!DNL Google] 會將長時間的標杆設為0.3%，但明確指出將此標杆設為0.1%以下會有好處。 以下是他們共用的詳細資料：

* 將垃圾郵件率維持在0.10%以下。
* 避免0.30%或更高的垃圾郵件率，特別是對於任何持續的時間段。
* 維持低垃圾郵件率，可讓傳送者更能因應使用者意見反應偶爾尖峰的情形。
* 同樣地，維持高垃圾郵件率將導致垃圾郵件分類增加。 改善垃圾郵件率可能需要一些時間，才能對垃圾郵件分類產生正面影響。

[!DNL Yahoo] 已表示他們的投訴臨界值也將在0.30%範圍內。

[!DNL Google] 和 [!DNL Yahoo]的目標不是因單一不良日數或導致投訴暫時激增的錯誤而懲罰寄件者。 相反地，他們專注於在很長一段時間內投訴率很高或傳送行為不好的寄件者。

如果您需要協助以監控您的投訴率，或想協助您制定減少投訴的策略，請洽詢您的Adobe傳遞顧問，或洽詢您的客戶團隊，以新增傳遞顧問（如果您尚未有）。

## 這對行銷人員有何影響？

未能遵守Gmail和 [!DNL Yahoo] 預期會導致電子郵件進入垃圾郵件資料夾或遭到封鎖（即從MBP返回指出電子郵件未傳遞）。

因此，Adobe強烈建議您完成上述變更，並確保儘快開始遵守這些變更。 現在也是您開始效能標竿的大好時機 [!DNL Yahoo] 和 [!DNL Google] 可讓您檢視您的量度在二月是否有任何重大變更。

我們在此提供協助，因此如果您有任何問題或需要支援，請洽詢您的Adobe傳遞顧問，或洽詢您的帳戶團隊，如果您還沒有傳遞顧問，請新增傳遞顧問。

## 有辦法解決這個問題嗎？

雖然這永遠是個問題，但事實是這些變更對使用者而言是有意義的 [!DNL Google] 和 [!DNL Yahoo]的平台。 這些使用者的這些要求會激勵他們執行這些規則。 我們不建議嘗試避免任何這些變更，而是要後退一步，考慮如何因應這些變更。

## 最終備註：

請注意，這目前不適用於傳送至的電子郵件 [!DNL Yahoo].JP或 [!DNL Gmail] 但是，它適用於來自這些位置的電子郵件。

## 其他資源（非這些變更專用）：

[Google寄件者准則](https://support.google.com/mail/answer/81126){target="_blank"}

[Google常見問題集](https://support.google.com/a/answer/14229414?sjid=2864589551334481470-NC){target="_blank"}

[Yahoo寄件者准則](https://senders.yahooinc.com/best-practices/){target="_blank"}

[Yahoo常見問題集](https://senders.yahooinc.com/faqs/){target="_blank"}
