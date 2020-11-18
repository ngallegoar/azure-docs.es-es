---
title: Actualización local de imágenes de Red Hat Enterprise Linux en Azure
description: Encuentre los pasos para realizar la actualización local desde imágenes de Red Hat Enterprise 7.x a la versión 8.x más reciente.
author: mathapli
ms.service: virtual-machines-linux
ms.topic: article
ms.date: 04/16/2020
ms.author: alsin
ms.reviewer: cynthn
ms.openlocfilehash: 48884e6faa5f26f027c772b44d5f960979a40d1d
ms.sourcegitcommit: 6109f1d9f0acd8e5d1c1775bc9aa7c61ca076c45
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2020
ms.locfileid: "94447509"
---
# <a name="red-hat-enterprise-linux-in-place-upgrades"></a>Actualizaciones locales de Red Hat Enterprise Linux

En este artículo se proporcionan instrucciones paso a paso sobre cómo realizar una actualización local de Red Hat Enterprise Linux 7 a Red Hat Enterprise Linux 8 mediante la utilidad Leapp en Azure. Durante la actualización local, el sistema operativo RHEL 7 existente se sustituye por la versión de RHEL 8.

>[!Note] 
> Las ofertas de SQL Server en Red Hat Enterprise Linux no admiten la actualización local en Azure.

## <a name="what-to-expect-during-the-upgrade"></a>Qué esperar durante la actualización
El sistema se reiniciará varias veces durante la actualización; esto es normal. El último reinicio actualizará la VM a la versión secundaria más reciente de RHEL 8.

## <a name="preparations-for-the-upgrade"></a>Preparación para la actualización
Las actualizaciones locales son la manera oficialmente recomendada de Red Hat y Azure para que los clientes puedan actualizar el sistema a la siguiente versión principal. Antes de realizar la actualización, hay algunas cosas que debe saber y tener en cuenta. 

>[!Important] 
> Haga una instantánea de la imagen antes de realizar la actualización.

>[!NOTE]
> Los comandos de este artículo deben ejecutarse con la cuenta raíz.

* Asegúrese de usar la versión más reciente de RHEL 7, que actualmente es RHEL 7.9. Si usa una versión bloqueada y no puede actualizar a RHEL 7.9, puede seguir [estos paso para cambiar a un repositorio que no sea EUS](https://docs.microsoft.com/azure/virtual-machines/workloads/redhat/redhat-rhui#switch-a-rhel-7x-vm-back-to-non-eus-remove-a-version-lock).

* Ejecute el comando siguiente para saber cómo va la actualización y si se completará. El comando debe generar un archivo en /var/log/leapp/leapp-Report.txt que explique el proceso y lo que se está haciendo, y si la actualización es posible o no.
    ```bash
    leapp preupgrade --no-rhsm
    ```

## <a name="steps-for-performing-the-upgrade"></a>Pasos para realizar la actualización

Siga estos pasos con cuidado. Se recomienda encarecidamente probar la actualización en una máquina de prueba antes de las instancias de producción.

1. Ejecute yum update para capturar los paquetes de cliente más recientes.
    ```bash
    yum update
    ```

1. Instale el paquete de leapp-client.
    ```bash
    yum install leapp-rhui-azure
    ```
    
1. Use el archivo leapp-data.tar.gz con repomap.csv y pes-events.json, presente en el [portal de RedHat](https://access.redhat.com/articles/3664871), y extráigalos. 
    1. Descargue el archivo.
    1. Extraiga el contenido y quite el archivo mediante el siguiente comando:
    ```bash
     tar -xzf leapp-data12.tar.gz -C /etc/leapp/files && rm leapp-data12.tar.gz
    ```
    


1. Agregue el archivo answers para Leapp.
    ```bash
    leapp answer --section remove_pam_pkcs11_module_check.confirm=True --add
    ```
    
1. Habilite PermitRootLogin en /etc/ssh/sshd_config.
    1. Abra el archivo /etc/ssh/sshd_config.
    1. Busque #PermitRootLogin yes.
    1. Quite # para quitar la marca de comentario.



1. Realice la actualización de Leapp.
    ```bash
    leapp upgrade --no-rhsm
    ```
1. Reinicie el servicio sshd para que los cambios surtan efecto.
    ```bash
    systemctl restart sshd
    ```
1. Comente PermitRootLogin en /etc/ssh/sshd_config de nuevo.
    1. Abra el archivo /etc/ssh/sshd_config.
    1. Busque #PermitRootLogin yes.
    1. Agregue # para agregar la marca de comentario.

## <a name="next-steps"></a>Pasos siguientes
* Obtenga más información sobre las [imágenes de Red Hat en Azure](./redhat-images.md).
* Obtenga más información sobre la [infraestructura de actualización de Red Hat](./redhat-rhui.md).
* Obtenga más información sobre la [oferta BYOS de RHEL](./byos.md).
* Puede encontrar información sobre los procesos de actualización local de Red Hat en la documentación de Red Hat sobre la [actualización de RHEL 7 a RHEL 8](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html-single/upgrading_from_rhel_7_to_rhel_8/index).
* Puede encontrar información sobre las directivas de soporte técnico de Red Hat para todas las versiones de RHEL en la página [Red Hat Enterprise Linux Life Cycle](https://access.redhat.com/support/policy/updates/errata) (Ciclo de vida de Red Hat Enterprise Linux).