---
title: Conexión a un recurso de área de trabajo de Synapse Studio desde una red restringida
description: En este artículo se explica cómo conectarse a recursos de un área de trabajo de Azure Synapse Studio desde una red restringida.
author: xujxu
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: security
ms.date: 10/25/2020
ms.author: xujiang1
ms.reviewer: jrasnick
ms.openlocfilehash: f2d8953ccae1057d7a7aa2d786fb7b641b3f6284
ms.sourcegitcommit: 0ce1ccdb34ad60321a647c691b0cff3b9d7a39c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2020
ms.locfileid: "93392532"
---
# <a name="connect-to-synapse-studio-workspace-resources-from-a-restricted-network"></a>Conexión a recursos de un área de trabajo de Synapse Studio desde una red restringida

Este artículo está dirigido a los administradores de TI de empresa que administran la red restringida de la empresa. El administrador de TI está a punto de habilitar la conexión de red entre Azure Synapse Studio y la estación de trabajo dentro de esta red restringida.

En este artículo, aprenderá a conectarse a su área de trabajo de Azure Synapse desde un entorno de red restringida. 

## <a name="prerequisites"></a>Prerrequisitos

* **Suscripción de Azure**: Si no tiene una suscripción a Azure, cree una [cuenta gratuita de Azure](https://azure.microsoft.com/free/) antes de empezar.
* **Área de trabajo de Azure Synapse**: Si no tiene una instancia de Synapse Studio, cree un área de trabajo de Synapse desde Azure Synapse Analytics. El nombre del área de trabajo se necesitará en el paso 4 a continuación.
* **Una red restringida**: El administrador de TI de la empresa mantiene la red restringida. El administrador de TI tiene permiso para configurar la directiva de red. El nombre de la red virtual y su subred serán necesarios en el paso 3 a continuación.


## <a name="step-1-add-network-outbound-security-rules-to-the-restricted-network"></a>Paso 1: Adición de reglas de seguridad de salida de red a la red restringida

Tendrá que agregar cuatro reglas de seguridad de salida de red con cuatro etiquetas de servicio. Obtenga [información general sobre las etiquetas de servicio](/azure/virtual-network/service-tags-overview.md). 
* AzureResourceManager
* AzureFrontDoor.Frontend
* AzureActiveDirectory
* AzureMonitor (opcional. Agregue este tipo de regla solo si quiere compartir los datos con Microsoft).

Los detalles de la regla de salida de **Azure Resource Manager** son como se indica a continuación. Al crear las otras tres reglas, sustituya el valor de "**Etiqueta de servicio de destino**" por la elección del nombre de la etiqueta de servicio "**AzureFrontDoor.Frontend**", "**AzureActiveDirectory**", "**AzureMonitor**" en la lista de selección desplegable.

![AzureResourceManager](./media/how-to-connect-to-workspace-from-restricted-network/arm-servicetag.png)


## <a name="step-2-create-azure-synapse-analytics-private-link-hubs"></a>Paso 2: Creación de Azure Synapse Analytics (centros de vínculos privados)

Tendrá que crear una instancia de Azure Synapse Analytics (centros de vínculos privados) desde Azure Portal. Busque "**Azure Synapse Analytics (centros de vínculos privados)** " a través del Azure Portal y, a continuación, rellene el campo necesario y créela. 

> [!Note]
> La región debe ser la misma que la del área de trabajo de Synapse.

![Creación de centros de vínculos privados de Synapse Analytics](./media/how-to-connect-to-workspace-from-restricted-network/private-links.png)

## <a name="step-3-create-private-endpoint-for-synapse-studio-gateway"></a>Paso 3: Creación de un punto de conexión privado para la puerta de enlace de Synapse Studio

Para acceder a la puerta de enlace de Synapse Studio, deberá crear un punto de conexión privado desde Azure Portal. Busque "**Vínculo privado**" a través de Azure Portal. Seleccione **Crear punto de conexión privado** en **Private Link Center** y, a continuación, rellene el campo necesario y créelo. 

> [!Note]
> La región debe ser la misma que la del área de trabajo de Synapse.

![Creación de un punto de conexión privado para Synapse Studio 1](./media/how-to-connect-to-workspace-from-restricted-network/plink-endpoint-1.png)

En la pestaña siguiente de **Recurso**, elija el centro de vínculos privado que se creó en el paso 2 anterior.

![Creación de un punto de conexión privado para Synapse Studio 2](./media/how-to-connect-to-workspace-from-restricted-network/plink-endpoint-2.png)

En la siguiente pestaña de **Configuración**, 
* elija el nombre de la red virtual restringida que tiene para **Red virtual**.
* Elija la subred de la red virtual restringida para **Subred**. 
* Seleccione **Sí** para la **Integración con una zona DNS privada**.

![Creación de un punto de conexión privado para Synapse Studio 3](./media/how-to-connect-to-workspace-from-restricted-network/plink-endpoint-3.png)

Una vez creado el punto de conexión de vínculo privado, puede acceder a la página de inicio de sesión de la herramienta web de Synapse Studio. Sin embargo, no podrá acceder a los recursos dentro del área de trabajo de Synapse aún, tendrá que completar el paso siguiente.

## <a name="step-4-create-private-endpoints-for-synapse-studio-workspace-resource"></a>Paso 4: Creación de puntos de conexión privados para un recurso de área de trabajo de Synapse Studio

Para acceder a los recursos dentro del recurso del área de trabajo de Synapse Studio, tendrá que crear al menos un punto de conexión de vínculo privado con el tipo **Dev** de **Subrecurso de destino**, y otros dos puntos de conexión de vínculo privado opcionales con tipos **Sql** o **SqlOnDemand**, en función de los recursos del área de trabajo de Synapse Studio a los que quiera acceder. La creación de este punto de conexión de vínculo privado para el área de trabajo de Synapse Studio es similar a la creación del punto de conexión anterior.  

Preste atención a las áreas siguientes en la pestaña **Recurso**:
* Seleccione **Microsoft.Synapse/workspaces** en **Tipo de recurso**.
* Seleccione **NombreDelÁreaDeTrabajo** en el **Recurso** que ha creado antes.
* Seleccione el tipo de punto de conexión en **Subrecurso de destino**:
  * **Sql** es para la ejecución de consultas SQL en el grupo de SQL.
  * **SqlOnDemand** es para la ejecución de consultas integrada de SQL.
  * **Dev** es para acceder a todo lo demás dentro de las áreas de trabajo de Synapse Studio. Debe crear al menos el punto de conexión de vínculo privado con este tipo.

![Creación de un punto de conexión privado para el área de trabajo de Synapse Studio](./media/how-to-connect-to-workspace-from-restricted-network/plinks-endpoint-ws-1.png)


## <a name="step-5-create-private-endpoints-for-synapse-studio-workspace-linked-storage"></a>Paso 5: Creación de puntos de conexión privados para un almacenamiento vinculado a un área de trabajo de Synapse Studio

Para acceder al almacenamiento vinculado con el explorador de almacenamiento en el área de trabajo de Synapse Studio, deberá crear un punto de conexión privado con las instrucciones similares del paso 3 anterior. 

Preste atención a las áreas siguientes en la pestaña **Recurso**:
* Seleccione **Microsoft.Synapse/storageAccounts** en **Tipo de recurso**.
* Seleccione **NombreDelÁreaDeTrabajo** en el **Recurso** que ha creado antes.
* Seleccione el tipo de punto de conexión en **Subrecurso de destino**:
  * **blob**: es para el almacenamiento de blobs de Azure.
  * **dfs**: es para Azure Data Lake Storage Gen2.

![Creación de un punto de conexión privado para el almacenamiento vinculado a un área de trabajo de Synapse Studio](./media/how-to-connect-to-workspace-from-restricted-network/plink-endpoint-storage.png)

Ahora puede acceder al recurso de almacenamiento vinculado desde el explorador de almacenamiento en el área de trabajo de Synapse Studio dentro de la red virtual.

Si el área de trabajo tiene la opción "**Habilitar red virtual administrada**" durante la creación del área de trabajo, como se muestra a continuación,

![Creación de un punto de conexión privado para el almacenamiento vinculado a un área de trabajo de Synapse Studio 1](./media/how-to-connect-to-workspace-from-restricted-network/ws-network-config.png)

y le gustaría que su cuaderno acceda a los recursos de almacenamiento vinculados con una cuenta de almacenamiento concreta, debe agregar una opción "**Puntos de conexión privados administrados**" en Synapse Studio. El valor de "**Nombre de la cuenta de almacenamiento**" debe coincidir con la cuenta a la que el cuaderno necesita acceder. Obtenga instrucciones detalladas en el artículo [Creación de un punto de conexión privado administrado al origen de datos](./how-to-create-managed-private-endpoints.md).

Una vez creado este punto de conexión, el "**Estado de la aprobación**" será "**Pendiente**", debe solicitar al propietario de esta cuenta de almacenamiento que apruebe la solicitud en la pestaña "**Conexiones de punto de conexión privado**" de esta cuenta de almacenamiento en Azure Portal. Una vez aprobada, el cuaderno podrá acceder a los recursos de almacenamiento vinculados con esta cuenta de almacenamiento.

Ahora, todo está listo. Puede acceder al recurso del área de trabajo de Synapse Studio.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre [Red virtual del área de trabajo administrada](./synapse-workspace-managed-vnet.md)

Más información sobre los [puntos de conexión privados administrados](./synapse-workspace-managed-private-endpoints.md)
