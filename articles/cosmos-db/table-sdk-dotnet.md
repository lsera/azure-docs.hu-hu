---
title: "Az Azure CosmosDB tábla API .NET SDK & erőforrások |} Microsoft Docs"
description: "Tudnivalók az Azure Cosmos DB tábla API beleértve a kiadási dátum, a használatból való kivonást dátumot és az egyes verziók között végrehajtott módosításokat."
services: cosmos-db
documentationcenter: .net
author: rnagpal
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/21/2018
ms.author: mimig
ms.openlocfilehash: af14e9dbd5f65d5cefa3d9424b8628f10d8160da
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 02/23/2018
---
# <a name="azure-cosmos-db-table-net-api-download-and-release-notes"></a>Az Azure Cosmos DB táblában .NET API: Töltse le és kibocsátási megjegyzések
> [!div class="op_single_selector"]
> * [.NET](table-sdk-dotnet.md)
> * [Java](table-sdk-java.md)
> * [Node.js](table-sdk-nodejs.md)
> * [Python](table-sdk-python.md)

|   |   |
|---|---|
|**SDK letöltése**|[NuGet](https://aka.ms/acdbtablenuget)|
|**API-JÁNAK dokumentációja**|[.NET API-referenciadokumentáció](https://aka.ms/acdbtableapiref)|
|**Gyors útmutató**|[Az Azure Cosmos DB: Egy alkalmazást a .NET- és a tábla API létrehozása](create-table-dotnet.md)|
|**Oktatóanyag**|[Azure Cosmos DB: A tábla API a .NET fejlesztést](tutorial-develop-table-dotnet.md)|
|**Aktuális támogatott keretrendszer**|[A Microsoft .NET-keretrendszer 4.5.1](https://www.microsoft.com/en-us/download/details.aspx?id=40779)|

> [!IMPORTANT]
> Ha az előzetes verzióban hozta létre a Table API-fiókot, hozzon létre egy [új Table API-fiókot](create-table-dotnet.md#create-a-database-account), amely használható az általánosan elérhető Table API SDK-kkal.
>

## <a name="release-notes"></a>Kibocsátási megjegyzések

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0
* Add TableQueryMaxItemCount, TableQueryEnableScan, TableQueryMaxDegreeOfParallelism and TableQueryContinuationTokenLimitInKb to TableRequestOptions
* Hibajavítások

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* Általános rendelkezésre állási kiadás

### <a name="a-name010-preview090-preview"></a><a name="0.1.0-preview"/>0.9.0-Preview
* Kezdeti előzetes kiadás

## <a name="release-and-retirement-dates"></a>Kiadás és a használatból való kivonást dátuma
A Microsoft biztosít értesítési legalább **12 hónapon keresztül** SDK eltávolítása érdekében vagy újabb támogatott verzióra való áttérés előtt.

A [windowsazure.Storage kifejezésre-PremiumTable](https://www.nuget.org/packages/WindowsAzure.Storage-PremiumTable/0.1.0-preview) preview csomag elavult, helyébe a [Microsoft.Azure.CosmosDB.Table](https://www.nuget.org/packages/Microsoft.Azure.CosmosDB.Table) csomag. 2018 November 15. a rendszerből a windowsazure.Storage kifejezésre-PremiumTable SDK, mely arra kéri, a kivont SDK nem engedélyezettek lesznek.

Új szolgáltatásait és funkcióit és optimalizálás csak hozzá az aktuális SDK, így javasoljuk, hogy mindig a legújabb SDK verzióra frissít legkorábban lehető. 

A szolgáltatás bármely Azure Cosmos DB kivont SDK használatával kérelmeket visszautasítja.
<br/>

| Verzió | Kiadás dátuma | Kivezetési dátum |
| --- | --- | --- |
| [1.0.0](#1.0.0) |2017. november 15.|--- |
| [0.9.0-preview](#0.9.0-preview) |2017. november 11. |--- |

## <a name="troubleshooting"></a>Hibaelhárítás

Ha a hibaüzenet jelenik meg 

```
Unable to resolve dependency 'Microsoft.Azure.Storage.Common'. Source(s) used: 'nuget.org', 
'CliFallbackFolder', 'Microsoft Visual Studio Offline Packages', 'Microsoft Azure Service Fabric SDK'`
```

megkísérlésekor. a Microsoft.Azure.CosmosDB.Table NuGet-csomagot használja, a probléma megoldására két lehetőség közül választhat:

* Csomag kezelése konzol segítségével telepíthet a Microsoft.Azure.CosmosDB.Table csomagot és annak függőségeit. Ehhez írja be a következőt a Csomagkezelő konzol megoldást. 
    ```
    Install-Package Microsoft.Azure.CosmosDB.Table -IncludePrerelease
    ```
    
* A Nuget csomag előnyben részesített felügyeleti eszköz használatával telepítse a Microsoft.Azure.Storage.Common Nuget-csomagot a Microsoft.Azure.CosmosDB.Table telepítése előtt.

## <a name="faq"></a>GYIK

[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Lásd még
Az Azure Cosmos DB tábla API-val kapcsolatos további információkért lásd: [Bevezetés az Azure Cosmos DB tábla API](table-introduction.md). 
