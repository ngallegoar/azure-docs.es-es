---
title: Solución de problemas de un objeto que no se está sincronizando con Azure Active Directory | Microsoft Docs
description: Solución de problemas de un objeto que no se está sincronizando con Azure Active Directory.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 08/10/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 77895fd81bd37e304d422aea64da0298fc1673ff
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "85356481"
---
# <a name="troubleshoot-an-object-that-is-not-synchronizing-with-azure-active-directory"></a>Solución de problemas de un objeto que no se está sincronizando con Azure Active Directory

Si un objeto no está sincronizando según lo previsto con Microsoft Azure Active Directory (Azure AD), puede ser debido a varias razones. Si ha recibido un de correo electrónico de error de Azure AD o si ve el error en Azure AD Connect Health, lea [Solución de errores durante la sincronización](tshoot-connect-sync-errors.md). Sin embargo, si va a solucionar un problema en el que el objeto no está en Azure AD, este es el artículo que tiene que leer. Describe cómo encontrar errores en la sincronización de Azure AD Connect de componentes locales.

>[!IMPORTANT]
>Para la implementación de Azure AD Connect con la versión 1.1.749.0 o superior, use la [tarea de solución de problemas](tshoot-connect-objectsync.md) del asistente para solucionar problemas de sincronización de objetos. 

## <a name="synchronization-process"></a>Proceso de sincronización

Antes de que investiguemos los problemas de sincronización de un atributo, revisemos el proceso de sincronización de Azure AD Connect:

  ![Diagrama del proceso de sincronización de Azure AD Connect](./media/tshoot-connect-object-not-syncing/syncingprocess.png)

### <a name="terminology"></a>**Terminología**

* **CS:** espacio conector, una tabla de una base de datos.
* **MV:** metaverso, una tabla de una base de datos.

### <a name="synchronization-steps"></a>**Pasos de sincronización**
el proceso de sincronización realiza los pasos siguientes:

1. **Importar desde AD:** los objetos de Active Directory se incorporan al espacio conector de Active Directory.

2. **Importar desde Azure AD:** los objetos de Azure AD se incorporan al espacio conector de Azure AD.

3. **Sincronización:** las reglas de sincronización entrantes y las reglas de sincronización salientes se ejecutan de menor a mayor según el número de orden de prioridad. Para ver las reglas de sincronización, vaya al Editor de reglas de sincronización desde las aplicaciones de escritorio. Las reglas de sincronización entrantes transfieren los datos del espacio conector al metaverso. Las reglas de sincronización salientes transfieren los datos del metaverso al espacio conector.

4. **Exportar a AD:** después de la sincronización, objetos se exportan desde el espacio conector de Active Directory a Active Directory.

5. **Exportar a Azure AD:** después de la sincronización, los objetos se exportan desde el espacio conector de Azure AD a Azure AD.

## <a name="troubleshooting"></a>Solución de problemas

Para encontrar los errores, examine varios sitios en el orden siguiente:

