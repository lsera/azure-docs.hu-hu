---
title: "Azure-alkalmazások és erőforrások figyelése |} Microsoft Docs"
description: "Microsoft-szolgáltatások és funkciók, amelyek az Azure-szolgáltatások és alkalmazások egy teljes felügyeleti stratégia áttekintése."
author: robb
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 1b962c74-8d36-4778-b816-a893f738f92d
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2018
ms.author: robb,bwren
ms.openlocfilehash: d8da175a551f7c589c313b2289b2a0209dbd2b56
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/02/2018
---
# <a name="monitoring-azure-applications-and-resources"></a>Azure-alkalmazások és erőforrások figyelése

A gyűjtése és elemzése adatok megállapításához a teljesítményt, állapotának és rendelkezésre állását, valamint az üzleti alkalmazás az erőforrásokat, amelyek attól függ, a folyamat figyelése. Egy hatékony felügyeleti stratégia segít megérteni a az alkalmazás összetevői részletes működését. Emellett segítséget nyújt a hasznos üzemidő növeléséhez a proaktív értesítés, kritikus fontosságú problémáit, hogy a megoldásuk mielőtt azok veszélyeztetnék.

Azure tartalmaz több szolgáltatást, amely külön-külön végrehajtani egy adott szerepkör vagy a feladatot a figyelés munkaterületen. Ezek a szolgáltatások együtt, egy átfogó megoldást nyújt az gyűjtése, elemzése és az alkalmazás és az őket támogató Azure-erőforrások telemetriai ható biztosításához. Ezek is működnek, ahhoz, hogy adja meg a figyelési környezet hibrid kritikus a helyszíni erőforrások figyelése. Az eszközök és a rendelkezésre álló adatok ismertetése első lépése az alkalmazás teljes felügyeleti stratégia kidolgozásában. 

Az alábbi ábrán látható az összetevőket, amelyek együttműködve biztosítják az Azure-erőforrások figyelése konceptuális ábrázolása. Az alábbi szakaszok ismertetik ezeket az összetevőket, és adja meg a részletes műszaki információkra mutató hivatkozásokat tartalmaz.

![Figyelés – áttekintés](media/monitoring-overview/overview.png)

## <a name="basic-monitoring"></a>Alapszintű figyelés
Alapvető figyelését teszi lehetővé alapvető és szükséges Azure-erőforrások között. Ezek a szolgáltatások minimális konfigurációs és gyűjthet alapvető figyelési premium-szolgáltatásokhoz használt.    

### <a name="azure-monitor"></a>Azure Monitor
[Az Azure figyelő](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md) lehetővé teszi, hogy alapvető figyelési Azure Service gyűjteményét tételével [metrikák](../monitoring-and-diagnostics/monitoring-overview-metrics.md), [tevékenységi naplóit](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md), és [diagnosztikai naplók](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md). Például a tevékenységnapló megtudhatja, amikor új erőforrásokat létrehozni vagy módosítani. 

Metrikák érhetők el, amely teljesítménystatisztikáit. Adja meg a különböző erőforrások és még az operációs rendszer a virtuális gépen. Az adat megtekintéséhez az Azure-portálon szoftverkategóriák valamelyikét, elküldi a Azure Naplóelemzés trendekkel és részletes elemzéséhez vagy proaktív értesítik a kritikus fontosságú problémáit riasztási szabályok létrehozásához.

### <a name="service-health"></a>Service Health
Az alkalmazás állapotát az Azure-szolgáltatásokhoz, amelyektől függ támaszkodik. [Az Azure szolgáltatás állapota](../service-health/service-health-overview.md) azonosítja a probléma merül fel az Azure-szolgáltatásokkal, amelyek hatással lehetnek az alkalmazás. Szolgáltatás állapota is segítséget nyújt az ütemezett karbantartási terv.

### <a name="azure-advisor"></a>Azure Advisor
[Az Azure Advisor](../advisor/advisor-overview.md) folyamatosan figyeli a erőforrás konfigurációs és használati telemetriai adatokat. Majd biztosít ajánlott eljárásai alapján személyre szabott javaslatokat. Ezek az ajánlások a következő segítségével javíthatja a teljesítményt, biztonsági és az erőforrásokat, amelyek támogatják az alkalmazások rendelkezésre állását.


## <a name="premium-monitoring-services"></a>Prémium szintű figyelési szolgáltatásokat
Az Azure-szolgáltatásokat gyűjtése és elemzése a figyelési adatok gazdag képességeit adja meg. Ezek a szolgáltatások alapszintű figyeléséről hozza létre, és az Azure-ban a közös funkcióinak előnyeit. A gyűjtött adatokat, hogy az alkalmazásaikat és infrastruktúrájukat egyedi betekintést biztosítson hatékony analytics biztosítanak. Adatok forgatókönyvek, amelyek a környezetében, a különböző jelentenek.

