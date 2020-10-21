---
title: Aprovisionamiento a petición del aprovisionamiento en la nube de Azure AD Connect
description: En este artículo se describe la característica de aprovisionamiento a petición.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 09/14/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 72623d69f9fb0cbc68758a362bb977e5e02d2c00
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91636967"
---
# <a name="azure-ad-connect-cloud-provisioning-on-demand-provisioning"></a>Aprovisionamiento a petición del aprovisionamiento en la nube de Azure AD Connect

El aprovisionamiento en la nube de Azure AD Connect ha introducido una nueva característica que le permitirá probar los cambios de configuración aplicándolos a un solo usuario.  Puede utilizar esta característica para validar y comprobar que los cambios realizados en la configuración se aplicaron correctamente y que se están sincronizando correctamente con Azure AD.  

> [!IMPORTANT] 
> Cuando se usa el aprovisionamiento a petición, los filtros de ámbito no se aplican al usuario seleccionado.  Esto significa que puede usar el aprovisionamiento a petición en los usuarios que están fuera de las unidades organizativas que haya especificado.


## <a name="using-on-demand-provisioning"></a>Uso del aprovisionamiento a petición
Para usar esta nueva característica, siga los pasos que se indican a continuación.


1.  En Azure Portal, seleccione **Azure Active Directory**.
2.  Seleccione **Azure AD Connect**.
3.  Seleccione **Administrar aprovisionamiento**.

    ![Administración del aprovisionamiento](media/how-to-configure/manage1.png)
4. En **Configuración**, seleccione su configuración.
5. En **Validar**, haga clic en el botón **Aprovisionar un usuario**. 

 ![Aprovisionamiento de un usuario](media/how-to-on-demand-provision/on-demand2.png)

6. En la pantalla del aprovisionamiento a petición,  escriba el **nombre distintivo** de un usuario y haga clic en el botón **Aprovisionar**.  
 
 ![Aprovisionamiento a petición](media/how-to-on-demand-provision/on-demand3.png)
7. Una vez finalizado, verá una pantalla de éxito y 4 casillas de color verde que indican que el elemento se ha aprovisionado correctamente.  Si hay errores, estos aparecerán a la izquierda.

  ![Correcto](media/how-to-on-demand-provision/on-demand4.png)

Ahora puede revisar el usuario y determinar si se han aplicado los cambios realizados en la configuración.  En el resto de este documento se describen las secciones individuales en las que se muestran los detalles de un usuario que se ha sincronizado correctamente.

## <a name="import-user-details"></a>Importación de los detalles de usuario
En esta sección se proporciona información sobre el usuario que se importó desde Active Directory.  Este es el aspecto del usuario antes de aprovisionarlo en Azure AD.  Haga clic en el vínculo **Ver detalles** para mostrar esta información.

![Importación de usuario](media/how-to-on-demand-provision/on-demand5.png)

Con esta información, puede ver los distintos atributos que se importaron y sus valores.  Si ha creado una asignación de atributos personalizados, podrá ver su valor aquí.
![Importación de los detalles de usuario](media/how-to-on-demand-provision/on-demand6.png)

## <a name="determine-if-user-is-in-scope-details"></a>Determinación de si el usuario está en los detalles del ámbito
En esta sección se proporciona información sobre si el usuario que se importó en Azure AD está dentro del ámbito.  Haga clic en el vínculo **Ver detalles** para mostrar esta información.

![Ámbito de usuario](media/how-to-on-demand-provision/on-demand7.png)

Con esta información, puede ver información adicional sobre el ámbito de sus usuarios.

![Detalles de ámbito del usuario](media/how-to-on-demand-provision/on-demand10a.png)

## <a name="match-user-between-source-and-target-system-details"></a>Coincidencia de usuario entre los detalles del sistema de origen y los de destino
En esta sección se proporciona información sobre si el usuario ya existe en Azure AD y si se debería combinar en lugar de aprovisionar un nuevo usuario.  Haga clic en el vínculo **Ver detalles** para mostrar esta información.
![Ver detalles](media/how-to-on-demand-provision/on-demand8.png)

Con esta información, puede ver si se encontró una coincidencia o si se va a crear un nuevo usuario.

![Información de usuario](media/how-to-on-demand-provision/on-demand11.png)

Los detalles de la búsqueda de coincidencias mostrarán un mensaje con una de las tres operaciones siguientes.  Son las siguientes:
- Create: se crea un usuario en Azure AD.
- Update: se actualiza un usuario en función de un cambio realizado en la configuración.
- Delete: se quita un usuario de Azure AD.

Dependiendo del tipo de operación que haya realizado, el mensaje variará.

## <a name="perform-action-details"></a>Realización de los detalles de la acción
En esta sección se proporciona información sobre el usuario que se aprovisionó o exportó en Azure AD después de aplicar la configuración.  Este es el aspecto del usuario una vez aprovisionado en Azure AD.  Haga clic en el vínculo **Ver detalles** para mostrar esta información.
![Realización de los detalles de la acción](media/how-to-on-demand-provision/on-demand9.png)

Con esta información, puede ver los valores de los atributos una vez aplicada la configuración.  ¿Tienen un aspecto similar al que se importó o son los diferentes?  ¿La configuración se aplicó correctamente?  

Esto le permitirá realizar un seguimiento de la transformación de los atributos mientras se desplaza desde la nube al inquilino de Azure AD.

![atributo Trace](media/how-to-on-demand-provision/on-demand12.png)

## <a name="next-steps"></a>Pasos siguientes 

- [¿Qué es el aprovisionamiento en la nube de Azure AD Connect?](what-is-cloud-provisioning.md)
- [Cómo realizar la instalación del aprovisionamiento en la nube de Azure AD Connect](how-to-install.md)
 