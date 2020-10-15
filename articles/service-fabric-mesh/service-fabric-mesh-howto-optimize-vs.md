---
title: Optimización de Visual Studio para Azure Service Fabric Mesh
description: En este artículo se muestra cómo optimizar el rendimiento de Visual Studio para los proyectos de Service Fabric Mesh para que la primera ejecución de depuración (F5) sea mucho más rápida.
author: georgewallace
ms.author: gwallace
ms.date: 11/29/2018
ms.topic: conceptual
ms.openlocfilehash: aa7a959128d3bcdfcce67d3abeac245975339a9f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91840309"
---
# <a name="optimize-visual-studio-performance-for-service-fabric-mesh-projects"></a>Optimización del rendimiento de Visual Studio para proyectos Service Fabric Mesh

En este artículo se muestra cómo optimizar el rendimiento de Visual Studio para los proyectos de Service Fabric Mesh para que la primera ejecución de depuración (F5) sea mucho más rápida.  

## <a name="change-visual-studio-settings"></a>Cambio de la configuración de Visual Studio
 
En Visual Studio, en **Tools** > **Options**  > **Service Fabric Mesh Tools** > **General** (Herramientas > Opciones > Herramientas de Service Fabric Mesh), puede ajustar la siguiente configuración:

- **Extraer las imágenes de Docker necesarias al abrir el proyecto** hace que la primera ejecución de depuración (F5) sea más rápida al iniciar el proceso de descarga de imágenes mientras el proyecto se carga.  
- **Deploy application on project open** (Implementar aplicación al abrir el proyecto) puede hacer que la primera ejecución de depuración (F5) sea más rápida al iniciar el proceso de implementación una vez abierto el proyecto.  
- **Remove application on project close** (Quitar la aplicación al cerrar el proyecto) reclama los recursos (CPU, RAM) asignados a la aplicación mediante la eliminación de la aplicación de Mesh cuando se cierra el proyecto.  

Cuando ve mensajes en la ventana de salida de Service Fabric Tools que indican que Visual Studio "extrae imágenes", "prepara" o "quita la aplicación", se refiere a la configuración anterior. Puede desactivar esta configuración.

## <a name="next-steps"></a>Pasos siguientes

Lea el tutorial [para depurar una aplicación Mesh](service-fabric-mesh-tutorial-debug-service-fabric-mesh-app.md)