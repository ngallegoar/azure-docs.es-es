---
title: Visualización de datos de Azure Data Explorer mediante Kibana
description: En este artículo, aprenderá a configurar Azure Data Explorer como origen de datos de Kibana.
author: orspod
ms.author: orspodek
ms.reviewer: guregini
ms.service: data-explorer
ms.topic: conceptual
ms.date: 03/12/2020
ms.openlocfilehash: fac9c78607e50dca384670bf4cc08b50f723312b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80065608"
---
# <a name="visualize-data-from-azure-data-explorer-in-kibana-with-the-k2bridge-open-source-connector"></a>Visualización de datos de Azure Data Explorer en Kibana con el conector de código abierto K2Bridge

K2Bridge (Kibana-Kusto Bridge) le permite usar Azure Data Explorer como origen de datos y para visualizar los datos en Kibana. K2Bridge es una aplicación en contenedor de [código abierto](https://github.com/microsoft/K2Bridge) que actúa como un proxy entre una instancia de Kibana y un clúster de Azure Data Explorer. En este artículo se describe cómo usar K2Bridge para crear esa conexión.

K2Bridge traduce las consultas de Kibana al lenguaje de consulta de Kusto (KQL) y devuelve a Kibana los resultados de Azure Data Explorer. 

   ![gráfico](media/k2bridge/k2bridge-chart.png)

K2Bridge admite la pestaña Descubrir de Kibana, donde puede hacer lo siguiente:
* Buscar y explorar datos
* Filtrar resultados
* Agregar o quitar campos en la cuadrícula de resultados
* Ver el contenido del registro
* Guardar y compartir búsquedas

En la imagen siguiente se muestra una instancia de Kibana enlazada a Azure Data Explorer mediante K2Bridge. La experiencia de usuario en Kibana no cambia.

   [![Página de Kibana](media/k2bridge/k2bridge-kibana-page.png)](media/k2bridge/k2bridge-kibana-page.png#lightbox)

## <a name="prerequisites"></a>Prerrequisitos

Para poder visualizar datos de Azure Data Explorer en Kibana, debe tener a punto lo siguiente:

* [Helm V3](https://github.com/helm/helm#install), el administrador de paquetes de Kubernetes.
* El clúster de Azure Kubernetes Service (AKS) y otros clústeres de Kubernetes (versiones 1.14 a 1.16) se han probado y comprobado. Si necesita un clúster de AKS, consulte los artículos sobre la implementación de un clúster de AKS [mediante la CLI de Azure](https://docs.microsoft.com/azure/aks/kubernetes-walkthrough) o [mediante Azure Portal](https://docs.microsoft.com/azure/aks/kubernetes-walkthrough-portal).
* Un [clúster de Azure Data Explorer](create-cluster-database-portal.md), que incluya lo siguiente:
    * La URL del clúster de Azure Data Explorer 
    * El nombre de la base de datos
    
* Una entidad de servicio de Azure AD autorizada para ver datos en Azure Data Explorer, que incluya lo siguiente:
    * El id. de cliente 
    * El secreto de cliente

    Se recomienda usar una entidad de servicio con el permiso "Visualizador". No se recomienda usar permisos superiores.

    * [Establezca los permisos de visualización del clúster para la entidad de servicio de Azure AD](https://docs.microsoft.com/azure/data-explorer/manage-database-permissions#manage-permissions-in-the-azure-portal).

    Para más información, sobre la entidad de servicio de Azure AD, consulte [Creación de una entidad de servicio de Azure AD](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#create-an-azure-active-directory-application).

## <a name="run-k2bridge-on-azure-kubernetes-service-aks"></a>Ejecución de K2Bridge en Azure Kubernetes Service (AKS)

De forma predeterminada, el gráfico de Helm de K2Bridge hace referencia a una imagen disponible públicamente ubicada en Microsoft Container Registry (MCR). MCR no requiere credenciales y funciona sin necesidad de configuración.

1. Descargue los gráficos de Helm necesarios.

1. Agregue la dependencia Elasticsearch a Helm. 
    El motivo de la dependencia Elasticsearch es que K2Bridge usa una instancia de Elasticsearch pequeña interna para atender las solicitudes relacionadas con los metadatos (como patrones de índice y consultas guardadas). No se guarda ningún dato empresarial en esta instancia interna, que se puede considerar como un detalle de implementación. 

    1. Para agregar la dependencia Elasticsearch a Helm:

        ```bash
        helm repo add elastic https://helm.elastic.co
        helm repo update
        ```

    1. Para obtener el gráfico de K2Bridge del directorio de gráficos del repositorio de GitHub, realice las acciones siguientes:
        1. Clonar el repositorio de [GitHub](https://github.com/microsoft/K2Bridge).
        1. Ir al directorio raíz del repositorio de K2Bridge.
        1. Ejecutar:

            ```bash
            helm dependency update charts/k2bridge
            ```

1. Implemente K2Bridge.

    1. Establezca las variables con los valores correctos para su entorno:

        ```bash
        ADX_URL=[YOUR_ADX_CLUSTER_URL] #For example, https://mycluster.westeurope.kusto.windows.net
        ADX_DATABASE=[YOUR_ADX_DATABASE_NAME]
        ADX_CLIENT_ID=[SERVICE_PRINCIPAL_CLIENT_ID]
        ADX_CLIENT_SECRET=[SERVICE_PRINCIPAL_CLIENT_SECRET]
        ADX_TENANT_ID=[SERVICE_PRINCIPAL_TENANT_ID]
        ```

    1. (Opcional) Habilite la telemetría de Azure Application Insights. 
        Si es la primera vez que usa Azure Application Insights, primero debe [crear un recurso de Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource). Tendrá que [copiar la clave de instrumentación](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource#copy-the-instrumentation-key) en una variable: 

        ```bash
        APPLICATION_INSIGHTS_KEY=[INSTRUMENTATION_KEY]
        COLLECT_TELEMETRY=true
        ```

    1. <a name="install-k2bridge-chart"></a>Instale el gráfico de K2Bridge:

        ```bash
        helm install k2bridge charts/k2bridge -n k2bridge --set image.repository=$REPOSITORY_NAME/$CONTAINER_NAME --set settings.adxClusterUrl="$ADX_URL" --set settings.adxDefaultDatabaseName="$ADX_DATABASE" --set settings.aadClientId="$ADX_CLIENT_ID" --set settings.aadClientSecret="$ADX_CLIENT_SECRET" --set settings.aadTenantId="$ADX_TENANT_ID" [--set image.tag=latest] [--set privateRegistry="$IMAGE_PULL_SECRET_NAME"] [--set settings.collectTelemetry=$COLLECT_TELEMETRY]
        ```

        En [Configuración](https://github.com/microsoft/K2Bridge/blob/master/docs/configuration.md) puede encontrar el conjunto completo de opciones de configuración.

    1. La salida del comando sugerirá el siguiente comando de Helm que se debe ejecutar para implementar Kibana. De manera opcional, ejecute el comando siguiente:

        ```bash
        helm install kibana elastic/kibana -n k2bridge --set image=docker.elastic.co/kibana/kibana-oss --set imageTag=6.8.5 --set elasticsearchHosts=http://k2bridge:8080
        ```
        
    1. Use el reenvío de puertos para acceder a Kibana en localhost: 

        ```bash
        kubectl port-forward service/kibana-kibana 5601 --namespace k2bridge
        ```
        
    1. Conéctese a Kibana a través de http://127.0.0.1:5601.

    1. Muestre Kibana a los usuarios finales. Dispone de varios métodos para hacerlo. El mejor método dependerá en gran medida del caso de uso.

        Por ejemplo:

        Muestre el servicio como un servicio LoadBalancer. Para ello, agregue el parámetro `--set service.type=LoadBalancer` al comando de instalación de Helm de K2Bridge ([anterior](#install-k2bridge-chart)).        
    
        A continuación, ejecute:
        
        ```bash
        kubectl get service -w -n k2bridge
        ```
        
        La salida debe ser similar a la siguiente: 

        ```bash
        NAME            TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
        kibana-kibana   LoadBalancer   xx.xx.xx.xx   <pending>      5601:30128/TCP   4m24s
        ```
 
        Después, puede usar la dirección IP externa generada que aparece para acceder a Kibana. Para ello, abra un explorador en `<EXTERNAL-IP>:5601`.

1. Configure patrones de índice para acceder a sus datos:  
En una nueva instancia de Kibana:
     1. Abra Kibana.
     1. Vaya a Administración.
     1. Seleccione **Index Patterns** (Patrones de índice). 
     1. Cree un patrón de índice.
El nombre del índice debe coincidir exactamente con el nombre de la tabla o la función, sin asterisco. Puede copiar la línea correspondiente de la lista.

> [!Note]
> Para la ejecución en otros proveedores de Kubernetes, cambie el objeto storageClassName de Elasticsearch en `values.yaml` para que se ajuste al que sugiere el proveedor.

## <a name="visualize-data"></a>Visualización de datos

Si Azure Data Explorer está configurado como un origen de datos para Kibana, puede usar Kibana para explorar los datos. 

1. En Kibana, en el menú de la izquierda, seleccione la pestaña **Descubrir**.

1. En la lista desplegable de la izquierda, seleccione un patrón de índice (en este caso, una tabla de Azure Data Explorer), que defina el origen de datos que desea explorar.
    
   ![Selección de patrón de índice](media/k2bridge/k2bridge-select-an-index-pattern.png)

1. Si sus datos tienen un campo de filtro de tiempo, puede especificar el intervalo de tiempo. En la parte superior derecha de la página, establezca un filtro de tiempo. De forma predeterminada, Descubrir muestra los datos de los últimos 15 minutos.

   ![Filtro de tiempo](media/k2bridge/k2bridge-time-filter.png)
    
1. La tabla de resultados muestra los primeros 500 registros. Puede expandir un documento para examinar los datos de sus campos en formato JSON o de tabla.

   ![Expansión de un registro](media/k2bridge/k2bridge-expand-record.png)

1. De forma predeterminada, la tabla de resultados incluye columnas para el origen del documento y el campo de hora (si existe). Puede elegir columnas específicas para agregarlas a la tabla de resultados. Para ello, seleccione **Agregar** junto al nombre del campo en la barra lateral izquierda.

   ![Columnas específicas](media/k2bridge/k2bridge-specific-columns.png)
    
1. En la barra de consulta, puede realizar las acciones siguientes para buscar los datos:
    * Escribir un término de búsqueda.
    * Usar la sintaxis de consulta de Lucene. 
    Por ejemplo:
        * Busque "error" para encontrar todos los registros que contienen este valor. 
        * Busque "estado: 200" para obtener todos los registros con un valor de estado de 200. 
    * Usar operadores lógicos (AND, OR y NOT).
    * Usar caracteres comodín (asterisco "\*" o signo de interrogación "?"). Por ejemplo:
        * La consulta `"destination_city: L*"` coincidirá con los registros en los que el valor de ciudad de destino empiece por "l" (K2Bridge no distingue entre mayúsculas y minúsculas).

    ![Ejecutar consulta](media/k2bridge/k2bridge-run-query.png)
    
    > [!Tip]
    > En [Búsqueda](https://github.com/microsoft/K2Bridge/blob/master/docs/searching.md), puede encontrar más reglas de búsqueda y lógica.

1. Para filtrar los resultados de la búsqueda, use la **lista de campos** de la barra lateral derecha de la página. 
    La lista de campos es donde puede ver:
    * Los primeros cinco valores del campo.
    * El número de registros que contienen el campo.
    * El porcentaje de registros que contienen cada valor. 
    
    >[!Tip]
    > Use el icono de lupa (+) para buscar todos los registros que contienen un valor específico.
    
    ![Lista de campos](media/k2bridge/k2bridge-field-list.png)
   
    También puede filtrar los resultados mediante el icono de lupa (+) de la vista de formato de la tabla de resultados de cada registro en la tabla de resultados.
    
     ![Lista de tablas](media/k2bridge/k2bridge-table-list.png)
    
1. Seleccione la opción **Guardar** o **Compartir** para la búsqueda.

     ![Guardar búsqueda](media/k2bridge/k2bridge-save-search.png)
