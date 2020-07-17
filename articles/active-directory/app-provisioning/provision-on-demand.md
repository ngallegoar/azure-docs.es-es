---
title: Aprovisionamiento de un usuario a petición mediante Azure Active Directory
description: Sincronización forzada
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-provisioning
ms.workload: identity
ms.topic: how-to
ms.date: 06/23/2020
ms.author: mimart
ms.reviewer: arvinh
ms.openlocfilehash: 78a56b6a848139c47d7934a47decb126afe00b7a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85297535"
---
# <a name="on-demand-provisioning"></a>Aprovisionamiento a petición
El aprovisionamiento a petición permite aprovisionar un usuario en una aplicación en cuestión de segundos. Puede usar la capacidad para solucionar rápidamente problemas de configuración, validar expresiones que haya definido, probar filtros de ámbito, etc. 

## <a name="how-to-use-on-demand-provisioning"></a>Procedimiento para usar el aprovisionamiento a petición 

1. Inicie sesión en **Azure Portal**.
2. Vaya a **Aplicaciones empresariales**.
3. Seleccione la aplicación y vaya a la página de configuración del aprovisionamiento.
4. Configure el aprovisionamiento al proporcionar las credenciales de administrador.
5. Haga clic en **Aprovisionamiento a petición**.
6. Busque un usuario por nombre, apellidos, nombre para mostrar, nombre principal de usuario o correo electrónico.
7. Seleccione Aprovisionar en la parte inferior de la página.

## <a name="understanding-the-provisioning-steps"></a>Reconocimiento de los pasos de aprovisionamiento
La capacidad de aprovisionamiento a petición intenta mostrar los pasos que realiza el servicio de aprovisionamiento al aprovisionar un usuario. Normalmente, hay cinco pasos para aprovisionar un usuario y uno o varios de los pasos siguientes se muestran en la experiencia de aprovisionamiento a petición.

### <a name="step-1-test-connection"></a>Paso 1: Prueba de conexión
El servicio de aprovisionamiento intenta autorizar el acceso a la aplicación de destino mediante la solicitud de un "usuario de prueba". El servicio de aprovisionamiento espera una respuesta que indique que está autorizado a continuar con los pasos de aprovisionamiento. Este paso solo se muestra si se produce un error en el paso. No se muestra en la experiencia de aprovisionamiento a petición si el paso se realiza correctamente. 

