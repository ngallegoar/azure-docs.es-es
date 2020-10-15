---
title: Varios clústeres de HDInsight y la misma cuenta de Azure Data Lake Storage
description: Obtenga información sobre cómo usar más de un clúster de HDInsight con una sola cuenta de Data Lake Storage
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive
ms.date: 12/18/2019
ms.openlocfilehash: 19c40f2a7609d556448641e78fdeffe83e8660b1
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86083957"
---
# <a name="use-multiple-hdinsight-clusters-with-an-azure-data-lake-storage-account"></a>Uso de varios clústeres de HDInsight con una cuenta de Azure Data Lake Storage

A partir de HDInsight versión 3.5, puede crear clústeres de HDInsight con cuentas de Azure Data Lake Storage como sistema de archivos predeterminado.
Data Lake Storage admite almacenamiento ilimitado, lo cual es idóneo no solo para hospedar grandes cantidades de datos, sino también para hospedar varios clústeres de HDInsight que compartan una única cuenta de Data Lake Storage. Para obtener instrucciones sobre cómo crear un clúster de HDInsight con Data Lake Storage como almacenamiento, vea [Inicio rápido: Configuración de clústeres en HDInsight](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md).

En este artículo se proporcionan recomendaciones para que el administrador de Data Lake Storage configure una única cuenta compartida de Data Lake Storage que pueda usarse en varios clústeres **activos** de HDInsight. Estas recomendaciones se aplican al hospedaje de varios clústeres de Apache Hadoop seguros y no seguros en una cuenta compartida de Data Lake Storage.

## <a name="data-lake-storage-file-and-folder-level-acls"></a>Listas de control de acceso de nivel de archivo y de carpeta de Data Lake Storage

En el resto de este artículo se da por supuesto que tiene un conocimiento profundo de las listas de control de acceso de nivel de archivo y de carpeta en Azure Data Lake Storage, que se describen en detalle en [Control de acceso en Azure Data Lake Storage](../data-lake-store/data-lake-store-access-control.md).

## <a name="data-lake-storage-setup-for-multiple-hdinsight-clusters"></a>Configuración de Data Lake Storage para varios clústeres de HDInsight

Empecemos por una jerarquía de carpetas de dos niveles para explicar las recomendaciones de uso de varios clústeres de HDInsight con una cuenta de Data Lake Storage. Supongamos que tiene una cuenta de Data Lake Storage con la estructura de carpetas **/clusters/finance**. Con esta estructura, todos los clústeres requeridos por la organización de Finanzas pueden usar /clusters/finance como la ubicación de almacenamiento. En el futuro, si otra organización (por ejemplo, Marketing) desea crear clústeres de HDInsight con la misma cuenta de Data Lake Storage, podría crear /clusters/marketing. De momento, usaremos simplemente **/clusters/finance**.

Para que los clústeres de HDInsight puedan utilizar esta estructura de carpetas eficazmente, el administrador de Data Lake Storage debe asignar los permisos adecuados, tal como se describe en la tabla. Los permisos mostrados en la tabla corresponden a las ACL de acceso, y no a las ACL predeterminadas.

|Carpeta  |Permisos  |usuario propietario  |grupo propietario  | Usuario con nombre | Permisos de usuario con nombre | Grupo con nombre | Permisos de grupo con nombre |
|---------|---------|---------|---------|---------|---------|---------|---------|
|/ | rwxr-x--x  |admin |admin  |Entidad de servicio |--x  |FINGRP   |r-x         |
|/clusters | rwxr-x--x |admin |admin |Entidad de servicio |--x  |FINGRP |r-x         |
|/clusters/finance | rwxr-x--t |admin |FINGRP  |Entidad de servicio |rwx  |-  |-     |

En la tabla,

- **admin** es el creador y el administrador de la cuenta de Data Lake Storage.
- **Entidad de servicio** es la entidad de servicio de Azure Active Directory (AAD) asociada a la cuenta.
- **FINGRP** es un grupo de usuarios creado en AAD que contiene usuarios de la organización de Finanzas.

