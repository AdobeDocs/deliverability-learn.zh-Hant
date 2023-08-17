---
title: Verizon Media Group（Yahoo、AOL、Verizon 等）
description: '"[!DNL Verizon Media Group] 通常是大多數B2C清單的前三個網域之一。 他們的行為有些獨特，因為如果出現信譽問題，他們通常會限制或大量郵件。」'
topics: Deliverability
jira: KT-5320
doc-type: article
activity: understand
role: Admin, Leader, User
level: Beginner
team: TM
exl-id: 43e6d3cb-23c3-4076-8026-a1a08e76bd1b
source-git-commit: 6b312cdbba496818337c97ec4f42962aea757901
workflow-type: tm+mt
source-wordcount: '290'
ht-degree: 2%

---

# [!DNL Verizon Media Group] （Yahoo、AOL、Verizon等）

[!DNL Verizon Media Group] 通常是大多數B2C清單的前三個網域之一。 他們的行為有些獨特，因為如果出現信譽問題，他們通常會限制或大量郵件。

以下是一些重點事項：

## 哪些資料重要

[!DNL Verizon Media Group] (VMG)已使用內容和URL篩選以及垃圾郵件投訴的組合，建置和維護他們自己的專屬垃圾郵件篩選器。 除了Gmail，他們是早期採用ISP之一，可依網域及IP位址篩選電子郵件。

## 他們提供哪些資料

VMG有一個FBL，用於將投訴資訊傳回給寄件者。 他們也會探索在未來新增更多資料。

## 寄件者信譽

寄件者的信譽是由IP位址、網域和寄件者位址的組合所組成。 信譽是使用傳統元件計算的，包括投訴、垃圾郵件陷阱、非作用中或格式錯誤的地址和參與。 VMG使用速率限制（也稱為節流）以及大量資料夾來抵禦垃圾郵件。 他們以一些方式補充其內部篩選系統 [!DNL Spamhaus] 黑名單，包括PBL、SBL和XBL以保護其使用者。

## Insights

VMG最近對舊的、非作用中的電子郵件地址有定期的維護期間。 這表示經常會觀察到無效地址跳出大幅增加，這可能會在短時間內影響您的傳送率。 這類原則也敏感地面對來自寄件者的高無效地址跳出率，這表示需要收緊贏取或參與政策。 傳送者通常會在大約1%的無效地址遇到負面影響。
