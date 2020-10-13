---
title: Conectores de la interfaz de usuario de Synchronization Service Manager de Azure AD | Microsoft Docs
description: Conozca la pestaña Conectores de Synchronization Service Manager para Azure AD Connect.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 60f1d979-8e6d-4460-aaab-747fffedfc1e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 07/13/2017
ms.subservice: hybrid
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.collection: M365-identity-device-management
ms.openlocfilehash: d215c2e200308664f24daa28a0054c8f1bcfc09c
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91319884"
---
# <a name="using-connectors-with-the-azure-ad-connect-sync-service-manager"></a>Uso de conectores con Sync Service Manager de Azure AD Connect

![Captura de pantalla que muestra Sync Service Manager.](./media/how-to-connect-sync-service-manager-ui-connectors/connectors.png)

La pestaña Conectores se usa para administrar todos los sistemas a los que está conectado el motor de sincronización.

## <a name="connector-actions"></a>Acciones del conector
| Acción | Comentario |
| --- | --- |
| Crear |No debe usarse. Para conectarse a los bosques de AD adicionales, use el Asistente para instalación. |
| Propiedades |Se usa para el filtrado por dominio y unidad organizativa. |
| [Eliminar](#delete) |Se usa para eliminar los datos en el espacio del conector o eliminar la conexión a un bosque. |
| [Configurar perfiles de ejecución](#configure-run-profiles) |A excepción del filtrado de dominio, no es necesario configurar ninguna otra opción. Puede utilizar esta acción para ver los perfiles de ejecución ya configurados. |
| Ejecute |Se usa para iniciar una ejecución única de un perfil. |
| Stop |Detiene un conector que esté ejecutando un perfil. |
| Exportar conector |No debe usarse. |
| Importar conector |No debe usarse. |
| Actualizar conector |No debe usarse. |
| Actualizar esquema |Actualiza el esquema en caché. Es preferible usar la opción en el Asistente para la instalación, ya que también actualizará las reglas de sincronización. |
| [Espacio del conector de búsqueda](#search-connector-space) |Se usa para buscar objetos y realizar un seguimiento de un objeto y sus datos por el sistema. |

### <a name="delete"></a>Eliminar
La acción de eliminación se usa para dos objetivos diferentes.  
![Captura de pantalla que muestra la ventana "Eliminación del conector" con la opción "Delete connector space only" (Eliminar solo el espacio del conector) seleccionada.](./media/how-to-connect-sync-service-manager-ui-connectors/connectordelete.png)

Con la opción **Delete connector space only** (Eliminar solo el espacio del conector) se eliminarán todos los datos, pero se mantendrá la configuración.

Con la opción **Delete Connector and connector space** (Eliminar el conector y el espacio del conector) se eliminarán los datos y la configuración. Esta opción se usa cuando no se desea volver a conectarse a un bosque.

Ambas sincronizarán todos los objetos y actualizarán los objetos del metaverso. Se trata de una operación de larga duración.

### <a name="configure-run-profiles"></a>Configurar perfiles de ejecución
Esta opción permite ver los perfiles de ejecución configurados para un conector.

![Captura de pantalla que muestra la ventana "Configure Run Profiles" (Configurar la ejecución de perfiles) con la opción "Delta Import" (Importación diferencial) seleccionada.](./media/how-to-connect-sync-service-manager-ui-connectors/configurerunprofiles.png)

### <a name="search-connector-space"></a>Espacio del conector de búsqueda
La acción del espacio del conector de búsqueda es útil para buscar objetos y solucionar problemas de datos.

![Captura de pantalla que muestra la ventana "Search Connector Space" (Espacio del conector de búsqueda).](./media/how-to-connect-sync-service-manager-ui-connectors/cssearch.png)

Empiece seleccionando un **ámbito**. Puede buscar según los datos (RDN, DN, delimitador, subárbol) o el estado del objeto (todas las demás opciones).  
![Captura de pantalla que muestra el menú desplegable "Ámbito".](./media/how-to-connect-sync-service-manager-ui-connectors/cssearchscope.png)  
Por ejemplo, si hace una búsqueda de un subárbol, obtiene todos los objetos de una unidad organizativa.  
![Captura de pantalla que muestra un ejemplo de una búsqueda de "subárbol".](./media/how-to-connect-sync-service-manager-ui-connectors/cssearchsubtree.png)  
En esta cuadrícula, puede seleccionar un objeto, elegir las **propiedades** y [realizar un seguimiento](tshoot-connect-object-not-syncing.md) desde el espacio del conector de origen, a través del metaverso, y hasta el espacio del conector de destino.

### <a name="changing-the-ad-ds-account-password"></a>Cambio de la contraseña de la cuenta de AD DS
Si cambia la contraseña de la cuenta, el servicio de sincronización ya no podrá importar o exportar cambios a las instancias locales de AD.   Verá lo siguiente:

- Se produce un error en el paso de importación y exportación para el conector de AD con el error "no-start-credentials".
- En el Visor de eventos de Windows, el registro de eventos de aplicación contiene un error con el identificador de evento 6000 y mensaje “El agente de administración "contoso.com" no se pudo ejecutar porque las credenciales no eran válidas”.

Para resolver el problema, actualice la cuenta de usuario de AD DS mediante lo siguiente:


1. Inicie el Synchronization Service Manager (INICIO → Synchronization Service).
</br>![Sync Service Manager](./media/how-to-connect-sync-service-manager-ui-connectors/startmenu.png)
2. Vaya a la pestaña **Conectores**.
3. Seleccione el conector de AD que está configurado para usar la cuenta de AD DS.
4. En Acciones, seleccione **Propiedades**.
5. En el cuadro de diálogo emergente, seleccione Connect to Active Directory Forest (Conectar con el bosque de Active Directory):
6. El nombre del bosque indica la instancia local de AD correspondiente.
7. El nombre de usuario indica la cuenta de AD DS que se usa para la sincronización.
8. Escriba la nueva contraseña de la cuenta de AD DS en el cuadro de texto Contraseña ![Utilidad de clave de cifrado de Azure AD Connect Sync](./media/how-to-connect-sync-service-manager-ui-connectors/key6.png).
9. Haga clic en Aceptar para guardar la nueva contraseña y reinicie Synchronization Service para quitar la contraseña antigua de la memoria caché.



## <a name="next-steps"></a>Pasos siguientes
Obtenga más información sobre la configuración de la [Sincronización de Azure AD Connect](how-to-connect-sync-whatis.md) .

Obtenga más información sobre la [Integración de las identidades locales con Azure Active Directory](whatis-hybrid-identity.md).
