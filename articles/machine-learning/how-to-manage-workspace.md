---
title: Creación de áreas de trabajo en el portal
titleSuffix: Azure Machine Learning
description: Aprenda a crear, ver y eliminar áreas de trabajo de Azure Machine Learning en Azure Portal.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: sgilley
author: sdgilley
ms.date: 09/30/2020
ms.topic: conceptual
ms.custom: how-to, fasttrack-edit
ms.openlocfilehash: d0b5d3678c3d9c7e55eede13c630510df89d5128
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2020
ms.locfileid: "92045574"
---
# <a name="create-and-manage-azure-machine-learning-workspaces-in-the-azure-portal"></a>Creación y administración de áreas de trabajo de Azure Machine Learning en Azure Portal


En este artículo creará, verá y eliminará [**áreas de trabajo de Azure Machine Learning**](concept-workspace.md) en Azure Portal para [Azure Machine Learning](overview-what-is-azure-ml.md).  El portal es la forma más sencilla de empezar a trabajar con áreas de trabajo, pero a medida que cambian las necesidades o aumentan los requisitos de automatización, también puede crear y eliminar áreas de trabajo [mediante la CLI](reference-azure-machine-learning-cli.md), [con código de Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py&preserve-view=true) o [a través de la extensión de VS Code](tutorial-setup-vscode-extension.md).

## <a name="create-a-workspace"></a>Crear un área de trabajo

Para crear un área de trabajo, necesita una suscripción de Azure. Si no tiene una suscripción de Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning](https://aka.ms/AMLFree).

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) con las credenciales de la suscripción de Azure. 

1. En la esquina superior izquierda de Azure Portal, seleccione **+ Crear un recurso**.

      ![Crear un nuevo recurso](./media/how-to-manage-workspace/create-workspace.gif)

1. Use la barra de búsqueda para encontrar **Machine Learning**.

1. Seleccione **Machine Learning**.

1. En el panel **Machine Learning**, seleccione **Crear** para comenzar.

1. Especifique la siguiente información para configurar la nueva área de trabajo:

   Campo|Descripción 
   ---|---
   Nombre del área de trabajo |Escriba un nombre único que identifique el área de trabajo. En este ejemplo, se usa **docs-ws**. Los nombres deben ser únicos en el grupo de recursos. Utilice un nombre que sea fácil de recordar y que se diferencie del de las áreas de trabajo creadas por otros. El nombre del área de trabajo no distingue mayúsculas de minúsculas.
   Suscripción |Seleccione la suscripción de Azure que quiera usar.
   Resource group | Use un grupo de recursos existente en su suscripción o escriba un nombre para crear un nuevo grupo de recursos. Un grupo de recursos almacena los recursos relacionados con una solución de Azure. En este ejemplo, se usa **docs-aml**. Necesita el rol *colaborador* o *propietario* para usar un grupo de recursos existente.  Para obtener más información sobre el acceso, consulte [Administración del acceso a un área de trabajo de Azure Machine Learning](how-to-assign-roles.md).
   Region | Seleccione la región de Azure más cercana a los usuarios y los recursos de datos para crear el área de trabajo.

