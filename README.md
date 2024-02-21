![](./.github/banner.png)

---

<p align="center">
  This repository contains a list of many methods to coerce a windows machine to authenticate to an attacker-controlled machine.
  <br>
  <img alt="GitHub repo size" src="https://img.shields.io/badge/coerce%20methods-15-brightgreen">
  <a href="https://twitter.com/intent/follow?screen_name=podalirius_" title="Follow"><img src="https://img.shields.io/twitter/follow/podalirius_?label=Podalirius&style=social"></a>
  <a href="https://www.youtube.com/c/Podalirius_?sub_confirmation=1" title="Subscribe"><img alt="YouTube Channel Subscribers" src="https://img.shields.io/youtube/channel/subscribers/UCF_x5O7CSfr82AfNVTKOv_A?style=social"></a>
  <br>
  <br>
</p>

All of these methods are callable by a standard user in the domain to force the machine account of the target Windows machine (usually a domain controller) to authenticate to an arbitrary target. The root cause of this "vulnerability/feature" in each of these methods is that Windows machines automatically authenticate to other machines when trying to access UNC paths (like `\\192.168.2.1\SYSVOL\file.txt`).

There are currently **15** working functions in **5** protocols.

---

> [!NOTE]
> 🎉 A lot of new methods are yet to be tested, if you want to try them: [possible-working-calls](./possible-working-calls/)
> This list will be triaged over time, eventhough I automated most of the work and autogenerated python proof of concept for each call, it takes time to triage these 240+ RPC calls.

---

## Protocols & Methods

+ **[MS-DFSNM]: Distributed File System (DFS): Namespace Management Protocol**
  + [Remote call to NetrDfsAddStdRoot (opnum 12)](./methods/MS-DFSNM%20-%20Distributed%20File%20System%20%28DFS%29%20Namespace%20Management%20Protocol/12.%20Remote%20call%20to%20NetrDfsAddStdRoot%20(opnum%2012)/README.md)
  + [Remote call to NetrDfsRemoveStdRoot (opnum 13)](./methods/MS-DFSNM%20-%20Distributed%20File%20System%20%28DFS%29%20Namespace%20Management%20Protocol/13.%20Remote%20call%20to%20NetrDfsRemoveStdRoot%20(opnum%2013)/README.md)


+ **[MS-EFSR]: Encrypting File System Remote (EFSRPC) Protocol**
  + [Remote call to EfsRpcOpenFileRaw (opnum 0)](./methods/MS-EFSR%20-%20Encrypting%20File%20System%20Remote%20%28EFSRPC%29%20Protocol/00.%20Remote%20call%20to%20EfsRpcOpenFileRaw%20(opnum%200)/README.md)
  + [Remote call to EfsRpcEncryptFileSrv (opnum 4)](./methods/MS-EFSR%20-%20Encrypting%20File%20System%20Remote%20%28EFSRPC%29%20Protocol/04.%20Remote%20call%20to%20EfsRpcEncryptFileSrv%20(opnum%204)/README.md)
  + [Remote call to EfsRpcDecryptFileSrv (opnum 5)](./methods/MS-EFSR%20-%20Encrypting%20File%20System%20Remote%20%28EFSRPC%29%20Protocol/05.%20Remote%20call%20to%20EfsRpcDecryptFileSrv%20(opnum%205)/README.md)
  + [Remote call to EfsRpcQueryUsersOnFile (opnum 6)](./methods/MS-EFSR%20-%20Encrypting%20File%20System%20Remote%20%28EFSRPC%29%20Protocol/06.%20Remote%20call%20to%20EfsRpcQueryUsersOnFile%20(opnum%206)/README.md)
  + [Remote call to EfsRpcQueryRecoveryAgents (opnum 7)](./methods/MS-EFSR%20-%20Encrypting%20File%20System%20Remote%20%28EFSRPC%29%20Protocol/07.%20Remote%20call%20to%20EfsRpcQueryRecoveryAgents%20(opnum%207)/README.md)
  + [Remote call to EfsRpcFileKeyInfo (opnum 12)](./methods/MS-EFSR%20-%20Encrypting%20File%20System%20Remote%20%28EFSRPC%29%20Protocol/12.%20Remote%20call%20to%20EfsRpcFileKeyInfo%20(opnum%2012)/README.md)
  + [Remote call to EfsRpcDuplicateEncryptionInfoFile (opnum 13)](./methods/MS-EFSR%20-%20Encrypting%20File%20System%20Remote%20%28EFSRPC%29%20Protocol/13.%20Remote%20call%20to%20EfsRpcDuplicateEncryptionInfoFile%20(opnum%2013)/README.md)
  + [Remote call to EfsRpcAddUsersToFileEx (opnum 15)](./methods/MS-EFSR%20-%20Encrypting%20File%20System%20Remote%20%28EFSRPC%29%20Protocol/15.%20Remote%20call%20to%20EfsRpcAddUsersToFileEx%20(opnum%2015)/README.md)


