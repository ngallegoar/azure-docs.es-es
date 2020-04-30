---
title: Implementación de una VM desde Azure Marketplace
description: Explica cómo implementar una máquina virtual desde una máquina virtual preconfigurada de Azure Marketplace.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 11/29/2018
ms.author: dsindona
ms.openlocfilehash: 2fb04e2e953e2bcbe479f7685b8042fdc7ea1767
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82146994"
---
# <a name="deploy-a-virtual-machine-from-the-azure-marketplace"></a>Implementación de una máquina virtual desde Azure Marketplace

> [!IMPORTANT]
> A partir del 13 de abril de 2020, comenzaremos el traslado de la administración de las ofertas de máquinas virtuales de Azure al Centro de partners. Después de la migración, las ofertas se crearán y administrarán en el Centro de partners. Siga las instrucciones que se indican en [Certificación de imágenes de máquina virtual de Azure](https://docs.microsoft.com/azure/marketplace/partner-center-portal/azure-vm-image-certification) para administrar las ofertas migradas.

En este artículo se explica cómo implementar una máquina virtual (VM) configurada previamente desde Azure Marketplace, mediante el script de Azure PowerShell proporcionado.  Este script también expone los puntos de conexión HTTP y HTTPS de WinRM en la VM.  El script requiere que ya tenga un certificado cargado en Azure Key Vault, como se describe en [Create certificates for Azure Key Vault](./cpp-create-key-vault-cert.md) (Creación de certificados para Azure Key Vault). 

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

## <a name="vm-deployment-template"></a>Plantilla de implementación de VM

La plantilla de implementación de VM de Azure de inicio rápido está disponible como el archivo en línea [azuredeploy.json](https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-vm-winrm-keyvault-windows/azuredeploy.json).  Contiene los parámetros siguientes:

|  **Parámetro**        |   **Descripción**                                 |
|  -------------        |   ---------------                                 |
| newStorageAccountName    | Nombre de la cuenta de almacenamiento                       |
| dnsNameForPublicIP    | Nombre DNS para la dirección IP pública. Debe estar en minúscula.    |
| adminUserName            | Nombre de usuario del administrador                          |
| adminPassword            | Contraseña del administrador                          |
| imagePublisher        | Editor de la imagen                                   |
| imageOffer            | Oferta de la imagen                                       |
| imageSku                | SKU de la imagen                                         |
| vmSize                | Tamaño de la VM                                    |
| vmName                | Nombre de la máquina virtual                                    |
| vaultName                | Nombre del almacén de claves.                             |
| vaultResourceGroup    | Grupo de recursos del almacén de claves.                   |
| certificateUrl        | Dirección URL para el certificado, incluida la versión en KeyVault, por ejemplo `https://testault.vault.azure.net/secrets/testcert/b621es1db241e56a72d037479xab1r7` |
|  |  |


## <a name="deployment-script"></a>Script de implementación

Edite el siguiente script de Azure PowerShell y ejecútelo para implementar la VM de Azure Marketplace especificada.

```powershell

New-AzResourceGroupDeployment -Name "dplvm$postfix" -ResourceGroupName "$rgName" -TemplateUri "https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-vm-winrm-keyvault-windows/azuredeploy.json" -newStorageAccountName "test$postfix" -dnsNameForPublicIP $vmName -adminUserName "isv" -adminPassword $pwd -vmSize "Standard_A2" -vmName $vmName -vaultName "$kvname" -vaultResourceGroup "$rgName" -certificateUrl $objAzureKeyVaultSecret.Id 

```


## <a name="next-steps"></a>Pasos siguientes

Una vez que ha implementado una VM preconfigurada, puede configurar y acceder a las soluciones y los servicios que contiene, o usarla para su posterior desarrollo. 
