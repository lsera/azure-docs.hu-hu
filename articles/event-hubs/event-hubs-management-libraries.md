---
title: "Az Azure Event Hubs kezelési kódtárakat |} Microsoft Docs"
description: "Az Event Hubs-névterek és az entitásokat a .NET-kezelése"
services: event-hubs
cloud: na
documentationcenter: na
author: sethmanheim
manager: timlt
ms.assetid: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 12/19/2017
ms.author: sethm
ms.openlocfilehash: 2ae2f8f2006507284338fb4fa62e4942476cf2bc
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/20/2017
---
# <a name="event-hubs-management-libraries"></a>Event Hubs kezelési kódtárakat

Az Event Hubs kezelési kódtárakat dinamikusan építhető ki az Event Hubs-névterek és az entitásokat. Dinamikus jellegű üzenetkezelési forgatókönyveket, és a komplex központi telepítései lehetővé teszi, hogy milyen entitások kiépítését programozott módon meghatározhatja. Ezek a kódtárak jelenleg érhetők el a .NET-hez.

## <a name="supported-functionality"></a>Támogatott funkciók

* Namespace létrehozási, frissítési, törlési
* Event Hubs létrehozási, frissítési, törlési
* Felhasználói csoport létrehozása, frissítés, törlés

## <a name="prerequisites"></a>Előfeltételek

Első lépések használatával az Event Hubs kezelési kódtárakat, hitelesítenie kell az Azure Active Directory (AAD). Az AAD megköveteli, hogy hitelesítse magát egy egyszerű szolgáltatást, amely az Azure-erőforrások hozzáférést tesz lehetővé. Egyszerű szolgáltatás létrehozása kapcsolatos információkért tekintse meg a következő cikkeket:  

* [Active Directory-alkalmazás és az erőforrások eléréséhez egyszerű szolgáltatás létrehozása az Azure-portál használatával](../azure-resource-manager/resource-group-create-service-principal-portal.md)
* [Szolgáltatásnév létrehozása erőforrások eléréséhez az Azure PowerShell használatával](../azure-resource-manager/resource-group-authenticate-service-principal.md)
* [Szolgáltatásnév létrehozása erőforrások eléréséhez az Azure CLI használatával](../azure-resource-manager/resource-group-authenticate-service-principal-cli.md)

Ezek az oktatóanyagok biztosítanak egy `AppId` (ügyfél-azonosító), `TenantId`, és `ClientSecret` (hitelesítési kulcs), amelyek használnak a hitelesítéshez a felügyeleti függvénytárai. Rendelkeznie kell **tulajdonos** ahhoz az erőforráscsoporthoz, amelyen futtatni kívánt engedélyekkel.

## <a name="programming-pattern"></a>Programozási minta

Minden Event Hubs erőforrás kezelheti a minta egy közös protokollt követi:

1. Aad-ben a jogkivonat beszerzése az `Microsoft.IdentityModel.Clients.ActiveDirectory` könyvtárban.
    ```csharp
    var context = new AuthenticationContext($"https://login.microsoftonline.com/{tenantId}");

    var result = await context.AcquireTokenAsync(
        "https://management.core.windows.net/",
        new ClientCredential(clientId, clientSecret)
    );
    ```

1. Hozzon létre a `EventHubManagementClient` objektum.
    ```csharp
    var creds = new TokenCredentials(token);
    var ehClient = new EventHubManagementClient(creds)
    {
        SubscriptionId = SettingsCache["SubscriptionId"]
    };
    ```

1. Állítsa be a `CreateOrUpdate` paramétereit, és a megadott értékeket.
    ```csharp
    var ehParams = new EventHubCreateOrUpdateParameters()
    {
        Location = SettingsCache["DataCenterLocation"]
    };
    ```

1. A hívás végrehajtása.
    ```csharp
    await ehClient.EventHubs.CreateOrUpdateAsync(resourceGroupName, namespaceName, EventHubName, ehParams);
    ```

## <a name="next-steps"></a>További lépések
* [.NET felügyeleti minta](https://github.com/Azure-Samples/event-hubs-dotnet-management/)
* [Microsoft.Azure.Management.EventHub hivatkozás](/dotnet/api/Microsoft.Azure.Management.EventHub) 