+ **[MS-FSRVP]: File Server Remote VSS Protocol**
  + [Remote call to IsPathSupported (opnum 8)](./methods/MS-FSRVP%20-%20File%20Server%20Remote%20VSS%20Protocol/08.%20Remote%20call%20to%20IsPathSupported%20(opnum%208)/README.md)
  + [Remote call to IsPathShadowCopied (opnum 9)](./methods/MS-FSRVP%20-%20File%20Server%20Remote%20VSS%20Protocol/09.%20Remote%20call%20to%20IsPathShadowCopied%20(opnum%209)/README.md) 

+ **[MS-PAR]: Print System Asynchronous Remote Protocol** 
  + [Remote call to RpcAsyncOpenPrinter (opnum 0)](./methods/MS-PAR%20-%20Print%20System%20Asynchronous%20Remote%20Protocol/00.%20Remote%20call%20to%20RpcAsyncOpenPrinter%20(opnum%200)/README.md)


+ **[MS-RPRN]: Print System Remote Protocol** 
  + [Remote call to RpcRemoteFindFirstPrinterChangeNotificationEx (opnum 65)](./methods/MS-RPRN%20-%20Print%20System%20Remote%20Protocol/65.%20Remote%20call%20to%20RpcRemoteFindFirstPrinterChangeNotificationEx%20(opnum%2065)/README.md)

## Protecting against coerced authentications

Microsoft does not consider coerced authentications as security vulnerability. In their point of view, only the relaying of authentications issued from coerced authentication consitute a security vulnerability. To prevent NTLM Relay Attacks on networks with NTLM enabled, domain administrators must ensure that services that permit NTLM authentication make use of protections such as [Extended Protection for Authentication (EPA)](https://msrc-blog.microsoft.com/2009/12/08/extended-protection-for-authentication/) or signing features such as SMB signing. The mitigations against NTLM relays are outlined in these bulletins:

+ **July 24, 2021** [KB5005413: Mitigating NTLM Relay Attacks on Active Directory Certificate Services (AD CS)](https://support.microsoft.com/en-us/topic/kb5005413-mitigating-ntlm-relay-attacks-on-active-directory-certificate-services-ad-cs-3612b773-4043-4aa9-b23d-b87910cd3429)
+ **December 08, 2009** [MSA974926: Credential Relaying Attacks on Integrated Windows Authentication](https://docs.microsoft.com/en-us/security-updates/SecurityAdvisories/2009/974926)
+ **August 12, 2009** [Extended Protection for Authentication](https://msrc-blog.microsoft.com/2009/12/08/extended-protection-for-authentication/)
+ **August 11, 2009** [MSA973811: Extended Protection for Authentication](https://docs.microsoft.com/en-us/security-updates/securityadvisories/2009/973811)

## Contributing

Feel free to open a pull request to add new methods.
