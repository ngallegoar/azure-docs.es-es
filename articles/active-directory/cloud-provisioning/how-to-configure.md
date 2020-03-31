---
title: Configuración del nuevo agente de aprovisionamiento en la nube de Azure AD Connect
description: En este artículo se describe cómo instalar el aprovisionamiento en la nube.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/26/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: c086227d23901cb7904d8cc0a768e9f4b5ad43db
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77620968"
---
# <a name="create-a-new-configuration-for-azure-ad-connect-cloud-based-provisioning"></a>Creación de una nueva configuración para el aprovisionamiento basado en la nube de Azure AD Connect

Una vez instalado el agente, debe iniciar sesión en Azure Portal y configurar el aprovisionamiento en la nube de Azure Active Directory (Azure AD) Connect. Siga los pasos que se indican a continuación para habilitar el agente.

## <a name="configure-provisioning"></a>Configuración del aprovisionamiento
Para configurar el aprovisionamiento, siga estos pasos.

1.  En Azure Portal, seleccione **Azure Active Directory**.
1.  Seleccione **Azure AD Connect**.
1.  Seleccione **Administración del aprovisionamiento (versión preliminar)** .

    ![Administración del aprovisionamiento (versión preliminar)](media/how-to-configure/manage1.png)

1.  Seleccione **Nueva configuración**.
1.  En la pantalla de configuración, el dominio local se ha rellenado previamente.
1.  Escriba un **correo electrónico de notificación de alerta**. Este correo electrónico recibirá una notificación cuando el aprovisionamiento no sea correcto.
1.  Mueva el selector a **Habilitar** y seleccione **Guardar**.

    ![Aprovisionamiento de Azure AD (versión preliminar)](media/tutorial-single-forest/configure2.png)

## <a name="scope-provisioning-to-specific-users-and-groups"></a>Especificación del ámbito de aprovisionamiento a usuarios y grupos específicos
Puede especificar el ámbito del agente para sincronizar usuarios y grupos específicos mediante el uso de grupos o unidades organizativas de Active Directory locales. No puede configurar grupos ni unidades organizativas dentro de una configuración. 

1.  En Azure Portal, seleccione **Azure Active Directory**.
1.  Seleccione **Azure AD Connect**.
1.  Seleccione **Administración del aprovisionamiento (versión preliminar)** .
1.  En **Configuración**, seleccione su configuración.

    ![Sección de configuración](media/how-to-configure/scope1.png)

1.  En **Configurar**, seleccione **Todos los usuarios** para cambiar el ámbito de la regla de configuración.

    ![Opción Todos los usuarios](media/how-to-configure/scope2.png)

1. A la derecha, puede cambiar el ámbito para incluir solo los grupos de seguridad. Escriba el nombre distintivo del grupo y seleccione **Agregar**.

    ![Opción Grupos de seguridad seleccionados](media/how-to-configure/scope3.png)

1.  O bien, puede cambiar el ámbito para incluir solo unidades organizativas específicas. Seleccione **Listo** y **Guardar**.  
2.  Una vez cambiado el ámbito, debe [reiniciar el aprovisionamiento](#restart-provisioning) para iniciar una sincronización inmediata de los cambios.

    ![Opción Unidades organizativas seleccionadas](media/how-to-configure/scope4.png)


## <a name="restart-provisioning"></a>Reinicio del aprovisionamiento 
Si no desea esperar a la siguiente ejecución programada, desencadene la ejecución del aprovisionamiento mediante el botón **Reiniciar aprovisionamiento**. 
1.  En Azure Portal, seleccione **Azure Active Directory**.
1.  Seleccione **Azure AD Connect**.
1.  Seleccione **Administración del aprovisionamiento (versión preliminar)** .
1.  En **Configuración**, seleccione su configuración.

    ![Selección de configuración para reiniciar el aprovisionamiento](media/how-to-configure/scope1.png)

1.  En la parte superior, seleccione **Reiniciar aprovisionamiento**.

## <a name="remove-a-configuration"></a>Eliminación de una configuración
Para eliminar una configuración, siga estos pasos.

1.  En Azure Portal, seleccione **Azure Active Directory**.
1.  Seleccione **Azure AD Connect**.
1.  Seleccione **Administración del aprovisionamiento (versión preliminar)** .
1.  En **Configuración**, seleccione su configuración.

    ![Selección de configuración para quitar la configuración](media/how-to-configure/scope1.png)

1.  En la parte superior de la pantalla de configuración, seleccione **Eliminar**.

    ![Botón Eliminar](media/how-to-configure/remove1.png)

>[!IMPORTANT]
>No se pide confirmación antes de eliminar una configuración. Asegúrese de que esta es la acción que desea realizar antes de seleccionar **Eliminar**.


## <a name="next-steps"></a>Pasos siguientes 

- [¿Qué es el aprovisionamiento?](what-is-provisioning.md)
- [¿Qué es el aprovisionamiento en la nube de Azure AD Connect?](what-is-cloud-provisioning.md)