### <a name="application-insights"></a>Application Insights
Használhat [Azure Application Insights](http://azure.microsoft.com/documentation/services/application-insights) figyelése rendelkezésre állását, teljesítményét és használatát az alkalmazás, hogy a felhőbeli vagy helyszíni helyezkedik el. 

Az alkalmazás működéséhez az Application insights szolgáltatással leírására, érhet mélyebben elemezheti. Ezután gyorsan azonosíthatja és hibák diagnosztizálása a felhasználót, hogy ezeket várakozás nélkül. Gyűjtött adatokat hogy az alkalmazás karbantartása és fejlesztése megalapozott döntések. 

Az Application Insights kiterjedt eszközei használják az általa gyűjtött adatokat a rendelkezik. Az Application Insights az adatok közös helyen tárolja. Kihasználja a megosztott szolgáltatását, például a riasztások, irányítópultok és a Log Analytics lekérdezési nyelv mélyreható elemzéseket is igénybe vehet.

### <a name="log-analytics"></a>Log Analytics
[Naplófájl Analytics](http://azure.microsoft.com/documentation/services/log-analytics) központi szereppel Azure figyelési begyűjtenie az adatokat a különböző erőforrások egy tárházba. Hiba elemezheti az adatokat egy hatékony lekérdező nyelv használatával. 

Application insights szolgáltatással és az Azure Security Center tárolja az adatokat a Naplóelemzési adatokat tárolja, és az elemzés motor használja. Adatok Azure-figyelő felügyeleti megoldás és a felhőben, vagy a helyszíni virtuális gépeken telepített ügynökök gyűjtött adatokat is. Ez a funkció megosztott alkotják a környezet átfogó képet nyújt segítséget. 


### <a name="service-map"></a>Szolgáltatástérkép
[Szolgáltatástérkép](../operations-management-suite/operations-management-suite-service-map.md) IaaS környezetét betekintést biztosít a virtuális gépek a különböző folyamatok és a más számítógépeken és a külső folyamatok függőségek elemzésével. Események, teljesítményadatok és felügyeleti megoldásokat Naplóelemzési integrálható. Minden számítógép és a környezet többi részére viszonya környezetében majd megtekintheti ezeket az adatokat. 

Szolgáltatástérkép hasonlít [Application Insights az alkalmazás-hozzárendelés](../application-insights/app-insights-app-map.md). Az alkalmazásokat támogató infrastruktúra-összetevőihez összpontosít.

### <a name="network-watcher"></a>Network Watcher
[Hálózati figyelő](../network-watcher/network-watcher-monitoring-overview.md) forgatókönyv-alapú figyelési és diagnosztika biztosít a különböző hálózati forgatókönyvek az Azure-ban. Az Azure metrikák és diagnosztika további elemzés céljából tárolja az adatokat. Működik együtt a különböző szempontjairól a hálózat figyeléséhez a következő megoldások:
* [A hálózati teljesítmény figyelése (NPM)](https://blogs.msdn.microsoft.com/azuregov/2017/09/05/network-performance-monitor-general-availability/): A felhő alapú hálózatfigyelési megoldás, amely a nyilvános felhők, adatközpontok és a helyszíni környezetben kapcsolatát figyeli.
* [Az ExpressRoute-figyelő](https://azure.microsoft.com/en-in/blog/monitoring-of-azure-expressroute-in-preview/): az NPM funkció, amely figyeli a végpontok közötti kapcsolat és a teljesítmény Azure ExpressRoute-Kapcsolatcsoportok keresztül.
* Forgalom elemzési: Egy felhőalapú megoldás, amely a felhasználó-és alkalmazás betekintést biztosít a felhő hálózaton.
* [DNS Analytics](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-dns): olyan megoldás, amely biztosítja a biztonság, teljesítmény és insights kapcsolatos műveletek, a DNS-kiszolgálók alapján.

### <a name="management-solutions"></a>Felügyeleti megoldások
[Megoldások](../log-analytics/log-analytics-add-solutions.md) logika, amely áttekintést adnak a egy adott alkalmazás vagy szolgáltatás csomagolt csoportja. Log Analyticshez való tárolását és elemzését az általuk összegyűjtött figyelési adatokat támaszkodnak. 

Adja meg a különböző Azure és a külső-szolgáltatások figyelése a Microsoft és a partnerek megoldások érhetők el. Figyelési megoldásoknak közé:
* [Tároló figyelés](../log-analytics/log-analytics-containers.md), segítségével megtekintheti, és a tároló több gazdagép kezeléséhez.
* [Az Azure SQL elemzés](../log-analytics/log-analytics-azure-sql.md), amely összegyűjti, és az Azure SQL-adatbázisok teljesítménymutatók visualizes.


## <a name="shared-functionality"></a>Megosztott funkció
A következő Azure eszközök kritikus ellátni a Premium-szolgáltatások figyelése. Több szolgáltatás megoszthatja őket, így veheti hasznát közös funkcióit és konfigurációk több szolgáltatásban.

### <a name="alerts"></a>Riasztások
[Az Azure riasztások](../monitoring-and-diagnostics/monitoring-overview-alerts.md) proaktív értesíti kritikus feltételek és vélhetően intézkedéseket. A riasztási szabályok adatokat használhatja több forrásból, beleértve a metrikák és a naplókat. Használata [művelet csoportok](../monitoring-and-diagnostics/monitoring-action-groups.md), tartalmazó címzettek és a riasztás válaszul műveletek egyedi beállítása. A követelmények alapján lehet riasztások webhookok használatával indítsa el a külső műveletek és a ITSM eszközök integrálható.

### <a name="dashboards"></a>Irányítópultok
Használhat [Azure irányítópultok](../azure-portal/azure-portal-dashboards.md) a különböző típusú adatok egyesítése egytáblás az Azure portálon. Majd Azure másokkal is megosztja az irányítópultot. 

Például létrehozhat egy irányítópultot, amely egyesíti:
- Csempék, amelyek megjelenítik a mérőszámok egy grafikonon
- Egy tábla tevékenység naplói
- Az Application Insights használati diagram
- A Naplóelemzési napló keresés kimenete

Naplóelemzési adatokat emellett exportálhatja [Power BI](https://docs.microsoft.com/power-bi/). Hiba kihasználhatja további képi megjelenítést. Is biztosíthatja az adatok belül, mind a szervezeten kívüli mások számára elérhető.

### <a name="metrics-explorer"></a>Metrikaböngésző
[Metrikák](../monitoring-and-diagnostics/monitoring-overview-metrics.md) egy Azure-erőforrás a művelet és a teljesítmény az erőforrás által létrehozott, numerikus érték. A Metrikaböngésző, küldhet metrikák Naplóelemzési analitikai adatok más forrásokból.



### <a name="activity-log"></a>Tevékenységnapló
[Tevékenységnapló](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) egy Azure-erőforrás működésével kapcsolatos adatokat biztosít. Az alábbiakat:
- Az erőforrás-konfiguráció változik.
- Az incidensek Állapotfigyelő szolgáltatás.
- Az erőforrás használata jobb javaslatait.
- Automatikus skálázás műveleteivel kapcsolatos információkat. 

Egy adott erőforráshoz naplók az Azure-portálon lapon tekintheti meg. Vagy több erőforrás származó naplók megtekintheti a tevékenységek napló Explorer. 

A Naplóelemzési küldhet tevékenységi naplóit is. A naplók elemezheti, megoldások, az ügynököt a virtuális gépek és a más forrásokból gyűjtött adatok használatával.


## <a name="example-scenarios"></a>Példaforgatókönyvek
Az alábbiakban a magas szintű példáiért, amelyek bemutatják, hogyan használható különböző eszközök az Azure-ban különböző helyzetek kezelésére.

### <a name="monitoring-a-web-application"></a>A webes alkalmazás figyelése
Fontolja meg az Azure App Service, Azure Storage és SQL-adatbázis Azure szolgáltatásba telepített webalkalmazás. Elérésével megkezdése [metrikák](../monitoring-and-diagnostics/monitoring-overview-metrics.md) és [tevékenységi naplóit](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) ezeket az erőforrásokat az Azure-portálon a felhasználók számára. Keresse meg a kritikus fontosságú adatokat, például az alkalmazás és az átlagos válaszidő-kérelmek számát. Is azonosíthatja végrehajtott bármilyen konfigurációs módosításokat.

Majd keresse meg a figyelő a portál metrikák és a naplókat a különböző erőforrások együtt megtekintéséhez. A metrika, szabványos paramétereinek meghatározása akkor [riasztási szabályok létrehozásához](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md). Ezek a szabályok proaktív értesítést küldenek, ha, például növeli az átlagos válaszideje meghaladja a küszöbértéket. Ahhoz, hogy az alkalmazás napi teljesítmény áttekintő képet, hozzon létre egy Azure irányítópult a mérőszámok, amelyek kritikus KPI-k diagramok megjelenítéséhez.

Az alkalmazás figyelést végrehajtásához, [konfigurálja az Application Insights](../application-insights/quick-monitor-portal.md). Most begyűjtheti a további adatok nyújt további betekintést a művelet és az alkalmazás teljesítményét. Az Application Insights észleli a alapul szolgáló, az alkalmazás-összetevők közötti kapcsolatokat. Lehetővé teszi a vizuális ábrázolását keresztül [alkalmazás-hozzárendelés](../application-insights/app-insights-app-map.md) alapján kialakulhat [végpont nyomkövetés](../application-insights/app-insights-transaction-diagnostics.md) felderítéséhez a pontos összetevőt, a függőségekkel vagy az adott probléma történt kivétel. 

Létrehozhat [rendelkezésreállás figyelésére szolgáló tesztek](../application-insights/app-insights-monitor-web-app-availability.md) proaktív a különböző régiókban az alkalmazás teszteléséhez. Segítségével a fejlesztők akkor [engedélyezése a Profilkészítő](../application-insights/enable-profiler-compute.md) , nyomon követheti az kérelmek és egy adott kódsort le kivételek. Ahhoz, hogy további betekintést az alkalmazásban használt szolgáltatások, vegye fel a [SQL elemzési megoldások](../log-analytics/log-analytics-azure-sql.md) Naplóelemzési a további adatok gyűjtéséért felelős ügyfélfeladatot. 

Némi várakozás után úgy dönt, hogy ha a hely teljesítményére a küszöbérték alá csökkent időszakokra alapvető okát. A Naplóelemzési lefuttatni egy lekérdezést. A program segít a konfiguráció az Application Insights által gyűjtött használati és teljesítményadatokat adatokat és a teljesítményadatokat, amely támogatja az alkalmazás Azure-erőforrások közötti összefüggéseket.


### <a name="monitoring-virtual-machines"></a>Virtuális gépek figyelése
Lehetősége van a Windows és Linux rendszerű virtuális gépek Azure-beli kombinációját. Azure figyelő megtekintésére használt [tevékenységi naplóit](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) és [gazdaszintű metrikák](../monitoring-and-diagnostics/monitoring-overview-metrics.md). Hozzáadhat a [Azure Diagnostics bővítmény](../virtual-machines/linux/tutorial-monitoring.md#install-diagnostics-extension) ahhoz, hogy a vendég operációs rendszer adatainak begyűjtése virtuális gépekhez. Ezután létrehozhat [riasztási szabályok](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md) proaktív elkészültéről, például a processzor kihasználtságát és a memória alapvető metrikák kereszt-küszöbértékeket.

Egy üzleti alkalmazás futó virtuális gépek kapcsolatos további adatokat gyűjthet, [Naplóelemzési munkaterület létrehozása és a Virtuálisgép-bővítmény engedélyezése](../log-analytics/log-analytics-quick-collect-azurevm.md) minden egyes számítógépen. Konfigurálja a [különböző adatforrások gyűjteménye](../log-analytics/log-analytics-data-sources.md) az alkalmazáshoz és [nézeteket hozhat létre](../log-analytics/log-analytics-view-designer.md) számára, a napi művelet és a teljesítményt. Majd [riasztási szabályok létrehozásához](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md) értesítse arról, ha adott hibaesemények érkezik. 

Folyamatosan a telepített ügynök állapotának figyelésére, akkor vegye fel a [ügyfélállapot-kezelési megoldás](../operations-management-suite/oms-solution-agenthealth.md). Ahhoz, hogy további betekintést az alkalmazást, akkor [adja hozzá a függőségi ügynök](../operations-management-suite/operations-management-suite-service-map-configure.md) ahhoz, hogy azok hozzáadása a virtuális gépek [Szolgáltatástérkép](../operations-management-suite/operations-management-suite-service-map.md). Szolgáltatástérkép felderíti a folyamatokat, és más szolgáltatásokkal gépek közötti kapcsolatok azonosítja. 

Egy jelentett leállás után a Szolgáltatástérkép az adott azon gépek azonosításához, a problémát tapasztalt Törvényszéki végrehajtásához használhatja. Ezután létrehozhat egy [a Log Analytics-adatok lekérdezése](../log-analytics/log-analytics-log-search-new.md) a jövőben a probléma azonosításához. És proaktív értesítést küldenek, ha a feltétel észleli a riasztási szabályt létrehozni.



## <a name="next-steps"></a>További lépések
További információ:

* [Az ignite-on 2016 videó az Azure figyelő](https://myignite.microsoft.com/videos/4977).
* [Ismerkedés az Azure-figyelő](monitoring-get-started.md).
* [Az Azure Diagnostics](../azure-diagnostics.md) Ha próbál-e a felhőalapú szolgáltatás, a virtuális gép, a virtuálisgép-méretezési csoport vagy az Azure Service Fabric-alkalmazás a problémák diagnosztizálásához.
* [Az Application Insights](https://azure.microsoft.com/documentation/services/application-insights/) Ha próbál az App Service web app alkalmazásban problémák diagnosztizálásához.
* [Naplófájl Analytics](https://azure.microsoft.com/documentation/services/log-analytics/) összegyűjtött figyelési adatok elemzéséhez.
