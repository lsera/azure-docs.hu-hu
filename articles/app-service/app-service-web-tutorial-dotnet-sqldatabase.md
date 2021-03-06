---
title: "ASP.NET-alkalmazás létrehozása az Azure-ban SQL Database használatával | Microsoft Docs"
description: "Megismerheti, hogyan tehet szert egy olyan, az Azure-ban működő ASP.NET-alkalmazásra, amely csatlakozik egy SQL Database-adatbázishoz."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: 03c584f1-a93c-4e3d-ac1b-c82b50c75d3e
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: csharp
ms.topic: tutorial
ms.date: 06/09/2017
ms.author: cephalin
ms.custom: mvc, devcenter
ms.openlocfilehash: bd5aa5186bdec84e1943887ef0980fa50cd26324
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 02/01/2018
---
# <a name="build-an-aspnet-app-in-azure-with-sql-database"></a>ASP.NET-alkalmazás létrehozása az Azure-ban SQL Database használatával

Az [Azure Web Apps](app-service-web-overview.md) egy hatékonyan méretezhető, önjavító webes üzemeltetési szolgáltatás. Ebből az oktatóanyagból megtudhatja, hogyan helyezhet üzembe egy adatvezérelt ASP.NET-webalkalmazást az Azure-ban, és hogyan csatlakoztathatja az [Azure SQL Database-adatbázishoz](../sql-database/sql-database-technical-overview.md). Az oktatóanyag eredménye egy, az Azure-ban futó és az SQL Database-adatbázishoz csatlakozó ASP.NET-alkalmazás lesz.

![Közzétett ASP.NET-alkalmazás az Azure-webalkalmazásban](./media/app-service-web-tutorial-dotnet-sqldatabase/azure-app-in-browser.png)

Eben az oktatóanyagban az alábbiakkal fog megismerkedni:

> [!div class="checklist"]
> * SQL Database-adatbázis létrehozása az Azure-ban
> * ASP.NET-alkalmazás csatlakoztatása SQL Database-adatbázishoz
> * Az alkalmazás üzembe helyezése az Azure-ban
> * Az adatmodell frissítése és az alkalmazás ismételt üzembe helyezése
> * Naplók streamelése az Azure-ból a saját terminálba
> * Az alkalmazás kezelése az Azure Portalon

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Előfeltételek

Az oktatóanyag elvégzéséhez:

