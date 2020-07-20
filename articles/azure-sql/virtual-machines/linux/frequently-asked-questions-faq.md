---
title: Preguntas más frecuentes sobre SQL Server en máquinas virtuales Linux | Microsoft Docs
description: En este artículo se brindan respuestas a las preguntas más frecuentes sobre cómo ejecutar SQL Server en máquinas virtuales Linux.
services: virtual-machines-linux
documentationcenter: ''
author: MashaMSFT
tags: azure-service-management
ms.service: virtual-machines-sql
ms.topic: troubleshooting
ms.workload: iaas-sql-server
ms.date: 12/13/2017
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: afd36eb079b6e5f29fa664050cded3c5ddd17d8e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84669473"
---
# <a name="frequently-asked-questions-for-sql-server-on-linux-virtual-machines"></a>Preguntas más frecuentes sobre SQL Server en máquinas virtuales Linux
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

> [!div class="op_single_selector"]
> * [Windows](../windows/frequently-asked-questions-faq.md)
> * [Linux](frequently-asked-questions-faq.md)

En este artículo se proporcionan respuestas a algunas de las preguntas más comunes sobre la ejecución de [SQL Server en máquinas virtuales Linux](sql-server-on-linux-vm-what-is-iaas-overview.md).

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a name="images"></a><a id="images"></a> Imágenes

