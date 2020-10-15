---
title: Optimización del tiempo de consulta mediante la estrategia de almacenamiento de tablas TOAST en Azure Database for PostgreSQL con un único servidor
description: En este artículo se describe cómo optimizar el tiempo de consulta con la estrategia de almacenamiento de tablas TOAST en Azure Database for PostgreSQL con un único servidor.
author: dianaputnam
ms.author: dianas
ms.service: postgresql
ms.topic: how-to
ms.date: 5/6/2019
ms.openlocfilehash: 3f1fa0affb821b00d4f5529841533e854e634377
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86116189"
---
# <a name="optimize-query-time-with-the-toast-table-storage-strategy"></a>Optimización del tiempo de consulta con la estrategia de almacenamiento de tablas TOAST 
En este artículo se describe cómo optimizar los tiempos de consulta con la estrategia de almacenamiento de tablas TOAST (técnica de almacenamiento de atributos de gran tamaño).

## <a name="toast-table-storage-strategies"></a>Estrategias de almacenamiento de tablas TOAST
Se utilizan cuatro estrategias diferentes para almacenar las columnas en el disco, las cuales pueden utilizar TOAST. Representan diversas combinaciones de compresión y almacenamiento fuera de línea. La estrategia puede establecerse en el nivel de tipo de datos y el nivel de columna.
- **Plain** evita la compresión y el almacenamiento fuera de línea. Deshabilita el uso de encabezados de byte único para los tipos de matriz de longitud variable. Plain es la única estrategia posible para las columnas de tipos de datos que no pueden usar TOAST.
- **Extended** permite tanto la compresión como el almacenamiento fuera de línea. Extended es el valor predeterminado para la mayoría de los tipos de datos que pueden usar TOAST. Primero se intenta la compresión. Si la fila sigue siendo demasiado grande, se prueba el almacenamiento fuera de línea.
- **External** permite el almacenamiento fuera de línea, pero no la compresión. Si usa External, las operaciones de subcadena en columnas anchas de texto y bytes son más rápidas. Aunque esta velocidad implica un aumento del espacio de almacenamiento. Estas operaciones se optimizan para capturar solo los elementos necesarios del valor fuera de línea cuando no está comprimido.
- **Main** permite la compresión, pero no el almacenamiento fuera de línea. Aún así, se realiza el almacenamiento fuera de línea para estas columnas, aunque solo como último recurso. Se produce cuando no hay ninguna otra manera de hacer que la fila sea lo suficientemente pequeña como para caber en una página.

## <a name="use-toast-table-storage-strategies"></a>Uso de las estrategias de almacenamiento de tablas TOAST
Si sus consultas acceden a tipos de datos que pueden usar TOAST, considere la posibilidad de usar la estrategia Main en lugar de la opción predeterminada Extended para reducir los tiempos de consulta. Main no excluye al almacenamiento fuera de línea. Si las consultas no acceden a los tipos de datos que pueden usar TOAST, puede ser ventajoso mantener la opción Extended. Una parte más grande de las filas de la tabla principal cabrá en la caché del búfer compartido, lo que ayuda al rendimiento.

Si tiene una carga de trabajo que use un esquema con tablas anchas y un gran recuento de caracteres, considere el uso de las tablas TOAST de PostgreSQL. Una tabla de ejemplo de un cliente tenía más de 350 columnas con varias columnas que abarcaban 255 caracteres. Una vez que la convirtió a la estrategia Main de la tabla TOAST, su tiempo de consulta de prueba comparativa se redujo de 4203 segundos a 467. Es una mejora del 89 por ciento.

## <a name="next-steps"></a>Pasos siguientes
Revise la carga de trabajo en busca de las características anteriores. 

Revise la siguiente documentación de PostgreSQL: 
- [Capítulo 68, Almacenamiento físico de base de datos](https://www.postgresql.org/docs/current/storage-toast.html) 