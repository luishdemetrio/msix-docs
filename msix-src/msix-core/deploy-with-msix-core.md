---
title: Deploy an MSIX package with MSIX Core
description: This article provides a step by step instruction on how to leverage the MSIX Core bootstrapper, which creates an application using ClickOnce that will allow your users to just download a setup.exe and install their MSIX app through the MSIX Core Installer.
ms.date: 11/15/2019
ms.topic: article
keywords: windows 10, windows 7, windows 8, Windows Server, uwp, msix, msixcore, 1709, 1703, 1607, 1511, 1507
ms.localizationpriority: medium
ms.custom: "RS5, seodec18"
---

# Deploy an MSIX package with MSIX Core
As mentioned in the overview, MSIX Core brings MSIX deployment support to Windows 7 SP1, Windows 8.1, currently supported Windows Server (with desktop experience), and Windows 10 versions prior to 1709 (Fall Anniversary Update). In order to get started, you must ensure that MSIX Core is installed on the OS.

For your convenience, we provide MSI installers. To locate the installer of your choice, go to our [release page](https://github.com/microsoft/msix-packaging/releases) and under **Assets** you will find the following:

1. msixmgrSetup-x64.msi
2. msixmgrSetup-86.msi

## MSI installation 
We recommend the MSI installation because it automatically adds msixmgr.exe to your search path and associates the MSIX extension with the installer.

Download either **msixmgrSetup-x64.msi** or **msixmgrSetup-x86.msi** (depending on your architecture) and install it on your Windows Device. 

> [!NOTE]
> It is important that you pick the correct installer for your architecture. This will impact where the installer will store important files. The name of the file may change based on the version of the installer. 

## Installing your certificate
MSIX packages are required to be signed. Before installing any .msix packages, make sure you have installed the certificate you used to sign your packages. You can do this using you normal workflows for installing certificate from your management tool. 

If you want to manually install a certificate you can run this command from an elevated command prompt: 
```
certutil -addstore root <insert certificate.cert>
```
> [!NOTE]
> You should add your trusted certificate under Trusted Root Certification Authority in all scenarios.

## Using the Command Line
Once the tool msixmgr.exe is installed, it can be used to manage your MSIX packages on this machine by searching, installing, and removing. The command line utility msixmgr.exe is intended for system administrators. It is most useful when run from administrative prompt. Not all commands when run from a regular command prompt will display to the console. See below for more details.

### Install
Using command prompt or PowerShell, navigate to the directory that contains the executables and run the following command to install notepadplus.msix. The -quietUX parameter can also be added at the end of the command so that users don't see the installer UI. For example: 
```
msixmgr.exe -AddPackage C:\SomeDirectory\notepadplus.msix -quietUX
```
### Querying for a specific MSIX Package
Searching for a specific package is possible by packageFullName, packageFamilyName and/or using wildcards as well. Supported wildcards are *(match any character) and ?(match single character). 
```
msixmgr.exe -FindPackage notepadplus_0.0.0.1_???__8wekyb3d8bbwe
msixmgr.exe -FindPackage *padplus_0.0.*
msixmgr.exe -FindPackage *epadplus_8wekyb3d8bbw?
```
### Uninstall
To uninstall use the following command: 
```
msixmgr.exe -RemovePackage notepadplus_0.0.0.1_x64__8wekyb3d8bbwe -quietUX
```
> [!NOTE]
> The commands above uses **notepadplus.msix** is one of our [sample packages](https://github.com/microsoft/msix-packaging/tree/master/MsixCore/Tests).