---
title: Qué es la cadena de bloques Data Manager para Azure Blockchain Service
description: La cadena de bloques Data Manager para capturar, transformar y entregar datos de Blockchain a Temas de Event Grid.
ms.date: 11/04/2019
ms.topic: conceptual
ms.reviewer: chroyal
ms.openlocfilehash: 78c93880007259267b26cf53e93de722be1c7323
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77209450"
---
# <a name="what-is-blockchain-data-manager-for-azure-blockchain-service"></a>¿Qué es Blockchain Data Manager para Azure Blockchain Service?

La cadena de bloques Data Manager captura, transforma y entrega datos de transacciones de Azure Blockchain Service a Temas de Azure Event Grid que proporcionan una integración de cadena de bloques de contabilidad confiable y escalable con los servicios de Azure.

En la mayoría de los escenarios de Blockchain empresarial, un libro de Blockchain es una parte de una solución. Por ejemplo, para transferir un recurso de una entidad a otra, necesita un mecanismo para enviar la transacción. Luego, necesita un mecanismo para leer los datos de contabilidad para asegurarse de que se ha producido la transacción, se aceptó y los cambios de estado resultantes se integran con la solución de un extremo a otro. En este ejemplo, si escribe un contrato inteligente para transferir recursos, puede usar la cadena de bloques Data Manager para integrar aplicaciones y almacenes de datos fuera de la cadena. En el ejemplo de transferencia de recursos, cuando se transfiere un recurso en el Blockchain, los eventos y los cambios de estado de propiedades los entrega la cadena de bloques Data Manager a través de Event Grid. Después, puede usar varios controladores de eventos posibles para Event Grid almacenar los datos de Blockchain fuera de la cadena o reaccionar a los cambios de estado en tiempo real.

La cadena de bloques Data Manager realiza tres funciones principales: capturar, transformar y entregar.

![Funciones de la cadena de bloques Data Manager](./media/data-manager/functions.png)

## <a name="capture"></a>Capturar

Cada instancia de Blockchain Data Manager se conecta a un nodo de transacción miembro de Azure Blockchain Service. Solo los usuarios con acceso al nodo de transacción pueden crear una conexión, lo que garantiza el control de acceso adecuado a los datos del cliente. Una instancia de la cadena de bloques Data Manager captura de forma confiable todos los datos de bloques sin procesar y de transacciones sin procesar del nodo de transacción y se puede escalar para admitir cargas de trabajo empresariales.

## <a name="transform"></a>Transformación

Puede usar la cadena de bloques Data Manager para descodificar el estado de los eventos y las propiedades mediante la configuración de aplicaciones de contrato inteligente en la cadena de bloques Data Manager. Para agregar un contrato inteligente, proporcione el contrato ABI y el código de bytes. La cadena de bloques Data Manager usa los artefactos de contrato inteligente para descodificar y detectar direcciones de contrato. Después de agregar la aplicación Blockchain a la instancia, la cadena de bloques Data Manager detecta de forma dinámica la dirección del contrato inteligente cuando se implementa el contrato inteligente en el consorcio y envía el estado de la propiedad y el evento descodificado a los destinos configurados.

## <a name="deliver"></a>Entrega

La cadena de bloques Data Manager admite varias conexiones salientes de tema Event Grid para cualquier instancia de cadena de bloques Data Manager determinada. Puede enviar datos de la cadena de bloques a un único destino o enviar datos de la cadena de bloques a varios destinos. Con la cadena de bloques Data Manager, puede crear una solución escalable de publicación de datos basada en eventos para cualquier implementación de Blockchain.

## <a name="configuration-options"></a>Opciones de configuración

Puede configurar la cadena de bloques Data Manager para satisfacer las necesidades de la solución. Por ejemplo, puede aprovisionar:

* Una única instancia de la cadena de bloques Data Manager para un miembro Azure Blockchain Service.
* Una instancia de la cadena de bloques Data Manager por nodo de transacción Azure Blockchain Service. Por ejemplo, los nodos de transacción privados pueden tener su propia instancia de cadena de bloques Data Manager para mantener la confidencialidad.
* Una instancia de cadena de bloques Data Manager puede admitir varias conexiones de salida. Una instancia de cadena de bloques Data Manager se puede usar para administrar todos los puntos de integración de publicación de datos para un miembro Azure Blockchain Service.

## <a name="next-steps"></a>Pasos siguientes

Intente [creando una instancia de cadena de bloques Data Manager](data-manager-portal.md) para un miembro Azure Blockchain Service.
