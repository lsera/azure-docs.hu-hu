---
title: "A begyűjtött Azure Log Analytics-adatok megtekintése vagy elemzése | Microsoft Docs"
description: "Ez a cikk egy olyan oktatóanyag, amely leírja, hogyan hozhatók létre naplóbeli keresések, és hogyan elemezhetők a Log Analytics-erőforrásokban tárolt adatok a Naplók keresése portálon.  Az oktatóanyag részeként néhány mintalekérdezést is futtatni fog különböző adattípusok visszaadásához és az eredmények elemzéséhez."
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/26/2017
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: fc5dcc945750b4ab4eef337dbd96bd051bb4dd81
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 02/21/2018
---
# <a name="view-or-analyze-data-collected-with-log-analytics-log-search"></a>A Log Analytics-naplókereséssel gyűjtött adatok megtekintése vagy elemzése

A Log Analyticsben a naplókereséssel lekérdezéseket hozhat létre a begyűjtött adatok elemzéséhez, és használhatja a már meglévő irányítópultokat, amelyeket a legértékesebb keresések grafikus nézeteivel testre is tud szabni.  Most, hogy meghatározta az Azure-beli virtuális gépekről és a tevékenységnaplókból származó működési adatok gyűjtésének részleteit, ebben az oktatóanyagban a következőkkel ismerkedhet meg:

> [!div class="checklist"]
> * Az Azure Log Analytics-erőforrás frissítése az új lekérdezési nyelvre 
> * Az eseményadatok egyszerű keresése és különböző funkciók használata az eredmények módosításához és szűréséhez 
> * A teljesítményadatok használatának módja

Az oktatóanyagban található példa elvégzéséhez szüksége lesz egy meglévő virtuális gépre, amely [a Log Analytics-munkaterülethez csatlakozik](log-analytics-quick-collect-azurevm.md).  

A lekérdezések létrehozása és szerkesztése, valamint a visszaadott adatok interaktív használata kétféleképpen végezhető el.  Az alapvető lekérdezésekhez használja az Azure Portal Naplók keresése oldalát, a speciális lekérdezéshez pedig a Bővített analitika portált. A két portál eltérő funkcióiról lásd a következővel foglalkozó cikket: [Portálok a naplólekérdezések létrehozásához és szerkesztéséhez az Azure Log Analyticsben](log-analytics-log-search-portals.md)

Ebben az oktatóanyagban az Azure Portal naplókeresési eszközét használjuk. 

