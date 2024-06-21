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

<img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-spread01.png" width="300" height="600" /> <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-spread02.png" width="300" height="600" />

We can see that the chain of this attack is threat actor using Social Engineering (SE) technic to spread the Malicous APK. Threat Actor or Scammer will be spread their malicious APK through victim's contact base on previous compromised. Few victims have received a message contains "Jemputan Majlis Perkahwinan" attached with APK named MAJELIS_JEMPUTAN.apk. The contains of messages also told victim to install the APK to see the event details. At this point, victim believe the invitation is valid because it come from their contact and keen to know the important date, details of the event.

# Graph Flow

Tunggu Sat!!

## Technical Analysis

### APK metadata information

Application name: SURAT JEMPUTAN

Package Name: com.scs.whatsbulk

MD5: e93420ff1fcd35e17b2dc7a4dc7667c3

SHA1: 1f43eccbaa3abe21641185e025deed604a525aec

SHA256: abb0b78b36044643e3a792e6db670d2fa7e31e3fa940eae518018c09e15a00f1

### Telegram API 

<img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-telebot-api.png" width="" height="" >

<img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-telegram-bot-format.png" width="" height="" >

### Application Behavior

### Permision Abuse

<img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-abuse-permision.png" width="" height="" >

From the picture above, we can see this APK file has five dangerous malware permision that are required to make it run perfectly. One of the interesting part is permision.WAKE_LOCK and permision.RECEIVE_BOOT_COMPLETED. This permision will allow the malicious APK to execute command before device sleep. Another one is permision to execute command after device completely boot.

### Manifest File

<img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-manifest.png" width="" height="" >

AndroidManifest file is a root element for android application. If we look at this picture above, it give us initial information about the application permision, version, package name. For this malicious APK, we have 10 abnormal permision request while perform installing process.

### Application Package

<img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-package.png" width="" height="" >

Whatsbulk is package name for this malicious APK. In this picture above, we can see there is 10 java file under Whatsbulk Folder. Of course we will explore the behaviour of the java code.

## Malicious capabilities

### Get device information

<img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-getdevice-info.png" width="" height="" >

Look at picture above, at the first code block, this application attempts to request the permision declare in AndroidManifest file.

<img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-check-phone.png" width="" height="" >

This code will manage SMS content string or SMS format about device specific information in order to send message to attacker C2 by using telegram API.

### Get Battery Status, SIM Information & IMSI

<img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-getbattery-status.png" width="" height="" >

At this point, we can see from the code above the application will gather information about the device battery status if it has permision READ_PHONE_STATE. It also check for SIM slot value and extract phone number. All this information will be send to attacker C2 as target information collection similar to log.

### Monitor SMS

<img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-monitor-sms.png" width="" height="" >

### Monitor Call

<img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-monitor-call.png" width="" height="" >

### Anti VM

<img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-anti-vm.png" width="" height="" >

### Notify Attacker

<img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-notify-infection.png" width="" height="" >

<img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-send-phone-number.png" width="" height="" >

### Observation

## Severity

<img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-severity.png" width="" height="" >

<img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-request-permision.png" width="" height="" >

Picture above show us about manifest analysis. This result gto from recompiled APK that didnt has any protection.

## APK Sign Information

<img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-signed.png" width="" height="" >

## CWE Information

<img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-cwe.png" width="" height="" >

## Developer Language

<img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-indo-slang.png" width="" height="" >

## Dry Run

### Installation

<img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-google-protect.png" width="300" height="600" >

During installation process, we observe that Google Play Protect has notify that APK will harmful victim's device. To continue this analysis, we just click 'Got it'.

<img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-asktoinstall1.png" width="300" height="600" >

Once again, a windows pop-up asking whether to proceed the installation or not. We click 'Install'

<img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-access-call-log.jpeg" width="300" height="600" > <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-access-contact.jpeg" width="300" height="600" >

<img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-phone-call.jpeg" width="300" height="600" > <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-read-sms.jpeg" width="300" height="600" >

<img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-installed.png" width="300" height="600" > <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-app-approach.png" width="300" height="600" >

### Enter Phone Number

<img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-enter-phone-no.png" width="300" height="600" >

### Received Notification

<img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-replicate-01.png" width="" height="" >

<img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-monitor-call-notify.png" width="" height="" >

### Received OTP

<img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-02.png" width="300" height="600" >

<p align="center"><img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-03.jpeg" width="250" height="250" ></p>

# Recommendation

## Prevention

## Indicator of Compromises

### MD5 Hash

<img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-hardcoded.png" width="" height="" >

<img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-sendata.png" width="" height="" >

<img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-request-access-while-install.png" width="" height="" >
