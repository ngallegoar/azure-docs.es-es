---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/30/2020
ms.author: glenga
ms.openlocfilehash: 44969063765099d350e21abfbd07792891443911
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80673206"
---
::: zone pivot="programming-language-python"  
## <a name="create-and-activate-a-virtual-environment"></a><a name="create-venv"></a>Creación y activación de un entorno virtual

En una carpeta adecuada, ejecute los comandos siguientes para crear y activar un entorno virtual denominado `.venv`. Asegúrese de usar Python 3.8, 3.7 o 3.6, que son compatibles con Azure Functions.

# <a name="bash"></a>[bash](#tab/bash)

```bash
python -m venv .venv
```

```bash
source .venv/bin/activate
```

Si Python no instaló el paquete venv en la distribución de Linux, ejecute el siguiente comando:

```bash
sudo apt-get install python3-venv
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

```powershell
py -m venv .venv
```

```powershell
.venv\scripts\activate
```

# <a name="cmd"></a>[Cmd](#tab/cmd)

```cmd
py -m venv .venv
```

```cmd
.venv\scripts\activate
```

---

Ejecute todos los comandos siguientes en este entorno virtual activado. (Para salir del entorno virtual, ejecute `deactivate`).
::: zone-end