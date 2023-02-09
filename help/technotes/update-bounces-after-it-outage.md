---
title: 在Italia Online中斷後更新退信資格
description: 了解如何在Italia Online中斷後更新退信資格
feature: Deliverability
exl-id: a11e88cf-bf37-42cc-9c09-1d58360459b7
hide: true
hidefromtoc: true
source-git-commit: aca77fb9326e34455a6fec7ffc9a7ad8e1750467
workflow-type: tm+mt
source-wordcount: '422'
ht-degree: 1%

---

# 在Italia Online中斷後更新不正確的硬退信 {#update-bounce-italia}

## 內容{#outage-context}

自1月22日（當地時間）起，Italia Online已經發生中斷，導致數次延遲和拒絕的電子郵件。 1月26日，該服務開始以有限的容量恢復。

受影響的網域包括： **libero.it**, **virgilio.it**, **inwind.it**, **iol.it**，和 **blu.it**.

此問題發生在1/22/2023到1/26/2023之間，但大部分的錯誤隔離發生在1月26日。

進一步了解官方通訊 [此處](https://tecnologia.libero.it/avviato-il-ritorno-online-di-libero-mail-e-virgilio-mail-66832){_blank}。


## 影響{#outage-impact}

如同在大部分的網際網路服務提供者(ISP)中斷時，透過Campaign或Journey Optimizer傳送的某些電子郵件會錯誤標示為退信。 這不僅會影響Adobe，而且在中斷期間，所有嘗試將電子郵件傳送至Italia Online的人。

症狀為：

* **軟跳出數** 包含訊息 `452 requested action aborted: try again later`  — 系統會自動重試，且不需要任何動作。

* **硬跳出數** 包含訊息 `550 <email address> recipient rejected` 已於當地時間1月26日早8點至下午2點被ISP退回，以防止發件人繼續超載其伺服器。 如Italia Online Postmaster所確認，這些並非真正的硬退信，因此我們建議取消隔離因該訊息而於2023年1月26日排除的所有電子郵件地址。

## 更新流程{#outage-update}

### Adobe Campaign{#ac-update}

根據標準退信處理邏輯，Adobe Campaign會使用 **[!UICONTROL Status]** 設定 **[!UICONTROL Quarantine]**. 若要修正此問題，您需要尋找和移除這些收件者，或變更其，以更新Campaign中的隔離表格 **[!UICONTROL Status]** to **[!UICONTROL Valid]** 以便夜間清理工作流程將其移除。

若要尋找受此問題影響的收件者，或若此情況再次發生於其他ISP，請參閱下列指示：

* 如需Campaign Classicv7和Campaign v8，請參閱 [本頁](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-quarantine-management.html?lang=en#unquarantine-bulk){_blank}。
* 若為Campaign Standard，請參閱 [本頁](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/understanding-quarantine-management.html?lang=en#unquarantine-bulk){_blank}。

### Adobe Journey Optimizer{#ajo-update}

根據標準退信處理邏輯，Adobe Journey Optimizer會透過 **[!UICONTROL Reason]** 設定 **[!UICONTROL Invalid Recipient]**. 若要更正此問題，您需要尋找並移除這些電子郵件地址，以更新隱藏清單。

識別後，即可使用 **[!UICONTROL Delete]** 按鈕。 這些位址隨後可納入未來的電子郵件行銷活動中。

深入了解 [本節](https://experienceleague.adobe.com/docs/journey-optimizer/using/configuration/monitor-reputation/manage-suppression-list.html#remove-from-suppression-list){_blank}。

