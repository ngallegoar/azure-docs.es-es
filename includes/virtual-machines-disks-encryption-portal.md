---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 03/23/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: b8073240bdda38757a5e4feee66c9f54746966c4
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2020
ms.locfileid: "80632043"
---
### <a name="portal"></a>Portal

La configuración de claves administradas por el cliente para los discos requerirá la creación de recursos en un orden determinado, si lo hace por primera vez. En primer lugar, tendrá que crear y configurar una instancia de Azure Key Vault.

#### <a name="setting-up-your-azure-key-vault"></a>Configuración de Azure Key Vault

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com/).
1. Busque y seleccione **Key Vaults**.

    [ ![sse-key-vault-portal-search.png](media/virtual-machines-disk-encryption-portal/sse-key-vault-portal-search.png)](media/virtual-machines-disk-encryption-portal/sse-key-vault-portal-search-expanded.png#lightbox)

    > [!IMPORTANT]
    > La instancia de Azure Key Vault, el conjunto de cifrado de disco, la VM, los discos y las instantáneas deben estar en la misma región y suscripción para que la implementación se realice correctamente.

1. Seleccione **+Agregar** para crear una nueva instancia de Key Vault.
1. Cree un nuevo grupo de recursos.
1. Escriba un nombre de almacén de claves, seleccione una región y seleccione un plan de tarifa.
1. Seleccione **Revisar y crear**, compruebe las opciones y, a continuación, seleccione **Crear**.

    ![Captura de pantalla de la experiencia de creación de Azure Key Vault. Muestra los valores concretos que ha creado](media/virtual-machines-disk-encryption-portal/sse-create-a-key-vault.png)

1. Una vez que el almacén de claves termine de implementarse, selecciónelo.
1. En **Configuración**, seleccione **Claves**.
1. Seleccione **Generar o importar**.

    ![Captura de pantalla del panel de configuración de recursos de Key Vault. Muestra el botón Generar/importar en la configuración.](media/virtual-machines-disk-encryption-portal/sse-key-vault-generate-settings.png)

1. Deje **Tipo de clave** establecido en **RSA** y **Tamaño de la clave RSA** establecido en **2048**.
1. Rellene las selecciones restantes como desee y, a continuación, seleccione **Crear**.

    ![Captura de pantalla de la hoja Crear una clave que aparece cuando se selecciona el botón Generar/importar](media/virtual-machines-disk-encryption-portal/sse-create-a-key-generate.png)

#### <a name="setting-up-your-disk-encryption-set"></a>Configuración del conjunto de cifrado de disco

1. Busque **conjuntos de cifrado de disco** y seleccione la opción.
1. Abra la hoja **Conjuntos de cifrado de disco** y seleccione **+Agregar**.

    ![Captura de pantalla de la pantalla principal del portal de cifrado de discos. Resaltado del botón Agregar](media/virtual-machines-disk-encryption-portal/sse-create-disk-encryption-set.png)

1. Seleccione el grupo de recursos, asigne un nombre al conjunto de cifrado y seleccione la misma región que el almacén de claves.
1. Seleccione **Almacén de claves y clave**.
1. Seleccione el almacén de claves y la clave que creó anteriormente, así como la versión.
1. Haga clic en **Seleccionar**.
1. Seleccione **Revisar y crear** y, a continuación, **Crear**.

    ![Captura de pantalla de la hoja de creación del cifrado de disco. Se muestra la suscripción, el grupo de recursos, el nombre del conjunto de cifrado de disco, la región y el selector de claves y de almacenes de claves.](media/virtual-machines-disk-encryption-portal/sse-disk-enc-set-blade-key.png)

1. Abra el conjunto de cifrado de disco una vez que termine de crear y seleccione la alerta que aparece.

    ![Captura de pantalla del elemento emergente de alertas: "Para asociar un disco, una imagen o una instantánea a este conjunto de cifrado de disco, debe conceder permisos al almacén de claves". Seleccione esta alerta para continuar](media/virtual-machines-disk-encryption-portal/sse-disk-enc-alert-fix.png)

Dos notificaciones deberían aparecer y completarse correctamente. Esto le permitirá usar el conjunto de cifrado de disco con el almacén de claves.

![Captura de pantalla de la asignación de roles y permisos correctos para el almacén de claves.](media/virtual-machines-disk-encryption-portal/disk-enc-notification-success.png)

#### <a name="deploy-a-vm"></a>Implementación de una máquina virtual

Ahora que ha creado y configurado el almacén de claves y el conjunto de cifrado de disco, puede implementar una VM mediante el cifrado.
El proceso de implementación de VM es similar al proceso de implementación estándar, las únicas diferencias son que debe implementar la VM en la misma región que los demás recursos y optar por usar una clave administrada por el cliente.

1. Busque **Máquinas virtuales** y seleccione **+Agregar** para crear una VM.
1. En la pestaña **Básico**, seleccione la misma región que el conjunto de cifrado de disco y Azure Key Vault.
1. Rellene los demás valores en la pestaña **Básico** como desee.

    ![Captura de pantalla de la experiencia de creación de la máquina virtual, con el valor de región resaltado.](media/virtual-machines-disk-encryption-portal/sse-create-a-vm-region.png)

1. En la pestaña **Discos**, seleccione **Cifrado en reposo con una clave administrada por el cliente**.
1. Seleccione el conjunto de cifrado de disco en el menú desplegable **Conjunto de cifrado de disco**.
1. Realice las selecciones restantes como desee.

    ![Captura de pantalla de la experiencia de creación de la máquina virtual, con la de hoja discos. Con la lista desplegable conjunto de cifrado de disco resaltada.](media/virtual-machines-disk-encryption-portal/sse-create-vm-select-cmk-encryption-set.png)

#### <a name="enable-on-an-existing-disk"></a>Habilitación en un disco existente

> [!CAUTION]
> Para habilitar el cifrado de disco en todos los discos conectados a una VM, será necesario detener la VM.
    
1. Vaya a una VM que esté en la misma región que uno de los conjuntos de cifrado de disco.
1. Abra la VM y seleccione **Detener**.

    ![Captura de pantalla de la superposición principal de la máquina virtual de ejemplo. Con el botón de detención resaltado](media/virtual-machines-disk-encryption-portal/sse-stop-VM-to-encrypt-disk.png)

1. Una vez finalizada la detención de la VM, seleccione **Discos** y, después, seleccione el disco que desea cifrar.

    ![Captura de pantalla de la máquina virtual de ejemplo, con la hoja de discos abierta. El disco del sistema operativo está resaltado, como un disco de ejemplo para que lo seleccione.](media/virtual-machines-disk-encryption-portal/sse-existing-disk-select.png)

1. Seleccione **Cifrado** y seleccione **Cifrado en reposo con una clave administrada por el cliente** y, a continuación, seleccione el conjunto de cifrado de disco en la lista desplegable.
1. Seleccione **Guardar**.

    ![Captura de pantalla del disco del sistema operativo de ejemplo. La hoja cifrado está abierta, el cifrado en reposo con una clave administrada por el cliente está resaltada, así como la instancia de Azure Key Vault de ejemplo. Después de efectuar esas selecciones, se selecciona el botón Guardar.](media/virtual-machines-disk-encryption-portal/sse-encrypt-existing-disk-customer-managed-key.png)

1. Repita este proceso para todos los demás discos conectados a la VM que desea cifrar.
1. Cuando los discos terminen de cambiar a las claves administradas por el cliente, si no hay ningún otro disco conectado que quiera cifrar, puede iniciar la VM.
