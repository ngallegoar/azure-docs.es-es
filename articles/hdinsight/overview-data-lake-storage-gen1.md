---
title: Introducción a Azure Data Lake Storage Gen1 en HDInsight
description: Introducción a Data Lake Storage Gen1 en HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: seoapr2020
ms.date: 04/21/2020
ms.openlocfilehash: 947dd125cf9c5f5874eed380b3d69cff11509e31
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "82187252"
---
# <a name="azure-data-lake-storage-gen1-overview-in-hdinsight"></a>Introducción a Azure Data Lake Storage Gen1 en HDInsight

Azure Data Lake Storage Gen1 es un repositorio a gran escala de nivel empresarial para cargas de trabajo de análisis de macrodatos. Con Azure Data Lake, puede capturar datos de cualquier tamaño, tipo y velocidad de ingesta. Los datos se capturan en un único lugar para análisis operativos y exploratorios.

Se puede acceder a Data Lake Storage Gen1 desde Hadoop (disponible con un clúster de HDInsight) mediante las API REST compatibles con WebHDFS. Data Lake Storage Gen1 está diseñado para habilitar el análisis de los datos almacenados y está optimizado para el rendimiento en escenarios de análisis de datos. Gen1 incluye las funciones que son esenciales para casos de uso empresariales del mundo real. Estas funciones incluyen la seguridad, manejabilidad, adaptabilidad, confiabilidad y disponibilidad.

Para obtener más información sobre Azure Data Lake Storage Gen1, consulte [Información general de Azure Data Lake Storage Gen1](../data-lake-store/data-lake-store-overview.md).

Entre las capacidades clave de Data Lake Storage Gen1 se incluyen las siguientes.

## <a name="compatibility-with-hadoop"></a>Compatibilidad con Hadoop

Data Lake Storage Gen1 es un sistema de archivos de Apache Hadoop compatible con HDFS y el entorno de Hadoop.  Las aplicaciones o los servicios de HDInsight que usen la API de WebHDFS se pueden integrar fácilmente con Data Lake Storage Gen1. Además, Data Lake Storage Gen1 expone una interfaz de REST compatible con WebHDFS para aplicaciones.

Los datos almacenados en Data Lake Storage Gen1 se pueden analizar fácilmente mediante marcos analíticos de Hadoop. Por ejemplo, MapReduce o Hive. Los clústeres de Azure HDInsight se pueden aprovisionar y configurar para que accedan directamente a datos almacenados en Data Lake Storage Gen1.

## <a name="unlimited-storage-petabyte-files"></a>Almacenamiento ilimitado, archivos de petabytes de tamaño

Data Lake Storage Gen1 proporciona almacenamiento ilimitado y es adecuado para almacenar diversos tipos de datos para análisis. No impone límites para los tamaño de cuentas o archivos. Tampoco para la cantidad de datos que se pueden almacenar en el lago de datos. El tamaño de los archivos individuales oscila entre kilobytes y petabytes, por lo que Data Lake Storage Gen1 es una buena opción para almacenar cualquier tipo de datos. Los datos se almacenan de forma duradera mediante la realización de varias copias Además, no hay ningún límite al período de tiempo durante el que se pueden almacenar los datos en el lago de datos.

## <a name="performance-tuning-for-big-data-analytics"></a>Rendimiento optimizado para el análisis de macrodatos

Data Lake Storage Gen1 está diseñado para sistemas de análisis. Sistemas que requieren un rendimiento masivo para consultar y analizar grandes cantidades de datos. Data Lake distribuye partes de un archivo entre varios servidores de almacenamiento individuales. Cuando está analizando los datos, esta configuración mejora el rendimiento de lectura cuando se lee el archivo en paralelo.

## <a name="readiness-for-enterprise-highly-available-and-secure"></a>Preparación para la empresa: durable y con una elevada disponibilidad

Data Lake Storage Gen1 proporciona la confiabilidad y disponibilidad estándar del sector. Los recursos de datos se almacenan de forma duradera realizando copias redundantes para protegerse ante los errores inesperados. Las empresas pueden usar Data Lake Storage Gen1 en sus soluciones como parte importante de su plataforma de datos existente.

