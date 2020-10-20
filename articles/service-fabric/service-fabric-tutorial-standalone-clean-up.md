---
title: Limpieza de un clúster independiente
description: En este tutorial aprenderá a eliminar los recursos de AWS o de Azure del clúster de Service Fabric independiente.
ms.topic: tutorial
ms.date: 07/22/2019
ms.custom: mvc
ms.openlocfilehash: 0d46e9068a311594f779411c3ccee2b408febb3f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91842893"
---
# <a name="tutorial-clean-up-your-standalone-cluster"></a>Tutorial: limpieza del clúster independiente

Los clústeres de Service Fabric independientes ofrecen la opción de elegir un entorno propio para hospedar Service Fabric. En esta serie de tutoriales se crea un clúster independiente hospedado en AWS o Azure y se implementa en él una aplicación.

Este tutorial es la cuarta parte de una serie. En esta parte del tutorial se muestra cómo eliminar los recursos de AWS o de Azure que se crearon para hospedar el clúster de Service Fabric.

En este artículo aprenderá lo siguiente:

> [!div class="checklist"]
> * Eliminación de un clúster de Service Fabric.
> * Eliminación de los recursos de AWS o de Azure.

## <a name="remove-a-service-fabric-cluster"></a>Eliminación de un clúster de Service Fabric.

1. Utilice RDP en la máquina virtual que usó para instalar Service Fabric.
2. Abra PowerShell.
3. Cambie el directorio a la carpeta extraída del segundo tutorial.
4. Ejecute el comando siguiente para eliminar el clúster de Service Fabric:

  ```powershell
  .\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json
  ```

5. Cuando se le solicite, escriba `Y`. Si se realizó correctamente, la salida tendrá un aspecto similar al siguiente (con sus propias direcciones IP):

  ```powershell
  Best Practices Analyzer completed successfully.
  Removing configuration from machine 172.31.21.141
  Removing configuration from machine 172.31.27.1
  Removing configuration from machine 172.31.20.163
  Configuration removed from machine 172.31.21.141
  Configuration removed from machine 172.31.27.1
  Configuration removed from machine 172.31.20.163
  The cluster is successfully removed.
  ```

## <a name="delete-aws-resources"></a>Eliminación de los recursos de AWS

1. Inicie sesión en la cuenta de AWS.
2. Vaya a la consola de EC2.
3. Seleccione los tres nodos que creó en la primera parte del tutorial.
4. Seleccione **Actions** > **Instance State** > **Terminate** (Acciones > Estado de instancia > Terminar).

## <a name="delete-azure-resources"></a>Eliminación de recursos de Azure

1. Inicie sesión en Azure Portal.
2. Vaya a la sección **Máquinas virtuales**.
3. Active las casillas de los tres nodos que creó en la primera parte del tutorial.
4. Seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió a eliminar los recursos que se crearon en los pasos anteriores.

> [!div class="checklist"]
> * Limpiar los recursos

> [!div class="nextstepaction"]
> [Volver al principio](service-fabric-tutorial-standalone-create-infrastructure.md)
