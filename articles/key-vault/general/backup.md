---
title: Copia de seguridad de Azure Key Vault | Microsoft Docs
description: Use este documento para ayudar a realizar copias de seguridad de un secreto, una clave o un certificado almacenados en Azure Key Vault.
services: key-vault
author: ShaneBala-keyvault
manager: ravijan
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 08/12/2019
ms.author: sudbalas
ms.openlocfilehash: 8a152e2771f0b207e81f42c6ecae2e4d14605051
ms.sourcegitcommit: 5cace04239f5efef4c1eed78144191a8b7d7fee8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86156384"
---
# <a name="azure-key-vault-backup"></a>Copia de seguridad de Azure Key Vault

En este documento se muestra cómo realizar una copia de seguridad de los secretos, claves y certificados individuales almacenados en el almacén de claves. Esta copia de seguridad está diseñada para proporcionarle una copia sin conexión de todos los secretos en el caso improbable de que pierda el acceso al almacén de claves.

## <a name="overview"></a>Información general

Key Vault proporciona automáticamente varias características para mantener la disponibilidad y evitar la pérdida de datos. Esta copia de seguridad solo se debe intentar si hay una justificación empresarial crítica para mantener una copia de seguridad de los secretos. La copia de seguridad de los secretos en el almacén de claves puede introducir desafíos operativos adicionales, como el mantenimiento de varios conjuntos de registros, permisos y copias de seguridad cuando los secretos expiran o rotan.

Key Vault mantiene la disponibilidad en escenarios de desastre y realizará automáticamente una conmutación por error de las solicitudes a una región emparejada sin que sea necesaria la intervención del usuario. Para más información, consulte el vínculo siguiente. [Guía de recuperación ante desastres para el servicio de Azure Key Vault](https://docs.microsoft.com/azure/key-vault/general/disaster-recovery-guidance)

Key Vault protege contra la eliminación accidental y malintencionada de los secretos mediante la protección de eliminación y purga de software. Si desea protección contra la eliminación accidental o malintencionada de los secretos, configure las características de protección de eliminación temporal y purga en el almacén de claves. Para más información, consulte el documento siguiente. [Recuperación de Azure Key Vault](https://docs.microsoft.com/azure/key-vault/general/overview-soft-delete)

## <a name="limitations"></a>Limitaciones

Azure Key Vault no tiene actualmente una forma de realizar una copia de seguridad de un almacén de claves completo en una sola operación. Ni Microsoft ni el equipo de Azure Key Vault permitirán el uso de los comandos enumerados en este documento para llevar a cabo una copia de seguridad automatizada de un almacén de claves.

Si intenta utilizar los comandos mostrados en el documento siguiente para crear una automatización personalizada, pueden producirse errores.

* La copia de seguridad de secretos con varias versiones puede provocar errores de tiempo de espera.
* La copia de seguridad crea una instantánea de un momento dado. Los secretos pueden renovarse durante una copia de seguridad, lo que provoca una falta de coincidencia de las claves de cifrado.
* Si se superan los límites de servicio del almacén de claves para las solicitudes por segundo, el almacén de claves se limitará y se producirá un error en la copia de seguridad.

## <a name="design-considerations"></a>Consideraciones de diseño

Al realizar una copia de seguridad de un objeto almacenado en el almacén de claves (secreto, clave o certificado), la operación de copia de seguridad descarga el objeto como un blob cifrado. Este blob no se puede descifrar fuera de Azure. Para obtener datos que se puedan usar de este blob, debe restaurar el blob en un almacén de claves dentro de la misma suscripción de Azure y en la zona geográfica de Azure.
[Zonas geográficas de Azure](https://azure.microsoft.com/global-infrastructure/geographies/)

## <a name="prerequisites"></a>Requisitos previos

* Permisos de nivel de colaborador o superior en una suscripción de Azure
* Un almacén de claves principal que contenga los secretos de los que desee realizar una copia de seguridad
* Un almacén de claves secundario en el que se restaurarán los secretos.

## <a name="back-up-and-restore-with-azure-portal"></a>Copia de seguridad y restauración con Azure Portal

### <a name="back-up"></a>Copia de seguridad

1. Acceda a Azure Portal.
2. Seleccione el almacén de claves.
3. Vaya al objeto (secreto, clave o certificado) del que desea realizar una copia de seguridad.

    ![Imagen](../media/backup-1.png)

4. Seleccione el objeto.
5. Selección de "Descargar copia de seguridad"

    ![Imagen](../media/backup-2.png)
    
6. Haga clic en el botón "Descargar".

    ![Imagen](../media/backup-3.png)
    
7. Almacene el blob cifrado en una ubicación segura.

### <a name="restore"></a>Restauración

1. Acceda a Azure Portal.
2. Seleccione el almacén de claves.
3. Desplácese hasta el tipo de objeto (secreto, clave o certificado) que desee restaurar.
4. Seleccione "Restaurar copia de seguridad".

    ![Imagen](../media/backup-4.png)
    
5. Vaya a la ubicación donde almacenó el blob cifrado.
6. Seleccione "Aceptar".

## <a name="back-up-and-restore-with-the-azure-cli"></a>Copia de seguridad y restauración con la CLI de Azure

```azurecli
## Login To Azure
az login

## Set your Subscription
az account set --subscription {AZURE SUBSCRIPTION ID}

## Register Key Vault as a Provider
az provider register -n Microsoft.KeyVault

## Backup a Certificate in Key Vault
az keyvault certificate backup --file {File Path} --name {Certificate Name} --vault-name {Key Vault Name} --subscription {SUBSCRIPTION ID}

## Backup a Key in Key Vault
az keyvault key backup --file {File Path} --name {Key Name} --vault-name {Key Vault Name} --subscription {SUBSCRIPTION ID}

## Backup a Secret in Key Vault
az keyvault secret backup --file {File Path} --name {Secret Name} --vault-name {Key Vault Name} --subscription {SUBSCRIPTION ID}

## Restore a Certificate in Key Vault
az keyvault certificate restore --file {File Path} --vault-name {Key Vault Name} --subscription {SUBSCRIPTION ID}

## Restore a Key in Key Vault
az keyvault key restore --file {File Path} --vault-name {Key Vault Name} --subscription {SUBSCRIPTION ID}

## Restore a Secret in Key Vault
az keyvault secret restore --file {File Path} --vault-name {Key Vault Name} --subscription {SUBSCRIPTION ID}

```

## <a name="next-steps"></a>Pasos siguientes

Active el registro y la supervisión para Azure Key Vault. [Registro de Azure Key Vault](https://docs.microsoft.com/azure/key-vault/general/logging)
