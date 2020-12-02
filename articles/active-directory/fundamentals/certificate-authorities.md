---
title: Entidades de certificados de Azure Active Directory
description: Lista de certificados de confianza utilizados en Azure
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 10/10/2020
ms.author: baselden
ms.reviewer: baselden
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: f88091fdf1bd39961ccf7a542aab3e7d2c3646e2
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2020
ms.locfileid: "96172676"
---
# <a name="certificate-authorities-used-by-azure-active-directory"></a>Entidades de certificación utilizadas por Azure Active Directory

> [!IMPORTANT]
> La información de esta página solo es pertinente para las entidades que especifican explícitamente una lista de entidades de certificación (CA) aceptables. Esta práctica, conocida como asignación de certificados, debe evitarse a menos que no haya otras opciones.

Cualquier entidad que intente acceder a los servicios de identidad de Azure Active Directory (Azure AD) mediante los protocolos TLS/SSL se presentarán con los certificados de las entidades de certificación que se enumeran a continuación. Si la entidad confía en esas entidades de certificación, puede usar los certificados para comprobar la identidad y la legitimidad de los servicios de identidad y establecer conexiones seguras.

Las entidades de certificación se pueden clasificar en entidades de certificación raíz y entidades de certificación intermedias. Normalmente, las entidades de certificación raíz tienen una o varias entidades de certificación intermedias asociadas. En este artículo se enumeran las entidades de certificación raíz que los servicios de identidad de Azure AD usan y las entidades de certificación intermedias asociadas a cada una de esas raíces. Para cada entidad de certificación, se incluyen los identificadores uniformes de recursos (URI) para descargar el acceso a la información de entidad de certificación (AIA) asociada y los archivos del punto de distribución de la lista de revocación de certificados (CDP). Cuando sea necesario, también proporcionamos un URI al punto de conexión del protocolo de estado de certificados en línea (OCSP).

## <a name="cas-used-in-azure-public-and-azure-us-government-clouds"></a>Entidades de certificación usadas en las nubes públicas de Azure y de Azure Government US

Los diferentes servicios pueden utilizar distintas entidades de certificación raíz o intermedias. Por lo tanto, es posible que se necesiten todas las entradas que se incluyen a continuación.

### <a name="digicert-global-root-g2"></a>DigiCert Global Root G2


