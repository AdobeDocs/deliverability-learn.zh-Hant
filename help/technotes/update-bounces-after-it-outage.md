---
title: 在Italia Online中斷後更新退回資格
description: 瞭解如何在Italia線上中斷後更新跳出資格
feature: Deliverability
exl-id: a11e88cf-bf37-42cc-9c09-1d58360459b7
hide: true
hidefromtoc: true
role: Admin
level: Beginner
source-git-commit: 6b312cdbba496818337c97ec4f42962aea757901
workflow-type: tm+mt
source-wordcount: '395'
ht-degree: 3%

---

# 在 Italia 線上中斷後更新不正確的永久性硬退件 {#update-bounce-italia}

## 內容{#outage-context}

自1月22日（當地時間）起，Italia Online已發生中斷，導致數次延遲並拒絕電子郵件。 服務於1月26日以有限容量開始恢復。

受影響的網域為： **libero.it**、**virgilio.it**、**inwind.it**、**iol.it**&#x200B;和&#x200B;**blu.it**。

此問題發生在2023年1月22日到2023年1月26日，但大多數錯誤隔離發生在1月26日。

在[這裡](https://tecnologia.libero.it/avviato-il-ritorno-online-di-libero-mail-e-virgilio-mail-66832){_blank}的官方通訊中瞭解更多資訊。


## 影響{#outage-impact}

如同在網際網路服務提供者(ISP)發生中斷的大多數情況一樣，透過Campaign或Journey Optimizer傳送的某些電子郵件會錯誤標示為跳出。 這不僅會影響Adobe，也會影響所有嘗試在中斷期間將電子郵件傳送到Italia Online的客戶。

症狀如下：

* **軟退信**，訊息為`452 requested action aborted: try again later` — 這些退信已自動重試，不需要任何動作。

* ISP已於1月26日當地時間上午8點至下午2點之間傳回包含訊息`550 <email address> recipient rejected`的&#x200B;**硬跳出**，以防止寄件者持續讓伺服器超載。 如義大利線上郵局主管所確認，這些並非真正的硬跳出，因此我們建議取消隔離2023年1月26日因該訊息而被排除的所有電子郵件地址。

## 更新流程{#outage-update}

### Adobe Campaign{#ac-update}

根據標準退信處理邏輯，Adobe Campaign會自動將這些收件者新增至隔離清單，**[!UICONTROL Status]**&#x200B;設定為&#x200B;**[!UICONTROL Quarantine]**。 若要修正此問題，您需要透過尋找並移除這些收件者，或將其&#x200B;**[!UICONTROL Status]**&#x200B;變更為&#x200B;**[!UICONTROL Valid]**，以在Campaign中更新隔離表格，讓夜間清理工作流程將移除這些收件者。

若要尋找受此問題影響的收件者，或當此問題再次發生在任何其他ISP身上時，請參閱下列指示：

* 若為Campaign Classicv7和Campaign v8，請參閱[此頁面](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-quarantine-management.html?lang=en#unquarantine-bulk){_blank}。
* 如需Campaign Standard，請參閱[此頁面](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/understanding-quarantine-management.html?lang=en#unquarantine-bulk){_blank}。

### Adobe Journey Optimizer{#ajo-update}

根據標準退信處理邏輯，Adobe Journey Optimizer會自動將這些電子郵件地址新增到隱藏清單中，**[!UICONTROL Reason]**&#x200B;設定為&#x200B;**[!UICONTROL Invalid Recipient]**。 若要修正此問題，您需要透過尋找並移除這些電子郵件地址來更新隱藏清單。

識別之後，可以使用&#x200B;**[!UICONTROL Delete]**&#x200B;按鈕從隱藏清單中手動移除這些地址。 這些地址隨後可包含在未來的電子郵件行銷活動中。

在[本節](https://experienceleague.adobe.com/docs/journey-optimizer/using/configuration/monitor-reputation/manage-suppression-list.html#remove-from-suppression-list){_blank}中瞭解更多資訊。

