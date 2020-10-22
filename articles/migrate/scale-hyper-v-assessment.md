---
title: Evaluación de grandes cantidades de máquinas virtuales de Hyper-V para la migración a Azure con Azure Migrate | Microsoft Docs
description: Describe cómo evaluar grandes cantidades de máquinas virtuales de Hyper-V para su migración a Azure mediante el servicio Azure Migrate.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: raynew
ms.openlocfilehash: 393c58334f4a1bead7e089d0a64b2d165b67401d
ms.sourcegitcommit: ce8eecb3e966c08ae368fafb69eaeb00e76da57e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2020
ms.locfileid: "92317174"
---
# <a name="assess-large-numbers-of-hyper-v-vms-for-migration-to-azure"></a>Evaluación de grandes cantidades de máquinas virtuales de Hyper-V para la migración a Azure

En este artículo se describe cómo evaluar grandes cantidades de máquinas virtuales de Hyper-V locales para su migración a Azure mediante la herramienta de evaluación del servidor de Azure Migrate.

[Azure Migrate](migrate-services-overview.md) proporciona un centro de herramientas que le ayuda a detecta las aplicaciones, la infraestructura y las cargas de trabajo, a evaluarlas y a migrarlas a Microsoft Azure. Este centro incluye herramientas de Azure Migrate y ofertas de fabricantes de software independientes (ISV) de terceros. 


En este artículo aprenderá a:
> [!div class="checklist"]
> * Planificar la evaluación a escala.
> * Configurar los permisos de Azure y preparar Hyper-V para la evaluación.
> * Crear un proyecto de Azure Migrate y crear una evaluación.
> * Revisar la evaluación mientras planea la migración.


> [!NOTE]
> Si quiere probar una prueba de concepto para evaluar un par de VM antes de realizar la evaluación a escala, siga nuestra [serie de tutoriales](./tutorial-discover-hyper-v.md).

## <a name="plan-for-assessment"></a>Planeación de la evaluación

Al planear la evaluación de un gran número de máquinas virtuales de Hyper-V, hay un par de cosas que hay que tener en cuenta:

- **Planeación de proyectos de Azure Migrate**: Averigüe cómo implementar proyectos de Azure Migrate. Por ejemplo, si los centros de datos están en zonas geográficas diferentes, o si necesita almacenar metadatos relacionados con la detección, la evaluación o la migración en una zona geográfica diferente, es posible que necesite varios proyectos.
- **Planear dispositivos**: Azure Migrate usa un dispositivo de Azure Migrate local, implementado como una máquina virtual de Hyper-V, para detectar continuamente máquinas virtuales para la evaluación y migración. El dispositivo supervisa los cambios de entorno, como la incorporación de VM, discos o adaptadores de red. También envía metadatos y datos de rendimiento acerca de ellos a Azure. Debe averiguar el número de dispositivos que se van a implementar.


## <a name="planning-limits"></a>Límites de planeación
 
Use los límites resumidos en esta tabla para la planeación.

**Planeamiento** | **Límites**
--- | --- 
**Proyectos de Azure Migrate** | Evalúe hasta 35 000 VM en un proyecto.
**Dispositivo con Azure Migrate** | Un dispositivo puede detectar hasta 5000 máquinas virtuales.<br/> Un dispositivo puede conectarse hasta a 300 hosts de Hyper-V.<br/> Un dispositivo solo se puede asociar con un único proyecto de Azure Migrate.<br/> Se puede asociar cualquier número de dispositivos a un solo proyecto de Azure Migrate. <br/><br/> 
**Grupo** | Puede agregar hasta 35 000 máquinas virtuales en un solo grupo.
**Evaluación de Azure Migrate** | Puede acceder hasta a 35 000 máquinas virtuales en una única evaluación.



## <a name="other-planning-considerations"></a>Otras consideraciones de planeación

- Para iniciar la detección desde el dispositivo, tiene que seleccionar cada host de Hyper-V. 
- Si está ejecutando un entorno de varios inquilinos, actualmente no puede detectar solo las máquinas virtuales que pertenecen a un inquilino específico. 

## <a name="prepare-for-assessment"></a>Preparación para la evaluación

Prepare Azure e Hyper-V para la evaluación del servidor. 

1. Consulte [los requisitos y las limitaciones de compatibilidad de Hyper-V](migrate-support-matrix-hyper-v.md).
2. Configure los permisos de la cuenta de Azure para interactuar con Azure Migrate.
3. Prepare las máquinas virtuales y los hosts de Hyper-V.

Siga las instrucciones de [este tutorial](./tutorial-discover-hyper-v.md) para definir estas configuraciones.

## <a name="create-a-project"></a>Crear un proyecto

Según los requisitos de planeación, haga lo siguiente:

1. Cree un proyecto de Azure Migrate.
2. Agregue la herramienta de evaluación del servidor de Azure Migrate a los proyectos.

[Más información](how-to-add-tool-first-time.md)

## <a name="create-and-review-an-assessment"></a>Creación y revisión de una evaluación

1. Cree evaluaciones para máquinas virtuales de Hyper-V.
1. Revise las evaluaciones en preparación para la planeación de la migración.

[Más información](tutorial-assess-hyper-v.md) sobre la creación y revisión de evaluaciones.
    

## <a name="next-steps"></a>Pasos siguientes

En este artículo:
 
> [!div class="checklist"] 
> * Planeó escalar evaluaciones de Azure Migrate para máquinas virtuales de Hyper-V
> * Preparó Azure e Hyper-V para la evaluación
> * Creó un proyecto de Azure Migrate y ejecutó las evaluaciones
> * Revisó las evaluaciones como preparación para la migración.

Ahora, [obtenga información](concepts-assessment-calculation.md) sobre cómo se calculan las evaluaciones y cómo [modificar estas](how-to-modify-assessment.md)