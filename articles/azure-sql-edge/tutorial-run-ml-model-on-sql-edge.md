---
title: Implementación del modelo de ML en Azure SQL Edge mediante ONNX
description: En la tercera parte de este tutorial de tres partes de Azure SQL Edge para predecir impurezas de mineral de hierro, ejecutará los modelos de aprendizaje automático de ONNX en SQL Edge.
keywords: ''
services: sql-edge
ms.service: sql-edge
ms.topic: tutorial
author: VasiyaKrishnan
ms.author: vakrishn
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: 9e5bb037b88b7c370e31d05c2d20fc6f558a8b39
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2020
ms.locfileid: "93422202"
---
# <a name="deploy-ml-model-on-azure-sql-edge-using-onnx"></a>Implementación del modelo de ML en Azure SQL Edge mediante ONNX 

En la tercera parte de este tutorial de tres partes para predecir impurezas de mineral de hierro en Azure SQL Edge, hará lo siguiente:

1. Usará Azure Data Studio para conectarse a SQL Database en la instancia de Azure SQL Edge.
2. Predecirá las impurezas de mineral de hierro con ONNX en Azure SQL Edge.

## <a name="key-components"></a>Componentes clave

1. La solución usa un valor predeterminado de 500 milisegundos entre cada mensaje enviado al centro de IoT Edge. Este valor se puede cambiar en el archivo **Program.cs**. 
   ```json
   TimeSpan messageDelay = configuration.GetValue("MessageDelay", TimeSpan.FromMilliseconds(500));
   ```
2. La solución generó un mensaje con los siguientes atributos. Agregue o quite los atributos según los requisitos. 
```json
{
    timestamp 
    cur_Iron_Feed
    cur_Silica_Feed 
    cur_Starch_Flow 
    cur_Amina_Flow 
    cur_Ore_Pulp_pH
    cur_Flotation_Column_01_Air_Flow
    cur_Flotation_Column_02_Air_Flow
    cur_Flotation_Column_03_Air_Flow
    cur_Flotation_Column_04_Air_Flow
    cur_Flotation_Column_01_Level
    cur_Flotation_Column_02_Level
    cur_Flotation_Column_03_Level
    cur_Flotation_Column_04_Level
    cur_Iron_Concentrate
}
```

## <a name="connect-to-the-sql-database-in-the-azure-sql-edge-instance-to-train-deploy-and-test-the-ml-model"></a>Conexión a SQL Database en la instancia de Azure SQL Edge para entrenar, implementar y probar el modelo de Machine Learning

1. Abra Azure Data Studio.

2. En la pestaña **Bienvenida**, inicie una nueva conexión con los detalles siguientes:

   |_Campo_|_Valor_|
   |-------|-------|
   |Tipo de conexión| Microsoft SQL Server|
   |Server|Dirección IP pública mencionada en la VM que se creó para esta demostración.|
   |Nombre de usuario|sa|
   |Contraseña|Contraseña segura que se usó al crear la instancia de Azure SQL Edge.|
   |Base de datos|Valor predeterminado|
   |Grupo del servidor|Valor predeterminado|
   |Nombre (opcional)|Puede proporcionar un nombre opcional.|

3. Haga clic en **Conectar**

4. En la sección **Archivo**, abra **/DeploymentScripts/MiningProcess_ONNX.jpynb** en la carpeta donde ha clonado los archivos de proyecto en la máquina.

5. Establezca el kernel en Python 3.


## <a name="next-steps"></a>Pasos siguientes

Para más información sobre el uso de modelos de ONNX en Azure SQL Edge, consulte [Aprendizaje automático e IA con ONNX en SQL Edge](onnx-overview.md).