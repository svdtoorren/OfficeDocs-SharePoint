---
title: "Create new certificates"
ms.reviewer: 
ms.author: v-nsatapathy
author: nimishasatapathy
manager: serdars
ms.date: 06/20/2022
audience: ITPro
f1.keywords:
- NOCSH
ms.topic: article
ms.prod: sharepoint-server-itpro
ms.localizationpriority: medium
ms.collection: IT_Sharepoint_Server_Top
ms.assetid: 88317397-e0cb-47c7-9093-7872bc685213
description: "Learn how you to create new SSL certificate."
---

 
# Create new certificates

[!INCLUDE[appliesto-2013-2016-2019-SUB-xxx-md](../includes/appliesto-2013-2016-2019-SUB-xxx-md.md)]

SharePoint Subscription Edition supports creating SSL certificate requests via the [New-SPCertificate](/powershell/module/sharepoint-server/new-spcertificate) PowerShell cmdlet. This is the first step in a three-step process to install a new SSL certificate.

After the SSL certificate request has been created via the operation, the SharePoint administrator must submit the certificate request to their SSL certificate authority. The SSL certificate authority then generates a signed certificate based on the request and return it to the SharePoint administrator, who can then import the certificate into SharePoint. The imported certificate is paired with the private key generated by the certificate request operation. The certificate is now ready for use by SharePoint.

```powershell
New-SPCertificate -FriendlyName <String> -CommonName <String> [-AlternativeNames <String[]>] [-OrganizationalUnit <String>] [-Organization <String>] [-Locality <String>] [-State <String>] [-Country <String>] [-Exportable] [-HashAlgorithm {Default | SHA256 | SHA384 | SHA512}] [-Path <String>] [-Force] [<CommonParameters>]
New-SPCertificate -FriendlyName <String> -CommonName <String> [-AlternativeNames <String[]>] [-OrganizationalUnit <String>] [-Organization <String>] [-Locality <String>] [-State <String>] [-Country <String>] [-Exportable] [-KeySize {0 | 2048 | 4096 | 8192 | 16384}] [-HashAlgorithm {Default | SHA256 | SHA384 | SHA512}] [-Path <String>] [-Force] [<CommonParameters>]
New-SPCertificate -FriendlyName <String> -CommonName <String> [-AlternativeNames <String[]>] [-OrganizationalUnit <String>] [-Organization <String>] [-Locality <String>] [-State <String>] [-Country <String>] [-Exportable] [-EllipticCurve {Default | nistP256 | nistP384 | nistP521}] [-HashAlgorithm {Default | SHA256 | SHA384 | SHA512}] [-Path <String>] [-Force] [<CommonParameters>]]
```

The cmdlet parameters are:

|Parameter|Description|
|--- |--- |
|FriendlyName| The friendly name for the certificate. This name can be used to help you remember the purpose of this certificate. The friendly name will only be visible to SharePoint farm administrators, not to end users.|
|CommonName | The primary DNS domain name or IP address that this certificate will be assigned to. Fully Qualified Domain Names (FQDNs) are recommended.|
|AlternativeNames | Additional DNS domain names or IP addresses that this certificate will be assigned to. Fully Qualified Domain Names (FQDNs) are recommended.|
|OrganizationalUnit | The name of your department within your organization or company. If this parameter isn't specified, the default organizational unit of the farm will be used.|
|Organization| The legally registered name of your organization or company. If this parameter isn't specified, the default organization of the farm will be used.|
|Locality | The name of the city or locality where your organization is legally located. Don't abbreviate the name. If this parameter isn't specified, the default locality of the farm will be used.|
|State | The name of the state or province where your organization is legally located. Don't abbreviate the name. If this parameter isn't specified, the default state of the farm will be used.|
|Country | The two letter country code where your organization is legally located. This must be an ISO 3166-1 alpha-2 country code. If this parameter isn't specified, the default country of the farm will be used.|
|Exportable| Specifies whether the private key of the certificate may be exported. If this parameter isn't specified, the private key of certificate deployed to the Windows Certificate Store on each server in the SharePoint farm won't be exportable, and SharePoint won't allow you to export the private key from within the SharePoint administration interface.|
|KeySize | Specifies to use the RSA key algorithm for your certificate, and the size of your public and private RSA keys in bits. Larger key sizes provide more cryptographic strength than smaller key sizes, but they're also more computationally expensive and take more time to complete the SSL / TLS connection. Select `2048` if you're unsure, which key size to use. Key sizes larger than `4096` are not recommended. If neither this parameter nor the `EllipticCurve` parameter is specified, the default key algorithm and key size / elliptic curve of the farm will be used.|
|EllipticCurve|Specifies to use the elliptic curve cryptography key algorithm for your certificate, and the elliptic curve of your public and private ECC keys. Larger elliptic curves provide more cryptographic strength than smaller elliptic curves, but they're also more computationally expensive and take more time to complete the SSL / TLS connection. Select `nistP256` if you're unsure, which elliptic curve to use. Elliptic curves larger than `nistP384` are not recommended. If neither this parameter nor the `KeySize` parameter is specified, the default key algorithm and key size / elliptic curve of the farm will be used.|
|HashAlgorithm|Specifies the hash algorithm of your certificate signature, which your certificate authority will use to verify that your certificate request hasn't been tampered with. Larger hash algorithms provide more cryptographic strength than smaller hash algorithms, but they're also more computationally expensive. Select `SHA256` if you're unsure, which hash algorithm to use. Hash algorithms larger than `SHA384` are not recommended. If this parameter isn't specified, the default hash algorithm of the farm will be used.|
|Path|Specifies the path to the certificate signing request file that will be generated.|
|Force| Specifies to overwrite a file if it already exists at the specified path.|
|AssignmentCollection| Manages objects for the purpose of proper disposal. Use of objects, such as SPWeb or SPSite, can use large amounts of memory and use of these objects in Windows PowerShell scripts requires proper memory management. Using the `SPAssignment` object, you can assign objects to a variable and dispose of the objects after they are needed to free up memory. When SPWeb, SPSite, or `SPSiteAdministration` objects are used, the objects are automatically disposed of if an assignment collection or the Global parameter is not used.|
|WhatIf|Shows what would happen if the cmdlet runs. The cmdlet is not run.|
|Confirm|Prompts you for confirmation before running the cmdlet.|

Example cmdlet syntax:

```powershell
New-SPCertificate -FriendlyName "Team Sites Certificate" -KeySize 2048 -CommonName sharepoint.contoso.com -AlternativeNames extranet.contoso.com, onedrive.contoso.com -OrganizationalUnit "Contoso IT Department" -Organization "Contoso" -Locality "Redmond" -State "Washington" -Country "US" -Exportable -HashAlgorithm SHA256 -Path "\\server\fileshare\Team Sites Certificate Signing Request.txt"
```