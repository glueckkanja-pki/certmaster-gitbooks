---
category: Troubleshooting
title: Troubleshooting
order: 1
---

# Policy Module

## Error on starting AD Certification Services

If you see the following error when trying to start AD CS after installing the policy module:

```text
 The Active Directory Certificate Services service could not be started.
 A system error has occurred.
 System error 2 has occurred.
 The system cannot find the file specified.
```

Then you may have missed to copy the registry keys from 

`HKLM:\SYSTEM\CurrentControlSet\Services\CertSvc\Configuration\[NAME OF YOUR CA]\PolicyModules\CertificateAuthority_MicrosoftDefault.Policy` 

to `HKLM:\SYSTEM\CurrentControlSet\Services\CertSvc\Configuration\[NAME OF YOUR CA]\PolicyModules\CertificateAuthority_DotNetDecorator.Policy`

Please see the [installation guide](https://github.com/glueckkanja-pki/certmaster-gitbooks/tree/43c614d05e1dc450020bcaaf35bffddbb53a56dc/1-general/installation/README.md#PolicyModule) for more details.