1. Los [registros de operaciones](#operations) para buscar errores que ha identificado el motor de sincronización durante la importación y sincronización.
2. El [espacio conector](#connector-space-object-properties) para buscar objetos que faltan y errores de sincronización.
3. El [metaverso](#metaverse-object-properties) para buscar problemas relacionados con los datos.

Inicie [Synchronization Service Manager](how-to-connect-sync-service-manager-ui.md) antes de comenzar a realizar estos pasos.

## <a name="operations"></a>Operaciones
La pestaña **Operaciones** de Synchronization Service Manager es donde debe comenzar la solucionar problemas. En esta pestaña se muestran los resultados de las operaciones más recientes. 

![Captura de pantalla de Synchronization Service Manager en la que se muestra la pestaña Operaciones seleccionada](./media/tshoot-connect-object-not-syncing/operations.png)  

La mitad superior de la pestaña **Operaciones** muestra todas las ejecuciones en orden cronológico. De forma predeterminada, el registro de operaciones conservará información sobre los últimos 7 días, pero puede cambiar este parámetro con el [programador](how-to-connect-sync-feature-scheduler.md). Busque las ejecuciones que no muestran un estado de **operación correcta**. Para cambiar la ordenación, haga clic en los encabezados.

La columna **Estado** contiene la información más importante, puesto que muestra el problema más grave de una ejecución. A continuación se muestra un resumen rápido de los estados más comunes en orden de prioridad de investigación (donde * indica varias cadenas de error posibles).

| Status | Comentario |
| --- | --- |
| stopped-* |La ejecución no pudo finalizar. Esto podría ocurrir, por ejemplo, si el sistema remoto está inactivo y no se puede conectar a él. |
| stopped-error-limit |Se han generado más de 5000 errores. La ejecución se ha detenido automáticamente debido al elevado número de errores. |
| completed-\*-errors |La ejecución finaliza, pero hay errores (menos de 5000) que deben investigarse. |
| completed-\*-warnings |La ejecución finalizó, pero algunos datos no tienen el estado esperado. Si se producen errores, este mensaje suele ser únicamente un síntoma. No investigue las advertencias hasta que haya resuelto los errores. |
| success |No hay ningún problema. |

Cuando seleccione una fila, la parte inferior de la pestaña **Operaciones** se actualizará para mostrar los detalles de la ejecución. En el lado izquierdo de esta área, es posible que tenga una lista denominada **Paso nº**. Esta lista solo aparecerá si tiene varios dominios en el bosque; cada dominio estará representado por un paso. El nombre de dominio puede encontrarse bajo el encabezado **Partición**. En el encabezado **Synchronization Statistics**(Estadísticas de sincronización) puede encontrar más información sobre el número de cambios que se han procesado. Seleccione los vínculos para obtener una lista de los objetos modificados. Si hay objetos con errores, estos se mostrarán en el encabezado **Errores de sincronización**.

### <a name="errors-on-the-operations-tab"></a>Errores en la pestaña Operaciones
Cuando se producen errores, Synchronization Service Manager muestra tanto el objeto del error como el propio error como vínculos que proporcionan información adicional.

![Captura de pantalla de errores en Synchronization Service Manager](./media/tshoot-connect-object-not-syncing/errorsync.png)  
Empiece seleccionando la cadena de error. (En la figura anterior, la cadena de error es **sync-rule-error-function-triggered**). En primer lugar, aparecerá información general del objeto. Para ver el error real, seleccione **Seguimiento de la pila**. Este seguimiento proporciona información de depuración del error.

Puede hacer clic con el botón derecho en la casilla **Call Stack Information** (Información de la pila de llamadas), hacer clic en **Seleccionar todo** y luego en **Copiar**. Después, copie la pila y busque el error en el editor que prefiera, como puede ser el Bloc de notas.

Si el error procede de **SyncRulesEngine**, la información de la pila de llamadas enumerará en primer lugar todos los atributos del objeto. Desplácese hacia abajo hasta que vea el encabezado **InnerException = >** .  

  ![Captura de pantalla de Synchronization Service Manager, que muestra información de error en el encabezado InnerException = >](./media/tshoot-connect-object-not-syncing/errorinnerexception.png)
  
La línea posterior al encabezado muestra el error. En la figura anterior, el error procede de una regla de sincronización personalizados que creó Fabrikam.

Si el error no proporciona suficiente información, puede fijarse en los datos. Seleccione el vínculo con el identificador de objeto y continúe solucionando el problema con el [objeto importado del espacio conector](#cs-import).

## <a name="connector-space-object-properties"></a>Propiedades de objeto del espacio del conector
Si la pestaña [**Operaciones**](#operations) no muestra ningún error, siga el objeto del espacio conector desde Active Directory hasta el metaverso a Azure AD. En esta ruta de acceso, debería encontrar dónde está el problema.

### <a name="searching-for-an-object-in-the-cs"></a>Búsqueda de un objeto en el espacio conector

En Synchronization Service Manager, seleccione **Conectores**, seleccione el Conector Active Directory y elija **Search Connector Space** (Buscar espacio conector).

En el cuadro **Ámbito**, seleccione **RDN** cuando quiera realizar una búsqueda en el atributo CN o **DN or anchor** (DN o delimitador cuando desee buscar en el atributo **distinguishedName**. Escriba un valor y seleccione **Buscar**. 
 
![Captura de pantalla de una búsqueda de espacio conector](./media/tshoot-connect-object-not-syncing/cssearch.png)  

Si no encuentra el objeto que está buscando, podría haberse filtrado con un [filtrado basado en dominios](how-to-connect-sync-configure-filtering.md#domain-based-filtering) o uno [basado en la unidad organizativa](how-to-connect-sync-configure-filtering.md#organizational-unitbased-filtering). Para comprobar que el filtrado se ha configurado según lo previsto, lea [Sincronización de Azure AD Connect: Configuración del filtrado](how-to-connect-sync-configure-filtering.md).

Puede realizar otra búsqueda útil seleccionando Conector Azure AD. En el cuadro **Ámbito**, seleccione **Pending Import** (Importación pendiente) y luego seleccione la casilla **Agregar**. Esta búsqueda proporciona todos los objetos sincronizados con Azure AD que no se pueden asociar con un objeto local.  

![Captura de pantalla de huérfanos de una búsqueda de espacio conector](./media/tshoot-connect-object-not-syncing/cssearchorphan.png) 
 
Esos objetos los crearon otro motor de sincronización o un motor de sincronización con una configuración de filtrado diferente. Estos objetos huérfanos ya no se administran. Revise esta lista y considere la posibilidad de quitar estos objetos mediante cmdlets de [Azure AD PowerShell](https://aka.ms/aadposh).

### <a name="cs-import"></a>Importación del espacio conector
Al abrir un objeto del espacio conector, aparecen varias pestañas en la parte superior. La pestaña **Importación** muestra los datos que se almacenan provisionalmente después de una importación.  

![Captura de pantalla de la ventana Connector Space Object Properties (Propiedades del objeto del espacio conector), con la pestaña Importar seleccionada](./media/tshoot-connect-object-not-syncing/csobject.png)    

En la columna **Valor antiguo** se muestran los datos almacenados en Connect; en la columna **Nuevo valor** se muestran los que se han recibido desde el sistema de origen y no se han aplicado todavía. Si se produce un error en el objeto, los cambios no se procesan.

La pestaña **Synchronization Error** (Error de sincronización) solo aparece en la ventana **Connector Space Object Properties** (Propiedades del objeto del espacio conector) solo si hay un problema con el objeto. Para más información, consulte cómo [solucionar problemas de errores de sincronización en la pestaña **Operaciones**](#errors-on-the-operations-tab).

![Captura de pantalla de la pestaña Synchronization Error (Error de sincronización) en la ventana Connector Space Object Properties (Propiedades del objeto del espacio conector)](./media/tshoot-connect-object-not-syncing/cssyncerror.png)  

### <a name="cs-lineage"></a>Linaje del espacio conector
La pestaña **Linaje** de la ventana **Connector Space Object Properties** (Propiedades del objeto del espacio conector) muestra cómo el objeto de espacio del conector está relacionado con el objeto de metaverso. Puede ver cuándo fue la última vez que el conector importó un cambio desde el sistema conectado y qué reglas que se han aplicado para rellenar los datos en el metaverso.  

![Captura de pantalla que muestra la pestaña Lineage (Linaje) en la ventana Connector Space Object Properties (Propiedades del objeto del espacio conector)](./media/tshoot-connect-object-not-syncing/cslineage.png)  

En la figura anterior, en la columna **Acción** se muestra una regla de sincronización entrante con la acción **Aprovisionar**. Esto indica que, siempre que este objeto de espacio del conector esté presente, el objeto de metaverso permanecerá. En cambio, si la lista de reglas de sincronización muestra una regla de sincronización saliente con una acción **Aprovisionar**, indica que este objeto se eliminará cuando se quite el objeto de metaverso.  

![Captura de pantalla de la ventana de linaje en la pestaña Lineage (Linaje) de la ventana Connector Space Object Properties (Propiedades del objeto del espacio conector)](./media/tshoot-connect-object-not-syncing/cslineageout.png)  

En la figura anterior, también puede ver en la columna **PasswordSync** que el espacio del conector entrante puede contribuir a los cambios realizados en la contraseña debido a que una regla de sincronización tiene el valor **True**. Esta contraseña se envía a Azure AD a través de la regla de salida.

Desde la pestaña **Lineage** (Linaje), puede obtener el metaverso seleccionando [**Metaverse Object Properties**](#mv-attributes) (Propiedades del objeto de metaverso).

### <a name="preview"></a>Vista previa
En la esquina inferior izquierda de la ventana **Connector Space Object Properties** (Propiedades del objeto del espacio conector) se encuentra el botón **Vista previa**. Seleccione este botón para abrir la página **Vista previa**, donde puede sincronizar un único objeto. Esta página es útil si está solucionando problemas de algunas reglas de sincronización personalizadas del cliente y desea ver el efecto de un cambio en un único objeto. Puede seleccionar **Sincronización completa** o **Sincronización diferencial**. También puede seleccionar **Generate Preview** (Generar vista previa), que únicamente mantendrá el cambio en la memoria. O bien seleccione **Vista previa de confirmación**, que actualiza el metaverso y llevará a cabo todos los cambios en los espacios conectores de destino.  

![Captura de pantalla de la página Vista previa con Iniciar vista previa seleccionado](./media/tshoot-connect-object-not-syncing/preview.png)  

En la vista previa puede inspeccionar el objeto y ver qué regla se aplicó para un flujo de atributo concreto.  

![Captura de pantalla de la página Vista previa en la que se muestra Import Attribute Flow (Importar flujo de atributo)](./media/tshoot-connect-object-not-syncing/previewresult.png)

### <a name="log"></a>Log
Junto al botón **Vista previa**, seleccione el botón **Registro** para abrir la página **Registro**. Aquí puede ver el estado y el historial de la sincronización de contraseñas. Para más información, consulte [Solución de problemas de sincronización de hash de contraseñas con la sincronización de Azure AD Connect](tshoot-connect-password-hash-synchronization.md).

## <a name="metaverse-object-properties"></a>Propiedades de objetos del metaverso
Se recomienda iniciar la búsqueda desde el espacio conector de Active Directory de origen. Pero también puede iniciar la búsqueda en el metaverso.

### <a name="searching-for-an-object-in-the-mv"></a>Búsqueda de un objeto en el metaverso
En Synchronization Service Manager, seleccione **Metaverse Search** (Búsqueda de metaverso), como en la siguiente figura. Cree una consulta que sabe que encuentra al usuario. Busque atributos comunes, como **accountName** (**sAMAccountName**) y **userPrincipalName**. Para más información, consulte [Búsqueda de metaverso de Synchronization Service Manager](how-to-connect-sync-service-manager-ui-mvsearch.md).

![Captura de pantalla de Synchronization Service Manager, con la pestaña Metaverse Search (Búsqueda de metaverso) seleccionada](./media/tshoot-connect-object-not-syncing/mvsearch.png)  

En la ventana **Resultados de la búsqueda**, haga clic en el objeto.

Si no se encuentra el objeto, este no ha alcanzado el metaverso. Continúe buscando el objeto en el [espacio conector](#connector-space-object-properties) de Active Directory. Si encuentra el objeto en el espacio conector de Active Directory, podría haber un error de sincronización que impide que el objeto llegue al metaverso o podría estarse aplicando un filtro de ámbito de regla de sincronización.

### <a name="object-not-found-in-the-mv"></a>Objeto no encontrado en el metaverso
Si el objeto está en el espacio conector de Active Directory pero no está presente en el metaverso, está aplicado un filtro de ámbito. Para ver el filtro de ámbito, vaya al menú de aplicación de escritorio y seleccione **Synchronization Rules Editor** (Editor de reglas de sincronización). Filtre las reglas que se aplican al objeto ajustando el siguiente filtro.

  ![Captura de pantalla de Synchronization Rules Editor (Editor de reglas de sincronización), que muestra una búsqueda de reglas de sincronización de entrada](./media/tshoot-connect-object-not-syncing/syncrulessearch.png)

Observe todas las reglas de la lista anterior y compruebe el **Filtro de ámbito**. En el siguiente filtro de ámbito, si el valor de **isCriticalSystemObject** es null, FALSE o está vacío, está en el ámbito.

  ![Captura de pantalla de un filtro de ámbito en una búsqueda de reglas de sincronización de entrada](./media/tshoot-connect-object-not-syncing/scopingfilter.png)

Vaya a la lista de atributos de la sección [Importación del espacio conector](#cs-import) y compruebe cuál de los filtros está evitando que el objeto se mueva al metaverso. La lista de atributos del **espacio conector** mostrará solo los atributos que no sean null ni estén vacíos. Por ejemplo, si **isCriticalSystemObject** no aparece en la lista, el valor de este atributo es null o está vacío.

### <a name="object-not-found-in-the-azure-ad-cs"></a>Objeto no encontrado en el espacio conector de Azure AD
Si el objeto no está presente en espacio conector de Azure AD pero sí lo está en el metaverso, busque el filtro de ámbito de las reglas salientes del espacio conector correspondiente y averigüe si el objeto no aparece debido a que los [atributos del metaverso](#mv-attributes) no cumplen los criterios.

Para ver el filtro de ámbito saliente, seleccione las reglas que se aplican al objeto ajustando el siguiente filtro. Vea cada regla y examine el valor de [atributos del metaverso](#mv-attributes) correspondiente.

  ![Captura de pantalla de una búsqueda de reglas de sincronización de salida en Synchronization Rules Editor (Editor de reglas de sincronización)](./media/tshoot-connect-object-not-syncing/outboundfilter.png)


### <a name="mv-attributes"></a>Atributos del metaverso
En la pestaña **Atributos** puede ver los valores y qué conectores los aportaron.  

![Captura de pantalla de la ventana Metaverse Object Properties (Propiedades del objeto de metaverso), con la pestaña Atributos seleccionada](./media/tshoot-connect-object-not-syncing/mvobject.png)  

Si un objeto no se está sincronizando, formule la siguiente pregunta sobre los estados de atributo en el metaverso:
- ¿Está presente el atributo **cloudFiltered** y está establecido en **True**? En caso afirmativo, se ha filtrado según los pasos descritos en el [Filtrado basado en atributos](how-to-connect-sync-configure-filtering.md#attribute-based-filtering).
- ¿Está presente el atributo **sourceAnchor**? En caso negativo, ¿tiene una topología de bosque de cuenta-recurso? Si un objeto se identifica como un buzón vinculado (el atributo **msExchRecipientTypeDetails** tiene el valor **2**), el valor de **sourceAnchor** lo proporcionará el bosque con una cuenta de Active Directory habilitada. Asegúrese de que la cuenta maestra se ha importado y sincronizado correctamente. La cuenta maestra debe aparecer entre los [conectores](#mv-connectors) del objeto.

### <a name="mv-connectors"></a>Conectores del metaverso
La pestaña **Conectores** muestra todos los espacios del conector que tienen una representación del objeto. 
 
![Captura de pantalla de la ventana Metaverse Object Properties (Propiedades del objeto de metaverso), con la pestaña Conectores seleccionada](./media/tshoot-connect-object-not-syncing/mvconnectors.png)  

Debe tener un conector para los siguientes elementos:

- Todos los bosques de Active Directory donde esté representado el usuario. Esta representación puede incluir **foreignSecurityPrincipals** y objetos de **contacto**.
- Un conector de Azure AD.

Si falta el conector en Azure AD, revise la sección de [Atributos del metaverso](#mv-attributes) para comprobar los criterios de aprovisionamiento en Azure AD.

Desde la pestaña **Conectores** también puede ir al [objeto del espacio conector](#connector-space-object-properties). Seleccione una fila y haga clic en **Propiedades**.

## <a name="next-steps"></a>Pasos siguientes
- Más información sobre la [sincronización de Azure AD Connect](how-to-connect-sync-whatis.md).
- Más información sobre la [identidad híbrida](whatis-hybrid-identity.md).
