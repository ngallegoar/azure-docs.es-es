---
title: Ejemplos de PowerShell
description: Busque ejemplos de Azure PowerShell para algunos de los escenarios de App Service habituales. Aprenda a automatizar las tareas de administración o implementación de App Service.
tags: azure-service-management
ms.assetid: b48d1137-8c04-46e0-b430-101e07d7e470
ms.topic: sample
ms.date: 03/08/2017
ms.custom: mvc
ms.openlocfilehash: f32db2a4dda0ceb69644b64a5c76155b448c5a28
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "74687027"
---
# <a name="powershell-samples-for-azure-app-service"></a>Ejemplos de PowerShell para Azure App Service

En la tabla siguiente se incluyen vínculos a scripts de PowerShell creados con Azure PowerShell.

| | |
|-|-|
|**Creación de la aplicación**||
| [Creación de una aplicación con implementación desde GitHub](./scripts/powershell-deploy-github.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Crea una aplicación de App Service que extrae código de GitHub. |
| [Creación de una aplicación con implementación continua desde GitHub](./scripts/powershell-continuous-deployment-github.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Crea una aplicación de App Service que implementa continuamente código desde GitHub. |
| [Creación de una aplicación e implementación de código con FTP](./scripts/powershell-deploy-ftp.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Crea una aplicación de App Service y se cargan archivos de un directorio local con FTP. |
| [Creación de una aplicación e implementación de código desde un repositorio local de GitHub](./scripts/powershell-deploy-local-git.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Crea una aplicación de App Service y configura la inserción de código desde un repositorio de Git local. |
| [Creación de una aplicación e implementación de código en un entorno de ensayo](./scripts/powershell-deploy-staging-environment.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Crea una aplicación de App Service con una ranura de implementación para cambios en el código de ensayo. |
|**Configuración de la aplicación**||
| [Asignar un dominio personalizado a una aplicación](./scripts/powershell-configure-custom-domain.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Crea una aplicación de App Service y le asigna un nombre de dominio personalizado. |
| [Enlace de un certificado SSL personalizado a una aplicación](./scripts/powershell-configure-ssl-certificate.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Crea una aplicación de App Service y enlaza a ella el certificado SSL de un nombre de dominio personalizado. |
|**Escalado de la aplicación**||
| [Escalado manual de una aplicación](./scripts/powershell-scale-manual.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Crea una aplicación de App Service y la escala a través de dos instancias. |
| [Escalado de una aplicación en todo el mundo con una arquitectura de alta disponibilidad](./scripts/powershell-scale-high-availability.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Crea dos aplicaciones de App Service en dos regiones geográficas diferentes y hace que estén disponibles mediante un punto de conexión único con Azure Traffic Manager. |
|**Conexión de la aplicación a recursos**||
| [Conexión de una aplicación a SQL Database](./scripts/powershell-connect-to-sql.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Crea una aplicación de App Service y una base de datos SQL y, a continuación, agrega la cadena de conexión de base de datos a la configuración de la aplicación. |
| [Conexión de una aplicación a una cuenta de almacenamiento](./scripts/powershell-connect-to-storage.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Crea una aplicación de App Service y una cuenta de almacenamiento y, a continuación, agrega la cadena de conexión de almacenamiento a la configuración de la aplicación. |
|**Copia de seguridad y restauración de la aplicación**||
| [Copia de seguridad de una aplicación](./scripts/powershell-backup-onetime.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Crea una aplicación de App Service y una copia de seguridad única para ella. |
| [Creación de una copia de seguridad programada para una aplicación](./scripts/powershell-backup-scheduled.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Crea una aplicación de App Service y una copia de seguridad programada para ella. |
| [Eliminación de una copia de seguridad de una aplicación](./scripts/powershell-backup-delete.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Elimina una copia de seguridad existente de una aplicación. |
| [Restauración de una aplicación desde una copia de seguridad](./scripts/powershell-backup-restore.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Restaura una aplicación desde una copia de seguridad completada previamente. |
| [Restauración de una copia de seguridad entre suscripciones](./scripts/powershell-backup-restore-diff-sub.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Restaura una aplicación web desde una copia de seguridad en otra suscripción. |
|**Supervisión de la aplicación**||
| [Supervisión de una aplicación con registros de servidor web](./scripts/powershell-monitor.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Crea una aplicación de App Service, habilita el registro para ella y descarga los registros en la máquina local. |
| | |
