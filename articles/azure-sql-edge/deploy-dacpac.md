---
title: 'Uso de paquetes DAC de SQL Database: Azure SQL Edge (versión preliminar)'
description: Obtenga más información sobre el uso de manifiestos DACPAC en Azure SQL Edge (versión preliminar)
keywords: SQL Edge, sqlpackage
services: sql-edge
ms.service: sql-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: 0ddd1544c6a51ff1e2f98a28e40d9eb2ee0b47c7
ms.sourcegitcommit: f1132db5c8ad5a0f2193d751e341e1cd31989854
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2020
ms.locfileid: "84233277"
---
# <a name="sql-database-dac-packages-in-sql-edge"></a>Paquetes DAC de SQL Database en SQL Edge

Azure SQL Edge (versión preliminar) es un motor de base de datos relacional optimizado y orientado a implementaciones de IoT y de tipo perimetral. Se basa en las versiones más recientes del motor de base de datos de Microsoft SQL Server, que proporciona funcionalidades de rendimiento, seguridad y procesamiento de consultas líderes en el sector. Junto con las capacidades de administración de bases de datos relacionales líderes del sector de SQL Server, Azure SQL Edge proporciona funcionalidad de streaming integrada para el análisis en tiempo real y el procesamiento de eventos complejos.

Azure SQL Edge también proporciona una implementación nativa de SqlPackage.exe que permite a los usuarios implementar un paquete [DAC de SQL Database](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications) durante la implementación de SQL Edge. Los manifiestos DACPAC de SQL Database se pueden implementar en SQL Edge mediante el parámetro SqlPackage expuesto a través de la opción `module twin's desired properties` del módulo SQL Edge:

```json
{
    "properties.desired":
    {
        "SqlPackage": "<Optional_DACPAC_ZIP_SAS_URL>",
        "ASAJobInfo": "<Optional_ASA_Job_ZIP_SAS_URL>"
    }
}
```

|Campo | Descripción |
|------|-------------|
| SqlPackage | El URI de Azure Blob Storage del archivo *.zip que contiene el paquete DAC de SQL Database.
| ASAJobInfo | El URI de Azure Blob Storage para el trabajo de Edge de ASA.

## <a name="use-a-sql-database-dac-package-with-sql-edge"></a>Uso de un paquete DAC de SQL Database con SQL Edge

Para usar un paquete DAC de SQL Database (*.dacpac) con SQL Edge, siga estos pasos:

1. Cree o extraiga un paquete DAC de SQL Database. Consulte [Extracción de un paquete DAC de una base de datos](/sql/relational-databases/data-tier-applications/extract-a-dac-from-a-database/) para más información sobre cómo generar un paquete DAC para una base de datos de SQL Server existente.

2. Comprima el archivo *.dacpac y cárguelo en una cuenta de Azure Blob Storage. Para más información sobre la carga de archivos en Azure Blob Storage, consulte [Carga, descarga y enumeración de blobs con Azure Portal](../storage/blobs/storage-quickstart-blobs-portal.md).

3. Genere una firma de acceso compartido para el archivo ZIP mediante Azure Portal. Para más información, consulte [Delegación de acceso con firmas de acceso compartidas (SAS)](../storage/common/storage-sas-overview.md).

4. Actualice la configuración del módulo SQL Edge para incluir el URI de acceso compartido para el paquete DAC. Para actualizar el módulo SQL Edge, realice estos pasos:

    1. En Azure Portal, vaya a la implementación de IoT Hub.

    2. En el panel izquierdo, seleccione **IoT Edge**.

    3. En la página de **IoT Edge**, busque y seleccione la instancia de IoT Edge en la que se implementa el módulo de SQL Edge.

    4. En la página de **Dispositivo de IoT Edge**, seleccione **Establecer módulo**.

    5. En la página **Establecer módulos**, seleccione **Configurar** en el módulo de SQL Edge.

    6. En el panel **Módulos personalizados de IoT Edge**, seleccione **Establecer propiedades deseadas de gemelo del módulo**. Actualice las propiedades deseadas para incluir el URI para la opción `SQLPackage`, como se muestra en el ejemplo siguiente.

        > [!NOTE]
        > El URI de SAS del siguiente archivo JSON es solo un ejemplo. Reemplace el URI por el URI real de la implementación.

        ```json
            {
                "properties.desired":
                {
                    "SqlPackage": "<<<SAS URL for the *.zip file containing the dacpac",
                }
            }
        ```

    7. Seleccione **Guardar**.

    8. En la página **Establecer módulos**, seleccione **Siguiente**.

    9. En la página **Establecer módulos**, seleccione **Siguiente** y, después, **Enviar**.

5. Después de realizar la actualización del módulo, el archivo de paquete DAC se descarga, se descomprime y se implementa en la instancia de SQL Edge.

En cada reinicio del contenedor de Azure SQL Edge, el paquete del archivo *.dacpac se descarga y se evalúa para realizar los cambios oportunos. Si se encuentra una nueva versión del archivo dacpac, los cambios se implementan en la base de datos de SQL Edge.

## <a name="next-steps"></a>Pasos siguientes

- [Implementación de SQL Edge mediante Azure Portal](deploy-portal.md).
- [Datos de streaming](stream-data.md)
- [Aprendizaje automático e IA con ONNX en SQL Edge (versión preliminar)](onnx-overview.md)