* Telepítse a [Visual Studio 2017](https://www.visualstudio.com/downloads/) szoftvert a következő számítási feladatokkal:
  - **ASP.NET és webfejlesztés**
  - **Azure-fejlesztés**

  ![ASP.NET és webfejlesztés és Azure-fejlesztés (Web és felhőszolgáltatások alatt)](media/app-service-web-tutorial-dotnet-sqldatabase/workloads.png)

## <a name="download-the-sample"></a>A minta letöltése

[Töltse le a mintaprojektet](https://github.com/Azure-Samples/dotnet-sqldb-tutorial/archive/master.zip).

Bontsa ki (csomagolja ki) a *dotnet-sqldb-tutorial-master.zip* fájlt.

A mintaprojekt az [Entity Framework Core](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application) szolgáltatást használó, alapszintű [ASP.NET MVC](https://www.asp.net/mvc) CRUD (létrehoz-olvas-frissít-töröl) alkalmazást tartalmaz.

### <a name="run-the-app"></a>Az alkalmazás futtatása

Nyissa meg a *dotnet-sqldb-tutorial-master/DotNetAppSqlDb.sln* fájlt a Visual Studióban. 

Az alkalmazás hibakeresés nélküli futtatásához nyomja le a `Ctrl+F5` billentyűkombinációt. Az alkalmazás megjelenik az alapértelmezett böngészőben. Kattintson az **Új létrehozása** hivatkozásra, és hozzon létre néhány *teendőt*. 

![A New ASP.NET Project (Új ASP.NET-projekt) párbeszédpanel](media/app-service-web-tutorial-dotnet-sqldatabase/local-app-in-browser.png)

Tesztelje az **Edit** (Szerkesztés), a **Details** (Részletek) és a **Delete** (Törlés) hivatkozást.

Az alkalmazás adatbázis-környezetet használ az adatbázishoz való kapcsolódáshoz. Ebben a példában az adatbázis-környezet a `MyDbConnection` nevű kapcsolati karakterláncot használja. A kapcsolati karakterlánc a *Web.config* fájlban adható meg, és a *Models/MyDatabaseContext.cs* fájl hivatkozik rá. A kapcsolati karakterlánc nevét az oktatóanyag későbbi részében is használni fogjuk az Azure-webalkalmazásnak egy Azure SQL Database-adatbázishoz való csatlakoztatásához. 

## <a name="publish-to-azure-with-sql-database"></a>Közzététel az Azure-ban az SQL Database használatával

A **Solution Explorer** (Megoldáskezelő) lapon kattintson a jobb gombbal a **DotNetAppSqlDb** projektre, és válassza a **Publish** (Közzététel) elemet.

![Közzététel a Megoldáskezelőből](./media/app-service-web-tutorial-dotnet-sqldatabase/solution-explorer-publish.png)

Győződjön meg arról, hogy a **Microsoft Azure App Service** van kiválasztva, és kattintson a **Publish** (Közzététel) elemre.

![Közzététel a projekt áttekintő oldaláról](./media/app-service-web-tutorial-dotnet-sqldatabase/publish-to-app-service.png)

A közzététel megnyitja a **Create App Service** (App Service létrehozása) párbeszédpanelt, amely segítségével létrehozhatja az ASP.NET-webalkalmazásnak az Azure-ban való futtatásához szükséges összes Azure-erőforrást.

### <a name="sign-in-to-azure"></a>Bejelentkezés az Azure-ba

A **Create App Service** (App Service létrehozása) párbeszédpanelen kattintson az **Add an account** (Fiók hozzáadása) gombra, majd jelentkezzen be az Azure-előfizetésébe. Ha már be van jelentkezve egy Microsoft-fiókba, győződjön meg arról, hogy abban a fiókban található az előfizetése. Ha nem abban a Microsoft-fiókban van az Azure-előfizetése, amelyikbe be van jelentkezve, kattintással adja hozzá a helyes fiókot.
   
![Bejelentkezés az Azure-ba](./media/app-service-web-tutorial-dotnet-sqldatabase/sign-in-azure.png)

Ha már bejelentkezett, ezen a panelen már létre is hozhatja az összes, az Azure-webapphoz szükséges erőforrást.

### <a name="configure-the-web-app-name"></a>A webalkalmazás nevének konfigurálása

Megtarthatja a webalkalmazás létrehozott nevét, vagy módosíthatja egy másik egyedi névre (érvényes karakterek: `a-z`, `0-9` és `-`). A webalkalmazás neve az alkalmazás alapértelmezett URL-címének a részeként használatos (`<app_name>.azurewebsites.net`, amelyben az `<app_name>` a webalkalmazás neve). A webalkalmazás nevének egyedinek kell lennie az Azure összes alkalmazása tekintetében. 

![A Create App Service (App Service létrehozása) párbeszédpanel](media/app-service-web-tutorial-dotnet-sqldatabase/wan.png)

> [!NOTE]
> Ne kattintson a **Create** (Létrehozás) gombra. Először egy SQL-adatbázist kell beállítania egy későbbi lépésben.

### <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot

[!INCLUDE [resource-group](../../includes/resource-group.md)]

A **Resource Group** (Erőforráscsoport) mellett kattintson a **New** (Új) elemre.

![A Resource Group (Erőforráscsoport) mellett kattintson a New (Új) elemre.](media/app-service-web-tutorial-dotnet-sqldatabase/new_rg2.png)

Adja a **myResourceGroup** nevet az erőforráscsoportnak.

### <a name="create-an-app-service-plan"></a>App Service-csomag létrehozása

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

Az **App Service Plan** (App Service-csomag) mellett kattintson a **New** (Új) elemre. 

A **Configure App Service Plan** (App Service-csomag konfigurálása) párbeszédpanelen konfigurálja az új App Service-csomagot a következő beállításokkal:

![App Service-csomag létrehozása](./media/app-service-web-tutorial-dotnet-sqldatabase/configure-app-service-plan.png)

| Beállítás  | Ajánlott érték | További tudnivalók |
| ----------------- | ------------ | ----|
|**App Service-csomag**| myAppServicePlan | [App Service-csomagok](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) |
|**Hely**| Nyugat-Európa | [Azure-régiók](https://azure.microsoft.com/regions/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) |
|**Méret**| Ingyenes | [Árképzési szintek](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)|

### <a name="create-a-sql-server-instance"></a>SQL Server-példány létrehozása

Adatbázis létrehozása előtt szüksége van egy [Azure SQL Database logikai kiszolgálóra](../sql-database/sql-database-features.md). A logikai kiszolgálók adatbázisok egy csoportját tartalmazzák, amelyeket a rendszer egy csoportként kezel.

Válassza az **Explore additional Azure services** (További Azure-szolgáltatások megismerése) elemet.

![A webapp nevének konfigurálása](media/app-service-web-tutorial-dotnet-sqldatabase/web-app-name.png)

A **Services** (Szolgáltatások) lapon kattintson az **SQL Database** melletti **+** ikonra. 

![A Services (Szolgáltatások) lapon kattintson az SQL Database melletti + ikonra.](media/app-service-web-tutorial-dotnet-sqldatabase/sql.png)

A **Configure SQL Database** (SQL Database konfigurálása) párbeszédpanelen kattintson az **SQL Server** melletti **New** (Új) elemre. 

A rendszer létrehoz egy egyedi kiszolgálónevet. Ez a név a `<server_name>.database.windows.net` logikai kiszolgáló alapértelmezett URL-címének a részeként használatos. Egyedinek kell lennie Azure összes logikaikiszolgáló-példányán. Módosíthatja a kiszolgáló nevét, ezen oktatóanyag esetén azonban tartsa meg a létrehozott értéket.

Adjon meg egy rendszergazdai felhasználónevet és jelszót. A jelszó „összetettségére” vonatkozó követelményeket a [jelszavakra vonatkozó szabályzattal](/sql/relational-databases/security/password-policy) foglalkozó cikkben tekintheti meg.

Jegyezze meg ezt a felhasználónevet és jelszót. Később szüksége lesz rájuk a logikaikiszolgáló-példány kezeléséhez.

![SQL Server-példány létrehozása](media/app-service-web-tutorial-dotnet-sqldatabase/configure-sql-database-server.png)

Kattintson az **OK** gombra. Ne zárja még be a **Configure SQL Database** (SQL Database konfigurálása) párbeszédpanelt.

### <a name="create-a-sql-database"></a>SQL-adatbázis létrehozása

A **Configure SQL Database** (SQL Database konfigurálása) párbeszédpanelen: 

* Tartsa meg a **Database Name** (Adatbázisnév) alapértelmezés szerint létrehozott értékét.
* A **Connection String Name** (Kapcsolati karakterlánc neve) mezőben írja be a *MyDbConnection* nevet. Ennek a névnek meg kell egyeznie a *Models/MyDatabaseContext.cs* fájlban hivatkozott kapcsolati karakterlánccal.
* Kattintson az **OK** gombra.

![SQL Database konfigurálása](media/app-service-web-tutorial-dotnet-sqldatabase/configure-sql-database.png)

A **Create App Service** (App Service létrehozása) párbeszédpanel jeleníti meg a létrehozott erőforrásokat. Kattintson a **Create** (Létrehozás) gombra. 

![a létrehozott erőforrások](media/app-service-web-tutorial-dotnet-sqldatabase/app_svc_plan_done.png)

Ha a varázsló befejezte az Azure-erőforrások létrehozását, közzéteszi az ASP.NET-alkalmazást az Azure-ban. Az alapértelmezett böngésző az üzembe helyezett alkalmazásra mutató URL-címmel indul el. 

Adjon hozzá néhány teendőt.

![Közzétett ASP.NET-alkalmazás az Azure-webalkalmazásban](./media/app-service-web-tutorial-dotnet-sqldatabase/azure-app-in-browser.png)

Gratulálunk! Az adatvezérelt ASP.NET-alkalmazás fut és elérhető az Azure App Service-ben.

## <a name="access-the-sql-database-locally"></a>Az SQL Database elérése helyben

A Visual Studio az **SQL Server Object Explorerben** teszi lehetővé az új SQL Database megismerését és egyszerű kezelését.

### <a name="create-a-database-connection"></a>Adatbázis-kapcsolat létrehozása

A **View** (Nézet) menüben válassza az **SQL Server Object Explorer** lehetőséget.

Az **SQL Server Object Explorer** felső részében kattintson az **Add SQL Server** (SQL Server hozzáadása) gombra.

### <a name="configure-the-database-connection"></a>Az adatbázis-kapcsolat konfigurálása

A **Connect** (Csatlakozás) párbeszédpanelen bontsa ki az **Azure** csomópontot. Az Azure-ban található összes SQL Database-példány látható itt.

Válassza ki a korábban létrehozott SQL Database-adatbázist. A korábban létrehozott kapcsolatot a rendszer automatikusan megadja a lap alján.

Írja be a korábban létrehozott adatbázis-rendszergazdai jelszót, és kattintson a **Connect** (Csatlakozás) elemre.

![Az adatbázis-kapcsolat konfigurálása a Visual Studióban](./media/app-service-web-tutorial-dotnet-sqldatabase/connect-to-sql-database.png)

### <a name="allow-client-connection-from-your-computer"></a>Ügyfélkapcsolat engedélyezése a számítógépen

Megnyílik a **Create a new firewall rule** (Új tűzfalszabály létrehozása) párbeszédpanel. Alapértelmezés szerint az SQL Database-példány csak Azure-szolgáltatásokból, például az Azure-webalkalmazásból származó kapcsolatokat engedélyez. Az adatbázishoz való kapcsolódáshoz hozzon létre egy tűzfalszabályt az SQL Database-példányban. A tűzfalszabály engedélyezi a helyi számítógép nyilvános IP-címét.

A párbeszédpanel már ki van töltve a számítógép nyilvános IP-címével.

Jelölje be az **Add my client IP** (Ügyfél IP-címének hozzáadása) választógombot, és kattintson az **OK** gombra. 

![Tűzfal beállítása az SQL Database-példány számára](./media/app-service-web-tutorial-dotnet-sqldatabase/sql-set-firewall.png)

Miután a Visual Studio befejezte a tűzfal beállítását az SQL Database-példány számára, a kapcsolat megjelenik az **SQL Server Object Explorerben**.

Itt elvégezheti a leggyakoribb adatbázis-műveleteket, például lekérdezések futtatását, nézetek és tárolt eljárások létrehozását és sok minden mást. 

Bontsa ki a kapcsolat > **Databases** (Adatbázisok)  > **&lt;saját adatbázis>** > **Tables** (Táblák) csomópontot. Kattintson a jobb gombbal a `Todoes` táblára, és válassza a **View Data** (Adatok megtekintése) lehetőséget. 

![SQL Database-objektumok megismerése](./media/app-service-web-tutorial-dotnet-sqldatabase/explore-sql-database.png)

## <a name="update-app-with-code-first-migrations"></a>Az alkalmazás frissítése Code First migrálásokkal

Az adatbázist és az Azure-ban található webes alkalmazást a Visual Studio jól ismert eszközeivel frissítheti. Ebben a lépésben Code First migrálásokat használ az Entity Frameworkben az adatbázisséma módosításához, majd annak közzétételéhez az Azure-ban.

Az Entity Framework Code First migrálásainak a használatával kapcsolatos további információkért tekintse meg a [Getting Started with Entity Framework 6 Code First using MVC 5](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application) (Ismerkedés az Entity Framework 6 Code First használatával az MVC 5 segítségével) című cikket.

### <a name="update-your-data-model"></a>Adatmodell frissítése

A kódszerkesztőben nyissa meg a _Models\Todo.cs_ fájlt. Adja hozzá a következő tulajdonságot a `ToDo` osztályhoz:

```csharp
public bool Done { get; set; }
```

### <a name="run-code-first-migrations-locally"></a>Code First migrálás helyi futtatása

Futtasson néhány parancsot a helyi adatbázis frissítéséhez. 

Kattintson a **Tools** (Eszközök) menü **NuGet Package Manager** > **Package Manager Console** (NuGet-csomagkezelő > Csomagkezelő konzol) elemére.

A Package Manager Console (Csomagkezelő konzol) ablakban engedélyezze a Code First migrálásokat:

```PowerShell
Enable-Migrations
```

Migrálás felvétele:

```PowerShell
Add-Migration AddProperty
```

Frissítse a helyi adatbázist:

```PowerShell
Update-Database
```

Az alkalmazás futtatásához nyomja le a `Ctrl+F5` billentyűkombinációt. Tesztelje a szerkesztést, a részletek megtekintését és a létrehozást lehetővé tevő hivatkozást.

Ha az alkalmazás betöltése hibák nélkül fejeződik be, a Code First migrálások sikeresek voltak. A lap azonban továbbra is ugyanúgy néz ki, mert az alkalmazáslogika még nem használja az új tulajdonságot. 

### <a name="use-the-new-property"></a>Az új tulajdonság használata

Hajtson végre néhány módosítást a kódban a `Done` tulajdonság használatához. Ebben az oktatóanyagban az egyszerűség kedvéért csak az `Index` és a `Create` nézetet módosítja, így láthatja a tulajdonságot működés közben.

Nyissa meg a _Controllers\TodosController.cs_ fájlt.

Keresse meg a `Create()` metódust az 52. sorban, és adja hozzá a `Done` kifejezést a `Bind` attribútum tulajdonságlistájához. Amikor végzett, a `Create()` metódus aláírása a következő kódhoz hasonló:

```csharp
public ActionResult Create([Bind(Include = "Description,CreatedDate,Done")] Todo todo)
```

Nyissa meg a _Views\Todos\Create.cshtml_ fájlt.

A Razor-kódban látnia kell a `<div class="form-group">` elemet, amely a `model.Description` elemet használja, majd egy másik `<div class="form-group">` elemet, amely a `model.CreatedDate` elemet használja. Közvetlenül ezután a két elem után adjon hozzá egy másik `<div class="form-group">` elemet, amely a `model.Done` elemet használja:

```csharp
<div class="form-group">
    @Html.LabelFor(model => model.Done, htmlAttributes: new { @class = "control-label col-md-2" })
    <div class="col-md-10">
        <div class="checkbox">
            @Html.EditorFor(model => model.Done)
            @Html.ValidationMessageFor(model => model.Done, "", new { @class = "text-danger" })
        </div>
    </div>
</div>
```

Nyissa meg a _Views\Todos\Index.cshtml_ fájlt.

Keresse meg az üres `<th></th>` elemet. Az elem felett adja hozzá a következő Razor-kódot:

```csharp
<th>
    @Html.DisplayNameFor(model => model.Done)
</th>
```

Keresse meg a `Html.ActionLink()` segédmetódusokat tartalmazó `<td>` elemet. Ezen `<td>` _felett_ adjon hozzá egy másik `<td>` elemet a következő Razor-kóddal:

```csharp
<td>
    @Html.DisplayFor(modelItem => item.Done)
</td>
```

Mást nem kell tennie a módosítások megtekintéséhez az `Index` és a `Create` nézetben. 

Az alkalmazás futtatásához nyomja le a `Ctrl+F5` billentyűkombinációt.

Most hozzáadhat teendőket, és bejelölheti a **Kész** jelölőnégyzetet. Ezután ennek meg kell jelennie a főoldalon befejezett elemként. Ne feledje, hogy az `Edit` nézetben nem jelenik meg a `Done` mező, mivel az `Edit` nézetet nem módosította.

### <a name="enable-code-first-migrations-in-azure"></a>Code First migrálások engedélyezése az Azure-ban

Most, hogy a kódmódosítás működik, az adatbázis-migrálást is beleértve, közzéteheti az Azure-webalkalmazásban, és frissítheti az SQL Database-adatbázist is a Code First migrálásokkal.

Fentiekhez hasonlóan kattintson jobb gombbal a projektre, majd válassza a **Publish** (Közzététel) lehetőséget.

Kattintson a **Settings** (Beállítások) elemre a közzétételi varázsló megnyitásához.

![Közzétételi beállítások megnyitása](./media/app-service-web-tutorial-dotnet-sqldatabase/publish-settings.png)

A varázslóban kattintson a **Next** (Tovább) gombra.

Ellenőrizze, hogy az SQL Database-adatbázis kapcsolati karakterlánca látható a **MyDatabaseContext (MyDbConnection)** alatt. Előfordulhat, hogy kell kiválasztania a **myToDoAppDb** adatbázist a legördülő listából. 

Válassza az **Execute Code First Migrations (runs on application start)** (Code First migrálások végrehajtása (alkalmazásindításakor fut)) elemet, majd kattintson a **Save** (Mentés) gombra.

![Code First migrálások engedélyezése az Azure-webalkalmazásban](./media/app-service-web-tutorial-dotnet-sqldatabase/enable-migrations.png)

### <a name="publish-your-changes"></a>A módosítások közzététele

Most, hogy engedélyezte a Code First migrálásokat az Azure-webalkalmazásban, tegye közzé a kódmódosításokat.

A közzétételi oldalon kattintson a **Publish** (Közzététel) elemre.

Próbálkozzon újabb teendő elemek hozzáadásával, kattintson a **Done** (Kész) gombra, ami után az elemeknek befejezett elemekként kell megjelenniük a kezdőlapon.

![Az Azure webalkalmazás a Code First migrálás után](./media/app-service-web-tutorial-dotnet-sqldatabase/this-one-is-done.png)

A meglévő teendők továbbra is megjelennek. Ha ismét közzéteszi az ASP.NET-alkalmazást, az SQL Database-ben meglévő adatok nem vesznek el. A Code First migrálások emellett csak az adatsémát módosítják, a meglévő adatokat érintetlenül hagyják.


## <a name="stream-application-logs"></a>Alkalmazásnaplók streamelése

A nyomkövetési üzeneteket közvetlenül az Azure-webalkalmazásból streamelheti a Visual Studióba.

Nyissa meg a _Controllers\TodosController.cs_ fájlt.

Minden művelet egy `Trace.WriteLine()` metódussal indul. Ez a kód azért lett hozzáadva, hogy megmutassa hogyan vehet fel nyomkövetési üzeneteket az Azure-webalkalmazásba.

### <a name="open-server-explorer"></a>A Server Explorer megnyitása

A **View** (Nézet) menüben válassza a **Server Explorer** (Kiszolgálókezelő) lehetőséget. A **Server Explorer** (Kiszolgálókezelő) segítségével konfigurálhatja az Azure-webalkalmazás bejelentkezési beállításait. 

### <a name="enable-log-streaming"></a>Naplóstreamelés engedélyezése

A **Server Explorer** (Kiszolgálókezelő) ablakában bontsa ki az **Azure** > **App Service** csomópontot.

Bontsa ki az Azure-webalkalmazás első létrehozásakor létrehozott **myResourceGroup** erőforráscsoportot.

Kattintson a jobb gombbal az Azure-webalkalmazásra, és válassza a **View Streaming Logs** (Streamelt naplók megtekintése) lehetőséget.

![Naplóstreamelés engedélyezése](./media/app-service-web-tutorial-dotnet-sqldatabase/stream-logs.png)

A naplókat mostantól az **Output** (Kimenet) ablakba streameli a rendszer. 

![Az Output (Kimenet) ablakban streamelt napló](./media/app-service-web-tutorial-dotnet-sqldatabase/log-streaming-pane.png)

Nem jelennek még azonban meg a nyomkövetési üzenetek. Ennek oka az, hogy amikor először választja a **View Streaming Logs** (Streamelt naplók megtekintése) lehetőséget, az Azure-webalkalmazás `Error` értékűre állítja be a nyomkövetési szintet, amely csak a hibaeseményeket rögzíti (a `Trace.TraceError()` metódussal).

### <a name="change-trace-levels"></a>Nyomkövetési szintek módosítása

Ha módosítani szeretné a nyomkövetési szinteket a többi nyomkövetési üzenet megjelenítéséhez, lépjen vissza a **Server Explorer** (Kiszolgálókezelő) ablakába.

Kattintson újra a jobb gombbal az Azure-webalkalmazásra, és válassza a **View Settings** (Megjelenítési beállítások) lehetőséget.

Az **Application Logging (File System)** (Alkalmazásnaplózás (fájlrendszer)) legördülő menüben válassza a **Verbose** (Részletes) elemet. Kattintson a **Save** (Mentés) gombra.

![Nyomkövetési szint módosítása Verbose (Részletes) értékűre](./media/app-service-web-tutorial-dotnet-sqldatabase/trace-level-verbose.png)

> [!TIP]
> Kísérletezhet a különböző szintű nyomkövetésekkel, hogy lássa, milyen típusú üzenetek jelennek meg az egyes szintek esetén. Az **Information** (Tájékoztatás) szint a `Trace.TraceInformation()`, `Trace.TraceWarning()` és `Trace.TraceError()` által létrehozott naplók mindegyikét tartalmazza, a `Trace.WriteLine()` által létrehozott naplókat azonban nem.
>
>

A böngészőben nyissa meg újra a webalkalmazását a *http://&lt;az alkalmazás neve>.azurewebsites.net* címen, majd próbáljon az Azure-ban a teendőlista alkalmazás körül kattintani. A nyomkövetési üzeneteket mostantól a Visual Studio **Output** (Kimenet) ablakába streameli a rendszer.

```console
Application: 2017-04-06T23:30:41  PID[8132] Verbose     GET /Todos/Index
Application: 2017-04-06T23:30:43  PID[8132] Verbose     GET /Todos/Create
Application: 2017-04-06T23:30:53  PID[8132] Verbose     POST /Todos/Create
Application: 2017-04-06T23:30:54  PID[8132] Verbose     GET /Todos/Index
```



### <a name="stop-log-streaming"></a>Naplóstreamelés leállítása

A naplóstreamelési szolgáltatás leállításához kattintson az **Output** (Kimenet) ablak **Stop monitoring** (Megfigyelés leállítása) gombjára.

![Naplóstreamelés leállítása](./media/app-service-web-tutorial-dotnet-sqldatabase/stop-streaming.png)

## <a name="manage-your-azure-web-app"></a>Az Azure-webalkalmazás kezelése

Lépjen az [Azure Portalra](https://portal.azure.com), és tekintse meg a létrehozott webalkalmazást. 



A bal oldali menüben kattintson az **App Service** lehetőségre, majd az Azure-webapp nevére.

![Navigálás a portálon az Azure-webapphoz](./media/app-service-web-tutorial-dotnet-sqldatabase/access-portal.png)

A webalkalmazás lapjára jut. 

Alapértelmezés szerint a portál megjeleníti az **Áttekintés** oldalt. Ezen az oldalon megtekintheti az alkalmazás állapotát. Itt elvégezhet olyan alapszintű felügyeleti feladatokat is, mint a böngészés, leállítás, elindítás, újraindítás és törlés. Az oldal bal oldalán lévő lapok a különböző megnyitható konfigurációs oldalakat jelenítik meg. 

![Az App Service lap az Azure Portalon](./media/app-service-web-tutorial-dotnet-sqldatabase/web-app-blade.png)

[!INCLUDE [Clean up section](../../includes/clean-up-section-portal-web-app.md)]

<a name="next"></a>

## <a name="next-steps"></a>További lépések

Ez az oktatóanyag bemutatta, hogyan végezheti el az alábbi műveleteket:

> [!div class="checklist"]
> * SQL Database-adatbázis létrehozása az Azure-ban
> * ASP.NET-alkalmazás csatlakoztatása SQL Database-adatbázishoz
> * Az alkalmazás üzembe helyezése az Azure-ban
> * Az adatmodell frissítése és az alkalmazás ismételt üzembe helyezése
> * Naplók streamelése az Azure-ból a saját terminálba
> * Az alkalmazás kezelése az Azure Portalon

Lépjen a következő oktatóanyaghoz, amelyből megtudhatja, hogyan képezhet le egyedi DNS-nevet a webalkalmazáshoz.

> [!div class="nextstepaction"]
> [Meglévő egyéni DNS-név hozzákapcsolása az Azure-webalkalmazásokhoz](app-service-web-tutorial-custom-domain.md)
