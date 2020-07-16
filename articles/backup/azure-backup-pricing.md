---
title: Precios de Azure Backup
description: Aprenda a estimar los costos para establecer presupuestos de los precios de Azure Backup.
ms.topic: conceptual
ms.date: 06/16/2020
ms.openlocfilehash: 274a61ff5a98fa1291f9d8917af9ab1d1b3da2fd
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85391118"
---
# <a name="azure-backup-pricing"></a>Precios de Azure Backup

Para información sobre los precios de Azure Backup, visite la [página de precios de Azure Backup](https://azure.microsoft.com/pricing/details/backup/).

## <a name="download-detailed-estimates-for-azure-backup-pricing"></a>Descarga de estimaciones detalladas de los precios de Azure Backup

Si busca estimar los costos para establecer presupuestos o para comparar costos, descargue el [estimador de precios de Azure Backup](https://aka.ms/AzureBackupCostEstimates) detallado.  

### <a name="what-does-the-estimator-contain"></a>¿Qué contiene el estimador?

La hoja del estimador de costos de Azure Backup tiene una opción para estimar todas las cargas de trabajo posibles de las que quiere hacer una copia de seguridad con Azure Backup. Algunas de las cargas de trabajo a las que va enfocada son:

- Máquinas virtuales de Azure
- Servidores locales
- SQL en VM de Azure
- SAP HANA en VM de Azure
- Recursos compartidos de archivos de Azure

## <a name="estimate-costs-for-backing-up-azure-vms-or-on-premises-servers"></a>Estimación de costos de copias de seguridad de VM de Azure o servidores locales

Para estimar los costos de hacer una copia de seguridad de VM de Azure o servidores locales con Azure Backup, necesitará los parámetros siguientes:

- Tamaño de las máquinas virtuales o servidores locales de los que intenta hacer una copia de seguridad
  - Escriba el "tamaño usado" de los discos o servidores de los que se debe hacer una copia de seguridad

- Número de servidores con ese tamaño

- ¿Cuál es la cantidad esperada de renovación de datos en estos servidores?<br>
  La renovación se refiere a la cantidad de cambios en los datos. Por ejemplo, si tiene una máquina virtual con 200 GB de datos de la que se va a hacer una copia de seguridad y si cada día cambian 10 GB de esos datos, la renovación diaria es del 5 %.

  - Una renovación más alta implicará que se hará una copia de seguridad de más datos

  - Elija **Baja** o **Moderada** para los servidores de archivos y **Alta** si ejecuta bases de datos

  - Si conoce el porcentaje de renovación **churn%** , puede usar la opción **Enter your own%** (Escriba su propio porcentaje)

- Elija la directiva de copia de seguridad

  - ¿Cuánto tiempo espera conservar las copias de seguridad "diarias"? (en días)

  - ¿Cuánto tiempo espera conservar las copias de seguridad "semanales"? (en semanas)

  - ¿Cuánto tiempo espera conservar las copias de seguridad "mensuales"? (en meses)

  - ¿Cuánto tiempo espera conservar las copias de seguridad "anuales"? (en años)

  - ¿Cuánto tiempo espera conservar las "instantáneas de restauración inmediata"? (de 1 a 5 días)

    - Esta opción permite restaurar desde hasta siete días de forma rápida con las instantáneas almacenadas en discos

- **Opcional**: Copia de seguridad de disco selectiva

  - Si usa la opción **Copia de seguridad de disco selectiva** al hacer una copia de seguridad de VM de Azure, elija la opción **Exclude Disk** (Excluir disco) y escriba el porcentaje de discos que se excluirá de la copia de seguridad en términos de tamaño. Por ejemplo, si tiene una máquina virtual conectada a tres discos con 200 GB usados en cada disco y quiere excluir dos de ellos de la copia de seguridad, escriba 66,7 %.

- **Opcional**: Redundancia de almacenamiento de copia de seguridad

  - Esto indica la redundancia de la cuenta de almacenamiento a la que van a ir los datos de copia de seguridad. Se recomienda usar **GRS** para obtener la máxima disponibilidad. Dado que garantiza que una copia de los datos de copia de seguridad se mantiene en otra región, ayuda a cumplir con varios estándares de cumplimiento. Cambie la redundancia a **LRS** si está haciendo una copia de seguridad de entornos de desarrollo o prueba que no necesitan una copia de seguridad de nivel empresarial. Seleccione la opción **RAGRS** en la hoja si quiere conocer los costos cuando la opción [Restauración entre regiones](backup-azure-arm-restore-vms.md#cross-region-restore) está habilitada para las copias de seguridad.

- **Opcional**: Modificación de precios regionales o aplicación de tarifas con descuento

  - Si quiere comprobar las estimaciones de una región distinta o tarifas con descuento, seleccione **Sí** en la opción **Try estimates for a different region?** (¿Quiere intentar estimaciones para otra región?) y escriba las tarifas con las que quiere ejecutar las estimaciones.

## <a name="estimate-costs-for-backing-up-sql-servers-in-azure-vms"></a>Estimación de los costos de copia de seguridad de servidores SQL Server en VM de Azure

Para estimar los costos de hacer una copia de seguridad de servidores SQL Server que se ejecutan en VM de Azure con Azure Backup, necesitará los parámetros siguientes:

- Tamaño de los servidores SQL Server de los que intenta hacer una copia de seguridad

- Número de servidores SQL Server con el tamaño anterior

- ¿Cuál es la compresión esperada para los datos de copia de seguridad de los servidores SQL Server?

  - La mayoría de los clientes de Azure Backup ve que los datos de copia de seguridad tienen un 80 % de compresión en comparación con el tamaño del servidor SQL Server cuando la compresión de SQL está **habilitada**.

  - Si espera ver una compresión diferente, escriba el número en este campo

- ¿Cuál es el tamaño esperado de las copias de seguridad de registros?

  - El % indica el tamaño de registro diario como % del tamaño del servidor SQL Server

- ¿Cuál es la cantidad esperada de renovación de datos diaria en estos servidores?

  - Por lo general, las bases de datos tienen una renovación "alta"

  - Si conoce el porcentaje de renovación **churn%** , puede usar la opción **Enter your own%** (Escriba su propio porcentaje)

- Elija la directiva de copia de seguridad

  - Tipo de copia de seguridad

    - La directiva más efectiva que puede elegir es **Diferenciales diarios** con copias de seguridad completas semanales, mensuales o anuales. Azure Backup puede realizar restauraciones a partir de diferenciales también con un solo clic.

    - También puede elegir tener una directiva con copias de seguridad completas diarias, semanales, mensuales o anuales. Esta opción usará un almacenamiento algo mayor que la primera opción.

  - ¿Cuánto tiempo espera conservar las copias de seguridad de "registros"? (en días) [de 7 a 35]

  - ¿Cuánto tiempo espera conservar las copias de seguridad "diarias"? (en días)

  - ¿Cuánto tiempo espera conservar las copias de seguridad "semanales"? (en semanas)

  - ¿Cuánto tiempo espera conservar las copias de seguridad "mensuales"? (en meses)

  - ¿Cuánto tiempo espera conservar las copias de seguridad "anuales"? (en años)

- **Opcional**: Redundancia de almacenamiento de copia de seguridad

  - Esto indica la redundancia de la cuenta de almacenamiento a la que van a ir los datos de copia de seguridad. Se recomienda usar **GRS** para obtener la máxima disponibilidad. Dado que garantiza que una copia de los datos de copia de seguridad se mantiene en otra región, ayuda a cumplir con varios estándares de cumplimiento. Cambie la redundancia a **LRS** si está haciendo una copia de seguridad de entornos de desarrollo o prueba que no necesitan una copia de seguridad de nivel empresarial.

- **Opcional**: Modificación de precios regionales o aplicación de tarifas con descuento

  - Si quiere comprobar las estimaciones de una región distinta o tarifas con descuento, seleccione **Sí** en la opción **Try estimates for a different region?** (¿Quiere intentar estimaciones para otra región?) y escriba las tarifas con las que quiere ejecutar las estimaciones.

## <a name="estimate-costs-for-backing-up-sap-hana-servers-in-azure-vms"></a>Estimación de los costos de copia de seguridad de servidores SAP HANA en VM de Azure

Para estimar los costos de hacer una copia de seguridad de servidores SAP HANA que se ejecutan en VM de Azure con Azure Backup, necesitará los parámetros siguientes:

- Tamaño total de las bases de datos de SAP HANA de las que intenta hacer una copia de seguridad. Debe ser la suma del tamaño de la copia de seguridad completa de cada una de las bases de datos, tal como se muestra en SAP HANA.
- Número de servidores SAP HANA con el tamaño anterior
- ¿Cuál es el tamaño esperado de las copias de seguridad de registros?
  - El % indica el tamaño promedio de registros diarios como % del tamaño total de bases de datos de SAP HANA de las que se hace una copia de seguridad en el servidor SAP HANA
- ¿Cuál es la cantidad esperada de renovación de datos diaria en estos servidores?
  - El % indica el tamaño promedio de la renovación diaria como % del tamaño total de bases de datos de SAP HANA de las que se hace una copia de seguridad en el servidor SAP HANA
  - Por lo general, las bases de datos tienen una renovación "alta"
  - Si conoce el porcentaje de renovación **churn%** , puede usar la opción **Enter your own%** (Escriba su propio porcentaje)
- Elija la directiva de copia de seguridad
  - Tipo de copia de seguridad
    - La directiva más efectiva que puede elegir es **Diferenciales diarios** con copias de seguridad completas **semanales, mensuales o anuales**. Azure Backup puede realizar restauraciones a partir de diferenciales también con un solo clic.
    - También puede elegir tener una directiva con copias de seguridad completas **diarias, semanales, mensuales o anuales**. Esta opción usará un almacenamiento algo mayor que la primera opción.
  - ¿Cuánto tiempo espera conservar las copias de seguridad de "registros"? (en días) [de 7 a 35]
  - ¿Cuánto tiempo espera conservar las copias de seguridad "diarias"? (en días)
  - ¿Cuánto tiempo espera conservar las copias de seguridad "semanales"? (en semanas)
  - ¿Cuánto tiempo espera conservar las copias de seguridad "mensuales"? (en meses)
  - ¿Cuánto tiempo espera conservar las copias de seguridad "anuales"? (en años)
- **Opcional**: Redundancia de almacenamiento de copia de seguridad
  - Esto indica la redundancia de la cuenta de almacenamiento a la que van a ir los datos de copia de seguridad. Se recomienda usar **GRS** para obtener la máxima disponibilidad. Dado que garantiza que una copia de los datos de copia de seguridad se mantiene en otra región, ayuda a cumplir con varios estándares de cumplimiento. Cambie la redundancia a **LRS** si está haciendo una copia de seguridad de entornos de desarrollo o prueba que no necesitan una copia de seguridad de nivel empresarial.
- **Opcional**: Modificación de precios regionales o aplicación de tarifas con descuento
  - Si quiere comprobar las estimaciones de una región distinta o tarifas con descuento, seleccione **Sí** en la opción **Try estimates for a different region?** (¿Quiere intentar estimaciones para otra región?) y escriba las tarifas con las que quiere ejecutar las estimaciones.

## <a name="next-steps"></a>Pasos siguientes

[¿Qué es el servicio Azure Backup?](backup-overview.md)
