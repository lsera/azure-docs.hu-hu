---
title: "Az Azure Cosmos Adatbázisba konzisztenciaszintek |} Microsoft Docs"
description: "Azure Cosmos-adatbázis segítségével egyenleg végleges konzisztencia, a rendelkezésre állás és a késleltetés kompromisszumot öt konzisztenciaszintek rendelkezik."
keywords: "az azure végleges konzisztencia cosmos db, azure, a Microsoft azure"
services: cosmos-db
author: mimig1
manager: jhubbard
editor: cgronlun
documentationcenter: 
ms.assetid: 3fe51cfa-a889-4a4a-b320-16bf871fe74c
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/12/2018
ms.author: mimig
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c3bd28316e3d2e7596021d6964594002d47d160a
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 02/13/2018
---
# <a name="tunable-data-consistency-levels-in-azure-cosmos-db"></a>Az Azure Cosmos Adatbázisba hangolható konzisztencia szintek
Azure Cosmos-adatbázis úgy van kialakítva egészen az alapoktól fel a globális terjesztési szem előtt az összes adatmodell. Előre jelezhető késés garanciák és több jól meghatározott laza konzisztencia modellek tervezték. Jelenleg az Azure Cosmos DB biztosít öt konzisztenciaszintek: erős, kötött elavulás, munkamenet, egységes előtag, és végleges. Kötött elavulás, munkamenet, egységes előtag és végleges biztosan néven "laza konzisztencia modellek" azok adjon meg kevesebb konzisztencia erős, mint amely a legtöbb rendelkezésre állású egységes modellje. 

Emellett a **erős** és **végleges konzisztencia** modellek gyakran által nyújtott elosztott adatbázisok Azure Cosmos DB három további gondosan kódolt és operationalized konzisztencia modellt kínál:  **a kötött elavulási**, **munkamenet**, és **konzisztens előtag**. Mindegyik konzisztencia szinten hasznosságát ellenőrzött valós használati esetek ellen. Együttesen ezek öt konzisztenciaszintek lehetővé teszi végre jól indokolt kompromisszumot konzisztencia, a rendelkezésre állás és a késleltetés között. 

## <a name="distributed-databases-and-consistency"></a>Elosztott adatbázisok és a konzisztencia
Kereskedelmi elosztott adatbázisok két kategóriába sorolhatók: adatbázisok, amelyek egyáltalán nem képes a jól meghatározott hangelemzés konzisztencia lehetőségeket, és két szélsőséges programozhatóság lehetőség (erős és a végleges konzisztencia) adatbázisok. 