Además, Data Lake Storage Gen1 también proporciona seguridad de nivel empresarial para los datos almacenados. Para obtener más información, consulte [Securing data in Azure Data Lake Storage Gen1](#data-security-in-data-lake-storage-gen1) (Protección de los datos almacenados en Azure Data Lake Storage Gen1).

## <a name="flexible-data-structures"></a>Estructuras de datos flexibles

Data Lake Storage Gen1 puede almacenar cualquier dato en su formato nativo, tal cual, sin necesidad de transformarlo antes. Data Lake Storage Gen1 no requiere la definición de un esquema antes de cargar los datos. Cada marco analítico interpreta los datos y define un esquema en el momento del análisis. Data Lake Storage Gen1 puede controlar datos estructurados. Así como datos no estructurados y semiestructurados.

Los contenedores de datos de Data Lake Storage Gen1 son básicamente carpetas y archivos. Se opera en los datos almacenados mediante los SDK, Azure Portal y Azure Powershell. Los datos que se colocan en el almacén con estos contenedores e interfaces pueden almacenar cualquier tipo de datos. Data Lake Storage Gen1 no realiza ningún control especial de datos según el tipo de datos.

## <a name="data-security-in-data-lake-storage-gen1"></a>Seguridad de datos en Data Lake Storage Gen1

Data Lake Storage Gen1 usa Azure Active Directory para la autenticación y listas de control de acceso (ACL) para administrar el acceso a los datos.

| **Característica** | **Descripción** |
| --- | --- |
| Authentication |Data Lake Storage Gen1 se integra con Azure Active Directory (Azure AD) para la administración de identidades y accesos para todos los datos almacenados en Data Lake Storage Gen1. Debido a la integración, Data Lake Storage Gen1 se beneficia de todas las características de Azure AD. Estas características incluyen: autenticación multifactor, acceso condicional y control de acceso basado en roles. Además, se incluye la supervisión del uso de la aplicación, la supervisión de la seguridad, las alertas, etc. Igualmente, Data Lake Storage Gen1 es compatible con el protocolo OAuth 2.0 para la autenticación en la interfaz de REST. Consulte [Autenticación en Azure Data Lake Storage Gen1 con Azure Active Directory](../data-lake-store/data-lakes-store-authentication-using-azure-active-directory.md).|
| Control de acceso |Data Lake Storage Gen1 proporciona control de acceso gracias a la compatibilidad con los permisos de estilo POSIX que expone el protocolo WebHDFS. Las listas de control de acceso se pueden habilitar en la carpeta raíz, en subcarpetas y en archivos individuales. Para obtener más información sobre cómo funcionan las ACL en el contexto de Data Lake Storage Gen1, consulte [Control de acceso en Data Lake Storage Gen1](../data-lake-store/data-lake-store-access-control.md). |
| Cifrado |Data Lake Storage Gen1 también proporciona el cifrado de los datos que se almacenan en la cuenta. Puede especificar la configuración de cifrado mientras crea una cuenta de Data Lake Storage Gen1. Puede elegir si cifrar o no los datos. Para obtener más información, consulte el artículo de [Cifrado de datos en Data Lake Storage Gen1](../data-lake-store/data-lake-store-encryption.md). Para obtener instrucciones sobre cómo proporcionar una configuración relacionada con el cifrado, consulte [Introducción a Azure Data Lake Storage Gen1 con Azure Portal](../data-lake-store/data-lake-store-get-started-portal.md). |

Para obtener más información sobre cómo proteger datos en Data Lake Storage Gen1, consulte [Protección de los datos almacenados en Azure Data Lake Storage Gen1](../data-lake-store/data-lake-store-secure-data.md).

## <a name="applications-that-are-compatible-with-data-lake-storage-gen1"></a>Aplicaciones compatibles con Data Lake Storage Gen1

Data Lake Storage Gen1 es compatible con la mayor parte de los componentes de código abierto del entorno de Hadoop. También se integra perfectamente con otros servicios de Azure.  Para obtener más información acerca de cómo se puede usar Data Lake Storage Gen1 con componentes de código abierto y con otros servicios de Azure, siga estos vínculos.

* Consulte [Abrir aplicaciones de macrodatos de código abierto que funcionan con Azure Data Lake Storage Gen1](../data-lake-store/data-lake-store-compatible-oss-other-applications.md).
* Consulte [Integración de Azure Data Lake Storage Gen1 con otros servicios de Azure](../data-lake-store/data-lake-store-integrate-with-other-services.md) para saber cómo usar Data Lake Storage Gen1 con otros servicios de Azure, y así poder obtener una gama más amplia de escenarios.
* Consulte [Uso de Azure Data Lake Storage Gen1 para requisitos de macrodatos](../data-lake-store/data-lake-store-data-scenarios.md).

## <a name="data-lake-storage-gen1-file-system-adl"></a>Sistema de archivos de Data Lake Storage Gen1 (adl://)

En entornos de Hadoop se puede acceder a Data Lake Storage Gen1 a través del nuevo sistema de archivos, AzureDataLakeFilesystem (adl://). El rendimiento de las aplicaciones y servicios que usan `adl://` se puede optimizar de maneras que no están actualmente disponibles en WebHDFS. Como resultado, se obtiene flexibilidad para disponer del mejor rendimiento mediante la opción recomendada adl://. También puede mantener el código existente mediante el uso continuo de la API de WebHDFS directamente. Azure HDInsight aprovecha completamente AzureDataLakeFilesystem para proporcionar el mejor rendimiento en Data Lake Storage Gen1.

Obtenga acceso a los datos en Data Lake Storage Gen1 mediante el siguiente URI:

`adl://<data_lake_storage_gen1_name>.azuredatalakestore.net`

Para obtener más información sobre cómo acceder a los datos en Data Lake Storage Gen1, consulte [Acciones disponibles con los datos almacenados](../data-lake-store/data-lake-store-get-started-portal.md#properties).

## <a name="next-steps"></a>Pasos siguientes

* [Introducción a Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-introduction.md)
* [Introducción a Almacenamiento de Azure](../storage/common/storage-introduction.md)
* [Introducción a Azure Data Lake Storage Gen2](./overview-data-lake-storage-gen2.md)