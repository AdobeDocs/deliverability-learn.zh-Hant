---
title: Verizon Media Group（Yahoo、AOL、Verizon 等）
description: 在大多數B2C清單中，[!DNL Verizon Media Group]通常是前三個網域之一。 他們的行為有些獨特，因為如果出現信譽問題，他們通常會限制或大量郵件。
topics: Deliverability
jira: KT-5320
doc-type: article
activity: understand
role: Admin, Leader, User
level: Beginner
team: TM
exl-id: 43e6d3cb-23c3-4076-8026-a1a08e76bd1b
TQID: https://experienceleague.adobe.com/ycELLXdqC1E3EIxywp-I1HWnSyWayRHcwkNOzmzSBvY
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
  - id: d0a3eab4-7b10-4d96-a71e-6c0f8e7b7c87
  - id: dfc56824-e8b9-499e-85d4-21aedb507314
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: f8a45b24-4be7-4f1b-909b-60d06b483a20
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2:
  - id: e1e0219c-f879-479f-8427-888ed2a6e9c2
source-git-commit: 75df8537199680e5f1fc4b98cefdb05220fee7bf
workflow-type: tm+mt
source-wordcount: 289
ht-degree: 2%

---

# [!DNL Verizon Media Group] （Yahoo、AOL、Verizon等）

在大多數B2C清單中，[!DNL Verizon Media Group]通常是前三個網域之一。 他們的行為有些獨特，因為如果出現信譽問題，他們通常會限制或大量郵件。

以下是一些重點事項：

## 哪些資料重要

[!DNL Verizon Media Group] (VMG)已使用內容和URL篩選以及垃圾郵件投訴的組合，建置和維護他們自己的專屬垃圾郵件篩選器。 除了Gmail，他們是早期採用ISP之一，可依網域及IP位址篩選電子郵件。

## 他們提供哪些資料

VMG有一個FBL，用於將投訴資訊傳回給寄件者。 他們也會探索在未來新增更多資料。

## 寄件者信譽

寄件者的信譽是由IP位址、網域和寄件者位址的組合所組成。 信譽是使用傳統元件計算的，包括投訴、垃圾郵件陷阱、非作用中或格式錯誤的地址和參與。 VMG使用速率限制（也稱為節流）以及大量資料夾來抵禦垃圾郵件。 他們以大約[!DNL Spamhaus]個黑名單（包括PBL、SBL和XBL）補充其內部篩選系統，以保護其使用者。

## Insights

VMG最近對舊的、非作用中的電子郵件地址有定期的維護期間。 這表示經常會觀察到無效地址跳出大幅增加，這可能會在短時間內影響您的傳送率。 這類原則也敏感地面對來自寄件者的高無效地址跳出率，這表示需要收緊贏取或參與政策。 傳送者通常會在大約1%的無效地址遇到負面影響。
