---
title: "A PostgreSQL egy kiszolgálóhoz az Azure-adatbázis visszaállítása"
description: "Ez a cikk ismerteti az Azure portál használatával PostgreSQL egy kiszolgálóhoz az Azure-adatbázis visszaállítása."
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 7607a3e60eec39de61c785b8ff75a9f11fa02d0c
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/02/2018
---
# <a name="how-to-back-up-and-restore-a-server-in-azure-database-for-postgresql-using-the-azure-portal"></a>Készítsen biztonsági másolatot, és a kiszolgáló Azure-adatbázis visszaállítása az Azure portál használatával PostgreSQL

## <a name="backup-happens-automatically"></a>Automatikusan megtörténik a biztonsági mentés
PostgreSQL-kiszolgálók Azure-adatbázis biztonsági mentése rendszeresen visszaállítási szolgáltatások engedélyezése. Ezzel a szolgáltatással is visszaállíthatja a kiszolgáló és az adatbázisok egy korábbi-időpontban, egy új kiszolgálóra.

## <a name="set-backup-configuration"></a>Biztonsági mentési konfigurációjának beállítása

Vagy helyileg redundáns vagy földrajzilag redundáns történő biztonsági mentésekre a kiszolgáló létrehozásakor, a kiszolgáló konfigurálása közötti választás a **Tarifacsomagot** ablak.

> [!NOTE]
> Miután a kiszolgáló akkor jön létre, milyen típusú redundancia rendelkezik, helyileg redundáns földrajzilag redundáns vs nem állítható át.
>

Az Azure portálon keresztül a kiszolgáló létrehozása során a **Tarifacsomagot** időszak, ahol ki kell választania vagy **helyileg redundáns** vagy **földrajzilag redundáns** biztonsági mentés a kiszolgáló. Ebben az ablakban is, ahol ki kell választania a **biztonsági mentés megőrzési időszak** – mennyi ideig (a napok száma) a kiszolgáló biztonsági másolatait tárolni szeretné.

   ![Tarifacsomag - válassza ki a biztonsági mentési redundancia](./media/howto-restore-server-portal/pricing-tier.png)

Ezeket az értékeket létrehozásakor beállításával kapcsolatos további információkért lásd: a [PostgreSQL server gyors üzembe helyezés az Azure Database](quickstart-create-server-database-portal.md).

A kiszolgáló biztonsági másolatok megőrzésének időtartama módosítható a következő lépéseit:
1. Jelentkezzen be az [Azure Portalra](https://portal.azure.com/).
2. Válassza ki az Azure-adatbázis PostgreSQL-kiszolgáló. Ez a művelet megnyitja az **áttekintése** lap.
3. Válassza ki **Tarifacsomagot** a menüben a **beállítások**. A csúszkával módosítható a **biztonsági mentés megőrzési időszak** a 7-es vagy 35 nap közötti, igény szerint.
Az alábbi képernyőképen az értékre nőtt 34 nap.
![Biztonsági mentés megőrzési időszak nőtt](./media/howto-restore-server-portal/3-increase-backup-days.png)

4. Kattintson a **OK** a módosítás megerősítéséhez.

A biztonsági mentés megőrzési időszak irányító milyen távolságban az idő lekérhető pont időponthoz kötött visszaállítás, mivel elérhető biztonsági másolatokat alapul. A következő szakaszban leírt további pont időponthoz kötött visszaállítás. 

## <a name="point-in-time-restore-in-the-azure-portal"></a>Az Azure portálon pont időponthoz kötött visszaállítás
Azure PostgreSQL-adatbázishoz lehetővé teszi a kiszolgáló helyreállítása vissza egy-időpontban, és azokat a kiszolgáló egy új példányát. A következő új kiszolgáló használata az adatok helyreállításához, vagy az ügyfél alkalmazásokat az új kiszolgálóra mutasson.

Például ha egy táblázat véletlenül dobva délben ma, akkor sikerült visszaállítása előtt déltől idő, a hiányzó tábla és adatokat lekérdezni az új példányt a kiszolgáló. Időpontban visszaállítást a kiszolgálón szint, nem az adatbázis szintjén.

Az alábbi lépéseket a minta kiszolgáló visszaállítása a-időpontban:
1. Az Azure-portálon válassza ki az Azure-adatbázis PostgreSQL-kiszolgáló. 

2. A kiszolgáló eszköztárán **áttekintése** lapon jelölje be **visszaállítása**.

   ![Azure-adatbázis PostgreSQL - áttekintése - Visszaállítás gomb](./media/howto-restore-server-portal/2-server.png)

3. Töltse ki a visszaállítási űrlapot a szükséges adatokkal:

   ![PostgreSQL - visszaállítási információk az Azure-adatbázis ](./media/howto-restore-server-portal/3-restore.png)
  - **Visszaállítási pont**: válassza ki a-időpontban a visszaállítani kívánt.
  - **Célkiszolgáló**: Adjon meg egy nevet az új kiszolgálóra.
  - **Hely**: a régió nem választhatók ki. Alapértelmezés szerint az ugyanaz, mint a forráskiszolgálón.
  - **A tarifacsomag**: ezek a paraméterek nem módosítható, ha a pont időponthoz kötött visszaállítás során. Ugyanaz, mint a forráskiszolgálóé. 

4. Kattintson a **OK** visszaállítása a kiszolgálón történő visszaállításához, egy-időpontban. 

5. Miután a visszaállítás befejezését, keresse meg az új kiszolgálóra, amely az adatok helyreállt a várt módon ellenőrzése jön létre.

>[!Note]
>A időpontban visszaállítás által létrehozott új kiszolgáló rendelkezik-e az azonos felügyeleti bejelentkezési nevet és jelszót, amely a meglévő kiszolgáló, a-időpontban érvényes választott. Módosíthatja a jelszót az új kiszolgáló **áttekintése** lap.

## <a name="next-steps"></a>További lépések
- További információ a szolgáltatás [biztonsági mentések](concepts-backup.md).
- További információ [az üzletmenet folytonossága](concepts-business-continuity.md) beállítások.
