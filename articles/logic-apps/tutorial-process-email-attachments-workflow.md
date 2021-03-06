---
title: "Munkafolyamatok kialakítása e-mailek és mellékletek feldolgozására – Azure Logic Apps | Microsoft Docs"
description: "Ez az oktatóanyag bemutatja, hogyan hozhat létre automatizált munkafolyamatokat e-mailek és mellékletek feldolgozására az Azure Logic Apps, az Azure Storage és az Azure Functions segítségével"
author: ecfan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 01/12/2018
ms.author: LADocs; estfan
ms.openlocfilehash: 8c327599585e67ccc6ebdf849d3e9cf9b95e7398
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 02/27/2018
---
# <a name="process-emails-and-attachments-with-a-logic-app"></a>E-mailek és mellékletek kezelése logikai alkalmazásokkal

Az Azure Logic Apps segítségével automatizálhatja a munkafolyamatait, és integrálhatja az adatokat különböző Azure- és Microsoft-szolgáltatások, más szolgáltatott szoftveralkalmazások (SaaS), valamint a helyszíni rendszerek között. Ez az oktatóanyag bemutatja, hogyan hozhat létre bejövő e-maileket és mellékleteket kezelő [logikai alkalmazásokat](../logic-apps/logic-apps-overview.md). A logikai alkalmazás feldolgozza a tartalmakat és az Azure-tárterületre menti azokat, és értesítést küld a felhasználóknak a tartalmak áttekintésével kapcsolatban. 

Eben az oktatóanyagban az alábbiakkal fog megismerkedni:

> [!div class="checklist"]
> * Az [Azure storage](../storage/common/storage-introduction.md) és a Storage Explorer konfigurálása a mentett e-mailek és mellékletek ellenőrzésére.
> * [Azure-függvény](../azure-functions/functions-overview.md) létrehozása az e-mailekben található HTML-formázás eltávolítására. Ebben az oktatóanyagban megtalálja az ehhez a függvényhez használható kódot.
> * Üres logikai alkalmazás létrehozása.
> * Eseményindító hozzáadása az e-mailek mellékleteinek figyelésére.
> * Feltétel hozzáadása annak ellenőrzéséhez, hogy az e-maileknek van-e melléklete.
> * Művelet hozzáadása az Azure-függvény meghívásához, ha egy e-mail melléklettel rendelkezik.
> * Művelet hozzáadása tárolóblobok létrehozására az e-mailek és a mellékletek számára.
> * Művelet hozzáadása e-mail-értesítések küldésére.

Az elkészült logikai alkalmazás nagyjából a következő munkafolyamathoz hasonlít:

![Magas szintű befejezett logikai alkalmazás](./media/tutorial-process-email-attachments-workflow/overview.png)

Ha nem rendelkezik Azure-előfizetéssel, <a href="https://azure.microsoft.com/free/" target="_blank">regisztrálhat egy ingyenes Azure-fiókra</a> az eljárás megkezdése előtt. 

## <a name="prerequisites"></a>Előfeltételek

