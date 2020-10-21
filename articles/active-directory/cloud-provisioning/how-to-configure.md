---
title: Configuración del nuevo agente de aprovisionamiento en la nube de Azure AD Connect
description: En este artículo se describe cómo instalar el aprovisionamiento en la nube.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 02/26/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: c8b18629a776dd98950f49b1f607cbc876abcd9c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91628912"
---
# <a name="create-a-new-configuration-for-azure-ad-connect-cloud-based-provisioning"></a>Creación de una nueva configuración para el aprovisionamiento basado en la nube de Azure AD Connect

Una vez instalado el agente, debe iniciar sesión en Azure Portal y configurar el aprovisionamiento en la nube de Azure Active Directory (Azure AD) Connect. Siga los pasos que se indican a continuación para habilitar el agente.

## <a name="configure-provisioning"></a>Configuración del aprovisionamiento
Para configurar el aprovisionamiento, siga estos pasos.

 1. En Azure Portal, seleccione **Azure Active Directory**.
 2. Seleccione **Azure AD Connect**.
 3. Seleccione **Administrar aprovisionamiento**.

 ![Administración del aprovisionamiento](media/how-to-configure/manage1.png)
 
 4. Seleccione **Nueva configuración**.
 5. En la pantalla de configuración, seleccione su dominio y si quiere habilitar la sincronización del hash de contraseñas.  Haga clic en **Crear**.  
 
 ![Creación de una nueva configuración](media/how-to-configure/configure1.png)


 6.  Se abrirá la pantalla Edit provisioning configuration (Editar la configuración de aprovisionamiento).

   ![Editar la configuración](media/how-to-configure/configure2.png)

 7. Escriba un **correo electrónico de notificación de alerta**. Este correo electrónico recibirá una notificación cuando el aprovisionamiento no sea correcto.
 8. Mueva el selector a Habilitar y seleccione Guardar.

## <a name="scope-provisioning-to-specific-users-and-groups"></a>Especificación del ámbito de aprovisionamiento a usuarios y grupos específicos
Puede especificar el ámbito del agente para sincronizar usuarios y grupos específicos mediante el uso de grupos o unidades organizativas de Active Directory locales. No puede configurar grupos ni unidades organizativas dentro de una configuración. 

 1.  En Azure Portal, seleccione **Azure Active Directory**.
 2. Seleccione **Azure AD Connect**.
 3. Seleccione **Administración del aprovisionamiento (versión preliminar)** .
 4. En **Configuración**, seleccione su configuración.

 ![Sección de configuración](media/how-to-configure/scope1.png)
 
 5. En **Configurar**, seleccione **Edit scoping filters** (Editar los filtros de ámbito) para cambiar el ámbito de la regla de configuración.
 ![Editar ámbito](media/how-to-configure/scope3.png)
 7. A la derecha, puede cambiar el ámbito.  Haga clic en **Listo** y en **Guardar** cuando haya terminado.
 8. Una vez cambiado el ámbito, debe [reiniciar el aprovisionamiento](#restart-provisioning) para iniciar una sincronización inmediata de los cambios.

## <a name="attribute-mapping"></a>Asignación de atributos
El aprovisionamiento en la nube de Azure AD Connect le permite asignar fácilmente atributos entre los objetos de usuario o de grupo locales y los objetos de Azure AD.  Puede personalizar las asignaciones de atributos predeterminadas según sus necesidades empresariales. Esto significa que puede cambiar o eliminar asignaciones de atributos existentes o crear nuevas asignaciones de atributos.  Para obtener más información, consulte la [asignación de atributos](how-to-attribute-mapping.md).

## <a name="on-demand-provisioning"></a>Aprovisionamiento a petición
El aprovisionamiento en la nube de Azure AD Connect le permite probar los cambios de configuración, aplicando estos cambios a un solo usuario o grupo.  Puede utilizar esta característica para validar y comprobar que los cambios realizados en la configuración se aplicaron correctamente y que se están sincronizando correctamente con Azure AD.  Para obtener más información, consulte el [aprovisionamiento a petición](how-to-on-demand-provision.md).

## <a name="restart-provisioning"></a>Reinicio del aprovisionamiento 
Si no desea esperar a la siguiente ejecución programada, desencadene la ejecución del aprovisionamiento mediante el botón **Reiniciar aprovisionamiento**. 
 1.  En Azure Portal, seleccione **Azure Active Directory**.
 2. Seleccione **Azure AD Connect**.
 3.  Seleccione **Administración del aprovisionamiento (versión preliminar)** .
 4. En **Configuración**, seleccione su configuración.

   ![Selección de configuración para reiniciar el aprovisionamiento](media/how-to-configure/scope1.png)

 5. En la parte superior, seleccione **Reiniciar aprovisionamiento**.

## <a name="remove-a-configuration"></a>Eliminación de una configuración
Para eliminar una configuración, siga estos pasos.

 1.  En Azure Portal, seleccione **Azure Active Directory**.
 2. Seleccione **Azure AD Connect**.
 3. Seleccione **Administración del aprovisionamiento (versión preliminar)** .
 4. En **Configuración**, seleccione su configuración.
   
   ![Selección de configuración para quitar la configuración](media/how-to-configure/scope1.png)

 5. En la parte superior de la pantalla de configuración, seleccione **Eliminar**.

>[!IMPORTANT]
>No se pide confirmación antes de eliminar una configuración. Asegúrese de que esta es la acción que desea realizar antes de seleccionar **Eliminar**.


## <a name="next-steps"></a>Pasos siguientes 

- [¿Qué es el aprovisionamiento?](what-is-provisioning.md)
- [¿Qué es el aprovisionamiento en la nube de Azure AD Connect?](what-is-cloud-provisioning.md)
