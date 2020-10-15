---
title: Autenticación de runbooks de Azure Automation con Amazon Web Services
description: En este artículo se explica como autenticar runbooks con Amazon Web Services.
keywords: autenticación AWS, configurar aws
services: automation
ms.subservice: process-automation
ms.date: 04/23/2020
ms.topic: conceptual
ms.openlocfilehash: acb056fb959fda320a14059e2b36a0f5e3fb0b37
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "83837199"
---
# <a name="authenticate-runbooks-with-amazon-web-services"></a>Autenticación de runbooks con Amazon Web Services

La automatización de las tareas comunes con recursos de Amazon Web Services (AWS) se puede realizar con los runbooks de Azure Automation. Puede automatizar muchas tareas en AWS mediante Runbooks de Automation exactamente igual que con recursos de Azure. Para la autenticación, debe tener una suscripción de Azure.

## <a name="obtain-aws-subscription-and-credentials"></a>Obtención de las credenciales y la suscripción de AWS

Para realizar la autenticación con AWS, debe obtener una suscripción AWS y especificar un conjunto de credenciales de AWS para autenticar los Runbooks que se ejecutan desde Azure Automation. Las credenciales específicas necesarias son la clave de acceso AWS y la clave secreta. Vea [Usar credenciales de AWS](https://docs.aws.amazon.com/powershell/latest/userguide/specifying-your-aws-credentials.html).

## <a name="configure-automation-account"></a>Configuración de la cuenta de Automation

Puede usar una cuenta de Automation existente para autenticarse con AWS. Como alternativa, puede dedicar una cuenta para runbooks que tengan como destino recursos de AWS. En este caso, cree una nueva [cuenta de Automation](automation-create-standalone-account.md).  

## <a name="store-aws-credentials"></a>Store de credenciales de AWS

Debe almacenar las credenciales de AWS como recursos en Azure Automation. Vea [Administración de claves de acceso para la cuenta de AWS](https://docs.aws.amazon.com/general/latest/gr/managing-aws-access-keys.html) para obtener instrucciones sobre cómo crear la clave de acceso y la clave secreta. Cuando las claves estén disponibles, copie el id. de la clave de acceso y el id. de la clave secreta en un lugar seguro. Puede descargar el archivo de clave para almacenarlo en un lugar seguro.

## <a name="create-credential-asset"></a>Crear recurso de credencial

Después de crear y copiar las claves de seguridad de AWS, debe crear un recurso de credencial con la cuenta de Automation. El recurso permite almacenar de forma segura las claves de AWS y hacer referencia a ellas en los runbooks. Vea [Crear un nuevo recurso de credencial con Azure Portal](shared-resources/credentials.md#create-a-new-credential-asset-with-the-azure-portal). Escriba la siguiente información de AWS en los campos correspondientes:
    
* **Nombre** - **AWScred** o un valor adecuado que siga las normas de nomenclatura
* **Nombre de usuario**: el id. de acceso
* **Contraseña**: el nombre de la clave secreta 

## <a name="next-steps"></a>Pasos siguientes

* Para obtener información sobre cómo crear runbooks para automatizar las tareas en AWS, vea [Implementación de una máquina virtual de Amazon Web Services con un runbook](automation-scenario-aws-deployment.md).