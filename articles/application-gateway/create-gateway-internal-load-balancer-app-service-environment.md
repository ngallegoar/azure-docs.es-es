---
title: 'Solución de problemas de Application Gateway en Azure: ASE de ILB | Microsoft Docs'
description: Aprenda a solucionar los problemas de una puerta de enlace de aplicaciones mediante un equilibrador de carga interno con App Service Environment.
services: vpn-gateway
documentationCenter: na
author: genlin
manager: dcscontentpm
editor: ''
tags: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/09/2020
ms.author: genli
ms.openlocfilehash: 8861e850e168169762d95c44a54b6a88a036f396
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84628530"
---
# <a name="back-end-server-certificate-is-not-allow-listed-for-an-application-gateway-using-an-internal-load-balancer-with-an-app-service-environment"></a>El certificado de servidor back-end no está incluido en la lista de permitidos en una puerta de enlace de aplicaciones que usa un equilibrador de carga interno con App Service Environment.

En este artículo, se soluciona el problema siguiente: un certificado no se incluye en la lista blanca al crear una puerta de enlace de aplicaciones utilizando un equilibrador de carga interno (ILB) junto con una instancia de App Service Environment (ASE) en el back-end cuando se usa TLS de un extremo a otro en Azure.

## <a name="symptoms"></a>Síntomas

Cuando se crea una puerta de enlace de aplicaciones mediante un ILB con ASE en el back-end, el servidor back-end puede pasar a un estado incorrecto. Este problema se produce si el certificado de autenticación de la puerta de enlace de aplicaciones no coincide con el certificado configurado en el servidor back-end. Por ejemplo, vea el siguiente escenario:

**Configuración de una puerta de enlace de aplicaciones**

- **Agente de escucha:** Multisitio
- **Puerto:** 443
- **Nombre de host:** test.appgwtestase.com
- **Certificado SSL:** CN=test.appgwtestase.com
- **Grupo de back-end:** dirección IP o FQDN
- **Dirección IP:** 10.1.5.11
- **Configuración de HTTP:** HTTPS
- **Puerto:** 443
- **Sondeo personalizado:** nombre de host (test.appgwtestase.com)
- **Certificado de autenticación:** .cer de test.appgwtestase.com
- **Estado de back-end:** incorrecto, el certificado del servidor back-end no está incluido en la lista de permitidos de Application Gateway.

**Configuración de ASE:**

- **IP de ILB:** 10.1.5.11
- **Nombre de dominio:** appgwtestase.com
- **App Service:** test.appgwtestase.com
- **Enlace SSL:** SSL SNI (CN=test.appgwtestase.com)

Cuando accede a la puerta de enlace de aplicaciones, recibe el siguiente mensaje de error porque el servidor back-end está en mal estado:

**502 - Web server received an invalid response while acting as a gateway or proxy server** (502: el servidor web ha recibido una respuesta no válida mientras actuaba como puerta de enlace o servidor proxy).

## <a name="solution"></a>Solución

Cuando no se usa un nombre de host para acceder a un sitio web HTTPS, el servidor back-end devuelve el certificado configurado del sitio web predeterminado, en caso de que SNI esté deshabilitado. Para un ILB de ASE, el certificado predeterminado procede del certificado de ILB. Si no hay ningún certificado configurado para el ILB, el certificado procede del certificado de aplicación de ASE.

Cuando se usa un nombre de dominio completo (FQDN) para acceder al ILB, el servidor back-end devuelve el certificado correcto que se cargó en la configuración HTTP. Si no es el caso, tenga en cuenta las siguientes opciones:

- Use FQDN en el grupo de back-end de la puerta de enlace de aplicaciones para que apunte a la dirección IP del ILB. Esta opción solo funciona si tiene configurados una zona DNS privada o un DNS personalizado. De lo contrario, tendrá que crear un registro "D" para un DNS público.

- Use el certificado cargado en el ILB o el certificado predeterminado (certificado de ILB) de la configuración HTTP. La puerta de enlace de aplicaciones obtiene el certificado cuando accede a la dirección IP del ILB durante el sondeo.

- Utilice un certificado comodín en el ILB y el servidor back-end, de forma que este certificado sea el mismo en todos los sitios web. Sin embargo, esta solución solamente es posible únicamente en el caso de los subdominios, pero no si cada sitio web necesita un nombre de host diferente.

- Desactive la opción **Uso para App Service** de la puerta de enlace de aplicaciones si está utilizando la dirección IP del ILB.

Para reducir la sobrecarga, puede cargar el certificado de ILB en la configuración HTTP para que la ruta de acceso de sondeo funcione. (Este paso es solo para la inclusión en la lista de permitidos. No se usa para la comunicación TLS). Para recuperar el certificado de ILB, puede acceder al ILB utilizando su dirección IP en el explorador con HTTPS, exportar después el certificado TLS/SSL en un formato CER codificado en Base-64 y cargarlo en la configuración HTTP correspondiente.

## <a name="need-help-contact-support"></a>¿Necesita ayuda? Ponerse en contacto con soporte técnico

Si sigue necesitando ayuda, [póngase en contacto con el servicio de soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para resolver el problema rápidamente.
