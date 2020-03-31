---
title: 'Uso de un equipo Windows con Hadoop en HDInsight: Azure'
description: Trabaje desde un equipo Windows en Hadoop en HDInsight. Administre y consulte clústeres con PowerShell, Visual Studio y herramientas de Linux. Desarrolle soluciones de macrodatos con .NET.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 12/20/2019
ms.openlocfilehash: 3ec50acc693452fe73d929effcea98b12fc5ff8b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75933935"
---
# <a name="work-in-the-apache-hadoop-ecosystem-on-hdinsight-from-a-windows-pc"></a>Trabajo en el ecosistema de Apache Hadoop en HDInsight desde un equipo con Windows

Obtenga información acerca de las opciones de desarrollo y administración en el equipo con Windows para trabajar en el ecosistema de Apache Hadoop en HDInsight.

HDInsight se basa en Apache Hadoop y componentes de Hadoop, tecnologías de código abierto desarrolladas en Linux. HDInsight versión 3.4 y posteriores usan la distribución Ubuntu de Linux como sistema operativo subyacente para el clúster. Sin embargo, puede trabajar con HDInsight desde un cliente Windows o un entorno de desarrollo de Windows.

## <a name="use-powershell-for-deployment-and-management-tasks"></a>Uso de PowerShell para tareas de implementación y administración

Azure PowerShell es un entorno de scripting que se puede usar para controlar y automatizar tareas de implementación y administración en HDInsight desde Windows.

Ejemplos de tareas que puede realizar con PowerShell:

* [Creación de clústeres con PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md).
* [Ejecución de consultas de Apache Hive mediante PowerShell](hadoop/apache-hadoop-use-hive-powershell.md).
* [Administración de clústeres con PowerShell](hdinsight-administer-use-powershell.md).

Siga los pasos para [instalar y configurar Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps) para obtener la versión más reciente.

## <a name="utilities-you-can-run-in-a-browser"></a>Utilidades que puede ejecutar en un explorador

Las utilidades siguientes tienen una interfaz de usuario web que se ejecuta en un explorador:
* **[Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview)** es un shell de línea de comandos interactivo que se ejecuta en el explorador y desde Azure Portal.

* **[Interfaz de usuario web de Apache Ambari](hdinsight-hadoop-manage-ambari.md)** es una utilidad de administración y supervisión disponible en Azure Portal que se puede usar para administrar diferentes tipos de trabajos, como:
    * [Uso de Ambari con la API REST](hdinsight-hadoop-manage-ambari-rest-api.md)
    * [Vista de Apache Hive en Apache Ambari](hadoop/apache-hadoop-use-hive-ambari-view.md)
    * [Vista de Apache Tez en Apache Ambari](hdinsight-debug-ambari-tez-view.md)

## <a name="data-lake-hadoop-tools-for-visual-studio"></a>Herramientas de Data Lake (Hadoop) para Visual Studio

Use Herramientas de Data Lake para Visual Studio para implementar y administrar topologías de Storm. Herramientas de Data Lake también instala el SDK de SCP.NET, que le permite desarrollar topologías de Storm C# con Visual Studio.

Antes de ir a los siguientes ejemplos, [instale y pruebe Herramientas de Data Lake para Visual Studio](hadoop/apache-hadoop-visual-studio-tools-get-started.md).

