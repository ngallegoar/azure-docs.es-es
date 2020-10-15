---
title: Cargas de trabajo de IBM en Azure | Microsoft Docs
description: Use un emulador del sistema central y otros servicios de asociados de Microsoft para volver a hospedar las cargas de trabajo de IBM z/OS con Microsoft Azure.
services: virtual-machines-linux
ms.service: virtual-machines-linux
documentationcenter: ''
author: njray
ms.author: edprice
manager: edprice
editor: edprice
ms.topic: conceptual
ms.date: 02/22/2019
tags: ''
keywords: ''
ms.openlocfilehash: e99685b4d1476ee527b9eb7f1332f6a6beece669
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86499979"
---
# <a name="ibm-workloads-on-azure"></a>Cargas de trabajo de IBM en Azure

Muchas cargas de trabajo del sistema central de IBM basadas en z/OS se pueden replicar en Azure sin pérdida de funcionalidad y sin que los usuarios perciban los cambios en sus sistemas subyacentes. Volver a hospedar aplicaciones en Azure le proporciona las características como las del sistema central que necesita, además de la elasticidad, la disponibilidad y el posible ahorro de costos de la nube.

Azure admite la integración con los entornos de sistema central de IBM existentes, lo que le permite migrar las aplicaciones que tienen sentido, ejecutar soluciones híbridas cuando sea necesario y realizar la migración con el tiempo. Aunque puede reescribir completamente los programas existentes basados en el sistema central para Azure, es más habitual volver a hospedarlos. La reescritura aumenta los costos, la complejidad y el tiempo de la migración proyectos. Al volver a realizar el hospedaje, puede hacer lo siguiente:

- Mover aplicaciones a un emulador basado en la nube.

- Migrar la base de datos a una base de datos basada en la nube.

- Reemplazar los módulos y el código mediante motores de transformación de código.

Además, el software de IBM, incluidos WebSphere y MQ, ahora está disponible en Azure Marketplace. Con una licencia de software de IBM, puede aprovechar las ventajas que aporta el escalado de la infraestructura a petición que ofrece Azure para poner en marcha una máquina virtual con rapidez.

Un amplio ecosistema de asociados está disponible para ayudarle a migrar sistemas centrales de IBM a Azure. La mayoría sigue un enfoque pragmático de reutilización siempre que sea posible, antes de embarcarse en una implementación por fases de reescritura o reemplazo de aplicaciones. Obtenga más instrucciones y ayuda de asociados en [Migración del sistema central en Azure Migration Center](https://azure.microsoft.com/migration/mainframe/).

**Pasos siguientes**

- [Mitos y verdades del sistema central](/azure/architecture/cloud-adoption/infrastructure/mainframe-migration/myths-and-facts)
- [Instalación del entorno de desarrollo y pruebas de IBM zD&T en Azure](./install-ibm-z-environment.md)
- [Set up an Application Developers Controlled Distribution (ADCD) in IBM zD&T v1](./demo.md) (Configuración de una instancia de Application Developers Controlled Distribution (ADCD) en IBM zD&T v1)
- [IBM DB2 pureScale en Azure](../../../linux/ibm-db2-purescale-azure.md)
