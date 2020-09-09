---
title: Protección contra amenazas para Azure Key Vault
description: En este artículo se explica cómo configurar Advanced Threat Protection para Azure Key Vault en Azure Security Center
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: memildin
ms.openlocfilehash: 7025b091b83a56b7b3df18c3f42cb84b163560bb
ms.sourcegitcommit: c94a177b11a850ab30f406edb233de6923ca742a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/01/2020
ms.locfileid: "89277772"
---
# <a name="threat-protection-for-azure-key-vault-preview"></a>Protección contra amenazas para Azure Key Vault (versión preliminar)

Advanced Threat Protection para Azure Key Vault proporciona una capa adicional de inteligencia de seguridad. Esta herramienta permite detectar intentos potencialmente perjudiciales de aprovechar las vulnerabilidades de sus cuentas de CosmosDB o de acceder a ellas. Gracias a la instancia nativa de Advanced Threat Protection de Azure Security Center, puede abordar amenazas sin ser un experto en seguridad y sin tener que aprender a manejar más sistemas de supervisión de seguridad.

Cuando Security Center detecta una actividad anómala, muestra alertas. También envía por correo electrónico al administrador de la suscripción los detalles de la actividad sospechosa y recomendaciones sobre cómo investigar y corregir las amenazas identificadas.

## <a name="configure-threat-protection-from-security-center"></a>Configuración de la protección contra amenazas desde Security Center

De forma predeterminada, Advanced Threat Protection está habilitado en todas las cuentas de Key Vault cuando se suscribe al plan de tarifa estándar de Security Center. Para obtener más información, consulte el apartado [Precios](security-center-pricing.md).

Para habilitar o deshabilitar la protección de una suscripción específica:

1. En el panel izquierdo de Security Center, seleccione **Precios y configuración**.

1. Seleccione la suscripción con las cuentas de almacenamiento para las que quiera habilitar o deshabilitar la protección contra amenazas.

1. Seleccione **Plan de tarifa**.

1. En el grupo **Seleccionar el plan de tarifa por tipo de recurso**, busque la fila de **Key Vault** y haga clic en **Habilitado** o **Deshabilitado**.

    [![Habilitar o deshabilitar Advanced Threat Protection de Key Vault en Azure Security Center](media/advanced-threat-protection-key-vault/atp-for-akv-enable-atp-for-akv.png)](media/advanced-threat-protection-key-vault/atp-for-akv-enable-atp-for-akv.png#lightbox)

1. Seleccione **Guardar**.


## <a name="next-steps"></a>Pasos siguientes

En este artículo aprendió a habilitar y deshabilitar Advanced Threat Protection de Azure Key Vault. 

Para obtener material relacionado, vea los siguientes artículos:

- [Protección contra amenazas en Azure Security Center](threat-protection.md): en este artículo se describen los orígenes de las alertas de seguridad en Azure Security Center.
- [Alertas de seguridad de Key Vault](alerts-reference.md#alerts-azurekv): la sección de Key Vault de la tabla de referencia para todas las alertas de Azure Security Center