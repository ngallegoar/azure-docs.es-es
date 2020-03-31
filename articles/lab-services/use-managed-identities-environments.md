---
title: Uso de identidades administradas de Azure para crear entornos en DevTest Labs | Microsoft Docs
description: Obtenga más información sobre el uso de identidades administradas en Azure para implementar entornos en un laboratorio de Azure DevTest Labs.
services: devtest-lab,lab-services
documentationcenter: na
author: spelluru
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/01/2019
ms.author: spelluru
ms.openlocfilehash: a4ba4206c01e492f2ae980c5806de1e72c7051c3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "73931151"
---
# <a name="use-azure-managed-identities-to-deploy-environments-in-a-lab"></a>Uso de identidades administradas de Azure para implementar entornos en un laboratorio 
Como propietario de un laboratorio, puede usar una identidad administrada para implementar entornos en un laboratorio. Esta característica es útil en escenarios en los que el entorno contiene o incluye referencias a recursos de Azure, como almacenes de claves, galerías de imágenes compartidas y redes que son externas al grupo de recursos del entorno. Permite la creación de entornos de espacio aislado que no se limitan al grupo de recursos de dicho entorno.

> [!NOTE]
> Actualmente, se admite una única identidad asignada por el usuario por laboratorio. 

## <a name="prerequisites"></a>Prerequisites
- [Creación, enumeración, eliminación o asignación de un rol a una identidad administrada asignada por el usuario mediante Azure Portal](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md). 

## <a name="use-azure-portal"></a>Usar Azure Portal
En esta sección, como propietario del laboratorio, use Azure Portal para agregar una identidad administrada por el usuario al laboratorio. 

1. En la página del laboratorio, seleccione **Configuración y directivas**. 
1. En la sección **Configuración**, seleccione **Identidad**.
1. Para agregar una identidad asignada por el usuario, seleccione **Agregar** en la barra de herramientas. 
1. Seleccione una **identidad** de una lista desplegable rellenada previamente.
1. Seleccione **Aceptar**.

    ![Adición de una identidad administrada por el usuario](./media/use-managed-identities-environments/add-user-managed-identity.png)
2. Verá la identidad administrada por el usuario agregada en la lista. 

    ![Identidad administrada por el usuario en la lista](./media/use-managed-identities-environments/identity-in-list.png)

Una vez guardado, el laboratorio usará esta identidad al implementar todos los entornos de laboratorio. También puede acceder al recurso de la identidad en Azure mediante la selección de la identidad en la lista. 

El propietario del laboratorio no necesita hacer nada especial al implementar un entorno siempre que la identidad agregada al laboratorio tenga permisos para los recursos externos a los que el laboratorio necesita tener acceso. 

Para cambiar la identidad administrada por el usuario asignada al laboratorio, primero debe quitar la identidad que se ha adjuntado al laboratorio y, a continuación, agregar otra al laboratorio. Para quitar una identidad adjuntada al laboratorio, seleccione **... (puntos suspensivos)** y haga clic en **Quitar**. 

![Identidad administrada por el usuario en la lista](./media/use-managed-identities-environments/replace-identity.png)  

## <a name="use-api"></a>Uso de la API

1. Tras crear una identidad, anote su id. de recurso. Debería ser similar al ejemplo siguiente: 

    `/subscriptions/0000000000-0000-0000-0000-00000000000000/resourceGroups/<RESOURCE GROUP NAME> /providers/Microsoft.ManagedIdentity/userAssignedIdentities/<NAME of USER IDENTITY>`.
1. Ejecute un método HTTPS PUT para agregar un nuevo recurso `ServiceRunner` al laboratorio similar al ejemplo siguiente. El recurso del ejecutor del servicio es un recurso de proxy para administrar y controlar identidades administradas en DevTest Labs. El nombre del ejecutor del servicio puede ser cualquier nombre válido, pero se recomienda usar el nombre del recurso de identidad administrada. 
 
    ```json
    PUT https://management.azure.com/subscriptions/{subId}/resourceGroups/{rg}/providers/Microsoft.Devtestlab/labs/{yourlabname}/serviceRunners/{serviceRunnerName}

    {
        "location": "{location}",
        "identity":{
            "type": "userAssigned",
            "userAssignedIdentities":{
                "[userAssignedIdentityResourceId]":{}
            }
        }
        "properties":{
            "identityUsageType":"Environment"
                     }
          
    }
    ```
 
    Este es un ejemplo: 

    ```json
    PUT https://management.azure.com/subscriptions/0000000000-0000-0000-0000-000000000000000/resourceGroups/exampleRG/providers/Microsoft.Devtestlab/labs/mylab/serviceRunners/sampleuseridentity

    {
        "location": "eastus",
        "identity":{
            "type": "userAssigned",
            "userAssignedIdentities":{
                "/subscriptions/0000000000-0000-0000-0000-000000000000000/resourceGroups/exampleRG/providers/Microsoft.ManagedIdentity/userAssignedIdentities/sampleuseridentity":{}
            }
        }
        "properties":{
            "identityUsageType":"Environment"
                     }
    }
    ```
 
Una vez que la identidad asignada por el usuario se agrega al laboratorio, el servicio de Azure DevTest Labs la usará al implementar entornos de Azure Resource Manager. Por ejemplo, si necesita la plantilla de Resource Manager para tener acceso a una imagen externa de la galería de imágenes compartidas, asegúrese de que la identidad que ha agregado al laboratorio tiene los permisos mínimos necesarios para el recurso de la galería de imágenes compartidas. 
