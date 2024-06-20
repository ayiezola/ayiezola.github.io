---
layout: post
title: Malicious Attacker use MAJELIS_JEMPUTAN.apk to Scam 
subtitle: Targeted to Malaysian
tags: [malicious apk]
comments: false
author: Ayiezola
---

# Introduction

Last year, we are seldomly heard about scammer actively attack Malaysian with Telegram Take Over. Base on the previous attack flow, the perpetrator use phishing link to lure victim to enter their telegram credentials then after few second, perpetrator will trying to take over victim telegram account with the information they have. Nowdays, the threat actor has changed their method by using android Android Package Kit (APK) with the same objective.

# Technical Summary

In order to spy sms, phone call log, grabbing victims contact, threat actor has to plant their malicious APK in victim's device. After successfully installed, APK will establish a process called 'Surat Jemputan' will keep running and monitor any SMS, call log received by victim and forward to Commmand Control (C2) for the objective to take over telegram account nor whatsapp account also victim contact. This objective will only achieve if victim successfully installed their malicious APK. 

# How threat actors distributed the malware

<img src="https://github.com/ayiezola/ayiezola.github.io/blob/master/assets/scammer/app-scam-spread01.png" width="300" height="600" ><img src="https://github.com/ayiezola/ayiezola.github.io/blob/master/assets/scammer/app-scam-spread02.png" width="300" height="600" >

We can see that the chain of this attack is threat actor using Social Engineering (SE) technic to spread the Malicous APK. Threat Actor or Scammer will be spread their malicious APK through victim's contact base on previous compromised. Few victims have received a message contains "Jemputan Majlis Perkahwinan" attached with APK named MAJELIS_JEMPUTAN.apk. The contains of messages also told victim to install the APK to see the event details. At this point, victim believe the invitation is valid because it come from their contact and keen to know the important date, details of the event.

# Graph Flow



## Technical Analysis

### APK metadata information

Application name: SURAT JEMPUTAN

Package Name: com.scs.whatsbulk

MD5: e93420ff1fcd35e17b2dc7a4dc7667c3

SHA1: 1f43eccbaa3abe21641185e025deed604a525aec

SHA256: abb0b78b36044643e3a792e6db670d2fa7e31e3fa940eae518018c09e15a00f1

### Application Behavior

<img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-asktoinstall.png" width="300" height="600" >

## Malicious capabilities



### Get device information

### SMS stealer

### Observation

# Recommendation

## Prevention

## Indicator of Compromises

### MD5 Hash