| Entidad de certificación raíz| Número de serie| Fecha de emisión y fecha de expiración| Huella digital SHA1| URI |
| - |- |-|-|-|-|
| DigiCert Global Root G2| 033af1e6a711a 9a0bb2864b11d09fae5| 1 de agosto de 2013 <br>15 de enero de 2038| df3c24f9bfd666761b268 073fe06d1cc8d4f82a4| [AIA](http://cacerts.digicert.com/DigiCertGlobalRootG2.crt)<br>[CDP](http://crl3.digicert.com/DigiCertGlobalRootG2.crl) |


#### <a name="associated-intermediate-cas"></a>Entidades de certificación intermedias asociadas

| Entidad de certificación intermedia y de emisión| Número de serie| Fecha de emisión y fecha de expiración| Huella digital SHA1| URI |
| - | - | - | - | - | 
| Microsoft Azure TLS Issuing CA 01| 0aafa6c5ca63c45141 ea3be1f7c75317| 29 de julio de 2020<br>27 de junio de 2024| 2f2877c5d778c31e0f29c 7e371df5471bd673173| [AIA](https://www.microsoft.com/pkiops/certs/Microsoft%20Azure%20TLS%20Issuing%20CA%2001%20-%20xsign.crt)<br>[CDP](https://www.microsoft.com/pkiops/crl/Microsoft%20Azure%20TLS%20Issuing%20CA%2001.crl)|
|Microsoft Azure TLS Issuing CA 02| 0c6ae97cced59983 8690a00a9ea53214| 29 de julio de 2020<br>27 de junio de 2024| e7eea674ca718e3befd 90858e09f8372ad0ae2aa| [AIA](https://www.microsoft.com/pkiops/certs/Microsoft%20Azure%20TLS%20Issuing%20CA%2002%20-%20xsign.crt)<br>[CDP](https://www.microsoft.com/pkiops/crl/Microsoft%20Azure%20TLS%20Issuing%20CA%2002.crl) |
| Microsoft Azure TLS Issuing CA 05| 0d7bede97d8209967a 52631b8bdd18bd| 29 de julio de 2020<br>27 de junio de 2024| 6c3af02e7f269aa73a fd0eff2a88a4a1f04ed1e5| [AIA](https://www.microsoft.com/pkiops/certs/Microsoft%20Azure%20TLS%20Issuing%20CA%2005%20-%20xsign.crt)<br>[CDP](https://www.microsoft.com/pkiops/crl/Microsoft%20Azure%20TLS%20Issuing%20CA%2005.crl) |
| Microsoft Azure TLS Issuing CA 06| 02e79171fb8021e93fe 2d983834c50c0| 29 de julio de 2020<br>27 de junio de 2024| 30e01761ab97e59a06b 41ef20af6f2de7ef4f7b0| [AIA](https://www.microsoft.com/pkiops/certs/Microsoft%20Azure%20TLS%20Issuing%20CA%2006.cer)<br>[CDP](https://www.microsoft.com/pkiops/crl/Microsoft%20Azure%20TLS%20Issuing%20CA%2006.crl) |


 ### <a name="baltimore-cybertrust-root"></a>Baltimore CyberTrust Root

| Entidad de certificación raíz| Número de serie| Fecha de emisión y fecha de expiración| Huella digital SHA1| URI |
| - | - | - | - | - | 
| Baltimore CyberTrust Root| 020000b9| 12 de mayo de 2000<br>12 de mayo de 2025| d4de20d05e66fc53fe 1a50882c78db2852cae474|<br>[CDP](http://crl3.digicert.com/Omniroot2025.crl)<br>[OCSP](http://ocsp.digicert.com/) |


#### <a name="associated-intermediate-cas"></a>Entidades de certificación intermedias asociadas

| Entidad de certificación intermedia y de emisión| Número de serie| Fecha de emisión y fecha de expiración| Huella digital SHA1| URI |
| - | - | - | - | - | 
| Microsoft RSA TLS CA 01| 703d7a8f0ebf55aaa 59f98eaf4a206004eb2516a| 21 de julio de 2020<br>8 de octubre de 2024| 417e225037fbfaa4f9 5761d5ae729e1aea7e3a42| [AIA](https://www.microsoft.com/pki/mscorp/Microsoft%20RSA%20TLS%20CA%2001.crt)<br>[CDP](https://mscrl.microsoft.com/pki/mscorp/crl/Microsoft%20RSA%20TLS%20CA%2001.crl)<br>[OCSP](http://ocsp.msocsp.com/) |
| Microsoft RSA TLS CA 02| b0c2d2d13cdd56cdaa 6ab6e2c04440be4a429c75| 21 de julio de 2020<br>20 de mayo de 2024| 54d9d20239080c32316ed 9ff980a48988f4adf2d| [AIA](https://www.microsoft.com/pki/mscorp/Microsoft%20RSA%20TLS%20CA%2002.crt)<br>[CDP](https://mscrl.microsoft.com/pki/mscorp/crl/Microsoft%20RSA%20TLS%20CA%2002.crl)<br>[OCSP](http://ocsp.msocsp.com/) |


 ### <a name="digicert-global-root-ca"></a>DigiCert Global Root CA

| Entidad de certificación raíz| Número de serie| Fecha de emisión y fecha de expiración| Huella digital SHA1| URI |
| - | - | - | - | - | 
| DigiCert Global Root CA| 083be056904246 b1a1756ac95991c74a| 9 de noviembre de 2006<br>9 de noviembre de 2031| a8985d3a65e5e5c4b2d7 d66d40c6dd2fb19c5436| [CDP](http://crl3.digicert.com/DigiCertGlobalRootCA.crl)<br>[OCSP](http://ocsp.digicert.com/) |


#### <a name="associated-intermediate-cas"></a>Entidades de certificación intermedias asociadas

| Entidad de certificación intermedia y de emisión| Número de serie| Fecha de emisión y fecha de expiración| Huella digital SHA1| URI |
| - | - | - | - | - |
| DigiCert SHA2 Secure Server CA| 01fda3eb6eca75c 888438b724bcfbc91| 8 de marzo de 2013
8 de marzo 2023| 1fb86b1168ec743154062 e8c9cc5b171a4b7ccb4| [AIA](http://cacerts.digicert.com/DigiCertSHA2SecureServerCA.crt)<br>[CDP](http://crl3.digicert.com/ssca-sha2-g6.crl)<br>[OCSP](http://ocsp.digicert.com/) |
| DigiCert SHA2 Secure Server CA |02742eaa17ca8e21 c717bb1ffcfd0ca0 |22 de septiembre de 2020<br>22 de septiembre de 2030|626d44e704d1ceabe3bf 0d53397464ac8080142c|[AIA](http://cacerts.digicert.com/DigiCertSHA2SecureServerCA-2.crt)<br>[CDP](http://crl3.digicert.com/DigiCertSHA2SecureServerCA.crl)<br>[OCSP](http://ocsp.digicert.com/)|


## <a name="cas-used-in-azure-china-21vianet-cloud"></a>Entidad de certificación usada en la nube de Azure China 21Vianet

### <a name="digicert-global-root-ca"></a>DigiCert Global Root CA


| Entidad de certificación raíz| Número de serie| Fecha de emisión y fecha de expiración| Huella digital SHA1| URI |
| - | - | - | - | - |
| DigiCert Global Root CA| 083be056904246b 1a1756ac95991c74a| 9 de noviembre de 2006<br>9 de noviembre de 2031| a8985d3a65e5e5c4b2d7 d66d40c6dd2fb19c5436| [CDP](http://ocsp.digicert.com/)<br>[OCSP](http://crl3.digicert.com/DigiCertGlobalRootCA.crl) |


#### <a name="associated-intermediate-ca"></a>Entidad de certificación intermedia asociada

| Entidad de certificación intermedia y de emisión| Número de serie| Fecha de emisión y fecha de expiración| Huella digital SHA1| URI |
| - | - | - | - | - | - |
| DigiCert Basic RSA CN CA G2| 02f7e1f982bad 009aff47dc95741b2f6| 4 de marzo de 2020<br>4 de marzo de 2030| 4d1fa5d1fb1ac3917c08e 43f65015e6aea571179| [AIA](http://cacerts.digicert.cn/DigiCertBasicRSACNCAG2.crt)<br>[CDP](http://crl.digicert.cn/DigiCertBasicRSACNCAG2.crl)<br>[OCSP](http://ocsp.digicert.cn/) |

## <a name="next-steps"></a>Pasos siguientes
[Más información sobre las cadenas de cifrado de Microsoft 365](/microsoft-365/compliance/encryption-office-365-certificate-chains?view=o365-worldwide)