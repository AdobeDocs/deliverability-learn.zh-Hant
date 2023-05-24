---
title: 在Italia Online停機後更新彈跳資格
description: 瞭解如何在Italia Online停機後更新彈跳資格
feature: Deliverability
exl-id: a11e88cf-bf37-42cc-9c09-1d58360459b7
hide: true
hidefromtoc: true
source-git-commit: aca77fb9326e34455a6fec7ffc9a7ad8e1750467
workflow-type: tm+mt
source-wordcount: '422'
ht-degree: 3%

---

# 在 Italia 線上中斷後更新不正確的永久性硬退件 {#update-bounce-italia}

## 內容{#outage-context}

從當地時間1月22日起，Italia Online已經出現停運，導致數次延誤和拒絕電子郵件。 1月26日，該服務開始以有限容量恢復。

受影響的域包括： **libero.it**。 **維吉里奧，**。 **不管**。 **iol-it**, **blu.it**。

這個問題從1/22/2023到1/26/2023，但大部分檢疫錯誤發生在1月26日。

在官方通信中瞭解更多資訊 [這裡](https://tecnologia.libero.it/avviato-il-ritorno-online-di-libero-mail-e-virgilio-mail-66832){_blank}。


## 影響{#outage-impact}

與大多數網際網路服務提供商(ISP)停機時的情況一樣，通過Campaign或Journey Optimizer發送的一些電子郵件被錯誤地標籤為收款。 這不僅影響Adobe，而且在停機期間，所有試圖將電子郵件發送到Italia Online的人都會受到影響。

症狀是：

* **軟邊界** 和留言 `452 requested action aborted: try again later`  — 將自動重試這些操作，不需要執行任何操作。

* **硬邊界** 和留言 `550 <email address> recipient rejected` ISP已於當地時間1月26日上午8點至下午2點返回，以防止發送方繼續超載其伺服器。 如Italia Online Postmaster所確認的，這些並非真正的硬性限制，因此我們建議取消對因該郵件而於2023年1月26日排除的所有電子郵件地址的隔離。

## 要更新的進程{#outage-update}

### Adobe Campaign{#ac-update}

根據標準彈出處理邏輯，Adobe Campaign自動將這些收件人添加到隔離清單中， **[!UICONTROL Status]** 設定 **[!UICONTROL Quarantine]**。 要更正此問題，您需要通過查找和刪除這些收件人或更改其來更新市場活動中的隔離表 **[!UICONTROL Status]** 至 **[!UICONTROL Valid]** 這樣夜間清理工作流就會刪除它們。

要查找受此問題影響的收件人，或在其他ISP再次出現此問題時，請參閱以下說明：

* 有關Campaign Classicv7和促銷活動v8，請參閱 [此頁](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-quarantine-management.html?lang=en#unquarantine-bulk){_blank}。
* 有關Campaign Standard，請參閱 [此頁](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/understanding-quarantine-management.html?lang=en#unquarantine-bulk){_blank}。

### Adobe Journey Optimizer{#ajo-update}

根據標準彈出處理邏輯，Adobe Journey Optimizer自動將這些電子郵件地址添加到禁止清單中， **[!UICONTROL Reason]** 設定 **[!UICONTROL Invalid Recipient]**。 要更正此問題，您需要通過查找並刪除這些電子郵件地址來更新隱藏清單。

一旦識別，就可以使用 **[!UICONTROL Delete]** 按鈕 然後，這些地址可以包括在未來的電子郵件活動中。

瞭解詳情 [此部分](https://experienceleague.adobe.com/docs/journey-optimizer/using/configuration/monitor-reputation/manage-suppression-list.html#remove-from-suppression-list){_blank}。

