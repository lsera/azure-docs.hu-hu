---
title: "A felhő (Python) - málna Pi csatlakozzon az Azure IoT Hub Raspberry Pi |} Microsoft Docs"
description: "Megtudhatja, hogyan kell beállítania, és Azure IoT-központ málna Pi adatokat küldeni az Azure felhőalapú platform ebben az oktatóanyagban málna Pi csatlakozni."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "az Azure iot raspberry pi, raspberry pi iot-központ, raspberry pi adatokat küldött a felhőben, raspberry pi felhőbe"
ms.service: iot-hub
ms.devlang: python
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/31/2017
ms.author: xshi
ms.openlocfilehash: 1b1a9dc960846cbc15ce09d0fd106e1492937439
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/11/2017
---
# <a name="connect-raspberry-pi-to-azure-iot-hub-python"></a>Raspberry Pi csatlakozni az Azure IoT Hub (Python)

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

Ebben az oktatóanyagban akkor először tanulás alapjainak málna Pi Raspbian futtató használata. Majd megtudhatja, hogyan kapcsolódhat zökkenőmentesen az eszközök a felhőbe [Azure IoT Hub](iot-hub-what-is-iot-hub.md). Windows 10 IoT minta, látogasson el a [Windows fejlesztői központ](http://www.windowsondevices.com/).

Még nem rendelkezik egy csomagot? Próbálja [málna Pi online szimulátor](iot-hub-raspberry-pi-web-simulator-get-started.md). Vagy egy új csomag vásárlása [Itt](https://azure.microsoft.com/develop/iot/starter-kits).

## <a name="what-you-do"></a>Mit

* Létrehoz egy IoT-központot.
* Eszköz regisztrálása az IoT hub a pi.
* A telepítő Raspberry Pi.
* Futtassa a mintaalkalmazást érzékelő adatokat küldeni az IoT hub Pi.

Az IoT-központ az Ön által létrehozott málna Pi csatlakozni. Majd futtassa a mintaalkalmazást a hőmérséklet és a páratartalom adatokat gyűjteni BME280 érzékelő Pi. Végezetül az érzékelő adatokat küldött az IoT hub.

## <a name="what-you-learn"></a>Ismertetett témák

* Megtudhatja, hogyan hozzon létre egy Azure IoT-központot, és az új eszköz kapcsolati karakterláncot.
* Hogyan BME280 érzékelő Pi kapcsolódni.
* Megtudhatja, hogyan futtatja a mintaalkalmazás Pi érzékelő adatok gyűjtéséért felelős ügyfélfeladatot.
* Hogyan érzékelő adatokat küldeni az IoT hub.

## <a name="what-you-need"></a>Mi szükséges

![Mi szükséges](media/iot-hub-raspberry-pi-kit-c-get-started/0_starter_kit.jpg)

* A Pi 2 málna vagy málna Pi 3 kártya.
* Aktív Azure-előfizetés. Ha az Azure-fiók nem rendelkezik [hozzon létre egy Azure próbafiókot](https://azure.microsoft.com/free/) csak néhány perc múlva.
* A figyelő egy USB-billentyűzet és egér Pi-hez.
* A Mac vagy a Windows vagy Linux rendszerű számítógép.
* Az internethez.
* Egy 16 GB-os vagy újabb microSD-kártyán.
* Egy USB-SD adapter vagy microSD-kártyán írása a microSD-kártyán operációsrendszer-képet.
* Egy 5-volt 2-amp tápegység a 6-mértékű kiszolgálóhasználat micro USB-kábellel.

A következő elemek nem kötelező:

* Az összeállított Adafruit BME280 hőmérséklet, a terhelés, és a páratartalom érzékelő.
* Egy breadboard.
* 6/aljzat átkötés fenyegetéseknek.
* Egy szórt 10 mm LED-jét.


> [!NOTE] 
Ezek az elemek nem kötelező, mert a kód a minta támogatási szimulált érzékelőadatait.


[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="set-up-raspberry-pi"></a>Málna Pi beállítása

### <a name="install-the-raspbian-operating-system-for-pi"></a>A Pi a Raspbian operációs rendszer telepítése

Készítse elő a microSD-kártyán Raspbian kép telepítéséhez.

1. Töltse le a Raspbian.
   1. [Töltse le az asztallal Raspbian Jessie](https://www.raspberrypi.org/downloads/raspbian/) (a .zip-fájlt).
   1. Bontsa ki a Raspbian lemezképet a számítógép egyik mappájába.
1. A microSD-kártyán Raspbian telepítése.
   1. [Töltse le és telepítse a Etcher SD-kártya író segédprogram](https://etcher.io/).
   1. Futtassa a Etcher, és az 1. lépésben válassza ki a kibontott Raspbian kép.
   1. Jelölje ki a microSD-kártyát meghajtót. Vegye figyelembe, hogy Etcher előfordulhat, hogy már választott ki a megfelelő meghajtó.
   1. Kattintson a microSD-kártyán Raspbian telepítése Flash.
   1. Eltávolítja a számítógépről a microSD-kártyán, ha a telepítés befejeződött. Biztonságos a microSD-kártyán közvetlenül eltávolítani, mert Etcher automatikusan kiadása vagy leválasztja a microSD-kártyán befejezése után is.
   1. A microSD-kártyán beszúrása Pi.

### <a name="enable-ssh-and-i2c"></a>SSH- és I2C engedélyezése

1. Pi csatlakozni a monitor, billentyűzet és egér. Indítsa el a Pi, majd jelentkezzen be Raspbian `pi` felhasználónevet és `raspberry` a jelszót.
1. Kattintson a Raspberry ikonra > **beállítások** > **málna Pi konfigurációs**.

   ![A Raspbian beállítások menü](media/iot-hub-raspberry-pi-kit-c-get-started/1_raspbian-preferences-menu.png)

1. Az a **felületek** lapon **I2C** és **SSH** való **engedélyezése**, és kattintson a **OK**. Ha nem rendelkezik a fizikai érzékelők és szimulált érzékelőadatait használni szeretne, ez a lépés nem kötelező megadni.

   ![I2C és a Raspberry Pi SSH engedélyezése](media/iot-hub-raspberry-pi-kit-c-get-started/2_enable-spi-ssh-on-raspberry-pi.png)

> [!NOTE] 
SSH- és I2C engedélyezéséhez található további referencia dokumentumok [raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) és [RASPI-CONFIG](https://www.raspberrypi.org/documentation/configuration/raspi-config.md).

### <a name="connect-the-sensor-to-pi"></a>Csatlakozás az érzékelő Pi

Használják a breadboard és átkötés LED és egy BME280 Pi kell kapcsolódniuk az alábbiak szerint. Ha még nem rendelkezik az érzékelő [hagyja ki ezt a szakaszt](#connect-pi-to-the-network).

![A Pi málna és érzékelő kapcsolat](media/iot-hub-raspberry-pi-kit-node-get-started/3_raspberry-pi-sensor-connection.png)

A BME280 érzékelő gyűjthet a hőmérséklet és a páratartalom adatokat. És a LED fog villogni, ha egy eszköz és a felhő közötti kommunikációt. 

PIN-kód érzékelő használja a következő vezetékezést:

| Indítsa el a (érzékelő & LED)     | Záró (tábla)            | Kábel szín   |
| -----------------------  | ---------------------- | ------------: |
| VDD (PIN-kód 5 g.)             | 3.3V PWR (1. Pin)       | A fehér kábel   |
| GND (7G PIN-kód)             | GND (PIN-kód 6)            | Barna kábel   |
| SDI (PIN-kód 10G)            | I2C1 SDA (3 PIN-kód)       | Piros kábel     |
| SCK (8G PIN-kód)             | I2C1 SCL (PIN-kód 5)       | Narancssárga kábel  |
| LED VDD (PIN-kód 18F)        | GPIO 24 (PIN-kód 18)       | A fehér kábel   |
| LED GND (PIN-kód 17F)        | GND (PIN-kód 20)           | Fekete kábel   |

Megjelenítéséhez kattintson ide [málna Pi 2. és 3 PIN-kód hozzárendelések](https://developer.microsoft.com/windows/iot/docs/pinmappingsrpi) hivatkozhat.

Miután sikeresen csatlakozott a málna Pi BME280, meg kell például a kép alatt.

![Csatlakoztatott Pi és BME280](media/iot-hub-raspberry-pi-kit-node-get-started/4_connected-pi.jpg)

### <a name="connect-pi-to-the-network"></a>A Pi csatlakoznak a hálózathoz

Kapcsolja be a Pi a micro USB-kábelen és a tápegység. Az Ethernet-kábel segítségével Pi csatlakozni a vezetékes hálózatra, vagy hajtsa végre a [málna Pi alapját utasításainak](https://www.raspberrypi.org/learning/software-guide/wifi/) Pi a vezeték nélküli hálózathoz való kapcsolódáshoz. Miután a Pi sikeresen csatlakozott a hálózathoz, azt kell jegyezze fel az a [IP-címet a pi](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-3-network-setup/finding-your-pis-ip-address).

![Vezetékes hálózathoz csatlakoznak](media/iot-hub-raspberry-pi-kit-node-get-started/5_power-on-pi.jpg)

> [!NOTE]
> Győződjön meg arról, hogy a Pi és a számítógép ugyanahhoz a hálózathoz csatlakozik. Például ha a számítógép vezeték nélküli hálózathoz Pi egy vezetékes hálózathoz van csatlakoztatva, előfordulhat, hogy nem látja az IP-cím devdisco kimenet.

## <a name="run-a-sample-application-on-pi"></a>A Pi mintaalkalmazás futtatása

### <a name="install-the-prerequisite-packages"></a>Az előfeltételként szükséges csomagok telepítése

A számítógép a következő SSH-ügyfél segítségével a málna Pi csatlakozni.
   
   **Windows-felhasználók**
   1. Töltse le és telepítse [PuTTY](http://www.putty.org/) Windows. 
   1. Az IP-cím a tartományban a gazdagép nevét (vagy IP-cím) szakaszában másolja, és válassza ki az SSH a kapcsolattípus.
   
   
   **Mac és Ubuntu felhasználók**
   
   A beépített SSH-ügyfél Ubuntu vagy macOS használja. Előfordulhat, hogy futtatásához szükséges `ssh pi@<ip address of pi>` Pi SSH-kapcsolaton keresztül csatlakozni.
   > [!NOTE] 
   Az alapértelmezett felhasználónév az `pi` , és a jelszó `raspberry`.


### <a name="configure-the-sample-application"></a>A mintaalkalmazás konfigurálása

1. Klónozza a mintaalkalmazást a következő parancs futtatásával:

   ```bash
   cd ~
   git clone https://github.com/Azure-Samples/iot-hub-python-raspberrypi-client-app.git
   ```
1. Nyissa meg a konfigurációs fájlban a következő parancsok futtatásával:

   ```bash
   cd iot-hub-python-raspberrypi-client-app
   nano config.py
   ```

   5 makrók van ebben a fájlban configurate is. Az első egy `MESSAGE_TIMESPAN`, amely megadja, hogy az időtartam (ezredmásodpercben) két felhőbe küldött üzenetek között. A második érték `SIMULATED_DATA`, vagyis az, hogy szimulált érzékelőadatait vagy nem logikai értéket. `I2C_ADDRESS`a I2C címet a BME280 érzékelő csatlakoztatva van. `GPIO_PIN_ADDRESS`a GPIO cím szolgál a LED-jét. Az utolsót `BLINK_TIMESPAN`, amely meghatározni a TimeSpan érték, ha a LED ezredmásodpercben be van kapcsolva.

   Ha Ön **nem rendelkezik az érzékelő**, beállíthatja a `SIMULATED_DATA` egy érték `True` a minta kérelem létrehozása és használata a szimulált érzékelőadatait.

1. Mentse és zárja be a vezérlő-O billentyűkombináció lenyomásával > Enter > CTRL-X.

### <a name="build-and-run-the-sample-application"></a>Hozza létre, és futtassa a mintaalkalmazást

1. A minta-alkalmazás létrehozása a következő parancs futtatásával. Mivel az Azure IoT készült SDK-k Python burkolók az Azure IoT eszköz C SDK fölött, szüksége lesz a C-függvénytárak összeállításához, ha azt szeretné, vagy hozza létre a Python-könyvtárak forráskód kell.

   ```bash
   sudo chmod u+x setup.sh
   sudo ./setup.sh
   ```
   > [!NOTE] 
   Azt is megadhatja a kívánt futtatásával verzió `sudo ./setup.sh [--python-version|-p] [2.7|3.4|3.5]`. Ha paraméter nélkül parancsfájlt futtatja, a parancsfájl automatikusan észleli a python telepített verzióját (keresési feladatütemezési 2.7 -> 3.4 -> 3.5). Győződjön meg arról, hogy a Python verzió tartja konzisztens során kialakításához és futtatásához. 
   
   > [!NOTE] 
   Épület a Python ügyféloldali kódtár (iothub_client.so) kisebb, mint 1GB RAM-MAL rendelkező Linux rendszerű eszközökön, láthatja a build a lent látható módon iothub_client_python.cpp felépítésekor első Beragadt 98 % `[ 98%] Building CXX object python/src/CMakeFiles/iothub_client_python.dir/iothub_client_python.cpp.o`. Ha ezt a problémát tapasztal, ellenőrizze az eszköz használatával a memória-felhasználás `free -m command` egy másik terminál ablakban ebben az időszakban. Ha kevés a memória iothub_client_python.cpp fájl fordítása közben fut, akkor előfordulhat, hogy ideiglenesen növelje a lapozófájl több elérhető memória, a Python ügyféloldali SDK-könyvtár eszköz sikeresen létrehozásához lekérdezni.
   
1. Futtassa a mintaalkalmazást a következő parancs futtatásával:

   ```bash
   python app.py '<your Azure IoT hub device connection string>'
   ```

   > [!NOTE] 
   Győződjön meg arról, hogy Ön-e beillesztési az eszköz kapcsolati karakterláncát azokat a szimpla idézőjelben. Ha a python, 3, akkor a parancs használata és `python3 app.py '<your Azure IoT hub device connection string>'`.


   A következő kimeneti bemutatja az érzékelő adatokat és az IoT hub küldött üzenetek kell megjelennie.

   ![Kimeneti - érzékelő adatokat küld az IoT hub málna Pi](media/iot-hub-raspberry-pi-kit-c-get-started/success.png
)

## <a name="next-steps"></a>Következő lépések

Egy mintaalkalmazás érzékelő adatokat gyűjteni, és küldje el az IoT hub futtatását. A málna Pi az IoT hub vagy küldési üzenetek a málna Pi egy parancssori felület a küldött üzenetek, olvassa el a [kezelése felhő eszközt az IOT hubbal-explorer oktatóanyag üzenetküldési](https://docs.microsoft.com/en-gb/azure/iot-hub/iot-hub-explorer-cloud-device-messaging).

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
