---
title: Instalación del agente de aprovisionamiento en la nube de Azure AD Connect
description: En este artículo se describe cómo instalar el agente de aprovisionamiento en la nube de Azure AD Connect.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 05/19/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 764071eeaf368ecf78679632cffbd6964db40aa5
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2020
ms.locfileid: "83681035"
---
# <a name="install-the-azure-ad-connect-cloud-provisioning-agent"></a>Instalación del agente de aprovisionamiento en la nube de Azure AD Connect
Este documento le guiará en el proceso de instalación del agente de aprovisionamiento de Azure Active Directory (Azure AD) Connect y en el modo de configurarlo inicialmente en Azure Portal.

>[!IMPORTANT]
>En las siguientes instrucciones de instalación se supone que se han cumplido todos los [requisitos previos](how-to-prerequisites.md).

La instalación y configuración del aprovisionamiento de Azure AD Connect se realiza en los pasos siguientes:
    
- [Instalación del agente](#install-the-agent)
- [Comprobación de la instalación del agente](#verify-agent-installation)


## <a name="install-the-agent"></a>Instalación del agente
Para instalar el agente, siga estos pasos.

1. Inicie sesión en el servidor que va a usar con permisos de administrador de empresa.
1. Vaya a Azure Portal. En la parte izquierda, seleccione **Azure Active Directory**.
1. Seleccione **Administrar el aprovisionamiento (versión preliminar)**  > **Revisar todos los agentes**.
1. Descargue el agente de aprovisionamiento de Azure AD Connect en Azure Portal.

   ![Descargar el agente local](media/how-to-install/install9.png)</br>
1. Ejecute el instalador de aprovisionamiento de Azure AD Connect (AADConnectProvisioningAgent.Installer).
1. En la pantalla **Microsoft Azure AD Connect Provisioning Agent Package**, acepte los términos de licencia y seleccione **Instalar**.

   ![Pantalla Microsoft Azure AD Connect Provisioning Agent Package](media/how-to-install/install1.png)</br>

1. Una vez finalizada esta operación, se inicia el Asistente para configuración. Inicie sesión con su cuenta de administrador global de Azure AD.
1. En la pantalla **Conectar Active Directory**, seleccione **Agregar directorio**. Después, inicie sesión con su cuenta de administrador de Active Directory. Esta operación permitirá agregar su directorio local. Seleccione **Next** (Siguiente).

   ![Pantalla Conexión de Active Directory](media/how-to-install/install3.png)</br>

1. En la pantalla **Configuración completa**, seleccione **Confirmar**. Esta operación registra y reinicia el agente.

   ![Pantalla de configuración finalizada](media/how-to-install/install4.png)</br>

1. Una vez finalizada esta operación, verá el aviso **La configuración del agente se ha comprobado correctamente.** Seleccione **Salir**.

   ![Botón Salir](media/how-to-install/install5.png)</br>
1. Si todavía ve la pantalla inicial **Microsoft Azure AD Connect Provisioning Agent Package**, seleccione **Cerrar**.

## <a name="verify-agent-installation"></a>Comprobación de la instalación del agente
La comprobación del agente se produce en Azure Portal y en el servidor local que ejecuta el agente.

### <a name="azure-portal-agent-verification"></a>Comprobación del agente en Azure Portal
Para comprobar que Azure ve el agente, siga estos pasos.

1. Inicie sesión en Azure Portal.
1. En la parte izquierda, seleccione **Azure Active Directory** > **Azure AD Connect**. En el centro, seleccione **Administración del aprovisionamiento (versión preliminar)** .

   ![Azure portal](media/how-to-install/install6.png)</br>

1.  En la pantalla **Aprovisionamiento de Azure AD (versión preliminar)** , seleccione **Revisar todos los agentes**.

    ![Opción Revisar todos los agentes](media/how-to-install/install7.png)</br>
 
1. En la pantalla **Agentes de aprovisionamiento locales**, verá los agentes que ha instalado. Compruebe que el agente en cuestión está ahí y que se ha marcado como *Activo*.

   ![Pantalla de agentes de aprovisionamiento local](media/how-to-install/verify1.png)</br>



### <a name="on-the-local-server"></a>En el servidor local
Para comprobar que el agente se ejecuta, siga estos pasos.

1.  Inicie sesión en el servidor con una cuenta de administrador.
1.  Abra **Servicios**. Para ello, vaya ahí o a **Inicio** > **Ejecutar** > **Services.msc**.
1.  En **Servicios**, asegúrese de que tanto el **Actualizador del Agente de Microsoft Azure AD Connect** como el **Agente de aprovisionamiento de Microsoft Azure AD Connect** están ahí y que su estado es *En ejecución*.

    ![Pantalla de servicios](media/how-to-troubleshoot/troubleshoot1.png)

>[!IMPORTANT]
>El agente se ha instalado, pero debe configurarse y habilitarse antes de que empiece a sincronizar a los usuarios. Para configurar un nuevo agente, consulte [Creación de una nueva configuración para el aprovisionamiento basado en la nube de Azure AD Connect](how-to-configure.md).



## <a name="next-steps"></a>Pasos siguientes 

- [¿Qué es el aprovisionamiento?](what-is-provisioning.md)
- [¿Qué es el aprovisionamiento en la nube de Azure AD Connect?](what-is-cloud-provisioning.md)
 