1. Cuando haya terminado de configurar el área de trabajo, seleccione **Revisar y crear**. También puede usar las secciones [Redes](#networking) y [Opciones avanzadas](#advanced) para configurar otros valores del área de trabajo.

1. Revise la configuración y realice cualquier cambio o corrección adicional. Cuando esté satisfecho con la configuración, seleccione **Crear**.

   > [!Warning] 
   > La creación del área de trabajo en la nube puede tardar varios minutos.

   Cuando finalice el proceso, aparecerá un mensaje de implementación correcta. 
 
 1. Para ver la nueva área de trabajo, seleccione **Ir al recurso**.

### <a name="networking"></a>Redes  

> [!IMPORTANT]  
> Para obtener más información sobre el uso de un punto de conexión privado y una red virtual con el área de trabajo, vea [Aislamiento de red y privacidad](how-to-enable-virtual-network.md).
    
1. La configuración de red predeterminada es usar un __Punto de conexión público__, que es accesible en la red pública de Internet. Para limitar el acceso al área de trabajo a una instancia de Azure Virtual Network que ha creado, puede seleccionar __Punto de conexión privado__ como el __Método de conectividad__ y, a continuación, usar __+ Agregar__ para configurar el punto de conexión. 
    
   :::image type="content" source="media/how-to-manage-workspace/select-private-endpoint.png" alt-text="Selección del punto de conexión privado":::  

1. En el formulario __Crear punto de conexión privado__, establezca la ubicación, el nombre y la red virtual que se va a usar. Si desea usar el punto de conexión con una zona DNS privada, seleccione __Integrar con la zona DNS privada__ y seleccione la zona mediante el campo __Zona DNS privada__. Seleccione __Aceptar__ para crear el punto de conexión.   

   :::image type="content" source="media/how-to-manage-workspace/create-private-endpoint.png" alt-text="Selección del punto de conexión privado":::   

1. Cuando haya terminado de configurar las redes, puede seleccionar __Revisar y crear__, o bien avanzar hasta la configuración de __Avanzado__ opcional. 

    > [!WARNING]    
    > Al crear un punto de conexión privado, se crea una nueva zona DNS privada denominada __privatelink.api.azureml.ms__. Contiene un vínculo a la red virtual. Si crea varias áreas de trabajo con puntos de conexión privados en el mismo grupo de recursos, solo la red virtual del primer punto de conexión privado se puede agregar a la zona DNS. Para agregar entradas para las redes virtuales usadas por áreas de trabajo o puntos de conexión privados adicionales, siga estos pasos: 
    >   
    > 1. En [Azure Portal](https://portal.azure.com), seleccione el grupo de recursos que contiene el área de trabajo. A continuación, seleccione el recurso de zona DNS privada denominado __privatelink.api.azureml.ms__.    
    > 2. En __Configuración__, seleccione __Vínculos de red virtual__. 
    > 3. Seleccione __Agregar__. En la página __Agregar el vínculo de red virtual__, proporcione un __Nombre de vínculo__ único y, a continuación, seleccione la __Red virtual__ que se va a agregar. Seleccione __Aceptar__ para agregar el vínculo de red.    
    >   
    > Para obtener más información, vea [Configuración de DNS para puntos de conexión privados de Azure](/azure/private-link/private-endpoint-dns).   

### <a name="vulnerability-scanning"></a>Examen de vulnerabilidades

Azure Security Center proporciona administración unificada de la seguridad y protección avanzada contra amenazas para cargas de trabajo en la nube híbrida. Debe permitir que Azure Security Center examine los recursos y seguir sus recomendaciones. Para más información, consulte [Introducción a Azure Defender para registros de contenedor](https://docs.microsoft.com/azure/security-center/azure-container-registry-integration) e [Introducción a Azure Defender para Kubernetes](https://docs.microsoft.com/azure/security-center/azure-kubernetes-service-integration).

### <a name="advanced"></a>Avanzado    

De forma predeterminada, las métricas y los metadatos del área de trabajo se almacenan en una instancia de Azure Cosmos DB que Microsoft mantiene. Estos datos se cifran con claves administradas por Microsoft.  

Para limitar los datos que Microsoft recopila sobre el área de trabajo, seleccione __High business impact workspace__ (Área de trabajo de alto impacto de negocio). Para más información sobre esta configuración, consulte [Cifrado en reposo](concept-enterprise-security.md#encryption-at-rest).

> [!IMPORTANT]  
> La selección de un alto impacto de negocio solo puede realizarse al crear un área de trabajo. Este valor no se puede cambiar tras la creación del área de trabajo.   
Si va a usar la versión __Enterprise__ de Azure Machine Learning, puede proporcionar su propia clave. Así, se crea la instancia de Azure Cosmos DB que almacena las métricas y los metadatos en la suscripción de Azure. Siga estos pasos para usar su propia clave:    

> [!IMPORTANT]  
> Antes de seguir estos pasos, debe completar las siguientes acciones:   
>   
> 1. Autorice __Machine Learning App__ (en Administración de identidades y acceso) con permisos de colaborador en la suscripción.  
> 1. Siga los pasos de [Configuración de claves administradas por el cliente](/azure/cosmos-db/how-to-setup-cmk) para:   
>     * Registrar el proveedor de Azure Cosmos DB   
>     * Creación y configuración de una instancia de Azure Key Vault 
>     * Generar una clave  
>   
>     No es necesario crear manualmente la instancia de Azure Cosmos DB; se crea una automáticamente durante la creación del área de trabajo. Esta instancia de Azure Cosmos DB se crea en un grupo de recursos independiente con un nombre basado en este patrón: `<your-workspace-resource-name>_<GUID>`.   
>   
> Este valor no se puede cambiar tras la creación del área de trabajo. Si elimina la instancia de Azure Cosmos DB que usa el área de trabajo, también debe eliminar el área de trabajo que la está usando.

1. Seleccione __Claves administradas por el cliente__ y, después, seleccione __Hacer clic para seleccionar una clave__.   

    :::image type="content" source="media/how-to-manage-workspace/advanced-workspace.png" alt-text="Selección del punto de conexión privado":::   

1. En el formulario __Seleccionar clave en Azure Key Vault__, seleccione una instancia de Azure Key Vault existente, una clave que contenga y la versión de la clave. Esta clave se usa para cifrar los datos almacenados en Azure Cosmos DB. Por último, use el botón __Seleccionar__ para usar esta clave. 

   :::image type="content" source="media/how-to-manage-workspace/select-key-vault.png" alt-text="Selección del punto de conexión privado":::

### <a name="download-a-configuration-file"></a>Descarga de un archivo de configuración

1. Si va a crear una [instancia de proceso](tutorial-1st-experiment-sdk-setup.md#azure), omita este paso.

1. Si tiene previsto usar código en el entorno local que haga referencia a esta área de trabajo, seleccione **Descargar config.json** en la sección **Información general** del área de trabajo.  

   ![Descargar config.json](./media/how-to-manage-workspace/configure.png)
   
   Coloque el archivo en la estructura de directorios que contiene los scripts de Python o las instancias de Jupyter Notebook. Puede estar en el mismo directorio, en un subdirectorio denominado *.azureml* o en un directorio principal. Al crear una instancia de proceso, este archivo se agrega automáticamente al directorio correcto de la máquina virtual.
## <a name="find-a-workspace"></a><a name="view"></a>Buscar un área de trabajo

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).

1. En el campo de búsqueda superior, escriba **Machine Learning**.  

1. Seleccione **Machine Learning**.

   ![Búsqueda del área de trabajo de Azure Machine Learning](./media/how-to-manage-workspace/find-workspaces.png)

1. Examine la lista de áreas de trabajo encontradas. Puede filtrar en función de suscripción, grupos de recursos y ubicaciones.  

1. Seleccione un área de trabajo para mostrar sus propiedades.

## <a name="delete-a-workspace"></a>Eliminar un área de trabajo

En [Azure Portal](https://portal.azure.com/), seleccione **Eliminar** en la parte superior del área de trabajo que desea eliminar.

:::image type="content" source="./media/how-to-manage-workspace/delete-workspace.png" alt-text="Selección del punto de conexión privado":::

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [aml-delete-resource-group](../../includes/aml-delete-resource-group.md)]

## <a name="troubleshooting"></a>Solución de problemas

### <a name="resource-provider-errors"></a>Errores del proveedor de recursos

[!INCLUDE [machine-learning-resource-provider](../../includes/machine-learning-resource-provider.md)]

### <a name="moving-the-workspace"></a>Movimiento del área de trabajo

> [!WARNING]
> No se admite mover el área de trabajo de Azure Machine Learning a otra suscripción ni mover la suscripción propietaria a un nuevo inquilino. Si lo hace, pueden producirse errores.

### <a name="deleting-the-azure-container-registry"></a>Eliminación de la instancia de Azure Container Registry

El área de trabajo de Azure Machine Learning usa Azure Container Registry (ACR) para algunas operaciones. La primera vez que se necesite una instancia de ACR, se creará automáticamente.

[!INCLUDE [machine-learning-delete-acr](../../includes/machine-learning-delete-acr.md)]

## <a name="next-steps"></a>Pasos siguientes

Siga el tutorial completo para obtener información sobre cómo usar un área de trabajo para compilar, entrenar e implementar modelos con Azure Machine Learning.

> [!div class="nextstepaction"]
> [Tutorial: Entrenamiento de modelos](tutorial-train-models-with-aml.md)
