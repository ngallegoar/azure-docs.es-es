---
title: Instrucciones de Azure Marketplace y AppSource para el anunciante | Azure
description: Instrucciones de Azure Marketplace y AppSource para los anunciantes de aplicaciones y servicios
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 06/13/2018
ms.author: dsindona
ms.openlocfilehash: 9156126ff2a3dbe936dc426c6f5097c6bba7b7b7
ms.sourcegitcommit: d0fd35f4f0f3ec71159e9fb43fcd8e89d653f3f2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2020
ms.locfileid: "80387618"
---
# <a name="guidelines"></a>Directrices  

<!--
## Guidelines for AppSource  
-->
---

## <a name="guidelines-for-azure-marketplace"></a>Instrucciones de Azure Marketplace  

### <a name="guidelines-for-creating-a-microsoft-id-to-manage-a-marketplace-account"></a>Instrucciones de creación de un identificador de Microsoft para administrar una cuenta de Marketplace  
Si más de una persona requiere acceso al mismo identificador de Microsoft que se utiliza para crear la cuenta de Marketplace, siga estas instrucciones para crear una cuenta de compañía. 

>[!IMPORTANT]
>Para autorizar que varios usuarios tengan acceso a su cuenta del Centro para desarrolladores de Microsoft (centro de desarrollo), Microsoft recomienda que utilice Azure Active Directory (Azure AD) para asignar roles a usuarios individuales. Cada usuario obtiene acceso a la cuenta iniciando sesión con las credenciales de Azure AD individuales. Microsoft sugiere que, al crear el identificador de Microsoft mediante una dirección de correo electrónico en un dominio registrado en su compañía, el correo electrónico no se asigne a un usuario individual. Un ejemplo es `windowsapps@fabrikam.com`.  
>*   Para más información, consulte la sección [Problema: Identificador de Microsoft en un dominio federado de Azure AD](#issue-microsoft-id-in-an-azure-ad-federated-domain).  

*   Limite el acceso al identificador de Microsoft al menor número posible de desarrolladores. 
*   Configure una lista de distribución (DL) de correo electrónico corporativo que incluya todos los usuarios que tienen acceso a su cuenta del Centro de desarrollo. Agregue la dirección de correo electrónico de DL a la información de seguridad. La DL permite a todos los empleados de la lista recibir códigos de seguridad cuando lo pidan y administrar la información de seguridad del identificador de Microsoft. Si no es posible configurar una lista de distribución, el propietario de la cuenta de correo electrónico individual debe estar disponible para tener acceso al código de seguridad y compartirlo cuando se le solicite.  
    *   Por ejemplo, se le pregunta al propietario cuando se agrega nueva información de seguridad al identificador de Microsoft o cuando se obtiene acceso al identificador de Microsoft desde un dispositivo nuevo.  
*   Agregue un número de teléfono de la empresa que no necesite extensión y al que puedan tener acceso miembros clave del equipo.  
*   En general, debe exigir a los desarrolladores que utilicen dispositivos de confianza para iniciar sesión en su cuenta del Centro de desarrollo. Todos los miembros clave del equipo deben tener acceso a estos dispositivos de confianza. El uso de dispositivos de confianza para el acceso, reduce la necesidad de enviar códigos de seguridad cuando un usuario obtiene acceso a la cuenta del Centro de desarrollo.  
*   Si tiene que conceder acceso a la cuenta del Centro de desarrollo desde un equipo que no es de confianza, debe limitar el acceso a cinco desarrolladores como máximo. Idealmente, los desarrolladores deben obtener acceso a la cuenta desde equipos que compartan la misma ubicación geográfica y de la red.  
*   Revise y compruebe con frecuencia la información de seguridad.  
    *   Para ver la información de seguridad, visite la página Configuración de seguridad en [account.live.com/proofs/Manage](https://account.live.com/proofs/Manage).

El acceso a su cuenta del Centro de desarrollo debe obtenerlo principalmente desde equipos de confianza. Es fundamental que obtenga acceso desde equipos de confianza, ya que hay un límite en el número de códigos generados por cuenta del Centro de desarrollo a la semana. El uso de equipos de confianza también permite la experiencia de inicio de sesión más segura y coherente. 
*   Para más información sobre la seguridad y las instrucciones de cuenta del Centro de desarrollo adicionales, visite la página Abrir una cuenta de desarrollador en [docs.microsoft.com/windows/uwp/publish/opening-a-developer-account](https://docs.microsoft.com/windows/uwp/publish/opening-a-developer-account). 

---

#### <a name="issue-microsoft-id-in-an-azure-ad-federated-domain"></a>Problema: Identificador de Microsoft en un dominio federado de Azure AD  
La cuenta corporativa puede federarse mediante Azure Active Directory (Azure AD). Si intenta crear un identificador de Microsoft con una dirección de correo electrónico corporativo que se federa con Azure AD, recibirá un error. Si recibe un error, debe confirmar con su equipo de TI si su cuenta está federada mediante Azure AD. El correo electrónico federado de Azure AD es un problema conocido y Microsoft está trabajando para resolverlo.  
*   Para más información sobre Azure AD, visite la página Documentación de Azure Active Directory en [docs.microsoft.com/azure/active-directory](https://docs.microsoft.com/azure/active-directory).

Microsoft recomienda una solución alternativa. Siga estos pasos para crear una dirección de correo electrónico en el dominio `outlook.com` y una regla que reenvíe las comunicaciones.  
1.  Vaya a la página Crear cuenta y haga clic en el vínculo Obtener una nueva dirección de correo electrónico. 
    *   Para suscribirse al identificador de Microsoft, visite la página Crear cuenta situada en [signup.live.com/signup](https://signup.live.com/signup).  
2.  Cree la nueva dirección de correo electrónico y escriba una contraseña. Se crea un identificador de Microsoft y un buzón de correo electrónico en el dominio `outlook.com`. Continúe el proceso de registro hasta crear la cuenta.  

    >[!IMPORTANT]
    >Debe utilizar una dirección de correo electrónico o una lista de distribución que esté registrada como identificador de Microsoft para registrarse en el Centro de desarrollo. Microsoft recomienda usar una lista de distribución para quitar la dependencia de los usuarios. Si la lista de distribución o la dirección de correo electrónico no está registrada, debe registrarla ahora.    

    >[!Important]
    >Si su dirección de correo electrónico se encuentra en el dominio de la compañía `Microsoft`, no podrá utilizarla para registrarse en el Centro de desarrollo.  

3.  Después de crear el identificador de Microsoft con la dirección de correo electrónico de Outlook, inicie sesión en el buzón de Outlook. Cree una regla de reenvío de correo. La regla de reenvío de correo debe mover todos los correos electrónicos de este buzón a la dirección de correo electrónico del dominio que creó para administrar la cuenta de Azure Marketplace.  
    *   Para iniciar sesión en el buzón de Outlook, visite la página de Outlook en [outlook.live.com/owa](https://outlook.live.com/owa).  
    *   Para más información sobre las reglas de reenvío, visite la página de uso de reglas de Outlook Web App para reenviar mensajes automáticamente a otra página de cuenta situada en [support.office.com/article/Use-rules-in-Outlook-Web-App-to-automatically-forward-messages-to-another-account-1433e3a0-7fb0-4999-b536-50e05cb67fed](https://support.office.com/article/Use-rules-in-Outlook-Web-App-to-automatically-forward-messages-to-another-account-1433e3a0-7fb0-4999-b536-50e05cb67fed).  

1.  La regla de reenvío envía todos los correos electrónicos y las comunicaciones que se reciben en la cuenta de correo electrónico de Outlook a la dirección de correo electrónico de un dominio registrado para la compañía. Debe utilizar su dirección de correo electrónico `outlook.com` para autenticarse tanto en el Centro de desarrollo como en Cloud Partner Portal.  

## <a name="next-steps"></a>Pasos siguientes

*   Visite la página [Guía del anunciante de Azure Marketplace y AppSource](https://docs.microsoft.com/azure/marketplace/marketplace-publishers-guide). 
 
---
