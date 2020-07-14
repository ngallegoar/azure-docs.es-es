---
title: Definición de la configuración de exportación en Azure API for FHIR
description: En este artículo se describe cómo definir la configuración de exportación en Azure API for FHIR
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 3/5/2020
ms.author: matjazl
ms.openlocfilehash: 46a55b83b38593a514d40a9f75d99739a1efb8a2
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84871025"
---
# <a name="configure-export-setting-and-export-the-data-to-a-storage-account"></a>Definición de la configuración de la exportación y exportación de los datos a una cuenta de almacenamiento

Azure API for FHIR admite el comando $export, que le permite exportar los datos de la cuenta de Azure API for FHIR a una cuenta de almacenamiento.

Para llevar a cabo la exportación en Azure API for FHIR es necesario realizar cuatro pasos:

1. Habilitación de la identidad administrada en el servicio Azure API for FHIR
2. Creación de una cuenta de Azure Storage (si no se ha hecho antes) y asignación de permisos para Azure API for FHIR a la cuenta de almacenamiento
3. Selección de la cuenta de almacenamiento en Azure API for FHIR como cuenta de almacenamiento de exportación
4. Ejecución de la exportación mediante la invocación del comando $export en Azure API for FHIR

## <a name="enabling-managed-identity-on-azure-api-for-fhir"></a>Habilitación de la identidad administrada en Azure API for FHIR

El primer paso para configurar Azure API for FHIR para la exportación es habilitar la identidad administrada en todo el sistema en el servicio. [Aquí](../active-directory/managed-identities-azure-resources/overview.md) puede leer todas las identidades administradas en Azure.

Para ello, vaya al servicio Azure API for FHIR y seleccione la hoja Identidad. Al cambiar el estado a Activado, se habilitará la identidad administrada en el servicio Azure API for FHIR.

![Habilitación de una entidad administrada](media/export-data/fhir-mi-enabled.png)

Ahora podemos pasar al siguiente paso y crear una cuenta de almacenamiento y asignar permiso a nuestro servicio.

## <a name="adding-permission-to-storage-account"></a>Concesión de permisos a la cuenta de almacenamiento

El siguiente paso de la exportación es asignar el permiso para que el servicio Azure API for FHIR escriba en la cuenta de almacenamiento.

Después de crear una cuenta de almacenamiento, vaya a la hoja Control de acceso (IAM) en Cuenta de almacenamiento y seleccione Agregar asignación de roles.

![Habilitación de una entidad administrada](media/export-data/fhir-export-role-assignment.png)

A continuación, se agregará el rol Colaborador de datos de Storage Blob a nuestro nombre de servicio.

![Habilitación de una entidad administrada](media/export-data/fhir-export-role-add.png)

Ahora estamos preparados para el paso siguiente, donde podemos seleccionar la cuenta de almacenamiento en Azure API for FHIR como cuenta de almacenamiento predeterminada para el comando $export.

## <a name="selecting-the-storage-account-for-export"></a>Selección de la cuenta de almacenamiento para el comando $export

El último paso antes de invocar el comando $export consiste en asignar la cuenta de almacenamiento de Azure en la que Azure API for FHIR exportará los datos. Para ello, vaya a la hoja Integración en el servicio Azure API for FHIR en Azure Portal y seleccione la cuenta de almacenamiento. 

![Habilitación de una entidad administrada](media/export-data/fhir-export-storage.png)

Después de estas operaciones, estamos preparados para exportar los datos mediante el comando $export.

## <a name="exporting-the-data-using-export-command"></a>Exportación de datos mediante el comando $export

Una vez que hemos configurado Azure API for FHIR para la exportación, podemos usar el comando $export para exportar los datos del servicio a la cuenta de almacenamiento que se ha especificado. Para obtener información sobre cómo invocar el comando $export en el servidor de FHIR, lea la documentación sobre la especificación de dicho comando en [https://hl7.org/Fhir/uv/bulkdata/export/index.html](https://hl7.org/Fhir/uv/bulkdata/export/index.html).

> [!IMPORTANT]
> Tenga en cuenta que actualmente Azure API for FHIR solo admite la exportación de nivel de sistema, como se define en la especificación de exportación en [https://hl7.org/Fhir/uv/bulkdata/export/index.html](https://hl7.org/Fhir/uv/bulkdata/export/index.html). Actualmente, no se admiten los parámetros de consulta con el comando $export.

>[!div class="nextstepaction"]
>[Configuración adicional](azure-api-for-fhir-additional-settings.md)