* A Logic Apps által támogatott e-mail-szolgáltató (például Office 365 Outlook, Outlook.com vagy Gmail) által üzemeltetett e-mail-fiók. Más szolgáltatók esetén [tekintse át az itt felsorolt összekötőket](https://docs.microsoft.com/connectors/).

  Ez a logikai alkalmazás Office 365 Outlook-fiókot használ. 
  Ha más e-mail-fiókot használ, az általános lépések ugyanazok, a felhasználói felület azonban némiképp eltérhet.

* Az <a href="http://storageexplorer.com/" target="_blank">ingyenes Microsoft Azure Storage Explorer</a> letöltése és telepítése. Az eszköz segítségével ellenőrizheti, hogy a Storage-tároló megfelelően van-e beállítva.

## <a name="sign-in-to-the-azure-portal"></a>Jelentkezzen be az Azure Portalra

Jelentkezzen be az <a href="https://portal.azure.com" target="_blank">Azure Portalra</a> az Azure-fiókja hitelesítő adataival.

## <a name="set-up-storage-to-save-attachments"></a>Tároló beállítása a mellékletek mentésére

A bejövő e-mailek és mellékletek blobként menthetőek egy [Azure Storage-tárolóba](../storage/common/storage-introduction.md). 

1. A Storage-tároló létrehozása előtt [hozzon létre egy tárfiókot](../storage/common/storage-create-storage-account.md#create-a-storage-account) az alábbi beállításokkal:

   | Beállítás | Érték | Leírás | 
   | ------- | ----- | ----------- | 
   | **Name (Név)** | attachmentstorageacct | A tárfiók neve | 
   | **Üzemi modell** | Resource Manager | Az [üzemi modell](../azure-resource-manager/resource-manager-deployment-model.md) az erőforrások üzembe helyezésének felügyeletéhez | 
   | **Fióktípus** | Általános célú | A [tárfiók típusa](../storage/common/storage-introduction.md#types-of-storage-accounts) | 
   | **Teljesítmény** | Standard | Ez a beállítás adja meg a támogatott adattípusokat és az adathordozót az adatok tárolásához. Lásd: [A tárfiókok típusai](../storage/common/storage-introduction.md#types-of-storage-accounts). | 
   | **Replikáció** | Helyileg redundáns tárolás (LRS) | Ez a beállítás határozza meg az adatok másolásának, tárolásának, felügyeletének és szinkronizálásának módját. Lásd: [Replikáció](../storage/common/storage-introduction.md#replication). | 
   | **Biztonságos átvitelre van szükség** | Letiltva | Ez a beállítás adja meg a kapcsolatokról érkező kérésekre vonatkozó biztonsági követelményeket. Lásd: [Biztonságos átvitel megkövetelése](../storage/common/storage-require-secure-transfer.md). | 
   | **Előfizetés** | <*your-Azure-subscription-name*> | Az Azure-előfizetés neve | 
   | **Erőforráscsoport** | LA-Tutorial-RG | A kapcsolódó erőforrások rendezéséhez és felügyeletéhez használt [Azure-erőforráscsoport](../azure-resource-manager/resource-group-overview.md) neve. <p>**Megjegyzés:** Az erőforráscsoportok adott régiókon belül léteznek. Bár az ebben az oktatóanyagban bemutatott elemek nem feltétlenül érhetőek el minden régióban, igyekezzen ugyanazt a régiót használni, amikor csak lehetséges. | 
   | **Hely** | USA 2. keleti régiója | A tárfiókkal kapcsolatos információk tárolására szolgáló régió | 
   | **Virtuális hálózatok konfigurálása** | Letiltva | Ebben az oktatóanyagban tartsa meg a **Letiltva** beállítást. | 
   |||| 

   A feladathoz az [Azure PowerShell](../storage/common/storage-quickstart-create-storage-account-powershell.md) vagy az [Azure CLI](../storage/common/storage-quickstart-create-storage-account-cli.md) is használható.
  
2. Miután az Azure telepíti a tárfiókot, kérje le annak hozzáférési kulcsát:

   1. A tárfiók menüjében a **Beállítások** alatt válassza a **Hozzáférési kulcsok** lehetőséget. 
   2. Keresse meg a **key1** elemet az **Alapértelmezett kulcsok** alatt, továbbá a tárfiók nevét.

      ![A tárfiók nevének és kulcsának másolása és mentése](./media/tutorial-process-email-attachments-workflow/copy-save-storage-name-key.png)

   A feladathoz az [Azure PowerShell](https://docs.microsoft.com/powershell/module/azurerm.storage/get-azurermstorageaccountkey) vagy az [Azure CLI](https://docs.microsoft.com/cli/azure/storage/account/keys?view=azure-cli-latest.md#az_storage_account_keys_list) is használható. 

3. Hozzon létre egy Storage-tárolót az e-mail-mellékletek számára.
   
   1. A tárfiók menüjében az **Áttekintés** panelen válassza a **Szolgáltatások** alatt a **Blobok** lehetőséget, majd kattintson a **+ Tároló** gombra.

   2. Írja be a „Mellékletek” nevet a tárolónak. A **Nyilvános hozzáférés szintje** alatt válassza a **Tároló (Névtelen olvasási hozzáférés tárolók és blobok esetén)** lehetőséget, majd kattintson az **OK** gombra.

   A feladathoz az [Azure PowerShell](https://docs.microsoft.com/powershell/module/azure.storage/new-azurestoragecontainer) vagy az [Azure CLI](https://docs.microsoft.com/cli/azure/storage/container?view=azure-cli-latest#az_storage_container_create) is használható. 
   Amikor elkészült, a Storage-tárolót a tárfiókjában találja itt, az Azure Portalon:

   ![Befejezett Storage-tároló](./media/tutorial-process-email-attachments-workflow/created-storage-container.png)

Ezután csatlakoztassa a Storage Explorert a tárfiókhoz.

## <a name="set-up-storage-explorer"></a>A Storage Explorer beállítása

Most csatlakoztassa a Storage Explorert a tárfiókjához, így ellenőrizheti, hogy a logikai alkalmazás megfelelően menti-e blobokként a mellékleteket a Storage-tárolóba.

1. Nyissa meg a Microsoft Azure Storage Explorert. Ha a Storage Explorer kéri az Azure-tárolóhoz való csatlakozásra, válassza a **Use a storage account name and key** > **Next** (Tárfiók nevének és kulcsának használata > Tovább) lehetőséget.
Ha a kérés nem jelenik meg, válassza az **Add account** (Fiók hozzáadása) lehetőséget az Explorer eszköztárán.

2. Az **Attach using Name and Key** (Csatolás a név és a kulcs használatával) területen adja meg a tárfiók nevét és a hozzáférési kulcsot, amelyeket korábban mentett. Válassza a **Next** > **Connect** (Tovább > Csatlakozás) lehetőséget.

3. Ellenőrizze a Storage Explorerben, hogy a tárfiók és a tároló megfelelően jelennek-e meg:

   1. Az **Explorer** alatt bontsa ki a **(Local and Attached)** (Helyi és csatolt) > 
   **Storage Accounts** (Tárfiókok) > **attachmentstorageaccount** > 
   **Blob-tárolók** elemet.

   2. Győződjön meg róla, hogy az „attachments” tároló most megjelent. 
   Például:

      ![Storage Explorer – Storage-tároló ellenőrzése](./media/tutorial-process-email-attachments-workflow/storage-explorer-check-contianer.png)

Ezután hozzon létre egy [Azure-függvényt](../azure-functions/functions-overview.md) a bejövő e-mailekben lévő a HTML-formázás eltávolításához.

## <a name="create-a-function-to-clean-html"></a>Függvény létrehozása a HTML-formázás törlésére

Most az ezekben a lépésekben megadott kódrészlet használatával hozzon létre egy Azure-függvényt az egyes bejövő e-mailekben található HTML-formázás eltávolítására. Így az e-mailek tartalma tisztább és könnyebben feldolgozható lesz. Ez a függvény azután a logikai alkalmazásból hívható majd meg.

1. A függvény létrehozása előtt [hozzon létre egy függvényalkalmazást](../azure-functions/functions-create-function-app-portal.md) az alábbi beállításokkal:

   | Beállítás | Érték | Leírás | 
   | ------- | ----- | ----------- | 
   | **Alkalmazás neve** | CleanTextFunctionApp | A függvényalkalmazás globálisan egyedi leíró neve | 
   | **Előfizetés** | <*your-Azure-subscription-name*> | A korábban is használt Azure-előfizetés | 
   | **Erőforráscsoport** | LA-Tutorial-RG | A korábban is használt Azure-erőforráscsoport | 
   | **Szolgáltatási csomag** | Használatalapú csomag | Ez a beállítás határozza meg az erőforrások, például a számítási teljesítmény lefoglalásának és méretezésének módját a függvényalkalmazás futtatásához. Lásd a [szolgáltatási csomagok összehasonlítását](../azure-functions/functions-scale.md). | 
   | **Hely** | USA 2. keleti régiója | A korábban is használt régió | 
   | **Storage** | cleantextfunctionstorageacct | Hozzon létre egy tárfiókot a függvényalkalmazás számára. Csak kisbetűket és számokat használjon. <p>**Megjegyzés:** Ez a tárfiók a függvényalkalmazást tartalmazza, és nem egyezik a korábban az e-mail-mellékletek számára létrehozott tárfiókkal. | 
   | **Application Insights** | Ki | Bekapcsolja az [Application Insights](../application-insights/app-insights-overview.md) alkalmazásmonitorozását, de ehhez az oktatóanyaghoz hagyja **kikapcsolva** a beállítást. | 
   |||| 

   Ha a függvényalkalmazás az üzembe helyezést követően nem nyílik meg automatikusan, az <a href="https://portal.azure.com" target="_blank">Azure Portalon</a> találja meg. Az Azure főmenüjéből válassza az **App Services** lehetőséget, majd azon belül magát a függvényalkalmazást.

   ![Létrehozott függvényalkalmazás](./media/tutorial-process-email-attachments-workflow/function-app-created.png)

   Ha az **App Services** nem jelenik meg az Azure menüjében, lépjen inkább a **További szolgáltatások** menüre. A keresőmezőben keressen rá a **Függvényalkalmazások** szóra. További információ: [A függvény létrehozása](../azure-functions/functions-create-first-azure-function.md).

   A feladathoz az [Azure CLI](../azure-functions/functions-create-first-azure-function-azure-cli.md) vagy [PowerShell- és Resource Manager-sablonok](../azure-resource-manager/resource-group-template-deploy.md) is használhatóak.

2. A **Függvényalkalmazások** alatt bontsa ki a **CleanTextFunctionApp** csoportot, és válassza a **Függvények** lehetőséget. A függvények eszköztárán válassza **+ Új függvény** lehetőséget.

   ![Új függvény létrehozása](./media/tutorial-process-email-attachments-workflow/function-app-new-function.png)

3. A **Válasszon ki egy sablont az alábbiak közül, vagy lépjen a gyors üzembe helyezéshez** területen válassza ki a **HttpTrigger - C#** függvénysablont.

   ![Függvénysablon kiválasztása](./media/tutorial-process-email-attachments-workflow/function-select-httptrigger-csharp-function-template.png)

4. Az **Adjon nevet a függvénynek** alatt adja meg a következőt: ```RemoveHTMLFunction```. A **HTTP-trigger** > **Engedélyszint** alatt tartsa meg az alapértelmezett **Függvény** értéket, és kattintson a **Létrehozás** gombra.

   ![A függvény neve](./media/tutorial-process-email-attachments-workflow/function-provide-name.png)

5. Miután megnyílik a szerkesztő, a sablonban lévő kód helyére illessze be ezt a kódot, amely eltávolítja a HTML-formázást és visszaadja az eredményeket a hívónak:

   ``` CSharp
   using System.Net;
   using System.Text.RegularExpressions;

   public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
   {
      log.Info($"HttpWebhook triggered");

      // Parse query parameter
      string emailBodyContent = await req.Content.ReadAsStringAsync();

      // Replace HTML with other characters
      string updatedBody = Regex.Replace(emailBodyContent, "<.*?>", string.Empty);
      updatedBody = updatedBody.Replace("\\r\\n", " ");
      updatedBody = updatedBody.Replace(@"&nbsp;", " ");

      // Return cleaned text
      return req.CreateResponse(HttpStatusCode.OK, new { updatedBody });

   }
   ```

6. Ha elkészült, kattintson a **Mentés** gombra. A függvény teszteléséhez válassza **Tesztelés** lehetőséget a szerkesztő jobb szélén lévő nyíl (**<**) ikon alatt. 

   ![A „Tesztelés” panel bezárása](./media/tutorial-process-email-attachments-workflow/function-choose-test.png)

7. A **Tesztelés** panelen a **Kérelem törzse** alatt írja be ezt a sort, majd kattintson a **Futtatás** gombra.

   ```json
   {"name": "<p><p>Testing my function</br></p></p>"}
   ```

   ![A függvény tesztelése](./media/tutorial-process-email-attachments-workflow/function-run-test.png)

   A **Kimenet** ablakban látható a függvény alábbi eredménye:

   ```json
   {"updatedBody":"{\"name\": \"Testing my function\"}"}
   ```

Miután ellenőrizte, hogy működik-e a függvény, készítse el a logikai alkalmazást. Bár ez az oktatóanyag bemutatja, hogyan hozhat létre olyan függvényt, amely eltávolítja az e-mailek HTML-formázását, a Logic Apps biztosít egy **HTML–szöveg** összekötőt is.

## <a name="create-your-logic-app"></a>A logikai alkalmazás létrehozása

1. Az Azure főmenüjében válassza az **Erőforrás létrehozása** > **Enterprise Integration** > **Logic App** elemet.

   ![Logikai alkalmazás létrehozása](./media/tutorial-process-email-attachments-workflow/create-logic-app.png)

2. A **Logikai alkalmazás létrehozása** területen adja meg a logikai alkalmazás alábbi adatait az itt látható módon. Ha elkészült, válassza a **Rögzítés az irányítópulton** > **Létrehozás** lehetőséget.

   ![Logikai alkalmazás adatainak megadása](./media/tutorial-process-email-attachments-workflow/create-logic-app-settings.png)

   | Beállítás | Érték | Leírás | 
   | ------- | ----- | ----------- | 
   | **Name (Név)** | LA-ProcessAttachment | A logikai alkalmazás neve | 
   | **Előfizetés** | <*your-Azure-subscription-name*> | A korábban is használt Azure-előfizetés | 
   | **Erőforráscsoport** | LA-Tutorial-RG | A korábban is használt Azure-erőforráscsoport |
   | **Hely** | USA 2. keleti régiója | A korábban is használt régió | 
   | **Log Analytics** | Ki | Ebben az oktatóanyagban tartsa meg a **Ki** beállítást. | 
   |||| 

3. Miután az Azure üzembe helyezte az alkalmazást, megnyílik a Logic Apps Designer, és egy bemutató videót és a gyakori logikaialkalmazás-minták sablonjait tartalmazó oldalt jelenít meg. A **Sablonok** területen válassza az **Üres logikai alkalmazás** elemet.

   ![Üres logikaialkalmazás-sablon kiválasztása](./media/tutorial-process-email-attachments-workflow/choose-logic-app-template.png)

Ezután adjon hozzá egy [eseményindítót](../logic-apps/logic-apps-overview.md#logic-app-concepts), amely a melléklettel rendelkező beérkező e-maileket figyeli. Minden logikai alkalmazást egy eseményindítónak kell indítania, amely akkor aktiválódik, ha egy adott esemény bekövetkezik, vagy ha az új adatok teljesítenek egy adott feltételt. További információkért lásd: [Az első logikai alkalmazás létrehozása](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="monitor-incoming-email"></a>A bejövő e-mailek monitorozása

1. A tervezőben írja be az „e-mail érkezésekor” kifejezést a keresőmezőbe. Válassza ki az alábbi eseményindítót az e-mail-szolgáltatónál: **<*az-e-mail-szolgáltatója*> - Új e-mail érkezésekor**, például:

   ![Válassza ezt az eseményindítót az e-mail-szolgáltatónál: „Új e-mail érkezésekor”](./media/tutorial-process-email-attachments-workflow/add-trigger-when-email-arrives.png)

   * Munkahelyi vagy iskolai Azure-fiókok esetében válassza az Office 365 Outlookot. 
   * Személyes Microsoft-fiókok esetében válassza az Outlook.com-összekötőt. 

2. Ha a rendszer kéri a hitelesítő adatokat, jelentkezzen be az e-mail-fiókjába, hogy a Logic Apps kapcsolatot létesíthessen vele.

3. Most adja meg az eseményindító által az új e-mailek szűréséhez alkalmazott feltételeket.

   1. Adja meg a mappát, az időtartamot és a gyakoriságot az e-mailek ellenőrzéséhez.

      ![A mappa, az időtartam és a gyakoriság megadása az e-mailek ellenőrzéséhez](./media/tutorial-process-email-attachments-workflow/set-up-email-trigger.png)

      | Beállítás | Érték | Leírás | 
      | ------- | ----- | ----------- | 
      | **Mappa** | Beérkezett üzenetek | Az ellenőrizni kívánt e-mail-mappa | 
      | **Intervallum** | 1 | Az ellenőrzések között kivárt intervallumok száma | 
      | **Gyakoriság** | Perc | Az ellenőrzések közötti intervallumok időegysége | 
      |  |  |  | 
  
   2. Kattintson a **Speciális beállítások megjelenítése** elemre, és adja meg az alábbi beállításokat:

      | Beállítás | Érték | Leírás | 
      | ------- | ----- | ----------- | 
      | **Melléklettel rendelkezik** | Igen | Csak a melléklettel rendelkező e-mailek beolvasása. <p>**Megjegyzés:** Az eseményindító nem törli az e-maileket a fiókból, csak ellenőrzi az új üzeneteket, és feldolgozza azokat, amelyek megfelelnek a tárgyszűrőnek. | 
      | **Mellékletek is** | Igen | A mellékletek egyszerű ellenőrzése helyett azok lekérése bemenetként a munkafolyamathoz. | 
      | **Tárgyszűrő** | ```Business Analyst 2 #423501``` | Az e-mail tárgyában keresendő szöveg | 
      |  |  |  | 

4. Ha egyelőre el szeretné rejteni az eseményindító részleteit, kattintson az eseményindító címsorába.

   ![Alakzat összecsukása a részletek elrejtéséhez](./media/tutorial-process-email-attachments-workflow/collapse-trigger-shape.png)

5. Mentse a logikai alkalmazást. A tervező eszköztárán válassza a **Mentés** parancsot.

   A logikai alkalmazás most már működőképes, de mindössze annyit csinál, hogy ellenőrzi az e-maileket. 
   Ezután adja hozzá egy feltételt, amely meghatározza a munkafolyamat folytatására vonatkozó kritériumokat.

## <a name="check-for-attachments"></a>Mellékletek ellenőrzése

1. Az eseményindító alatt válassza az **+ Új lépés** > **Feltétel hozzáadása** elemet.

   Amikor megjelenik a feltétel alakzata, alapértelmezés szerint a paraméterek vagy a dinamikus tartalmak listája jelenik meg, és tartalmazza az előző lépésből átvett azon paramétereket, amelyeket bemenetként a munkafolyamatba foglalhat. 
   A böngésző szélessége határozza meg, hogy melyik lista fog megjelenni.

2. Adjon egy leíróbb nevet a feltételnek.

   1. A feltétel címsorán válassza a **három pont** (**...** ) > **Átnevezés** lehetőséget.

      Például, ha a böngésző keskeny nézetben jelenik meg:

      ![Feltétel átnevezése](./media/tutorial-process-email-attachments-workflow/condition-rename.png)

      Ha a böngésző széles nézetben jelenik meg, és a dinamikus tartalmak listája miatt a három pont (...) nem látható, zárja be a listát a **Dinamikus tartalom hozzáadása** gombra kattintva a feltételen belül. 
      
      ![A Dinamikus tartalom lista bezárása](./media/tutorial-process-email-attachments-workflow/close-dynamic-content-list.png)

   2. Nevezze át a feltételt a következő leírással: ```If email has attachments and key subject phrase```

3. A feltétel leírása kifejezés megadásával. 

   1. A feltétel alakzaton belül válassza a **Szerkesztés speciális módban** lehetőséget.

      ![Feltétel szerkesztése speciális módban](./media/tutorial-process-email-attachments-workflow/edit-advanced-mode.png)

   2. A szövegmezőbe írja be a következő kifejezést:

      ```@equals(triggerBody()?['HasAttachment'], bool('true'))```

      A kifejezés az eseményindító törzsében (az oktatóanyagban ez az e-mail) lévő **HasAttachment** tulajdonság értékét hasonlítja össze a ```True``` logikai objektummal. 
      Ha a két érték egyezik, az e-mail legalább egy melléklettel rendelkezik, a feltétel teljesül, és a munkafolyamat folytatódik.

      A feltétel most a következő példához hasonlít:

      ![Feltételkifejezés](./media/tutorial-process-email-attachments-workflow/condition-expression.png)

   3. Válassza a **Szerkesztés egyszerű módban** lehetőséget. A kifejezés eredménye most a következő:

      ![Kifejezés eredménye](./media/tutorial-process-email-attachments-workflow/condition-expression-resolved.png)

      > [!NOTE]
      > A kifejezések manuális létrehozásához egyszerű módban kell dolgoznia, és a dinamikus listának meg kell lennie nyitva, hogy használhassa a kifejezésszerkesztőt. A **Kifejezés** mezőben választhatja ki a függvényeket. A **Dinamikus tartalom** mezőben választhatja ki az ezekhez a függvényekhez használni kívánt paramétermezőket.
      > Az oktatóanyag később bemutatja a kifejezések manuális létrehozását.

4. Mentse a logikai alkalmazást.

### <a name="test-your-condition"></a>A feltétel tesztelése

Most tesztelje le, hogy a feltétel megfelelően működik-e:

1. Ha a logikai alkalmazás még nem futna, kattintson a **Futtatás** gombra a tervező eszköztáron.

   Ezzel a lépéssel manuálisan indíthatja a logikai alkalmazást, és nem kell megvárnia, amíg letelik a megadott időtartam. 
   Addig azonban semmi nem történik, amíg a teszt e-mail meg nem érkezik a postaládájába. 

2. Küldjön magának egy e-mailt, amely megfelel az alábbi feltételeknek:

   * Az e-mail tárgya tartalmazza az eseményindító **Tárgyszűrőjében** megadott szöveget: ```Business Analyst 2 #423501```

   * Az e-mail rendelkezik egy melléklettel. 
   Most csak hozzon létre egy üres szövegfájlt, és csatolja a fájlt az e-mailhez.

   Amint az e-mailek megérkezik, a logikai alkalmazás ellenőrzi a mellékleteket és a megadott szöveget a tárgyban.
   Ha a feltétel teljesül, az eseményindító aktiválódik, a Logic Apps-motorral létrehozat egy logikai alkalmazáspéldányt, és elindíttatja a munkafolyamatot. 

3. A logikai alkalmazás menüjében az **Áttekintés** gombra kattintva ellenőrizze, hogy az eseményindító aktiválódott, és a logikai alkalmazás sikeresen lefutott-e.

   ![Az eseményindító és a futtatási előzmények ellenőrzése](./media/tutorial-process-email-attachments-workflow/checkpoint-run-history.png)

   Ha az eseményindító nem aktiválódott, vagy a logikai alkalmazás a sikeres aktiválás ellenére nem futott le, tekintse meg a [logikai alkalmazás hibaelhárításával foglalkozó szakaszt](../logic-apps/logic-apps-diagnosing-failures.md).

Ezt követően adja meg a **Ha igaz** ágban végrehajtandó műveleteket. Az e-mailek és esetleges mellékleteik mentéséhez törölni kell az e-mail törzsének HTML-formázását, majd blobokat létrehozni a Storage-tárolóban az e-mail és a mellékletek számára.

> [!NOTE]
> A logikai alkalmazásnak semmit nem kell tennie a **Ha hamis** ágon, azaz akkor, ha egy e-mail nem rendelkezik melléklettel. Soron kívüli feladatként az oktatóanyag befejezése után a **Ha hamis** ágban is hozzáadhat valamilyen megfelelő műveletet, amelyet végre szeretne hajtatni.

## <a name="call-the-removehtmlfunction"></a>A RemoveHTMLFunction meghívása

1. A logikai alkalmazás menüjében válassza a **Logic App tervező** lehetőséget. A **Ha igaz** ágban válassza a **Művelet hozzáadása** lehetőséget.

2. Keressen rá az „azure functions” kifejezésre, és válassza a következő műveletet: **Azure Functions – Válasszon ki egy Azure-függvényt**

   ![„Azure Functions – Válasszon ki egy Azure-függvényt” műveletének kiválasztása](./media/tutorial-process-email-attachments-workflow/add-action-azure-function.png)

3. Válassza ki a korábban létrehozott függvényalkalmazást: **CleanTextFunctionApp**

   ![Az Azure-függvényalkalmazás kiválasztása](./media/tutorial-process-email-attachments-workflow/add-action-select-azure-function-app.png)

4. Most válassza ki a függvényt: **RemoveHTMLFunction**

   ![Az Azure-függvény kiválasztása](./media/tutorial-process-email-attachments-workflow/add-action-select-azure-function.png)

5. Nevezze át a függvényalakzatot a következő leírással: ```Call RemoveHTMLFunction to clean email body``` 

6. A függvényalakzatban adja meg a függvény feldolgozandó bemenetét. Adja meg az e-mail törzsét az itt ismertetett módon:

   ![A függvény által várt kérelemtörzs megadása](./media/tutorial-process-email-attachments-workflow/add-email-body-for-function-processing.png)

   1. A **Kérelem törzse** mezőben adja meg a következő szöveget: 
   
      ```{ "emailBody": ``` 

      Amíg a következő lépésben be nem fejezi ezt a bejegyzést, egy érvénytelen JSON-formátumra figyelmeztető hibaüzenet jelenik meg.
      A függvény előző tesztelésekor a megadott bemenet a JavaScript Object Notation (JSON) formátumot használta. 
      Ezért a kérelem törzsének is ezt a formátumot kell követnie. 

   2. A paraméterek vagy a dinamikus tartalmak listájából válassza ki a **Törzs** mezőt az **Új e-mail érkezésekor** alatt.
   A **Törzs** mező után írja be a záró kapcsos zárójelet: ```}```

      ![A függvénybe beadandó kérelemtörzs megadása](./media/tutorial-process-email-attachments-workflow/add-email-body-for-function-processing2.png)

      A logikai alkalmazás definíciójában ez a bejegyzés a következő formátumban jelenik meg:

      ```{ "emailBody": "@triggerBody()?['Body']" }```

7. Mentse a logikai alkalmazást.

Ezután adjon hozzá egy műveletet, amely egy blobot hoz létre a Storage-tárolóban az e-mail törzsének mentéséhez.

## <a name="create-blob-for-email-body"></a>Blob létrehozása az e-mail törzséhez

1. Az Azure-függvényalakzat alatt válassza a **Művelet hozzáadása** lehetőséget. 

2. A **Válasszon műveletet** területen keressen a „blob” kifejezésre, és válassza a következő műveletet: **Azure Blob Storage – Blob létrehozása**

   ![Művelet hozzáadása blob létrehozására az e-mail törzse számára](./media/tutorial-process-email-attachments-workflow/create-blob-action-for-email-body.png)

3. Ha még nem épített ki kapcsolatot Azure Storage-fiókkal, létesítsen kapcsolatot a tárfiókkal az itt bemutatott beállításokkal. Ha elkészült, kattintson a **Létrehozás** gombra.

   ![Tárfiókkapcsolat létrehozása](./media/tutorial-process-email-attachments-workflow/create-storage-account-connection-first.png)

   | Beállítás | Érték | Leírás | 
   | ------- | ----- | ----------- | 
   | **Kapcsolat neve** | AttachmentStorageConnection | A kapcsolat leíró neve | 
   | **Tárfiók** | attachmentstorageacct | A mellékletek mentéséhez korábban létrehozott tárfiók neve | 
   |||| 

4. Nevezze át a **Blob létrehozása** műveletet a következő leírással: ```Create blob for email body```

5. A **Blob létrehozása** műveletnél adja meg ezeket az adatokat, és válassza ki ezeket a paramétereket a blob létrehozásához az itt ismertetett módon:

   ![Blob adatainak megadása az e-mail-törzs számára](./media/tutorial-process-email-attachments-workflow/create-blob-for-email-body.png)

   | Beállítás | Érték | Leírás | 
   | ------- | ----- | ----------- | 
   | **Mappa elérési útja** | /attachments | A korábban létrehozott tároló elérési útja és neve. Tallózással is kikeresheti és kiválaszthatja a tárolót. | 
   | **Blob neve** | **Feladó** mező | Az e-mail-feladó nevének beadása a blob neveként. A paraméterek vagy a dinamikus tartalmak listájából válassza ki a **Feladó** mezőt az **Új e-mail érkezésekor** alatt. | 
   | **Blob tartalma** | **Tartalom** mező | A HTML-mentes e-mail-törzs beadása a blob tartalmaként. A paraméterek vagy a dinamikus tartalmak listájából válassza ki a **Törzs** mezőt a **Call RemoveHTMLFunction to clean email body** (A RemoveHTMLFunction meghívása az e-mail-törzs megtisztításához) területen. |
   |||| 

6. Mentse a logikai alkalmazást. 

### <a name="check-attachment-handling"></a>A mellékletek kezelésének ellenőrzése

A következő lépés annak tesztelése, hogy a logikai alkalmazás a megadott módon kezeli-e az e-maileket:

1. Ha a logikai alkalmazás még nem futna, kattintson a **Futtatás** gombra a tervező eszköztáron.

2. Küldjön magának egy e-mailt, amely megfelel az alábbi feltételeknek:

   * Az e-mail tárgya tartalmazza az eseményindító **Tárgyszűrőjében** megadott szöveget: ```Business Analyst 2 #423501```

   * Az e-mail legalább egy melléklettel rendelkezik. 
   Most csak hozzon létre egy üres szövegfájlt, és csatolja a fájlt az e-mailhez.

   * Az e-mailek rendelkezik teszttartalmakkal a szövegtörzsben, például: 

     ```
     Testing my logic app
     ```

   Ha az eseményindító nem aktiválódott, vagy a logikai alkalmazás a sikeres aktiválás ellenére nem futott le, tekintse meg a [logikai alkalmazás hibaelhárításával foglalkozó szakaszt](../logic-apps/logic-apps-diagnosing-failures.md).

3. Ellenőrizze, hogy a logikai alkalmazás mentette-e az e-mailt a megfelelő tárolóba. 

   1. A Storage Explorerben bontsa ki a **(Local and Attached)** (Helyi és csatolt) > 
   **Tárfiókok** (Storage Accounts) > **attachmentstorageacct (External)** (külső) > 
   **Blob Containers** (Blob-tárolók) > **attachments** elemet.

   2. Ellenőrizze az **attachments** tárolóban az e-mailt. 

      Ekkor még csak az e-mail jelenik meg a tárolóban, mivel a logikai alkalmazás egyelőre nem dolgozza fel a mellékleteket.

      ![A mentett e-mailek ellenőrzése a Storage Explorerben](./media/tutorial-process-email-attachments-workflow/storage-explorer-saved-email.png)

   3. Amikor végzett, törölje az e-mailt a Storage Explorerben.

4. Ha szeretné, az egyelőre tétlen **Ha hamis** ág teszteléséhez küldhet egy olyan e-mailt, amely nem felel meg a feltételeknek.

Ezután adjon hozzá egy iterációt az összes e-mail-melléklet feldolgozására.

## <a name="process-attachments"></a>Mellékletek feldolgozása

A logikai alkalmazás egy **for each** iterációt alkalmaz az e-mailben lévő összes feldolgozásához.

1. A **Blob létrehozása az e-mail törzséhez** alakzatban válassza a **... Továbbiak** lehetőséget, és a következő parancsot: **Iteráció hozzáadása**

   ![„for each” iteráció hozzáadása](./media/tutorial-process-email-attachments-workflow/add-for-each-loop.png)

2. Nevezze át az iterációt a következő leírással: ```For each email attachment```

3. Most adja meg az iteráció által feldolgozandó adatokat. Kattintson a **Kimenet választása az előző lépésekből** mezőbe. A paraméterek vagy a dinamikus tartalmak listájából válassza ki a **Mellékletek** lehetőséget. 

   ![A „Mellékletek” elem kiválasztása](./media/tutorial-process-email-attachments-workflow/select-attachments.png)

   A **Mellékletek** mező egy tömböt továbbít, amely az e-mailben szereplő összes mellékletet tartalmazza. 
   A **For each** iteráció a tömbben beadott mindegyik elemre vonatkozóan megismétli a műveleteket.

4. Mentse a logikai alkalmazást.

Ezután adja hozzá a műveletet, amely az egyes mellékleteket blobként menti az **attachments** Storage-tárolóba.

## <a name="create-blobs-for-attachments"></a>Blobok létrehozása a mellékletek számára

1. A **For each** iterációban válassza a **Művelet hozzáadása** lehetőséget, így megadhatja az egyes talált mellékleteken végrehajtandó feladatot.

   ![Művelet hozzáadása az iterációhoz](./media/tutorial-process-email-attachments-workflow/for-each-add-action.png)

2. A **Művelet kiválasztása** területen keressen a „blob” kifejezésre, majd válassza a következő műveletet: **Azure Blob Storage – Blob létrehozása**

   ![Művelet hozzáadása blob létrehozásához](./media/tutorial-process-email-attachments-workflow/create-blob-action-for-attachments.png)

3. Nevezze át a **Blob létrehozása 2** műveletet a következő leírással: ```Create blob for each email attachment```

4. A **Blob létrehozása mindegyik e-mail-melléklet számára** műveletnél adja meg ezeket az adatokat, és válassza ki a paramétereket az egyes blobok létrehozásához az itt ismertetett módon:

   ![Blob adatainak megadása](./media/tutorial-process-email-attachments-workflow/create-blob-per-attachment.png)

   | Beállítás | Érték | Leírás | 
   | ------- | ----- | ----------- | 
   | **Mappa elérési útja** | /attachments | A korábban létrehozott tároló elérési útja és neve. Tallózással is kikeresheti és kiválaszthatja a tárolót. | 
   | **Blob neve** | **Név** mező | A paraméterek vagy a dinamikus tartalmak listájából válassza ki a **Név** elemet, hogy a blob neveként a melléklet neve legyen beadva. | 
   | **Blob tartalma** | **Tartalom** mező | A paraméterek vagy a dinamikus tartalmak listájából válassza ki a **Tartalom** elemet, hogy a blob tartalmaként a melléklet tartalma legyen beadva. |
   |||| 

5. Mentse a logikai alkalmazást. 

### <a name="check-attachment-handling"></a>A mellékletek kezelésének ellenőrzése

A következő lépés annak tesztelése, hogy a logikai alkalmazás a megadott módon kezeli-e a mellékleteket:

1. Ha a logikai alkalmazás még nem futna, kattintson a **Futtatás** gombra a tervező eszköztáron.

2. Küldjön magának egy e-mailt, amely megfelel az alábbi feltételeknek:

   * Az e-mail tárgya tartalmazza az eseményindító **Tárgyszűrőjében** megadott szöveget: ```Business Analyst 2 #423501```

   * Az e-mail legalább két melléklettel rendelkezik. 
   Most csak hozzon létre két üres szövegfájlt, és csatolja a fájlokat az e-mailhez.

   Ha az eseményindító nem aktiválódott, vagy a logikai alkalmazás a sikeres aktiválás ellenére nem futott le, tekintse meg a [logikai alkalmazás hibaelhárításával foglalkozó szakaszt](../logic-apps/logic-apps-diagnosing-failures.md).

3. Ellenőrizze, hogy a logikai alkalmazás elmentette-e az e-mailt és a mellékleteket a megfelelő tárolóba. 

   1. A Storage Explorerben bontsa ki a **(Local and Attached)** (Helyi és csatolt) > 
   **Tárfiókok** (Storage Accounts) > **attachmentstorageacct (External)** (külső) > 
   **Blob Containers** (Blob-tárolók) > **attachments** elemet.

   2. Ellenőrizze az **attachments** tárolóban az e-mailt és a mellékleteket egyaránt.

      ![A mentett e-mailek és mellékletek ellenőrzése](./media/tutorial-process-email-attachments-workflow/storage-explorer-saved-attachments.png)

   3. Amikor végzett, törölje az e-mailt és a mellékleteket a Storage Explorerben.

Ezután adjon meg egy műveletet, hogy a logikai alkalmazás egy e-mail-üzenetet küldjön a mellékletek áttekintésére vonatkozóan.

## <a name="send-email-notifications"></a>E-mail-értesítések küldése

1. A **Ha igaz** ágban az **e-mail-mellékletekre vonatkozó For each** iterációban válassza a **Művelet hozzáadása** lehetőséget. 

   ![Művelet hozzáadása a „for each” iteráció alatt](./media/tutorial-process-email-attachments-workflow/add-action-send-email.png)

2. A **Válasszon műveletet** területen keressen rá az „e-mail küldése” kifejezésre, majd válassza ki a kívánt levelezési szolgáltató „e-mail küldése” műveletét. Ha a műveletek listáját adott szolgáltatásra szeretné szűrni, először kiválaszthatja az összekötőt az **Összekötők** területen.

   ![Az „e-mail küldése” művelet kiválasztása az e-mail-szolgáltatóhoz](./media/tutorial-process-email-attachments-workflow/add-action-select-send-email.png)

   * Munkahelyi vagy iskolai Azure-fiókok esetében válassza az Office 365 Outlookot. 
   * Személyes Microsoft-fiókok esetében válassza az Outlook.com-összekötőt. 

3. Ha a rendszer kéri a hitelesítő adatokat, jelentkezzen be az e-mail-fiókjába, hogy a Logic Apps kapcsolatot létesíthessen vele.

4. Nevezze át az **E-mail küldése** műveletet a következő leírással: ```Send email for review```

5. Adja meg a művelet adatait, és válassza ki az e-mailbe szövegében szerepeltetni kívánt mezőket az itt ismertetett módon. Ha üres sorokat kíván hozzáadni a szerkesztőmezőkhöz, nyomja le a Shift + Enter billentyűkombinációt.  

   Ha például a dinamikus tartalmak listáját használja:

   ![E-mail-értesítés küldése](./media/tutorial-process-email-attachments-workflow/send-email-notification.png)

   Ha egy várt mezőt nem talál a listában, válassza a **Továbbiak** lehetőséget az **Új e-mail érkezésekor** elem mellett a dinamikus tartalmak listájában vagy a paraméterek listája végén.

   | Beállítás | Érték | Megjegyzések | 
   | ------- | ----- | ----- | 
   | **Címzett** | <*recipient-email-address*> | Tesztelési célokra használhatja a saját e-mail-címét. | 
   | **Tárgy**  | ```ASAP - Review applicant for position: ``` **Tárgy** | Az e-mail tárgya, amelyet használni kíván. A paraméterek vagy a dinamikus tartalmak listájából válassza ki a **Tárgy** mezőt az **Új e-mail érkezésekor** területen. | 
   | **Törzs** | ```Please review new applicant:``` <p>```Applicant name: ``` **Feladó** <p>```Application file location: ``` **Elérési út** <p>```Application email content: ``` **Törzs** | Az e-mail törzsének tartalma. A paraméterek vagy a dinamikus tartalmak listájából válassza ki az alábbi mezőket: <p>- A **Feladó** mezőt az **Új e-mail érkezésekor** alatt </br>- Az **Elérési út** mezőt a **Blob létrehozása az e-mail törzséhez** alatt </br>- A **Törzs** mezőt a **Call RemoveHTMLFunction to clean email body** (A RemoveHTMLFunction meghívása az e-mail-törzs megtisztításához) alatt | 
   |||| 

   Ha olyan mezőt választ ki, amely egy tömböt tartalmaz, például a **Tartalom** elemet, amely egy mellékleteket tartalmazó tömb, a tervező automatikusan hozzáad egy „For each” iterációt a mezőre hivatkozó művelet köré. 
   Így a logikai alkalmazás a tömb mindegyik elemén végrehajthatja az adott műveletet. 
   Az iteráció eltávolításához törölje a mezőt a tömbből, helyezze a hivatkozó műveletet a tömbön kívül, válassza az iteráció címsorában lévő három pontot (**...**), majd válassza a **Törlés** lehetőséget.
     
6. Mentse a logikai alkalmazást. 

Következő lépésként tesztelje a logikai alkalmazást, amely most az alábbi példára hasonlít:

![Befejezett logikai alkalmazás](./media/tutorial-process-email-attachments-workflow/complete.png)

## <a name="run-your-logic-app"></a>A logikai alkalmazás futtatása

1. Küldjön magának egy e-mailt, amely megfelel az alábbi feltételeknek:

   * Az e-mail tárgya tartalmazza az eseményindító **Tárgyszűrőjében** megadott szöveget: ```Business Analyst 2 #423501```

   * Az e-mail egy vagy több melléklettel rendelkezik. 
   Megint felhasználhatja az üres szövegfájlokat az előző tesztből. 
   Ha valószerűbb forgatókönyvet szeretne, csatoljon egy önéletrajzfájlt.

   * Az e-mail törzse tartalmazza a következő szöveget (másolható és beilleszthető):

     ```
     Name: Jamal Hartnett   
     
     Street address: 12345 Anywhere Road   
     
     City: Any Town   
     
     State or Country: Any State   
     
     Postal code: 00000   
     
     Email address: jamhartnett@outlook.com   
     
     Phone number: 000-000-0000   
     
     Position: Business Analyst 2 #423501   

     Technical skills: Dynamics CRM, MySQL, Microsoft SQL Server, JavaScript, Perl, Power BI, Tableau, Microsoft Office: Excel, Visio, Word, PowerPoint, SharePoint, and Outlook   

     Professional skills: Data, process, workflow, statistics, risk analysis, modeling; technical writing, expert communicator and presenter, logical and analytical thinker, team builder, mediator, negotiator, self-starter, self-managing  
     
     Certifications: Six Sigma Green Belt, Lean Project Management   
     
     Language skills: English, Mandarin, Spanish   
     
     Education: Master of Business Administration   
     ```

2. Futtassa a logikai alkalmazást. Ha sikeresen fut, a logikai alkalmazás egy, a következőhöz hasonló e-mailt küld:

   ![Logikai alkalmazás által küldött e-mail-értesítés](./media/tutorial-process-email-attachments-workflow/email-notification.png)

   Ha nem kap e-mailt, ellenőrizze a levélszemét mappát. 
   Előfordulhat, hogy az ilyen típusú levelek fennakadnak a levélszemétszűrőn. 
   Ha nem biztos abban, hogy a logikai alkalmazás megfelelően futott-e, tekintse meg a [logikai alkalmazás hibaelhárításával foglalkozó szakaszt](../logic-apps/logic-apps-diagnosing-failures.md).

Gratulálunk, sikeresen létrehozott és futtatott egy logikai alkalmazást, amely feladatokat automatizál különböző Azure-szolgáltatásokban, és egyéni kódokat hív meg.

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

Ha már nincs rá szükség, törölje a logikai alkalmazást és a kapcsolódó erőforrásokat tartalmazó erőforráscsoportot. Az Azure főmenüjében lépjen az **Erőforráscsoportok** elemre, és válassza ki a logikai alkalmazás erőforráscsoportját. Válassza az **Erőforráscsoport törlése** elemet. Megerősítésként írja be az erőforráscsoport nevét, és válassza a **Törlés** lehetőséget.

![Logikai alkalmazás erőforráscsoportjának törlése](./media/tutorial-process-email-attachments-workflow/delete-resource-group.png)

## <a name="get-support"></a>Támogatás kérése

* A kérdéseivel látogasson el az [Azure Logic Apps fórumára](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* A funkciókkal kapcsolatos ötletek elküldéséhez vagy megszavazásához látogasson el a [Logic Apps felhasználói visszajelzéseinek oldalára](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>További lépések

Az oktatóanyagban létrehoztunk egy logikai alkalmazást e-mail mellékletek, feldolgozására és tárolására Azure-szolgáltatások, például az Azure Storage és az Azure Functions integrálásával. Most megismerkedhet a logikai alkalmazások létrehozásához használható egyéb összekötőkkel.

> [!div class="nextstepaction"]
> [További tudnivalók a Logic Apps összekötőiről](../connectors/apis-list.md)
