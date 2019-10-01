---
category: General
title: Installation
order: 2
---

# installation

Glück & Kanja provides consulting and support for the installation of the Certificate Master components.

The web site runs on a Windows Server 2012 R2 or newer with IIS installed. The Windows service typically runs on the same machine and is part of the same installation package.

## Web Site

Secure this web site with a manually requested TLS certificate. Disable Anonymous access and switch to Integrated Authentication.

## Windows Service

The executable installs itself with the command `CertificateMaster.Service.exe install`.

## [Policy Module](installation.md)

Install the Visual C++ Redistributable Runtime for Visual Studio 2017 \(64 bit\) as a prerequisite.

Copy the DLLs into `C:\Windows\System32\`, so the system can find them. Register the COM DLL poldotnet.dll with the command `regsvr32 poldotnet.dll`.

Copy the default policy module's configuration in the registry to keep your settings for processing templates \(this is recommended\). To do so, navigate to `HKLM:\SYSTEM\CurrentControlSet\Services\CertSvc\Configuration\[NAME OF YOUR CA]\PolicyModules`, for example in Powershell or with regedit. There will be a registry key `CertificateAuthority_MicrosoftDefault.Policy` used by the default policy module. The default policy module will still be loaded in the backend, but accesses the key `CertificateAuthority_DotNetDecorator.Policy` instead. Thus, copy the first key to the latter.

