---
title: Actualización local de imágenes de Red Hat Enterprise Linux en Azure
description: Encuentre los pasos para realizar la actualización local desde imágenes de Red Hat Enterprise 7.x a la versión 8.x más reciente.
author: mathapli
ms.service: virtual-machines-linux
ms.topic: article
ms.date: 04/16/2020
ms.author: alsin
ms.reviewer: cynthn
ms.openlocfilehash: beede74134affeb3ee0d4bdd20d5da3b4c5e6eda
ms.sourcegitcommit: 04fb3a2b272d4bbc43de5b4dbceda9d4c9701310
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2020
ms.locfileid: "94566629"
---
# <a name="red-hat-enterprise-linux-in-place-upgrades"></a>Actualizaciones locales de Red Hat Enterprise Linux

En este artículo se proporcionan instrucciones paso a paso sobre cómo realizar una actualización local de Red Hat Enterprise Linux 7 a Red Hat Enterprise Linux 8 mediante la utilidad Leapp en Azure. Durante la actualización local, el sistema operativo RHEL 7 existente se sustituye por la versión de RHEL 8.

>[!Note] 
> Las ofertas de SQL Server en Red Hat Enterprise Linux no admiten la actualización local en Azure.

## <a name="what-to-expect-during-the-upgrade"></a>Qué esperar durante la actualización
El sistema se reiniciará varias veces durante la actualización; esto es normal. El último reinicio actualizará la VM a la versión secundaria más reciente de RHEL 8. 

El proceso de actualización puede tardar desde 20 minutos hasta un par de horas, dependiendo de factores como el tamaño de la máquina virtual y el número de paquetes instalados en el sistema.

## <a name="preparations-for-the-upgrade"></a>Preparación para la actualización
La actualización local es la manera oficialmente recomendada de Red Hat y Azure para que los clientes puedan actualizar el sistema a la siguiente versión principal. Antes de realizar la actualización, hay algunas cosas que debe saber y tener en cuenta. 

>[!Important] 
> Haga una instantánea de la imagen antes de realizar la actualización.

>[!NOTE]
> Los comandos de este artículo deben ejecutarse con la cuenta raíz.

* Asegúrese de usar la versión más reciente de RHEL 7, que actualmente es RHEL 7.9. Si usa una versión bloqueada y no puede actualizar a RHEL 7.9, puede seguir [estos paso para cambiar a un repositorio que no sea EUS](https://docs.microsoft.com/azure/virtual-machines/workloads/redhat/redhat-rhui#switch-a-rhel-7x-vm-back-to-non-eus-remove-a-version-lock).

* Ejecute el comando siguiente para saber cómo va la actualización y si se completará. El comando debe generar un archivo en /var/log/leapp/leapp-Report.txt que explique el proceso y lo que se está haciendo, y si la actualización es posible o no.
    ```bash
    leapp preupgrade --no-rhsm
    ```
* Asegúrese de que la consola serie funcione, ya que permite la supervisión durante el proceso de actualización.

* Habilitación del acceso raíz SSH en `/etc/ssh/sshd_config`
    1. Abra el archivo `/etc/ssh/sshd_config`.
    1. Busque #PermitRootLogin yes.
    1. Quite # para quitar la marca de comentario.

## <a name="steps-for-performing-the-upgrade"></a>Pasos para realizar la actualización

Siga estos pasos con cuidado. Se recomienda encarecidamente probar la actualización en una máquina de prueba antes de las instancias de producción.

1. Ejecute yum update para capturar los paquetes de cliente más recientes.
    ```bash
    yum update -y
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

1. Realice la actualización de Leapp.
    ```bash
    leapp upgrade --no-rhsm
    ```
1.  Cuando el comando `leapp upgrade` finalice sin errores, reinicie manualmente el sistema para completar el proceso. El sistema se reiniciará un par de veces y durante ese tiempo no estará disponible. Supervise el proceso mediante la consola serie.

1.  Compruebe que la actualización se completó correctamente.
    ```bash
    uname -a && cat /etc/redhat-release
    ```

1. Quite el acceso SSH raíz una vez finalizada la actualización.
    1. Abra el archivo `/etc/ssh/sshd_config`.
    1. Busque #PermitRootLogin yes.
    1. Agregue # para agregar la marca de comentario.

1. Reinicie el servicio sshd para que los cambios surtan efecto.
    ```bash
    systemctl restart sshd
    ```

## <a name="common-issues"></a>Problemas comunes
Estos son algunos de los casos comunes en los que se puede producir un error en el proceso de `leapp preupgrade` o `leapp upgrade`.

**Error: No se encontraron coincidencias para los siguientes patrones de complemento deshabilitados**
```plaintext
STDERR:
No matches found for the following disabled plugin patterns: subscription-manager
Warning: Packages marked by Leapp for upgrade not found in repositories metadata: gpg-pubkey
```
**Solución**\
Deshabilite el complemento de administrador de suscripciones editando el archivo `/etc/yum/pluginconf.d/subscription-manager.conf` y cambiando la opción de habilitado a `enabled=0`.

Esto se debe a que el complemento yum del administrador de suscripciones está habilitado, y no se usa para las máquinas virtuales de Pago por uso.

**Error: Posibles problemas con el inicio de sesión remoto con**  raíz `leapp preupgrade` podría producir el siguiente error:
```structured-text
============================================================
                     UPGRADE INHIBITED
============================================================

Upgrade has been inhibited due to the following problems:
    1. Inhibitor: Possible problems with remote login using root account
Consult the pre-upgrade report for details and possible remediation.

============================================================
                     UPGRADE INHIBITED
============================================================
```
**Solución**\
Habilite el acceso raíz en `/etc/sshd_conf`.
Esto se debe a que no se habilita el acceso SSH raíz en `/etc/sshd_conf` según la sección "[Preparación para la actualización](#preparations-for-the-upgrade)". 

## <a name="next-steps"></a>Pasos siguientes
* Obtenga más información sobre las [imágenes de Red Hat en Azure](./redhat-images.md).
* Obtenga más información sobre la [infraestructura de actualización de Red Hat](./redhat-rhui.md).
* Obtenga más información sobre la [oferta BYOS de RHEL](./byos.md).
* Puede encontrar información sobre los procesos de actualización local de Red Hat en la documentación de Red Hat sobre la [actualización de RHEL 7 a RHEL 8](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html-single/upgrading_from_rhel_7_to_rhel_8/index).
* Puede encontrar información sobre las directivas de soporte técnico de Red Hat para todas las versiones de RHEL en la página [Red Hat Enterprise Linux Life Cycle](https://access.redhat.com/support/policy/updates/errata) (Ciclo de vida de Red Hat Enterprise Linux).