Para obtener instrucciones sobre cómo crear una aplicación de AAD (que también crea una entidad de servicio), consulte [Creación de una aplicación de Azure Active Directory](../active-directory/develop/howto-create-service-principal-portal.md#register-an-application-with-azure-ad-and-create-a-service-principal). Para obtener instrucciones sobre cómo crear un grupo de usuarios en AAD, consulte [Administración de grupos en Azure Active Directory](../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

Algunos puntos que se deben tener en cuenta:

- El administrador de Data Lake Storage debe crear la estructura de carpetas de dos niveles ( **/clusters/finance/** ) y aprovisionarla con los permisos adecuados **antes** de utilizar la cuenta de almacenamiento para clústeres. Esta estructura no se crea automáticamente durante la creación de clústeres.
- El ejemplo anterior recomienda establecer el grupo propietario de **/clusters/finance** como **FINGRP** y permitir acceso **r-x** a FINGRP a la jerarquía de carpetas completa desde la raíz. Esto garantiza que los miembros de FINGRP pueden navegar desde la raíz de la estructura de carpetas.
- En el caso de que varias entidades de servicio de AAD puedan crear clústeres en **/clusters/finance**, el bit sticky (cuando se establece en la carpeta **finance**) garantiza que una carpeta creada por una entidad de servicio no puede eliminar a otra.
- Una vez que la estructura de carpetas y los permisos estén configurados, el proceso de creación de clústeres de HDInsight crea una ubicación de almacenamiento específica para clústeres en **/clusters/finance/** . Por ejemplo, el almacenamiento para un clúster con el nombre fincluster01 podría ser **/clusters/finance/fincluster01**. La propiedad y los permisos para las carpetas creadas por un clúster de HDInsight se muestran en esta tabla.

    |Carpeta  |Permisos  |usuario propietario  |grupo propietario  | Usuario con nombre | Permisos de usuario con nombre | Grupo con nombre | Permisos de grupo con nombre |
    |---------|---------|---------|---------|---------|---------|---------|---------|
    |/clusters/finance/ fincluster01 | rwxr-x---  |Entidad de servicio |FINGRP  |- |-  |-   |-  |

## <a name="recommendations-for-job-input-and-output-data"></a>Recomendaciones para los datos de entrada y salida de trabajos

Se recomienda que los datos de entrada y salida de un trabajo se almacenen en una carpeta fuera de **/clusters**. Esto garantiza que incluso si se elimina la carpeta específica del clúster para liberar espacio de almacenamiento, las entradas y salidas de trabajo y seguirán disponibles para su uso futuro. En ese caso, asegúrese de que la jerarquía de carpetas para almacenar las entradas y salidas de trabajo permita un nivel de acceso adecuado para la entidad de servicio.

## <a name="limit-on-clusters-sharing-a-single-storage-account"></a>Límite de clústeres que comparten una única cuenta de almacenamiento

El límite del número de clústeres que pueden compartir una única cuenta de Data Lake Storage depende de la carga de trabajo ejecutada en esos clústeres. Tener demasiados clústeres o cargas de trabajo muy elevadas en los clústeres que comparten una cuenta de almacenamiento puede causar limitaciones en la entrada o salida de datos de la cuenta de almacenamiento.

## <a name="support-for-default-acls"></a>Soporte para ACL predeterminadas

Al crear una entidad de servicio con acceso de un usuario con nombre (como se indica en la tabla anterior), se recomienda **no** agregar el usuario con nombre con una ACL predeterminada. El aprovisionamiento de acceso de un usuario con nombre mediante las ACL predeterminadas da como resultado la asignación de 770 permisos de usuario propietario, grupo propietario y otros. Mientras que el valor predeterminado de 770 no retira permisos al usuario propietario (7) o grupo propietario (7), retira todos los permisos del resto (0). Esto da como resultado un problema conocido con un determinado caso de uso que se describe en detalle en la sección [Problemas conocidos y soluciones alternativas](#known-issues-and-workarounds).

## <a name="known-issues-and-workarounds"></a>Problemas conocidos y soluciones alternativas

Esta sección muestra los problemas conocidos del uso de HDInsight con Data Lake Storage y sus soluciones alternativas.

### <a name="publicly-visible-localized-apache-hadoop-yarn-resources"></a>Recursos YARN de Apache Hadoop localizados visibles públicamente

Cuando se crea una nueva cuenta de Azure Data Lake Storage, el directorio raíz se aprovisiona automáticamente con los bits de permiso de acceso de ACL establecidos en 770. El usuario propietario de la carpeta raíz está configurado como el usuario que creó la cuenta (el administrador de Data Lake Storage) y el grupo propietario está configurado como el grupo principal del usuario que creó la cuenta. No se proporciona acceso para otros usuarios.

Se sabe que esta configuración afecta a un caso de uso de HDInsight específico descrito en [YARN 247](https://hwxmonarch.atlassian.net/browse/YARN-247). Los envíos de trabajos pueden generar un error con un mensaje similar al siguiente:

```output
Resource XXXX is not publicly accessible and as such cannot be part of the public cache.
```

Como se indica en el JIRA YARN vinculado anteriormente, al localizar recursos públicos el localizador valida que todos los recursos solicitados son realmente públicos comprobando sus permisos en el sistema de archivos remoto. Se rechaza la localización de cualquier recurso local que no se ajuste a esa condición. La comprobación de permisos incluye acceso de lectura al archivo para otros usuarios. Este escenario no funciona en principio al hospedar clústeres de HDInsight en Azure Data Lake, ya que Azure Data Lake deniega el acceso a otros usuarios en el nivel de la carpeta raíz.

#### <a name="workaround"></a>Solución alternativa

Configure los permisos de lectura y ejecución para **otros** mediante la jerarquía (por ejemplo, en **/** , **/clusters** y **/clusters/finance**), como se muestra en la tabla anterior.

## <a name="see-also"></a>Consulte también

- [Inicio rápido: Set up clusters in HDInsight](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md) (Guía de inicio rápido: Configuración de clústeres en HDInsight).
- [Uso de Data Lake Storage Gen2 con clústeres de Azure HDInsight](hdinsight-hadoop-use-data-lake-storage-gen2.md)