Ejemplos de tareas que puede hacer con Visual Studio y Herramientas de Data Lake para Visual Studio:
* [Implementación y administración de topologías de Storm desde Visual Studio](storm/apache-storm-deploy-monitor-topology-linux.md)
* [Desarrollo de las topologías de C# para Storm con Visual Studio](storm/apache-storm-develop-csharp-visual-studio-topology.md) Los bits incluyen plantillas de ejemplo para topologías de Storm que puede conectar a bases de datos, como Azure Cosmos DB y SQL Database.

## <a name="visual-studio-and-the-net-sdk"></a>Visual Studio y el SDK de .NET

Puede usar Visual Studio con el SDK de .NET para administrar clústeres y desarrollar aplicaciones de macrodatos. Puede usar otros IDE para las siguientes tareas, pero los ejemplos se muestran en Visual Studio.

Ejemplos de tareas que puede realizar con el SDK de .NET en Visual Studio:
* [SDK de Azure HDInsight para .NET](https://docs.microsoft.com/dotnet/api/overview/azure/hdinsight?view=azure-dotnet).
* [Ejecución de consultas de Apache Hive mediante .NET SDK](hadoop/apache-hadoop-use-hive-dotnet-sdk.md).
* [Uso de funciones definidas por el usuario de C# con el streaming de Apache Hive y Apache Pig en Apache Hadoop](hadoop/apache-hadoop-hive-pig-udf-dotnet-csharp.md).

## <a name="intellij-idea-and-eclipse-ide-for-spark-clusters"></a>Intellij IDEA y Eclipse IDE para clústeres de Spark

Tanto [Intellij IDEA](https://www.jetbrains.com/idea/download) como [Eclipse IDE](https://www.eclipse.org/downloads/) sirven para:
* Desarrollar y enviar una aplicación Spark en Scala en un clúster de Spark en HDInsight.
* Acceder a recursos de clúster de Spark.
* Desarrollar y ejecutar localmente una aplicación Spark en Scala.

En estos artículos se muestra cómo hacerlo:
* Intellij IDEA: [Creación de aplicaciones Apache Spark mediante el complemento Kit de herramientas de Azure para Intellij y el SDK de Scala.](spark/apache-spark-intellij-tool-plugin.md)
* IDE de Eclipse o IDE de Scala para Eclipse: [Creación de aplicaciones Apache Spark y Azure Toolkit for Eclipse](spark/apache-spark-eclipse-tool-plugin.md)

## <a name="notebooks-on-spark-for-data-scientists"></a>Notebooks en Spark para científicos de datos

Los clústeres de Apache Spark en HDInsight incluyen notebooks y kernels de Apache Zeppelin que se pueden usar con cuadernos de Jupyter Notebook.

* [Aprenda a usar kernels en clústeres de Apache Spark con cuadernos de Jupyter Notebook para probar aplicaciones Spark](spark/apache-spark-zeppelin-notebook.md)
* [Aprenda a usar cuadernos de Apache Zeppelin en clústeres de Azure Spark para ejecutar trabajos de Spark](spark/apache-spark-jupyter-notebook-kernels.md)

## <a name="run-linux-based-tools-and-technologies-on-windows"></a>Ejecución en Windows de tecnologías y herramientas basadas en Linux

Si se produce una situación en que debe usar una herramienta o tecnología que solo está disponible en Linux, tenga en cuenta las siguientes opciones:

* **Bash en Ubuntu en Windows 10** proporciona un subsistema de Linux en Windows. Bash permite ejecutar utilidades de Linux directamente sin tener que mantener una instalación de Linux dedicada. Consulte la [Guía de instalación del subsistema de Windows para Linux para Windows 10](https://docs.microsoft.com/windows/wsl/install-win10) para conocer los pasos de instalación.  Otros [shells de Unix](https://www.gnu.org/software/bash/) también funcionarán.
* **Docker para Windows** proporciona acceso a muchas herramientas basadas en Linux y se puede ejecutar directamente desde Windows. Por ejemplo, puede usar Docker para ejecutar al cliente Beeline para Hive directamente desde Windows. También puede usar Docker para ejecutar un notebook de Jupyter local y conectarse de forma remota con Spark en HDInsight. [Introducción a Docker para Windows](https://docs.docker.com/docker-for-windows/)
* **[MobaXTerm](https://mobaxterm.mobatek.net/)** permite examinar gráficamente el sistema de archivos del clúster a través de una conexión SSH.

## <a name="cross-platform-tools"></a>Herramientas multiplataforma

La interfaz de la línea de comandos (CLI) de Azure es la experiencia de línea de comandos multiplataforma de Microsoft para administrar los recursos de Azure.  Para obtener más información, consulte [Interfaz de la línea de comandos (CLI) de Azure](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest).

## <a name="next-steps"></a>Pasos siguientes

Si no ha trabajado antes en clústeres basados en Linux, consulte los artículos siguientes:
* [Configuración de Apache Hadoop, Apache Kafka, Apache Spark u otros clústeres](hdinsight-hadoop-provision-linux-clusters.md)
* [Sugerencias para clústeres de HDInsight en Linux](hdinsight-hadoop-linux-information.md)