Az előbbi replikációs protokolljainak részletessége komoly fejfájást okozhat az alkalmazásfejlesztőknek, ráadásul rajtuk múlik az egyensúlyi állapot megteremtése a konzisztencia, a rendelkezésre állás, a késés és az átviteli sebesség között. Az utóbbinál pedig komoly terhet jelent a választás a két véglet közül. A kutatási adatok bősége és a több mint 50 konzisztenciamodellre vonatkozó javaslatok ellenére az elosztott adatbázisok közössége eleddig nem volt képes az erős és végleges konzisztencián túl szabványosítani a konzisztenciaszinteket. Cosmos DB lehetővé teszi a fejlesztők számára öt jól meghatározott konzisztencia modellek mentén konzisztencia pontszámot – erős, kötött elavulás, választhat [munkamenet](http://dl.acm.org/citation.cfm?id=383631), egységes előtag, és végleges. 

![Az Azure Cosmos DB több jól definiált (enyhén korlátozott) konzisztenciamodellt is elérhetővé tesz](./media/consistency-levels/five-consistency-levels.png)

Az alábbi táblázat azt illusztrálja, hogy melyik konzisztenciaszint milyen garanciákat biztosít.
 
**Konzisztenciaszintek és garanciák**

| Konzisztenciaszint | Garanciák |
| --- | --- |
| Erős | Linearizability. Olvasási garantáltan a legfrissebb elemet adja vissza.|
| Kötött elavulás | Konzisztens előtag. Az olvasások k verzióval vagy t időközzel követik az írásokat |
| Munkamenet   | Konzisztens előtag. Monoton olvasások, monoton írások, írás utáni visszaolvasás, beolvasás utáni írás |
| Konzisztens előtag | A visszaadott frissítések között az összes frissítés néhány egymást követő verziója szerepel, mindig megfelelő sorrendben |
| Végleges  | Nem sorrendben történő olvasások |

Konfigurálhatja az alapértelmezett konzisztenciaszintet Cosmos DB-fiókjában (és később felülbírálhatja a konzisztenciát egy adott olvasási kérésen). Belsőleg az alapértelmezett konzisztencia szint a partíció készletek, amelyek esetleg régiók adatainak vonatkozik. Azure Cosmos DB bérlők használata a munkamenet-konzisztencia készül 73 % és 20 % inkább a kötött elavulási. Azure Cosmos DB ügyfél körülbelül 3 % kísérletezhet különböző konzisztenciaszintek kezdetben előtt az adott konzisztencia téve az alkalmazásokban a rendezése. Azure Cosmos DB bérlők csak 2 %-át egy kérelem alapon konzisztenciaszintek bírálja felül. 

A Cosmos DB olvasási kiszolgált munkamenetben, egységes előtag és végleges konzisztencia kétszer szerint olcsó, olvasások erős, vagy a kötött elavulási konzisztencia. Cosmos DB rendelkezik iparágvezető átfogó SLA-k, beleértve a rendelkezésre állás, az átviteli sebesség és a késleltetés mellett konzisztencia biztosítja. Azure Cosmos DB védelmi funkciókat alkalmaz a [linearizability ellenőrző](http://dl.acm.org/citation.cfm?id=1806634), amely a szolgáltatás telemetriai folyamatosan működik, és nyilvánosan jelentéseket bármely konzisztencia megsértését jelenti. A kötött elavulási, az Azure Cosmos DB figyeli, és jelenti a bármely k és t határainak megsértését jelenti. Az összes öt laza konzisztenciaszintek, Azure Cosmos DB is jelent a [probabilistically kötött elavulási metrika](http://dl.acm.org/citation.cfm?id=2212359) előfizetve.  

## <a name="service-level-agreements"></a>Szolgáltatásszint-szerződések

Az Azure Cosmos DB nyújt átfogó 99,99 % [SLA-k](https://azure.microsoft.com/support/legal/sla/cosmos-db/) mely garancia átviteli, a konzisztencia, a rendelkezésre állási és a késleltetés az Azure Cosmos DB adatbázis-fiókokat az öt konzisztencia egyik konfigurált egy Azure régió hatóköre szinteket, vagy több Azure-régiók, sem a négy laza konzisztenciaszintek konfigurált átfedés adatbázis fiókok. Továbbá függetlenül a választott egy konzisztenciaszint, Azure Cosmos DB kínál egy 99.999 % SLA olvasási rendelkezésre álláshoz két vagy több Azure-régiók átfedés adatbázis fiókok.

## <a name="scope-of-consistency"></a>Konzisztencia hatóköre
A lépésköz legyen konzisztencia egy felhasználói kérelem hatókörét. Egy írási kérelem egy insert, replace, upsert vannak rendelve, vagy törölhető a tranzakció. Írási műveleteket, mint a olvasás/lekérdezés tranzakció is egy felhasználói kérelem hatókörét. A felhasználó előfordulhat, hogy szükséges kattintva megjelenítheti a nagy keresztül eredménykészlet, az átfedés több partíció, de egyes olvassa el a tranzakció egyetlen oldalra hatókörű, és a belül egyetlen partícióra kiszolgált.

## <a name="consistency-levels"></a>Konzisztenciaszintek
A Cosmos DB fiókját az adatbázis-fiókra, amely az összes gyűjteményt (és adatbázisok) egy alapértelmezett konzisztenciaszint adhat meg. Alapértelmezés szerint minden olvasási és a felhasználó által definiált erőforrások állították ki lekérdezések használja a következő adatbázisfiókot beállított alapértelmezett konzisztencia szint. A meghatározott olvasás/lekérdezési kérelmek használatával az egyes támogatott API-k konzisztenciaszint is enyhíteni. Öt típusa van konzisztenciaszintek az Azure Cosmos DB replikációs protokoll által támogatott, adjon meg egy egyértelmű kompromisszum között adott konzisztencia biztosítja, és a teljesítmény, ebben a szakaszban leírtak szerint.

**Erős**: 

* Erős konzisztenciát biztosít egy [linearizability](https://aphyr.com/posts/313-strong-consistency-models) garantálja az olvasási garantált, hogy a legfrissebb elemet adja vissza. 
* Az erős konzisztencia biztosítja, hogy egy írási csak látható után már tartósan által a replikák többsége kvórum. Írás már vagy szinkron módon tartósan használta az elsődleges, mind a kvórum több másodlagos adatbázist, vagy azt végrehajtása megszakadt. Egy olvasási mindig nyugtázta a legtöbb, olvassa el a kvórum, az ügyfél egy nem véglegesített vagy részleges írási soha nem látható, és mindig olvassa el a legújabb nyugtázott írási garantáltan. 
* Az erős konzisztencia használatára konfigurált Azure Cosmos DB fiókok nem társítható egynél több Azure-régiót Azure Cosmos DB fiókkal.  
* Egy olvasási művelet költsége (a [egységek kérelem](request-units.md) felhasznált) erős konzisztencia értéke nagyobb, mint a munkamenet és végleges, de ugyanaz, mint a kötött elavulási.

**A kötött elavulási**: 

* A kötött elavulási konzisztencia biztosítja, hogy az lehet, hogy legfeljebb olvasási késés által írt *K* verziója vagy az előtagok elem vagy *t* időköz. 
* Ezért kiválasztása kötött elavulási, a "elavulási" konfigurálható kétféleképpen: verziók száma *K* az elem, amely olvasási verziót használják, és az időtartam alatt a írási *t* 
* A kötött elavulási ajánlatok teljes globális sorrendjét kivételével a "elavulási időszakban." A monoton olvasási garanciák léteznek belül és kívül egyaránt a "elavulási ablak." régión belül 
* A kötött elavulási konzisztencia erősebb garancia munkamenet, konzisztens-előtag vagy végleges konzisztencia biztosítja. Globálisan elosztott alkalmazásokhoz javasoljuk, használja a kötött elavulási forgatókönyvek hova kerüljenek az erős konzisztencia rendelkezik, de is érdemes a rendelkezésre állás 99,99 % és kis késleltetése.   
* A kötött elavulási konzisztencia konfigurált Azure Cosmos DB fiókok tetszőleges számú Azure-régiók társíthatja Azure Cosmos DB fiókjuk. 
* Egy olvasási művelet (tekintetében felhasznált RUs) költsége a kötött elavulási, azaz magasabb-munkamenet és végleges konzisztencia, de ugyanaz, mint az erős konzisztencia.

**Munkamenet**: 

* A Globális konzisztenciahiba modellek erős és a kötött elavulási konzisztencia szintek által kínált, eltérően a munkamenet-konzisztencia ügyfél hatókörét. 
* A munkamenet-konzisztencia minden olyan esetekben, ahol van szó egy eszközre vagy felhasználóra vonatkozik-munkamenetet, mivel azt biztosítja, hogy monoton olvasási, monoton írás és Olvasás saját írási műveletek (RYW) biztosítja, hogy az ideális. 
* Munkamenet-konzisztencia kiszámítható konzisztencia biztosít egy munkamenet esetében, és a maximális átviteli sebesség olvasási, miközben a legkisebb késés írási és olvasási műveletek. 
* A munkamenet-konzisztencia konfigurált Azure Cosmos DB fiókok tetszőleges számú Azure-régiók társíthatja Azure Cosmos DB fiókjuk. 
* Egy olvasási művelet (tekintetében felhasznált RUs) költségét munkamenet konzisztenciaszint nem kisebb, mint erős és a kötött elavulási, de több mint végleges konzisztencia.

<a id="consistent-prefix"></a>
**Egységes előtag**: 

* Egységes előtag garantálja, hogy bármilyen további írás hiányában a csoportban lévő replikák végül össze vonva. 
* Egységes előtag biztosítja, hogy az, hogy olvasási műveletek nem veszi észre üzemen kívüli írási műveleteket. Ha sorrendben végrehajtott írási műveletek `A, B, C`, ügyfél látja el, majd `A`, `A,B`, vagy `A,B,C`, de soha nem megfelelő sorrendben például `A,C` vagy `B,A,C`.
* Egységes előtag konzisztencia konfigurált Azure Cosmos DB fiókok tetszőleges számú Azure-régiók társíthatja Azure Cosmos DB fiókjuk. 

**Végső**: 

* Végleges konzisztencia biztosítja, hogy bármilyen további írás hiányában a csoportban lévő replikák végül össze vonva. 
* Végleges konzisztencia formája, amely a leggyengébb konzisztencia, ahol az adott ügyfél lehet, hogy a régebbi, mint a korábban látott értékek le.
* Végleges konzisztencia nyújtja a leggyengébb olvasási konzisztenciát, de a legkisebb mértékű késleltetést biztosítja mind az Olvasás, mind az írás.
* A végleges konzisztencia konfigurált Azure Cosmos DB fiókok tetszőleges számú Azure-régiók társíthatja Azure Cosmos DB fiókjuk. 
* Egy olvasási művelet (tekintetében felhasznált RUs) költsége a végleges konzisztencia szintje a legalacsonyabb az Azure Cosmos DB konzisztenciaszintek.

## <a name="configuring-the-default-consistency-level"></a>Az alapértelmezett konzisztencia szint konfigurálása
1. Az a [Azure-portálon](https://portal.azure.com/), az ugrósávon kattintson **Azure Cosmos DB**.
2. Az a **Azure Cosmos DB** lapon, válassza ki a következő adatbázisfiókot módosításához.
3. Kattintson a fiók lapon **alapértelmezett konzisztencia**.
4. Az a **alapértelmezett konzisztencia** lapon válassza ki az új konzisztencia szintet, majd kattintson **mentése**.
   
    ![Képernyőfelvétel a beállítások ikonra, és alapértelmezett konzisztencia bejegyzés kiemelve](./media/consistency-levels/database-consistency-level-1.png)

## <a name="consistency-levels-for-queries"></a>A lekérdezések konzisztenciaszintek
Alapértelmezés szerint a felhasználó által definiált erőforrások a lekérdezések konzisztenciaszint megegyezik a konzisztenciaszint olvasása. Alapértelmezés szerint az index frissítése szinkron módon minden insert, replace vagy a Cosmos DB tárolóhoz elem törlése. Ez lehetővé teszi, hogy a konzisztencia szinttel azonos pont olvasások tiszteletben lekérdezések. Míg Azure Cosmos DB írási optimalizált, és írási műveletek, a szinkron index karbantartási és a kiszolgáló konzisztens lekérdezések tartós köteteket támogatja, egyes gyűjtemények indexét lazily frissítésére is konfigurálhat. További Lusta indexelő a írási hanghatások és ideális tömeges adatfeldolgozást forgatókönyvek esetén, ha egy munkaterhelés elsősorban olvasási műveleteket.  

| Az indexelő mód | Olvasások | Lekérdezések |
| --- | --- | --- |
| A CONSISTENT (alapértelmezett) |Válassza ki az erős, kötött elavulási, munkamenet, egységes előtag, vagy végleges |Válassza ki az erős, kötött elavulás, munkamenet, vagy végleges |
| Lassú |Válassza ki az erős, kötött elavulási, munkamenet, egységes előtag, vagy végleges |Végleges |
| Nincs |Válassza ki az erős, kötött elavulási, munkamenet, egységes előtag, vagy végleges |Nem alkalmazható |

Mint az olvasási kéréseket csökkenthető a konzisztenciaszint minden API-ban meghatározott lekérdezési kérelem.

## <a name="consistency-levels-for-the-mongodb-api"></a>A MongoDB API konzisztenciaszintek

Azure Cosmos-adatbázis jelenleg két konzisztencia beállításokkal, erős és végleges, 3.4-es verziójú MongoDB valósítja meg. Mivel Azure Cosmos DB multi-api, a konzisztencia beállítások a fiók szintjén és a konzisztencia kényszerítése minden API vezérli.  MongoDB 3.6, amíg nincs egy munkamenet-konzisztencia fogalma, ha a MongoDB API-fiók használata a munkamenet-konzisztencia, a konzisztencia van vissza a végleges MongoDB API-k használata esetén. Fiók MongoDB API-a-saját-olvasható garancia van szüksége, ha a fiók alapértelmezett konzisztencia szintje meg erős, vagy a kötött elavulási.

## <a name="next-steps"></a>További lépések
Ha azt szeretné, további elolvasása konzisztenciaszintek és kompromisszumot elvégzéséhez, a következőket javasoljuk:

* Doug Terry. A replikált adatok konzisztencia viszonylag baseball (videó) keresztül.   
  [https://www.youtube.com/watch?v=gluIh8zd26I](https://www.youtube.com/watch?v=gluIh8zd26I)
* Doug Terry. A replikált adatok konzisztencia viszonylag baseball keresztül.   
  [http://research.microsoft.com/pubs/157411/ConsistencyAndBaseballReport.pdf](http://research.microsoft.com/pubs/157411/ConsistencyAndBaseballReport.pdf)
* Doug Terry. Munkamenet garanciák gyengén konzisztens replikált adatok.   
  [http://dl.acm.org/citation.cfm?id=383631](http://dl.acm.org/citation.cfm?id=383631)
* Daniel Abadi. Konzisztencia mellékhatásokkal Modern elosztott adatbázis rendszerek kialakításában: CAP csak a szövegegység része ".   
  [http://computer.org/csdl/mags/co/2012/02/mco2012020037-abs.html](http://computer.org/csdl/mags/co/2012/02/mco2012020037-abs.html)
* Peter Bailis, Shivaram Venkataraman, Michael J. tw, Joseph M. Hellerstein, adatmegőrzési Stoica. Probabilisztikus kötött elavulási (PBS) a gyakorlati részleges határozatképességére.   
  [http://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf](http://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf)
* Wernernek Vogels. Végső konzisztens - javított változat.    
  [http://allthingsdistributed.com/2008/12/eventually_consistent.html](http://allthingsdistributed.com/2008/12/eventually_consistent.html)
* MONi Naor, Avishai gyapjú, a betöltés, a kapacitás és kvórum rendszerek, a számítástechnikai, v.27 n.2, p.423 447, 1998. április SIAM napló rendelkezésre állását.
  [http://epubs.siam.org/doi/abs/10.1137/S0097539795281232](http://epubs.siam.org/doi/abs/10.1137/S0097539795281232)
* Sebastian Burckhardt, Chris Dern, Macanal Musuvathi, Roy Tan, Line-up: a teljes és automatikus linearizability-ellenőrző eljárás programozási nyelv tervezési és megvalósítási, június 05-10-es, 2010, Toronto, Ontario, 2010-es ACM SIGPLAN konferencia Kanadában [doi > 10.1145/1806596.1806634] [http://dl.acm.org/citation.cfm?id=1806634](http://dl.acm.org/citation.cfm?id=1806634)
* Peter Bailis, Shivaram Venkataraman, Michael J. tw, Joseph M. Hellerstein adatmegőrzési Stoica Probabilistically kötött elavulási gyakorlati részleges határozatképességére, a VLDB dotációs, még az v.5 n.8, p.776 787, 2012 áprilisi eljárásai az [http:// DL.ACM.org/CITATION.cfm?ID=2212359](http://dl.acm.org/citation.cfm?id=2212359)
