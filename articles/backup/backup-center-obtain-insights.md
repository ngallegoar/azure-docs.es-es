---
title: Obtención de conclusiones mediante el Centro de copias de seguridad
description: Obtenga información sobre cómo analizar las tendencias históricas y sacar conclusiones más profundas sobre las copias de seguridad con el Centro de copias de seguridad.
ms.topic: conceptual
ms.date: 09/01/2020
ms.openlocfilehash: 5964f285089feea721a0b452efed884e905b89cc
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2020
ms.locfileid: "90993557"
---
# <a name="obtain-insights-using-backup-center"></a>Obtención de conclusiones mediante el Centro de copias de seguridad

Para analizar las tendencias históricas y obtener información más detallada sobre las copias de seguridad, el Centro de copias de seguridad proporciona una interfaz para [Informes de Backup](configure-reports.md), que usa [Registros de Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-logs) y [Libros de Azure](https://docs.microsoft.com/azure/azure-monitor/platform/workbooks-overview). Informes de Backup ofrece las siguientes funcionalidades:

- Asignación y previsión del almacenamiento en la nube consumido.

- Auditoría de copias de seguridad y restauraciones.

- Identificación de las tendencias clave con diferentes niveles de granularidad.

- Obtención de visibilidad y conclusiones sobre las oportunidades de optimización de costos para las copias de seguridad.

## <a name="supported-scenarios"></a>Escenarios admitidos

- Informes de Backup no está disponible actualmente para copias de seguridad del Servidor de Azure Database for PostgreSQL.

- Consulte la [matriz de compatibilidad](backup-center-support-matrix.md) para obtener una lista detallada de escenarios admitidos y no admitidos.

## <a name="get-started"></a>Introducción

### <a name="configure-your-vaults-to-send-data-to-a-log-analytics-workspace"></a>Configuración de almacenes para enviar datos a un área de trabajo de Log Analytics

[Obtenga información sobre cómo configurar los valores de diagnóstico a gran escala para los almacenes](https://docs.microsoft.com/azure/backup/configure-reports#get-started)

### <a name="view-backup-reports-in-the-backup-center-portal"></a>Visualización de Informes de Backup en el portal del Centro de copias de seguridad

Al seleccionar el elemento de menú **Informes de Backup** en el Centro de copias de seguridad, se abren los informes. Elija una o varias áreas de trabajo de Log Analytics para ver y analizar la información clave de las copias de seguridad.

![Informes de Backup en el Centro de copias de seguridad](./media/backup-center-obtain-insights/backup-center-backup-reports.png)

A continuación se muestran las vistas disponibles:

1. **Resumen**: use esta pestaña para obtener información general sobre el conjunto de copias de seguridad. [Más información](https://docs.microsoft.com/azure/backup/configure-reports#summary)

1. **Elementos de copia de seguridad**: use esta pestaña para ver información y tendencias sobre el almacenamiento en la nube consumido en el nivel de elemento de Backup. [Más información](https://docs.microsoft.com/azure/backup/configure-reports#backup-items)

1. **Uso**: use esta pestaña para ver los parámetros de facturación principales de las copias de seguridad. [Más información](https://docs.microsoft.com/azure/backup/configure-reports#usage)

1. **Trabajos**: use esta pestaña para ver tendencias de ejecución prolongada de los trabajos, como el número de trabajos con errores por día y las causas principales de los errores de los trabajos. [Más información](https://docs.microsoft.com/azure/backup/configure-reports#jobs)

1. **Directivas**: use esta pestaña para ver información sobre todas las directivas activas, como el número de elementos asociados y el almacenamiento en la nube total consumido por los elementos de los que se ha realizado una copia de seguridad por una directiva determinada. [Más información](https://docs.microsoft.com/azure/backup/configure-reports#policies)

1. **Optimizar**: use esta pestaña para obtener visibilidad sobre las posibles oportunidades de optimización de costos para las copias de seguridad. [Más información](https://docs.microsoft.com/azure/backup/configure-reports#optimize)

## <a name="next-steps"></a>Pasos siguientes

- [Supervisión y funcionamiento de las copias de seguridad](backup-center-monitor-operate.md)
- [Control del conjunto de copias de seguridad](backup-center-govern-environment.md)
- [Acciones con el Centro de copias de seguridad](backup-center-actions.md)
