---
title: Introducción a Microsoft Azure Data Box Disk | Microsoft Docs en datos
description: Describe Azure Data Box Disk, una solución en la nube que permite transferir grandes cantidades de datos en Azure
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: overview
ms.date: 06/18/2019
ms.author: alkohli
Customer intent: As an IT admin, I need to understand what Data Box Disk is and how it works so I can use it to import on-premises data into Azure.
ms.openlocfilehash: 067d818b7d23fc0b83cb1d4255bfbb8659149412
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "79215733"
---
# <a name="what-is-azure-data-box-disk"></a>¿Qué es Azure Data Box Disk?

La solución Microsoft Azure Data Box Disk le permite enviar terabytes de datos locales a Azure de forma rápida, económica y confiable. La transferencia de datos segura se acelera enviándole de 1 a 5 discos de estado sólido (SSD). Estos discos de 8 TB cifrados se envían al centro de datos a través de un operador regional. 

Puede configurar, conectar y desbloquear rápidamente los discos a través del servicio Data Box en Azure Portal. Copie los datos en discos y vuelva a enviar los discos a Azure. En el centro de datos de Azure, los datos se cargan automáticamente de las unidades a la nube mediante un vínculo de carga de red rápido y privado.

## <a name="use-cases"></a>Casos de uso

Utilice Data Box Disk para transferir TB de datos en escenarios sin conectividad de red limitada. El movimiento de datos puede ser único, periódico o una transferencia de datos masiva inicial seguida de transferencias periódicas. 

- **Migración única**: cuando se mueve gran cantidad de datos locales a Azure. Por ejemplo, mover datos de cintas sin conexión a datos de archivo de un almacenamiento esporádico de Azure.
- **Transferencia incremental**: cuando se realiza una transferencia masiva inicial con Data Box Disk (inicialización) seguido de transferencias incrementales a través de la red. Por ejemplo, Commvault y Data Box Disk se usan para mover las copias de seguridad a Azure. A esta migración la sigue la copia de datos incrementales a través de la red al almacenamiento de Azure. 
- **Cargas periódicas**: cuando se genera periódicamente una gran cantidad de datos y es necesario moverlos a Azure. Por ejemplo, en la exploración de energía, donde el contenido de vídeo se genera en plataformas petrolíferas y parques eólicos.

## <a name="the-workflow"></a>El flujo de trabajo

Un flujo típico incluye los siguientes pasos:

1. **Solicitar**: cree un pedido en Azure Portal, proporcione la información de envío y la cuenta de almacenamiento de Azure de destino para los datos. Si hay discos disponibles, Azure cifra, prepara y envía los discos con un identificador de seguimiento del envío.

2. **Recibir**: una vez que se entregan los discos, desempaquete y conecte los discos al equipo que tiene los datos que se van a copiar. Desbloquee los discos.
    
3. **Copiar datos**: arrastre y colóquelos para copiar los datos en los discos.

4. **Devolver**: prepare y envíe los discos de nuevo al centro de datos de Azure.

5. **Cargar**: los datos se copian automáticamente de los discos a Azure. Los discos se borran de forma segura según las directrices del Instituto Nacional de Normas y Tecnología (NIST).

Durante este proceso, se le notificará por correo electrónico de todos los cambios de estado. Para más información sobre el flujo detallado, consulte [cómo implementar instancias de Data Box Disk en Azure Portal](data-box-disk-quickstart-portal.md).


## <a name="benefits"></a>Ventajas

Data Box Disk está pensado para mover grandes cantidades de datos a Azure sin que afecte a la red. La solución tiene las siguientes ventajas:

- **Velocidad**: Data Box Disk usa una conexión USB 3.0 para mover hasta 35 TB de datos en Azure en menos de una semana.   

- **Fácil de usar**: Data Box es una solución fácil de usar.

    - Los discos utilizan conexión USB prácticamente sin tiempo de instalación.
    - Los discos tienen un factor de forma pequeño que hace que sean fáciles de manipular.
    - Los discos no tienen ningún requisito de alimentación externa.
    - Los discos pueden usarse con un servidor de centro de datos, equipo de escritorio o portátil.
    - La solución proporciona un seguimiento total a través de Azure Portal.    

- **Proteger**: Data Box Disk tiene protecciones de seguridad integradas para los discos, los datos y el servicio. 
    - Los discos son resistente a alteraciones y admiten la funcionalidad de actualización segura. 
    - Los datos en los discos se protegen con un cifrado AES de 128 bits en todo momento. 
    - Los discos solo se pueden desbloquear con una clave proporcionada en Azure Portal. 
    - El servicio está protegido por las características de seguridad de Azure. 
    - Cuando los datos se cargan en Azure, los discos se limpian, según las normas NIST 800-88r1.  
    
Para más información, vaya a [Azure Data Box Disk security and data protection](data-box-disk-security.md) (Protección de datos y seguridad de Azure Data Box Disk).


## <a name="features-and-specifications"></a>Características y especificaciones


| Especificaciones                                          | Descripción              |
|---------------------------------------------------------|--------------------------|
| Peso                                                  | < 2 libras por caja. Hasta 5 discos en la caja                |
| Dimensions                                              | Disco: SSD de 2,5" |            
| Cables                                                  | 1 cable USB 3.1 por disco|
| Capacidad de almacenamiento por pedido                              | 40 TB (utilizable ~ 35 TB)|
| Capacidad de almacenamiento de disco                                   | 8 TB (utilizable ~ 7 TB)|
| Interfaz de datos                                          | USB   |
| Seguridad                                                | Previamente cifrada mediante BitLocker y la actualización segura <br> Discos protegidos por clave de paso <br> Datos cifrados en todo momento  |
| Velocidad de transferencia de datos                                      | Hasta 430 MBps en función del tamaño de archivo      |
|Administración                                               | Portal de Azure |


## <a name="region-availability"></a>Disponibilidad en regiones

Para más información sobre la disponibilidad regional, consulte [Productos disponibles por región](https://azure.microsoft.com/global-infrastructure/services/?products=databox&regions=all). Data Box Disk también se puede implementar en la nube de Azure Government. Para más información, consulte [¿Qué es Azure Government?](https://docs.microsoft.com/azure/azure-government/documentation-government-welcome).


## <a name="pricing"></a>Precios

Para más información sobre los precios, vaya a la [página precios](https://azure.microsoft.com/pricing/details/databox/disk/).

## <a name="next-steps"></a>Pasos siguientes

- Revise los [requisitos de Data Box Disk](data-box-disk-system-requirements.md).
- Información acerca de los [límites de Data Box Disk](data-box-disk-limits.md).
- Implemente rápidamente [Azure Data Box Disk](data-box-disk-quickstart-portal.md) en Azure Portal.
