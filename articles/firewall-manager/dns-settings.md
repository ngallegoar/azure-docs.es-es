---
title: Configuración de DNS de directiva de Azure Firewall (versión preliminar)
description: Puede configurar las directivas de Azure Firewall con la configuración del servidor DNS y del proxy DNS.
services: firewall-manager
author: vhorne
ms.service: firewall-manager
ms.topic: how-to
ms.date: 06/30/2020
ms.author: victorh
ms.openlocfilehash: bf189e4c9853d9f2ff6e8495423f4f36f14f41d4
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "85611805"
---
# <a name="azure-firewall-policy-dns-settings-preview"></a>Configuración de DNS de directiva de Azure Firewall (versión preliminar)

> [!IMPORTANT]
> La configuración DNS de Azure Firewall actualmente está en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Puede configurar un servidor DNS personalizado y habilitar el proxy DNS para las directivas de Azure Firewall. Puede configurar estas opciones al implementar el firewall o una versión posterior desde la página **Configuración DNS**.

## <a name="dns-servers"></a>Servidores DNS

Un servidor DNS mantiene y resuelve los nombres de dominio en direcciones IP. De forma predeterminada, Azure Firewall usa Azure DNS para la resolución de nombres. La configuración de **servidor DNS** le permite configurar sus propios servidores DNS para la resolución de nombres de Azure Firewall. Puede configurar un único o varios servidores.

### <a name="configure-custom-dns-servers"></a>Configuración de servidores DNS personalizados

1. Seleccione la directiva de firewall.
2. En **Configuración**, seleccione **Configuración DNS**.
3. En **Servidores DNS**, puede escribir o agregar servidores DNS existentes que se hayan especificado previamente en la red virtual.
4. Seleccione **Guardar**.
5. El firewall dirige ahora el tráfico DNS a los servidores DNS especificados para la resolución de nombres.

## <a name="dns-proxy-preview"></a>Proxy DNS (versión preliminar)

Puede configurar Azure Firewall para que actúe como proxy DNS. Un proxy DNS actúa como intermediario para las solicitudes DNS de las máquinas virtuales cliente a un servidor DNS. Si configura un servidor DNS personalizado, debe habilitar el proxy DNS para evitar la falta de coincidencia de la resolución DNS y habilitar el filtrado de FQDN en las reglas de red.

Si no habilita el proxy DNS, las solicitudes DNS del cliente pueden viajar a un servidor DNS en un momento diferente o devolver una respuesta diferente en comparación con la del firewall. El proxy DNS coloca Azure Firewall en la ruta de acceso de las solicitudes de cliente para evitar incoherencias.

La configuración del proxy DNS requiere tres pasos:
1. Habilite el proxy DNS en la configuración DNS de Azure Firewall.
2. Opcionalmente, configure el servidor DNS personalizado o use el valor predeterminado proporcionado.
3. Por último, debe configurar la dirección IP privada de Azure Firewall como una dirección DNS personalizada en la configuración del servidor DNS de la red virtual. Esto garantiza que el tráfico DNS se dirija a Azure Firewall.

### <a name="configure-dns-proxy-preview"></a>Configuración del proxy DNS (versión preliminar)

Para configurar el proxy DNS, debe configurar los servidores DNS de la red virtual para que usen la dirección IP privada del firewall. A continuación, habilite el proxy DNS en la **configuración DNS** de la directiva de Azure Firewall.

#### <a name="configure-virtual-network-dns-servers"></a>Configuración de los servidores DNS de la red virtual

1. Seleccione la red virtual a la que se enrutará el tráfico DNS mediante Azure Firewall.
2. En **Configuración**, seleccione **Servidores DNS**.
3. Seleccione **Personalizado**, en **Servidores DNS**.
4. Escriba la dirección IP privada del firewall.
5. Seleccione **Guardar**.

#### <a name="enable-dns-proxy-preview"></a>Habilitación del proxy DNS (versión preliminar)

1. Seleccione la directiva de Azure Firewall.
2. En **Configuración**, seleccione **Configuración DNS**.
3. De manera predeterminada, el **proxy DNS** está deshabilitado. Cuando está habilitado, el firewall escucha en el puerto 53 y reenvía las solicitudes DNS a los servidores DNS configurados.
4. Revise la configuración de los **servidores DNS** para asegurarse de que la configuración sea adecuada para su entorno.
5. Seleccione **Guardar**.

## <a name="next-steps"></a>Pasos siguientes

[Filtrado de FQDN en reglas de red](fqdn-filtering-network-rules.md)