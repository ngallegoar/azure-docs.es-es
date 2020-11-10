---
title: 'Servicios de datos habilitados para Azure Arc: notas de la versión'
description: Notas de la versión más recientes
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
ms.date: 10/29/2020
ms.topic: conceptual
ms.openlocfilehash: 82dd2f16fa43b52ba4c6dfacd26da5da622523b2
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2020
ms.locfileid: "93321720"
---
# <a name="release-notes---azure-arc-enabled-data-services-preview"></a>Notas de la versión: servicios de datos habilitados para Azure Arc (versión preliminar)

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="october-2020"></a>Octubre de 2020 

Número de versión de la CLI de datos de Azure (`azdata`): 20.2.3. Descarga en [https://aka.ms/azdata](https://aka.ms/azdata).

### <a name="breaking-changes"></a>Últimos cambios

Esta versión presenta los siguientes cambios importantes: 

* En la definición de recursos personalizada (CRD) de PostgreSQL, el término `shards` cambia a `workers`. Este término (`workers`) coincide con el nombre del parámetro de la línea de comandos.

* `azdata arc postgres server delete` solicita confirmación antes de eliminar una instancia de PostgreSQL.  Use `--force` para omitir la solicitud.

### <a name="additional-changes"></a>Cambios adicionales

* Se ha agregado un nuevo parámetro opcional a `azdata arc postgres server create` denominado `--volume-claim mounts`. El valor es una lista separada por comas de montajes de notificación de volumen. Un montaje de notificación de volumen es un par de tipo de volumen y nombre de PVC. El único tipo de volumen admitido actualmente es `backup`.  En PostgreSQL, cuando el tipo de volumen es `backup`, PVC se monta en `/mnt/db-backups`.  Esto permite compartir copias de seguridad entre instancias de PostgresSQL para que la copia de seguridad de una instancia de PostgresSQL pueda restaurarse en otra.

* Nuevo nombre corto para las definiciones de recursos personalizadas de PostgresSQL: 

  * `pg11` 

  * `pg12`

* La carga de telemetría proporciona al usuario:

   * El número de puntos cargados en Azure

     o 

   * Si no se ha cargado ningún dato en Azure, una solicitud para volver a intentarlo.

* `azdata arc dc debug copy-logs` ahora también hace lecturas en la carpeta `/var/opt/controller/log` y recopila los registros del motor de PostgreSQL en Linux.

*   Visualización de un indicador de funcionamiento durante la creación y restauración de copias de seguridad con Hiperescala de PostgreSQL.

* `azdata arc postrgres backup list` ahora incluye información de tamaño de copia de seguridad.

* La propiedad de nombre de administrador de SQL Managed Instance se ha agregado a la columna derecha de la hoja de información general en Azure Portal.

* Azure Data Studio admite la configuración del número de nodos de trabajo, el núcleo virtual y la configuración de memoria para Hiperescala de PostgreSQL. 

* La versión preliminar admite la copia de seguridad y restauración de las versiones 11 y 12 de PostgreSQL.

## <a name="september-2020"></a>Septiembre de 2020

Los servicios de datos habilitados para Azure Arc se publican como versión preliminar pública. Los servicios de datos habilitados para Arc permiten administrar los servicios de datos en cualquier lugar.

- Instancia administrada de SQL
- Hiperescala de PostgreSQL

Para obtener instrucciones, consulte [¿Qué son los servicios de datos habilitados para Azure Arc?](overview.md)

## <a name="known-limitations-and-issues"></a>Problemas y limitaciones conocidos

- Los nombres de instancia no pueden tener más de 13 caracteres
- No hay actualización local para el controlador de datos de Azure Arc ni las instancias de base de datos.
- Las imágenes de contenedor de los servicios de datos habilitados para Arc no están firmadas.  Es posible que deba configurar los nodos de Kubernetes para permitir la extracción de imágenes de contenedor sin firmar.  Por ejemplo, si usa Docker como entorno de ejecución del contenedor, puede establecer la variable de entorno DOCKER_CONTENT_TRUST=0 y reiniciar.  Otros entornos de ejecución de contenedores tienen opciones similares, como ocurre con [OpenShift](https://docs.openshift.com/container-platform/4.5/openshift_images/image-configuration.html#images-configuration-file_image-configuration).
- No se pueden crear instancias de SQL Managed Instance ni grupos de servidores de Hiperescala de PostgreSQL habilitada para Azure Arc desde Azure Portal.
- Por ahora, si usa NFS, debe establecer `allowRunAsRoot` en `true` en el archivo del perfil de implementación antes de crear el controlador de datos de Azure Arc.
- Solo autenticación de inicio de sesión de SQL y PostgreSQL.  No se admiten Azure Active Directory ni Active Directory.
- La creación de un controlador de datos en OpenShift requiere restricciones de seguridad relajadas.  Para obtener información detallada, consulte la documentación.
- No se admite la actualización a una versión de Kubernetes más reciente si usa el motor de Azure Kubernetes Service (Motor de AKS) en Azure Stack Hub con el controlador de datos de Azure Arc e instancias de base de datos. Desinstale el controlador de datos de Azure Arc y todas las instancias de base de datos antes de actualizar el clúster de Kubernetes.
- Azure Kubernetes Service (AKS), los clústeres que abarcan [varias zonas de disponibilidad](../../aks/availability-zones.md) no se admiten actualmente para los servicios de datos habilitados para Azure Arc. Para evitar este problema, al crear el clúster de AKS en Azure Portal, si selecciona una región en la que hay zonas disponibles, desactive todas las zonas del control de selección. Consulte la siguiente imagen:

   :::image type="content" source="media/release-notes/aks-zone-selector.png" alt-text="Desactive las casillas de cada zona para no especificar ninguna.":::


### <a name="known-issues-for-azure-arc-enabled-postgresql-hyperscale"></a>Problemas conocidos de Hiperescala de PostgreSQL habilitado para Azure Arc   

- La versión preliminar no admite la copia de seguridad y restauración del motor de PostgreSQL versión 11. Solo admite la copia de seguridad y restauración de la versión 12 de PostgreSQL.
- `azdata arc dc debug copy-logs` no recopila los registros del motor de PostgreSQL en Windows.
- Al volver a crear un grupo de servidores con el nombre de uno que se acaba de eliminar, se puede producir un error o un bloqueo. 
   - **Solución alternativa** No vuelva a usar el mismo nombre al volver a crear un grupo de servidores o espere al servicio externo o al equilibrador de carga de un grupo de servidores eliminado previamente. Si se da por hecho que el nombre del grupo de servidores que ha eliminado era `postgres01` y que se hospedaba en un espacio de nombres `arc`, antes de volver a crear un grupo de servidores con el mismo nombre, espere hasta que `postgres01-external-svc` no aparezca en la salida del comando kubectl `kubectl get svc -n arc`.
 - La página Información general y la página de configuración Proceso y almacenamiento de Azure Data Studio se cargan lentamente. 



## <a name="next-steps"></a>Pasos siguientes
  
> **¿Solo desea realizar algunas pruebas?**  
> Empiece a trabajar rápidamente con el [Inicio rápido de Azure Arc](https://github.com/microsoft/azure_arc#azure-arc-enabled-data-services) en Azure Kubernetes Service (AKS), AWS Elastic Kubernetes Service (EKS), Google Cloud Kubernetes Engine (GKE) o en una máquina virtual de Azure.

[Instalación de las herramientas de cliente](install-client-tools.md)

[Creación del controlador de datos de Azure Arc](create-data-controller.md) (requiere primero la instalación de las herramientas de cliente).

[Creación de una instancia administrada de Azure SQL en Azure Arc](create-sql-managed-instance.md) (requiere primero la creación de un controlador de datos de Azure Arc).

[Creación de un grupo de servidores de Hiperescala de Azure Database for PostgreSQL en Azure Arc](create-postgresql-hyperscale-server-group.md) (requiere primero la creación de un controlador de datos de Azure Arc).
