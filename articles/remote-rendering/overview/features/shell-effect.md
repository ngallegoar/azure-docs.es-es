---
title: Representación del shell
description: Explica cómo usar el efecto de representación del shell
author: jumeder
ms.author: jumeder
ms.date: 10/23/2020
ms.topic: article
ms.custom: devx-track-csharp
ms.openlocfilehash: f59c4f8225d31b61df08f30863c8b9300e20e820
ms.sourcegitcommit: 6109f1d9f0acd8e5d1c1775bc9aa7c61ca076c45
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2020
ms.locfileid: "94447337"
---
# <a name="shell-rendering"></a>Representación del shell

El estado del shell del [componente de invalidación de estado jerárquico](../../overview/features/override-hierarchical-state.md) es un efecto de transparencia. A diferencia de la representación [transparente](../../overview/features/override-hierarchical-state.md), solo está visible la capa de los objetos que está en primer plano, de forma similar a lo que sucede en la representación opaca. Además, la apariencia normal de los objetos se puede modificar cuando se representan como shells. El efecto está pensado para los casos de uso en los que el usuario se debe alejar visualmente de las partes no importantes y mantener una conciencia espacial de toda la escena.

La apariencia de los objetos representados por el shell se puede configurar a través del estado global `ShellRenderingSettings`. Todos los objetos que usan la representación del shell utilizarán el mismo valor. No hay parámetros por objeto.

## <a name="shellrenderingsettings-parameters"></a>Parámetros de ShellRenderingSettings

La clase `ShellRenderingSettings` contiene la configuración relacionada con las propiedades de la representación del shell global:

| Parámetro      | Tipo    | Description                                             |
|----------------|---------|---------------------------------------------------------|
| `Desaturation` | FLOAT   | La cantidad de desaturación que se aplica al color final habitual del objeto. El valor oscila entre 0 (sin desaturación) y 1 (desaturación total) |
| `Opacity`      | FLOAT   | La opacidad de los objetos representados por el shell. El valor oscila entre 0 (invisible) y 1 (completamente opaco) |

Vea también la tabla siguiente, donde encontrará ejemplos de los efectos de los parámetros cuando se aplican a una escena completa:

|                | 0 | 0,25 | 0.5 | 0,75 | 1.0 | 
|----------------|:-:|:----:|:---:|:----:|:---:|
| **Desaturación** | ![Desaturación: 0,0](./media/shell-desaturation-00.png) | ![Desaturación: 0,25](./media/shell-desaturation-025.png) | ![Desaturación: 0,5](./media/shell-desaturation-05.png) | ![Desaturación: 0,75](./media/shell-desaturation-075.png) | ![Desaturación: 1,0](./media/shell-desaturation-10.png) |
| **Opacidad**      | ![Opacidad: 0,0](./media/shell-opacity-00.png) | ![Opacidad: 0,25](./media/shell-opacity-025.png) | ![Opacidad: 0,5](./media/shell-opacity-05.png) | ![Opacidad: 0,75](./media/shell-opacity-075.png) | ![Opacidad: 1,0](./media/shell-opacity-10.png) |

El efecto de shell se aplica al color opaco final con el que se representará la escena en caso contrario. Aquí se incluye la [invalidación del estado jerárquico del tinte](../../overview/features/override-hierarchical-state.md).

## <a name="example"></a>Ejemplo

El código siguiente muestra un uso de ejemplo del estado `ShellRenderingSettings` a través de la API:

```cs
void SetShellSettings(AzureSession session)
{
    ShellRenderingSettings shellRenderingSettings = session.Actions.ShellRenderingSettings;
    shellRenderingSettings.Desaturation = 0.5f;
    shellRenderingSettings.Opacity = 0.1f;
}
```

```cpp
void SetShellSettings(ApiHandle<AzureSession> session)
{
    ApiHandle<ShellRenderingSettings> shellRenderingSettings = session->Actions()->GetShellRenderingSettings();
    shellRenderingSettings->SetDesaturation(0.5f);
    shellRenderingSettings->SetOpacity(0.1f);
}
```

## <a name="performance"></a>Rendimiento

La característica de representación del shell conlleva una pequeña sobrecarga constante, en comparación con la representación opaca estándar. Es considerablemente más rápido que usar materiales transparentes en objetos o que la representación [transparente](../../overview/features/override-hierarchical-state.md). El rendimiento empeorar con mayor intensidad si no cambian a la representación del shell toda una escena, sino solo algunas partes. Esta degradación puede producirse porque los objetos adicionales que se revelan requieren representación. A este respecto, el rendimiento se comporta de forma similar a la característica de [planos de corte](../../overview/features/cut-planes.md).

## <a name="next-steps"></a>Pasos siguientes

* [Componente de invalidación de estado jerárquico](../../overview/features/override-hierarchical-state.md)