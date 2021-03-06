


**Utolsó dokumentálja a frissítés**: március 6 10:00 AM csendes-óceáni TÉLI.

A legutóbbi közzétételének egy [CPU biztonsági rések új osztály](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180002) néven ismert spekulatív végrehajtási ügyféloldali csatorna támadások, így további egyértelműség kérő ügyfelek kérdéseket.  

Az infrastruktúra, amely Azure elkülöníti egymástól ügyfél munkaterheléseinek védett.  Ez azt jelenti, hogy más Azure-on futó ügyfelek nem támadható meg az alkalmazás biztonsági rések használatával.

> [!NOTE] 
> Késedelmes. február 2018 Intel Corporation közzétett frissített [mikrokód változat útmutatást](https://newsroom.intel.com/wp-content/uploads/sites/11/2018/03/microcode-update-guidance.pdf) azok mikrokód kiadások, amely stabilitást és nyilvánosságralegutóbbibiztonságirésekelleniállapotának[Google projekt nulla](https://googleprojectzero.blogspot.com/2018/01/reading-privileged-memory-with-side.html). Az Azure-beli függeszthetők a megoldást [2018. január 3](https://azure.microsoft.com/en-us/blog/securing-azure-customers-from-cpu-vulnerability/) Intel mikrokód frissítés nem érinti. Microsoft erős megoldást már vezetnek be Azure-ügyfél más Azure-bérlőt elleni védelméhez.  
>
> Intel mikrokód címek variant 2 színkép ([CVE-2017-5715](https://www.cve.mitre.org/cgi-bin/cvename.cgi?name=2017-5715)), amely csak akkor alkalmazható futtató megosztott vagy nem megbízható munkaterhelések belül a virtuális gépek Azure-on lánctámadások elleni védelem érdekében. A mérnökök teszteli a stabilitás minimalizálása érdekében a mikrokód, mielőtt elérhetővé tétele az Azure-ügyfél számára a részeinek teljesítményére.  Kevés ügyfelet a virtuális gépeken nem megbízható alkalmazásokat és szolgáltatásokat futtathatnak, mivel a legtöbb ügyfél nem kell ahhoz, hogy ez a funkció egyszer kiadott. 
>
> További információ áll rendelkezésre a ezen a lapon frissülni fog.  






## <a name="keeping-your-operating-systems-up-to-date"></a>Az operációs rendszer frissítése

Bár egy operációsrendszer-frissítést nem szükséges a más felhasználóktól Azure-on futó Azure-on futó alkalmazások elkülönítése, ez a beállítás mindig ajánlott eljárás az operációsrendszer-verziók naprakész állapotban tartása érdekében. 

Az a következő ajánlatok az alábbiakban az operációs rendszer frissítésére a javasolt művelet: 

<table>
<tr>
<th>Az ajánlat</th> <th>Javasolt művelet </th>
</tr>
<tr>
<td>Azure Cloud Services </td>  <td>Automatikus frissítés engedélyezéséhez, vagy ellenőrizni kell, hogy a legújabb vendég operációs rendszer.</td>
</tr>
<tr>
<td>Az Azure Linux virtuális gépek</td> <td>Frissítések telepítése az operációs rendszer szolgáltatótól, ha elérhető. </td>
</tr>
<tr>
<td>A Windows Azure virtuális gépek </td> <td>Győződjön meg arról, hogy egy támogatott víruskereső alkalmazást futtatja, az operációs rendszer frissítéseinek telepítése előtt. Lépjen kapcsolatba a víruskereső szoftver gyártójával kompatibilitási információt.<p> Telepítse a [január a biztonság összegzése](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180002). </p></td>
</tr>
<tr>
<td>Egyéb Azure PaaS szolgáltatások</td> <td>Nincs szükség a ezeket a szolgáltatásokat használó ügyfelek számára. Azure szolgáltatás automatikusan az operációsrendszer-verziók naprakész. </td>
</tr>
</table>

## <a name="additional-guidance-if-you-are-running-untrusted-code"></a>További útmutatás, ha a nem megbízható kód futtatja 

Nincsenek további felhasználói beavatkozásra van szükség, kivéve, ha futtatja a nem megbízható kód. Ha engedélyezi a kódot, amely nem bízik meg (például engedélyezi egy bináris vagy kódrészletet, amely, majd hajtsa végre a felhőben az alkalmazáson belül feltölteni az ügyfelek), akkor a következő további lépéseket kell tenni.  


### <a name="windows"></a>Windows 
Ha Windows használ, és nem megbízható kód üzemeltet, is ki kell jelölni egy Windows-szolgáltatás néven Kernel virtuális cím (KVA) Shadowing, így további védelmet spekulatív végrehajtási ügyféloldali csatorna sebezhetőségekkel szemben. Ez a funkció alapértelmezés szerint ki van kapcsolva, és befolyásolhatja a teljesítményt, ha engedélyezve van. Hajtsa végre a [Windows Server KB4072698](https://support.microsoft.com/help/4072698/windows-server-guidance-to-protect-against-the-speculative-execution) utasításokat a védelem engedélyezése a kiszolgálón. Ha Azure Felhőszolgáltatások futtat, győződjön meg arról, hogy futnak-e WA-VENDÉG-operációsrendszer-5.15_201801-01 vagy WA-VENDÉG-OS-4.50_201801-01 (rendszertől érhető el a 2018 január 10) vagy a engedélyezése a beállításjegyzék-kulcs egy indítási feladattal.


### <a name="linux"></a>Linux
Ha Linux használunk, és nem megbízható kód üzemeltető, frissítse is Linux egy újabb verzióra, amely rendszermag lap-tábla elkülönítési (KPTI), amely elválasztja a felhasználói felület tartozó a rendszermag által használt lap táblák. Ezek azok mérséklési Linux operációs rendszert futtató frissíteni, és ha elérhető terjesztési szolgáltatótól származó. Az operációs rendszer szolgáltató segítségével megállapíthatja, hogy e védelmet engedélyezett vagy letiltott alapértelmezés szerint.



## <a name="next-steps"></a>További lépések

További tudnivalókért lásd: [CPU biztonsági rés védelmét biztosító Azure-ügyfelek](https://azure.microsoft.com/blog/securing-azure-customers-from-cpu-vulnerability/).
