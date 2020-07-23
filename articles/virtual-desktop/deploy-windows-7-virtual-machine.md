---
title: Implementación de una máquina virtual con Windows 7 en Windows Virtual Desktop (Azure)
description: Implementación de una máquina virtual con Windows 7 en Windows Virtual Desktop.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 07/11/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 542d0bcea6cfae2af2918d699952d4c054bd68cb
ms.sourcegitcommit: 0b2367b4a9171cac4a706ae9f516e108e25db30c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2020
ms.locfileid: "86277613"
---
# <a name="deploy-a-windows-7-virtual-machine-on-windows-virtual-desktop"></a>Implementación de una máquina virtual Windows 7 en Windows Virtual Desktop

>[!IMPORTANT]
>Este contenido se aplica a la actualización de primavera de 2020 con objetos de Windows Virtual Desktop para Azure Resource Manager. Si usa la versión de otoño de 2019 de Windows Virtual Desktop sin objetos de Azure Resource Manager, consulte [este artículo](./virtual-desktop-fall-2019/deploy-windows-7-virtual-machine.md).
>
> La actualización de primavera de 2020 de Windows Virtual Desktop se encuentra actualmente en versión preliminar pública. Esta versión preliminar se ofrece sin un Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. 
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

El proceso de implementación de una máquina virtual con Windows 7 en Windows Virtual Desktop es ligeramente diferente al de las máquinas virtuales que ejecutan versiones posteriores de Windows. En esta guía se explica cómo implementar Windows 7.

## <a name="prerequisites"></a>Requisitos previos

Antes de empezar, siga las instrucciones de [Creación de un grupo hosts con PowerShell](create-host-pools-powershell.md) para crear un grupo de hosts. Si va a usar el portal, siga las instrucciones de los pasos 1 a 9 del artículo [Creación de un grupo de hosts con Azure Portal](create-host-pools-azure-marketplace.md). Después, seleccione **Revisar y crear** para crear un grupo de hosts vacío. 

## <a name="configure-a-windows-7-virtual-machine"></a>Configuración de una máquina virtual con Windows 7

Una vez que haya realizado los requisitos previos, estará listo para configurar la máquina virtual de Windows 7 para su implementación en Windows Virtual Desktop.

Para configurar una máquina virtual de Windows 7 en Windows Virtual Desktop:

1. Inicie sesión en el Azure Portal y busque la imagen de Windows 7 Enterprise o cargue su propia imagen personalizada de Windows 7 Enterprise (x64).
2. Implemente una o varias máquinas virtuales con Windows 7 Enterprise como sistema operativo host. Asegúrese de que las máquinas virtuales permiten el Protocolo de escritorio remoto (RDP) (el puerto TCP/3389).
3. Conéctese al host de Windows 7 Enterprise mediante el RDP y autentíquese con las credenciales que definió al configurar la implementación.
4. Agregue la cuenta que usó al conectarse al host con RDP al grupo "Usuario de Escritorio remoto". Si no la agrega, es posible que no pueda conectarse a la máquina virtual después de conectarla a su dominio de Active Directory.
5. Vaya a Windows Update en la máquina virtual.
6. Instale todas las actualizaciones de Windows en la categoría Importante.
7. Instale todas las actualizaciones de Windows en la categoría Opcional (excepto los paquetes de idioma). Este proceso instala la actualización del Protocolo de escritorio remoto 8.0 ([KB2592687](https://www.microsoft.com/download/details.aspx?id=35387)) que necesita para completar estas instrucciones.
8. Abra el Editor de directivas de grupo local y vaya a **Configuración del equipo** > **Plantillas administrativas** > **Componentes de Windows** > **Servicios de escritorio remoto** > **Host de sesión de escritorio remoto** > **Entorno de sesión remota**.
9. Habilite la directiva del Protocolo de escritorio remoto 8.0.
10. Una esta máquina virtual al dominio de Active Directory.
11. Reinicie la máquina virtual, para lo que debe ejecutar el siguiente comando:

     ```cmd
     shutdown /r /t 0
     ```

12. Siga las instrucciones descritas [aquí](/powershell/module/az.desktopvirtualization/new-azwvdregistrationinfo?view=azps-4.3.0) para obtener un token de registro.
      
      - Si prefiere usar Azure Portal, también puede ir a la página de información general del grupo de hosts al que desea agregar la máquina virtual y crear un token en ella.
  
13. [Descargue el agente de Windows Virtual Desktop para Windows 7](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE3JZCm).
14. [Descargue el administrador de agente de Windows Virtual Desktop para Windows 7](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE3K2e3).
15. Abra el instalador del agente de Windows Virtual Desktop y siga las instrucciones. Cuando se le solicite, proporcione la clave de registro que ha creado en el paso 12.
16. Abra Windows Virtual Desktop Agent Manager y siga las instrucciones.
17. Opcionalmente, bloquee el puerto TCP/3389 para quitar el acceso directo del protocolo de escritorio remoto a la máquina virtual.
18. También puede confirmar que .NET Framework se encuentra al menos en la versión 4.7.2. Esta actualización es especialmente importante si va a crear una imagen personalizada.

## <a name="next-steps"></a>Pasos siguientes

La implementación de Windows Virtual Desktop ya está lista para usarse. [Descargue la versión más reciente del cliente de Windows Virtual Desktop](https://aka.ms/wvd/clients/windows) para comenzar.

Para obtener una lista de los problemas conocidos, así como instrucciones para la solución de problemas de Windows 7 en Windows Virtual Desktop, consulte nuestro artículo para la solución de problemas en [Solución de los problemas de las máquinas virtuales con Windows 7 en Windows Virtual Desktop](./virtual-desktop-fall-2019/troubleshoot-windows-7-vm.md).
