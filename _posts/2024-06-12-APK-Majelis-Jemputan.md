---
layout: post
title: Scammer use MAJELIS_JEMPUTAN.apk to generate money.
subtitle: Analysis & Explaination
tags: [malicious apk]
comments: false
author: Ayiezola
---

# Introduction

Last year, we are seldomly heard about scammer actively attack Malaysian with Telegram Take Over. Base on the previous attack flow, the perpetrator use phishing link to lure victim to enter their telegram credentials then after few second, perpetrator will trying to take over victim telegram account with the information they have. Nowdays, the threat actor has changed their method by using android Android Package Kit (APK) with the same objective. This insident has been report by Malaysia news portal [Harian Metro](https://www.hmetro.com.my/mutakhir/2024/06/1096977/waspada-mesej-jemputan-kahwin-digital-guna-fail-apk).

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

This code above is about to monitor incoming SMS received by victims. We know that scammer are really waiting for the OTP code to proceed further malicious action. The first code under function SMSMonitor is about the extract SMS Data messages then it will initializes an array SmsMessage Objects.

Second code block will combines all the string to form a complete text messages.

Third code block will retrieve sender phone number also subscription ID from extras. Then last code block will prepare input data and request network connectivity status CONNECTED.

### Monitor Call

<img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-monitor-call.png" width="" height="" >

This code will monitor for caller phone number also check if call is not ringing or caller phone number is NULL, the it will do nothing. It also check for Subscription ID from which SIM slot. Then all the information will be enqueuing to be send to attacker C2.

### Successfully Install & Notify Attacker

<img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-notify-infection.png" width="" height="" >

Code above will check if the next button is clicked, then update MainActivity with victim phone number. Call SendIntro to execute notification to attacker. Start MainActivity then mark first setup has complete.

<img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-firstsetup-notify.png" width="" height="" >

Function above will notify attacker after first setup has been successfully install in victim device. Attacker will be notify right after this process has been done using telegram API.

### Anti VM

<img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-anti-vm.png" width="" height="" >

From this picture above, our analysis tool has been detected this APK are compiled by dexlib 2.x

> If any of the dexlib families have been used to create a DEX file, you can be fairly suspicious it has been cracked and it may have been injected with malware. For more info on how we used compiler fingerprinting to detect malware and cracks, check out our talk Android Compiler Fingerprinting. [Source](https://rednaga.io/2016/07/31/detecting_pirated_and_malicious_android_apps_with_apkid/)

### Observation

## Permision Severity

<img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-request-permision.png" width="" height="" >

Picture above show us about manifest analysis. This result got from recompiled APK that didnt has any protection.

<img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-severity.png" width="" height="" >

## APK Sign Information

<img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-signed.png" width="" height="" >

## CWE Information

<img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-cwe.png" width="" height="" >

There is java file named SendIntro.java has been store sensitive information. That is attacker Telegram Bot API. 

<img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-chatid-api.png" width="" height="" >

## Developer Language

<img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-indo-slang.png" width="" height="" >

The text in the message string is in Indonesian Slang, so we can assume this APK is come from the same circle with the previous attack like [MyMaid APK](https://notes.netbytesec.com/2022/05/scam-and-malicious-apk-targeting.html), [Kad Kahwin Digital](https://notes.netbytesec.com/2023/06/kahwin-sms-stealer-target-Malaysia.html) also [MyPetronas](https://notes.netbytesec.com/2022/09/scam-android-app-steals-bank.html). NetByteSec Team has done their analysis with these APK. You can check this later for more information.

## Dry Run

To achieve a clear picture of this attack flow, we have to run this APK in our Lab environment. 

### Installation

<p align="center"><img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-google-protect.png" width="300" height="600" ></p>p

During installation process, we observe that Google Play Protect has notify that APK will harmful victim's device. To continue this analysis, we just click 'Got it'.

<p align="center"><img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-asktoinstall1.png" width="300" height="600" ></p>p

Once again, a windows pop-up asking whether to proceed the installation or not. We click 'Install'

<p align="center"><img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-access-call-log.jpeg" width="300" height="600" > <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-access-contact.jpeg" width="300" height="600" ></p>

<p align="center"><img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-phone-call.jpeg" width="300" height="600" > <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-read-sms.jpeg" width="300" height="600" ></p>

<p align="center"><img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-installed.png" width="300" height="600" > <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-app-approach.png" width="300" height="600" ></p>

During installation process, we notices this application will pop-up a windows for each permision request. Phone call logs, access contacts, manage phone calls and permision to view and send messages. It's crucial to be aware about all of these permisions request from any APK we are trying to install. This is the top malware permision that can compromise our security and privacy.

### Enter Phone Number

<p align="center"><img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-enter-phone-no.png" width="300" height="600" ></p>p

Once successfully install, a window form pop-up and we have to enter our phone number. We decide to use temporary phone number just to understand how this attack flow going.

### Received OTP

<p align="center"><img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-02.png" width="300" height="600" ></p>p

<p align="center"><img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-03.jpeg" width="250" height="250" ></p>

A few minute later, we observe that our testing phone has receive OTP code for whatsapp registration. A windows message has prompt saying that whatsapp registration code has been request. We also received a SMS with OTP code for WhatsApp registration on a new device.

<p align="center"><img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-shopeepay.jpeg" width="250" height="250" ></p>

We also received OTP code for shopeepay registration. Both of this messages are clearly using Indonesian slang. 

Base on this analysis, we decided to replicate this APK and setup our own C2 using telegram Bot API. To make it work, we have to setup new telegram Bot, telegram API key and Telegram Chat ID. After everything look fine and ready, we start to install that APK into a testing phone. We manage to send SMS and make a call.

### Received Notification

<p align="center"><img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-replicate-01.png" width="" height="" ></p>

<p align="center"><img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-monitor-call-notify.png" width="" height="" ></p>

Looking at picture above, our C2 have receive notifications containing information as discussed at the beginning of this analysis like device model, Telco, caller phone number, text message, SMS sender phoner number and battery status.

## Recommendation & Prevention

This attack are active from previous year and it become wellknown. We recommend all family members that received this awareness need to spread to their parents. Threat actor will be everywhere and everyone can be their victim. The important thing is we have to know what we download and install in our device. Please download application that come from google play or official/trusted website. Kindly update application to the latest version. 

If you have installed this malicious APK, please diconnect your network and uninstall it. Check which and what service are running on your device. Also check application properties. 

## Indicator of Compromises

Domain Contact: api.telegram.org

### MD5 Hash

<p align="center"><img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/scammer/app-scam-apk-md5.png" width="" height="" ></p>