## <a name="log-in-to-azure-portal"></a>Bejelentkezés az Azure Portalra
Jelentkezzen be az Azure Portalra a [https://portal.azure.com](https://portal.azure.com) címen. 

## <a name="open-the-log-search-portal"></a>A naplókeresési portál megnyitása 
Először nyissa meg a naplókeresési portált.   

1. Az Azure Portalon kattintson a **Minden szolgáltatás** lehetőségre. Az erőforrások listájába írja be a **Log Analytics** kifejezést. Ahogy elkezd gépelni, a lista a beírtak alapján szűri a lehetőségeket. Válassza a **Log Analytics** elemet.
2. A Log Analytics-előfizetések ablaktábláján válasszon egy munkaterületet, majd válassza ki a **Naplóbeli keresés** csempét.<br> ![Naplóbeli keresés gomb](media/log-analytics-tutorial-viewdata/azure-portal-01.png)

A portálon a Log Analytics-erőforrások oldalának tetején megjelenhet egy szalag, amely felhívja a figyelmét a Log Analytics frissítésére.<br> ![Log Analytics frissítési felhívás az Azure Portalon](media/log-analytics-tutorial-viewdata/log-analytics-portal-upgradebanner.png)

A Log Analytics nemrég bevezetett egy új lekérdezési nyelvet, amely megkönnyíti a lekérdezések összeállítását, a különböző forrásokból származó adatok összevetését és a tendenciák vagy problémák gyors azonosítására szolgáló elemzések elvégzését.

A frissítés folyamata nem bonyolult.  A folyamat megkezdéséhez kattintson a **További információ és frissítés** szalagra.  Olvassa végig a frissítéssel kapcsolatos további információkat a frissítésre vonatkozó információk lapján, és kattintson a **Frissítés most** gombra.

A folyamat néhány percet igénybe vesz, és az állapotát a menü **Értesítések** területén követheti nyomon. Az [új lekérdezési nyelv előnyeit](log-analytics-log-search-upgrade.md#why-the-new-language) ismertető témakörből további részleteket is megtudhat.

## <a name="create-a-simple-search"></a>Egyszerű keresés létrehozása
A feldolgozható adatok lekérdezésének leggyorsabb módja az egyszerű lekérdezés, amely egy tábla összes rekordját visszaadja.  Ha Windows vagy Linux rendszerű ügyfelek vannak csatlakoztatva a munkaterülethez, akkor az adatok az Event (Esemény, Windows) vagy a Syslog (Linux) táblában találhatók.

Írja be a következő lekérdezések egyikét a keresőmezőbe, és kattintson a keresés gombra.  

```
Event
```
```
Syslog
```

A rendszer az alapértelmezett listanézetben adja vissza az adatokat, és megjeleníti, hogy összesen hány rekord lett visszaadva.

![Egyszerű lekérdezés](media/log-analytics-tutorial-viewdata/log-analytics-portal-eventlist-01.png)

Az egyes rekordoknak csak az első néhány tulajdonsága jelenik meg.  Kattintson a **részletek megjelenítése** gombra egy adott rekord összes tulajdonságának megjelenítéséhez.

## <a name="filter-results-of-the-query"></a>A lekérdezés eredményeinek szűrése
A képernyő bal oldalán található a szűrő ablaktábla, amellyel szűrőt adhat a lekérdezéshez anélkül, hogy közvetlenül módosítaná azt.  Egy adott rekordtípushoz több rekordtulajdonság is megjelenik, és egy vagy több tulajdonságértéket is kiválaszthat a keresési eredmények szűkítése érdekében.

Ha az **Event** táblával dolgozik, jelölje be az **EVENTLEVELNAME** területen lévő **Error** (Hiba) elem melletti jelölőnégyzetet.   Ha a **Syslog** táblával dolgozik, jelölje be a **SEVERITYLEVEL** területen lévő **err** elem melletti jelölőnégyzetet.  Így a lekérdezés a következőképpen módosul, és csak a hibaeseményeket jeleníti meg.

```
Event | where (EventLevelName == "Error")
```
```
Syslog | where (SeverityLevel == "err")
```

![Szűrés](media/log-analytics-tutorial-viewdata/log-analytics-portal-eventlist-02.png)

Ha tulajdonságokat szeretne adni a szűrő ablaktáblához, akkor az egyik rekord tulajdonságmenüjében válassza a **Hozzáadás a szűrőkhöz** elemet.

![Hozzáadás a szűrőhöz menü](media/log-analytics-tutorial-viewdata/log-analytics-portal-eventlist-03.png)

Ugyanazon szűrő beállításához válassza ki egy olyan rekord tulajdonságmenüjéből a **Szűrő** elemet, amely rendelkezik a szűrendő értékkel.  

Csak olyan tulajdonságoknál adhatja meg a **Szűrő** beállítást, amelyek neve kéken jelenik meg, ha föléjük viszi a mutatót.  Ezek a *kereshető* mezők, amelyek keresési feltételekhez vannak indexelve.  A szürke mezők *kereshető szabad szöveges* mezők, amelyeknél csak a **Hivatkozások megjelenítése** beállítás aktív.  Ez a beállítás azokat a rekordokat adja vissza, amelyeknek bármely tulajdonságában megtalálható a keresett érték.

Egyetlen tulajdonság alapján csoportosíthatja az eredményeket, ha kiválasztja a rekord menüjének **Csoportosítási szempont** elemét.  Ez egy [summarize](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/summarize-operator) (összegző) operátort ad a lekérdezéshez, amely egy diagramban jeleníti meg az eredményeket.  Több tulajdonság alapján is csoportosíthat, de ehhez közvetlenül kell szerkesztenie a lekérdezést.  Válassza ki a rekord menüjét a **Számítógép** tulajdonság mellett, és válassza a **Csoportosítási szempont: „Computer”** (Számítógép) lehetőséget.  

![Csoportosítás számítógép alapján](media/log-analytics-tutorial-viewdata/log-analytics-portal-eventlist-04.png)

## <a name="work-with-results"></a>Munka az eredményül kapott adatokkal
A naplókeresési portál számos funkcióval rendelkezik a lekérdezések eredményeinek feldolgozásához.  Rendezheti, szűrheti és csoportosíthatja az eredményeket az adatok elemzése érdekében a tényleges lekérdezés módosítása nélkül.  A lekérdezések eredményei alapértelmezés szerint nincsenek rendezve.

Ha táblázatos formában szeretné megtekinteni az adatokat, ami további lehetőségeket nyújt a szűréshez és a rendezéshez, kattintson a **Tábla** gombra.  

![Tábla nézet](media/log-analytics-tutorial-viewdata/log-search-portal-table-01.png)

Kattintson az egyik rekordnál található nyílra a rekord részleteinek megtekintéséhez.

![Az eredmények rendezése](media/log-analytics-tutorial-viewdata/log-search-portal-table-02.png)

Bármely mező alapján rendezhet, ha az oszlopfejlécére kattint.

![Az eredmények rendezése](media/log-analytics-tutorial-viewdata/log-search-portal-table-03.png)

Az oszlopban található valamelyik érték alapján szűrni is tudja az eredményeket, ha a szűrő gombra kattint, és megadja a szűrőfeltételt.

![Szűrés eredményei](media/log-analytics-tutorial-viewdata/log-search-portal-table-04.png)

Egy oszlop alapján úgy csoportosíthat, ha az oszlopfejlécet az eredmények tetejére húzza.  Több mező alapján úgy csoportosíthat, ha több oszlopot húz felülre.

![Az eredmények csoportosítása](media/log-analytics-tutorial-viewdata/log-search-portal-table-05.png)


## <a name="work-with-performance-data"></a>Munka a teljesítményadatokkal
A rendszer a Windows- és a Linux-ügynökök teljesítményadatait is a Log Analytics-munkaterületen tárolja, a **Perf** táblában.  A teljesítményrekordok ugyanúgy néznek ki, mint a többi rekord, és egy egyszerű lekérdezés írható hozzájuk, amely az eseményekhez hasonlóan az összes teljesítményrekordot visszaadja.

```
Perf
```

![Teljesítményadatok](media/log-analytics-tutorial-viewdata/log-analytics-portal-perfsearch-01.png)

Nem szerencsés azonban, ha a rendszer az összes teljesítményobjektum és számláló több millió rekordját visszaadja.  A fent használt módszerekkel szűrheti a kapott adatokat, vagy azt is megteheti, hogy a következő lekérdezést közvetlenül beírja a napló keresőmezőjébe.  Ez csak a Windows és Linux rendszerű számítógépek processzorhasználati rekordjait adja vissza.

```
Perf | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time")
```

![Processzorhasználat](media/log-analytics-tutorial-viewdata/log-analytics-portal-perfsearch-02.png)

Ezzel egy adott számlálóra korlátozza az adatokat, de nem rendezi őket olyan formátumba, amely megkönnyítené a használatukat.  Megjelenítheti az adatokat egy vonaldiagramban, de először csoportosítania kell őket a Computer (Számítógép) és a TimeGenerated (Létrehozás időpontja) érték alapján.  Ha több mező alapján szeretne csoportosítani, közvetlenül kell módosítania a lekérdezést a következők szerint.  Ez a **CounterValue** tulajdonság [avg](https://docs.loganalytics.io/docs/Language-Reference/Aggregation-functions/avg()) függvényével számítja ki órás lebontásban az átlagértéket.

```
Perf  | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time") | summarize avg(CounterValue) by Computer, TimeGenerated
```

![Teljesítményadatok diagram](media/log-analytics-tutorial-viewdata/log-analytics-portal-perfsearch-03.png)

Most, hogy az adatok megfelelően vannak csoportosítva, megjelenítheti őket egy vizuális diagramban a [render](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/render-operator) operátor hozzáadásával.  

```
Perf  | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time") | summarize avg(CounterValue) by Computer, TimeGenerated | render timechart
```

![Vonaldiagram](media/log-analytics-tutorial-viewdata/log-analytics-portal-linechart-01.png)

## <a name="next-steps"></a>További lépések
Ez az oktatóanyag bemutatta, hogyan hozhatók létre alapszintű naplókeresések az esemény- és teljesítményadatok elemzéséhez.  A következő oktatóanyagból megtudhatja, hogyan jelenítheti meg az adatokat egy irányítópult létrehozásával.

> [!div class="nextstepaction"]
> [Log Analytics-irányítópultok létrehozása és megosztása](log-analytics-tutorial-dashboards.md)