**Sugerencias para la solución de problemas**
* Asegúrese de que ha proporcionado credenciales válidas a la aplicación de destino, como el token secreto y la dirección URL del inquilino. Las credenciales requeridas varían según la aplicación. Se pueden encontrar tutoriales de configuración detallados [aquí](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list). 
* Asegúrese de que la aplicación de destino admite el filtrado por los atributos emparejados definidos en la hoja de asignaciones de atributos. Es posible que deba consultar la documentación de la API proporcionada por el desarrollador de la aplicación a fin de entender los filtros que admite.  
* En el caso de las aplicaciones SCIM, puede usar una herramienta como Postman para asegurarse de que la aplicación responda a las solicitudes de autorización como espera el servicio de aprovisionamiento de Azure AD. Se puede encontrar un ejemplo de solicitud [aquí](https://docs.microsoft.com/azure/active-directory/app-provisioning/use-scim-to-provision-users-and-groups#request-3).

### <a name="step-2-import-user"></a>Paso 2: Importación de usuario
Después, el servicio de aprovisionamiento recupera el usuario desde el sistema de origen. Los atributos de usuario que el servicio recupera se usan más adelante para evaluar si el usuario está en el ámbito del aprovisionamiento, buscar un usuario existente en el sistema de destino y determinar qué atributos de usuario se van a exportar al sistema de destino. 

**Ver detalles**

En la sección Ver detalles se muestran las propiedades del usuario que se han importado desde el sistema de origen (por ejemplo, Azure AD).

**Sugerencias para la solución de problemas**
* Se puede producir un error al importar el usuario si falta el atributo emparejado en el objeto de usuario del sistema de origen. Puede resolver este error si actualiza el objeto de usuario con un valor para el atributo emparejado o si cambia el atributo emparejado en la configuración de aprovisionamiento.  
* Si falta un atributo que esperaba en la lista que se ha importado, asegúrese de que el atributo tenga un valor en el objeto de usuario del sistema de origen. El servicio de aprovisionamiento no admite actualmente el aprovisionamiento de atributos NULL. 
* Asegúrese de que la página de asignación de atributos de configuración de aprovisionamiento contiene el atributo que espera. 

### <a name="step-3-determine-if-user-is-in-scope"></a>Paso 3: Determinación de si el usuario está en el ámbito
Después, el servicio de aprovisionamiento determina si el usuario está en el [ámbito](https://docs.microsoft.com/azure/active-directory/app-provisioning/how-provisioning-works#scoping) del aprovisionamiento. El servicio tiene en cuenta aspectos como si el usuario está asignado a la aplicación, si el ámbito está establecido para sincronizar asignados o sincronizar todo, y los filtros de ámbito definidos en la configuración de aprovisionamiento.  

**Ver detalles**

En la sección Ver detalles se muestran las condiciones de ámbito que se han evaluado. Puede ver una o varias de las propiedades siguientes:
* **Activo en sistema de origen** indica que el usuario tiene la propiedad is active establecida en true en Azure AD.
* **Asignado a aplicación** indica que el usuario está asignado a la aplicación en Azure AD.
* **Ámbito sincronizar todo** indica que la configuración de ámbito permite todos los usuarios y grupos del inquilino.
* **Usuario tiene rol requerido** indica que el usuario tiene los roles necesarios para aprovisionarse en la aplicación. 
* **Filtros de ámbito** también aparece si se han definido filtros de ámbito para la aplicación. El filtro se muestra con el siguiente formato: {título de filtro de ámbito} {atributo de filtro de ámbito} {operador de filtro de ámbito} {valor de filtro de ámbito}. 

**Sugerencias para la solución de problemas**
* Asegúrese de que ha definido un rol de ámbito válido. Por ejemplo, evite usar el operador ["Mayor que"](https://docs.microsoft.com/azure/active-directory/app-provisioning/define-conditional-rules-for-provisioning-user-accounts#create-a-scoping-filter) con un valor no entero. 
* Si el usuario no tiene el rol necesario, revise las sugerencias indicadas [aquí](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-config-problem-no-users-provisioned#provisioning-users-assigned-to-the-default-access-role). 

### <a name="step-4-match-user-between-source-and-target"></a>Paso 4: Emparejamiento del usuario entre origen y destino
En este paso, el servicio intenta emparejar el usuario que se ha recuperado en el paso de importación con un usuario del sistema de destino. 

**Ver detalles**

En la página Ver detalles se muestran las propiedades de los usuarios que se han emparejado en el sistema de destino. Las propiedades que se ven en el panel de contexto varían así:
* Si no hay usuarios emparejados en el sistema de destino, no se ve ninguna propiedad.
* Si hay un usuario emparejado en el sistema de destino, se ven las propiedades de ese usuario emparejado del sistema de destino.
* Si hay varios usuarios emparejados, se ven las propiedades de ambos usuarios emparejados.
* Si hay varios atributos emparejados que forman parte de las asignaciones de atributos, cada atributo emparejado se evalúa secuencialmente y se muestran los usuarios emparejados. 

**Detalles de solución de problemas**
* El servicio de aprovisionamiento no puede emparejar de forma única a un usuario del origen con un usuario del destino. Esto se puede resolver al asegurarse de que el atributo emparejado sea único. 
* Asegúrese de que la aplicación de destino admite el filtrado por el atributo definido como atributo emparejado.  

### <a name="step-5-perform-action"></a>Paso 5: Realización de acción
Por último, el servicio de aprovisionamiento realiza una acción, como crear, actualizar, eliminar u omitir el usuario. 

**Ver detalles**

La sección Ver detalles muestra los atributos que se han modificado en la aplicación de destino. Esto representa la salida final de la actividad del servicio de aprovisionamiento y los atributos que se han exportado. Si se produce un error en este paso, los atributos mostrados representan los atributos que el servicio de aprovisionamiento ha intentado modificar.  

**Sugerencias para la solución de problemas**
* Los errores para exportar cambios pueden variar considerablemente. Vea la [documentación](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs#error-codes) de los registros de aprovisionamiento para conocer los errores habituales.


## <a name="frequently-asked-questions"></a>Preguntas más frecuentes
**¿Es necesario desactivar el aprovisionamiento para usar el aprovisionamiento a petición?** En las aplicaciones que usan un token de portador de larga duración o un nombre de usuario y una contraseña para la autorización no se necesitan pasos adicionales. Las aplicaciones que usan OAuth para la autorización actualmente requieren que el trabajo de aprovisionamiento se detenga para usar el aprovisionamiento a petición. En esta categoría se encuentran aplicaciones como G Suite, Box, Workplace by Facebook y Slack. Se está trabajando para permitir la ejecución del aprovisionamiento a petición para todas las aplicaciones, sin necesidad de detener el aprovisionamiento. 

**¿Cuánto tarda el aprovisionamiento a petición?** Normalmente tarda menos de 30 segundos. 

## <a name="known-limitations"></a>Limitaciones conocidas
Hoy en día hay algunas limitaciones conocidas. Publique en [UserVoice](https://aka.ms/appprovisioningfeaturerequest) para clasificar mejor por orden de prioridad las mejoras que se van a realizar a continuación. Tenga en cuenta que estas limitaciones son específicas de la capacidad de aprovisionamiento a petición. Para obtener detalles sobre si una aplicación admite el aprovisionamiento de grupos, las eliminaciones, etc., vea el tutorial de la aplicación. 

* Las aplicaciones WorkDay, AWS y SuccessFactors no admiten el aprovisionamiento a petición.
* No se admite el aprovisionamiento de grupos y roles a petición.
* No se admite la deshabilitación ni la eliminación de usuarios y grupos.

## <a name="next-steps"></a>Pasos siguientes

* [Solución de problemas de aprovisionamiento](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-config-problem)
