---
title: Administración de fórmulas de Azure DevTest Labs para crear máquinas virtuales | Microsoft Docs
description: En este artículo se explica cómo crear una fórmula a partir de una base (una imagen personalizada, una imagen de Marketplace u otra fórmula) o una máquina virtual existente.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 841dd95a-657f-4d80-ba26-59a9b5104fe4
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2020
ms.author: spelluru
ms.openlocfilehash: 4f8c8d4ff0a8014fe0b9a6ae4aead35ec7df4bf6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79501994"
---
# <a name="manage-azure-devtest-labs-formulas"></a>Administración de fórmulas de Azure DevTest Labs

[!INCLUDE [devtest-lab-formula-definition](../../includes/devtest-lab-formula-definition.md)]

En este artículo se explica cómo crear una fórmula a partir de una base (una imagen personalizada, una imagen de Marketplace u otra fórmula) o una máquina virtual existente. Este artículo también le guía a través de la administración de las fórmulas existentes.

## <a name="create-a-formula"></a>Creación de una fórmula
Cualquier usuario con permisos de *usuarios* de DevTest Labs puede crear máquinas virtuales tomando una fórmula como base. Hay dos maneras de crear fórmulas: 

* Desde una base: use esta forma cuando desee definir todas las características de la fórmula.
* Desde una máquina virtual de laboratorio existente: use esta forma cuando desee crear una fórmula basada en la configuración de una máquina virtual existente.

Para obtener más información sobre cómo agregar usuarios y permisos, consulte [Adición de propietarios y usuarios en Azure DevTest Labs](./devtest-lab-add-devtest-user.md).

### <a name="create-a-formula-from-a-base"></a>Creación de una fórmula desde una base
Los siguientes pasos le guiarán por el proceso de creación de una fórmula a partir de una imagen personalizada, una imagen de Marketplace u otra fórmula.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

2. Seleccione **Todos los servicios** y, luego, **DevTest Labs** en la lista.

3. En la lista de laboratorios, seleccione el laboratorio que desee.  

4. En la página del laboratorio, seleccione **Fórmulas (bases reutilizables)** en el menú de la izquierda.
5. En la página **Fórmulas**, seleccione **+ Agregar**.
   
    ![Agregar una fórmula](./media/devtest-lab-create-formulas/add-formula.png)

6. En la hoja **Elegir una base**, seleccione la base (imagen personalizada o imagen de Marketplace) a partir de la cual quiere crear la fórmula.
7. En la pestaña **Configuración básica** de la página **Crear fórmula**, especifique los valores siguientes:
   
    * **Nombre de fórmula** : escriba un nombre para la fórmula. Este valor se mostrará en la lista de imágenes base al crear la máquina virtual. El nombre se valida a medida que lo escribe y, si no es válido, un mensaje le indicará los requisitos para un nombre válido.
    - Escriba una **descripción** opcional para la fórmula. 
    * **Nombre de usuario**: escriba un nombre de usuario al que se concederán privilegios de administrador.
    * **Contraseña** : escriba, o seleccione en la lista desplegable, un valor que esté asociado con el secreto (contraseña) que desee usar para el usuario especificado. Para información sobre cómo guardar secretos en un almacén de claves y utilizarlos al crear recursos de laboratorio, consulte [Store secrets in Azure Key Vault](devtest-lab-store-secrets-in-key-vault.md) (Almacenamiento de secretos en un almacén de claves de Azure).

        Seleccione **Usar un secreto guardado** si desea utilizar un secreto de Azure Key Vault en lugar de una contraseña. 
    * **Tamaño de máquina virtual**: seleccione **Cambiar tamaño** para cambiar el tamaño de la VM. 
    - **Tipo de disco de sistema operativo**: seleccione el tipo de disco que desea usar (HDD estándar, SSD estándar o SSD Premium).
    * **Artefactos**: seleccione la página **Add or Remove Artifacts** (Agregar o quitar artefactos), en la que podrá seleccionar y configurar los artefactos que quiere agregar a la imagen base. Para más información acerca de los artefactos, consulte [Creación de artefactos personalizados para la máquina virtual de Azure DevTest Labs](devtest-lab-artifact-author.md).

        ![Página Configuración básica](./media/devtest-lab-create-formulas/basic-settings.png)