1. **¿Qué imágenes de la galería de máquinas virtuales de SQL Server están disponibles?**

   Azure mantiene imágenes de máquinas virtuales para todas las versiones principales compatibles con SQL Server en todas las ediciones de Linux y Windows. Para más información, consulte la lista completa de [imágenes de máquinas virtuales Linux](sql-server-on-linux-vm-what-is-iaas-overview.md#create) e [imágenes de máquinas virtuales Windows](../windows/sql-server-on-azure-vm-iaas-what-is-overview.md#payasyougo).

1. **¿Están actualizadas las imágenes existentes de la galería de máquinas virtuales de SQL Server?**

   Cada dos meses, las imágenes de SQL Server de la galería de máquinas virtuales se actualizan con las últimas actualizaciones de Linux y Windows. En las imágenes de Linux, esto incluye las últimas actualizaciones del sistema. En el caso de las imágenes de Windows, esto incluye cualquier actualización que se marque como importante en Windows Update, incluidas las actualizaciones de seguridad importantes de SQL Server y los Service Pack. Las actualizaciones acumulativas de SQL Server se tratan de forma diferente para Linux y Windows. En Linux, las actualizaciones acumulativas de SQL Server también se incluyen en la actualización. Pero en este momento, las máquinas virtuales de Windows no se actualizan con las actualizaciones acumulativas de SQL Server o Windows Server.

1. **¿Qué paquetes relacionados de SQL Server se instalan también?**

   Para ver los paquetes de SQL Server que se instalan de manera predeterminada en máquinas virtuales Linux con SQL Server, consulte [Paquetes instalados](sql-server-on-linux-vm-what-is-iaas-overview.md#packages).

1. **¿Se pueden quitar las imágenes de máquinas virtuales de SQL Server de la galería?**

   Sí. Azure solo mantiene una imagen por edición y versión principal. Por ejemplo, cuando se lanza un nuevo Service Pack de SQL Server, Azure agrega una nueva imagen a la galería para dicho Service Pack. La imagen SQL Server para el Service Pack anterior se quita inmediatamente de Azure Portal. Sin embargo, todavía está disponible desde PowerShell para su aprovisionamiento durante los tres meses siguientes. Transcurridos tres meses, la imagen del Service Pack anterior dejará de estar disponible. Esta directiva de eliminación también se aplicaría si una versión de SQL Server no recibe soporte técnico cuando llega al final de su ciclo de vida.

## <a name="creation"></a>Creación

1. **¿Cómo puedo crear una máquina virtual Linux con SQL Server?**

   La solución más simple es crear una máquina virtual Linux que incluya SQL Server. Para ver un tutorial sobre cómo suscribirse a Azure y crear una máquina virtual con SQL Server desde el portal, consulte [Aprovisionamiento de una máquina virtual Linux con SQL Server en Azure Portal](sql-vm-create-portal-quickstart.md). También tiene la opción de instalar manualmente SQL Server en una máquina virtual con una edición de licencia libre (Developer o Express) o reutilizando una licencia local. Si trae su propia licencia, debe disponer de [Movilidad de Licencias a través de Software Assurance en Azure](https://azure.microsoft.com/pricing/license-mobility).

1. **¿Por qué no puedo aprovisionar una máquina virtual RHEL o SLES con SQL Server con una suscripción de Azure con un límite de gasto?**

   Las máquinas virtuales RHEL y SLES requieren una suscripción sin límite de gasto y con un método de pago comprobado (con frecuencia, una tarjeta de crédito) asociado con la suscripción. Si aprovisiona una máquina virtual RHEL o SLES sin quitar el límite de gasto, se deshabilitará la suscripción y se detendrán todas las máquinas virtuales o servicios. Si de todos modos ejecuta en este estado, para volver a habilitar la suscripción, [quite el límite de gasto](https://account.windowsazure.com/subscriptions). El crédito restante se restaurará para el ciclo de facturación actual, pero se le cobrará un recargo por la imagen de máquina virtual RHEL o SLES en la tarjeta de crédito si elige volver a iniciar la operación y sigue ejecutándola.

## <a name="licensing"></a>Licencias

1. **¿Cómo puedo instalar mi copia de SQL Server con licencia en una máquina virtual de Azure?**

   En primer lugar, cree una máquina virtual solo para Linux OS. Luego ejecute los [pasos de instalación de SQL Server](https://docs.microsoft.com/sql/linux/sql-server-linux-setup#platforms) para la distribución de Linux. Salvo que instale una de las ediciones de SQL Server de licencia abierta, debe tener también una licencia de SQL Server y [Movilidad de Licencias a través de Software Assurance en Azure](https://azure.microsoft.com/pricing/license-mobility/).

1. **¿Hay imágenes de máquina virtual Linux de tipo "traiga su propia licencia" (BYOL) para SQL Server?**

   En este momento, no hay ninguna imagen de máquina virtual Linux BYOL para SQL Server. Sin embargo, puede instalar manualmente SQL Server en una máquina virtual solo Linux como se analizó en las preguntas anteriores.

1. **¿Puedo cambiar una máquina virtual para usar mi propia licencia de SQL Server si se ha creado desde una de las imágenes de la galería de pago por uso?**

   No. No puede cambiar de licencias de pago por segundo a usar su propia licencia. Debe crear una máquina virtual Linux nueva, instalar SQL Server y migrar los datos. Consulte la pregunta anterior para más detalles sobre cómo traer su propia licencia.

## <a name="administration"></a>Administración

1. **¿Puedo administrar una máquina virtual Linux con SQL Server mediante SQL Server Management Studio (SSMS)?**

   Sí, pero SSMS es actualmente una herramienta solo para Windows. Debe conectarse de forma remota desde una máquina Windows para usar SSMS con máquinas virtuales Linux que usan SQL Server. La nueva herramienta [mssql-conf](https://docs.microsoft.com/sql/linux/sql-server-linux-configure-mssql-conf) puede realizar muchas tareas administrativas de forma local en Linux. Para obtener una herramienta de administración de base de datos multiplataforma, consulte [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/what-is).

1. **¿Puedo quitar SQL Server por completo de una máquina virtual con SQL Server?**

   Sí, pero se le seguirá cobrando por la máquina virtual con SQL Server, tal y como se describe en [Orientación de precios de SQL Server para máquinas virtuales de Azure](../windows/pricing-guidance.md?toc=%2fazure%2fvirtual-machines%2flinux%2fsql%2ftoc.json). Si ya no necesita SQL Server, puede implementar una nueva máquina virtual y migrar los datos y aplicaciones a la nueva máquina virtual. Después, puede quitar la máquina virtual de SQL Server.

## <a name="updating-and-patching"></a>Actualizaciones y revisiones

1. **¿Cómo actualizo a una nueva versión o edición de SQL Server en una máquina virtual de Azure?**

   Actualmente, no hay ninguna actualización para SQL Server que se ejecute en una máquina virtual de Azure. Cree una máquina virtual de Azure nueva con la versión o edición de SQL Server que desee y, luego, migre las bases de datos al servidor nuevo con las [técnicas de migración de datos estándar](https://docs.microsoft.com/sql/linux/sql-server-linux-migrate-overview).

## <a name="general"></a>General

1. **¿Las soluciones de alta disponibilidad de SQL Server son compatibles con las máquinas virtuales de Azure?**

   De momento, no. Tanto los grupos de disponibilidad AlwaysOn como los clústeres de conmutación por error requieren una solución de clústeres en Linux, como Pacemaker. Las distribuciones de Linux compatibles con SQL Server no admiten sus complementos de alta disponibilidad en la nube.

## <a name="resources"></a>Recursos

**Máquinas virtuales Linux**:

* [Introducción a SQL Server en máquinas virtuales Linux](sql-server-on-linux-vm-what-is-iaas-overview.md)
* [Aprovisionamiento de SQL Server en máquinas virtuales Linux](sql-vm-create-portal-quickstart.md)
* [Documentación de SQL Server en Linux](https://docs.microsoft.com/sql/linux/sql-server-linux-overview)

**Máquinas virtuales Windows**:

* [Introducción a SQL Server en máquinas virtuales Windows](../windows/sql-server-on-azure-vm-iaas-what-is-overview.md)
* [Aprovisionamiento de SQL Server en máquinas virtuales Windows](../windows/sql-vm-create-portal-quickstart.md)
* [Preguntas más frecuentes (Windows)](../windows/frequently-asked-questions-faq.md)