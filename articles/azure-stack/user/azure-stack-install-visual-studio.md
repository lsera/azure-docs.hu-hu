---
title: "Telepítse a Visual Studio, és csatlakozzon az Azure-verem |} Microsoft Docs"
description: "Ismerje meg, a Visual Studio telepítése, és kapcsolódjon az Azure-verem szükséges lépések"
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
ms.assetid: 2022dbe5-47fd-457d-9af3-6c01688171d7
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2018
ms.author: mabrigg
ms.openlocfilehash: 5eae2553af1e41ace878a2f8f2c1a1656c63a7c4
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/02/2018
---
# <a name="install-visual-studio-and-connect-to-azure-stack"></a>Telepítse a Visual Studio, és csatlakozzon az Azure-verem

*A következőkre vonatkozik: Azure verem integrált rendszerek és az Azure verem szoftverfejlesztői készlet*

Visual Studio használata és telepítése Azure Resource Manager [sablonok](azure-stack-arm-templates.md) Azure-készletben. Segítségével a jelen cikkben ismertetett lépéseket telepítse a Visual Studio eszközből [Azure verem szoftverfejlesztői készlet](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), vagy egy Windows-alapú külső ügyfél Ha keresztül kapcsolódó [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn). A lépések a Visual Studio 2015 Community Edition új telepítés végrehajtásához. Tudjon meg többet az [párhuzamos](https://msdn.microsoft.com/library/ms246609.aspx) más Visual Studio verziói között.

## <a name="install-visual-studio"></a>A Visual Studio telepítése
1. Töltse le és futtassa a [Webplatform-telepítő](https://www.microsoft.com/web/downloads/platform.aspx).             
2. Keresse meg **Visual Studio Community 2015 Microsoft Azure SDK - 2.9.6**, kattintson a **Hozzáadás**, és **telepítése**.

    ![Képernyőfelvétel a WebPI telepítés lépései](./media/azure-stack-install-visual-studio/image1.png) 

3. Távolítsa el a **Microsoft Azure PowerShell** az Azure SDK részeként telepített.

    ![Képernyőkép a Azure PowerShell telepítése és törlése programok felület](./media/azure-stack-install-visual-studio/image2.png) 

4. [A PowerShell telepítése az Azure Stack szolgáltatáshoz](azure-stack-powershell-install.md)

5. Újraindítja az operációs rendszert a telepítés befejezése után.

## <a name="connect-to-azure-stack"></a>Csatlakozás az Azure Stackhez

1. Indítsa el a Visual Studio.

2. Az a **nézet** menü **Cloud Explorer**.

3. Jelölje ki az új ablaktáblán **fiók hozzáadása** , és jelentkezzen be Azure Active Directory hitelesítő adataival.  
    ![Képernyőkép az Cloud Explorer egyszer bejelentkezve, és kapcsolódik az Azure-verem](./media/azure-stack-install-visual-studio/image6.png)

Miután bejelentkezett, [sablonok telepítése](azure-stack-deploy-template-visual-studio.md) vagy tallózással keresse meg a rendelkezésre álló típusok és az erőforráscsoportok saját sablonok létrehozása.  

## <a name="next-steps"></a>További lépések

 - [Az Azure-verem sablonok fejlesztése](azure-stack-develop-templates.md)