8. Cambie a la pestaña **Configuración avanzada** y especifique los siguientes valores:
    - **Red virtual**: para cambiar la red virtual en la que se encuentra la máquina virtual, seleccione **Cambiar la red virtual**. 
    - **Subred**: para cambiar la subred, seleccione **Cambiar la subred**. 
    - **Configuración de dirección IP**: especifique si desea direcciones IP públicas, privadas o compartidas. Para obtener más información sobre las direcciones IP compartidas, consulte [Understand shared IP addresses in Azure DevTest Labs](./devtest-lab-shared-ip.md) (Direcciones IP compartidas en Azure DevTest Labs).
    - **Fecha y hora de expiración**: no puede editar este campo. 
    - **Make this machine claimable** (Poder reclamar esta máquina): poder reclamar una máquina significa que no se podrá asignar la propiedad durante su creación. En su lugar, los usuarios del laboratorio podrán asumir propiedad ("reclamar") de la máquina en la hoja del laboratorio.  

        ![Página Configuración básica](./media/devtest-lab-create-formulas/advanced-settings.png)
8. Seleccione **Enviar** para crear la fórmula.

9. Cuando se ha creado la fórmula, se muestra en la lista de la página **Fórmulas**.

### <a name="create-a-formula-from-a-vm"></a>Creación de una fórmula desde una máquina virtual
Los siguientes pasos le guiarán a través del proceso de creación de una fórmula basada en una máquina virtual existente. 

> [!NOTE]
> Para crear una fórmula desde una máquina virtual, la máquina virtual debe haberse creado después del 30 de marzo de 2016. 
> 
> 

1. Inicie sesión en [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Seleccione **Todos los servicios** y, luego, **DevTest Labs** en la lista.
3. En la lista de laboratorios, seleccione el laboratorio que desee.  
4. En la página **Información general** del laboratorio, seleccione la máquina virtual desde la que desee crear la fórmula.
   
    ![Máquinas virtuales de laboratorios](./media/devtest-lab-create-formulas/my-vms.png)
5. En la página de la máquina virtual, seleccione **Crear fórmula (base reutilizable)** .
   
    ![Crear fórmula](./media/devtest-lab-create-formulas/create-formula-menu.png)
6. En la página **Crear fórmula**, escriba un **Nombre** y una **Descripción** para la nueva fórmula.
   
    ![Página Crear fórmula](./media/devtest-lab-create-formulas/create-formula-blade.png)
7. Seleccione **Aceptar** para crear la fórmula.

## <a name="modify-a-formula"></a>Modificación de una fórmula
Para modificar una fórmula, siga estos pasos:

1. Inicie sesión en [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Seleccione **Todos los servicios** y, luego, **DevTest Labs** en la lista.
3. En la lista de laboratorios, seleccione el laboratorio que desee.  
4. En la página del laboratorio, seleccione **Fórmulas (bases reutilizables)** .
   
    ![Menú Fórmula](./media/devtest-lab-manage-formulas/lab-settings-formulas.png)
5. En la hoja **Lab formulas** (Fórmulas de laboratorio), seleccione la fórmula que quiere modificar.
6. En la página **Actualizar fórmula**, realice las modificaciones deseadas y seleccione **Actualizar**.

## <a name="delete-a-formula"></a>Eliminación de una fórmula
Para eliminar una fórmula, siga estos pasos:

1. Inicie sesión en [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Seleccione **Todos los servicios** y, luego, **DevTest Labs** en la lista.
3. En la lista de laboratorios, seleccione el laboratorio que desee.  
4. En la página **Configuración** del laboratorio, seleccione **Fórmulas**.
   
    ![Menú Fórmula](./media/devtest-lab-manage-formulas/lab-settings-formulas.png)
5. En la página **Lab formulas** (Fórmulas de laboratorio), seleccione los puntos suspensivos a la derecha de la fórmula que desea eliminar.
   
    ![Menú Fórmula](./media/devtest-lab-manage-formulas/lab-formulas-blade.png)
6. En el menú contextual de la fórmula, seleccione **Eliminar**.
   
    ![Menú contextual de fórmula](./media/devtest-lab-manage-formulas/formula-delete-context-menu.png)
7. Seleccione **Sí** en el cuadro de diálogo de confirmación de eliminación.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Entradas blogs relacionadas
* [Custom images or formulas? (¿Imágenes personalizadas o fórmulas?)](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)

## <a name="next-steps"></a>Pasos siguientes
Cuando haya creado una fórmula para usarla al crear una máquina virtual, el siguiente paso consiste en [agregar una máquina virtual al laboratorio](devtest-lab-add-vm.md).

