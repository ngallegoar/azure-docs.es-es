---
title: Conexión a los recursos de un área de trabajo en Azure Synapse Analytics Studio desde una red restringida
description: En este artículo se explica cómo conectarse a los recursos de un área de trabajo desde una red restringida.
author: xujxu
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: security
ms.date: 10/25/2020
ms.author: xujiang1
ms.reviewer: jrasnick
ms.openlocfilehash: 7cff2d8245095489fbba3b7af24b416885995e4d
ms.sourcegitcommit: 295db318df10f20ae4aa71b5b03f7fb6cba15fc3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2020
ms.locfileid: "94637139"
---
# <a name="connect-to-workspace-resources-from-a-restricted-network"></a>Conexión a los recursos de un área de trabajo desde una red restringida

Imaginemos que es administrador de TI y debe administrar la red restringida de su organización. Quiere habilitar la conexión de red entre Azure Synapse Analytics Studio y una estación de trabajo dentro de esta red restringida. Este artículo le muestra cómo.

## <a name="prerequisites"></a>Prerrequisitos

* **Suscripción de Azure**: Si no tiene una suscripción a Azure, cree una [cuenta gratuita de Azure](https://azure.microsoft.com/free/) antes de empezar.
* **Área de trabajo de Azure Synapse Analytics**: puede crear una desde Azure Synapse Analytics. Necesita el nombre del área de trabajo en el paso 4.
* **Una red restringida**: el administrador de TI mantiene la red restringida para la organización y tiene permiso para configurar la directiva de la red. Necesita el nombre de la red virtual y la subred en el paso 3.


## <a name="step-1-add-network-outbound-security-rules-to-the-restricted-network"></a>Paso 1: Adición de reglas de seguridad de salida de red a la red restringida

Tendrá que agregar cuatro reglas de seguridad de salida de red con cuatro etiquetas de servicio. 
* AzureResourceManager
* AzureFrontDoor.Frontend
* AzureActiveDirectory
* AzureMonitor (Este tipo de regla es opcional. Agréguelo solo si quiere compartir los datos con Microsoft).

En la captura de pantalla siguiente se muestran los detalles de la regla de salida de Azure Resource Manager.

![Captura de pantalla de los detalles de la etiqueta de servicio de Azure Resource Manager.](./media/how-to-connect-to-workspace-from-restricted-network/arm-servicetag.png)

Al crear las otras tres reglas, reemplace el valor de **Etiqueta de servicio de destino** por **AzureFrontDoor.Frontend**, **AzureActiveDirectory** o **AzureMonitor** en la lista.

Para más información, consulte la [información general sobre las etiquetas de servicio](/azure/virtual-network/service-tags-overview.md).

## <a name="step-2-create-private-link-hubs"></a>Paso 2: Creación de centros de vínculos privados

Luego, cree centros de vínculos privados en Azure Portal. Para encontrar esta opción en el portal, busque *Azure Synapse Analytics (centros de vínculos privados)* y rellene la información necesaria para crearlo. 

> [!Note]
> Asegúrese de que el valor de **Región** sea el mismo que el de la región donde está el área de trabajo de Azure Synapse Analytics.

![Captura de pantalla de Crear un centro de vínculo privado de Synapse.](./media/how-to-connect-to-workspace-from-restricted-network/private-links.png)

## <a name="step-3-create-a-private-endpoint-for-your-gateway"></a>Paso 3: Creación de un punto de conexión privado para una puerta de enlace

Para acceder a la puerta de enlace de Azure Synapse Analytics Studio, debe crear un punto de conexión privado desde Azure Portal. Para encontrar esta opción en el portal, busque *Private Link*. En **Private Link Center**, seleccione **Crear punto de conexión privado** y rellene la información necesaria para crearlo. 

> [!Note]
> Asegúrese de que el valor de **Región** sea el mismo que el de la región donde está el área de trabajo de Azure Synapse Analytics.

![Captura de pantalla de Crear un punto de conexión privado, pestaña Aspectos básicos.](./media/how-to-connect-to-workspace-from-restricted-network/plink-endpoint-1.png)

En la pestaña **Recurso**, elija el centro de vínculo privado que creó en el paso 2.

![Captura de pantalla de Crear un punto de conexión privado, pestaña Recurso.](./media/how-to-connect-to-workspace-from-restricted-network/plink-endpoint-2.png)

En la pestaña **Configuración**: 
* En **Red virtual**, seleccione el nombre de la red virtual restringida.
* En **Subred**, seleccione la subred de la red virtual restringida. 
* En **Integrar con la zona DNS privada**, seleccione **Sí**.

![Captura de pantalla de Crear un punto de conexión privado, pestaña Configuración.](./media/how-to-connect-to-workspace-from-restricted-network/plink-endpoint-3.png)

Una vez creado el punto de conexión de vínculo privado, puede acceder a la página de inicio de sesión de la herramienta web de Azure Synapse Analytics Studio. Sin embargo, todavía no puede acceder a los recursos dentro del área de trabajo. Para ello, debe completar el paso siguiente.

## <a name="step-4-create-private-endpoints-for-your-workspace-resource"></a>Paso 4: Creación de puntos de conexión privados para el recurso del área de trabajo

Para acceder a los recursos dentro del recurso del área de trabajo de Azure Synapse Analytics Studio, debe crear lo siguiente:

- Al menos un punto de conexión de vínculo privado con un tipo **Dev** de **Subrecurso de destino**.
- Dos otros puntos de conexión de vínculo privado opcionales con tipos de **Sql** o **SqlOnDemand**, en función de los recursos del área de trabajo a los que quiere acceder.

La creación de estos es similar a la creación del punto de conexión del paso anterior.  

En la pestaña **Recurso**:

* En **Tipo de recurso**, seleccione **Microsoft.Synapse/workspaces**.
* En **Recurso**, seleccione el nombre del área de trabajo que creó anteriormente.
* En **Subrecurso de destino**, seleccione el tipo de punto de conexión:
  * **Sql** es para la ejecución de consultas SQL en el grupo de SQL.
  * **SqlOnDemand** es para la ejecución de consultas integrada de SQL.
  * **Dev** es para acceder a todo lo demás dentro de las áreas de trabajo de Azure Synapse Analytics Studio. Debe crear al menos un punto de conexión de vínculo privado con este tipo.

![Captura de pantalla de Crear un punto de conexión privado, pestaña Recurso, área de trabajo.](./media/how-to-connect-to-workspace-from-restricted-network/plinks-endpoint-ws-1.png)


## <a name="step-5-create-private-endpoints-for-workspace-linked-storage"></a>Paso 5: Creación de puntos de conexión privados para un almacenamiento vinculado a un área de trabajo

Para acceder al almacenamiento vinculado con el explorador de almacenamiento en el área de trabajo de Azure Synapse Analytics Studio, debe crear un punto de conexión privado. Estos pasos son similares a los del paso 3. 

En la pestaña **Recurso**:
* En **Tipo de recurso**, seleccione **Microsoft.Synapse/storageAccounts**.
* En **Recurso**, seleccione el nombre de la cuenta de almacenamiento que creó anteriormente.
* En **Subrecurso de destino**, seleccione el tipo de punto de conexión:
  * **blob** es para Azure Blob Storage.
  * **dfs** es para Azure Data Lake Storage Gen2.

![Captura de pantalla de Crear un punto de conexión privado, pestaña Recurso, almacenamiento.](./media/how-to-connect-to-workspace-from-restricted-network/plink-endpoint-storage.png)

Ahora puede acceder al recurso de almacenamiento vinculado. Dentro de la red virtual, en el área de trabajo de Azure Synapse Analytics Studio, puede usar el explorador de almacenamiento para acceder al recurso de almacenamiento vinculado.

Puede habilitar una red virtual administrada para el área de trabajo, tal como se muestra en esta captura de pantalla:

![Captura de pantalla de Crear un área de trabajo de Synapse, con la opción Habilitar red virtual administrada resaltada.](./media/how-to-connect-to-workspace-from-restricted-network/ws-network-config.png)

Si quiere que su cuaderno acceda a los recursos de almacenamiento vinculados con una cuenta de almacenamiento determinada, agregue puntos de conexión privados administrados en Azure Synapse Analytics Studio. El nombre de la cuenta de almacenamiento debe coincidir con la cuenta a la que el cuaderno necesita acceder. Para más información, consulte [Creación de un punto de conexión privado administrado al origen de datos](./how-to-create-managed-private-endpoints.md).

Después de crear este punto de conexión, el estado de aprobación muestra el estado **Pendiente**. Solicite la aprobación del propietario de esta cuenta de almacenamiento, en la pestaña **Conexiones de punto de conexión privado** de esta cuenta de almacenamiento en Azure Portal. Una vez aprobada, el cuaderno podrá acceder a los recursos de almacenamiento vinculado en esta cuenta de almacenamiento.

Ahora, todo está listo. Puede acceder al recurso del área de trabajo de Azure Synapse Analytics Studio.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre la [red virtual del área de trabajo administrada](./synapse-workspace-managed-vnet.md).

Más información sobre los [puntos de conexión privados administrados](./synapse-workspace-managed-private-endpoints.md).
