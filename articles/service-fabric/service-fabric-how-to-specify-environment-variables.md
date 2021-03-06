---
title: "Útmutató adja meg a környezeti változók szolgáltatások, az Azure Service Fabric |} Microsoft Docs"
description: "Bemutatja, hogyan használható a környezeti változók a Service Fabric-alkalmazások"
documentationcenter: .net
author: mikkelhegn
manager: markfuss
editor: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/06/2017
ms.author: mikhegn
ms.openlocfilehash: d487eeadde9f9a45549763863f8fe5b06b2945a4
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/16/2018
---
# <a name="how-to-specify-environment-variables-for-services-in-service-fabric"></a>A Service Fabric szolgáltatások környezeti változók megadása

Ez a cikk bemutatja, hogyan környezeti változók egy szolgáltatás vagy a tároló megadása a Service Fabric.

## <a name="procedure-for-specifying-environment-variables-for-services"></a>Környezeti változók szolgáltatások megadására szolgáló eljárás

Ebben a példában szereplő változó egy tároló beállításához. A cikk feltételezi, hogy már rendelkezik egy alkalmazás és szolgáltatás jegyzékfájl.

1. Nyissa meg a ServiceManifest.xml fájlt.
1. Az a `CodePackage` elem, adjon hozzá egy új `EnvironmentVariables` elem és egy `EnvironmentVariable` elem minden környezeti változó.

    ```xml
      <CodePackage Name="MyCode" Version="CodeVersion1">
      ...
        <EnvironmentVariables>
          <EnvironmentVariable Name="MyEnvVariable" Value="DeafultValue"/>
          <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
        </EnvironmentVariables>
      </CodePackage>
    ```

    Az alkalmazásjegyzékben szereplő környezeti változókat felülbírálható lesz.

1. A környezeti változókat az alkalmazásjegyzékben felülbírálásához használja a `EnvironmentOverrides` elemet.

    ```xml
      <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontEndServicePkg" ServiceManifestVersion="1.0.0" />
        <EnvironmentOverrides CodePackageRef="MyCode">
          <EnvironmentVariable Name="MyEnvVariable" Value="OverrideValue"/>
        </EnvironmentOverrides>
      </ServiceManifestImport>
    ```

## <a name="next-steps"></a>További lépések
Ebben a cikkben ismertetett alapfogalmakat némelyike kapcsolatos további tudnivalókért tekintse meg a [több környezetek cikkek alkalmazásokat kezeléséhez](service-fabric-manage-multiple-environment-app-configuration.md).

Más elérhető a Visual Studio alkalmazás-felügyeleti képességekkel kapcsolatos információkért lásd: [kezelése a Service Fabric-alkalmazások, a Visual Studio](service-fabric-manage-application-in-visual-studio.md).