---
title: "Modifier les mots de passe à l’aide de StorSimple Device Manager | Microsoft Docs"
description: "Explique comment utiliser le service StorSimple Manager pour modifier vos mots de passe du Gestionnaire d'instantanés StorSimple et d’administrateur de l’appareil."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: f178509c-f4e1-48a8-90b2-d4ad050eeb30
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/02/2017
ms.author: alkohli
ms.openlocfilehash: 843d0a4bcc1807a6b1995d067e0f9e572307775d
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/04/2017
---
# <a name="use-the-storsimple-manager-service-to-change-your-storsimple-passwords"></a>Utilisation du service StorSimple Manager pour modifier vos mots de passe StorSimple
> [!NOTE]
> Le portail Azure Classic pour StorSimple est déconseillé. Vos instances de StorSimple Device Manager seront automatiquement déplacées vers le nouveau portail Azure à la date de désapprobation planifiée. Vous serez prévenu de ce déplacement par un e-mail et une notification du portail. Ce document sera également bientôt retiré. Pour afficher la version de cet article applicable au nouveau portail Azure, accédez à [Utiliser le service StorSimple Manager pour modifier vos mots de passe StorSimple](storsimple-8000-change-passwords.md). Si vous avez des questions concernant le déplacement, consultez [FAQ : Déplacement vers le portail Azure](storsimple-8000-move-azure-portal-faq.md).

## <a name="overview"></a>Vue d'ensemble
La page **Configurer** du portail Azure Classic contient tous les paramètres reconfigurables d’un appareil StorSimple géré par un service StorSimple Manager. Ce didacticiel explique comment utiliser la page **Configurer** pour modifier le mot de passe d’administrateur de l’appareil ou le mot de passe StorSimple de Snapshot Manager.

## <a name="change-the-device-administrator-password"></a>Modification du mot de passe d’administrateur de l’appareil
Quand vous utilisez l’interface Windows PowerShell pour accéder à l’appareil StorSimple, vous devez entrer un mot de passe d’administrateur de l’appareil. À l’inscription du premier appareil StorSimple auprès d’un service, le mot de passe par défaut de cette interface est *Password1*. Pour assurer la sécurité de vos données, il vous est demandé de modifier ce mot de passe à la fin de l’inscription. Vous ne pouvez pas quitter le processus d'inscription sans modifier ce mot de passe. Pour plus d’informations, voir [Étape 3 : Configuration et inscription de l’appareil via Windows PowerShell pour StorSimple](storsimple-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

Le mot de passe qui a été défini initialement dans l’interface Windows PowerShell au moment de l’inscription peut ensuite être modifié via le portail Azure Classic. Pour modifier le mot de passe d’administrateur de l’appareil, procédez comme suit.

#### <a name="to-change-the-device-administrator-password"></a>Pour modifier le mot de passe d’administrateur de l’appareil
1. Dans le portail Classic, cliquez sur **Appareils** > **Configurer** pour votre appareil.
2. Faites défiler l’écran vers le bas jusqu’à la section **Mot de passe de l’administrateur de l’appareil** . Indiquez un mot de passe Administrateur contenant entre 8 et 15 caractères. Le mot de passe doit contenir une combinaison d’au moins 3 caractères en majuscules, en minuscules, numériques et spéciaux.
3. Confirmez le mot de passe.
4. Cliquez sur **Enregistrer** au bas de la page.

Le mot de passe Administrateur d’appareil doit maintenant être à jour. Vous pouvez utiliser ce mot de passe modifié pour accéder à l’interface Windows PowerShell.

## <a name="change-the-storsimple-snapshot-manager-password"></a>Modification du mot de passe du Gestionnaire d’instantanés StorSimple
Le Gestionnaire d’instantanés StorSimple réside sur l’ordinateur hôte Windows et permet aux administrateurs de gérer les sauvegardes de votre appareil StorSimple sous la forme d’instantanés cloud ou locaux.

Au moment de configurer un appareil dans le Gestionnaire d’instantanés StorSimple, vous êtes invité à fournir l’adresse IP et le mot de passe de votre appareil de stockage afin de l’authentifier. Ce mot de passe est d’abord configuré via l’interface Windows PowerShell. Pour plus d’informations, voir [Étape 3 : Configuration et inscription de l’appareil via Windows PowerShell pour StorSimple](storsimple-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

Le mot de passe qui a été défini initialement dans l’interface Windows PowerShell au moment de l’inscription peut ensuite être modifié via le portail Classic. Pour modifier le mot de passe du Gestionnaire d’instantanés StorSimple, procédez comme suit.

#### <a name="to-change-the-storsimple-snapshot-manager-password"></a>Pour modifier le mot de passe du Gestionnaire d’instantanés StorSimple
1. Dans le portail Classic, cliquez sur **Appareils** > **Configurer** pour votre appareil.
2. Faites défiler l’écran vers le bas jusqu’à la section **Gestionnaire d’instantanés StorSimple** . Entrez un mot de passe 14 ou 15 caractères. Assurez-vous que le mot de passe contient une combinaison d’au moins 3 caractères en majuscules, en minuscules, numériques et spéciaux.
3. Confirmez le mot de passe.
4. Cliquez sur **Enregistrer** au bas de la page.

Le mot de passe du Gestionnaire d’instantanés StorSimple doit maintenant être mis à jour.

## <a name="next-steps"></a>Étapes suivantes
* En savoir plus sur la [sécurité StorSimple](storsimple-security.md).
* En savoir plus sur la [modification de la configuration de votre appareil](storsimple-modify-device-config.md).
* En savoir plus sur [l’utilisation du service StorSimple Manager pour gérer votre appareil StorSimple](storsimple-manager-service-administration.md).

