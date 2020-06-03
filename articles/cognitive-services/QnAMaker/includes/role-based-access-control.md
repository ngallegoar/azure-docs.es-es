---
title: archivo de inclusión
description: archivo de inclusión
ms.topic: include
ms.custom: include file
ms.date: 03/11/2020
ms.openlocfilehash: b34d3ed2481600cde7ffb3275d635b8a57926bd6
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83665923"
---
Se proporcionan los siguientes roles para la colaboración:

|Role|Funcionalidades|Acceso de API|Permisos de API|
|--|--|--|--|
|Propietario|All|Clave de autenticación|All|
|Colaborador|Todas, excepto la capacidad de agregar nuevos miembros a los roles|Clave de autenticación|Todas, excepto la capacidad de agregar nuevos miembros a los roles|
|Leer QnA Maker<br>(lectura)|Exportar/Descargar<br>Prueba|Token de portador|1. Descargar API de KB<br>2. Lista de KB para la API de usuario<br>3. Obtención de los detalles de la base de conocimientos<br>4. Descargar modificaciones<br>Generar respuestas |
|Editor de QnA Maker<br>(lectura/escritura)|Exportar/Descargar<br>Prueba<br>Actualizar KB<br>Exportar KB<br>Importar KB<br>Reemplazar KB<br>Crear una base de conocimiento|Token de portador|1. Crear API de KB<br>2. Actualizar API de KB<br>3. Reemplazar API de KB<br>4. Reemplazar modificaciones<br>5. "Entrenar API" [en el nuevo modelo de servicio v5]|
|Usuario de Cognitive Service<br>(lectura, escritura y publicación)|All|Token de portador|All|