---
title: Zonas horarias de la instancia administrada
description: Obtenga información sobre los detalles de la zona horaria de la Instancia administrada de Azure SQL Database
services: sql-database
ms.service: sql-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MladjoA
ms.author: mlandzic
ms.reviewer: ''
ms.date: 09/03/2019
ms.openlocfilehash: 0dbed3db8e106b9bfe1b48ff2b9bc52840fc4c3a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79228584"
---
# <a name="time-zones-in-azure-sql-database-managed-instance"></a>Zonas horarias en la Instancia administrada de Azure SQL Database

Hora universal coordinada (UTC) es la zona horaria recomendada para la capa de datos de soluciones en la nube. La Instancia administrada de Azure SQL Database también ofrece una elección de zonas horarias para satisfacer las necesidades de las aplicaciones existentes que almacenan valores de fecha y hora, y que llaman a funciones de fecha y hora con un contexto implícito de una zona horaria específica.

Las funciones de Transact-SQL, como [GETDATE()](https://docs.microsoft.com/sql/t-sql/functions/getdate-transact-sql) o el código CLR observan la zona horaria establecida en el nivel de instancia. Los trabajos del Agente SQL Server también siguen las programaciones según la zona horaria de la instancia.

  >[!NOTE]
  > La instancia administrada es la única opción de implementación de Azure SQL Database que admite la configuración de zona horaria. Las demás opciones de implementación siguen siempre UTC.
Si necesita interpretar la información de fecha y hora en una zona horaria distinta de UTC, use el valor [AT TIME ZONE](https://docs.microsoft.com/sql/t-sql/queries/at-time-zone-transact-sql) en bases de datos SQL únicas y agrupadas.

## <a name="supported-time-zones"></a>Zonas horarias admitidas

Un conjunto de zonas horarias admitidas se hereda del sistema operativo subyacente de la instancia administrada. Se actualiza periódicamente para obtener nuevas definiciones de zona horaria y reflejar los cambios en las existentes.

[La directiva de cambios de horario de verano y zona horaria](https://aka.ms/time) garantiza una precisión histórica de 2010 en adelante.

Una lista con los nombres de las zonas horarias admitidas se expone a través de la vista [sys.time_zone_info](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-time-zone-info-transact-sql) del sistema.

## <a name="set-a-time-zone"></a>Configuración de una zona horaria

La zona horaria de una instancia administrada se puede configurar solo durante la creación de instancia. La zona horaria predeterminada es UTC.

  >[!NOTE]
  > La zona horaria de una instancia administrada existente no se puede cambiar.

### <a name="set-the-time-zone-through-the-azure-portal"></a>Establezca la zona horaria a través de Azure Portal

Al especificar parámetros para una nueva instancia, seleccione una zona horaria de la lista de zonas horarias admitidas.
  
![Configuración de una zona horaria durante la creación de la instancia](media/sql-database-managed-instance-timezone/01-setting_timezone-during-instance-creation.png)

### <a name="azure-resource-manager-template"></a>Plantilla del Administrador de recursos de Azure

Especifique la propiedad timezoneId en la [plantilla de Resource Manager](https://aka.ms/sql-mi-create-arm-posh) para establecer la zona horaria durante la creación de la instancia.

```json
"properties": {
                "administratorLogin": "[parameters('user')]",
                "administratorLoginPassword": "[parameters('pwd')]",
                "subnetId": "[parameters('subnetId')]",
                "storageSizeInGB": 256,
                "vCores": 8,
                "licenseType": "LicenseIncluded",
                "hardwareFamily": "Gen5",
                "collation": "Serbian_Cyrillic_100_CS_AS",
                "timezoneId": "Central European Standard Time"
            },

```

Al final de este artículo se proporciona una lista de valores admitidos para la propiedad timezoneId.

Si no se especifica, la zona horaria se establece en UTC.

## <a name="check-the-time-zone-of-an-instance"></a>Comprobación de la zona horaria de una instancia

La función [CURRENT_TIMEZONE](https://docs.microsoft.com/sql/t-sql/functions/current-timezone-transact-sql) devuelve un nombre para mostrar de la zona horaria de la instancia.

## <a name="cross-feature-considerations"></a>Consideraciones multicaracterística

### <a name="restore-and-import"></a>Restauración e importación

Puede restaurar un archivo de copia de seguridad o importar datos en una instancia administrada desde una instancia o servidor con una configuración de zona horaria diferente. Al hacerlo, asegúrese de practicar precaución. Analice el comportamiento de la aplicación y los resultados de las consultas e informes, al igual que lo haría al transferir datos entre dos instancias de SQL Server con una configuración de zona horaria diferente.

### <a name="point-in-time-restore"></a>Restauración a un momento dado

Al realizar una restauración de un punto determinado, la hora a la que realizar la restauración se interpreta como hora UTC. De esta forma, se evita cualquier ambigüedad debido al horario de verano y sus posibles cambios.

### <a name="auto-failover-groups"></a>Grupos de conmutación por error automática

No es obligatorio usar la misma zona horaria entre una instancia principal y secundaria de un grupo de conmutación por error, pero se recomienda encarecidamente.

  >[!WARNING]
  > Se recomienda encarecidamente usar la misma zona horaria para la instancia principal y secundaria en un grupo de conmutación por error. Debido a algunos casos de uso poco frecuentes, mantener la misma zona horaria en todas las instancias principales y secundarias no es obligatorio. Es importante comprender que en el caso de una conmutación por error automática o manual, la instancia secundaria conservará su zona horaria original.

## <a name="limitations"></a>Limitaciones

- La zona horaria de una instancia administrada existente no se puede cambiar.
- Los procesos externos que se inician desde los trabajos del Agente SQL Server no observan la zona horaria de la instancia.

## <a name="list-of-supported-time-zones"></a>Lista de zonas horarias admitidas

| **Id. de zona horaria** | **Nombre para mostrar de la zona horaria** |
| --- | --- |
| Hora estándar de línea de fecha | (UTC-12:00) Línea internacional de cambio de fecha oeste |
| UTC-11 | (UTC-11:00) Hora universal coordinada-11 |
| Hora estándar aleutiana | (UTC-10:00) Islas Aleutianas |
| Hora estándar de Hawái | (UTC-10:00) Hawái |
| Hora estándar de Marquesas | (UTC-09:30) Islas Marquesas |
| Hora estándar de Alaska | (UTC-09:00) Alaska |
| UTC-09 | (UTC-09:00) Hora universal coordinada-09 |
| Hora estándar del Pacífico (México) | (UTC-08:00) Baja California |
| UTC-08 | (UTC-08:00) Hora universal coordinada-08 |
| Hora estándar del Pacífico | (UTC-08:00) Hora del Pacífico (EE.UU. y Canadá) |
| Hora estándar de las Montañas, EE. UU. | (UTC-07:00) Arizona |
| Hora estándar de las Montañas (México) | (UTC-07:00) Chihuahua, La Paz, Mazatlán |
| Hora estándar de las Montañas | (UTC-07:00) Hora de las Montañas Rocosas (EE. UU. y Canadá) |
| Hora estándar de Centroamérica | (UTC-06:00) América Central |
| Hora estándar del Centro | (UTC-06:00) Hora central (EE.UU. y Canadá) |
| Hora estándar de Isla de Pascua | (UTC-06:00) Isla de Pascua |
| Hora estándar del Centro (México) | (UTC-06:00) Guadalajara, Ciudad de México, Monterrey |
| Hora estándar del Centro de Canadá | (UTC-06:00) Saskatchewan |
| Hora estándar del Pacífico, Sudamérica | (UTC-05:00) Bogotá, Lima, Quito, Río Branco |
| Hora estándar del Este (México) | (UTC-05:00) Chetumal |
| Hora estándar del Este | (UTC-05:00) Hora del este (EE.UU. y Canadá) |
| Hora estándar de Haití | (UTC-05:00) Haití |
| Hora estándar de Cuba | (UTC-05:00) La Habana |
| Hora estándar del Este de EE. UU. | (UTC-05:00) Indiana (este) |
| Hora estándar de Islas Turcas y Caicos | (UTC-05:00) Islas Turcas y Caicos |
| Hora estándar de Paraguay | (UTC-04:00) Asunción |
| Hora estándar del Atlántico | (UTC-04:00) Hora del Atlántico (Canadá) |
| Hora estándar de Venezuela | (UTC-04:00) Caracas |
| Hora estándar del Centro, Brasil | (UTC-04:00) Cuiabá |
| Hora estándar del Este, Sudamérica | (UTC-04:00) Georgetown, La Paz, Manaos, San Juan |
| Hora estándar del Pacífico, Sudamérica | (UTC-04:00) Santiago |
| Hora estándar de Terranova | (UTC-03:30) Terranova |
| Estándar de Tocantins | (UTC-03:00) Araguaína |
| E. Hora estándar Sudamérica | (UTC-03:00) Brasilia |
| Hora estándar de Sudamérica Este | (UTC-03:00) Cayena, Fortaleza |
| Hora estándar de Argentina | (UTC-03:00) Ciudad Autónoma de Buenos Aires |
| Hora estándar de Groenlandia | (UTC-03:00) Groenlandia |
| Hora estándar de Montevideo | (UTC-03:00) Montevideo |
| Hora estándar de Magallanes | (UTC-03:00) Punta Arenas |
| Hora estándar de Saint Pierre | (UTC-03:00) San Pedro y Miquelón |
| Hora estándar de Bahía | (UTC-03:00) Salvador |
| UTC-02 | (UTC-02:00) Hora universal coordinada-02 |
| Hora estándar del Atlántico Central | (UTC-02:00) Atlántico central - Antiguo |
| Hora estándar de las Azores | (UTC-01:00) Azores |
| Hora estándar de Cabo Verde | (UTC-01:00) Isla de Cabo Verde |
| UTC | (UTC) Hora universal coordinada |
| Hora estándar GMT | (UTC+00:00) Dublín, Edimburgo, Lisboa, Londres |
| Hora estándar de Greenwich | (UTC+00:00) Monrovia, Reykjavik |
| Hora estándar Europa Oriental | (UTC+01:00) Ámsterdam, Berlín, Berna, Roma, Estocolmo, Viena |
| Hora estándar Europa Central | (UTC+01:00) Belgrado, Bratislava, Budapest, Liubliana, Praga |
| Hora estándar romance | (UTC+01:00) Bruselas, Copenhague, Madrid, París |
| Hora estándar de Marruecos | (UTC+01:00) Casablanca |
| Hora estándar de Santo Tomé | (UTC+01:00) Santo Tomé |
| Hora estándar centroeuropea | (UTC+01:00) Sarajevo, Skopie, Varsovia, Zagreb |
| Hora estándar de África Central Occidental | (UTC+01:00) África Central Occidental |
| Hora estándar de Jordania | (UTC+02:00) Ammán |
| Hora estándar GTB | (UTC+02:00) Atenas, Bucarest |
| Hora estándar de Oriente Medio | (UTC+02:00) Beirut |
| Hora estándar de Egipto | (UTC+02:00) El Cairo |
| E. estándar Europa Oriental | (UTC+02:00) Chisinau |
| Hora estándar de Siria | (UTC+02:00) Damasco |
| Hora estándar de Cisjordania | (UTC+02:00) Gaza, Hebrón |
| Hora estándar de Sudáfrica | (UTC+02:00) Harare, Pretoria |
| Hora estándar FLE | (UTC+02:00) Helsinki, Kiev, Riga, Sofia, Tallin, Vilna |
| Hora estándar de Israel | (UTC+02:00) Jerusalén |
| Hora estándar de Kaliningrado | (UTC+02:00) Kaliningrado |
| Hora estándar de Sudán | (UTC+02:00) Jartum |
| Hora estándar de Libia | (UTC+02:00) Trípoli |
| Hora estándar de Namibia | (UTC+02:00) Windhoek |
| Hora estándar arábica | (UTC+03:00) Bagdad |
| Hora estándar de Turquía | (UTC+03:00) Estambul |
| Hora estándar árabe | (UTC+03:00) Kuwait, Riad |
| Hora estándar de Belarús | (UTC+03:00) Minsk |
| Hora estándar de Rusia | (UTC+03:00) Moscú, San Petersburgo |
| E. Hora estándar de África Oriental | (UTC+03:00) Nairobi |
| Hora estándar de Irán | (UTC+03:30) Teherán |
| Hora estándar árabe | (UTC+04:00) Abu Dhabi, Mascate |
| Hora estándar de Astrakhan | (UTC+04:00) Astrakhan, Ulyanovsk |
| Hora estándar de Azerbaiyán | (UTC+04:00) Bakú |
| Zona horaria 3 de Rusia | (UTC+04:00) Izhevsk, Samara |
| Hora estándar de Mauricio | (UTC+04:00) Port Louis |
| Hora estándar de Sarátov | (UTC + 04:00) Sarátov |
| Hora estándar de Georgia | (UTC+04:00) Tiflis |
| Hora estándar de Volgogrado | (UTC+04:00) Volgogrado |
| Hora estándar del Cáucaso | (UTC+04:00) Ereván |
| Hora estándar de Afganistán | (UTC+04:30) Kabul |
| Hora estándar Asia Occidental | (UTC+05:00) Asjabad, Tashkent |
| Hora estándar de Ekaterimburgo | (UTC+05:00) Ekaterimburgo |
| Hora estándar de Pakistán | (UTC+05:00) Islamabad, Karachi |
| Hora estándar de La India | (UTC+05:30) Chennai, Kolkata, Mumbai, Nueva Delhi |
| Hora estándar de Sri Lanka | (UTC+05:30) Sri Jayawardenepura |
| Hora estándar de Nepal | (UTC+05:45) Katmandú |
| estándar de Asia Central Norte | (UTC+06:00) Astaná |
| Hora estándar de Bangladés | (UTC+06:00) Dacca |
| Hora estándar de Omsk | (UTC+06:00) Omsk |
| Hora estándar de Myanmar | (UTC+06:30) Yangón (Rangún) |
| Hora estándar de Asia Sureste | (UTC+07:00) Bangkok, Hanói, Yakarta |
| Hora estándar de Altai | (UTC+07:00) Barnaul, Gorno-Altaysk |
| Hora estándar de Mongolia Occidental | (UTC+07:00) Hovd |
| Hora estándar Asia Norte | (UTC+07:00) Krasnoyarsk |
| Hora estándar de Asia Central Norte | (UTC+07:00) Novosibirsk |
| Hora estándar de Tomsk | (UTC+07:00) Tomsk |
| Hora estándar de China | (UTC+08:00) Beijing, Chongqing, Hong Kong, Urumqi |
| Hora estándar del este de Asia Norte | (UTC+08:00) Irkutsk |
| Hora estándar de Singapur | (UTC+08:00) Kuala Lumpur, Singapur |
| Hora Hora estándar de Australia Oriental | (UTC+08:00) Perth |
| Hora estándar de Taipéi | (UTC+08:00) Taipéi |
| Hora estándar de Ulán Bator | (UTC+08:00) Ulán Bator |
| Hora estándar de Australia Central Oeste | (UTC+08:45) Eucla |
| Hora estándar de Transbaikal | (UTC+09:00) Chita |
| Hora estándar de Tokio | (UTC+09:00) Osaka, Sapporo, Tokio |
| Hora estándar de Corea del Norte | (UTC+09:00) Pyongyang |
| Hora estándar de Corea | (UTC+09:00) Seúl |
| Hora estándar de Yakutsk | (UTC+09:00) Yakutsk |
| Hora Hora estándar de Australia Oriental | (UTC+09:30) Adelaida |
| Hora estándar de Australia Central | (UTC+09:30) Darwin |
| E. Hora estándar de Australia Oriental | (UTC+10:00) Brisbane |
| Hora estándar Australia Oriental | (UTC+10:00) Canberra, Melbourne, Sídney |
| Hora estándar del Pacífico Occidental | (UTC+10:00) Guam, Port Moresby |
| Hora estándar de Tasmania | (UTC+10:00) Hobart |
| Hora estándar de Vladivostok | (UTC+10:00) Vladivostok |
| Hora estándar de Lord Howe | (UTC+10:30) Isla Lord Howe |
| Hora estándar de Bougainville | (UTC+11:00) Isla Bougainville |
| Zona horaria 10 de Rusia | (UTC+11:00) Chokurdakh |
| Hora estándar de Magadán | (UTC+11:00) Magadán |
| Hora estándar de Norfolk | (UTC+11:00) Isla Norfolk |
| Hora estándar de Sajalín | (UTC+11:00) Sajalín |
| Hora estándar del Pacífico Central | (UTC+11:00) Islas Solomon, Nueva Caledonia |
| Zona horaria 11 de Rusia | (UTC+12:00) Anádyr, Petropávlovsk-Kamchatski |
| Hora estándar de Nueva Zelanda | (UTC+12:00) Auckland, Wellington |
| UTC+12 | (UTC+12:00) Hora universal coordinada+12 |
| Hora estándar de Fiyi | (UTC+12:00) Fiyi |
| Hora estándar de Kamchatka | (UTC+12:00) Petropavlovsk-Kamchatsky - Antiguo |
| Hora estándar de las Islas Chatham | (UTC+12:45) Islas Chatham |
| UTC+13 | (UTC+13:00) Hora universal coordinada+13 |
| Hora estándar de Tonga | (UTC+13:00) Nukualofa |
| Hora estándar de Samoa | (UTC+13:00) Samoa |
| Hora estándar de Islas de la Línea | (UTC+14:00) Isla de Kiritimati |

## <a name="see-also"></a>Consulte también 

- [CURRENT_TIMEZONE (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/functions/current-timezone-transact-sql)
- [AT TIME ZONE (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/queries/at-time-zone-transact-sql)
- [sys.time_zone_info (Transact-SQL)](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-time-zone-info-transact-sql)
