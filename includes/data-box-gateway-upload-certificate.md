---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 10/15/2020
ms.author: alkohli
ms.openlocfilehash: 27a4d775b8d88e02be69655e5adfc6155f867ef6
ms.sourcegitcommit: 16c7fd8fe944ece07b6cf42a9c0e82b057900662
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2020
ms.locfileid: "96581234"
---
Un certificado SSL correcto garantiza que envía la información cifrada al servidor correcto. Además del cifrado, el certificado también permite la autenticación. Puede cargar su propio certificado SSL de confianza mediante la interfaz de PowerShell del dispositivo.

1. [Conéctese a la interfaz de PowerShell](#connect-to-the-powershell-interface).
2. Use el cmdlet `Set-HcsCertificate` para cargar el certificado. Cuando se le solicite, proporcione los siguientes parámetros:

   - `CertificateFilePath`: ruta de acceso al recurso compartido que contiene el archivo de certificado en formato *.pfx*.
   - `CertificatePassword`: contraseña usada para proteger el certificado.
   - `Credentials`: nombre de usuario para obtener acceso al recurso compartido que contiene el certificado. Proporcione la contraseña para el recurso compartido de red cuando se le solicite.

     En el ejemplo siguiente se muestra el uso de este cmdlet:

     ```
     Set-HcsCertificate -Scope LocalWebUI -CertificateFilePath "\\myfileshare\certificates\mycert.pfx" -CertificatePassword "mypassword" -Credential "Username"
     ```
